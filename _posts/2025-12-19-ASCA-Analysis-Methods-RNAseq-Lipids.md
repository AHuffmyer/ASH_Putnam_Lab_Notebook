---
layout: post
title: ASCA Analysis for RNAseq and Lipidomics Data for the 2023 Hawaii project 
date: '2025-12-19'
categories: Larval_Symbiont_TPC_2023
tags: ASCA GeneExpression Lipidomics Lipids Mcapitata Molecular Multivariate R 
---

This post describes the analytical methods, approach, and main results from ASCA (ANOVA Simultaneous Component Analysis) analyses conducted on *Montipora capitata* larval multi-omic data: (1) RNAseq gene expression ASCA analysis, (2) functional enrichment of RNAseq results, and (3) lipidomics ASCA analysis. In this project and these analyses, I am examining the predominant multivariate gene and lipid patterns in larvae from three parental phenotypes across three temperatures using a factorial design. 

# Overview

ASCA (ANOVA Simultaneous Component Analysis) is a multivariate analysis approach that combines ANOVA with Principal Component Analysis (PCA) to identify coordinated patterns of variation in high-dimensional datasets. In this study, I applied ASCA to examine molecular responses of *M. capitata* larvae to:

- **Temperature treatments**: 27°C (ambient), 30°C (moderate stress), and 33°C (high stress)
- **Parental phenotypes**: Wildtype (mixed *Cladocopium* and *Durusdinium* symbionts), Bleached (parents with bleaching history; *Cladocopium* only), and Nonbleached (parents without bleaching history; mixed symbionts)

The goal of this study is to determine metabolic responses to temperature and if responses are mediated by parental history/symbiont community. 

The analyses are implemented in three related scripts:
1. [**rna-seq_ASCA.Rmd**](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_ASCA.Rmd): ASCA analysis of gene expression data
2. [**rna-seq_functional_enrichment_ASCA_topGO.Rmd**](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_functional_enrichment_ASCA_topGO.Rmd): Functional enrichment of genes driving each pattern from ASCA
3. [**lipids_ASCA.Rmd**](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/lipids_ASCA.Rmd): ASCA analysis of lipidomic data

I previously [conducted preliminary ASCA analysis in genes](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/ASCA-RNAseq-Analysis-Temperature-Parent-Effects/) [and lipids](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/ASCA-Lipids-Analysis-Temperature-Parent-Effects/) but I have since made improvements and updates in the model. This post represents the revised full analysis with model validation that will be described in our publication.  

# tl;dr

There are clear patterns in both gene expression and lipidomics due to temperature and larval phenotype/symbiont community. These patterns relate to stress response and metabolism.  

1. **Temperature responses**: Both lipid and gene datasets show strong temperature effects, with genes involved in fatty acid biosynthesis enriched in temperature PC1, consistent with lipid remodeling patterns enriched for fatty acids. 
2. **Symbiont effects**: Both datasets separate by symbiont community, supporting the importance of symbiont identity in mediating molecular phenotypes
3. **Parental effects**: Both datasets show constitutive differences by parental history, supporting transgenerational effects on molecular phenotypes

# Why is ASCA appropriate for this study? 

ASCA is particularly well-suited for analyzing -omics data from full factorial experimental designs for several key reasons:

## Advantages for High-Dimensional Data

1. **Handles more variables than observations**: Unlike traditional MANOVA, ASCA can analyze datasets where the number of variables (genes, lipids) far exceeds the number of samples. In this study, we analyzed ~28,600 genes and ~900 lipids across 54 samples.

2. **Accommodates random effects**: ASCA implemented through the ALASCA package uses a linear mixed model framework, allowing inclusion of random effects (e.g., sample as a random effect) which is limited in traditional differential expression analyses like DESeq2.

3. **Captures coordinated responses**: Rather than examining each gene or lipid individually, ASCA identifies principal components that represent coordinated molecular responses across the entire dataset. This allows for the dominant patterns to emerge in the analysis and overcomes the limitations of pairwise comparisons in multi-factor experimental designs. 

## Benefits for Factorial Designs

4. **Partitions variance by experimental factors**: ASCA decomposes the total variance into contributions from main effects (temperature, parent) and interactions (temperature × parent), revealing which factors drive different patterns of molecular variation. We can set the model to examine specific effects and extract the coordinated responses for each effect individually. 

5. **Handles ordered categorical variables**: Temperature was treated as an ordered categorical variable (27°C → 30°C → 33°C), allowing detection of linear and non-linear response patterns across the temperature gradient between parental phenotypes.

6. **Statistical validation**: The ALASCA package implements bootstrap validation (n=1,000) to assess the significance and reproducibility of identified components. 

## Applicability to Multi-Omic Data

7. **Consistent framework across data types**: The same ASCA approach can be applied to different molecular data types (transcriptomics, metabolomics, lipidomics, proteomics) as long as the data structure is similar (samples × features matrix). This will allow us to integratively compare the patterns from each dataset. 

8. **Appropriate for count-based and continuous data**: With proper normalization and transformation, ASCA handles both count-based data (RNAseq; no within-model normalization following VST transformation) and continuous measurements (lipid abundances; standard devation normalization of concentration data). 

## Limitations of ASCA 

1. **Memory and computing power**: ASCA can take a lot of computing power and memory, especially for highly dimensional data like gene expression. To overcome this limitation, I ran analyses on the Roberts Lab RStudio server, Raven. This also worked on the RStudio server on UW's HPC, Klone. To run full model validation (n=1,000 bootstrapping runs), this took ~5 min for lipid data and ~1.5 h for gene expression data. 

2. **Multivariate descriptions, not individual testing**: ASCA models are descriptive and require additional follow up to test whether individual features are significantly different between groups. We will be able to see which genes and lipids are most strongly associated with each pattern, but this is slightly different than a direct univariate test. If there are features of interest that you want to test, follow up ASCA with ANOVA's or linear models as appropriate. 

3. **Results depend on your choices**: Like all models, the results will vary depending on your processing choices including normalization and transformation, effects chosen, model structure, etc. 

4. **Interpretation can be subjective**: Assigning biological meaning to each pattern can be interpretive and subjective. Focus on dominant patterns and components and support conclusions with additional analyses. 

# Analysis 1: RNAseq ASCA Analysis

This post details the ASCA analyses for these datasets. We have also performed DESeq2, PCA and PERMANOVA analyses as well as PLSDA analyses that complement this approach.  

Note that unsupervised multivariate analyses identified significant effects of phenotype, temperature, and interactions between phenotype and temperature. These effects will all be evaluated here.  

- [Genes](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/DESeq2-Analysis-of-Mcap-2023-RNAseq/)
- [Lipids](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Preliminary-lipidomic-and-metabolomics-analysis-of-Hawaii-2023-larval-data/)

**Script for ASCA analysis of genes**: [`scripts/rna-seq_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_ASCA.Rmd)

**Script for functional enrichment of genes identified by ASCA**: [`scripts/rna-seq_functional_enrichment_ASCA_topGO.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_functional_enrichment_ASCA_topGO.Rmd)

## Dataset and Experimental Design

The RNAseq dataset includes:

- **Samples**: 54 larval samples (6 biological replicates per treatment combination)
- **Genes**: ~28,600 genes after filtering (genes present at >10 counts in at least 10% of samples)
- **Design**: 3 temperatures × 3 parental phenotypes (full factorial)

## Analytical Approach

### Step 1: Data Preparation and Normalization

RNAseq count data requires appropriate normalization before ASCA analysis:

```
# Filter genes using pOverA approach
filt <- filterfun(pOverA(0.1, 10))
gfilt <- genefilter(gcount, filt)
gkeep <- gcount[gfilt,]

# Apply variance stabilizing transformation (VST) using DESeq2
gdds <- DESeqDataSetFromMatrix(countData = gcount_filt,
                              colData = metadata_ordered,
                              design = ~temperature + parent + temperature:parent)

gvst <- vst(gdds, blind=TRUE)
normalized_counts <- assay(gvst)
```

**Why VST normalization?** Variance stabilizing transformation removes the mean-variance relationship in count data and stabilizes variance across the dynamic range, making the data suitable for PCA-based methods like ASCA. VST is preferred over log transformation for count data because it handles low counts better and provides more stable variance.

### Step 2: Format Data for ASCA

The normalized data is converted to long format required by the ALASCA package:

```
long_data <- normalized_df %>%
  pivot_longer(cols = -c(sample, temperature, parent), 
               names_to = "variable", 
               values_to = "value")
```

### Step 3: ASCA Model Specification

The ASCA model is specified to test main effects and interactions:  

```
res_all <- ALASCA(
  long_data,
  value ~ temperature * parent + (1|sample),
  effects = c("temperature", "parent", "temperature:parent"),
  n_validation_runs = 1000,
  scale_function = "none",  # No additional scaling since data is VST-normalized
  validate = TRUE,
  stratification_column = "code",  # Ensures balanced resampling
  p_adjust_method = "fdr",
  reduce_dimensions = TRUE
)
```

**Key model features**:
- **Main effects**: Temperature and parent are modeled as fixed effects
- **Interaction term**: temperature:parent captures non-additive effects
- **Random effect**: (1|sample) accounts for sample-specific variation
- **No scaling**: Data is already variance-stabilized, so no additional scaling is needed
- **Stratified validation**: Ensures each bootstrap includes representatives from all treatment groups (critical with small sample sizes)
- **1000 validation runs**: Provides robust statistical assessment of component significance

I ran this analysis on Raven to allow for intense memory requirements.  

### Step 4: Component Identification

ASCA performs PCA on the variance attributed to each effect separately. For each effect (temperature, parent, temperature:parent), the analysis identifies principal components and their variance explained. The first 10 components are examined for each effect. I selected PC's that explained >10% of total variance for further analysis and interpretation.  

### Step 5: Identifying Important Genes

For each principal component, genes are then ranked by their loading scores (contribution to the component). "Important" genes are identified using a cumulative variance approach. I used a cut off of 25%. This results in a list of genes that **account for the first 25% of variance, identifying the strongest components of each pattern**. Note that if there are fewer genes that add up to that 25%, then those genes are stronger drivers accounting for more explanatory variance than a component that has more genes responsible for the first 25% variance. 

For example, 

```
# Rank genes by absolute loading
temp_pc1_load <- pc1_loadings %>%
  arrange(desc(abs_loading)) %>%
  mutate(
    var_contrib = loading^2,
    cum_var = cumsum(var_contrib) / sum(var_contrib)
  )

# Select genes explaining first 25% of variance
important_genes <- temp_pc1_load %>%
  filter(cum_var <= 0.25)
```

**Rationale for 25% threshold**: This stringent cutoff (tested against 50%, 60% and 80% thresholds) retains only the strongest drivers of each pattern while maintaining a manageable gene list size for functional enrichment. The number of genes identified (~500-1,000 per PC) represents coordinated patterns rather than individual comparisons. This results in identification of the strongest genes and aids in data reduction for interpretation of the strongest results.  

## Main Results: Temperature

The ASCA analysis identified two major patterns of coordinated gene expression related to temperature:

### Pattern 1 (Temperature PC1): Linear Temperature Response

- **Variance explained**: ~88% (strong)
- **Pattern**: Linear gradient across temperatures (27°C → 30°C → 33°C) including both increasing and decreasing expression
- **Genes identified**: 808 genes
- **Biological interpretation**: Core thermal stress response affecting larvae similarly regardless of parental phenotype

There were two PC's explaining >10% of variation due to temperature.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/temp_scree_plot.png?raw=true)

The pattern includes genes that shift linearly with temperature.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/temp_PC1.png?raw=true)

808 genes were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/temp_PC1_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded genes and their expression across temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/Temp_PC1_top10_genes.png?raw=true)

All genes are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/temp_PC1_heatmap.png?raw=true)

These genes were then analyzed using TopGO enrichment as I have preformed previously.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/functional/temp_PC1_enrichment_parent_terms.png?raw=true)

Genes that change linearly with temperature include functions related to thermal stress including: 

- Microautophagy
- Response to protein and DNA damage
- Cellular oxidative response and detoxification
- Regulation of DNA, mRNA splicing, and miRNA metabolism
- Regulation of cell cycle, growth, and proliferation 
- Fatty acid an dlipid metabolism
- Stress, homeostasis, and apoptosis pathways 

### Pattern 2 (Temperature PC2): Moderate Temperature Specific Response

- **Variance explained**: ~11% (minor temperature component)
- **Pattern**: Differential expression at moderate (30°C) temperatures
- **Genes identified**: 742 genes
- **Biological interpretation**: Unique response to moderate (30°C) temperature regardless of parental phenotype 

The pattern includes genes that are either higher or lower in expression at 30°C compared to 27°C and 33°C. This pattern is a more minor component and weaker than the response we saw in PC1.    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/temp_PC2.png?raw=true)

742 genes were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/temp_PC2_abs_loadings_rank.png?raw=true)

All genes are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/temp_PC2_heatmap.png?raw=true)

These genes were then analyzed using TopGO enrichment as I have preformed previously.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/functional/temp_PC2_enrichment_parent_terms.png?raw=true)

Genes that change at moderate temperature include functions related to thermal responses including: 

- DNA replication
- Bicarbonate transport and calcium ion sequestration
- Cellular oxidative responses and detoxification 
- Glutathione and one-carbon metabolism 
- Innate immune responses
- Methylation 

## Main Results: Parental Phenotype/Symbiont Community 

The ASCA analysis identified two major patterns of coordinated gene expression by parental phenotype:

### Pattern 1 (Parent PC1): Differential expression in larvae from bleached parents with Cladocopium symbionts 

- **Variance explained**: ~55% (strong)
- **Pattern**: Differential expression (higher or lower) in larvae with Cladocopium only symbionts (from bleaching sensitive parents) compared to those with mixed symbionts  
- **Genes identified**: 326 genes
- **Biological interpretation**: Core molecular characteristics of larvae with either *Cladocopium* symbionts or larvae with a mixture of symbionts regardless of temperature 

There were two PC's explaining >10% of variation due to parental phenotype/symbiont.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/parent_scree_plot.png?raw=true)

The pattern includes genes that are differential in the bleached group.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/parent_PC1.png?raw=true)

326 genes were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/parent_PC1_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded genes and their expression across phenotypes.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/Parent_PC1_top10_genes.png?raw=true)

All genes are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/parent_PC1_heatmap.png?raw=true)

These genes were then analyzed using TopGO enrichment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/functional/parent_PC1_enrichment_parent_terms.png?raw=true)

Genes that are different in larvae with different symbiont communities include: 

- Ion transport
- Regulation of cell cycle and growth
- Protein localization
- Regulation of signaling 
- Core metabolic processes 
- Ammonium metabolism 
- Arachidonic acid metabolism (a trophic/nutritional biomarker) 

### Pattern 2 (Parent PC2): Differential expression in larvae from each parental phenotype 

- **Variance explained**: ~45% (moderate)
- **Pattern**: Differential expression between each parental phenotype wth particular deviation in the widltype group    
- **Genes identified**: 408 genes
- **Biological interpretation**: Core molecular characteristics of larvae unique to each parental phentype

The pattern includes genes that are particularly differential in the wildtype group but vary across phenotypes.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/parent_PC2.png?raw=true)

408 genes were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/parent_PC2_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded genes and their expression across phenotypes.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/Parent_PC2_top10_genes.png?raw=true)

All genes are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/parent_PC2_heatmap.png?raw=true)

These genes were then analyzed using TopGO enrichment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/functional/parent_PC2_enrichment_parent_terms.png?raw=true)

Genes that are different in larvae from different parental phenotypes (and particularly different in wildtype) include: 

- Microautophagy
- Signaling and regulation of signaling 
- Regulation of symbiotic interactions
- ROS processes
- RNA splicing 

## Main Results: Interactive effects between temperature and phenotype 

The ASCA analysis identified three major patterns of coordinated gene expression in response to temperature modulated by parental phenotype. Note that these interactive terms are more complex to interpret and we may chose to target specific components within these patterns in the future.  

### Pattern 1 (Interaction PC1): Differential expression between parental phenotypes at 33°C temperature 

- **Variance explained**: ~40% (moderate)
- **Pattern**: Differential expression between parental phenotypes (particularly wildtype) at 33°C
- **Genes identified**: 718 genes
- **Biological interpretation**: Differential response between parental phenotypes modulated by 33°C conditions

There were three PC's explaining >10% of variation due to interactive effects.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_scree_plot.png?raw=true)

The pattern includes genes that are either differential in the wildtype group at 33°C or differential at 27 and 30°C.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC1.png?raw=true)

718 genes were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC1_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded genes and their expression across phenotypes.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/Interaction_PC1_top10_genes.png?raw=true)

All genes are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC1_heatmap.png?raw=true)

These genes were then analyzed using TopGO enrichment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/functional/interaction_PC1_enrichment_parent_terms.png?raw=true)

Genes that are different between parental phenotype modulated by 33°C: 

- Cell wall structure 
-  mRNA splicing
-  Lipid storage
-  Fatty acid metabolism
-  Ammonium ion metabolism 
-  Methylation 

### Pattern 2 (Interaction PC2): Differential expression between parental phenotypes at 30-33°C temperature 

- **Variance explained**: ~31% (moderate)
- **Pattern**: Differential expression between parental phenotypes (particularly wildtype) at 30-33°C
- **Genes identified**: 899 genes
- **Biological interpretation**: Differential response between parental phenotypes modulated by 30-33°C conditions

The pattern includes genes that are either differential in the wildtype group at 30-33°C or differential at 27°C but not 30-33°C.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC2.png?raw=true)

899 genes were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC2_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded genes and their expression across phenotypes.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/Interaction_PC2_top10_genes.png?raw=true)

All genes are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC2_heatmap.png?raw=true)

These genes were then analyzed using TopGO enrichment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/functional/interaction_PC2_enrichment_parent_terms.png?raw=true)

Genes that are different between parental phenotype modulated by 30-33°C: 

- Anion and ion transport
- Cell proliferation, apoptosis, and cell fate
- Glutathione metabolic process
- Arachidonic acid metabolism 
- Cellular heat response and oxidant detoxification
- Ammonium ion transport

### Pattern 3 (Interaction PC3): Differential expression between parental phenotypes at 30-33°C temperature 

- **Variance explained**: ~20% (moderate)
- **Pattern**: Differential expression between all parental phenotypes at 30-33°C
- **Genes identified**: 808 genes
- **Biological interpretation**: Differential response between all parental phenotypes modulated by 30-33°C conditions

The pattern includes genes that are either differential at 30-33°C or differential at 27°C but not 30-33°C.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC3.png?raw=true)

808 genes were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC3_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded genes and their expression across phenotypes.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/Interaction_PC3_top10_genes.png?raw=true)

All genes are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/interaction_PC3_heatmap.png?raw=true)

These genes were then analyzed using TopGO enrichment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/functional/interaction_PC3_enrichment_parent_terms.png?raw=true)

Genes that are different between parental phenotypes modulated by 30-33°C: 

- Microautophagy
- Signal regulation
- Amino acid biosynthesis
- Cellular detoxification
- Lactate metabolism
- Immune activiation

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/genes/upset.png?raw=true)

**Minimal overlap between patterns**: An upset plot analysis showed that >90% of "important" genes were unique to each PC, confirming that these represent distinct biological patterns rather than redundant information. There is more overlap between interactive patterns and temperature patterns, which is not surprising.  

# Analysis 2: Lipidomics ASCA Analysis

**Script**: [`scripts/lipids_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/lipids_ASCA.Rmd)

## Why ASCA is Appropriate for Lipidomic Data

ASCA is equally suitable for lipidomic data as for transcriptomic data because:

1. **Similar data structure**: Both datasets consist of a samples × features matrix with continuous measurements
2. **High dimensionality**: Lipidomic datasets typically include hundreds of lipid species, creating the same challenge of more features than samples
3. **Coordinated regulation**: Lipids within the same class or pathway often change together in response to stress, making them ideal for multivariate pattern detection
4. **Experimental design**: The same full factorial design (temperature × parent) applies to both data types
5. **Biological integration**: Lipid and gene expression changes should be coordinated (e.g., fatty acid biosynthesis genes and fatty acid lipids)

## Dataset and Experimental Design

The lipidomic dataset includes:
- **Samples**: 54 larval samples (different replicates than RNA samples)
- **Lipid features**: ~900 lipid features
- **Design**: Same 3 temperatures × 3 parental phenotypes
- **Measurement**: Area-normalized peak intensities from MS-based lipidomics

## Analytical Approach

### Step 1: Data Preparation

Lipidomic data processing:

```
# Load processed lipidomic data
lipids <- read_csv("output/lipids_metabolites/lipids/processed_lipids.csv")

# Convert to long format
long_data <- lipids %>%
  select(sample, temperature, parent, lipid, area_normalized) %>%
  rename(variable = lipid, value = area_normalized)

# Factor ordering (temperature as ordered categorical)
long_data$temperature <- factor(long_data$temperature,
                               levels=c("Ambient", "Moderate (+3)", "High (+6)"))
long_data$parent <- factor(long_data$parent,
                          levels=c("Wildtype", "Nonbleached", "Bleached"))
```

### Step 2: Data Transformation

Unlike VST-normalized RNAseq data, raw lipidomic abundances benefit from log transformation:

```
# Check distribution
hist(long_data$value)  # Often right-skewed

# Apply log1p transformation (log(1 + x) to handle zeros)
long_data <- long_data %>%
  mutate(value = log1p(value))

hist(long_data$value)  # More normally distributed
```

**Why log1p transformation?** Lipidomic peak areas span several orders of magnitude with right-skewed distributions. Log transformation stabilizes variance and normalizes distributions, making the data more suitable for PCA. The log1p variant (log(1 + x)) handles zero values without requiring imputation.

### Step 3: ASCA Model Specification

The ASCA model for lipids is nearly identical to the RNAseq model but includes scaling:

```
set.seed(123)

res_all <- ALASCA(
  long_data,
  value ~ temperature * parent + (1|sample),
  effects = c("temperature", "parent", "temperature:parent"),
  n_validation_runs = 1000,
  scale_function = "sdall",  # Scale by overall SD
  validate = TRUE,
  stratification_column = "code",
  p_adjust_method = "fdr",
  reduce_dimensions = TRUE
)
```

**Differences from RNAseq model**:

- **scale_function = "sdall"**: Scales all variables by overall standard deviation (appropriate for log-transformed data; RNAseq used "none" because VST already stabilizes variance)
- **Faster runtime and less memory**: ~5 minutes for full validation (vs. 1.5 hours for RNAseq) due to fewer features

### Step 4: Identifying Important Lipids

The same cumulative variance approach is used with a 25% threshold:

```
# Rank lipids by loading contribution
temp_pc1_load <- pc1_loadings %>%
  arrange(desc(abs_loading)) %>%
  mutate(
    var_contrib = loading^2,
    cum_var = cumsum(var_contrib) / sum(var_contrib)
  )

# Find 25% cumulative variance threshold
idx_25_var <- which(temp_pc1_load$cum_var >= 0.25)[1]

# Select important lipids
important_lipids <- temp_pc1_load %>%
  filter(rank <= idx_25_var)
```

**Why 25% threshold for lipids?** Lipidomic datasets have fewer features than RNAseq (~hundreds vs. ~thousands), so a lower threshold still provides a reasonable number of important features while maintaining stringency. 

## Main Results: Temperature 

### Pattern 1 (Temperature PC1): Moderate temperature specific response

- **Variance explained**: ~88% (strong)
- **Pattern**: Differential lipids at 30°C moderate temperature compared to 27 and 33°C
- **Lipids identified**: 77 lipids
- **Biological interpretation**: Core thermal stress response affecting larvae similarly regardless of parental phenotype

There were two PC's explaining >10% of variation due to temperature.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/temp_scree_plot.png?raw=true)

The pattern includes lipids that shift more strongly at moderate temperature.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/temp_PC1.png?raw=true)

77 lipids were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/temp_PC1_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded lipids and their expression across temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/Temp_PC1_top10_lipids.png?raw=true)

All lipids are shown in this heatmap ordered by PC loading score. Most are elevated at 30°C.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/temp_PC1_heatmap.png?raw=true)

I then used [LION/web](http://www.lipidontology.com/) for functional enrichment analysis of important lipids as a preliminary approach to interpretation.  

The lipid classes enriched in lipids differential at 30°C include:  

- C16:0 
- Fatty acids with 16 carbons
- Sphingolipids
- Plasma membrane
- Fatty acid with 16-18 carbons
- Fatty acid with <18 carbons 

### Pattern 2 (Temperature PC2): Linear temperature response

- **Variance explained**: ~11% (minor)
- **Pattern**: Lipids that shift linearly with temperature
- **Lipids identified**: 33 lipids
- **Biological interpretation**: Core thermal stress response affecting larvae similarly regardless of parental phenotype

The pattern includes lipids that shift linearly with temperature.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/temp_PC2.png?raw=true)

33 lipids were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/temp_PC2_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded lipids and their expression across temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/Temp_PC2_top10_lipids.png?raw=true)

All lipids are shown in this heatmap ordered by PC loading score.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/temp_PC2_heatmap.png?raw=true)

The lipid classes enriched in lipids differential across temperature include:  

- Fatty acid with 3-5 double bonds

## Main Results: Phenotype 

### Pattern 1 (Phenotype PC1): Differential lipids in larvae with Cladocopium symbionts from bleaching sennsitive parents 

- **Variance explained**: ~76% (strong)
- **Pattern**: Lipids differential in larvae with Cladocopium symbionts from bleaching sensistive parents
- **Lipids identified**: 77 lipids
- **Biological interpretation**: Core lipidomic differences in larvae with Cladocopium symbionts from bleaching sensistive parents regardless of temperature

There were two PC's explaining >10% of variation due to phenotype.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/parent_scree_plot.png?raw=true)

The pattern includes lipids that are different in bleached larvae.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/parent_PC1.png?raw=true)

38 lipids were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/parent_PC1_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded lipids and their expression across temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/Parent_PC1_top10_lipids.png?raw=true)

All lipids are shown in this heatmap ordered by PC loading score. Most are elevated in bleached larvae with a couple depleted in bleached larvae. FA 28:7 is highly differential and may be a signature of Durusdinium symbionts.     

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/parent_PC1_heatmap.png?raw=true)

I then used [LION/web](http://www.lipidontology.com/) for functional enrichment analysis of important lipids as a preliminary approach to interpretation.  

The lipid classes enriched in lipids differential in bleached larvae include:  

- Fatty acid with <18 carbons
- Neutral intrinsic curvature
- 1-alkyl 2-acylglycerophosphocholines

### Pattern 2 (Phenotype PC2): Differential lipids in wildtype larvae

- **Variance explained**: ~24% (minor-moderate)
- **Pattern**: Lipids differential in wildtype larvae 
- **Lipids identified**: 23 lipids
- **Biological interpretation**: Core lipidomic differences in wildtype larvae 

The pattern includes lipids that are different in wildtype larvae.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/parent_PC2.png?raw=true)

23 lipids were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/parent_PC2_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded lipids and their expression across temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/Parent_PC2_top10_lipids.png?raw=true)

All lipids are shown in this heatmap ordered by PC loading score.     

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/parent_PC2_heatmap.png?raw=true)

I then used [LION/web](http://www.lipidontology.com/) for functional enrichment analysis of important lipids as a preliminary approach to interpretation.  

The lipid classes enriched in lipids differential in bleached larvae include:  

- Fatty acid with 26 carbons

## Main Results: Interaction 

Interactive effects were weak in unsupervised models and are weak in this analysis, so these may not be analyzed in detail in the paper.  

### Pattern 1 (Interaction PC1): Differential lipids in larvae with wildtype symbionts at 30°C

- **Variance explained**: ~67% (strong)
- **Pattern**: Lipids differential in wildtype larvae at moderate temperature
- **Lipids identified**: 55 lipids
- **Biological interpretation**: Lipidomic response to moderate temperature modulated by parental phentype

There were two PC's explaining >10% of variation due to interactive effects.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/interaction_scree_plot.png?raw=true)

The pattern includes lipids that are different between phentypes at moderate temperature.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/interaction_PC1.png?raw=true)

55 lipids were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/interaction_PC1_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded lipids and their expression across temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/Interaction_PC1_top10_lipids.png?raw=true)

All lipids are shown in this heatmap ordered by PC loading score. Most are higher in wildtype larvae at 30°C.      

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/interaction_PC1_heatmap.png?raw=true)

I then used [LION/web](http://www.lipidontology.com/) for functional enrichment analysis of important lipids as a preliminary approach to interpretation.  

The lipid classes enriched in lipids differential in bleached larvae include:  

- Membrane component
- Ceramide phosphocholines (sphingomeylins)
- Endosome/lysosome
- High transition temperature
- Headgroup with positive charge
- Diacylglycerophosphocholines
- Bilayer thickness 
- Lateral diffusion 

### Pattern 2 (Interaction PC2): Differential lipids between phenotypes at elevated temperature

- **Variance explained**: ~23% (minor-moderate)
- **Pattern**: Lipids differential between phenotypes at elevated temperature
- **Lipids identified**: 53 lipids
- **Biological interpretation**: Lipidomic response to temperature modulated by parental phentype

The pattern includes lipids that are different between phenotypes at 30-33°C temperature.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/interaction_PC2.png?raw=true)

53 lipids were determined as "important" in this PC at the 25% variance threshold.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/interaction_PC2_abs_loadings_rank.png?raw=true)

Here are examples of the top loaded lipids and their expression across temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/Interaction_PC2_top10_lipids.png?raw=true)

All lipids are shown in this heatmap ordered by PC loading score. Differences are minor.    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/interaction_PC2_heatmap.png?raw=true)

I then used [LION/web](http://www.lipidontology.com/) for functional enrichment analysis of important lipids as a preliminary approach to interpretation.  

The lipid classes enriched in lipids differential in bleached larvae include:  

- Alkyldiacylglycerols
- Headgroup with neural charge

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/asca/20251219/lipids/upset.png?raw=true)

**Minimal overlap between patterns**: An upset plot analysis showed that >90% of "important" lipids were unique to each PC, confirming that these represent distinct biological patterns rather than redundant information. There is more overlap between interactive patterns and temperature patterns, which is not surprising. The strongest PCs have the most unique lipids.   

## Integration with Gene Expression Results and overall findings 

The lipidomic ASCA results complement and validate the gene expression findings:

1. **Temperature responses**: Both datasets show strong temperature effects, with genes involved in fatty acid biosynthesis enriched in temperature PC1, consistent with lipid remodeling patterns enriched for fatty acids. 
2. **Symbiont effects**: Both datasets separate by symbiont community, supporting the importance of symbiont identity in mediating molecular phenotypes
3. **Parental effects**: Both datasets show constitutive differences by parental history, supporting transgenerational effects on molecular phenotypes

## Comparison to Alternative Approaches

**vs. DESeq2 differential expression**:
- ASCA captures coordinated patterns; DESeq2 identifies individual genes
- ASCA includes random effects; DESeq2 limited to fixed effects
- ASCA shows factor contributions; DESeq2 shows pairwise contrasts
- Both approaches are complementary rather than alternatives

**vs. WGCNA (Weighted Gene Co-expression Network Analysis)**:
- ASCA partitions by experimental factors; WGCNA clusters by correlation
- ASCA provides statistical validation; WGCNA identifies modules
- ASCA requires experimental design; WGCNA works with any dataset
- Both identify coordinated responses but from different perspectives

**vs. traditional MANOVA**:
- ASCA handles p >> n; MANOVA requires n >> p
- ASCA includes random effects; MANOVA limited to fixed effects
- ASCA provides PCA visualization; MANOVA provides test statistics
- ASCA is essentially a modern extension of MANOVA for high-dimensional data

# Key Takeaways

1. **ASCA is well-suited for full factorial designs in -omics studies**: The method explicitly models experimental factors and their interactions, providing clear interpretation of how temperature and parental effects structure molecular variation.

2. **Appropriate normalization is critical**: RNAseq requires variance-stabilizing transformation; lipidomics benefits from log transformation. The choice affects downstream scaling decisions.

3. **Coordinated patterns reveal biological mechanisms**: Rather than examining thousands of features individually, ASCA identifies 3-5 major patterns that represent distinct biological processes (thermal stress, symbiont effects, parental effects).

4. **Multi-omic integration is facilitated**: Using the same ASCA framework for different molecular data types enables direct comparison of patterns across data types (e.g., fatty acid biosynthesis genes and fatty acid lipids both respond linearly to temperature).

5. **Functional enrichment provides mechanistic insight**: Linking important genes to biological processes (TopGO) reveals what cellular mechanisms drive each pattern, moving from pattern description to biological interpretation.

6. **Validation ensures robustness**: Bootstrap validation with stratified resampling (1000 runs) provides confidence that patterns are biologically meaningful rather than statistical artifacts.

# Data and Code Availability

All analysis scripts and data are available in the project GitHub repository:

- **RNAseq ASCA script**: [`scripts/rna-seq_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_ASCA.Rmd)
- **Functional enrichment script**: [`scripts/rna-seq_functional_enrichment_ASCA_topGO.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/rna-seq_functional_enrichment_ASCA_topGO.Rmd)
- **Lipidomics ASCA script**: [`scripts/lipids_ASCA.Rmd`](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/lipids_ASCA.Rmd)
- **RNAseq results**: [`output/rna_seq/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/output/rna_seq/asca)
- **Lipidomics results**: [`output/lipids_metabolites/lipids/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/output/lipids_metabolites/lipids/asca)
- **Figures**: [`figures/rna_seq/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/figures/rna_seq/asca) and [`figures/lipids/asca/`](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/figures/lipids/asca)
- **Full repository**: [github.com/AHuffmyer/larval_symbiont_TPC](https://github.com/AHuffmyer/larval_symbiont_TPC)

# References

- Smilde, A.K. et al. (2005) ANOVA-simultaneous component analysis (ASCA): a new tool for analyzing designed metabolomics data. *Bioinformatics* 21(13):3043-3048
- Jansen, J.J. et al. (2005) ASCA: analysis of multivariate data obtained from an experimental design. *Journal of Chemometrics* 19:469-481
- Jarmund, A.H. et al. (2020) ALASCA: An R package for longitudinal and cross-sectional analysis of multivariate data by ASCA-based methods. *Frontiers in Molecular Biosciences* 7:585662
- Love, M.I. et al. (2014) Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology* 15:550
- Alexa, A. and Rahnenfuhrer, J. (2023) topGO: Enrichment Analysis for Gene Ontology. R package
- Trigg, S.A. et al. (2020) Transcriptomic responses to elevated pCO2 reveal threshold and cascade effects in an Antarctic pteropod. *BMC Genomics* 21:796
