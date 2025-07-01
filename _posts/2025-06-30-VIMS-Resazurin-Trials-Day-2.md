---
layout: post
title: VIMS Resazurin Trials Day 2
date: '2025-07-01'
categories: VIMS_Oysters
tags: Resazurin Oyster 
---

This post details resazurin trials at VIMS from Day 2. Today we ran full family trials with temperature exposure. 

# Overview 

We are visiting VIMS this week to conduct resazurin trials of *C. virginica* oyster families that are going to be outplanted into the field. We are going to sample oysters from 50 families with half of them representing families selected for low salinity performance and the other half selected for high salinity performance.   

There are predicted performance values and traits we can use to correlate to the resazurin data and once they are deployed in the field, we can use the observed performance data as well.  

## Protocol 

See my [notebook post from yesterday](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/VIMS-Resazurin-Trials-Day-1/) for methods development and the reason behind our methods decisions for today.   

Today we following this procedure:  

- Allocate oysters to designated 24-well plates based on our [plate maps](https://github.com/RobertsLab/vims-resazurin/tree/main/output/plate-maps). We loaded 24 total plates with a total of 480 oysters! 
- Take an image for [size analysis](https://github.com/RobertsLab/vims-resazurin/tree/main/data/size). Analyze size using ImageJ. 
- Complete metadata [for today's trials](https://github.com/RobertsLab/vims-resazurin/blob/main/data/trial_metadata.xlsx).  
- Add chilled resazurin solution to each well at a volume of 2.5 mL. 
- Take initial T0 measurements and set incubation in plate reader at 40°C.
- Add plates to the incubator at 40°C. Start the timer for exposures. 
- Stagger plates to read n=4 plates every 10 minutes for continuous plate reading throughout the day. 
- Take hourly measurements at T1 and T2. 
- After T2, we noticed high metabolic rates (although rates were slower to increase than yesterday due to the chilled resazurin). We then took the plates out of high temperature and put into the fridge at 4°C to start chilling the plates. Plates were added to the fridge at 2.5 hours of exposure. 
- Take measurements at T3 and T4. 
- Following T4, remove oysters from the plates and clean. 
- After each timepoint, we uploaded the [plate files here](https://github.com/RobertsLab/vims-resazurin/tree/main/data/plate-files/20250701).
- Throughout, record temperature in the wells using infrared thermometer.  

The rates of change in resazurin color were slower than yesterday, although a few samples did exceed our thresholds for over saturation (fluorescence > 3000).  

## Results 

### Temperature 

We monitored the temperature in the wells throughout the trial. Here is what the temperature looked like:  

- Timepoint 0-2: 40°C
- Timepoint 3-4: 4°C 
 
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250701/temps.png?raw=true)

Average temperature was about 35°C during temperature exposure, chilling back down to the low-mid 20's by the end of the trial.  

### Resazurin

I used the same approach as my previous resazurin analyses to produce change in size-normalized fluorescence over time for each oyster. In addition, I filtered the data to remove any sample that exceeded 3,000 raw resazurin fluorescence values at any point during the trial, due to the problem of oversaturation we had yesterday. This excluded 150 out of 480 oysters. We can return to this filtering as we proceed with trials over the next few days.  

Here are our preliminary results from today! 

- There was a significant difference in metabolic rates between families. Some families had steeper slopes and some had more shallow slopes over the trial. Variability is generally quite low within family.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250701/family_trajectories.png?raw=true)

- When looking at salinity phenotype, there was significantly higher metabolic rates in the low salinity families.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250701/phenotype_trajectories.png?raw=true)

- Total fluorescence over the course of the trial also significantly varied by family and phenotype.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250701/end.png?raw=true)

- Finally, I calculated the change in fluorescence between each hour. For example, `delta_T0_T1` indicates change in fluorescence from the initial time point to the first hour time point. There was significant variation in hour-specific metabolic rates between families. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250701/deltas.png?raw=true)

We will add more data to these datasets tomorrow and Thursday! 

## Preparation for tomorrow 

- I prepared the resazurin solution for tomorrow using the same recipe as I did yesterday (prepared 1.5 L). 
- We washed and prepared all of the plates and prefilled the plates with resazurin. Plates are kept in the fridge over night. 
- We prepared all of the plate maps and data sheets for tomorrow. 

**Working resazurin solution: 1,500 mL**   

- 1,481 mL filtered seawater  
- 3,330 µL resazurin concentrated stock solution (made above) 
- 1,498 µL DMSO 
- 15 mL antibiotic solution 

## Decisions for tomorrow 

We will run trials in the same format tomorrow and Thursday!     