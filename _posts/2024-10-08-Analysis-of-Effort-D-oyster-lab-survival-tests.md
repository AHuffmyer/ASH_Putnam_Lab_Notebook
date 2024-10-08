---
layout: post
title: Analysis of Effort D oyster lab survival tests
date: '2024-10-08'
categories: RobertsLab_Oysters WSG_USDA
tags: Cgigas Oyster R Survival
---

Today I ran scripts to analyze Effort D survival incubations for oysters in the lab. 

# Overview 

GitHub repositories for this project can be found at the following locations:  

- [Oyster stress hardening](https://github.com/RobertsLab/project-gigas-conditioning/tree/main) 

The script used here can be found on [GitHub at this link](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/code/survival_lab.Rmd).  

In this project, we exposed oyster seed to hardening stressors (sub lethal temperature exopsure) in the spring. We are now conducting lab thermal stress tests to test survivorship. Counterparts of these oysters are deployed at field sites.  

Noah's notebook posts for these incubations [can be found here](https://genefish.wordpress.com/).  

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
data<-read_excel("data/survival/lab_survival/lab_survival.xlsx")
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
stress<-data%>%filter(temperature=="46C")
```

## Generate Kaplan Meier survival curves 

### Control temperature 18C

```
s1 <- survfit(Surv(time, status) ~ treatment, data = control)
str(s1)
```

Plot the survival function  

```
survfit2(Surv(time, status) ~ treatment+temperature, data = control) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability"
  )
```

Estimate survival at 24 hours.   

```
summary(survfit(Surv(time, status) ~ treatment, data = control), times = 24)
```

Use a log rank model to determine statistical differences in curves.   

```
survdiff(Surv(time, status) ~ treatment, data = control)
```

No significant difference. P=1  

Analyze again with a Cox proportional hazards model.   

```
coxph(Surv(time, status) ~ treatment, data = control)

coxph(Surv(time, status) ~ treatment, data = control) %>% 
  tbl_regression(exp = TRUE) 
```

No significant difference. P=1  

There is no difference in curves with p=1. This makes sense because there was 0 mortality.  

# Generate Kaplan Meier survival curves 

## Stress temperature 46C

```
s2 <- survfit(Surv(time, status) ~ treatment, data = stress)
str(s2)
```

Plot the survival function   

```
survfit2(Surv(time, status) ~ treatment+temperature, data = stress) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability"
  )
```

Estimate survival at 24 hours.    
 
```
summary(survfit(Surv(time, status) ~ treatment, data = stress), times = 24)
```

Use a log rank model to determine statistical differences in curves.   

```
survdiff(Surv(time, status) ~ treatment, data = stress)
```

No significant difference. P=0.7    

Analyze again with a Cox proportional hazards model.   

```
coxph(Surv(time, status) ~ treatment, data = stress)

coxph(Surv(time, status) ~ treatment, data = stress) %>% 
  tbl_regression(exp = TRUE) 
```

No significant difference. P=0.7 

## Generate plots 
 
Set theme.   

```
my_theme<-theme_classic()
```

Control  

```
plot1<-survfit2(Surv(time, status) ~ treatment, data = control) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability",
    title="18°C"
  )+
  ylim(0,1)+
  my_theme+
  geom_text(x=10, y=0.2, label="Cox PH p>0.05")+
  #geom_text(x=10, y=0.2, label="Cox PH p=0.8")+
  theme(legend.position="none")
```

Stress  

```
plot2<-survfit2(Surv(time, status) ~ treatment, data = stress) %>% 
  ggsurvfit() +
  labs(
    x = "Hours",
    y = "Survival probability",
    title = "46°C"
  )+
  ylim(0,1)+
  my_theme+
  geom_text(x=10, y=0.2, label="Cox PH p>0.05")+
  #geom_text(x=10, y=0.15, label="Temperature vs control p=0.002")+
  #geom_text(x=10, y=0.10, label="All others vs control p>0.05")+
  theme(legend.position="right")
```

Assemble plot  

```
plots<-plot_grid(plot1, plot2, rel_widths=c(0.65,1), ncol=2)

ggsave(plots, filename="figures/survival/KMcurves.png", width=10, height=4)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/survival_lab/KMcurves.png?raw=true)

No difference between hardening treatments. There is a clear effect of the incubation stress temperature as desired!  

We will keep running oysters through this protocol from all efforts.    