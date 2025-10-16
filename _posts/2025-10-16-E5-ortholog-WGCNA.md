---
layout: post
title: WGCNA of ortholog expression for the E5 timeseries molecular project
date: '2025-10-16'
categories: E5
tags: GeneExpression WGCNA R
---

Today I ran a preliminary WGCNA on ortholog expression for the E5 time series project. 

# Weighted Gene Co-expression Network Analysis (WGCNA) of Ortholog Expression Across Coral Species

## Introduction

Weighted Gene Co-expression Network Analysis (WGCNA) is a systems biology method for understanding gene expression patterns and identifying modules of highly correlated genes. In this analysis, we applied WGCNA to orthologous gene expression data from three coral species to identify gene modules that respond coordinately across species, site and time points in the E5 molecular time series project.

This analysis builds upon our previous orthology identification work, which established 9,800 ortholog groups with representatives across three coral species. By examining co-expression patterns of these orthologs, we can identify conserved transcriptional programs and species-specific responses to environmental conditions.

## Study Design

### Species and Samples

The analysis includes RNA-seq data from three coral species representing different ecological strategies:

- **Acropora pulchra (ACR)**: Fast-growing, branching coral (40 samples)
- **Porites evermanni (POR)**: Slow-growing, massive coral (38 samples)  
- **Pocillopora tuahiniensis (POC)**: Intermediate growth, branching coral (39 samples)

### Experimental Design

Samples were collected from Moorea, French Polynesia across four time points (TP1-TP4) representing different environmental conditions. The experiment included:

- **Total samples**: 117 RNA-seq libraries
- **Time points**: 4 developmental/environmental time points per colony
- **Sites**: Manava and Mahana collection sites
- **Biological replicates**: Multiple colonies per species per time point

### Input Data

The analysis used variance-stabilized transformation (VST) normalized counts from the DESeq2 pipeline:

- **Input file**: `vst_counts_matrix.csv` from analysis 14-pca-orthologs
- **Genes**: 9,800 ortholog groups (OG_XXXXX identifiers)
- **Samples**: 117 columns representing individual RNA-seq libraries
- **Format**: VST-normalized expression values across all samples

VST normalization was chosen because it stabilizes variance across the expression range, making the data more suitable for correlation-based analyses like WGCNA.

## WGCNA Methodology

### Network Construction Parameters

The analysis followed the standard WGCNA workflow with the following key parameters:

**1. Data Preparation**

- Removed genes with zero variance (only 4 genes removed, retaining 9,796 genes)
- Validated sample-metadata correspondence
- Transposed matrix for WGCNA input (samples as rows, genes as columns)

**2. Soft-Thresholding Power Selection**

- **Network type**: Signed network (distinguishes positive and negative correlations)
- **Tested powers**: 1-10, 12, 14, 16, 18, 20
- **Selection criterion**: Scale-free topology fit R² ≥ 0.8
- **Chosen power**: 16 (based on achieving scale-free topology)

The soft-thresholding power transforms the correlation matrix into a weighted adjacency matrix, emphasizing strong correlations while downweighting weak ones. A signed network was used to preserve biological directionality of co-expression relationships.

**3. Module Detection**

- **Method**: blockwiseModules (for computational efficiency)
- **TOM type**: Signed Topological Overlap Matrix
- **Correlation type**: Pearson
- **Minimum module size**: 30 genes
- **Merge cut height**: 0.25 (merges modules with eigengene correlation > 0.75)
- **Dynamic tree cut**: Used to identify initial modules

**4. Trait Association**

The analysis examined module correlations with three categorical traits:
- **Time point**: TP1, TP2, TP3, TP4
- **Site**: Manava, Maharepa
- **Species**: Acropora pulchra, Porites evermanni, Pocillopora tuahiniensis

## Results

### Module Identification

WGCNA identified **14 distinct co-expression modules** plus one unassigned module (grey or 0):

| module | n_orthologs |
|--------|-------------|
| 1      | 2646        |
| 2      | 1198        |
| 3      | 1144        |
| 4      | 967         |
| 5      | 884         |
| 6      | 876         |
| 0      | 757         |
| 7      | 706         |
| 8      | 250         |
| 9      | 84          |
| 10     | 63          |
| 11     | 63          |
| 12     | 62          |
| 13     | 50          |
| 14     | 46          |

**Total**: 9,796 genes assigned across 15 groups

### Module Size Distribution

The modules show a hierarchical distribution typical of biological networks:

- **Large modules** (>1,000 genes): representing core biological processes
- **Medium modules** (500-1,000 genes): specialized but common functions
- **Small modules** (30-250 genes): highly specialized or condition-specific responses
- **Grey module** (757 genes): Unassigned genes with low network connectivity

### Module-Trait Relationships

The analysis generated a comprehensive heatmap showing correlations between module eigengenes (ME) and experimental traits. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251016/heatmap.png?raw=true)

This heatmap shows each module on the y axis grouped in a dendrogram. The x axis shows the categorical variables. Each cell shows the correlation of ortholog expression in the module and the variable with non-significant associations shown without text. Red indicates positive correlation and blue indicates negative correlation.  

Key findings include:

**Temporal Patterns:**

- Several modules showed strong time-dependent expression patterns
- Identified warm season (TP1/TP2, e.g., ME8) vs cool season (TP3/TP4, e.g., ME11) modules

**Species-Specific Patterns:**

- Some modules exhibited species-specific expression
- Species-divergent modules suggest lineage-specific adaptations with some showing positive correlations with 1 species in particular 

**Site Effects:**

- Modules correlated with collection site (Manava vs Maharepa)
- Site-specific modules may reflect local environmental adaptation

**Statistical Significance:**

- Only correlations with p < 0.05 are displayed in the heatmap
- Benjamini-Hochberg (BH) multiple testing correction applied per trait
- Module eigengene clustering revealed related modules with similar trait associations

### Hub Genes and Module Membership

The analysis calculated module membership (kME) for each gene, representing its correlation with its module eigengene. Hub genes (high kME values) are:

- **Central to module function**: Highly connected within their module
- **Potential biomarkers**: Representative of module expression patterns
- **Regulatory candidates**: May coordinate expression of other module members

For each module, the top 20 hub genes were identified based on kME values. These hub orthologs represent conserved regulatory nodes across all three coral species.

We can examine this data in more detail as desired.  

## Technical Notes

### Computational Approach

- **R packages**: WGCNA, tidyverse, ComplexHeatmap
- **Multi-threading**: Enabled for improved performance
- **Memory management**: blockwiseModules used for efficient handling of large datasets

### Quality Control

1. **Sample QC**: goodSamplesGenes() function verified data quality (all samples passed)
2. **Gene filtering**: Removed zero-variance genes (minimal - only 4 genes)
3. **Correlation method**: Pearson correlation appropriate for VST-normalized data
4. **Missing data**: All samples had complete expression profiles

### Statistical Considerations

- **Multiple testing correction**: BH adjustment applied per trait to control false discovery rate
- **Sample size**: 117 samples provides adequate power for correlation analysis
- **Module stability**: Minimum module size of 30 ensures robust modules
- **Merge criterion**: 0.75 eigengene correlation threshold balances module granularity

## References

The WGCNA method is described in:

Langfelder P, Horvath S (2008) WGCNA: an R package for weighted correlation network analysis. BMC Bioinformatics 9:559.

Zhang B, Horvath S (2005) A general framework for weighted gene co-expression network analysis. Statistical Applications in Genetics and Molecular Biology 4:17.

## Source Code

The complete analysis is documented in the R Markdown notebook:
- **Script**: [`18-ortholog-wgcna.Rmd`](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/18-ortholog-wgcna.Rmd)
- **Output directory**: [`M-multi-species/output/18-ortholog-wgcna/`](https://github.com/urol-e5/timeseries_molecular/tree/main/M-multi-species/output/18-ortholog-wgcna)
