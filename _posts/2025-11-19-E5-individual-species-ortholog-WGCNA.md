---
layout: post
title: Individual species WGCNA for E5 timeseries molecular project 
date: '2025-11-19'
categories: E5
tags: GeneExpression WGCNA R
---

Today I ran individual species WGCNA's as an extension of previous WGCNA analyses for the E5 timeseries molecular project.  

# Overview 

Please see [my last post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/E5-ortholog-WGCNA-module-plots/) for all the details on the WGCNA. 

The code can be [found here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/18-ortholog-wgcna.Rmd).  

The output files and figures of this script can be found [on GitHub here](https://github.com/urol-e5/timeseries_molecular/tree/main/M-multi-species/output/18-ortholog-wgcna).  

# Recap of WGCNA results of all species combined

As a reminder, here is the heatmap showing module correlations with factors of interest and physiological metrics from WGCNA of all species together.  

We are most interested in calcification related genes for our paper, so I have shown the key plots related to calcification. All plots showing relationships to other physiological metrics are in the output folder.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251016/heatmap_phys.png?raw=true)

I then generated plots that show module eigengene expression values for each module. 

The module eigengene is the first principal component of the expression values of all genes within a module. It represents the *dominant shared expression pattern* of that module across samples. In other words, the eigengene value for each sample summarizes how “high” or “low” the genes in that module are expressed collectively, allowing us to compare module-level expression across species, timepoints, sites, or physiological states rather than examining individual genes.

Here are some notes on these plots.  

- They are faceted by WGCNA module (15 total modules) 
- The metric of interest is on the x-axis 
- Each panel with a red star shows modules that are significantly correlated with that factor or metric 

#### Modules by time 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_by_time.png?raw=true)

#### Modules by species 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_by_species.png?raw=true)

#### Modules by calcification

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_calc.umol.cm2.hr.png?raw=true)

My concern with this combined analysis is that the differences in calcification between species dominate the correlations. Therefore, I wanted to try running WGCNA for each species separately to examine modules of genes correlated with calcification *within* each species and compare if the same orthologs are correlated with calcification across species.  

# Acropora WGCNA 

This analysis for each species followed the same methods as done for the combined WGCNA but only for a subsest of samples.  

#### Module-trait correlations 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/acr/acr_wgcna.png?raw=true)

There are several modules correlated with calcification, which I plot below. 

#### Modules by time 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/acr/MEs_by_time.png?raw=true)

I noticed more clear trends with time in this separate WGCNA. I think that removing the effect of species helps reveal more specific relationships with timepoint and physiological metrics. 

#### Modules by calcification

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/acr/MEs_vs_calc.umol.cm2.hr.png?raw=true)

There are several modules significantly correlated with calcification. These results seem more robust to me as they are not confounded by large species differences. 

# Pocillopora WGCNA 

This analysis for each species followed the same methods as done for the combined WGCNA but only for a subsest of samples.  

#### Module-trait correlations 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/poc/poc_wgcna.png?raw=true)

There is only one module correlated with calcification, which I plot below. 

#### Modules by time 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/poc/MEs_by_time.png?raw=true)

I noticed more clear trends with time in this separate WGCNA. I think that removing the effect of species helps reveal more specific relationships with timepoint and physiological metrics. 

#### Modules by calcification

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/poc/MEs_vs_calc.umol.cm2.hr.png?raw=true)

Only one module was related to calcification (24) in *Pocillopora*.  

# Porites WGCNA 

This analysis for each species followed the same methods as done for the combined WGCNA but only for a subsest of samples.  

#### Module-trait correlations 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/por/por_wgcna.png?raw=true)

There is only one module correlated with calcification, which I plot below. 

#### Modules by time 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/por/MEs_by_time.png?raw=true)

I noticed more clear trends with time in this separate WGCNA. I think that removing the effect of species helps reveal more specific relationships with timepoint and physiological metrics. 

#### Modules by calcification

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/por/MEs_vs_calc.umol.cm2.hr.png?raw=true)

There are several modules related to calcification. 

# Comparing orthologs related to calcification 

For each species, I then extracted a list of orthologs from all modules significantly correlated with calcification for comparison. 

Here is an upset plot showing the overlap.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251118/all/upset.png?raw=true)

There are a few important things to notice: 

- Most orthologs are unique to each analysis - meaning that each analysis is identifying different orthologs that correlate with calcification. 
- Calcification related orthologs appear to be highly species specific and dependent on whether analysis are conducted at individual species levels
- The most overlap in orthologs appears between Acropora calcification orthlogs and orthlogs identified in the combined species analysis ("all"). This indicates that Acropora was strongly driving modules that correlated with calcification in the combined analysis. 

# Take Homes 

The take homes from this analysis are: 

- Each species shows unique orthologs related to calcification
- Individual species WGCNA appear to be the most robust way to analyze the data 

Our next steps are to decide how we want to proceed with identifying modules or lists of orthologs to use for our timeseries molecular manuscript.  

## Source Code

The complete analysis is documented in the R Markdown notebook:
- **Script**: [`18-ortholog-wgcna.Rmd`](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/18-ortholog-wgcna.Rmd)
- **Output directory with figures generated**: [`M-multi-species/output/18-ortholog-wgcna/`](https://github.com/urol-e5/timeseries_molecular/tree/main/M-multi-species/output/18-ortholog-wgcna)
