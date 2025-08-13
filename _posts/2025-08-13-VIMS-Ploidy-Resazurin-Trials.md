---
layout: post
title: VIMS Ploidy Resazurin Trials
date: '2025-08-13'
categories: VIMS_Oysters
tags: Resazurin Oyster Ploidy 
---

This post details resazurin trials at VIMS between diploids and triploids. Experiments were conducted at VIMS and I analyzed the data.   

**tl;dr: Ploidy shows a significant effect with higher metabolic rates in diploid oysters as compared to triploid!** 

# Overview and approach 

Protocols were followed as we previously described for trials at VIMS. See my [notebook post here for protocol information](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/VIMS-Resazurin-Trials-Day-2/).  

Data and metadata were uploaded to GitHub [in our respository here](https://github.com/RobertsLab/vims-resazurin/tree/main/data/ploidy). Scripts used to analyze the data are available [here in the GitHub repo](https://github.com/RobertsLab/vims-resazurin/blob/main/code/ploidy-trials.Rmd). 

In this set of trials, VIMS researchers placed diploid or triploid oysters of the "HNRY" family in 12-well plates (n=24 plates with n=144 oysters per ploidy) with resazurin solution. Plates were then added to incubators set at 40°C for 2 hours, after which point they were place in the fridge for another 2 hours. Resazurin fluorescence was measured every hour.  

Blanks were not included in this trial and therefore the experiment will be repeated to include blanks. Blanks are critical to account for background variation in the resazurin solution due to batch composition, temperature changes, and light exposure.  

For this analysis, data are size normalized but are not blank corrected. Therefore, we can only make conclusions about relative differences between the groups as the absolute values are not comparable to previous measurements.  

# Results 

All figures can be [viewed here](https://github.com/RobertsLab/vims-resazurin/tree/main/figures/ploidy). I have included the most important ones below.

#### Diploids show higher metabolic rates than triploids 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250813/ploidy_model_predictions.png?raw=true)  

```
model<-full_data%>%
  lmer((value) ~ ploidy * timepoint + (1|well:plate) + (1|plate), data=.)
  
Type III Analysis of Variance Table with Satterthwaite's method
                  Sum Sq Mean Sq NumDF   DenDF  F value    Pr(>F)    
ploidy              5.18    5.18     1  246.29  11.4155 0.0008465 ***
timepoint        1405.30  351.32     4 1084.00 774.0123 < 2.2e-16 ***
ploidy:timepoint   16.58    4.14     4 1084.00   9.1302  2.96e-07 ***
```

#### Diploids show higher total metabolic activity than triploids 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250813/auc_ploidy.png?raw=true)  

```
model<-lmer(AUC ~ ploidy + (1|plate), data=aucs)

Type III Analysis of Variance Table with Satterthwaite's method
       Sum Sq Mean Sq NumDF  DenDF F value    Pr(>F)    
ploidy 183.05  183.05     1 246.35  11.092 0.0009998 ***
```

Super interesting! 

# Next steps 

Next, the VIMS folks will re run the trials to include blanks for proper blank correction. These oysters will then be individually tagged, deployed in the field, and tracked for performance. We can then correlate resazurin response with field performance!    
