---
layout: post
title: Growth analysis of outplants Manchester 
date: '2025-09-29'
categories: Manchester_Hardening RobertsLab_Oysters WSG_USDA
tags: Cgigas Growth Oyster
---

This post details initial growth analysis of our field deployments for our Manchester hardening experiment at the Manchester NOAA station.  

# GitHub repositories  

Data for these projects are stored on GitHub in the following location:  

- [Manchester Hardening](https://github.com/RobertsLab/manchester-hardening)

## Overview 

In this project, we conducted a 2-week thermal hardening of seed provided by the USDA ARS POGS program.     

Oysters were exposed to the following conditions:  

- Control: Ambient temperatures (approx. 14-16°C)
- Treated: Daily exposure to elevated 25°C temperatures for approx. 5 hours

After 2 weeks of these treatments, oysters were outplanted at Manchester on the docks in SEPA cages to assess survival and growth over the summer.  

Oysters were outplanted on June 16, 2025 and assessed for growth and survival on September 23, 2025. 

Outplants were redeployed and will be checked again in fall/winter.  

Today I did a prelminiary analysis of growth on these outplants. See my [previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Assessing-field-outplants-at-Manchester/) for survival updates.  

## Growth analysis  

I first visualized size of each family by treatment from calculated size (volume mm-3) at the September assessment.  

While there were differences between families, there was no difference in final size.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250929/family_size.png?raw=true) 

```
model<-data%>%
  filter(date=="20250923")%>%
  
  lmer(volume.mm3 ~ family * treatment + (1|tag) + (1|cage), data=.)

anova(model)

Type III Analysis of Variance Table with Satterthwaite's method
                     Sum Sq   Mean Sq NumDF   DenDF F value    Pr(>F)    
family           2948621547 327624616     9 18.0168  6.6416 0.0003406 ***
treatment         121129846 121129846     1  2.0335  2.4555 0.2556288    
family:treatment  733060642  81451182     9 18.0168  1.6512 0.1744170    
```

I then examined growth by including date in the analysis. However, I realized we are missing initial size images for family YC24-099, so it will not be included in this portion of the analysis.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250929/family_growth_rate.png?raw=true)  

```
model<-data%>%
  filter(!family=="YC24-099")%>%
  
  lmer(volume.mm3 ~ date * family * treatment + (1|tag) + (1|cage), data=.)

anova(model)

Type III Analysis of Variance Table with Satterthwaite's method
                          Sum Sq    Mean Sq NumDF   DenDF   F value    Pr(>F)    
date                  4.4681e+10 4.4681e+10     1 1576.13 1877.1077 < 2.2e-16 ***
family                1.3807e+09 1.7259e+08     8   15.65    7.2507 0.0004566 ***
treatment             3.5061e+07 3.5061e+07     1    2.02    1.4730 0.3478276    
date:family           5.0243e+09 6.2803e+08     8 1576.14   26.3846 < 2.2e-16 ***
date:treatment        1.8740e+08 1.8740e+08     1 1576.13    7.8728 0.0050801 ** 
family:treatment      3.5514e+08 4.4393e+07     8   15.65    1.8650 0.1389266    
date:family:treatment 1.7635e+09 2.2043e+08     8 1576.14    9.2607 1.535e-12 ***

```

There are significant interactive effects between date, family, and treatment. I examined these using posthoc tests.  

Here are the families with differences in size between treatments:  

- YC24-137 (September): More growth in the temperature hardened treatment
- YC24-105 (September): More growth in the temperature hardened treatment

There were no differences between treatments within families at the start of the experiment.  

This suggests there are family-specific effects to treatment that we will examine in further analyses.  


