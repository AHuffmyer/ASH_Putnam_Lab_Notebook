---
layout: post
title: Resazurin testing of open and closed oysters 
date: '2025-06-18'
categories: RobertsLab_Oysters USDA_Families
tags: Resazurin Oyster Cgigas
---

This post details activities of a resazurin trial to measure fluorescence in oysters and monitor whether they were open or closed at the time of measurement. 

This work was conducted by Noah O. in the lab. View his lab [notebook entries here](https://genefish.wordpress.com/).  

# Overview 

In brief, the trials included the following:  

- Analyzed metabolism via resazurin in n=10 oysters at control (23°C) and elevated (35°C) temperatures.  
- Conducted measurements in both fluorescence mode on the new plate reader. 
- Measured resazurin hourly for 3-4 hours.    

In my preliminary data analysis, I will test the following:  

- Does metabolic rate change between oysters that were open vs those that were closed? 

Data were uploaded to [GitHub here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/testing).  

# Data analysis 

The script for this analysis [is on GitHub here](https://github.com/RobertsLab/resazurin-assay-development/blob/main/scripts/testing/open-closed-analysis.Rmd) and will be updated with each round of analysis and trials.  

For this preliminary testing, rates are not size normalized.  

### Data preparation and calculations 

I prepared the data using the following steps. 

- Read in data from excel format 
- Read in sample metadata 
- Normalize all signals to the initial time point signal

This generates a metric of change in resazurin signal over time for each oyster.      

A higher value for fluorescence indicates more metabolic activity, because the solution fluoresces more as the reaction progresses.    

I then plotted the data and examined preliminary simple linear models to compare rates between open and closed oysters at each temperature.   
 
### Does metabolic rate change between open and closed oysters? 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20250618/open-closed.png?raw=true)

- This figure shows resazurin values (change in fluorescence) over the entire trial for each temperature treatment in open and closed oysters. 
- It is clear that change in fluorescence (and therefore metabolic rate) are higher in *closed* oysters, compared to open oysters.  
- This is counter to my expectations! At high temperature, this may be a signature of metabolic stress and metabolic depression associated with gaping behavior.  
- There are only a few open oysters at the control temperature. For those that were open, rates were also lower than closed oysters.  
- There was no difference between temperatures.    

This test does continue to validate that resazurin measures metabolic rate even in closed oysters. Further, this suggests that higher metabolic rates observed at high temperature are not due to more oysters being open. Rather, it seems that the decrease in metabolic rate may be a strong stress response.   

# Next steps

- Potentially conduct additional trials to increase sample size 
- Proceed with additional testing 

