---
layout: post
title: WGCNA with physiology correlations of ortholog expression for the E5 timeseries molecular project
date: '2025-10-28'
categories: E5
tags: GeneExpression WGCNA R
---

Today I updated a previously generated WGCNA to include correlations with physiology data. 

# Overview 

Please see [my last post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/E5-ortholog-WGCNA/) for all the details on the WGCNA. This post only contains the updated figure with physiology correlations. The code can be [found here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/18-ortholog-wgcna.Rmd).  

# Updated analysis to include physiology correlations

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
- **Physiology data**: Data frame with physiology for 14 metrics. 

VST normalization was chosen because it stabilizes variance across the expression range, making the data more suitable for correlation-based analyses like WGCNA.

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

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251016/heatmap_phys.png?raw=true)

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

**Physiology Correlations:**

- There are lots of correlations with physiological metrics! 
- The correlation patterns show that modules correlated with *Porites evermanni* are also correlated with physiological metrics including biomass and photosynthetic parameters. We know these parameters are strongly different between species, so this is expected. 
- We can look at the specific genes and relationships and determine how these relate to species differences.  

**Statistical Significance:**

- Only correlations with p < 0.05 are displayed in the heatmap
- Benjamini-Hochberg (BH) multiple testing correction applied per trait
- Module eigengene clustering revealed related modules with similar trait associations

## Source Code

The complete analysis is documented in the R Markdown notebook:
- **Script**: [`18-ortholog-wgcna.Rmd`](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/18-ortholog-wgcna.Rmd)
- **Output directory**: [`M-multi-species/output/18-ortholog-wgcna/`](https://github.com/urol-e5/timeseries_molecular/tree/main/M-multi-species/output/18-ortholog-wgcna)
