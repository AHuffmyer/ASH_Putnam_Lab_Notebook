---
layout: post
title: VIMS Ploidy Resazurin Trials - Round 2 
date: '2025-08-26'
categories: VIMS_Oysters
tags: Resazurin Oyster Ploidy 
---

This post details resazurin trials at VIMS between diploids and triploids. Experiments were conducted at VIMS and I analyzed the data. This is the second round of trials that included blanks and will be used to track individual performance through time.   

**tl;dr: Ploidy shows a significant effect with higher metabolic rates in diploid oysters as compared to triploid!** 

# Overview and approach 

Protocols were followed as we previously described for trials at VIMS. See my [notebook post here for protocol information](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/VIMS-Resazurin-Trials-Day-2/).  

Data and metadata were uploaded to GitHub [in our respository here](https://github.com/RobertsLab/vims-resazurin/tree/main/data/ploidy). Scripts used to analyze the data are available [here in the GitHub repo](https://github.com/RobertsLab/vims-resazurin/blob/main/code/ploidy-trials.Rmd). 

In this set of trials, VIMS researchers placed diploid or triploid oysters of the "HNRY" family in cups (n=84 oysters per ploidy) with resazurin solution. Plates were then added to incubators set at 40°C for 2 hours, after which point they were place in the fridge for another 2 hours. Resazurin fluorescence was measured every hour.  

For this analysis, data are size normalized and blank corrected. These oysters were individually tagged and outplanted. Later, we will correlate metabolic rate with individual performance in the field. 

# Results 

All figures can be [viewed here](https://github.com/RobertsLab/vims-resazurin/tree/main/figures/ploidy). I have included the most important ones below.

#### Diploids show higher metabolic rates than triploids 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250826/ploidy_model_predictions.png?raw=true)  

```
model<-full_data%>%
  lmer(sqrt(value) ~ ploidy * timepoint + (1|well:plate) + (1|plate), data=.)
  
Type III Analysis of Variance Table with Satterthwaite's method
                  Sum Sq Mean Sq NumDF  DenDF   F value  Pr(>F)    
ploidy             0.019  0.0193     1 164.19    2.2199 0.13816    
timepoint        112.850 28.2125     4 664.00 3247.9168 < 2e-16 ***
ploidy:timepoint   0.101  0.0254     4 664.00    2.9193 0.02065 * 
```

Diploids show higher metabolic rates than triploids. Note that this difference is small, but significant.  

#### Diploids show a trend for higher total metabolic activity than triploids, but this was not significant 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250826/auc_ploidy.png?raw=true)  

```
model<-lmer(sqrt(AUC) ~ ploidy + (1|plate), data=aucs)

Type III Analysis of Variance Table with Satterthwaite's method
        Sum Sq Mean Sq NumDF  DenDF F value Pr(>F)
ploidy 0.18214 0.18214     1 164.18  2.2257 0.1376
```

There is a trend for higher AUC, but this was not significant. This is likely due to the small difference in metabolic rates seen above.  

# Next steps 

The VIMS team will be individually tagged, deployed in the field, and tracked for performance. We can then correlate resazurin response with field performance!    
