---
layout: post
title: E5 metabolomics preliminary analysis
date: '2025-05-18'
categories: E5
tags: R Metabolomics Multivariate Pocillopora Porites Acropora  
---

This post details preliminary data analysis of metabolomic reponses of corals in the E5 timeseries project.  

# Overview 

In this analysis, I performed preliminary multivariate analyses of coral metabolomic data in the E5 project. 

The goal was to evaluate whether or not there are metabolomic shifts between species, time points, and sites and identify drivers of these differences. 

The script for this analysis [is on GitHub here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/07-metabolomics-analysis.Rmd).  

# tl;dr 

The main findings are as follows:  

- There are large differences in the metabolomes between species.  
- There are significant time effects on the metabolome within each species, but weak or no site effects. 
- There are key metabolites that drive differences between species and between time points.  

My next steps will be to investigate these differences and drivers in more details and examine metabolites of interest (e.g., central metabolism, nitrogen cycling). I will also re evaluate potential outliers.  

# Data analysis 

The script for this analysis is linked above, so I will not include the full code in this post. Instead, I will describe the process of data analysis and post the figures that were produced with my preliminary interpretations.  

Data processing was conducted using the following steps:  

1. Read in data and add relevant metadata 
2. Filter to remove internal standard compounds
3. Filter to remove compounds not measured in at least one pooled biological quality control 
4. Remove metabolites with NA from the facility in >10% of samples 
5. Normalize to tissue protein content 
6. Impute remaining missing values (<10%) with group median (grouped by compound, species, timepoint, and site) 

This processing led to a total of 141 metabolites that passed all filtering and quality control. Metabolite abundance is measured as protein normalized relative concentration.  

Data analysis was then conducted using the following steps:  

1. PCA and PERMANOVA of species effects
2. PCA and PERMANOVA of site and time effects within species (*site effects were not significant, so further analyses were conducted to examine species and time effects only*)  
3. Plot heatmap of metabolite concentration (z-scores) by species and by timepoint within species 
4. PLSDA and VIP identification of metabolites that discriminate between species 
5. PLSDA and VIP identification of metabolites that discriminate between timepoint within species 
6. Examine metabolite signatures of energetic state for the E5 modeler team - not described here, see [this notebook post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/E5-timeseries-energy-state-exploratory-analysis-Part-2/)  

# Results 

## 1. PCA and PERMANOVA of species effects

How does the metabolome differ between species? **Yes, strongly.**   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/species-pca.png?raw=true)

```
permanova<-adonis2(vegan ~ species * timepoint * site, data = data_wide, method='eu')

adonis2(formula = vegan ~ species * timepoint * site, data = data_wide, method = "eu")
                       Df SumOfSqs      R2       F Pr(>F)    
species                 2   3369.4 0.25154 16.9073  0.001 ***
timepoint               3    598.1 0.04465  2.0009  0.001 ***
site                    1     88.8 0.00663  0.8913  0.567    
species:timepoint       4    580.4 0.04333  1.4561  0.016 *  
species:site            2    280.9 0.02097  1.4097  0.066 .  
timepoint:site          3    327.4 0.02444  1.0953  0.301    
species:timepoint:site  4    577.1 0.04309  1.4480  0.021 *  
Residual               76   7572.8 0.56535                   
Total                  95  13395.0 1.00000  

```

There were significant and strong effects of species and timepoint on metabolomic responses. There also appear to be weak interactions with site.  

Due to the strong effect of species, I will analyze site and time effects within each species.  

## 2. PCA and PERMANOVA of site and time effects within species

#### Acropora  

How does the metabolome differ between site and time in Acropora? **Yes, time impacts the metabolome.**   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/acr-pca.png?raw=true)

```
permanova<-adonis2(vegan_acr ~ timepoint * site, data = acr_wide, method='eu')

adonis2(formula = vegan_acr ~ timepoint * site, data = acr_wide, method = "eu")
               Df SumOfSqs      R2      F Pr(>F)  
timepoint       2    522.7 0.13240 2.0744  0.011 *
site            1    146.9 0.03721 1.1659  0.258  
timepoint:site  2    380.5 0.09637 1.5099  0.080 .
Residual       23   2897.9 0.73401                
Total          28   3948.0 1.00000  
```

There were significant effects of time, but no effect of site in Acropora.  

#### Pocillopora  

How does the metabolome differ between site and time in Pocillopora? **Yes, time impacts the metabolome.**   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/poc-pca.png?raw=true)

```
permanova<-adonis2(vegan_poc ~ timepoint * site, data = poc_wide, method='eu')

adonis2(formula = vegan_poc ~ timepoint * site, data = poc_wide, method = "eu")
               Df SumOfSqs      R2      F Pr(>F)   
timepoint       2    531.1 0.13451 2.0928  0.003 **
site            1    203.0 0.05142 1.6000  0.065 . 
timepoint:site  2    295.7 0.07490 1.1653  0.222   
Residual       23   2918.2 0.73916                 
Total          28   3948.0 1.00000  
```

There were significant effects of time, but no effect of site in Pocillopora.  

#### Porites  

How does the metabolome differ between site and time in Porites? **No, there were no large scale shifts in metabolome in Porites.**   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/por-pca.png?raw=true)

```
permanova<-adonis2(vegan_por ~ timepoint * site, data = por_wide, method='eu')

adonis2(formula = vegan_por ~ timepoint * site, data = por_wide, method = "eu")
               Df SumOfSqs      R2      F Pr(>F)  
timepoint       3    517.4 0.09917 1.2789  0.074 .
site            1    175.2 0.03359 1.2995  0.172  
timepoint:site  3    478.9 0.09180 1.1839  0.152  
Residual       30   4045.5 0.77544                
Total          37   5217.0 1.00000 
```

There were no significant effects of time or site in Porites, indicating no large scale shifts in the metabolome in ths species.  

Note that there are a couple samples that may be outliers in Porites and I will further evaluate these samples.  

## 3. Plot heatmap of metabolite concentration (z-scores) by species and by timepoint within species 

Due to non-significant effects of site within each species in analyses above, I am proceeding with analyzing the effects of timepoint only in this round of analysis. I will return to site effects on specific metabolites in the future.   

I then generated Z-scores of metabolites to visualize differences in abundance between species and then between time points within species.   

#### Metabolite abundance between species 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/heatmap-species.png?raw=true)

There are clear sets of metabolites that are elevated and depleted in each species.  

#### Metabolite abundance between timepoints within species  

**Acropora:**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/heatmap-acr.png?raw=true)

There are clear sets of metabolites that are elevated and depleted at each time point in Acropora. 

**Pocillopora:**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/heatmap-poc.png?raw=true)

There are clear sets of metabolites that are elevated and depleted at each time point in Pocillopora.

**Porites:**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/heatmap-por.png?raw=true)

As we saw in the multivariate analysis above, the metabolome appears to be much more stable across time in Porites. There are some sets of metabolites that are elevated at each time point, particularly in TP4.  

## 4. PLSDA and VIP identification of metabolites that discriminate between species

I then used a partial least squares discriminant analysis and identification of variable importance in projection to identify metabolites driving the differences between species and time that we found in the above analyses.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/plsda.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/vip.png?raw=true)

There is a clear separation in metabolomes between species. The top metabolites driving this separation include metabolites involved in central carbon metabolism, lipid catabolism/metabolism, and amino acid metabolism. 

## 5. PLSDA and VIP identification of metabolites that discriminate between timepoint within species 

I then ran the same PLSDA and VIP analysis within each species to examine drivers of changes across time points.  

#### Pocillopora 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/poc-plsda.png?raw=true)

There seem to be differences between time points.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/poc-vip.png?raw=true)

Metabolites differentiating time points include amino acids, signaling metabolites, central metabolism, and lipid metabolism compounds.  

#### Acropora 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/acr-plsda.png?raw=true)

There seem to be differences between time points.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/acr-vip.png?raw=true)

Metabolites differentiating time points include amino acids, signaling metabolites, central metabolism, and lipid metabolism compounds.  

#### Porites 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/por-plsda.png?raw=true)

The two outliers may be masking changes by time points. It appears that ther emay be slight time point differences. I'll re evalutate these outliers.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/metabolomics/por-vip.png?raw=true)

Metabolites differentiating time points include amino acids, nitrogen metabolism and central carbon metabolism. 
