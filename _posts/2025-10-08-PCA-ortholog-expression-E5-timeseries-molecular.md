---
layout: post
title: PCA of ortholog expression for E5 timeseries molecular project 
date: '2025-10-08'
categories: E5
tags: GeneExpression
---

# Overview

This notebook describes a **Principal Component Analysis (PCA)** and **PERMANOVA** of **orthologous gene expression** across three coral species (*Acropora*, *Porites*, and *Pocillopora*) and multiple timepoints (T1–T4). The goal is to assess overall variance structure, evaluate differences among species and timepoints, and identify orthologs that contribute most to the primary axes of variation.

The full script can be found [on GitHub here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/14-pca-plsda-ortholog-expression.Rmd).   

Principal Component Analysis (PCA) is an unsupervised multivariate statistical technique used to reduce the dimensionality of large datasets while retaining the majority of their variance. PCA transforms a set of potentially correlated variables (e.g., gene expression values) into a new set of uncorrelated variables called principal components (PCs), which are linear combinations of the original variables. Each principal component is constructed to capture the maximum possible variance in the data under the constraint that it is orthogonal to all preceding components. Mathematically, PCA is performed by eigendecomposition of the covariance (or correlation) matrix or, equivalently, by singular value decomposition (SVD) of the centered data matrix. The first few components, which explain the greatest proportion of total variance, provide a low-dimensional representation of the data that highlights dominant patterns such as sample clustering or major sources of variation. By projecting high-dimensional data into this reduced space, PCA facilitates visualization, noise reduction, and identification of key variables (loadings) driving observed patterns.

# Set Up

Load libraries.   

```
library(vegan)
library(mixOmics)
library(ggplot2)
library(tidyverse)
library(DESeq2)
library(factoextra)
library(matrixStats)
library(ggrepel)

options(stringsAsFactors = FALSE)
knitr::opts_chunk$set(echo = TRUE)
```

# Data Loading and Preparation

I read in ortholog annotations and gene count matrices for three species from MOSAIC outputs and merged them by ortholog group ID.    

```
# Load ortholog annotations
orthos <- read_csv("https://raw.githubusercontent.com/urol-e5/timeseries_molecular/refs/heads/main/M-multi-species/output/12-ortho-annot/ortholog_groups_annotated.csv")

# Load species-specific count matrices
apul <- read_csv("https://gannet.fish.washington.edu/gitrepos/urol-e5/timeseries_molecular/D-Apul/output/02.20-D-Apul-RNAseq-alignment-HiSat2/apul-gene_count_matrix.csv")
peve <- read_csv("https://gannet.fish.washington.edu/gitrepos/urol-e5/timeseries_molecular/E-Peve/output/02.20-E-Peve-RNAseq-alignment-HiSat2/peve-gene_count_matrix.csv")
ptua <- read_csv("https://gannet.fish.washington.edu/gitrepos/urol-e5/timeseries_molecular/F-Ptua/output/02.20-F-Ptua-RNAseq-alignment-HiSat2/ptua-gene_count_matrix.csv")
```

Ortholog groups were selected and merged across species to create a combined count matrix containing 9,800 orthologs present at non-NA values in all 117 samples.   

Metadata was parsed from sample names to extract species and timepoint information.  

# Variance Stabilizing Transformation (VST)

Low-count and incomplete ortholog rows were removed. A variance stabilizing transformation was applied using `DESeq2::vst()` to normalize count variance across the dynamic range blind to study design.  

```
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData   = metadata,
  design    = ~ species * timepoint
)

vst_obj <- vst(dds, blind = TRUE)
vst_mat <- assay(vst_obj)
```

This transformation produces homoskedastic data suitable for multivariate analyses (e.g. PCA, PERMANOVA).  

# PERMANOVA

I then used PERMANOVA to test whether species, timepoint, or their interaction significantly affect global ortholog expression.

```
test <- as.data.frame(t(vst_mat))
test$sample <- rownames(test)
test$species <- metadata$species[match(test$sample, rownames(metadata))]
test$timepoint <- metadata$timepoint[match(test$sample, rownames(metadata))]

vegan <- scale(test[c(1:9800)])
permanova <- adonis2(vegan ~ species * timepoint, data = test, method = 'eu')
permanova
```

```
Results:

Factor	F-statistic	p-value	R²
Species	47.11	0.001	0.43
Timepoint	2.80	0.001	0.04
Species × Timepoint	2.08	0.001	0.06
```

Interpretation:

- Ortholog expression profiles differ strongly among species (R2=0.43), minimally among timepoints (R2=0.04), and show significant, but minimally influential interactions (R2=0.06), indicating strong species-specific ortholog expression.
- Due to strong species effects I'll look at those primarily here.  

# PCA Visualization

I visualized major patterns using PCA on the 9,800 orthologs.

```
gPCAdata <- plotPCA(vst_obj, intgroup = c("species", "timepoint"), returnData=TRUE, ntop=9800)
percentVar <- round(100 * attr(gPCAdata, "percentVar"))

ggplot(gPCAdata, aes(PC1, PC2, color = species, shape = timepoint)) + 
  geom_point(size = 3) +
  stat_ellipse() +
  xlab(paste0("PC1: ", percentVar[1], "% variance")) +
  ylab(paste0("PC2: ", percentVar[2], "% variance")) +
  theme_classic() +
  ggtitle("PCA of VST-transformed ortholog expression (9,800 genes)")
```

PC1 and PC2 together explained ~55% of the total variance.

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251008/plot5.png?raw=true)

I then plotted our biological samples by species in a PCA of PC1 and PC2.    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251008/plot4.png?raw=true)

# Ortholog Loadings

I extracted loading scores to identify orthologs contributing most to PC1 and PC2.

```
pr <- prcomp(t(vst_mat), center = TRUE, scale. = FALSE)
loadings <- as.data.frame(pr$rotation[, 1:2]) %>%
  rownames_to_column("ortholog") %>%
  mutate(magnitude = sqrt(PC1^2 + PC2^2))

Top Loading Orthologs
pc1_lolli <- loadings %>%
  slice_max(order_by = abs(PC1), n = 30) %>%
  mutate(ortholog = reorder(ortholog, abs(PC1)))

ggplot(pc1_lolli, aes(x = abs(PC1), y = ortholog)) +
  geom_segment(aes(x = 0, xend = abs(PC1), yend = ortholog)) +
  geom_point() +
  labs(title = "Top Ortholog Loadings along PC1", x = "PC1 Loading", y = NULL) +
  theme_classic()
```

Loading scores of top 30 orthologs on PC1 are:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251008/plot3.png?raw=true)

Here is what PC2 looks like: 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251008/plot2.png?raw=true)

These orthologs represent the strongest drivers of inter-species differences in ortholog expression.

# Biplot of Samples and Ortholog Loadings

A PCA biplot displays samples colored by species and arrows representing high-loading orthologs.

```
# Build sample score data
scores <- as.data.frame(pr$x[, 1:2]) %>%
  rownames_to_column("sample") %>%
  left_join(as.data.frame(colData(vst_obj)) %>%
              rownames_to_column("sample") %>%
              select(sample, species, timepoint),
            by = "sample")

# Select top 30 loadings
loads_top <- loadings %>%
  arrange(desc(magnitude)) %>%
  slice_head(n = 30)

# Scale arrows
sx <- diff(range(scores$PC1))
sy <- diff(range(scores$PC2))
lx <- diff(range(loads_top$PC1))
ly <- diff(range(loads_top$PC2))
arrow_scale <- 0.7 * min(sx / lx, sy / ly)

ggplot(scores, aes(PC1, PC2, color = species, shape = timepoint)) +
  geom_point(size = 3) +
  stat_ellipse(linewidth = 0.3, linetype = 2, alpha = 0.6) +
  geom_segment(data = loads_top,
               aes(x = 0, y = 0, xend = PC1 * arrow_scale, yend = PC2 * arrow_scale),
               arrow = arrow(length = unit(0.15, "cm")), color = "gray30") +
  geom_text_repel(data = loads_top,
                  aes(x = PC1 * arrow_scale, y = PC2 * arrow_scale, label = ortholog),
                  size = 3, color = "gray20", max.overlaps = 50) +
  labs(title = "PCA biplot: samples + top 30 ortholog loadings",
       x = paste0("PC1 (", round(100*pvar[1]), "%)"),
       y = paste0("PC2 (", round(100*pvar[2]), "%)")) +
  theme_classic()
 ```

Arrows indicate the direction and strength of orthologs influencing PC1 and PC2, helping interpret which ortholog groups drive separation among species or timepoints.

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251008/plot1.png?raw=true)

# Take-Home Points

## 1. What the PCA is Doing

- PCA reduces 9,800 ortholog expression variables into principal components that capture the largest sources of variation.

- For example, PC1 captures the dominant variance separating all species. 

- PC2 captures secondary variance separating *Pocillopora* from *Acropora* and *Porites*. 

- Loadings indicate which orthologs most strongly influence each axis.

## 2. What We Learn from the PCA

- Strong clustering by species shows that each coral genus has a distinct global transcriptomic signature in shared orthologs.

- Minimal separation by timepoint suggests conserved temporal expression dynamics across taxa, although this is largely overwhelmed by species differences. We would need to do a separate analysis within species to examine time point differences. 

- Orthologs with high PC1 loadings may represent divergent core pathways distinguishing taxa. Orthologs with high PC2 loadings may represent orthologs that specifically distinguish *Pocillopora*.  

- Combined with PERMANOVA, these results confirm species identity is the primary driver of ortholog expression patterns. 

# Next Steps

- Examine functional enrichment of top-loading orthologs to interpret biological pathways.

- Compare PCA with sparse tensor analysis.  

- Integrate multi-omic layers (epigenetics, metabolomics, lipidomics) to explore cross-domain variance structure.