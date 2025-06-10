---
layout: post
title: Sequim Bay Oyster Spring Assessment 
date: '2025-06-10'
categories: WSG_USDA RobertsLab_Oysters
tags: Oyster Cgigas Growth
---

This post summarizes size analysis of oyster outplants at the Sequim Bay site.  

# Overview 

In this project, we exposed 2023 POGS oysters to a two week daily temperature stress (treated) or control conditions (control) and then outplanted them in May 2024 at Sequim Bay. This project is also known as "Effort A" in our 2023-2024 hardening experiments.   

Last week, I took images of the oysters and undergraduates in the lab measured length and width.  

In this post, I'll show the results of size analysis.  

We will return to this site to track growth later in the summer.  

Data and code for this analysis ("Sequim Bay" site) can be [found on GitHub here](https://github.com/RobertsLab/project-gigas-conditioning/tree/main).   

## Survival 

There was minimal mortality in all bags and no differences in survival. Here, I am measuring size. These oysters are from the same source population and same starting size.  

## Growth 

From field images, we obtained length and width measurements for each oyster. I used the polynomial growth equation generated for 2023 POGS seed at the Goose Point site to estimate oyster volume.  

I then plotted volume for each replicate bag.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250610/bag_sizes.png?raw=true)

Size is pretty consistent between replicate bags. There is one bag from the control group that I removed as an outlier (bag G096). In the field I noticed the oysters were very small in this one bag. It has been removed from analysis. 

When summarized by treatment, here are the size results.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250610/treatment_sizes.png?raw=true)

Models show that there is a significant difference (P=0.029) in size between treatments. 

```
model<-lmer(sqrt(Predicted_Volume_Poly) ~ treatment + (1|bag:treatment), data=data)

anova(model)

Type III Analysis of Variance Table with Satterthwaite's method
          Sum Sq Mean Sq NumDF  DenDF F value  Pr(>F)  
treatment 9350.4  9350.4     1 4.8613   10.76 0.02287 *
```

Interestingly, size is smaller in treated oysters. This is consistent with our findings of reduced size in treated oysters in 2023 POGS seed in Westcott.  