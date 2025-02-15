---
layout: post
title: E5 timeseries project multiomic WGCNA Part 1  
date: '2025-02-14'
categories: E5
tags: Integration Molecular R WGCNA
---

This post details my first round of analysis of correlations between gene expression (RNAseq) and methylation (WGBS) in the E5 time series molecular samples.  

# Overview 

In this project, we have collected mulit-omic data for the [E5 time series project](https://github.com/urol-e5/timeseries). In this analysis, I am conducting correlation network analyses to examine linkages between RNAseq and WGBS data sets and physiological metrics of interest.   

You can view the [GitHub repo for this project here](https://github.com/urol-e5/timeseries_molecular/tree/main). The script used for the analysis detailed below can be found at the [direct GitHub link here](https://github.com/urol-e5/timeseries_molecular/blob/main/D-Apul/code/18-Apul-multiomic-correlation-network.Rmd).    

# tl;dr 

I identified several modules of co-expressed/correlated genes and CpG features. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_mrna_cpg_module_features.png?raw=true)  

Some of these modules are significantly correlated to each other, as shown in the network below.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_mrna_cpg_cor_network.png?raw=true)  

Some of these modules are significantly correlated to physiological metrics and particular time points.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_module_trait_heatmap.png?raw=true)  

The next step is to dig into the modules that correlate with physiological metrics of interest and look into the co-expression relationships between mRNA and CpG features.  

# Set up

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
```

# Acropora pulchra

## Load and format data

Gene expression mRNA data.

``` r
# RNA variance stabilized counts data
genes <- read_csv("D-Apul/output/02.20-D-Apul-RNAseq-alignment-HiSat2/apul-gene_count_matrix.csv")
```

    ## Rows: 44371 Columns: 41
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): gene_id
    ## dbl (40): 1A1, 1A10, 1A12, 1A2, 1A8, 1A9, 1B1, 1B10, 1B2, 1B5, 1B9, 1C10, 1C...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

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

    ## New names:
    ## Rows: 117 Columns: 19
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (13): SampleName, WellNumber, AzentaSampleName, ColonyID, Timepoint, Sam... dbl
    ## (5): SampleNumber, Plate, TotalAmount-ng, Volume-uL, Conc-ng.uL lgl (1):
    ## MethodUsedForSpectrophotometry
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `` -> `...19`

``` r
colonies<-unique(metadata$ColonyID)
```

Load physiology data.

``` r
phys<-read_csv("https://github.com/urol-e5/timeseries/raw/refs/heads/master/time_series_analysis/Output/master_timeseries.csv")%>%filter(colony_id_corr %in% colonies)%>%
  select(colony_id_corr, species, timepoint, site, Host_AFDW.mg.cm2, Sym_AFDW.mg.cm2, Am, AQY, Rd, Ik, Ic, calc.umol.cm2.hr, cells.mgAFDW, prot_mg.mgafdw, Ratio_AFDW.mg.cm2, Total_Chl, Total_Chl_cell, cre.umol.mgafdw)
```

    ## Rows: 448 Columns: 46
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (10): colony_id, colony_id_corr, species, timepoint, month, site, nutrie...
    ## dbl (36): cre.umol.mgprot, Host_AFDW.mg.cm2, Sym_AFDW.mg.cm2, Host_DW.mg.cm2...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

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

View an example of the data structure 

    ##  $ ACR-265-TP4_10x_processed.txt: spc_tbl_ [4,088,229 × 2] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##   ..$ CpG_ntLink_0_25624: chr [1:4088229] "CpG_ntLink_0_25692" "CpG_ntLink_0_25699" "CpG_ntLink_0_25761" "CpG_ntLink_0_25777" ...
    ##   ..$ 0.000000          : chr [1:4088229] "0.000000" "0.000000" "0.000000" "0.000000" ...
    ##   ..- attr(*, "spec")=
    ##   .. .. cols(
    ##   .. ..   .default = col_character(),
    ##   .. ..   CpG_ntLink_0_25624 = col_character(),
    ##   .. ..   `0.000000` = col_character()
    ##   .. .. )

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

We had 12,093,025 CpGs before filtering and have only 507 after filtering. This makes sense because most CpGs were not methylated or measured in all samples.  

Only keep the sample information in the column name.  

``` r
colnames(filtered_data) <- gsub("^(.*?)_.*$", "\\1", colnames(filtered_data))
```

Next I will format the genes, WGBS, and physiology data sets to have consistent sample ID naming.  

Only keep genes that are non-zero in all samples.  

``` r
filtered_genes <- genes %>%
  filter(if_all(everything(), ~ .x > 0))
```

There were 44371 genes identified with 10717 genes kept after filtering to only keep genes detected at >0 in all samples.  

## Rename samples to be consistent between data sets

The goal is to have each data frame with the feature as row names and sample ID (ACR-ID-TP) as column names.  

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

Create a matching sample name in the physiology set.  

``` r
#change the format of the timepoint column 
phys <- phys %>%
  filter(species=="Acropora")%>%
  mutate(timepoint = gsub("timepoint", "TP", timepoint))%>%
  mutate(code = paste0(colony_id_corr, "-", timepoint))

#set row names and trim to useful columns
phys<-as.data.frame(phys)
rownames(phys)<-phys$code
phys<-phys%>%
  select(!code)

#remove calcification and antioxidant capacity that have NAs 
phys<-phys%>%
  select(!c(cre.umol.mgafdw, calc.umol.cm2.hr))%>%
  select(!c(colony_id_corr, species, timepoint, site))
```

We have physiology data from 39 out of 40 colonies in this data set.  

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

Use a variance stabilized transformation for genes and wgbs data sets.  

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

I had to round wgbs data to whole integers for normalization - need to return to this to decide if this is appropriate.

``` r
str(filtered_data)
```

    ## 'data.frame':    507 obs. of  40 variables:
    ##  $ ACR-225-TP1: num  15.25 12.5 6.09 1.9 9.72 ...
    ##  $ ACR-225-TP2: num  27 23.7 44.8 48.4 60 ...
    ##  $ ACR-225-TP3: num  4.55 4.55 23.53 27.78 16.67 ...
    ##  $ ACR-225-TP4: num  21.7 17.8 48.3 50 28.6 ...
    ##  $ ACR-229-TP1: num  8.11 13.95 42.22 43.9 21.21 ...
    ##  $ ACR-229-TP2: num  5.13 5.13 34.04 31.91 38.89 ...
    ##  $ ACR-229-TP3: num  9.09 9.09 31.82 37.21 28.95 ...
    ##  $ ACR-229-TP4: num  2.38 2.7 32.56 36.36 21.43 ...
    ##  $ ACR-237-TP1: num  23.8 21 65.2 65 76.2 ...
    ##  $ ACR-237-TP2: num  42.2 32.8 85.7 86.4 87.5 ...
    ##  $ ACR-237-TP3: num  30.6 30.6 71.9 68.8 54.5 ...
    ##  $ ACR-237-TP4: num  33.3 25 72.4 71 76.2 ...
    ##  $ ACR-244-TP1: num  16.7 18.8 43.8 44.8 50 ...
    ##  $ ACR-244-TP2: num  43.1 38.8 72.2 75.7 77.4 ...
    ##  $ ACR-244-TP3: num  35.7 34.1 57.1 63.2 52.4 ...
    ##  $ ACR-244-TP4: num  25.5 27.5 61.4 61 54.3 ...
    ##  $ ACR-265-TP1: num  21.7 28.1 21.3 25.6 20 ...
    ##  $ ACR-265-TP2: num  27.3 23.1 45.5 51.7 48 ...
    ##  $ ACR-265-TP3: num  24 16.7 43.6 48.8 19.4 ...
    ##  $ ACR-265-TP4: num  14.3 22.7 35.8 37.3 24.2 ...
    ##  $ ACR-139-TP1: num  43.8 43.3 40 44.9 35.1 ...
    ##  $ ACR-139-TP2: num  23.5 18.8 30.8 28.3 31.8 ...
    ##  $ ACR-139-TP3: num  23.3 17.9 22 25 30.8 ...
    ##  $ ACR-139-TP4: num  37.8 39.5 26.2 31.6 27 ...
    ##  $ ACR-145-TP1: num  12.5 9.09 41.46 41.46 44.83 ...
    ##  $ ACR-145-TP2: num  21.2 21.1 50 46.5 30 ...
    ##  $ ACR-145-TP3: num  19.2 18.5 38.2 40.6 14.3 ...
    ##  $ ACR-145-TP4: num  16 13.2 34.8 40.5 52.9 ...
    ##  $ ACR-150-TP1: num  16.4 18.2 53.8 63 54.2 ...
    ##  $ ACR-150-TP2: num  34.6 33.3 78.9 75 68.8 ...
    ##  $ ACR-150-TP3: num  34.1 26.2 66.7 73.7 66.7 ...
    ##  $ ACR-150-TP4: num  30.2 24.2 68 66.7 38.1 ...
    ##  $ ACR-173-TP1: num  7.89 5.41 34.09 34.21 20 ...
    ##  $ ACR-173-TP2: num  16.1 21.9 34.5 39.3 33.3 ...
    ##  $ ACR-173-TP3: num  16.7 16 23.8 28.8 10 ...
    ##  $ ACR-173-TP4: num  4.76 5 16.36 16.33 14.29 ...
    ##  $ ACR-186-TP1: num  16.7 8.7 36 32.7 21.4 ...
    ##  $ ACR-186-TP2: num  13.8 17.5 39.5 42.5 16.1 ...
    ##  $ ACR-186-TP3: num  12.2 13.6 34.5 36.8 12.5 ...
    ##  $ ACR-186-TP4: num  28.9 25 52.5 52.5 17.9 ...

``` r
#round to integers 
filtered_data<-filtered_data %>%
  mutate(across(where(is.numeric), round))

dds_wgbs <- DESeqDataSetFromMatrix(countData = filtered_data, 
                              colData = metadata, 
                              design = ~ Timepoint * Site)
```

    ## converting counts to integer mode

    ## Warning in DESeqDataSet(se, design = design, ignoreRank): some variables in
    ## design formula are characters, converting to factors

``` r
# Variance Stabilizing Transformation
vsd_wgbs <- assay(varianceStabilizingTransformation(dds_wgbs, blind = TRUE))
```

    ## -- note: fitType='parametric', but the dispersion trend was not well captured by the
    ##    function: y = a/x + b, and a local regression fit was automatically substituted.
    ##    specify fitType='local' or 'mean' to avoid this message next time.

Scale physiological variables (mean of 0 and std dev of 1).

``` r
phys <- phys %>%
  mutate(across(where(is.numeric), scale))
```

Rbind together the data sets in preparation for wgcna.  

``` r
combined_data<-rbind(vsd_genes, vsd_wgbs)
```

This dataset now has all CpGs and genes included.  

## Conduct module correlations with WGCNA

Set soft threshold.  

``` r
options(stringsAsFactors = FALSE)
enableWGCNAThreads()  # Enable multi-threading
```

    ## Allowing parallel execution with up to 47 working processes.

``` r
allowWGCNAThreads(nThreads = 2)
```

    ## Allowing multi-threading with up to 2 threads.

``` r
# Combine mRNA and lncRNA datasets
datExpr <- t(combined_data)

sum(is.na(datExpr))  # Should be 0
```

    ## [1] 0

``` r
sum(!is.finite(as.matrix(datExpr)))  # Should be 0
```

    ## [1] 0

``` r
# Remove genes/samples with missing or infinite values
datExpr <- datExpr[complete.cases(datExpr), ]
datExpr <- datExpr[, colSums(is.na(datExpr)) == 0]

# # Choose a set of soft-thresholding powers
powers <- c(seq(from = 1, to=19, by=2), c(21:30)) #Create a string of numbers from 1 through 10, and even numbers from 10 through 20
# 
# # Call the network topology analysis function
sft <-pickSoftThreshold(datExpr, powerVector = powers, verbose = 5)
```

    ## pickSoftThreshold: will use block size 3986.
    ##  pickSoftThreshold: calculating connectivity for given powers...
    ##    ..working on genes 1 through 3986 of 11224
    ##    ..working on genes 3987 through 7972 of 11224
    ##    ..working on genes 7973 through 11224 of 11224
    ##    Power SFT.R.sq  slope truncated.R.sq mean.k. median.k. max.k.
    ## 1      1    0.143  0.606          0.742 3060.00  2.97e+03   5100
    ## 2      3    0.888 -0.978          0.979  655.00  4.76e+02   2190
    ## 3      5    0.959 -1.190          0.992  240.00  1.17e+02   1290
    ## 4      7    0.968 -1.260          0.993  113.00  3.61e+01    875
    ## 5      9    0.977 -1.270          0.996   62.00  1.33e+01    642
    ## 6     11    0.980 -1.280          0.996   37.60  5.26e+00    496
    ## 7     13    0.977 -1.280          0.990   24.50  2.22e+00    398
    ## 8     15    0.977 -1.280          0.988   16.90  1.03e+00    329
    ## 9     17    0.979 -1.270          0.987   12.20  5.18e-01    278
    ## 10    19    0.982 -1.260          0.989    9.10  2.68e-01    238
    ## 11    21    0.980 -1.250          0.985    6.99  1.43e-01    208
    ## 12    22    0.981 -1.250          0.984    6.18  1.05e-01    195
    ## 13    23    0.978 -1.250          0.983    5.50  7.78e-02    183
    ## 14    24    0.975 -1.250          0.979    4.92  5.79e-02    173
    ## 15    25    0.967 -1.250          0.973    4.42  4.40e-02    163
    ## 16    26    0.969 -1.250          0.977    3.98  3.33e-02    155
    ## 17    27    0.953 -1.250          0.963    3.61  2.54e-02    147
    ## 18    28    0.955 -1.250          0.962    3.28  1.94e-02    140
    ## 19    29    0.956 -1.250          0.964    2.99  1.49e-02    133
    ## 20    30    0.948 -1.260          0.957    2.74  1.16e-02    127

Plot the results.

``` r
sizeGrWindow(9, 5)
par(mfrow = c(1,2));
cex1 = 0.9;
# # # Scale-free topology fit index as a function of the soft-thresholding power
plot(sft$fitIndices[,1], -sign(sft$fitIndices[,3])*sft$fitIndices[,2],
      xlab="Soft Threshold (power)",ylab="Scale Free Topology Model Fit,signed R^2",type="n",
     main = paste("Scale independence"));
 text(sft$fitIndices[,1], -sign(sft$fitIndices[,3])*sft$fitIndices[,2],
     labels=powers,cex=cex1,col="red");
# # # this line corresponds to using an R^2 cut-off
 abline(h=0.9,col="red")
# # # Mean connectivity as a function of the soft-thresholding power
 plot(sft$fitIndices[,1], sft$fitIndices[,5],
     xlab="Soft Threshold (power)",ylab="Mean Connectivity", type="n",
     main = paste("Mean connectivity"))
 text(sft$fitIndices[,1], sft$fitIndices[,5], labels=powers, cex=cex1,col="red")
```

Selected power will be 5 (the first value to cross 0.9 threshold).

### Generate network

``` r
selected_power<-5 

# Network construction (adjust power based on sft output)
net = blockwiseModules(datExpr, power = selected_power,
                       TOMType = "unsigned", minModuleSize = 30,
                       reassignThreshold = 0, mergeCutHeight = 0.25,
                       numericLabels = TRUE, pamRespectsDendro = FALSE,
                       saveTOMs = FALSE, verbose = 3)
```

    ##  Calculating module eigengenes block-wise from all genes
    ##    Flagging genes and samples with too many missing values...
    ##     ..step 1
    ##  ....pre-clustering genes to determine blocks..
    ##    Projective K-means:
    ##    ..k-means clustering..
    ##    ..merging smaller clusters...
    ## Block sizes:
    ## gBlocks
    ##    1    2    3 
    ## 4882 3279 3063 
    ##  ..Working on block 1 .
    ##     TOM calculation: adjacency..
    ##     ..will use 2 parallel threads.
    ##      Fraction of slow calculations: 0.000000
    ##     ..connectivity..
    ##     ..matrix multiplication (system BLAS)..
    ##     ..normalization..
    ##     ..done.
    ##  ....clustering..
    ##  ....detecting modules..
    ##  ....calculating module eigengenes..
    ##  ....checking kME in modules..
    ##      ..removing 256 genes from module 1 because their KME is too low.
    ##      ..removing 43 genes from module 2 because their KME is too low.
    ##      ..removing 2 genes from module 3 because their KME is too low.
    ##  ..Working on block 2 .
    ##     TOM calculation: adjacency..
    ##     ..will use 2 parallel threads.
    ##      Fraction of slow calculations: 0.000000
    ##     ..connectivity..
    ##     ..matrix multiplication (system BLAS)..
    ##     ..normalization..
    ##     ..done.
    ##  ....clustering..
    ##  ....detecting modules..
    ##  ....calculating module eigengenes..
    ##  ....checking kME in modules..
    ##      ..removing 38 genes from module 1 because their KME is too low.
    ##      ..removing 21 genes from module 2 because their KME is too low.
    ##      ..removing 17 genes from module 3 because their KME is too low.
    ##      ..removing 22 genes from module 4 because their KME is too low.
    ##      ..removing 14 genes from module 5 because their KME is too low.
    ##      ..removing 4 genes from module 6 because their KME is too low.
    ##      ..removing 7 genes from module 7 because their KME is too low.
    ##      ..removing 6 genes from module 8 because their KME is too low.
    ##      ..removing 7 genes from module 9 because their KME is too low.
    ##      ..removing 2 genes from module 10 because their KME is too low.
    ##      ..removing 1 genes from module 15 because their KME is too low.
    ##      ..removing 2 genes from module 20 because their KME is too low.
    ##  ..Working on block 3 .
    ##     TOM calculation: adjacency..
    ##     ..will use 2 parallel threads.
    ##      Fraction of slow calculations: 0.000000
    ##     ..connectivity..
    ##     ..matrix multiplication (system BLAS)..
    ##     ..normalization..
    ##     ..done.
    ##  ....clustering..
    ##  ....detecting modules..
    ##  ....calculating module eigengenes..
    ##  ....checking kME in modules..
    ##      ..removing 44 genes from module 1 because their KME is too low.
    ##      ..removing 65 genes from module 2 because their KME is too low.
    ##      ..removing 34 genes from module 3 because their KME is too low.
    ##      ..removing 58 genes from module 4 because their KME is too low.
    ##      ..removing 9 genes from module 5 because their KME is too low.
    ##      ..removing 3 genes from module 6 because their KME is too low.
    ##      ..removing 5 genes from module 7 because their KME is too low.
    ##      ..removing 1 genes from module 8 because their KME is too low.
    ##      ..removing 1 genes from module 14 because their KME is too low.
    ##      ..removing 1 genes from module 15 because their KME is too low.
    ##  ..merging modules that are too close..
    ##      mergeCloseModules: Merging modules whose distance is less than 0.25
    ##        Calculating new MEs...

View modules.

``` r
moduleEigengenes = moduleEigengenes(datExpr, colors = net$colors)$eigengenes

length(table(net$unmergedColors))
```

    ## [1] 39

``` r
length(table(net$colors))
```

    ## [1] 24

``` r
MEs<-net$MEs
moduleLabels<-net$colors
```

There are 24 modules after merging similar modules from original 39 modules.  

Determine whether mRNA and or wgbs features are present in each module.  

``` r
# Get gene names and corresponding module colors
gene_module_info <- data.frame(
  Gene = colnames(datExpr),
  Module = moduleLabels
)

# Check structure
head(gene_module_info)
```

    ##                  Gene Module
    ## FUN_002326 FUN_002326      9
    ## FUN_002305 FUN_002305      9
    ## FUN_002309 FUN_002309      9
    ## FUN_002311 FUN_002311      9
    ## FUN_002314 FUN_002314      9
    ## FUN_002320 FUN_002320      9

``` r
#Add ME to all the names
gene_module_info$Module <- paste0("ME", gene_module_info$Module)
```

Classify modules based on the proportion of the module comprised by mRNAs.  

``` r
# Function to calculate the proportion of mRNAs (genes with "FUN" in ID)
calculate_mRNA_proportion <- function(genes) {
  total_genes <- length(genes)
  mRNA_count <- sum(grepl("FUN", genes))
  
  # Proportion of mRNAs
  proportion_mRNA <- mRNA_count / total_genes
  return(proportion_mRNA)
}

# Apply the function to each module
module_mRNA_proportion <- tapply(gene_module_info$Gene, 
                                 gene_module_info$Module, 
                                 calculate_mRNA_proportion)

# View the proportions
module_mRNA_proportion
```

    ##       ME0       ME1      ME10      ME11      ME12      ME13      ME14      ME15 
    ## 0.7862481 0.9992572 1.0000000 0.0000000 0.7473684 0.9893617 0.9888889 1.0000000 
    ##      ME16      ME17      ME18      ME19       ME2      ME20      ME21      ME22 
    ## 0.9882353 0.9642857 0.9250000 0.6142857 0.9920213 1.0000000 0.5781250 0.9344262 
    ##      ME23       ME3       ME4       ME5       ME6       ME7       ME8       ME9 
    ## 0.3617021 0.9916782 0.9893276 0.9874372 0.9869792 0.8126984 0.9776786 0.8571429

Most modules are a mix of the two, but some have only mRNA and some have only CpGs.  

### Run correlation between modules.

``` r
cor_matrix = cor(moduleEigengenes)
```

Compute correlations with Spearman correlation and BH p-value adjustment.  

``` r
# Compute Spearman correlation between mRNA and lncRNA
apul_cor_results <- corr.test(moduleEigengenes, method = "spearman", adjust = "BH")

# Extract correlation values and p-values
apul_cor_matrix <- apul_cor_results$r  # Correlation coefficients
apul_p_matrix <- apul_cor_results$p  # Adjusted p-values
```

Construct network.  

``` r
# Set correlation and significance thresholds
cor_threshold <- 0.6  # Adjust based on desired stringency
p_threshold <- 0.05

# Convert correlation matrix into an edge list
apul_significant_edges <- which(abs(apul_cor_matrix) > cor_threshold & apul_p_matrix < p_threshold, arr.ind = TRUE)

apul_edge_list <- data.frame(
  mRNA = rownames(apul_cor_matrix)[apul_significant_edges[,1]],
  CpG = colnames(apul_cor_matrix)[apul_significant_edges[,2]],
  correlation = apul_cor_matrix[apul_significant_edges]
)

# Construct network graph
apul_network <- graph_from_data_frame(apul_edge_list, directed = FALSE)

module_mRNA_proportion<-as.data.frame(module_mRNA_proportion)

V(apul_network)$prop_mrna <- module_mRNA_proportion$module_mRNA_proportion[match(V(apul_network)$name, rownames(module_mRNA_proportion))]
```

Plot network.  

``` r
# Visualize network
plot1<-ggraph(apul_network, layout = "fr") +  # Force-directed layout
  geom_edge_link(aes(edge_alpha = correlation), show.legend = TRUE, width=3) +
  geom_node_point(aes(colour=prop_mrna), size = 5) +
  scale_colour_gradient(name="Prop. mRNA", low = "purple", high = "cyan3")+
  geom_node_text(aes(label = name), repel = TRUE, size = 4) +
  theme_void() +
  labs(title = "A. pulchra mRNA-CpG Network");plot1
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_mrna_cpg_cor_network.png?raw=true)  

``` r
ggsave(plot1, filename="D-Apul/output/18-Apul-multiomic-correlation-networks/Apulchra_mrna_cpg_cor_network.png", width=8, height=8)
```

This is pretty neat! There are clearly many linkages between modules of mRNA/CpG features.  

### Plot eigengene patterns and proportions of mRNA and lncRNAs

``` r
module_mRNA_proportion$module_CpG_proportion<-1-module_mRNA_proportion$module_mRNA_proportion
```

View total size of modules.  

``` r
module_sizes <- table(moduleLabels)
module_sizes<-as.data.frame(module_sizes)
module_sizes$module<-paste0("ME", module_sizes$moduleLabels)
```

Plot a stacked bar plot.  

``` r
stack_data<-module_mRNA_proportion
stack_data$module<-rownames(stack_data)
stack_data$size<-module_sizes$Freq[match(stack_data$module, module_sizes$module)]

stack_data$module <- factor(stack_data$module, 
                             levels = rev(stack_data$module[order(stack_data$size)]))

stack_data<-stack_data%>%
  mutate(mRNAs=module_mRNA_proportion*size)%>%
  mutate(CpGs=module_CpG_proportion*size)%>%
  select(!c(module_mRNA_proportion, module_CpG_proportion, size))


# Reshape the data for ggplot (long format)

stack_long <- melt(stack_data[, c("module", "mRNAs", "CpGs")], 
                    id.vars = "module",
                    variable.name = "Feature", 
                    value.name = "Count")

plot2<-ggplot(stack_long, aes(x = module, y = Count, fill = Feature)) +
  geom_bar(stat = "identity") +
  scale_fill_manual(values = c("mRNAs" = "skyblue", 
                               "CpGs" = "salmon"), 
                    labels = c("mRNA", "CpGs"))+
  theme_classic() +
  labs(title = "A. pulchra module mRNA and CpG components",
       x = "Module",
       y = "Counts",
       fill = "Feature") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1));plot2
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_mrna_cpg_module_features.png?raw=true)  

There are several modules that contain both mRNA and CpG features. There is one module (ME11), that is all CpG features.  

``` r
ggsave(plot2, filename="D-Apul/output/18-Apul-multiomic-correlation-networks/Apulchra_mrna_cpg_module_features.png", width=8, height=6)
```

Next plot eigengene expression of each module across samples.  

``` r
#convert wide format to long format for plotting  
head(moduleEigengenes)
```

``` r
plot_MEs <- moduleEigengenes 

plot_MEs$sample<-rownames(plot_MEs)

plot_MEs<-plot_MEs%>%
  pivot_longer(
    cols = where(is.numeric),  # Select only numeric columns
    names_to = "Module",       # Name for the new column containing the column names
    values_to = "Mean"         # Name for the new column containing the values
  )

expression_plots<-plot_MEs%>%
  group_by(Module) %>%
  
  ggplot(aes(x=sample, y=Mean)) +
  facet_wrap(~ Module)+
    geom_hline(yintercept = 0, linetype="dashed", color = "grey")+
  geom_point()+
  geom_line(aes(group=1))+
  ggtitle("Acropora pulchra mRNA-CpG modules")+
  theme_classic()+ 
  theme(axis.text.x=element_text(angle = 90, hjust=1)); expression_plots
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_module_expression_sample.png?raw=true)  

``` r
ggsave(expression_plots, filename="D-Apul/output/18-Apul-multiomic-correlation-networks/Apulchra_module_expression_sample.png", width=20, height=14)
```
We can see here that module 11 is elevated in only one sample. We will likely need to disregard this module for additional analysis - but it would be interesting to see why this group of CpGs is different in only one sample.   

Plot expression by time point.  

``` r
plot_MEs$Timepoint<-metadata$Timepoint[match(plot_MEs$sample, rownames(metadata))]

expression_plots2<-plot_MEs%>%
  group_by(Module) %>%
  
  ggplot(aes(x=Timepoint, y=Mean)) +
  facet_wrap(~ Module)+
  geom_hline(yintercept = 0, linetype="dashed", color = "grey")+
  geom_boxplot()+
  geom_point()+
  #geom_line(aes(group=Timepoint))+
  ggtitle("Acropora pulchra mRNA-CpG modules")+
  theme_classic()+ 
  theme(axis.text.x=element_text(angle = 90, hjust=1)); expression_plots2
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_module_expression_timepoint.png?raw=true) 

``` r
ggsave(expression_plots2, filename="D-Apul/output/18-Apul-multiomic-correlation-networks/Apulchra_module_expression_timepoint.png", width=20, height=14)
```

Some module expression does appear to change by time point.    

Plot expression by site.  

``` r
plot_MEs$Site<-metadata$Site[match(plot_MEs$sample, rownames(metadata))]

expression_plots3<-plot_MEs%>%
  group_by(Module) %>%
  
  ggplot(aes(x=Site, y=Mean)) +
  facet_wrap(~ Module)+
  geom_hline(yintercept = 0, linetype="dashed", color = "grey")+
  geom_boxplot()+
  geom_point()+
  #geom_line(aes(group=Timepoint))+
  ggtitle("Acropora pulchra mRNA-CpG modules")+
  theme_classic()+ 
  theme(axis.text.x=element_text(angle = 90, hjust=1)); expression_plots3
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_module_expression_site.png?raw=true) 

``` r
ggsave(expression_plots3, filename="D-Apul/output/18-Apul-multiomic-correlation-networks/Apulchra_module_expression_site.png", width=20, height=14)
```

Some modules may be related to site, but we will conduct module-trait correlations next to check.  

### Plot correlations of specific modules

Which modules were significantly correlated? Show correlation matrix.

``` r
# Compute Spearman correlation between mRNA and lncRNA
apul_cor_results 
```

``` r
# Extract correlation values and p-values
apul_cor_matrix  # Correlation coefficients
```

``` r
apul_p_matrix  # Adjusted p-values
```

``` r
plot3<-ggcorrplot(apul_cor_results$r, 
           type = "lower", # Only plot lower triangle
           p.mat = apul_p_matrix, 
           sig.level = 0.05,  # Show significant correlations
           insig = "blank",  # Remove insignificant correlations
           lab = TRUE,  # Show correlation coefficients
           lab_size = 4,  # Label size
           colors = c("blue", "white", "red"),  # Color gradient
           title = "A. pulchra Module Correlation Matrix");plot3
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_module_cor_matrix.png?raw=true) 

``` r
ggsave(plot3, filename="D-Apul/output/18-Apul-multiomic-correlation-networks/Apulchra_module_cor_matrix.png", width=10, height=8)
```

## Plot correlations between modules and physiological metrics and other metadata

Add a column in the phys data as a 0 or 1 for each time point to allow us to conduct correlations to each specific time point.

Additionally, add in a column to indicate site.  

``` r
phys$Site<-metadata$Site[match(rownames(phys), rownames(metadata))]
phys$Timepoint<-metadata$Timepoint[match(rownames(phys), rownames(metadata))]

head(phys)
```

``` r
phys<-phys%>%
  mutate(Mahana = if_else(Site=="Mahana", 1, 0))%>%
  mutate(Manava = if_else(Site=="Manava", 1, 0))%>%
  select(!Site)

phys<-phys%>%
  mutate(TP1 = if_else(Timepoint=="TP1", 1, 0))%>%
  mutate(TP2 = if_else(Timepoint=="TP2", 1, 0))%>%
  mutate(TP3 = if_else(Timepoint=="TP3", 1, 0))%>%
  mutate(TP4 = if_else(Timepoint=="TP4", 1, 0))%>%
  select(!Timepoint)

head(phys)
```

``` r
setdiff(rownames(moduleEigengenes), rownames(phys))
```

    ## [1] "ACR-265-TP4"

``` r
#sample ACR-265-TP4 is not in the phys dataset, remove for this analysis 

cor_modules <- moduleEigengenes[!rownames(moduleEigengenes) %in% "ACR-265-TP4", ]

# Compute correlation between module eigengenes and physiological traits
module_trait_cor <- cor(cor_modules, phys, use = "pairwise.complete.obs", method = "pearson")

# Compute p-values for significance testing
module_trait_pvalues <- corPvalueStudent(module_trait_cor, nrow(datExpr))

# Print results
print(module_trait_cor)   # Correlation values
```

``` r
print(module_trait_pvalues)   # P-values
```

Visualize these correlations using a complex heatmap.  

``` r
# Define significance threshold (e.g., p < 0.05)
significance_threshold <- 0.05

# Create a matrix of text labels for pheatmap
text_labels <- ifelse(module_trait_pvalues < significance_threshold, 
                      paste0("**", round(module_trait_cor, 2), "**"),  # Asterick significant values
                      round(module_trait_cor, 2))  # Regular for non-significant values

# Convert to matrix to ensure proper display
text_labels <- matrix(text_labels, nrow = nrow(module_trait_cor), dimnames = dimnames(module_trait_cor))

# Generate the heatmap
plot4<-pheatmap(module_trait_cor, 
         main = "Module-Physiology Correlation", 
         display_numbers = text_labels, 
         cluster_rows = TRUE, cluster_cols = FALSE);plot4
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/wgcna/20240214/Apulchra_module_trait_heatmap.png?raw=true) 

``` r
ggsave(plot4, filename="D-Apul/output/18-Apul-multiomic-correlation-networks/Apulchra_module_trait_heatmap.png", width=10, height=8)
```

I next need to plot individual modules against traits to view the
relationships.
