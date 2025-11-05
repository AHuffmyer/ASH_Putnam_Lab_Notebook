---
layout: post
title: WGCNA with eigengene expression plots of ortholog expression for the E5 timeseries molecular project
date: '2025-11-05'
categories: E5
tags: GeneExpression WGCNA R
---

Today I updated a previously generated WGCNA to include plots that show module eigengene expression against factors of interest.  

# Overview 

Please see [my last post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/E5-ortholog-WGCNA/) for all the details on the WGCNA. Find [my previous heatmap with correlations for each module](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/E5-ortholog-WGCNA-physiology-correlations/) in my past post here. The code can be [found here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/18-ortholog-wgcna.Rmd).  

# Module eigengene expression

As a reminder, here is the heatmap showing module correlations with factors of interest and physiological metrics.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251016/heatmap_phys.png?raw=true)

I then generated plots that show module eigengene expression values for each module. 

The module eigengene is the first principal component of the expression values of all genes within a module. It represents the *dominant shared expression pattern* of that module across samples. In other words, the eigengene value for each sample summarizes how “high” or “low” the genes in that module are expressed collectively, allowing us to compare module-level expression across species, timepoints, sites, or physiological states rather than examining individual genes.

Here are some notes on these plots.  

- They are faceted by WGCNA module (15 total modules) 
- The metric of interest is on the x-axis 
- Each panel with a red star shows modules that are significantly correlated with that factor or metric 

#### Modules by site 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_by_site.png?raw=true)

#### Modules by time 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_by_time.png?raw=true)

#### Modules by species 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_by_species.png?raw=true)

#### Modules by maximal photosynthesis rate 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Am.png?raw=true)

#### Modules by photosynthetic efficiency

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_AQY.png?raw=true)

#### Modules by calcification

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_calc.umol.cm2.hr.png?raw=true)

#### Modules by symbiont cell density

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_cells.mgAFDW.png?raw=true)

#### Modules by antioxidant capacity 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_cre.umol.mgafdw.png?raw=true)

#### Modules by host biomass

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Host_AFDW.mg.cm2.png?raw=true)

#### Modules by compensation irradiance

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Ic.png?raw=true)

#### Modules by saturating irradiance

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Ik.png?raw=true)

#### Modules by host protein content

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_prot_mg.mgafdw.png?raw=true)

#### Modules by host:symbiont biomass ratio

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Ratio_AFDW.mg.cm2.png?raw=true)

#### Modules by respiration

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Rd.png?raw=true)

#### Modules by symbiont biomass

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Sym_AFDW.mg.cm2.png?raw=true)

#### Modules by total chlorophyll per symbiont cell

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Total_Chl_cell.png?raw=true)

#### Modules by total chlorophyll per unit biomass

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/20251105/MEs_vs_Total_Chl.png?raw=true)

## Source Code

The complete analysis is documented in the R Markdown notebook:
- **Script**: [`18-ortholog-wgcna.Rmd`](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/18-ortholog-wgcna.Rmd)
- **Output directory with figures generated**: [`M-multi-species/output/18-ortholog-wgcna/`](https://github.com/urol-e5/timeseries_molecular/tree/main/M-multi-species/output/18-ortholog-wgcna)
