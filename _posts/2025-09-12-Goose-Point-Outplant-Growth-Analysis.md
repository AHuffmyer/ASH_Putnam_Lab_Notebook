---
layout: post
title: Goose Point Oyster Summer Year 2 Assessment Analysis  
date: '2025-09-12'
categories: WSG_USDA RobertsLab_Oysters
tags: Oyster Cgigas Growth
---

This post summarizes size analysis of oyster outplants at the Goose Point site up to the 20250825 sampling time point.  

# Overview 

In this project, we exposed 2023 POGS cohort oysters to either weekly temperature stress or weekly fresh water stress and then outplanted them in June 2024 at Goose Point (Palix river site). This project is also known as "Effort E" in our 2023-2024 hardening experiments.   

I took images of the oysters during the assessment in August and Aakriti V., an undergraduate researcher in the lab, measured length and width.  

In this post, I'll show the results of size analysis.  

We will return to this site to track growth and survival later in the fall.  

Data and code for this analysis ("Goose Point" site) can be [found on GitHub here](https://github.com/RobertsLab/project-gigas-conditioning/tree/main).   

We conducted two experiments: Fresh Water hardening and Temperature hardening. Each of these received the respetive treatment exposure for 3 h weekly for about 6 weeks prior to outplanting back in early 2024. 

We are now able to look at year 2 summer mortality and growth data.  

## Survival 

We did finally see some significant mortality in our bags. There is a trend for higher mortality in treated oysters, especially those that were exposed to fresh waster hardening.    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250912/field_total_mortality_goose_point.png?raw=true)

This is showing the number of total dead oysters across the entire outplant period. We started with 100 oysters, so the number of dead oysters corresponds to percentage mortality.  

## Growth 

From field images, we obtained length and width measurements for each oyster. I used the polynomial growth equation generated for 2023 POGS seed at the Goose Point site from September 2024 to estimate oyster volume.  

Analysis are conducted for each experiment separately. Each experimental group is compared to it's respective control.  

### Growth over time  

Here is a view of growth over time for each treatment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250912/growth_boxplot.png?raw=true)  

Here are a few notes and observations:  

- Growth is continuing linearly across the time series
- Oysters treated with fresh water hardening are larger than their respective controls by 8/25 measurements. This was significant (see models below). 
- Oysters treated with temperature hardening had a trend for being a tad smaller than their respective controls, but this was not significant (see models below). 

We also noted that there was more mortality in fresh water treated oysters - perhaps this has to do with increased growth or size in the treated oysters. 

We will see if this difference remains at the next sampling in the fall.  

### Model analysis 

I then ran linear models to look at differences in growth rates between treated and control oysters for each experiment/hardening effort.  

The model results are shown below. 

**Temperature Experiment: No meaningful difference in growth**  

```
model<-data%>%
  filter(group %in% c("Weekly Temperature Control", "Weekly Temperature Treated"))%>%
  
  lmer(sqrt(Predicted_Volume_Poly) ~ treatment * date + (1|field_cattle_tag:treatment), data=.)

summary(model)
anova(model)

qqPlot(residuals(model))

emm<-emmeans(model, ~treatment|date)
pairs(emm)

emm<-emmeans(model, ~treatment)
pairs(emm)
```

```
Type III Analysis of Variance Table with Satterthwaite's method
                Sum Sq Mean Sq NumDF  DenDF   F value    Pr(>F)    
treatment          500     500     1    4.0    1.2304    0.3295    
date           8400596 2100149     4 2783.1 5163.3781 < 2.2e-16 ***
treatment:date   20176    5044     4 2783.1   12.4010 5.343e-10 ***

contrast          estimate   SE df t.ratio p.value
 control - treated     5.35 4.82  4   1.109  0.3295

```

There is no significant difference in size by the 8/25 outplant period. The only significant difference is that control oysters were slightly smaller at the start of the experiment. 

**Fresh Water Experiment: Higher growth in treated oysters**  

```
model<-data%>%
  filter(group %in% c("Weekly Fresh Water Control", "Weekly Fresh Water Treated"))%>%
  
  lmer(sqrt(Predicted_Volume_Poly) ~ treatment * date + (1|field_cattle_tag:treatment), data=.)

summary(model)
anova(model)

qqPlot(residuals(model))

emm<-emmeans(model, ~treatment|date)
pairs(emm)

emm<-emmeans(model, ~treatment)
pairs(emm)
```

```
Type III Analysis of Variance Table with Satterthwaite's method
                 Sum Sq Mean Sq NumDF  DenDF   F value Pr(>F)    
treatment          2912    2912     1    6.1    7.6528 0.0323 *  
date           12338226 3084557     4 3689.0 8105.9386 <2e-16 ***
treatment:date    54077   13519     4 3689.0   35.5277 <2e-16 ***

 contrast          estimate   SE  df z.ratio p.value
 control - treated    -8.11 2.93 Inf  -2.766  0.0057

```

Fresh water treated oysters were significantly larger than their respective controls with the greatest difference at the 8/25 time point.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250912/growth.png?raw=true)

# Next steps 

We will assess oysters for size and mortality later in the fall season.  
