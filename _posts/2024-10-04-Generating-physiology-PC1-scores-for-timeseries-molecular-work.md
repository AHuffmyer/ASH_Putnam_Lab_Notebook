---
layout: post
title: Generating physiology PC1 scores for timeseries molecular work
date: '2024-10-04'
categories: E5
tags: Multivariate Physiology R
---

Today I generated PC's for each species in the E5 physiology time series to use for correlations to gene expression.  

# Overview 

The repository for this project is [on GitHub here](https://github.com/urol-e5/timeseries_molecular/tree/main).    

The script used to make these plots and output data [is located here](https://github.com/urol-e5/timeseries_molecular/blob/main/scripts/02-phys-PC1.Rmd) with output saved [here](https://github.com/urol-e5/timeseries_molecular/tree/main/output/02-phys-PC1).    

**Goal:** Generate PC1 scores for each sample that was sequenced in a PCA of physiological metrics that related to energetic status for each species. 

## Set Up    

```
knitr::opts_chunk$set(echo = TRUE, warning=FALSE, message=FALSE)
```

```
## install packages if you dont already have them
if (!require("tidyverse")) install.packages("tidyverse")
if (!require("ggplot2")) install.packages("ggplot2")
if (!require("RColorBrewer")) install.packages("RColorBrewer")
if (!require("lme4")) install.packages("lme4")
if (!require("lmerTest")) install.packages("lmerTest")
if (!require("car")) install.packages("car")
if (!require("effects")) install.packages("effects")
if (!require("ggfortify")) install.packages("ggfortify")
if (!require("cowplot")) install.packages("cowplot")
if (!require("vegan")) install.packages("vegan")
if (!require("corrr")) install.packages("corrr")
if (!require("ggcorrplot")) install.packages("ggcorrplot")
if (!require("GGally")) install.packages("GGally")
if (!require("broom")) install.packages("broom")
if (!require("cowplot")) install.packages("cowplot")
if (!require("pairwiseAdonis")) BiocManager::install("pairwiseAdonis")
#devtools::install_github("pmartinezarbizu/pairwiseAdonis/pairwiseAdonis")

# load packages
library(tidyverse)
library(ggplot2)
library(RColorBrewer)
library(lme4)
library(lmerTest)
library(car)
library(effects)
library(ggfortify)
library(cowplot)
library(vegan)
library(corrr)
library(ggcorrplot)
library(GGally)
library(broom)
library(cowplot)
library(pairwiseAdonis)
```

## Read in data 

```
phys<-read_csv("data/phys_master_timeseries.csv")%>%
  select(colony_id_corr, species, timepoint, site, Host_AFDW.mg.cm2, Sym_AFDW.mg.cm2, Am, Rd, Ratio_AFDW.mg.cm2, cells.mgAFDW)%>%
  rename(colony_id=colony_id_corr)

length(unique(as.factor(phys$colony_id))) #140 colonies

metadata<-read_csv("data/rna_metadata.csv")%>%
  rename(colony_id=`Colony ID`, timepoint=Timepoint, haplotype=`Species/Strain`)%>%
  select(colony_id, timepoint, haplotype)%>%
  mutate(timepoint=gsub("TP", "timepoint", timepoint))
  
length(unique(as.factor(metadata$colony_id))) #30 colonies

data<-left_join(metadata, phys)

length(unique(as.factor(data$colony_id))) #30 colonies
```

Data now contains phys data for molecular samples.   

Selected metrics include:  

- Host biomass (AFDW normalized to surface area)
- Symbiont biomass (AFDW normalized to surface area)
- Symbiont cell density (cells normalized to AFDW)
- Max photosynthesis (oxygen production normalized to surface area)
- Respiration (oxygen consumption normalized to surface area)
- Ratio host:symbiont biomass 

## Acropora PC1 

Generate a PC plot for physiology selected metrics for Acropora.    

```
acr_data<-data%>%
  filter(species=="Acropora")

scaled_acr<-prcomp(acr_data%>%select(where(is.numeric)), scale=TRUE, center=TRUE) 

acr_PCA<-ggplot2::autoplot(scaled_acr, data=acr_data, frame.colour="timepoint", loadings=FALSE,  colour="timepoint", shape="site", loadings.label.colour="black", loadings.colour="black", loadings.label=TRUE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5, frame.type = 'norm') + 
  theme_classic()+
   theme(legend.text = element_text(size=12), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=12, face="bold"), 
        axis.text = element_text(size=12), 
        axis.title = element_text(size=12,  face="bold"));acr_PCA

ggsave(acr_PCA, filename="output/02-phys-PC1/acr_PCA.png", width=6, height=6)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/acr_PCA.png?raw=true)

Extract PC scores for each sample on PC1. 

```
acr_scores<-scaled_acr$x

acr_scores<-acr_scores%>%
  as.data.frame()%>%
  mutate(colony_id=acr_data$colony_id)%>%
  mutate(timepoint=acr_data$timepoint)%>%
  select(colony_id, timepoint, PC1)%>%
  write_csv(., file="output/02-phys-PC1/acr_PC1_scores.csv")
```

# Pocillopora PC1

Generate a PC plot for physiology selected metrics for Pocillopora  

```
poc_data<-data%>%
  filter(species=="Pocillopora")

poc_data<-poc_data[complete.cases(poc_data), ]

scaled_poc<-prcomp(poc_data%>%select(where(is.numeric)), scale=TRUE, center=TRUE) 

poc_PCA<-ggplot2::autoplot(scaled_poc, data=poc_data, frame.colour="timepoint", loadings=FALSE,  colour="timepoint", shape="site", loadings.label.colour="black", loadings.colour="black", loadings.label=TRUE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5, frame.type = 'norm') + 
  theme_classic()+
   theme(legend.text = element_text(size=12), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=12, face="bold"), 
        axis.text = element_text(size=12), 
        axis.title = element_text(size=12,  face="bold"));poc_PCA

ggsave(poc_PCA, filename="output/02-phys-PC1/poc_PCA.png", width=6, height=6)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/poc_PCA.png?raw=true)

Extract PC scores for each sample on PC1. 

```
poc_scores<-scaled_poc$x

poc_scores<-poc_scores%>%
  as.data.frame()%>%
  mutate(colony_id=poc_data$colony_id)%>%
  mutate(timepoint=poc_data$timepoint)%>%
  select(colony_id, timepoint, PC1)%>%
  write_csv(., file="output/02-phys-PC1/poc_PC1_scores.csv")
```

# Porites PC1

Generate a PC plot for physiology selected metrics for Porites  

```
por_data<-data%>%
  filter(species=="Porites")

por_data<-por_data[complete.cases(por_data), ]

scaled_por<-prcomp(por_data%>%select(where(is.numeric)), scale=TRUE, center=TRUE) 

por_PCA<-ggplot2::autoplot(scaled_por, data=por_data, frame.colour="timepoint", loadings=FALSE,  colour="timepoint", shape="site", loadings.label.colour="black", loadings.colour="black", loadings.label=TRUE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5, frame.type = 'norm') + 
  theme_classic()+
   theme(legend.text = element_text(size=12), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=12, face="bold"), 
        axis.text = element_text(size=12), 
        axis.title = element_text(size=12,  face="bold"));por_PCA

ggsave(por_PCA, filename="output/02-phys-PC1/por_PCA.png", width=6, height=6)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/por_PCA.png?raw=true)

Extract PC scores for each sample on PC1. 

```
por_scores<-scaled_por$x

por_scores<-por_scores%>%
  as.data.frame()%>%
  mutate(colony_id=por_data$colony_id)%>%
  mutate(timepoint=por_data$timepoint)%>%
  select(colony_id, timepoint, PC1)%>%
  write_csv(., file="output/02-phys-PC1/por_PC1_scores.csv")
```

