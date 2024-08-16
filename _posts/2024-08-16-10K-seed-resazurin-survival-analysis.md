---
layout: post
title: 10K seed resazurin survival analysis
date: '2024-08-16'
categories: 10K_Seed RobertsLab_Oysters
tags: Oyster R Resazurin Survival
---

This post details survival analysis from 10K Seed resazurin trial survivorship measurements.  

# Overview 

In this project, we exposed oyster seed (C. gigas) at the Point Whitney Shellfish Hatchery to 5 different stress hardening treatments:  

- Control (no treatment)
- Immune 
- Temperature (35°C)
- Fresh water
- Fresh water at high temperature (35°C)

These treatments were carried out 3x per week (1 h exposure) for 3 weeks in two replicate bags per treatment (n=10 total bags, each with ~1,000 seed).   

After treatment, I transported 150 seed from each bag (n=2 bags per treatment) to UW campus for testing and sampling.  

See my [previous notebook post here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Sampling-10K-seed-oyster-project-at-UW/) for more details on this project.  

This post shows the results of survival curve analysis from metabolic rate incubations using resazurin. For more information on these incubations, [visit Colby's notebook](https://genefish.wordpress.com/author/colbyelvrum/). I'll make a post with a full resazurin protocol soon.  

# Statistical Methods 

For this analysis, we used Kaplan-Meier Survivorship curves with Cox Proportional Hazards models to analyze differences in survival between hardening treatments at 18°C and 42°C. This represents a control and acute stress temperature. Oysters were incubated at these temperatures in resazurin solution (non toxic and non lethal) for metabolic rate measurements. During these measurements, survival was measured hourly during 4 h incubations and at 24 h after incubations. Survival was tracked for each individual over time. Oysters were considered dead when gaping was observed and the oyster did not react or close when probed.  

Data and code for this analysis are in the 10K Seed [GitHub repository here](https://github.com/RobertsLab/10K-seed-Cgigas).  

## Set up 

Set up workspace, set options, and load required packages.   
 
```
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE)
```

Load libraries. 

```
library(tidyverse)
library(ggplot2)
library(survival)
library(readxl)
library(ggsurvfit)
library(gtsummary)
library(cardx)
library(cowplot)
```

## Read data 

Read in data. 

```
data<-read_excel("data/survival/survival_resazurin.xlsx")
```

Turn into long format. 

```
data<-data%>%
  pivot_longer(names_to="time", values_to="status", cols=`0`:`24`)%>%
  mutate(time=as.numeric(time))
```

Create data frames with control and high temperature subsets. 

```
control<-data%>%filter(temperature=="18C")
stress<-data%>%filter(temperature=="42C")
```

## Generate Kaplan Meier survival curves 

### Control temperature 18C

```
s1 <- survfit(Surv(time, status) ~ hardening, data = control)
str(s1)
```

Plot the survival function

```
survfit2(Surv(time, status) ~ hardening+temperature, data = control) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability"
  )
```

Estimate survival at 24 hours. 

```
summary(survfit(Surv(time, status) ~ hardening, data = control), times = 24)
```

Estimated survival is ~87% in control temperature. 

Use a log rank model to determine statistical differences in curves.  

```
survdiff(Surv(time, status) ~ hardening, data = control)

# Call:
# survdiff(formula = Surv(time, status) ~ hardening, data = control)
# 
#                                     N Observed Expected (O-E)^2/E (O-E)^2/V
# hardening=control                 360       18     19.1   0.06234   0.11188
# hardening=fresh-water             240       12     12.7   0.04156   0.06630
# hardening=fresh-water-temperature 240       16     12.7   0.84156   1.34252
# hardening=immune                  240       13     12.7   0.00584   0.00932
# hardening=temperature             240       11     12.7   0.23442   0.37396
# 
#  Chisq= 1.5  on 4 degrees of freedom, p= 0.8 
```

There is no difference in survival at 18C at p=0.8. 

Analyze again with a Cox proportional hazards model.
 
```
coxph(Surv(time, status) ~ hardening, data = control)

# coxph(formula = Surv(time, status) ~ hardening, data = control)
# 
#                                      coef exp(coef) se(coef)      z     p
# hardeningfresh-water              0.02005   1.02025  0.37269  0.054 0.957
# hardeningfresh-water-temperature  0.36099   1.43475  0.34368  1.050 0.294
# hardeningimmune                   0.07687   1.07991  0.36398  0.211 0.833
# hardeningtemperature             -0.06777   0.93447  0.38271 -0.177 0.859
# 
# Likelihood ratio test=1.57  on 4 df, p=0.8147
# n= 1320, number of events= 70  

coxph(Surv(time, status) ~ hardening, data = control) %>% 
  tbl_regression(exp = TRUE) 
```

Control is the reference level. No differences in survival at 18°C. 

## Generate Kaplan Meier survival curves 

### Stress temperature 42C

```
s2 <- survfit(Surv(time, status) ~ hardening, data = stress)
str(s2)
```

Plot the survival function

```
survfit2(Surv(time, status) ~ hardening+temperature, data = stress) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability"
  )
```

Estimate survival at 24 hours. 

```
summary(survfit(Surv(time, status) ~ hardening, data = stress), times = 24)
```

Estimated stress survival is 12-26%. Temperature hardened is the lowest.    

Use a log rank model to determine statistical differences in curves. 

```
survdiff(Surv(time, status) ~ hardening, data = stress)

# Call:
# survdiff(formula = Surv(time, status) ~ hardening, data = stress)
# 
#                                     N Observed Expected (O-E)^2/E (O-E)^2/V
# hardening=control                 360      116    133.1     2.195     5.048
# hardening=fresh-water             240       83     88.7     0.370     0.756
# hardening=fresh-water-temperature 240       83     88.7     0.370     0.756
# hardening=immune                  240       97     88.7     0.771     1.577
# hardening=temperature             240      109     88.7     4.632     9.470
# 
#  Chisq= 13.9  on 4 degrees of freedom, p= 0.007 
```

There is a significant difference in survival at p=0.007. 

Analyze again with a Cox proportional hazards model. 

```
coxph(Surv(time, status) ~ hardening, data = stress)

# Call:
# coxph(formula = Surv(time, status) ~ hardening, data = stress)
# 
#                                     coef exp(coef) se(coef)     z       p
# hardeningfresh-water             0.08431   1.08797  0.14377 0.586 0.55759
# hardeningfresh-water-temperature 0.04540   1.04644  0.14380 0.316 0.75224
# hardeningimmune                  0.20959   1.23317  0.13763 1.523 0.12780
# hardeningtemperature             0.40947   1.50602  0.13343 3.069 0.00215
# 
# Likelihood ratio test=11.11  on 4 df, p=0.02538
# n= 1320, number of events= 488 

coxph(Surv(time, status) ~ hardening, data = stress) %>% 
  tbl_regression(exp = TRUE) 
```

Control is the reference level. There is a significant difference between control and temperature hardening survival. 

# Generate plots 
 
Set theme. 

```
my_theme<-theme_classic()
```

Control

```
plot1<-survfit2(Surv(time, status) ~ hardening, data = control) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability",
    title="18°C"
  )+
  ylim(0,1)+
  my_theme+
  geom_text(x=10, y=0.2, label="Cox PH p=0.8")+
  theme(legend.position="none")
```

Stress

```
plot2<-survfit2(Surv(time, status) ~ hardening, data = stress) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability",
    title = "42°C"
  )+
  ylim(0,1)+
  my_theme+
  geom_text(x=10, y=0.2, label="Cox PH p=0.025")+
  geom_text(x=10, y=0.15, label="Temperature vs control p=0.002")+
  geom_text(x=10, y=0.10, label="All others vs control p>0.05")+
  theme(legend.position="right")
```

Assemble plot

```
plots<-plot_grid(plot1, plot2, rel_widths=c(0.65,1), ncol=2)

ggsave(plots, filename="figures/survival/KMcurves.png", width=10, height=4)
```

![](https://github.com/RobertsLab/10K-seed-Cgigas/blob/main/figures/survival/KMcurves.png?raw=true)

# Conclusions and next steps 

- We found that "hardening" in elevated temperature sensitized oysters to stress and resulted in lower survival than those that were not treated or those in any other treatment.  
- Interestingly, oysters exposed to both fresh water and high temperature survived more than those only exposed to high temperature. 
- Overall, these treatments did not appear to improve survival and may actually decrease survival under acute stress.  

Next steps:  

- Next, I will analyze resazurin metabolic rate data and analyze the relationship between metabolic rate and survival of each individual.  


