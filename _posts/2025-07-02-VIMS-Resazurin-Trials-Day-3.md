---
layout: post
title: VIMS Resazurin Trials Day 3
date: '2025-07-02'
categories: VIMS_Oysters
tags: Resazurin Oyster 
---

This post details resazurin trials at VIMS from Day 3. Today we ran full family trials with temperature exposure in the same way we ran trials yesterday. 

# Overview 

We are visiting VIMS this week to conduct resazurin trials of *C. virginica* oyster families that are going to be outplanted into the field. We are going to sample oysters from 50 families with half of them representing families selected for low salinity performance and the other half selected for high salinity performance.   

There are predicted performance values and traits we can use to correlate to the resazurin data and once they are deployed in the field, we can use the observed performance data as well.  

## Protocol 

See my [notebook post from yesterday](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/VIMS-Resazurin-Trials-Day-2/) for the procedure that we followed today and yesterday's results. This post provides an update on activities today and new results! 

Today we following this procedure:  

- Allocate oysters to designated 24-well plates based on our [plate maps](https://github.com/RobertsLab/vims-resazurin/tree/main/output/plate-maps). We loaded 24 total plates with a total of 480 oysters! 
- Take an image for [size analysis](https://github.com/RobertsLab/vims-resazurin/tree/main/data/size). Analyze size using ImageJ. 
- Complete metadata [for today's trials](https://github.com/RobertsLab/vims-resazurin/blob/main/data/trial_metadata.xlsx).  
- Add chilled resazurin solution to each well at a volume of 2.5 mL. 
- Take initial T0 measurements.
- Add plates to the incubator at 40°C. Start the timer for exposures. 
- Stagger plates to read n=4 plates every 10 minutes for continuous plate reading throughout the day. 
- Take hourly measurements at T1 and T2. 
- After T2, put plates into the fridge at 4°C to start chilling the plates. Plates were added to the fridge at 2.5 hours of exposure. 
- Take measurements at T3 and T4. 
- Following T4, remove oysters from the plates and clean. 
- After each timepoint, we uploaded the [plate files here](https://github.com/RobertsLab/vims-resazurin/tree/main/data/plate-files/20250702).
- Throughout, record temperature in the wells using infrared thermometer.  

We also ran two more test plates today using oysters from family 90. This family is included in our experimental plates as well. 

- Plate 71: n=20 oysters of family 90 held in the fridge (temperature = 21-22°C) 
- Plate 72: n=20 oysters of family 90 held on the benchtop (temperature = 10-16°C) 

The purpose of these two tests was to document baseline metabolic rates in these oystes. The plates were read every hour for 4 hours as we did with all other plates today.  

## Observations from today 

Here are some of our observations and troubleshooting during the trials today.  

- The resazurin solution started out colder today. This is because the plates were filled last night and then kept in the fridge all night. 
- Possibly related to this, the fluorescence values were much slower to increase during T0-T2 time periods today. The values actually looked much more like I would expect and we did not have near as many oysters that exceeded the threshold today. The range of values was similar, but the rates did not increase as dramatically during the first hour. We will account for variation by day in our statistical models. 
- We also noticed that during the heat stress period (0-2.5 hr) there was variation in temperatures in the plates with row A being the warmest and row D being the coolest (difference of 2-5°C). We hypothesized this is because of the direction they were loaded in the incubator with heat sources coming from the row A side of the plates. Luckily, there is a replicate of each family in every row, so we can account for this variation statistically. In the future, plates should be rotated during the incubation regularly. The blank values were higher in warmer rows, which will allow us to account for differences by row if we need to.    
- Blank values went back to being very consistent after removal from the incubator due to high consistency in temperature in the plates. We also saw high consistency in our test plates in the fridge and counter top, further emphasizing that the incubator induces intra-plate temperature variability.  
- During our trials, we took temperature measurements from wells in the middle of the plate (see below).  
- Temperatures followed the same pattern today as they did yesterday, with the exception of starting colder today.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/temps.png?raw=true)

- There were some oysters that were quite small and others that were quite large. I recommend using the mid range of sizes for future trials (>10mm and less than 18mm) for 24-well plates. Here is the size distrution.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/size.png?raw=true)

Here are some pictures from today.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/pic1.jpeg?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/pic2.jpeg?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/pic3.jpeg?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/pic4.jpeg?raw=true)

## Results from today 

### Temperature test plates  

Our test plates returned some interesting results! We looked at metabolic rates for family 90 at the cool, room temp, and high temperatures.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/test.png?raw=true)  

We noticed that metabolic rates are clearly much higher for oysters that went through the high temperature stress than those at room temp or chilled. Interestingly, metabolic rates at room temp were lower than we expected, but were higher than chilled metabolic rates (as we did expect). This would suggest that the oysters do not necessarily have an elevated baseline metabolic rate, but instead respond very strongly to acute stress.  

### Experimental plates 

I used the same approach as my previous resazurin analyses to produce change in size-normalized fluorescence over time for each oyster. Here are our preliminary results from today! 

- Rates were slower to increase today, but reached very similar final values over the trial.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/individual_trajectories.png?raw=true)

- There was a significant difference in metabolic rates between families. Some families had steeper slopes and some had more shallow slopes over the trial. There is a high degree of variability between families.    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/family_trajectories.png?raw=true)

- As expected due to colder starting temperature, rates were generally lower today than yesterday. This will be accounted for statistically using random effects in our models. Here is a view of each family with a separate line for each day. You can see today's values are lower. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/family_trajectories_date.png?raw=true)

- There is quite a spread in metabolic response between families.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/family_trajectories2.png?raw=true)

- When looking at salinity phenotype, there was significantly higher metabolic rates in the low salinity families. This was consistent with yesterday - here are all data for both days.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250702/phenotype_trajectories.png?raw=true)

We will add more data to these datasets Thursday! I'll also show the results of some additional analyses and modeling after we get all of the data tomorrow.  

## Preparation for tomorrow 

- I prepared the resazurin solution for tomorrow using the same recipe as I did yesterday (prepared 1.5 L). 
- We washed, labeled, and prepared all of the plates and prefilled the plates with resazurin. Plates are kept in the fridge over night. 
- We prepared all of the plate maps and data sheets for tomorrow. 

**Working resazurin solution: 1,500 mL**   

- 1,481 mL filtered seawater  
- 3,330 µL resazurin concentrated stock solution (made above) 
- 1,498 µL DMSO 
- 15 mL antibiotic solution 