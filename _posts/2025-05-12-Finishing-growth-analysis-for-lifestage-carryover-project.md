---
layout: post
title: Finishing growth analysis for the lifestage carry over project 
date: '2025-05-12'
categories: WSG_USDA RobertsLab_Oysters
tags: R Growth Cgigas Oyster  
---

This post details growth analyses for the Lifestage Carry Over project in the Roberts Lab.  

# Overview 

In this analysis, we looked at growth of oysters in the lifestage carry over project. The original experiment is detailed in [Eric’s notebook here](https://eric-ess.github.io/notebook/).  

The GitHub repo for this project is [located here](https://github.com/RobertsLab/project-gigas-carryover/tree/main/lifestage_carryover).  

In a [previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Oyster-Lifestage-Carryover-Growth-Analysis/) I show some of the preliminary data. Today, I'll be analyzing the long term growth data.   

We conducted this project in October-December 2023 and this growth analysis tracks size from October 2023 to October 2024.  

The script used for this analysis is [on GitHub here](https://github.com/RobertsLab/project-gigas-carryover/blob/main/lifestage_carryover/scripts/02.01-growth.Rmd).  

## Tracking individual oyster growth over time 

After the initial experiment was completed (March 2024), I moved oysters into individual bags for individual growth tracking.  

This growth data included adults and seed. Spat were not measured individually due to the small size and will be analyzed in the section below.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/lco/20250512/individual_length_time.png?raw=true)

There was no difference in growth over time depending on the original treatment. There is a trend for higher growth in control, but this is not significant.  

## Tracking spat growth over time  

Spat were significantly larger in the treated group in August (the warmer time of the year). 

There is a significant month x treatment effect (P<0.001). 

```
Type III Analysis of Variance Table with Satterthwaite's method
                 Sum Sq Mean Sq NumDF   DenDF F value    Pr(>F)    
month           10374.6 1296.83     8 12.4698 55.5057 1.522e-08 ***
treatment           1.5    1.46     1  2.5899  0.0624 0.8212679    
month:treatment  1139.5  142.44     8 21.0479  6.0966 0.0004108 ***
```

Interestingly, treated spat are significantly larger in August 2024, but are significantly smaller in October 2024.    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/lco/20250512/spat_lengths_box.png?raw=true)





