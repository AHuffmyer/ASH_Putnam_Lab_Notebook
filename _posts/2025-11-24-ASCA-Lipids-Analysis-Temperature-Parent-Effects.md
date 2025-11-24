---
layout: post
title: ASCA Analysis of Temperature and Parent Effects on Lipid Profiles
date: '2025-11-24'
categories: Larval_Symbiont_TPC_2023
tags: Mcapitata Lipidomics Lipids ASCA R
---

This post details ANOVA Simultaneous Component Analysis (ASCA) of lipidomics data to examine how temperature and parental phenotype affect lipids in *Montipora capitata* larvae.

This follows the same structure as my previous ASCA analysis of gene expression.  

# Overview

In this analysis, I used ASCA (ANOVA Simultaneous Component Analysis) to examine coordinated lipidomic patterns in coral larvae across temperatures (27°C, 30°C, and 33°C) and between parental phenotypes (Wildtype, Bleached, and Nonbleached). These phenotypes also vary in symbiont community composition with Wildtype and Nonbleached hosting a mixture of *Cladocopium* and *Durusdinium* with Bleached only hosting *Cladocopium*. ASCA (ALASCA package in R) is a multivariate method that combines ANOVA with PCA to identify the main patterns of variation in lipid concentration data and understand which lipids drive those patterns.

Unlike differential analysis and PLSDA/VIP approaches that examine features one at a time, ASCA captures coordinated expression patterns (principle components) across the entire dataset, indicating how groups of lipids respond together to experimental factors like temperature and phenotype effects in this study.

The data and analysis scripts for this project can be found in the [GitHub repository](https://github.com/AHuffmyer/larval_symbiont_TPC).

Note that in this analysis we see the same major patterns [as we did for gene expression](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/ASCA-RNAseq-Analysis-Temperature-Parent-Effects/)! 

# Data and Methods

## Dataset

The analysis uses lipidomics data from *M. capitata* larvae that were:

- Exposed to three temperatures: 27°C (ambient), 30°C (moderate), and 33°C (high stress)
- Derived from three parental phenotypes: Wildtype (C and D symbionts), Bleached (parents with bleaching history; C symbints), and Nonbleached (parents without bleaching history; C and D symbints)  

The full dataset includes 843 lipids (after filtering) across 54 samples.  Lipids were normalized to sample tissue protein.   

## Analysis Approach

I performed ASCA using the ALASCA package in R, which uses a linear mixed model framework. Here is some more information about ASCA analyses:  

- ASCA (Analysis of Variance Simultaneous Component Analyses) combines ANOVA with PCA approaches and can be a powerful tool for longitudinal multivariate data (e.g., time series, temperature gradients, physical distance or location, or other categorical ordered assignments). ASCA is useful for the analysis of both fixed and random effects on high-dimensional multivariate data when other multivariate ANOVA analyses (i.e., MANOVA) or differential feature analyses are not able to include random effects or more variables than there are observations.

- In my case, I want to know how lipid concentration (843 lipids) changes across a temperature gradient in three parental histories in coral larvae. Therefore, I am testing for variation in our multivariate large transcriptomic data across categorical predictors of temperature and parent phenotype. We will also use sample as a random effect to account for repeated measures.

I have previously used [PLSDA and VIP analyses](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Preliminary-lipidomic-and-metabolomics-analysis-of-Hawaii-2023-larval-data/) which worked well, but was limited in the ability to uncover specific underlying patterns and their strength in the 3x3 factorial design. 

- ASCA analysis is commonly used for chemical data, especially in the medical field. We will apply this analysis for our lipidomic data, because the core structure of the data (i.e., a count matrix) is the same. Studies often use this analysis to conduct time series analyses. Here, we are going to use temperature as our ordered catagorical variable.

- This analysis could also be very useful for time series analysis of -omic data or physiological matrices when the data is highly dimensional and you want to test multivariate responses across multiple categorical predictors. The ability to include random effects is also useful because this is limited in analyses such as DEG and WGCNA approaches.

The workflow includes:  

1. **Load the data**: Load in filtered and normalized data produced in [this script](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/lipidomics_area.Rmd).  
2. **Model the effects**: Used the model `value ~ temperature * parent + (1|sample)` to examine:
   - Main effect of temperature
   - Main effect of parent
   - Temperature × parent interaction
   - Sample as a random effect
3. **Identify components**: Used PCA to identify principal components that capture the most variation in lipidomic profiles
4. **Validate components**: Used bootstrapping (20 validation runs for exploratory analysis; will be increased to 100) to assess component significance
5. **Extract key genes**: Identified lipids that explained the first 50% of variance of each PC (strongest contributors)  

The complete analysis script can be found at [`scripts/rna-seq_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/lipids_ASCA.Rmd).  

# Model 

The full script is found at the link above. Here is an example of the ASCA model.  

```
res_all <- ALASCA(
  long_data,
  value ~ temperature * parent + (1|sample),
  n_validation_runs = 20,
  validate = TRUE,
  reduce_dimensions = TRUE
)
```

This model includes main effects of temperature and parent as well as their interaction with sample as a random effect. For preliminary testing I am using n=20 permutations for validation runs.  

View the user guide for this package in R [here](https://andjar.github.io/ALASCA/articles/ALASCA.html).  

# Results

## Scree Plot: Variance Explained

The scree plot shows how much variance is explained by each principal component across the first 10 components. We analyzed the first 10 components. 

![Scree Plot](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/scree_plot.png?raw=true)

The scree plot reveals that the first three principal components capture the majority of meaningful variation (>10% variance explained by each) in the lipid data:

- **PC1** explains approximately 45% of the variance, representing the dominant pattern of lipid variation across temperature 
- **PC2** explains approximately 30% of variance, capturing a secondary pattern
- **PC3** explains approximately 10% of variance, representing a third pattern of coordinated lipids

Components beyond PC3 show diminishing returns with variance explained at <10% of variance. This suggests that the main biological signals related to temperature and parent effects are captured in the first three components, which we'll examine in detail below. 

## PC1: Primary Temperature Response Pattern

The first principal component captures the largest source of variation in lipids and describes changes in lipids across temperature, with a trend for lipids at max/min at the moderate temperature. This matches what we have seen before in PLSDA analyses.  

### PC1 Effect Plot

![PC1 Effect](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC1.png?raw=true)

This plot shows the PC loading score across temperature - in other words, how lipids on this PC respond to temperature. This PC shows a response to temperature with peaking concentrations at moderate temperature. The absolute direction in the effect plot includes two patterns - lipids that increase at moderate temperatures (positive loadings) and lipids that decrease at moderate temperature (negative loadings). The positive loadings are much stronger, indicating lipids that drive this PC are those that peak at moderate temperature.  

- **Temperature effects**: The component scores show an effect of  temperature and indicating that temperature is a major driver of PC1 variation

### PC1 Top Contributing Lipids

To illustrate lipids in this PC, I generated a plot with standardized concentration values for the top 20 lipids (top 10 positive and top 10 negative loadings) for visualization purposes. Note that the positive loading lipids are the ones that contribute more meaningfully to this PC.  

![PC1 Genes](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC1_genes.png?raw=true) 

These top lipids are composed of TG, DG, and PC lipids.  

## PC2: Symbiont-Mediated Response Pattern

The second principal component captures additional variation not explained by PC1. This PC captured variation that is due to symbiont community. This is because it shows clear separation between bleached parental history (*Cladocopium* symbionts only) and the wildtype and nonbleached parental histories (*Cladocopium* and *Durusdinium* symbiont mixture).  

### PC2 Effect Plot

![PC2 Effect](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipids_asca/PC2.png?raw=true)   

PC2 reveals a more complex pattern than PC1 due primarily to symbiont community identity. There are lipids that have both strong positive and negative loadings.  

- **Non-linear temperature response**: Unlike PC1's linear gradient, PC2 shows a more stable response to temperature. This suggests constituative and baseline variation in expression between parental phenotypes driven by symbiont community identity. 
- **Enhanced parent differences**: The separation between parental phenotypes is more pronounced in PC2, particularly at 30°C moderate temperature.  

Note that there are strong positive and negative loadings, indicating there are lipids that are both higher and lower in Bleached larvae compared to the other groups.   

### PC2 Top Contributing Genes

![PC2 Genes](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC2_genes.png?raw=true)  

These example lipid loadings of the top 10 positive and negative gene loadings shows clear differences in expression between larvae with *Cladocopium* symbionts and those with a mixture of *Cladocopium* and *Durusdinium* symbionts.  

For example, FA 28:7, PC O-36:8, and ST27:2;O are strong drivers. These lipids were also identified in previous PLSDA/VIP analyses.   

## PC3: Parental History Response Pattern

PC3 described variation in lipids unique to each parental history, although differences are smaller than in the first 2 PCs. 

### PC3 Effect Plot

![PC3 Effect](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC3.png?raw=true)  

- **Parental phenotype divergence**: PC3 reveals small separation between parental phenotypes. This PC seems to describe constituative expression differences dependent on parental history. 

The larger confidence intervals in PC3 indicate more variability, which is expected for a tertiary component capturing less variance overall compared to PC1 and PC2.   

### PC3 Top Contributing Genes

![PC3 Genes](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC3_genes.png?raw=true)

These lipids represent more specialized responses that complement the major patterns captured in PC1 and PC2. In particular, these lipids show some differences between parental histories regardless of temperature. Some lipids show high separation between wildtype and the other two groups while others show unique responses of each parental phenotype.   

# Selecting "important" lipids on each PC 

In the ASCA analysis, all lipids have a loading score on all PCs, with a higher loading score indicating higher relative importance of that lipid to that biological pattern. There are several ways to determine which lipids are "important" to each PC, but no one hard rule that is followed. In this analysis, I used cumulative variance explained to pull out the genes that contribute the most variance explained to each PC as the strongest drivers of the pattern. This is the same approach I used for the gene expression dataset.

Here, I tested 80%, 60%, and 50% thresholds. Due to a high number of lipids on each PC, I chose the most stringent cut off (50%) to keep only the strongest drivers.  

Specifically, I extracted a list of lipids that explained the first 50% of variance on each PC. I'll use these for additional analyses later on.   

Here are the plots that show how this threshold was identified and the number of lipids identified as "strongly important" drivers of each PC.  

### PC1: Temperature effects 

198 lipids were identified as "important" in this pattern.  

Ranking of PC loading scores with threshold at 50% explained variance.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC1_abs_loadings_rank.png?raw=true)   

Cumulative variance explained with threshold at 50%.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC1_cumulative_variance.png?raw=true)  

### PC2: Symbiont-mediated effects 

99 lipids were identified as "important" in this pattern.  

Ranking of PC loading scores with threshold at 50% explained variance.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC2_abs_loadings_rank.png?raw=true)   

Cumulative variance explained with threshold at 50%.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC2_cumulative_variance.png?raw=true)  

### PC3: Parental phenotype effects  

89 lipids were identified as "important" in this pattern.  

Ranking of PC loading scores with threshold at 50% explained variance.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC3_abs_loadings_rank.png?raw=true)   

Cumulative variance explained with threshold at 50%.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC3_cumulative_variance.png?raw=true)  

I then looked at the distribution of important lipids across each group. If we are identifying the strongest drivers of each pattern, there shouldn't be a high degree of overlap in important lipids between PC components.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/lipid_asca/PC1_PC2_PC3_upset.png?raw=true)  

From this we see that the vast majority of lipids are unique to each PC and biological pattern! There is a smaller degree of overlap between PC1 and PC2, which may be due to differences in how each phenotype responds to temperature.   

# Take Homes

Here are my take homes from this ASCA analysis. These main take homes are very similar to what we see in gene expression.   

1. **Strong temperature response**: PC1 shows a temperature response, indicating that many lipids respond to thermal stress
2. **Symbiont-mediated responses**: PC2 shows separation due to symbiont community identity, emphasizing the importance of symbiont community in mediating parental effects through constituative differences in lipid profiles that may be due to symbiotic nutritional interactions
3. **Parental history specific responses**: PC3 shows that each parental history has distinct lipid patterns that are indicative of constituative differences in lipids in offspring potentially due to parental provisioning

Overall, the ASCA analysis provided a way to view strong underlying patterns in lipids that are informative for the biology. 

# Next Steps

Next, I plan to conduct additional targeted analyses to plot the concentrations of important lipids and conduct enrichment using LION.    

# Data Availability

All data and analysis scripts are available on GitHub:

- **Analysis script**: [`scripts/lipids_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/lipids_ASCA.Rmd)
- **Figures**: [`figures/lipids/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/figures/lipids/asca)
- **Gene lists**: [`output/lipids_metabolites/lipids/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/output/lipids_metabolites/lipids/asca)
- **Full repository**: [github.com/AHuffmyer/larval_symbiont_TPC](https://github.com/AHuffmyer/larval_symbiont_TPC) 

# References

- Smilde, A.K. et al. (2005) ANOVA-simultaneous component analysis (ASCA): a new tool for analyzing designed metabolomics data. *Bioinformatics* 21(13):3043-3048
- Jansen, J.J. et al. (2005) ASCA: analysis of multivariate data obtained from an experimental design. *Journal of Chemometrics* 19:469-481
- Jarmund, A.H. et al. (2020) ALASCA: An R package for longitudinal and cross-sectional analysis of multivariate data by ASCA-based methods. *Frontiers in Molecular Biosciences* 7:585662
