---
layout: post
title: Goose Point Oyster Spring Assessment Analysis  
date: '2025-06-13'
categories: WSG_USDA RobertsLab_Oysters
tags: Oyster Cgigas Growth
---

This post summarizes size analysis of oyster outplants at the Goose Point site.  

# Overview 

In this project, we exposed 2023 POGS oysters to either weekly temperature stress or weekly fresh water stress and then outplanted them in June 2024 at Goose Point (Palix river site). This project is also known as "Effort E" in our 2023-2024 hardening experiments.   

I took images of the oysters during the assessment in May and Noah K., an undergraduate researcher in the lab, measured length and width.  

In this post, I'll show the results of size analysis.  

We will return to this site to track growth later in the summer.  

Data and code for this analysis ("Goose Point" site) can be [found on GitHub here](https://github.com/RobertsLab/project-gigas-conditioning/tree/main).   

## Survival 

There was minimal mortality in all bags and no differences in survival. Here, I'll focus on size effects. We may see mortality differences throughout the summer.    

## Growth 

From field images, we obtained length and width measurements for each oyster. I used the polynomial growth equation generated for 2023 POGS seed at the Goose Point site from September 2024 to estimate oyster volume.  

Analysis are conducted for each experiment separately. Each experimental group is compared to it's respective control.  

### Temperature experiment 

I plotted volume for each replicate bag.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250613/temp_plot1.png?raw=true)

Size is pretty consistent between replicate bags. There is one bag from the treated group that appears to be smaller than the others. I did not remove this bag from analysis and will instead account for the variation using random effects in models.  

When summarized by treatment, here are the size results.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250613/temp_plot2.png?raw=true)

There is no difference in size between treatments within any date time point or overall.  

### Fresh water experiment 

I plotted volume for each replicate bag.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250613/fresh_plot1.png?raw=true)

Size is pretty consistent between replicate bags. I accounted for variation between bags using random effects in models.  

When summarized by treatment, here are the size results.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250613/fresh_plot2.png?raw=true)

There is no difference in size between treatments within any date time point or overall. 

# Next steps 

We will assess oysters for size and mortality later in the summer.  
