---
layout: post
title: ASCA Analysis of Temperature and Parent Effects on Gene Expression
date: '2025-11-18'
categories: Larval_Symbiont_TPC_2023
tags: Mcapitata GeneExpression Molecular ASCA GeneExpression R
---

This post details ANOVA Simultaneous Component Analysis (ASCA) of RNAseq data to examine how temperature and parental phenotype affect gene expression patterns in *Montipora capitata* larvae.

# Overview

In this analysis, I used ASCA (ANOVA Simultaneous Component Analysis) to examine coordinated gene expression patterns in coral larvae across temperatures (27°C, 30°C, and 33°C) and between parental phenotypes (Wildtype, Bleached, and Nonbleached). These phenotypes also vary in symbiont community composition with Wildtype and Nonbleached hosting a mixture of *Cladocopium* and *Durusdinium* with Bleached only hosting *Cladocopium*. ASCA (ALASCA package in R) is a multivariate method that combines ANOVA with PCA to identify the main patterns of variation in gene expression data and understand which genes drive those patterns.

Unlike differential expression analysis that examines genes one at a time, ASCA captures coordinated expression patterns (principle components) across the entire dataset, indicating how groups of genes respond together to experimental factors like temperature and phenotype effects in this study.

The data and analysis scripts for this project can be found in the [GitHub repository](https://github.com/AHuffmyer/larval_symbiont_TPC).

# Data and Methods

## Dataset

The analysis uses RNAseq data from *M. capitata* larvae that were:

- Exposed to three temperatures: 27°C (ambient), 30°C (moderate), and 33°C (high stress)
- Derived from three parental phenotypes: Wildtype (C and D symbionts), Bleached (parents with bleaching history; C symbints), and Nonbleached (parents without bleaching history; C and D symbints)  

The full dataset includes ~28,600 genes (after filtering) across 54 samples.  

## Analysis Approach

I performed ASCA using the ALASCA package in R, which uses a linear mixed model framework. Here is some more information about ASCA analyses:  

- ASCA (Analysis of Variance Simultaneous Component Analyses) combines ANOVA with PCA approaches and can be a powerful tool for longitudinal multivariate data (e.g., time series, temperature gradients, physical distance or location, or other categorical ordered assignments). ASCA is useful for the analysis of both fixed and random effects on high-dimensional multivariate data when other multivariate ANOVA analyses (i.e., MANOVA) or differential gene expression analyses are not able to include random effects or more variables than there are observations.

- In my case, I want to know how gene expression (>20,000 genes) changes across a temperature gradient in three parental histories in coral larvae. Therefore, I am testing for variation in our multivariate large transcriptomic data across categorical predictors of temperature and parent phenotype. We will also use sample as a random effect to account for repeated measures.

I have previously used [DESeq2](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/DESeq2-Analysis-of-Mcap-2023-RNAseq/) analysis which worked well, but was limited in the ability to uncover specific underlying patterns and their strength in the 3x3 factorial design. 

- ASCA analysis is commonly used for chemical data, especially in the medical field. We will apply this analysis for our gene expression data, because the core structure of the data (i.e., a count matrix) is the same. Studies often use this analysis to conduct time series analyses. Here, we are going to use temperature as our ordered catagorical variable.

- This analysis could also be very useful for time series analysis of -omic data or physiological matrices when the data is highly dimensional and you want to test multivariate responses across multiple categorical predictors. The ability to include random effects is also useful because this is limited in analyses such as DEG and WGCNA approaches.

The workflow includes:  

1. **Normalize the data**: Applied variance stabilizing transformation (VST) to RNAseq counts using DESeq2
2. **Model the effects**: Used the model `value ~ temperature * parent + (1|sample)` to examine:
   - Main effect of temperature
   - Main effect of parent
   - Temperature × parent interaction
   - Sample as a random effect
3. **Identify components**: Used PCA to identify principal components that capture the most variation in gene expression
4. **Validate components**: Used bootstrapping (50 validation runs for exploratory analysis) to assess component significance
5. **Extract key genes**: Identified genes that explained the first 50% of variance of each PC (strongest contributors)  

The complete analysis script can be found at [`scripts/rna-seq_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_ASCA.Rmd).  

# Model 

The full script is found at the link above. Here is an example of the ASCA model.  

```
res_all <- ALASCA(
  long_data,
  value ~ temperature * parent + (1|sample),
  n_validation_runs = 50,
  validate = TRUE,
  reduce_dimensions = TRUE
)
```

This model includes main effects of temperature and parent as well as their interaction with sample as a random effect. For preliminary testing I am using n=50 permutations for validation runs.  

View the user guide for this package in R [here](https://andjar.github.io/ALASCA/articles/ALASCA.html).  

# Results

## Scree Plot: Variance Explained

The scree plot shows how much variance is explained by each principal component across the first 10 components. We analyzed the first 10 components. 

![Scree Plot](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/scree_plot.png?raw=true)

The scree plot reveals that the first three principal components capture the majority of meaningful variation in the gene expression data:

- **PC1** explains approximately 45% of the variance, representing the dominant pattern of gene expression variation across temperature and parent treatments
- **PC2** explains approximately 20% of variance, capturing a secondary pattern
- **PC3** explains approximately 15% of variance, representing a third pattern of coordinated gene expression

Components beyond PC3 show diminishing returns with variance explained at <10% of variance. This suggests that the main biological signals related to temperature and parent effects are captured in the first three components, which we'll examine in detail below. 

## PC1: Primary Temperature Response Pattern

The first principal component captures the largest source of variation in gene expression and describes changes in gene expression linearly across temperature.

### PC1 Effect Plot

![PC1 Effect](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC1.png?raw=true)

This plot shows the PC loading score across temperature - in other words, how gene expression of genes on this PC respond to temperature. This PC shows linear response to temperature. The absolute direction in the effect plot includes two patterns - genes that increase in response to temperature (positive loadings) and genes that decrease in response to temperature (negative loadings). Because gene expression and regulation is complex, we are not distinguishing between these two but instead we will analyze all genes that respond linearly to temperature together for functional enrichment (below).  

- **Temperature gradient**: The component scores show a strong progression across temperatures (27°C → 30°C → 33°C), indicating that temperature is a major driver of PC1 variation

The error bars (bootstrap confidence intervals) show that these differences are robust and reproducible. The clear separation suggests that PC1 captures a coordinated transcriptional response to thermal stress.

### PC1 Top Contributing Genes

To illustrate gene expression in this PC, I generated a plot with standardized gene expression values for the top 20 genes (top 10 positive and top 10 negative loadings) for visualization purposes. 

![PC1 Genes](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC1_genes.png?raw=true) 

We can see that the genes follow clear linear patterns of increasing or decreasing by tempreature with minimal differences between parents. 

We will examine these genes in more detail below.  

## PC2: Symbiont-Mediated Response Pattern

The second principal component captures additional variation not explained by PC1. This PC captured variation that is due to symbiont community. This is because it shows clear separation between bleached parental history (*Cladocopium* symbionts only) and the wildtype and nonbleached parental histories (*Cladocopium* and *Durusdinium* symbiont mixture).  

### PC2 Effect Plot

![PC2 Effect](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC2.png?raw=true)   

PC2 reveals a more complex pattern than PC1 due primarily to symbiont community identity.  

- **Non-linear temperature response**: Unlike PC1's linear gradient, PC2 shows a more stable response to temperature but with deviation in expression at 33°C in bleached larvae compared to the other parental phenotyeps. This suggests constituative and baseline variation in expression between parental phenotypes driven by symbiont community identity. 
- **Enhanced parent differences**: The separation between parental phenotypes is more pronounced in PC2, particularly at 33°C.  

### PC2 Top Contributing Genes

![PC2 Genes](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC2_genes.png?raw=true)  

These example gene loadings of the top 10 positive and negative gene loadings shows clear differences in expression between larvae with *Cladocopium* symbionts and those with a mixture of *Cladocopium* and *Durusdinium* symbionts. In some cases, there are genes that show slightly greater differences between these groups at 33°C (examples include gene g6908 and gene g1339). 

We will look at these genes in more detail with functional enrichment.  


## PC3: Parental History Response Pattern

PC3 described variation in gene expression unique to each parental history. 

### PC3 Effect Plot

![PC3 Effect](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC3.png?raw=true)  

- **Parental phenotype divergence**: PC3 reveals separation between parental phenotypes. This PC seems to describe constituative expression differences dependent on parental history. 

The larger confidence intervals in PC3 indicate more variability, which is expected for a tertiary component capturing less variance overall compared to PC1 and PC2.   

### PC3 Top Contributing Genes

![PC3 Genes](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC3_genes.png?raw=true)

These genes represent more specialized responses that complement the major patterns captured in PC1 and PC2. In particular, these genes show strong differences between parental histories regardless of temperature. Some genes show high separation between wildtype and the other two groups while others show unique responses of each parental phenotype.  

We will explore these genes in more detail with functional enrichment.  

# Selecting "important" genes on each PC 

In the ASCA analysis, all genes have a loading score on all PCs, with a higher loading score indicating higher relative importance of that gene to that biological pattern. There are several ways to determine which genes are "important" to each PC, but no one hard rule that is followed. In this analysis, I used cumulative variance explained to pull out the genes that contribute the most variance explained to each PC as the strongest drivers of the pattern.  

Here, I tested 80%, 60%, and 50% thresholds. Due to a high number of genes on each PC, I chose the most stringent cut off (50%) to keep only the strongest gene drivers for functional enrichment. The number of DEGs identified in previous analyses due to temperature and parent were similar to the number of important genes I found in the 80% threshold cut off.  

Specifically, I extracted a list of genes that explained the first 50% of variance on each PC.  

Here are the plots that show how this threshold was identified and the number of genes identified as "strongly important" drivers of each PC.  

### PC1: Temperature effects 

4,053 genes were identified as "important" in this pattern.  

Ranking of PC loading scores with threshold at 50% explained variance.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC1_abs_loadings_rank.png?raw=true)   

Cumulative variance explained with threshold at 50%.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC1_cumulative_variance.png?raw=true)  

### PC2: Symbiont-mediated effects 

3,247 genes were identified as "important" in this pattern.  

Ranking of PC loading scores with threshold at 50% explained variance.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC2_abs_loadings_rank.png?raw=true)   

Cumulative variance explained with threshold at 50%.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC2_cumulative_variance.png?raw=true)  

### PC3: Parental phenotype effects  

3,225 genes were identified as "important" in this pattern.  

Ranking of PC loading scores with threshold at 50% explained variance.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC3_abs_loadings_rank.png?raw=true)   

Cumulative variance explained with threshold at 50%.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC3_cumulative_variance.png?raw=true)  

I then looked at the distribution of important genes across each group. If we are identifying the strongest drivers of each pattern, there shouldn't be a high degree of overlap in important genes between PC components.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/PC1_PC2_PC3_upset.png?raw=true)  

From this we see that the vast majority of genes are unique to each PC and biological pattern! There is a smaller degree of overlap between PC2 and PC3, which make sense because these PC's both describe variation by phenotype. 

# Functional enrichment of each biological pattern observed  

I then conducted TopGO functional enrichment [in this script](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_functional_enrichment_ASCA_topGO.Rmd). Output is included [in the figures folder here](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/figures/rna_seq/asca/functional_enrichment).  

In the plots below I show functional enrichment results of the GO parent terms. Note that these are not the individual Biological Process terms because there is a very high number of terms. Those can be explored separately using tables output by the script linked above. The parent GO terms are useful for viewing the larger biological processes.  

These plots show the P-value from weighted Fisher exact tests (-log10 transformed so darker colors indicate lower p-values) against the Mean Gene Ratio for each parent term. The Gene Ratio indicates the number of significant genes associated with a GO term divided by the number of significant genes in total. A higher gene ratio indicates a higher proportion of genes in our gene set that are attributed to a particular GO term. 

### Temperature responses in larvae 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/functional_enrichment/PC1_enrichment_parent_terms.png?raw=true)   

Some interesting terms include functions related to:  

- Protein folding and unfolding
- Oxidative stress and cellular protection to DNA damage and immune response
- Cell cycle regulation and proliferation 
- Membrane trafficking and transport
- Metabolism and energy shifts 
- Immune and symbiosis signaling 
- Structural and developmental processes 

All of these functions are indicative of core thermal stress responses in corals and shifts in metabolic pathways to survive stressful conditions. Because we have lipidomic data, it is also interesting to note that there is significant enrichment in fatty acid biosynthetic processes.  

### Symbiont-mediated responses in larvae 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/functional_enrichment/PC2_enrichment_parent_terms.png?raw=true)  

Some interesting terms include functions related to: 

- Oxidative stress regulation and redox homeostasis 
- Nitrogen transport and metabolism (i.e., ammonium)
- Metabolite transport and nutritional exchange 
- Cell signaling and communication
- Developmental and structural remodeling 
- Cell cycle regulation 

These functions suggest that coral larvae with *Cladocopium* have distinct gene expression profiles from corals with *Cladocopium* and *Durusdinium* symbiont mixtures. These functions seem to be related to constituative differences in capacity and response for oxidative stress, nitrogen management, and nutritional transport along with communication functions. These functions also relate to thermal tolerance, which makes sense because we have observed higher thermal tolerance in corals with mixtures of symbionts or those hosting *Durusdinium* exclusively.  

### Parental history specific responses in offspring 

![](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/figures/rna_seq/asca/functional_enrichment/PC3_enrichment_parent_terms.png?raw=true)  

Here are the interesting functions that parent GO terms are related to:  

- Metabolic reprogramming (i.e., carbohydrate, fatty acid, amino acid metabolism)
- Stress responses and apoptosis 
- Development and cell structure 
- Signaling and communication 

These results suggest different baseline expression activity depending on parental history. It is interesting to note that this includes stress responses and metabolic programming, which again are related to thermal tolerance and parental history varies with thermal tolerance in this study.  

# Take Homes

Here are my take homes from this ASCA analysis:  

1. **Strong temperature response**: PC1 shows a clear temperature gradient from 27°C to 33°C, indicating that many genes respond to thermal stress
2. **Symbiont-mediated responses**: PC2 shows separation due to symbiont community identity, emphasizing the importance of symbiont community in mediating parental effects through constituative differences in expression in offspring
3. **Parental history specific responses**: PC3 shows that each parental history has unique gene expression patterns that are indicative of constituative differences in gene expression in offspring 

Overall, the ASCA analysis provided a new way to view strong underlying patterns in gene expression that are informative for the biology. 

# Next Steps

Next, I plan to conduct additional targeted pairwise analyses using DESeq2 and running this same ASCA workflow on the lipidomic dataset to see if the same biological patterns are seen.  

# Data Availability

All data and analysis scripts are available on GitHub:

- **Analysis script**: [`scripts/rna-seq_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_ASCA.Rmd)
- **Figures**: [`figures/rna_seq/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/figures/rna_seq/asca)
- **Gene lists**: [`output/rna_seq/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/output/rna_seq/asca)
- **Full repository**: [github.com/AHuffmyer/larval_symbiont_TPC](https://github.com/AHuffmyer/larval_symbiont_TPC)

# References

- Smilde, A.K. et al. (2005) ANOVA-simultaneous component analysis (ASCA): a new tool for analyzing designed metabolomics data. *Bioinformatics* 21(13):3043-3048
- Jansen, J.J. et al. (2005) ASCA: analysis of multivariate data obtained from an experimental design. *Journal of Chemometrics* 19:469-481
- Jarmund, A.H. et al. (2020) ALASCA: An R package for longitudinal and cross-sectional analysis of multivariate data by ASCA-based methods. *Frontiers in Molecular Biosciences* 7:585662
