---
layout: post
title: Initial analysis of resazurin metabolic rates in 10K Seed oysters
date: '2024-10-03'
categories: 10K_Seed RobertsLab_Oysters WSG_USDA
tags: Cgigas R Resazurin Survival Oyster
---

This post details initial analysis of resazurin metabolic rate measurements in 10K Seed oysters. 

# Overview 

In this project, we exposed oyster seed (C. gigas) at the Point Whitney Shellfish Hatchery to 5 different stress hardening treatments:  

- Control (no treatment)
- Immune 
- Temperature (35°C)
- Fresh water
- Fresh water at high temperature (35°C)

These treatments were carried out 3x per week (1 h exposure) for 3 weeks in two replicate bags per treatment (n=10 total bags, each with ~1,000 seed).   

After treatment, I transported 150 seed from each bag (n=2 bags per treatment) to UW campus for testing and sampling.  

See my [previous notebook post here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Sampling-10K-seed-oyster-project-at-UW/) for more details on this project. This post shows the results of metabolic rate incubations using resazurin. For more information on these incubations, [visit Colby's notebook](https://genefish.wordpress.com/author/colbyelvrum/). See [this notebook post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/10K-seed-resazurin-survival-analysis/) for survival analysis from the resazurin testing.

The full code for this post can be found [on GitHub here](https://github.com/RobertsLab/10K-seed-Cgigas/blob/main/scripts/resazurin.Rmd).  
 
In the survival analysis, we found that survival was lower in the temperature hardened group than other groups. No other treatment was different than the control group.  

![](https://github.com/RobertsLab/10K-seed-Cgigas/blob/main/figures/survival/KMcurves.png?raw=true)

Here, I will test a few questions:  

1) Did hardening treatment affect metabolic rates under elevated temperature? **tl;dr: not really** 

2) Did metabolic rate differ between oysters that lived or died under high temperature? **tl;dr: yes, very strongly - oysters that ultimately died had higher metabolic rates**  

3) Did hardening treatment modulate metabolic response to high temperature differently in those that survived vs those that died? **tl;dr: yes, metabolic rates showed a much stronger decline in those that lived, suggesting that metabolic depression may have been a successful strategy at 42°C.** 

4) Does metabolic rate explain the lower survival of the temperature hardened group? **tl;dr: nothing super obvious! Its possible that early peaks in metabolism in those that died at high temperature indicates they were more metabolically sensitive.** 

# Model results and plots 

Here are the model results. The full code is found on GitHub [here](https://github.com/RobertsLab/10K-seed-Cgigas/blob/main/scripts/resazurin.Rmd), I've just posted the highlight here first.  

Metabolic rates were calculated as the fluorescence value for each sample at each time point (hourly measurements during 4 hr incubatinos) normalized to the starting value. Normalized fluorescence values were then corrected by substracting fluorescence values of the corresponding blank samples at the respective temperature.  

## metabolic rate ~ temperature x hardening x mortality 

I first ran a model for the effect of temperature, hardening, and mortality on max fluorescence. Bag nested in hardening treatment is a random effect.  

```
model3<-lmer(log_value ~ temperature * hardening * final.mortality + (1|bag:hardening), data=final)

summary(model3)
anova(model3)
rand(model3)
```

```
Type III Analysis of Variance Table with Satterthwaite's method
                                      Sum Sq Mean Sq NumDF  DenDF F value    Pr(>F)    
temperature                           3.6378  3.6378     1 414.99 81.7245 < 2.2e-16 ***
hardening                             0.0658  0.0165     4   5.45  0.3696    0.8223    
final.mortality                       1.2131  1.2131     1 415.04 27.2519 2.829e-07 ***
temperature:hardening                 0.3119  0.0780     4 414.99  1.7519    0.1377    
temperature:final.mortality           0.0506  0.0506     1 416.28  1.1374    0.2868    
hardening:final.mortality             0.1500  0.0375     4 415.04  0.8424    0.4988    
temperature:hardening:final.mortality 0.1823  0.0456     4 416.31  1.0240    0.3945  

```

There is a significant effect of temperature and final mortality status on metabolic rates.  

Because mortality status affects rates, I then conducted models to look at the effect of temperature and hardening across metabolic rates (including time effects) within those that lived and those that died.  

I then generated a plot to examine these effects.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/resazurin/resazurin_plot3.png?raw=true)

This plot shows fluorescence values after 4 hours of incubation for those that remained alive (gray) and those that died (red) in 18°C and 42°C temperatures (x-axis).  

**Take homes**: As the model showed, there is a significant increase in metabolic rates in those that died (red), especially at 42°C. Regardless of mortality status, oysters at 42°C had lower metabolic rates, indicative of metabolic depression.  

Given these effects, we can now look at metabolic rates during the incubations for those that lived and those that died.  

## metabolic rate ~ time x hardening x temperature in oysters that lived 

I then ran a model to look at fluorescence over time during the incubation (metabolic rate) between temperatures and hardening treatments for those that lived with bag and sample included as a random effect for repeated measures.  

```
model2a<-lmer(log_value ~ time * temperature * hardening + (1|bag:hardening:sample) + (1|bag), data=alive)
```

```
Type III Analysis of Variance Table with Satterthwaite's method
                            Sum Sq Mean Sq NumDF  DenDF   F value  Pr(>F)    
time                       21.9388 21.9388     1 823.57 1518.9828 < 2e-16 ***
temperature                 0.0400  0.0400     1 290.51    2.7689 0.09719 .  
hardening                   0.0118  0.0030     4   6.50    0.2045 0.92744    
time:temperature            1.7012  1.7012     1 823.57  117.7889 < 2e-16 ***
time:hardening              0.1214  0.0303     4 823.57    2.1009 0.07889 .  
temperature:hardening       0.0180  0.0045     4 289.93    0.3115 0.87021    
time:temperature:hardening  0.2046  0.0512     4 823.57    3.5422 0.00709 ** 
```

In oysters that lived, there is a significant effect of time x temperature x hardening on metabolic rates, indicating that metabolic rate at each temperature is modulated by hardening treatment.  

I then plotted metabolic rate over time for each temperature and hardening treatment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/resazurin/alive_metabolism.png?raw=true)

In oysters that lived, metabolic rates were much lower at 42°C than 18°C, *except* for the control oysters, which showed no difference in metabolic rates between temperatures.  

## metabolic rate ~ time x hardening x temperature in oysters that died  

I then modeled the same effects in oysters that died. 

```
model2b<-lmer(log_value ~ time * temperature * hardening + (1|bag:hardening:sample) + (1|bag), data=dead)
```

```
Type III Analysis of Variance Table with Satterthwaite's method
                           Sum Sq Mean Sq NumDF  DenDF   F value  Pr(>F)    
time                       44.616  44.616     1 936.07 3420.6823 < 2e-16 ***
temperature                 0.050   0.050     1 345.83    3.8151 0.05160 .  
hardening                   0.023   0.006     4   6.94    0.4374 0.77853    
time:temperature            1.403   1.403     1 936.07  107.5350 < 2e-16 ***
time:hardening              0.132   0.033     4 936.07    2.5327 0.03898 *  
temperature:hardening       0.015   0.004     4 345.22    0.2791 0.89141    
time:temperature:hardening  0.033   0.008     4 936.07    0.6253 0.64452    
```

Metabolic rates differed across temperatures and across hardening treatments. There was no three way interaction in oysters that died.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/resazurin/dead_metabolism.png?raw=true)  

Metabolic rates showed less differention between temperatures in oysters that died. Interestingly, rates peaked earliest in the temperature hardened group than the other groups, which may be a reason why they died earlier than other hardening treatments.  

# Conclusions

Overall, we saw clear effects of temperature on metabolic rates and clear differences in metabolism between those that survived and those that died, regardless of hardening treatment.  

There were minimal differences between hardening treatment, but some signals that control groups had higher metabolic resilience in those that lived and metabolic rates may be particularly sensitive in the temperature hardened oysters.  




