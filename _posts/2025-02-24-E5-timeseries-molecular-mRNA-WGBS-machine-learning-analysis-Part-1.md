---
layout: post
title: E5 timeseries molecular mRNA-WGBS machine learning analysis Part 1
date: '2025-02-24'
categories: E5
tags: Molecular Multiomic R MachineLearning GeneExpression Epigenetics
---

This post details an exploratory analysis to try out machine learning analyses to predict mRNA state from WGBS state for the E5 time series molecular project.  

This script is conducted for *Acropora pulchra* - we will run the other species next.  

# Apul time series mRNA-WGBS machine learning predictions

## Set up

Load libraries

``` r
library(tidyverse)
library(ggplot2)
library(DESeq2)
library(igraph)
library(psych)
library(tidygraph)
library(ggraph)
library(WGCNA)
library(edgeR)
library(reshape2)
library(ggcorrplot)
library(corrplot)
library(rvest)
library(purrr)
library(pheatmap)
library(glmnet)
library(caret)
```

## Load and format data

Gene expression mRNA data.

``` r
# RNA variance stabilized counts data
genes <- read_csv("D-Apul/output/02.20-D-Apul-RNAseq-alignment-HiSat2/apul-gene_count_matrix.csv")
```

``` r
genes<-as.data.frame(genes)

rownames(genes)<-genes$gene_id

genes<-genes%>%select(!gene_id)
```

Load metadata.

``` r
metadata<-read_csv("M-multi-species/data/rna_metadata.csv")%>%select(AzentaSampleName, ColonyID, Timepoint)%>%
  filter(grepl("ACR", ColonyID))
```

``` r
colonies<-unique(metadata$ColonyID)
```

Load physiology data.

``` r
phys<-read_csv("https://github.com/urol-e5/timeseries/raw/refs/heads/master/time_series_analysis/Output/master_timeseries.csv")%>%filter(colony_id_corr %in% colonies)%>%
  select(colony_id_corr, species, timepoint, site, Host_AFDW.mg.cm2, Sym_AFDW.mg.cm2, Am, AQY, Rd, Ik, Ic, calc.umol.cm2.hr, cells.mgAFDW, prot_mg.mgafdw, Ratio_AFDW.mg.cm2, Total_Chl, Total_Chl_cell, cre.umol.mgafdw)
```

``` r
#add site information into metadata 
metadata$Site<-phys$site[match(metadata$ColonyID, phys$colony_id_corr)]
```

Load WGBS data.

``` r
#pull processed files from Gannet 

# Define the base URL
base_url <- "https://gannet.fish.washington.edu/seashell/bu-github/timeseries_molecular/D-Apul/output/15.5-Apul-bismark/"

# Read the HTML page
page <- read_html(base_url)

# Extract links to files
file_links <- page %>%
  html_nodes("a") %>%
  html_attr("href")

# Filter for files ending in "processed.txt"
processed_files <- file_links[grepl("processed\\.txt$", file_links)]

# Create full URLs
file_urls <- paste0(base_url, processed_files)

# Function to read a file from URL
read_processed_file <- function(url) {
  read_table(url, col_types = cols(.default = "c"))  # Read as character to avoid parsing issues
}

# Import all processed files into a list
processed_data <- lapply(file_urls, read_processed_file)

# Name the list elements by file name
names(processed_data) <- processed_files

# Print structure of imported data
str(processed_data)
```

``` r
# add a header row that has "CpG" for the first column and "sample" for the second column, which will be populated by the file name 

processed_data <- Map(function(df, filename) {
  colnames(df) <- c("CpG", filename)  # Rename columns
  return(df)
}, processed_data, names(processed_data))  # Use stored file names

#merge files together by "CpG"
merged_data <- purrr::reduce(processed_data, full_join, by = "CpG")

# Print structure of final merged data
str(merged_data)
```

Replace any NA with 0.

``` r
# Convert all columns (except "CpG") to numeric and replace NAs with 0
merged_data <- merged_data %>%
  mutate(across(-CpG, as.numeric)) %>%  # Convert all except CpG to numeric
  mutate(across(-CpG, ~ replace_na(.x, 0)))  # Replace NA with 0 in numeric columns
```

## Filter data sets

Only keep CpGs that have a non-zero value in all samples.

``` r
filtered_data <- merged_data %>%
  filter(if_all(-CpG, ~ .x > 0))
```

We had 12,093,025 CpGs before filtering and have only 507 after
filtering. This makes sense because most CpGs were not methylated in all
samples.

Only keep the sample information in the column name.

``` r
colnames(filtered_data) <- gsub("^(.*?)_.*$", "\\1", colnames(filtered_data))
```

Next I will format the genes, WGBS, and physiology data sets to have
consistent sample ID naming.

Only keep genes that are non-zero in all samples.

``` r
filtered_genes <- genes %>%
  filter(if_all(everything(), ~ .x > 0))
```

There were 44371 genes identified with 10717 genes kept after filtering
to only keep genes detected >0 in all samples.

## Rename samples to be consistent between data sets

The goal is to have each data frame with the feature as row names and
sample ID (ACR-ID-TP) as column names.

Format metadata.

``` r
metadata$code<-paste0(metadata$ColonyID, "-", metadata$Timepoint)
```

Format gene data set.

``` r
# Create a named vector for mapping: SampleName -> colonyID
rename_map <- setNames(metadata$code, metadata$AzentaSampleName)

# Rename matching columns in the gene dataset
colnames(filtered_genes) <- ifelse(colnames(filtered_genes) %in% names(rename_map), 
                                  rename_map[colnames(filtered_genes)], 
                                  colnames(filtered_genes))
```

View differences in column names between genes and wgbs data sets.

``` r
setdiff(colnames(filtered_data), colnames(filtered_genes))
```

    ## [1] "CpG"

All colony names match!

Set CpG id as rownames in the wgbs dataset.

``` r
filtered_data<-as.data.frame(filtered_data)

rownames(filtered_data)<-filtered_data$CpG

filtered_data<-filtered_data%>%
  select(!CpG)
```

The genes and wgbs data sets are now formatted the same way.

Set metadata with samples as column names.

``` r
metadata<-as.data.frame(metadata)
rownames(metadata)<-metadata$code
metadata<-metadata%>%
  select(ColonyID, Timepoint, Site)
```

## Transform data

Set the order of genes, wgbs, and metadata to all be the same.

``` r
# Ensure rownames of metadata are used as the desired column order
desired_order <- rownames(metadata)

# Find columns in genes_data that match the desired order
matching_columns <- intersect(desired_order, colnames(filtered_genes))

# Reorder columns in genes_data to match metadata rownames
filtered_genes <- filtered_genes %>%
  select(all_of(matching_columns))

# Print the updated column order
print(colnames(filtered_genes))
```

``` r
print(rownames(metadata))
```

Repeat for wgbs data.

``` r
# Find columns in genes_data that match the desired order
matching_columns <- intersect(desired_order, colnames(filtered_data))

# Reorder columns in genes_data to match metadata rownames
filtered_data <- filtered_data %>%
  select(all_of(matching_columns))

# Print the updated column order
print(colnames(filtered_data))
```

``` r
print(rownames(metadata))
```

Use a variance stabilized transformation for the genes and wgbs data
sets.

``` r
dds_genes <- DESeqDataSetFromMatrix(countData = filtered_genes, 
                              colData = metadata, 
                              design = ~ Timepoint * Site)
```

    ## converting counts to integer mode

    ## Warning in DESeqDataSet(se, design = design, ignoreRank): some variables in
    ## design formula are characters, converting to factors

``` r
# Variance Stabilizing Transformation
vsd_genes <- assay(vst(dds_genes, blind = TRUE))
```

I had to round wgbs data to whole integers for normalization - need to
return to this to decide if this is appropriate.

``` r
str(filtered_data)
```

``` r
#round to integers 
filtered_data<-filtered_data %>%
  mutate(across(where(is.numeric), round))

dds_wgbs <- DESeqDataSetFromMatrix(countData = filtered_data, 
                              colData = metadata, 
                              design = ~ Timepoint * Site)
```


``` r
# Variance Stabilizing Transformation
vsd_wgbs <- assay(varianceStabilizingTransformation(dds_wgbs, blind = TRUE))
```


## Feature selection

Reduce dimensionality through feature selection.

``` r
str(vsd_wgbs)
```


``` r
str(vsd_genes)
```


There are more CpGs than genes in our dataset, so we need to reduce
dimensionality.

Reduce features using a pca and keep the top 20 PCs. We can return to
this later to see if we want to include more PCs.

Reduce genes.

``` r
# Perform PCA on gene expression matrix
pca_genes <- prcomp(t(vsd_genes), scale. = TRUE)

# Select top PCs that explain most variance (e.g., top 50 PCs)
explained_var <- summary(pca_genes)$importance[2, ]  # Cumulative variance explained
num_pcs <- min(which(cumsum(explained_var) > 0.9))  # Keep PCs that explain 90% variance

gene_pcs <- as.data.frame(pca_genes$x[, 1:num_pcs])  # Extract selected PCs
```

Reduce genes.

``` r
# Perform PCA on gene expression matrix
pca_wgbs <- prcomp(t(vsd_wgbs), scale. = TRUE)

# Select top PCs that explain most variance (e.g., top 50 PCs)
explained_var <- summary(pca_wgbs)$importance[2, ]  # Cumulative variance explained
num_pcs <- min(which(cumsum(explained_var) > 0.9))  # Keep PCs that explain 90% variance

wgbs_pcs <- as.data.frame(pca_wgbs$x[, 1:num_pcs])  # Extract selected PCs
```

``` r
dim(gene_pcs)
```

    ## [1] 40 21

``` r
dim(wgbs_pcs)
```

    ## [1] 40 24

We have 21 gene expression PCs and 24 WGBS PCs.

``` r
# Ensure sample matching between gene and methylation PCs
common_samples <- intersect(rownames(gene_pcs), rownames(wgbs_pcs))
gene_pcs <- gene_pcs[common_samples, ]
wgbs_pcs <- wgbs_pcs[common_samples, ]
```

Train elastic models to predict gene expression PCs from methylation
PCs.

``` r
train_models <- function(response_pcs, predictor_pcs) {
  models <- list()
  
  for (pc in colnames(response_pcs)) {
    y <- response_pcs[[pc]]  # Gene expression PC
    X <- as.matrix(predictor_pcs)  # Methylation PCs as predictors
    
    # Train elastic net model (alpha = 0.5 for mix of LASSO & Ridge)
    model <- cv.glmnet(X, y, alpha = 0.5)
    
    models[[pc]] <- model
  }
  
  return(models)
}

# Train models predicting gene expression PCs from methylation PCs
models <- train_models(gene_pcs, wgbs_pcs)
```

Extract feature importance.

``` r
get_feature_importance <- function(models) {
  importance_list <- lapply(models, function(model) {
    coefs <- as.matrix(coef(model, s = "lambda.min"))[-1, , drop = FALSE]  # Convert to regular matrix & remove intercept
    
    # Convert to data frame
    coefs_df <- data.frame(Feature = rownames(coefs), Importance = as.numeric(coefs))
    
    return(coefs_df)
  })
  
  # Combine feature importance across all predicted gene PCs
  importance_df <- bind_rows(importance_list) %>%
    group_by(Feature) %>%
    summarize(MeanImportance = mean(abs(Importance)), .groups = "drop") %>%
    arrange(desc(MeanImportance))
  
  return(importance_df)
}

feature_importance <- get_feature_importance(models)
head(feature_importance, 20)  # Top 20 predictive methylation PCs
```

    ## # A tibble: 20 × 2
    ##    Feature MeanImportance
    ##    <chr>            <dbl>
    ##  1 PC22             0.326
    ##  2 PC9              0.254
    ##  3 PC10             0.227
    ##  4 PC8              0.222
    ##  5 PC5              0.217
    ##  6 PC15             0.210
    ##  7 PC4              0.206
    ##  8 PC23             0.204
    ##  9 PC6              0.186
    ## 10 PC7              0.179
    ## 11 PC20             0.167
    ## 12 PC11             0.159
    ## 13 PC3              0.154
    ## 14 PC18             0.147
    ## 15 PC21             0.138
    ## 16 PC16             0.138
    ## 17 PC13             0.123
    ## 18 PC19             0.120
    ## 19 PC12             0.116
    ## 20 PC2              0.104

Evaluate performance.

``` r
evaluate_model_performance <- function(models, response_pcs, predictor_pcs) {
  results <- data.frame(PC = colnames(response_pcs), R2 = NA)
  
  for (pc in colnames(response_pcs)) {
    y <- response_pcs[[pc]]
    X <- as.matrix(predictor_pcs)
    
    model <- models[[pc]]
    preds <- predict(model, X, s = "lambda.min")
    
    R2 <- cor(y, preds)^2  # R-squared metric
    results[results$PC == pc, "R2"] <- R2
  }
  
  return(results)
}

performance_results <- evaluate_model_performance(models, gene_pcs, wgbs_pcs)
summary(performance_results$R2)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##  0.1221  0.3658  0.5542  0.5167  0.7092  0.8128       6

Plot results.

``` r
# Select top 20 predictive methylation PCs
top_features <- feature_importance %>% top_n(20, MeanImportance)

# Plot
ggplot(top_features, aes(x = reorder(Feature, MeanImportance), y = MeanImportance)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  coord_flip() +  # Flip for readability
  theme_minimal() +
  labs(title = "Top 20 Predictive Methylation PCs",
       x = "Methylation PC",
       y = "Mean Importance")
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/machine-learning/unnamed-chunk-27-1.png?raw=TRUE)<!-- -->

``` r
ggplot(performance_results, aes(x = PC, y = R2)) +
  geom_point(color = "darkred", size = 3) +
  geom_hline(yintercept = mean(performance_results$R2, na.rm = TRUE), linetype = "dashed", color = "blue") +
  theme_minimal() +
  labs(title = "Model Performance Across Gene Expression PCs",
       x = "Gene Expression PC",
       y = "R² (Variance Explained)") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))  # Rotate labels
```

    ## Warning: Removed 6 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/machine-learning/unnamed-chunk-28-1.png?raw=TRUE)<!-- -->

View components associated with PCs

``` r
# Get the PCA rotation (loadings) matrix from the original WGBS PCA
wgbs_loadings <- pca_wgbs$rotation  # Each column corresponds to a PC

# Identify the top predictive PCs (from feature importance)
top_predictive_pcs <- feature_importance$Feature[1:5]  # Select top 5 most predictive PCs

# Extract the loadings for those PCs
top_loadings <- wgbs_loadings[, top_predictive_pcs, drop = FALSE]

# Convert to data frame and reshape for plotting
top_loadings_df <- as.data.frame(top_loadings) %>%
  rownames_to_column(var = "CpG") %>%
  pivot_longer(-CpG, names_to = "Methylation_PC", values_to = "Loading")

# View top CpGs contributing most to each PC
top_cpgs <- top_loadings_df %>%
  group_by(Methylation_PC) %>%
  arrange(desc(abs(Loading))) %>%
  slice_head(n = 20)  # Select top 10 CpGs per PC

print(top_cpgs)
```

    ## # A tibble: 100 × 3
    ## # Groups:   Methylation_PC [5]
    ##    CpG                     Methylation_PC Loading
    ##    <chr>                   <chr>            <dbl>
    ##  1 CpG_ptg000027l_13526774 PC10             0.141
    ##  2 CpG_ptg000027l_13526778 PC10             0.139
    ##  3 CpG_ptg000027l_13526776 PC10             0.134
    ##  4 CpG_ptg000024l_4334180  PC10            -0.134
    ##  5 CpG_ptg000023l_16429552 PC10             0.130
    ##  6 CpG_ptg000024l_4334178  PC10            -0.125
    ##  7 CpG_ptg000024l_4334569  PC10             0.117
    ##  8 CpG_ptg000024l_4334182  PC10            -0.115
    ##  9 CpG_ptg000016l_4107446  PC10             0.113
    ## 10 CpG_ptg000047l_404708   PC10            -0.112
    ## # ℹ 90 more rows

View top 20 CpGs associated with PC9 (the most important PC)

``` r
print(top_cpgs%>%filter(Methylation_PC=="PC9"))
```

    ## # A tibble: 20 × 3
    ## # Groups:   Methylation_PC [1]
    ##    CpG                     Methylation_PC Loading
    ##    <chr>                   <chr>            <dbl>
    ##  1 CpG_ptg000023l_15451833 PC9            -0.154 
    ##  2 CpG_ptg000016l_831394   PC9             0.129 
    ##  3 CpG_ptg000031l_13258965 PC9            -0.120 
    ##  4 CpG_ptg000024l_4334495  PC9             0.119 
    ##  5 CpG_ptg000030l_2489083  PC9            -0.118 
    ##  6 CpG_ptg000024l_4334221  PC9             0.112 
    ##  7 CpG_ptg000022l_5439870  PC9             0.109 
    ##  8 CpG_ptg000022l_5985634  PC9            -0.108 
    ##  9 CpG_ptg000012l_5662480  PC9            -0.107 
    ## 10 CpG_ptg000008l_19954852 PC9            -0.107 
    ## 11 CpG_ptg000024l_4334485  PC9             0.106 
    ## 12 CpG_ntLink_6_20175819   PC9            -0.104 
    ## 13 CpG_ptg000023l_15451819 PC9            -0.101 
    ## 14 CpG_ptg000016l_831268   PC9             0.101 
    ## 15 CpG_ptg000031l_13258948 PC9            -0.0988
    ## 16 CpG_ptg000031l_5818313  PC9            -0.0977
    ## 17 CpG_ntLink_6_20175807   PC9            -0.0959
    ## 18 CpG_ptg000024l_4334521  PC9             0.0942
    ## 19 CpG_ptg000001l_2901983  PC9             0.0935
    ## 20 CpG_ptg000024l_213653   PC9             0.0920

``` r
ggplot(top_cpgs, aes(x = reorder(CpG, abs(Loading)), y = Loading, fill = Methylation_PC)) +
  geom_bar(stat = "identity") +
  coord_flip() +  
  theme_minimal() +
  labs(title = "Top CpGs Contributing to Most Predictive Methylation PCs",
       x = "CpG Site",
       y = "Loading Strength") +
  facet_grid(~Methylation_PC, scales = "free_y")  # Separate plots for each PC
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/machine-learning/unnamed-chunk-31-1.png?raw=TRUE)<!-- -->

View predicted vs actual gene expression values to evaluate model.

``` r
# Choose a gene expression PC to visualize (e.g., the most predictable one)
best_pc <- performance_results$PC[which.max(performance_results$R2)]

# Extract actual and predicted values for that PC
actual_values <- gene_pcs[[best_pc]]
predicted_values <- predict(models[[best_pc]], as.matrix(wgbs_pcs), s = "lambda.min")

# Create data frame
prediction_df <- data.frame(
  Actual = actual_values,
  Predicted = predicted_values
)

# Scatter plot with regression line
ggplot(prediction_df, aes(x = Actual, y = lambda.min)) +
  geom_point(color = "blue", alpha = 0.7) +
  geom_smooth(method = "lm", color = "red", se = FALSE) +
  theme_minimal() +
  labs(title = paste("Predicted vs. Actual for", best_pc),
       x = "Actual Gene Expression PC",
       y = "Predicted Gene Expression PC") +
  annotate("text", x = min(actual_values), y = max(predicted_values), 
           label = paste("R² =", round(max(performance_results$R2, na.rm=TRUE), 3)), 
           hjust = 0, color = "black", size = 5)
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/machine-learning/unnamed-chunk-32-1.png?raw=TRUE)<!-- -->

``` r
ggplot(performance_results, aes(y = R2)) +
  geom_boxplot(fill = "lightblue", alpha = 0.7) +
  theme_minimal() +
  labs(title = "Distribution of Predictive Performance (R²) Across PCs",
       y = "R² (Variance Explained)")
```

    ## Warning: Removed 6 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/machine-learning/unnamed-chunk-33-1.png?raw=TRUE)<!-- -->

``` r
# Compute correlation between actual and predicted gene expression PCs
predicted_matrix <- sapply(models, function(m) predict(m, as.matrix(wgbs_pcs), s = "lambda.min"))

# Ensure matrices are the same size
predicted_matrix <- predicted_matrix[, colnames(gene_pcs), drop = FALSE]  # Align columns

# Compute correlation matrix, handling missing values
cor_matrix <- cor(predicted_matrix, as.matrix(gene_pcs), use = "pairwise.complete.obs")

# Replace NA or Inf values with zero
cor_matrix[is.na(cor_matrix) | is.infinite(cor_matrix)] <- 0  

# Plot heatmap
pheatmap(cor_matrix, color = colorRampPalette(c("blue", "white", "red"))(50),
         main = "Correlation Between Actual and Predicted Gene Expression PCs",
         fontsize = 10)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/machine-learning/unnamed-chunk-34-1.png?raw=TRUE)<!-- -->
