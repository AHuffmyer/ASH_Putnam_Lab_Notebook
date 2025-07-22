---
layout: post
title: Resazurin measurements for oyster oxgen correlations 
date: '2025-07-21'
categories: RobertsLab_Oysters 
tags: Resazurin Oyster Cgigas
---

This post details activities of resazurin measurements of 80 oysters to calculate resazurin response to correlate to oxygen measurements later this week. 

This work was conducted by Madeline and Aakriti in the lab. View their lab [notebook entries here](https://genefish.wordpress.com/). See Madeline's notebook [post from today here](https://genefish.wordpress.com/2025/07/21/resazurin-oxygen-correlation-experiment/). 

# Overview 

We followed our lab [resazurin protocol](https://robertslab.github.io/resources/protocols/resazurin-assay/) with the following modifications:   

- Analyzed metabolism via resazurin in n=80 oysters at 27°C 
- Measured resazurin hourly for 5 hours 
- Oysters were placed in 12 well plates (n=10 oysters per plate with n=2 blanks per plate)
- Made a batch of 500 mL resazurin working stock and added 4 mL to each well
- Read resazurin on the BioTek Agilent Synergy HTX plate reader using standard protocols.   

Data were uploaded to [GitHub here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/oxygen-correlation).  

# Experimental set up

Madeline allocated n=80 oysters (8-11 mm length) into plastic boxes to keep track of individual oysters. We measured resazurin response on these individuals today and we will measure oxygen evolution response on the same individuals later this week.  

This will allow us to correlate resazurin and oxygen measurements on the same individuals.  

Oysters were allocated to boxes on Friday last week. The boxes have slits on the bottom and an open top to allow for water exchange. We allocated oysters into two boxes (Box 1 and Box 2) with n=40 oysters per box identified by coordinates (e.g., Box 1 A1, Box 1 A2, etc).  

Madeline took images of the oysters to measure length of each oyster for size normalization. The images are stored [on GitHub here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/oxygen-correlation/size-images). 

View metadata [here](https://github.com/RobertsLab/resazurin-assay-development/blob/main/data/oxygen-correlation/metadata.xlsx).     

Plate files from today are stored [here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/oxygen-correlation/plate-files).  

Each hour, plates were checked for temperature and read on the plate reader.  

After the trial, oysters were rinsed in DI water to remove resazurin and placed back in their boxes in the tanks to hold for oxygen measurements later this week.  

See Madeline's notebook [post from today here](https://genefish.wordpress.com/2025/07/21/resazurin-oxygen-correlation-experiment/).  

# Results 

The script for this analysis [is on GitHub here](https://github.com/RobertsLab/resazurin-assay-development/blob/main/scripts/oxygen-correlation/resazurin-analysis-oxygen-correlation.Rmd) and will be updated with each round of analysis and trials.  

### Data preparation and calculations 

I prepared the data using the following steps. 

- Read in data from .txt format 
- Read in sample metadata 
- Normalize all signals to the initial time point signal and size 

This generates a size-normalized metric of change in resazurin signal over time for each oyster.      

A higher value for fluorescence indicates more metabolic activity, because the solution fluoresces more as the reaction progresses.    

I then plotted the data for each oyster. 

![](https://github.com/RobertsLab/resazurin-assay-development/blob/main/figures/oxygen-correlation/individual_trajectories.png?raw=true)

There is a range of rates in oysters (with one being much higher than the others).  

10 oysters were dead at the end of the trial and these will be removed from the correlation tests as they will not be run for oxygen.  
 
# Next steps 

- Conduct oxygen measurements Thursday-Friday 
- Conduct correlations 