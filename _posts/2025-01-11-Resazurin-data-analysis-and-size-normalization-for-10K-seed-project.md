---
layout: post
title: Resazurin data analysis and size normalization for 10K seed project
date: '2025-01-11'
categories: 10K_Seed Analysis RobertsLab_Oysters WSG_USDA
tags: Cgigas Oyster Resazurin R
---

This post details revised 10K seed resazurin metabolic rate analysis incorporating size data for metabolic rate normalization.  

# Overview 

This post includes the entire script and analysis (and is a bit long). The GitHub project is located [here](https://github.com/RobertsLab/10K-seed-Cgigas/) and [here is a direct link to the script](https://github.com/RobertsLab/10K-seed-Cgigas/blob/main/scripts/resazurin.Rmd).  

The associated survival analysis [is on GitHub here](https://github.com/RobertsLab/10K-seed-Cgigas/blob/main/scripts/survival.Rmd) and detailed [in my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/10K-seed-resazurin-survival-analysis/).  

In this project, we exposed *C. gigas* oyster seed to five environmental hardening treatments: immune (PolyIC exposure), temperature, fresh water, fresh water + temperature, and a control group.  

We then exposed these oysters to either control (18°C) or acute stress (42°C) conditions in the lab in an incubation for 4h and measured metabolic rates using a resazurin colorimetric assay developed by [Colby in the lab](https://genefish.wordpress.com/author/colbyelvrum/). After the 4h incubation, we returned oysters to ambient conditions and then assessed them 24h later for survival.  

In a [past analysis](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Initial-analysis-of-resazurin-metabolic-rates-in-10K-Seed-oysters/), I examined the effects of acute stress and hardening treatment on metabolic rates measured using the [resazurin assay and sampling](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Sampling-10K-seed-oyster-project-at-UW/). 

This post details the analysis following revisions with size normalizing metabolic rates with data collected and analyzed [by Noah from images](https://github.com/RobertsLab/resources/issues/2031). 

# Results  

The analysis script is long, so here are the take homes. See the link below for a published markdown with the full statistical output. Analyses were conducted using linear mixed effect models and generalized binomial mixed effect models in R.  

Throughout, elevated fluorescence of resazurin during the assyas indicates higher metabolic rate.   

The statements below represent statistically significant results.    

### 1. There was no difference in mortality between hardening treatments, but mortality was higher at 42°C  

Mortality was higher at 42°C over time.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/survival_temperature.png?raw=true)

Mortality did not vary significantly between hardening treatments.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/survival_hardening_temperature.png?raw=true)

### 2. There were minimal effects of hardening treatment on metabolic rates under stress 

Metabolic rates at high and ambient temperature showed little difference between hardening treatments. Hardening treatment metabolic rates did not differ in those that survived the entire trial, those that survived to 4h but died by 24h, or in those that died by 4h. 

Metabolic rates in oysters that survived the entire trial did not differ by hardening treatment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/hardening_temperature_alive24h-2.png?raw=true)

Rates also did not differ in those that survived the initial 4h stress, but died by 24h later.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/hardening_temperature_alive4h-dead24h-1.png?raw=true)

Finally, rates did not differ in those that died within the initial 4h incubation. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/hardening_42C_dead4h.png?raw=true)

### 3. Risk of mortality with increased metabolic rate (metabolic susceptibility to stress) was slightly higher in the fresh-water-temperature exposed group as compared to other hardening treatments 

Risk of mortality is significantly higher at lower metabolic rates in the fresh-water-temperature exposed group. This suggests that this group had a lower metabolic threshold to stress.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/hardening_mortality-risk_42C_dead4h.png?raw=true)

This can also be seen when comparing metabolic rates in oysters that lived vs died during the 4h incubation. Note that oysters that died in the fresh-water-temperature group had a lower metabolic rate than the other treatment groups.  Across all groups, oysters that died had higher metabolic rates during the initial 4h stress incubation (more on this below).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/hardening_42C_mortality_dead4h.png?raw=true)

Because hardening treatment effects were minimal to none, I then incorporated hardening treatment as a random effect and evaluated the effects of temperature on oyster response to the resazurin assay.    

### 4. Metabolic rates were substantially lower at 42°C (high) compared to 18°C (control) indicating metabolic depression under acute temperature stress  

Metabolic rates were lower at 42°C in oysters that survived through the entire 24 h trial.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/temperature_alive24h.png?raw=true)

Metabolic rates were lower at 42°C in oysters that survived through the initial 4 h incubation but died by 24 h.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/temperature_alive4h_dead24h.png?raw=true)

Metabolic rates were lower at 42°C in oysters that survived through the initial 4 h incubation regardless of mortality by 24 h.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/temperature_alive4h.png?raw=true)

### 5. At 42°C, oysters that survived had lower metabolic rates than those that died during the 4h incubation stress, indicating that capacity for metabolic depression may favor survival

Oysters that survived the 4 h incubation stress had lower metabolic rates, indicating that capacity for metabolic depression may favor survival. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/temperature_mortality_4h.png?raw=true)

### 6. Metabolic rates were higher in oysters that were most susceptible to stress and died within the first 4h stress incubation 

Metabolic rates were higher in oysters that died within 4h, but were not different between those that survived and died by 24h.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/mortality_metabolic-rate_comparison_42C.png?raw=true)

### 7. Under acute stress at 42°C, increased metabolic rate increases risk of mortality

Higher metabolic rates were related to significantly increased risk of mortality at the following time point.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/mortality_risk_metabolic_rate_42C_4h.png?raw=true)

### 8. Oysters under acute stress were far more sensitive to metabolic stress with metabolic thresholds lower than those at 18°C 

Oysters were far more sensitive to increases in metabolic stress under acute stress than under ambient temperatures. Interestingly, even under ambient conditions, oysters with the highest metabolic rates were more likely to experience mortality. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250116/temperature_mortality-risk_alive4h_dead24h.png?raw=true)

## Take home thoughts 

- These results demonstrate that oysters are metabolically susceptible to mortality and capacity for metabolic depression may favor survival.  
- Metabolic rates are significantly predictive of mortality and may be indicative of thermal tolerance in oysters. 
- We can clearly measure changes in metabolic rate in oysters over time and detect metabolic signals of stress. 

# Full code and statistical output  

The full output and code presented in a markdown format can be viewed on [GitHub here!](https://github.com/RobertsLab/10K-seed-Cgigas/blob/main/scripts/resazurin.md)
