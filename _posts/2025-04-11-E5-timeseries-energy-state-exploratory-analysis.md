---
layout: post
title: E5 timeseries energy state exploratory analysis Part 1
date: '2025-04-24'
categories: E5
tags: Physiology R 
---

This post details our first attempt at an exploratory analysis to examine gene expression indicators of energetic state. We examined expression of genes involved in key metabolic pathways of energy metabolism and storage across time in our E5 time series molecular project. 

# Overview 

We followed these steps: 

- Downloaded all of the proteins from Uniprot that contained that GO term and made that a database 
- BLASTp query of protein fasta of a coral species against this database
- From the hits at evalue of 10-20 we did PCA to see if there were visual differences across time points.
- Ran a 4 group (4 time points) supervised PLSDA to determine support for separation and extracted VIPs >1
- Visualized expression of top differentiating VIPs across time points 
- Ran PCA of VIPs >1 to calculated PC1 as our measure of the X axis in the toy model
- Analysis conducted for each species separately

The GO terms we selected for exploration were: 

- Glycolytic metabolism GO:0006096
- Gluconeogenesis GO:0006094
- Lipid catabolism GO:0016042
- Fatty acid beta oxidation GO:0006635
- Starvation GO:0042594
- Lipid biosynthesis GO:000861
- Protein catabolic process GO:0030163

Steven identified the genes [in this notebook post here](https://sr320.github.io/tumbling-oysters/posts/41-RI-coral/).   

Scripts can be found [on GitHub here for A. pulchra](https://github.com/urol-e5/timeseries_molecular/blob/main/D-Apul/code/23-Apul-energetic-state.Rmd) and [P. evermanni](https://github.com/urol-e5/timeseries_molecular/blob/main/E-Peve/code/07-Peve-energetic-state.Rmd). We will run analyses on P. tuahiniensis next.     

# tl;dr 

Gene expression of key GO terms may provide an indicator of energetic state. We need to think about how we groundtruth gene expression in these categories with metabolomic/lipidomic/physiological data to infer relationship to energetic state.  

There is temporal variation in gene expression of genes in these GO categories. The strongest differentiating genes show expression show to distinct seasonal periods: November/January and March/September. March and September correspond with the high temperature and high light seasons while November and January are the cooler seasons. 
 
Expression of genes in each GO term significantly correlate with calcification. This makes sense because calcification was highest in March-September. 

There is some variation between A. pulchra and P. evermanni. For example, gene expression correlates with antioxidant capacity in P. evermanni but not A. pulchra. This is due to variation in seasonal physiology responses in these species.  

I have included my full analysis below for A. pulchra. 

# A. pulchra 

Analysis of gene expression of A pulchra energetic state using the
following gene set GO categories:

## Set up

Load libraries

``` r
library(ggplot2)
library(vegan)
library(mixOmics)
library(readxl)
library(factoextra)
library(ggfortify)
library(ComplexHeatmap)
library(viridis)
library(lme4)
library(lmerTest)
library(emmeans)
library(broom.mixed)
library(broom)
library(tidyverse)
library(RVAideMemoire)
library(Hmisc)
library(corrplot)
```

## Read in gene count matrix

Gene count matrix for all genes

``` r
# raw gene counts data (will filter and variance stabilize)
Apul_genes <- read_csv("D-Apul/output/02.20-D-Apul-RNAseq-alignment-HiSat2/apul-gene_count_matrix.csv")
```

``` r
Apul_genes <- as.data.frame(Apul_genes)

# format gene IDs as rownames (instead of a column)
rownames(Apul_genes) <- Apul_genes$gene_id
Apul_genes <- Apul_genes%>%dplyr::select(!gene_id)

# load and format metadata
metadata <- read_csv("M-multi-species/data/rna_metadata.csv")%>%dplyr::select(AzentaSampleName, ColonyID, Timepoint) %>%
  filter(grepl("ACR", ColonyID))
```

``` r
metadata$Sample <- paste(metadata$ColonyID, metadata$Timepoint, sep = "_")

colonies <- unique(metadata$ColonyID)

# Load physiological data
phys<-read_csv("https://github.com/urol-e5/timeseries/raw/refs/heads/master/time_series_analysis/Output/master_timeseries.csv")%>%filter(colony_id_corr %in% colonies)%>%
  dplyr::select(colony_id_corr, species, timepoint, site, Host_AFDW.mg.cm2, Sym_AFDW.mg.cm2, Am, AQY, Rd, Ik, Ic, calc.umol.cm2.hr, cells.mgAFDW, prot_mg.mgafdw, Ratio_AFDW.mg.cm2, Total_Chl, Total_Chl_cell, cre.umol.mgafdw)
```

``` r
# format timepoint
phys$timepoint <- gsub("timepoint", "TP", phys$timepoint)

#add column with full sample info
phys <- merge(phys, metadata, by.x = c("colony_id_corr", "timepoint"), by.y = c("ColonyID", "Timepoint")) %>%
  dplyr::select(-AzentaSampleName)
  
#add site information into metadata 
metadata$Site<-phys$site[match(metadata$ColonyID, phys$colony_id_corr)]

# Rename gene column names to include full sample info (as in miRNA table)
colnames(Apul_genes) <- metadata$Sample[match(colnames(Apul_genes), metadata$AzentaSampleName)]
```

## Gene set 1: Glycolysis GO:0006096

Load in gene set generated by Apul-energy-go script

``` r
glycolysis_go<-read_table(file="D-Apul/output/23-Apul-energy-GO/Apul_blastp-GO:0006096_out.tab")%>%pull(var=1)
```

``` r
glycolysis_go <- str_remove(glycolysis_go, "-T1$")
glycolysis_go <- str_remove(glycolysis_go, "-T2$")
```

Subset gene count matrix for this gene set.

``` r
glycolysis_genes<-Apul_genes%>%
  filter(rownames(.) %in% glycolysis_go)
```

Calculate the sum of the total gene set for each sample.

``` r
glycolysis_genes<-as.data.frame(t(glycolysis_genes))

glycolysis_genes$Sample<-rownames(glycolysis_genes)

glycolysis_genes<-glycolysis_genes %>%
  rowwise() %>%
  mutate(glycolysis_count = sum(c_across(where(is.numeric)))) %>%
  ungroup()%>%
  as.data.frame()
```

Merge into master data frame with metadata and physiology as a new
column called “glycolysis”.

``` r
data<-left_join(phys, glycolysis_genes)
```

Plot over timepoints.

``` r
plot<-data%>%
  ggplot(aes(x=timepoint, y=glycolysis_count, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-7-1.png?raw=true)<!-- -->

Plot as a PCA.

``` r
pca_data <- data %>% dplyr::select(c(starts_with("FUN"), colony_id_corr, timepoint))

# Identify numeric columns
numeric_cols <- sapply(pca_data, is.numeric)

# Among numeric columns, find those with non-zero sum
non_zero_cols <- colSums(pca_data[, numeric_cols]) != 0

# Combine non-numeric columns with numeric columns that have non-zero sum
pca_data_cleaned <- cbind(
  pca_data[, !numeric_cols],                  # All non-numeric columns
  pca_data[, numeric_cols][, non_zero_cols]   # Numeric columns with non-zero sum
)
```

``` r
scaled.pca<-prcomp(pca_data_cleaned%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_cleaned%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_cleaned, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_cleaned, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)   
    ## timepoint  3      914 0.15721 2.1763  0.004 **
    ## Residual  35     4900 0.84279                 
    ## Total     38     5814 1.00000                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Significant differences in glycolysis gene expression profile between
time points.

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_cleaned, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-11-1.png?raw=true)<!-- -->

Which genes are driving this? Run PLSDA and VIP.

``` r
#assigning datasets 
X <- pca_data_cleaned
                
levels(as.factor(X$timepoint))
```

``` r
Y <- as.factor(X$timepoint) #select treatment names
Y
```

``` r
X<-X%>%dplyr::select(where(is.numeric)) #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y) # 1 Run the method
            
plotIndiv(MyResult.plsda, ind.names = FALSE, legend=TRUE, legend.title = "Glycolysis", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-12-1.png?raw=true)<!-- -->

Extract VIPs.

``` r
#extract
treatment_VIP <- PLSDA.VIP(MyResult.plsda)
treatment_VIP_df <- as.data.frame(treatment_VIP[["tab"]])
treatment_VIP_df
```

``` r
# Converting row names to column
treatment_VIP_table <- rownames_to_column(treatment_VIP_df, var = "Gene")

#filter for VIP > 1
treatment_VIP_1 <- treatment_VIP_table %>% 
  filter(VIP >= 1.5)

#plot
VIP_list_plot<-treatment_VIP_1 %>%
            arrange(VIP) %>%
  
  ggplot( aes(x = VIP, y = reorder(Gene,VIP,sum))) +
  geom_point() +
  ylab("Gene") +
  xlab("VIP Score") +
  ggtitle("Glycolysis") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"));VIP_list_plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-13-1.png?raw=true)<!-- -->

Gene FUN_010519 is the most important - plot this. This is also the gene
most important for lipolysis.

``` r
plot<-data%>%
  ggplot(aes(x=timepoint, y=FUN_010519, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-14-1.png?raw=true)<!-- -->

Plot second most important.

``` r
plot<-data%>%
  ggplot(aes(x=timepoint, y=FUN_004577, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-15-1.png?raw=true)<!-- -->

Plot third most important.

``` r
plot<-data%>%
  ggplot(aes(x=timepoint, y=FUN_037979, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-16-1.png?raw=true)<!-- -->

Look at a PCA of the differentiating genes.

``` r
#extract list of VIPs
vip_genes<-treatment_VIP_1%>%pull(Gene)

#turn to wide format with 
pca_data_vips<-pca_data_cleaned%>%dplyr::select(all_of(c("timepoint", "colony_id_corr", vip_genes)))
```

``` r
scaled.pca<-prcomp(pca_data_vips%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_vips%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_vips, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_vips, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)    
    ## timepoint  3   206.58 0.45302 9.6627  0.001 ***
    ## Residual  35   249.42 0.54698                  
    ## Total     38   456.00 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Significant differences in glycolysis gene expression profile between
time points.

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_vips, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Glycolysis")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-20-1.png?raw=true)<!-- -->

Pull out PC1 score for each sample for GO term.

``` r
scores <- scaled.pca$x
scores<-as.data.frame(scores)
scores<-scores%>%dplyr::select(PC1)

scores$sample<-pca_data_vips$colony_id_corr
scores$timepoint<-pca_data_vips$timepoint

scores<-scores%>%
  rename(glycolysis=PC1)

head(scores)
```

## Gene set 2: Gluconeogenesis GO:0006094

Load in gene set generated by Apul-energy-go script.  

``` r
gluconeo_go<-read_table(file="D-Apul/output/23-Apul-energy-GO/Apul_blastp-GO:0006094_out.tab")%>%pull(var=1)
```

``` r
gluconeo_go <- str_remove(gluconeo_go, "-T1$")
gluconeo_go <- str_remove(gluconeo_go, "-T2$")
gluconeo_go <- str_remove(gluconeo_go, "-T3$")
```

Subset gene count matrix for this gene set.

``` r
gluconeo_genes<-Apul_genes%>%
  filter(rownames(.) %in% gluconeo_go)
```

Calculate the sum of the total gene set for each sample.

``` r
gluconeo_genes<-as.data.frame(t(gluconeo_genes))

gluconeo_genes$Sample<-rownames(gluconeo_genes)

gluconeo_genes<-gluconeo_genes %>%
  rowwise() %>%
  mutate(gluconeo_count = sum(c_across(where(is.numeric)))) %>%
  ungroup()%>%
  as.data.frame()
```

Merge into master data frame with metadata and physiology as a new
column called “glycolysis”.

``` r
data2<-left_join(phys, gluconeo_genes)
```

    ## Joining with `by = join_by(Sample)`

Plot over timepoints.

``` r
plot<-data2%>%
  ggplot(aes(x=timepoint, y=gluconeo_count, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-26-1.png?raw=true)<!-- -->

Plot as a PCA.

``` r
pca_data <- data2 %>% dplyr::select(c(starts_with("FUN"), colony_id_corr, timepoint))

# Identify numeric columns
numeric_cols <- sapply(pca_data, is.numeric)

# Among numeric columns, find those with non-zero sum
non_zero_cols <- colSums(pca_data[, numeric_cols]) != 0

# Combine non-numeric columns with numeric columns that have non-zero sum
pca_data_cleaned <- cbind(
  pca_data[, !numeric_cols],                  # All non-numeric columns
  pca_data[, numeric_cols][, non_zero_cols]   # Numeric columns with non-zero sum
)
```

``` r
scaled.pca<-prcomp(pca_data_cleaned%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_cleaned%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_cleaned, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_cleaned, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)    
    ## timepoint  3   1213.5 0.20212 2.9554  0.001 ***
    ## Residual  35   4790.5 0.79788                  
    ## Total     38   6004.0 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Significant differences in gluconeo gene expression profile between time
points.

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_cleaned, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-30-1.png?raw=true)<!-- -->

Which genes are driving this? Run PLSDA and VIP.

``` r
#assigning datasets 
X <- pca_data_cleaned
                
levels(as.factor(X$timepoint))
```

``` r
Y <- as.factor(X$timepoint) #select treatment names
Y
```

``` r
X<-X%>%dplyr::select(where(is.numeric)) #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y) # 1 Run the method
            
plotIndiv(MyResult.plsda, ind.names = FALSE, legend=TRUE, legend.title = "Gluconeogenesis", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-31-1.png?raw=true)<!-- -->

Extract VIPs.

``` r
#extract
treatment_VIP <- PLSDA.VIP(MyResult.plsda)
treatment_VIP_df <- as.data.frame(treatment_VIP[["tab"]])
treatment_VIP_df
```

``` r
# Converting row names to column
treatment_VIP_table <- rownames_to_column(treatment_VIP_df, var = "Gene")

#filter for VIP > 1
treatment_VIP_1 <- treatment_VIP_table %>% 
  filter(VIP >= 1)

#plot
VIP_list_plot<-treatment_VIP_1 %>%
            arrange(VIP) %>%
  
  ggplot( aes(x = VIP, y = reorder(Gene,VIP,sum))) +
  geom_point() +
  ylab("Gene") +
  xlab("VIP Score") +
  ggtitle("Gluconeogenesis") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"));VIP_list_plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-32-1.png?raw=true)<!-- -->

Gene FUN_001654 is the most important - plot this. This is also the gene
most important for lipolysis.

``` r
plot<-data2%>%
  ggplot(aes(x=timepoint, y=FUN_001654, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-33-1.png?raw=true)<!-- -->

Plot second most important.

``` r
plot<-data2%>%
  ggplot(aes(x=timepoint, y=FUN_007020, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-34-1.png?raw=true)<!-- -->

Plot third most important.

``` r
plot<-data2%>%
  ggplot(aes(x=timepoint, y=FUN_010519, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-35-1.png?raw=true)<!-- -->

Look at a PCA of the differentiating genes.

``` r
#extract list of VIPs
vip_genes<-treatment_VIP_1%>%pull(Gene)

#turn to wide format with 
pca_data_vips<-pca_data_cleaned%>%dplyr::select(all_of(c("timepoint", "colony_id_corr", vip_genes)))
```

``` r
scaled.pca<-prcomp(pca_data_vips%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_vips%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_vips, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_vips, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)    
    ## timepoint  3   845.59 0.30483 5.1157  0.001 ***
    ## Residual  35  1928.41 0.69517                  
    ## Total     38  2774.00 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Significant differences in gluconeogenesis gene expression profile
between time points.

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_vips, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Gluconeogenesis")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-39-1.png?raw=true)<!-- -->

Pull out PC1 score for each sample for GO term.

``` r
scores1 <- scaled.pca$x
scores1<-as.data.frame(scores1)
scores1<-scores1%>%dplyr::select(PC1)

scores1$sample<-pca_data_vips$colony_id_corr
scores1$timepoint<-pca_data_vips$timepoint

scores1<-scores1%>%
  rename(gluconeogenesis=PC1)

scores<-left_join(scores, scores1)
```

``` r
head(scores)
```

## Gene set 3: Lipolysis/lipid catabolism GO:0016042

Load in gene set generated by Apul-energy-go script

``` r
lipolysis_go<-read_table(file="D-Apul/output/23-Apul-energy-GO/Apul_blastp-GO:0016042_out.tab")%>%pull(var=1)
```

``` r
lipolysis_go <- str_remove(lipolysis_go, "-T1$")
lipolysis_go <- str_remove(lipolysis_go, "-T2$")
lipolysis_go <- str_remove(lipolysis_go, "-T3$")
lipolysis_go <- str_remove(lipolysis_go, "-T4$")
```

Subset gene count matrix for this gene set.

``` r
lipolysis_genes<-Apul_genes%>%
  filter(rownames(.) %in% lipolysis_go)
```

Calculate the sum of the total gene set for each sample.

``` r
lipolysis_genes<-as.data.frame(t(lipolysis_genes))

lipolysis_genes$Sample<-rownames(lipolysis_genes)

lipolysis_genes<-lipolysis_genes %>%
  rowwise() %>%
  mutate(lipolysis_count = sum(c_across(where(is.numeric)))) %>%
  ungroup()%>%
  as.data.frame()
```

``` r
data3<-left_join(phys, lipolysis_genes)
```

    ## Joining with `by = join_by(Sample)`

Plot over timepoints.

``` r
plot<-data3%>%
  ggplot(aes(x=timepoint, y=lipolysis_count, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-45-1.png?raw=true)<!-- -->

Plot as a PCA.

``` r
pca_data <- data3 %>% dplyr::select(c(starts_with("FUN"), colony_id_corr, timepoint))

# Identify numeric columns
numeric_cols <- sapply(pca_data, is.numeric)

# Among numeric columns, find those with non-zero sum
non_zero_cols <- colSums(pca_data[, numeric_cols]) != 0

# Combine non-numeric columns with numeric columns that have non-zero sum
pca_data_cleaned <- cbind(
  pca_data[, !numeric_cols],                  # All non-numeric columns
  pca_data[, numeric_cols][, non_zero_cols]   # Numeric columns with non-zero sum
)
```

``` r
scaled.pca<-prcomp(pca_data_cleaned%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_cleaned%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_cleaned, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_cleaned, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)   
    ## timepoint  3   4717.2 0.15854 2.1981  0.005 **
    ## Residual  35  25036.8 0.84146                 
    ## Total     38  29754.0 1.00000                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

View by timepoint

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_cleaned, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-49-1.png?raw=true)<!-- -->

Which genes are driving this? Run PLSDA and VIP.

``` r
#assigning datasets 
X <- pca_data_cleaned
                
levels(as.factor(X$timepoint))
```

``` r
Y <- as.factor(X$timepoint) #select treatment names
Y
```

``` r
X<-X%>%dplyr::select(where(is.numeric)) #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y) # 1 Run the method
            
plotIndiv(MyResult.plsda, ind.names = FALSE, legend=TRUE, legend.title = "Lipolysis", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-50-1.png?raw=true)<!-- -->

Extract VIPs.

``` r
#extract
treatment_VIP <- PLSDA.VIP(MyResult.plsda)
treatment_VIP_df <- as.data.frame(treatment_VIP[["tab"]])
treatment_VIP_df
```

``` r
# Converting row names to column
treatment_VIP_table <- rownames_to_column(treatment_VIP_df, var = "Gene")

#filter for VIP > 1
treatment_VIP_1 <- treatment_VIP_table %>% 
  filter(VIP >= 1.5)

#plot
VIP_list_plot<-treatment_VIP_1 %>%
            arrange(VIP) %>%
  
  ggplot( aes(x = VIP, y = reorder(Gene,VIP,sum))) +
  geom_point() +
  ylab("Gene") +
  xlab("VIP Score") +
  ggtitle("Lipolysis") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"));VIP_list_plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-51-1.png?raw=true)<!-- -->

Gene FUN_001654 is the most important - plot this.

``` r
plot<-data3%>%
  ggplot(aes(x=timepoint, y=FUN_001654, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-52-1.png?raw=true)<!-- -->

Plot second most important

``` r
plot<-data3%>%
  ggplot(aes(x=timepoint, y=FUN_035010, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-53-1.png?raw=true)<!-- -->

Plot third most important

``` r
plot<-data3%>%
  ggplot(aes(x=timepoint, y=FUN_017777, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-54-1.png?raw=true)<!-- -->

Look at a PCA of the differentiating genes.

``` r
#extract list of VIPs
vip_genes<-treatment_VIP_1%>%pull(Gene)

#turn to wide format with 
pca_data_vips<-pca_data_cleaned%>%dplyr::select(all_of(c("timepoint", "colony_id_corr", vip_genes)))
```

``` r
scaled.pca<-prcomp(pca_data_vips%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_vips%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_vips, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_vips, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)    
    ## timepoint  3   640.67 0.40142 7.8239  0.001 ***
    ## Residual  35   955.33 0.59858                  
    ## Total     38  1596.00 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Significant differences in lipolysis gene expression profile between
time points.

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_vips, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Lipolysis")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-58-1.png?raw=true)<!-- -->

Pull out PC1 score for each sample for GO term.

``` r
scores1 <- scaled.pca$x
scores1<-as.data.frame(scores1)
scores1<-scores1%>%dplyr::select(PC1)

scores1$sample<-pca_data_vips$colony_id_corr
scores1$timepoint<-pca_data_vips$timepoint

scores1<-scores1%>%
  rename(lipolysis=PC1)

scores<-left_join(scores, scores1)
```


``` r
head(scores)
```

## Gene set 4: Fatty acid beta oxidation GO:0006635

Load in gene set generated by Apul-energy-go script

``` r
ffa_go<-read_table(file="D-Apul/output/23-Apul-energy-GO/Apul_blastp-GO:0006635_out.tab")%>%pull(var=1)
```

``` r
ffa_go <- str_remove(ffa_go, "-T1$")
ffa_go <- str_remove(ffa_go, "-T2$")
ffa_go <- str_remove(ffa_go, "-T3$")
ffa_go <- str_remove(ffa_go, "-T4$")
```

Subset gene count matrix for this gene set.

``` r
ffa_genes<-Apul_genes%>%
  filter(rownames(.) %in% ffa_go)
```

Calculate the sum of the total gene set for each sample.

``` r
ffa_genes<-as.data.frame(t(ffa_genes))

ffa_genes$Sample<-rownames(ffa_genes)

ffa_genes<-ffa_genes %>%
  rowwise() %>%
  mutate(ffa_count = sum(c_across(where(is.numeric)))) %>%
  ungroup()%>%
  as.data.frame()
```

``` r
data4<-left_join(phys, ffa_genes)
```

Plot over timepoints.

``` r
plot<-data4%>%
  ggplot(aes(x=timepoint, y=ffa_count, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-64-1.png?raw=true)<!-- -->

Plot as a PCA.

``` r
pca_data <- data4 %>% dplyr::select(c(starts_with("FUN"), colony_id_corr, timepoint))

# Identify numeric columns
numeric_cols <- sapply(pca_data, is.numeric)

# Among numeric columns, find those with non-zero sum
non_zero_cols <- colSums(pca_data[, numeric_cols]) != 0

# Combine non-numeric columns with numeric columns that have non-zero sum
pca_data_cleaned <- cbind(
  pca_data[, !numeric_cols],                  # All non-numeric columns
  pca_data[, numeric_cols][, non_zero_cols]   # Numeric columns with non-zero sum
)
```

``` r
scaled.pca<-prcomp(pca_data_cleaned%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_cleaned%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_cleaned, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_cleaned, method = "eu")
    ##           Df SumOfSqs     R2      F Pr(>F)   
    ## timepoint  3   1115.7 0.1717 2.4185  0.005 **
    ## Residual  35   5382.3 0.8283                 
    ## Total     38   6498.0 1.0000                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


View by timepoint

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_cleaned, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Fatty Acid Beta Oxidation")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-68-1.png?raw=true)<!-- -->

Which genes are driving this? Run PLSDA and VIP.

``` r
#assigning datasets 
X <- pca_data_cleaned
                
levels(as.factor(X$timepoint))
```

``` r
Y <- as.factor(X$timepoint) #select treatment names
Y
```

``` r
X<-X%>%dplyr::select(where(is.numeric)) #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y) # 1 Run the method
            
plotIndiv(MyResult.plsda, ind.names = FALSE, legend=TRUE, legend.title = "FFA Beta Oxidation", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-69-1.png?raw=true)<!-- -->

Extract VIPs.

``` r
#extract
treatment_VIP <- PLSDA.VIP(MyResult.plsda)
treatment_VIP_df <- as.data.frame(treatment_VIP[["tab"]])
treatment_VIP_df
```

``` r
# Converting row names to column
treatment_VIP_table <- rownames_to_column(treatment_VIP_df, var = "Gene")

#filter for VIP > 1
treatment_VIP_1 <- treatment_VIP_table %>% 
  filter(VIP >= 1)

#plot
VIP_list_plot<-treatment_VIP_1 %>%
            arrange(VIP) %>%
  
  ggplot( aes(x = VIP, y = reorder(Gene,VIP,sum))) +
  geom_point() +
  ylab("Gene") +
  xlab("VIP Score") +
  ggtitle("Beta Oxidation") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"));VIP_list_plot
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-70-1.png?raw=true)<!-- -->

Gene FUN_035010 is the most important - plot this.

``` r
plot<-data3%>%
  ggplot(aes(x=timepoint, y=FUN_035010, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-71-1.png?raw=true)<!-- -->

Plot second most important

``` r
plot<-data3%>%
  ggplot(aes(x=timepoint, y=FUN_014637, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-72-1.png?raw=true)<!-- -->

Plot third most important

``` r
plot<-data3%>%
  ggplot(aes(x=timepoint, y=FUN_031950, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-73-1.png?raw=true)<!-- -->

Look at a PCA of the differentiating genes.

``` r
#extract list of VIPs
vip_genes<-treatment_VIP_1%>%pull(Gene)

#turn to wide format with 
pca_data_vips<-pca_data_cleaned%>%dplyr::select(all_of(c("timepoint", "colony_id_corr", vip_genes)))
```

``` r
scaled.pca<-prcomp(pca_data_vips%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_vips%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_vips, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_vips, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)    
    ## timepoint  3   777.28 0.26914 4.2963  0.001 ***
    ## Residual  35  2110.72 0.73086                  
    ## Total     38  2888.00 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


``` r
plot2<-ggplot2::autoplot(scaled.pca, data=pca_data_vips, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Lipolysis")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot2
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-77-1.png?raw=true)<!-- -->

Pull out PC1 score for each sample for GO term.

``` r
scores1 <- scaled.pca$x
scores1<-as.data.frame(scores1)
scores1<-scores1%>%dplyr::select(PC1)

scores1$sample<-pca_data_vips$colony_id_corr
scores1$timepoint<-pca_data_vips$timepoint

scores1<-scores1%>%
  rename(fa_beta=PC1)

scores<-left_join(scores, scores1)
```

``` r
head(scores)
```

## Gene set 5: Starvation GO:0042594

Load in gene set generated by Apul-energy-go script

``` r
starve_go<-read_table(file="D-Apul/output/23-Apul-energy-GO/Apul_blastp-GO:0042594_out.tab")%>%pull(var=1)
```


``` r
starve_go <- str_remove(starve_go, "-T1$")
starve_go <- str_remove(starve_go, "-T2$")
starve_go <- str_remove(starve_go, "-T3$")
starve_go <- str_remove(starve_go, "-T4$")
```

Subset gene count matrix for this gene set.

``` r
starve_genes<-Apul_genes%>%
  filter(rownames(.) %in% starve_go)
```

Calculate the sum of the total gene set for each sample.

``` r
starve_genes<-as.data.frame(t(starve_genes))

starve_genes$Sample<-rownames(starve_genes)

starve_genes<-starve_genes %>%
  rowwise() %>%
  mutate(starve_count = sum(c_across(where(is.numeric)))) %>%
  ungroup()%>%
  as.data.frame()
```

Merge into master data frame with metadata and physiology as a new
column called “glycolysis”.

``` r
data5<-left_join(phys, starve_genes)
```

    ## Joining with `by = join_by(Sample)`

Plot over timepoints.

``` r
plot<-data5%>%
  ggplot(aes(x=timepoint, y=starve_count, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-83-1.png?raw=true)<!-- -->

Plot as a PCA.

``` r
pca_data <- data5 %>% dplyr::select(c(starts_with("FUN"), colony_id_corr, timepoint))

# Identify numeric columns
numeric_cols <- sapply(pca_data, is.numeric)

# Among numeric columns, find those with non-zero sum
non_zero_cols <- colSums(pca_data[, numeric_cols]) != 0

# Combine non-numeric columns with numeric columns that have non-zero sum
pca_data_cleaned <- cbind(
  pca_data[, !numeric_cols],                  # All non-numeric columns
  pca_data[, numeric_cols][, non_zero_cols]   # Numeric columns with non-zero sum
)
```

``` r
scaled.pca<-prcomp(pca_data_cleaned%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_cleaned%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_cleaned, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_cleaned, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)   
    ## timepoint  3    12602 0.16177 2.2515  0.004 **
    ## Residual  35    65298 0.83823                 
    ## Total     38    77900 1.00000                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_cleaned, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-87-1.png?raw=true)<!-- -->

Which genes are driving this? Run PLSDA and VIP.

``` r
#assigning datasets 
X <- pca_data_cleaned
                
levels(as.factor(X$timepoint))
```

    ## [1] "TP1" "TP2" "TP3" "TP4"

``` r
Y <- as.factor(X$timepoint) #select treatment names
Y
```

``` r
X<-X%>%dplyr::select(where(is.numeric)) #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y) # 1 Run the method
            
plotIndiv(MyResult.plsda, ind.names = FALSE, legend=TRUE, legend.title = "Starvation", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-88-1.png?raw=true)<!-- -->

Extract VIPs.

``` r
#extract
treatment_VIP <- PLSDA.VIP(MyResult.plsda)
treatment_VIP_df <- as.data.frame(treatment_VIP[["tab"]])
treatment_VIP_df
```

    
``` r
# Converting row names to column
treatment_VIP_table <- rownames_to_column(treatment_VIP_df, var = "Gene")

#filter for VIP > 1
treatment_VIP_1 <- treatment_VIP_table %>% 
  filter(VIP >= 1)

#plot
VIP_list_plot<-treatment_VIP_1 %>%
            arrange(VIP) %>%
  
  ggplot( aes(x = VIP, y = reorder(Gene,VIP,sum))) +
  geom_point() +
  ylab("Gene") +
  xlab("VIP Score") +
  ggtitle("Starvation") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"));VIP_list_plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-89-1.png?raw=true)<!-- -->


``` r
plot<-data5%>%
  ggplot(aes(x=timepoint, y=FUN_034029, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-90-1.png?raw=true)<!-- -->

Plot second most important

``` r
plot<-data5%>%
  ggplot(aes(x=timepoint, y=FUN_014794, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-91-1.png?raw=true)<!-- -->

Plot third most important

``` r
plot<-data5%>%
  ggplot(aes(x=timepoint, y=FUN_014886, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-92-1.png?raw=true)<!-- -->

Look at a PCA of the differentiating genes.

``` r
#extract list of VIPs
vip_genes<-treatment_VIP_1%>%pull(Gene)

#turn to wide format with 
pca_data_vips<-pca_data_cleaned%>%dplyr::select(all_of(c("timepoint", "colony_id_corr", vip_genes)))
```

``` r
scaled.pca<-prcomp(pca_data_vips%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_vips%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_vips, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_vips, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)    
    ## timepoint  3     8675 0.24209 3.7266  0.001 ***
    ## Residual  35    27159 0.75791                  
    ## Total     38    35834 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_vips, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Starvation")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-96-1.png?raw=true)<!-- -->

Pull out PC1 score for each sample for GO term.

``` r
scores1 <- scaled.pca$x
scores1<-as.data.frame(scores1)
scores1<-scores1%>%dplyr::select(PC1)

scores1$sample<-pca_data_vips$colony_id_corr
scores1$timepoint<-pca_data_vips$timepoint

scores1<-scores1%>%
  rename(starve=PC1)

scores<-left_join(scores, scores1)
```

``` r
head(scores)
```


## Gene set 6: Lipid biosynthesis GO:0008610

Load in gene set generated by Apul-energy-go script

``` r
lipid_go<-read_table(file="D-Apul/output/23-Apul-energy-GO/Apul_blastp-GO:0008610_out.tab")%>%pull(var=1)
```

``` r
lipid_go <- str_remove(lipid_go, "-T1$")
lipid_go <- str_remove(lipid_go, "-T2$")
lipid_go <- str_remove(lipid_go, "-T3$")
lipid_go <- str_remove(lipid_go, "-T4$")
```

Subset gene count matrix for this gene set.

``` r
lipid_genes<-Apul_genes%>%
  filter(rownames(.) %in% lipid_go)
```

Calculate the sum of the total gene set for each sample.

``` r
lipid_genes<-as.data.frame(t(lipid_genes))

lipid_genes$Sample<-rownames(lipid_genes)

lipid_genes<-lipid_genes %>%
  rowwise() %>%
  mutate(lipid_count = sum(c_across(where(is.numeric)))) %>%
  ungroup()%>%
  as.data.frame()
```

``` r
data6<-left_join(phys, lipid_genes)
```

Plot over timepoints.

``` r
plot<-data6%>%
  ggplot(aes(x=timepoint, y=lipid_count, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-102-1.png?raw=true)<!-- -->

Plot as a PCA.

``` r
pca_data <- data6 %>% dplyr::select(c(starts_with("FUN"), colony_id_corr, timepoint))

# Identify numeric columns
numeric_cols <- sapply(pca_data, is.numeric)

# Among numeric columns, find those with non-zero sum
non_zero_cols <- colSums(pca_data[, numeric_cols]) != 0

# Combine non-numeric columns with numeric columns that have non-zero sum
pca_data_cleaned <- cbind(
  pca_data[, !numeric_cols],                  # All non-numeric columns
  pca_data[, numeric_cols][, non_zero_cols]   # Numeric columns with non-zero sum
)
```

``` r
scaled.pca<-prcomp(pca_data_cleaned%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_cleaned%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_cleaned, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_cleaned, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)   
    ## timepoint  3     9468 0.15779 2.1858  0.005 **
    ## Residual  35    50534 0.84221                 
    ## Total     38    60002 1.00000                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_cleaned, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-106-1.png?raw=true)<!-- -->

Which genes are driving this? Run PLSDA and VIP.

``` r
#assigning datasets 
X <- pca_data_cleaned
                
levels(as.factor(X$timepoint))
```


``` r
Y <- as.factor(X$timepoint) #select treatment names
Y
```


``` r
X<-X%>%dplyr::select(where(is.numeric)) #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y) # 1 Run the method
            
plotIndiv(MyResult.plsda, ind.names = FALSE, legend=TRUE, legend.title = "Lipid Synthesis", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-107-1.png?raw=true)<!-- -->

Extract VIPs.

``` r
#extract
treatment_VIP <- PLSDA.VIP(MyResult.plsda)
treatment_VIP_df <- as.data.frame(treatment_VIP[["tab"]])
treatment_VIP_df
```
``` r
# Converting row names to column
treatment_VIP_table <- rownames_to_column(treatment_VIP_df, var = "Gene")

#filter for VIP > 1
treatment_VIP_1 <- treatment_VIP_table %>% 
  filter(VIP >= 1.5)

#plot
VIP_list_plot<-treatment_VIP_1 %>%
            arrange(VIP) %>%
  
  ggplot( aes(x = VIP, y = reorder(Gene,VIP,sum))) +
  geom_point() +
  ylab("Gene") +
  xlab("VIP Score") +
  ggtitle("Lipid Catabolism") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"));VIP_list_plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-108-1.png?raw=true)<!-- -->

``` r
plot<-data6%>%
  ggplot(aes(x=timepoint, y=FUN_035821, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-109-1.png?raw=true)<!-- -->

Plot second most important

``` r
plot<-data6%>%
  ggplot(aes(x=timepoint, y=FUN_006730, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-110-1.png?raw=true)<!-- -->

Plot third most important

``` r
plot<-data6%>%
  ggplot(aes(x=timepoint, y=FUN_010519, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-111-1.png?raw=true)<!-- -->

Look at a PCA of the differentiating genes.

``` r
#extract list of VIPs
vip_genes<-treatment_VIP_1%>%pull(Gene)

#turn to wide format with 
pca_data_vips<-pca_data_cleaned%>%dplyr::select(all_of(c("timepoint", "colony_id_corr", vip_genes)))
```

``` r
scaled.pca<-prcomp(pca_data_vips%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_vips%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_vips, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_vips, method = "eu")
    ##           Df SumOfSqs      R2      F Pr(>F)    
    ## timepoint  3     1108 0.39404 7.5864  0.001 ***
    ## Residual  35     1704 0.60596                  
    ## Total     38     2812 1.00000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_vips, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Lipid Synthesis")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-115-1.png?raw=true)<!-- -->

Pull out PC1 score for each sample for GO term.

``` r
scores1 <- scaled.pca$x
scores1<-as.data.frame(scores1)
scores1<-scores1%>%dplyr::select(PC1)

scores1$sample<-pca_data_vips$colony_id_corr
scores1$timepoint<-pca_data_vips$timepoint

scores1<-scores1%>%
  rename(lipids=PC1)

scores<-left_join(scores, scores1)
```

``` r
head(scores)
```


## Gene set 7: Protein catabolic process GO:0030163

Load in gene set generated by Apul-energy-go script

``` r
protein_go<-read_table(file="D-Apul/output/23-Apul-energy-GO/Apul_blastp-GO:0030163_out.tab")%>%pull(var=1)
```

``` r
protein_go <- str_remove(protein_go, "-T1$")
protein_go <- str_remove(protein_go, "-T2$")
protein_go <- str_remove(protein_go, "-T3$")
protein_go <- str_remove(protein_go, "-T4$")
```

Subset gene count matrix for this gene set.

``` r
protein_genes<-Apul_genes%>%
  filter(rownames(.) %in% protein_go)
```

Calculate the sum of the total gene set for each sample.

``` r
protein_genes<-as.data.frame(t(protein_genes))

protein_genes$Sample<-rownames(protein_genes)

protein_genes<-protein_genes %>%
  rowwise() %>%
  mutate(protein_count = sum(c_across(where(is.numeric)))) %>%
  ungroup()%>%
  as.data.frame()
```

``` r
data7<-left_join(phys, protein_genes)
```

    ## Joining with `by = join_by(Sample)`

Plot over timepoints.

``` r
plot<-data7%>%
  ggplot(aes(x=timepoint, y=protein_count, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-121-1.png?raw=true)<!-- -->

Plot as a PCA.

``` r
pca_data <- data7 %>% dplyr::select(c(starts_with("FUN"), colony_id_corr, timepoint))

# Identify numeric columns
numeric_cols <- sapply(pca_data, is.numeric)

# Among numeric columns, find those with non-zero sum
non_zero_cols <- colSums(pca_data[, numeric_cols]) != 0

# Combine non-numeric columns with numeric columns that have non-zero sum
pca_data_cleaned <- cbind(
  pca_data[, !numeric_cols],                  # All non-numeric columns
  pca_data[, numeric_cols][, non_zero_cols]   # Numeric columns with non-zero sum
)
```

``` r
scaled.pca<-prcomp(pca_data_cleaned%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_cleaned%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_cleaned, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_cleaned, method = "eu")
    ##           Df SumOfSqs    R2      F Pr(>F)  
    ## timepoint  3    12758 0.155 2.1401  0.011 *
    ## Residual  35    69550 0.845                
    ## Total     38    82308 1.000                
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_cleaned, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-125-1.png?raw=true)<!-- -->

Which genes are driving this? Run PLSDA and VIP.

``` r
#assigning datasets 
X <- pca_data_cleaned
                
levels(as.factor(X$timepoint))
```

``` r
Y <- as.factor(X$timepoint) #select treatment names
Y
```

``` r
X<-X%>%dplyr::select(where(is.numeric)) #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y) # 1 Run the method
            
plotIndiv(MyResult.plsda, ind.names = FALSE, legend=TRUE, legend.title = "Protein Catabolism", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-126-1.png?raw=true)<!-- -->

Extract VIPs.

``` r
#extract
treatment_VIP <- PLSDA.VIP(MyResult.plsda)
treatment_VIP_df <- as.data.frame(treatment_VIP[["tab"]])
treatment_VIP_df
```


``` r
# Converting row names to column
treatment_VIP_table <- rownames_to_column(treatment_VIP_df, var = "Gene")

#filter for VIP > 1
treatment_VIP_1 <- treatment_VIP_table %>% 
  filter(VIP >= 1)

#plot
VIP_list_plot<-treatment_VIP_1 %>%
            arrange(VIP) %>%
  
  ggplot( aes(x = VIP, y = reorder(Gene,VIP,sum))) +
  geom_point() +
  ylab("Gene") +
  xlab("VIP Score") +
  ggtitle("Protein Catabolism") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"));VIP_list_plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-127-1.png?raw=true)<!-- -->


``` r
plot<-data7%>%
  ggplot(aes(x=timepoint, y=FUN_035867, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-128-1.png?raw=true)<!-- -->

Plot second most important

``` r
plot<-data7%>%
  ggplot(aes(x=timepoint, y=FUN_011924, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-129-1.png?raw=true)<!-- -->

Plot third most important

``` r
plot<-data7%>%
  ggplot(aes(x=timepoint, y=FUN_010717, group=colony_id_corr))+
  facet_wrap(~species)+
  geom_point()+
  geom_line()+
  theme_classic();plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-130-1.png?raw=true)<!-- -->

Look at a PCA of the differentiating genes.

``` r
#extract list of VIPs
vip_genes<-treatment_VIP_1%>%pull(Gene)

#turn to wide format with 
pca_data_vips<-pca_data_cleaned%>%dplyr::select(all_of(c("timepoint", "colony_id_corr", vip_genes)))
```

``` r
scaled.pca<-prcomp(pca_data_vips%>%dplyr::select(where(is.numeric)), scale=TRUE, center=TRUE) 
```

Prepare a PCA plot

``` r
# scale data
vegan <- scale(pca_data_vips%>%dplyr::select(where(is.numeric)))

# PerMANOVA 
permanova<-adonis2(vegan ~ timepoint, data = pca_data_vips, method='eu')
permanova
```

    ## Permutation test for adonis under reduced model
    ## Terms added sequentially (first to last)
    ## Permutation: free
    ## Number of permutations: 999
    ## 
    ## adonis2(formula = vegan ~ timepoint, data = pca_data_vips, method = "eu")
    ##           Df SumOfSqs     R2      F Pr(>F)    
    ## timepoint  3     8642 0.2288 3.4613  0.001 ***
    ## Residual  35    29130 0.7712                  
    ## Total     38    37772 1.0000                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1


``` r
plot<-ggplot2::autoplot(scaled.pca, data=pca_data_vips, loadings=FALSE,  colour="timepoint", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  theme_classic()+
  ggtitle("Protein Catabolism")+
   theme(legend.text = element_text(size=18), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));plot
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-134-1.png?raw=true)<!-- -->

Pull out PC1 score for each sample for GO term.

``` r
scores1 <- scaled.pca$x
scores1<-as.data.frame(scores1)
scores1<-scores1%>%dplyr::select(PC1)

scores1$sample<-pca_data_vips$colony_id_corr
scores1$timepoint<-pca_data_vips$timepoint

scores1<-scores1%>%
  rename(protein=PC1)

scores<-left_join(scores, scores1)
```


``` r
head(scores)
```


## Output PC1 scores for each GO term for each sample to calculate a relative score for each function for each sample

``` r
head(scores)
```

``` r
scores<-scores%>%
  rename(colony_id_corr=sample)
```

## Correlate PC scores to physiology metrics to see if energy state in each term relates to physiology 

``` r
#merge scores into physiology data frame
head(phys)
```


``` r
head(scores)
```


``` r
main<-left_join(phys, scores)
```

``` r
head(main)
```


``` r
main<-main%>%
  dplyr::select(where(is.numeric))

head(main)
```

Compute correlations

``` r
# Compute correlations with rcorr
corr_result <- rcorr(as.matrix(main), type = "pearson")

# Extract correlation matrix and p-values
cor_matrix <- corr_result$r
p_matrix <- corr_result$P

# Get correlation between sets of interest only
terms<-c("glycolysis", "gluconeogenesis", "lipolysis", "fa_beta", "starve", "protein", "lipids")
phys_var<-c("Host_AFDW.mg.cm2", "Sym_AFDW.mg.cm2", "Am", "AQY", "Rd", "Ik", "Ic", "calc.umol.cm2.hr", "cells.mgAFDW", "prot_mg.mgafdw", "Ratio_AFDW.mg.cm2", "Total_Chl", "Total_Chl_cell", "cre.umol.mgafdw")

# Subset the correlation matrix
subset_cor <- cor_matrix[terms, phys_var, drop = FALSE]

# Subset the p-value matrix
subset_p <- p_matrix[terms, phys_var, drop = FALSE]
```

Plot heatmap

``` r
# Create matrix of formatted p-values
p_labels <- matrix(sprintf("%.3f", subset_p), nrow = nrow(subset_p))

# Plot with corrplot
corrplot(subset_cor, 
         method = "color",          # Fill squares with color
         col = colorRampPalette(c("blue", "white", "red"))(200),
         type = "full",
         tl.col = "black",          # Text label color
         tl.cex = 1.1,              # Label size
         addCoef.col = "black",     # Show correlation values (optional)
         number.cex = 0.8,          # Text size for numbers
         p.mat = subset_p,             # p-value matrix
         insig = "blank",           # Only show significant
         diag = FALSE               # Hide diagonal
)
```

![](https://raw.githubusercontent.com/AHuffmyer/ASH_Putnam_Lab_Notebook/tree/master/images/NotebookImages/E5_molecular/23-Apul-energetic-state_files/figure-gfm/unnamed-chunk-139-1.png?raw=true)<!-- -->





