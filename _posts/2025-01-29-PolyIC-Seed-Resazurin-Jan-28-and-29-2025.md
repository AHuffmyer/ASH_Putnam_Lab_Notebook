---
layout: post
title: PolyIC Seed Resazurin Assay January 28 and 29 2025
date: '2025-01-29'
categories: PolyIC_Larvae Protocol RobertsLab_Oysters
tags: Cgigas Oyster Protocol Resazurin Survival
---

This post details resazurin analyses conducted from 28-29 January 2025. 

# Overview 

We are running resazurin metabolic assays in the lab to test thermal tolerance of seed from the [PolyIC immune hardening project](https://github.com/RobertsLab/polyIC-larvae). In this project, we exposed broodstock to PolyIC immune challenges and then reared their offspring through larval stages and now we have seed in the lab. Our goal is to evaluate whether parental immune challenge elicits differences in thermal tolerance in seed.  

More information can be found on the [project GitHub](https://github.com/RobertsLab/polyIC-larvae) and in [my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#polyic-larvae).   

The protocol for this work can be found in [my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/) and will be continually updated as we conduct assays.  

We have [previously used resazurin assays](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-data-analysis-and-size-normalization-for-10K-seed-project/) to characterize effects of thermal stress in oysters and found strong differences in metabolic rate under stress as well as significant differences in metabolic sensitivity and mortality. We will adapt these assays for use here to examine thermal tolerance in our PolyIC seed.  

Undergraduate researchers in our lab are working on this project and [are posting daily notebook posts on our lab blog here](https://genefish.wordpress.com/).  

# Tank conditions 

Oysters are being maintained in FTR tanks at 10°C with daily pH, salinity, and temperature measurements. Oysters are fed weekly with Reed Mariculture Shellfish Diet 1800 (batch 24188). A water change (parital, 5 gal) is performed weekly and salinity is maintained at 23-25 psu. These measurements are recorded in the tank room lab notebook and entered into the GitHub repo linked above.  

We also have temperature loggers in the FTR tanks (one in each tank) as well as a temperature logger at control and high resazurin temperatures.  

# Preparing solutions 

I prepared solutions [as written in the protocol in my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/).  

# 28 January 2025 

We ran a full trial as written in [our working protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/).  

Today, we randomly selected data collection at the control temperature (16°C) and an elevated temperature (40°). In previous [testing](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Assay-Testing/), there was complete mortality at 42°C with metabolic depression. In addition, there was an increase of metabolic rates at a lower temperature of 36°C. Therefore, we decided to conduct this measurement at multiple temperatures and reduce the upper thermal stress to 40°C.  

The temperatures we will use are:  

15-16°C (control)   
25°C  
30°C  
35°C  
40°C  

Each day we conduct measurements, we will randomly select an elevated temperature. We will run plates at control and the selected elevated temperature to allow us to account for any background variation in response due to time and conditions in FTR tanks.   

We will plan to conduct these measurements over 4-5 days of data collection (1-2 days of collection each week).  

## Notes from our methods proceedures 

### size images 

We found that it was best to photograph size by inverting the plate and imaging the spat from the bottom. This was only possible if the spat stuck to the bottom of the plate. I was able to do this successfully by adding spat to a plastic petri dish and allocating with tweezers. This way, water tension from the wet oyster held them on the bottom of the plate.  

Here is an example:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/size.jpg?raw=true) 

If the oysters were dried before adding to the plate, they did not stick. In this case, we took images across the plate with the oyster at the bottom of the well. This just requires multiple images to see all spat.  

Here is an example:  

![](https://github.com/RobertsLab/polyIC-larvae/blob/main/data/resazurin/images/20250128/plate2/IMG_8765.jpeg?raw=true)  

![](https://github.com/RobertsLab/polyIC-larvae/blob/main/data/resazurin/images/20250128/plate2/IMG_8766.jpeg?raw=true)  

In order to size normalize data, we will measure the length of each oyster and normalize change in fluorescence to oyster length (mm).  

It will be much easier to measure size using the plate inversion method, so we should try to do this as much as possible.  

### Allocating oysters 

We were able to load the oysters into four plates allocated as [described in our protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/) in about 30 minutes with three people. With one person, this will take about 1 hour.   

We loaded four plates today, which will be the loading protocol we will use each day at the specified temperature.  

- *Plate 1*: 16°C (control temperature) with control spat. Wells A1-D6 = control-1; wells D7-E6 = blanks; wells E7-H12 = control-2

- *Plate 2*: 16°C (control temperature) with treated spat. Wells A1-D6 = treated-4; wells D7-E6 = blanks; wells E7-H12 = treated-5

- *Plate 3*: Elevated temperaure (40°C) with control spat. Wells A1-D6 = control-1; wells D7-E6 = blanks; wells E7-H12 = control-2 

- *Plate 4*: Elevated temperature (40°C) with treated spat. Wells A1-D6 = treated-4; wells D7-E6 = blanks; wells E7-H12 = treated-5

### Reading plates 

Reading plates each hour was simple and easy! We read the values each hour and I analyzed the data live using the script [here](https://github.com/RobertsLab/polyIC-larvae/blob/main/scripts/resazurin/resazurin-analysis.Rmd).  

I generated results and describe the preliminary results below.  

### Survival assessments 

We used two microscopes with two people observing and one person recording survival. We recorded survival in a plate map format with an "X" designating a well that had a dead oyster.  

We took out each oyster and looked at it under a microscope [following our protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/).    

We processed survival in about an hour and I expect it would take 2-2.5 hours with a single person processing.  

## Preliminary results 

Until we analyze oyster size, the results below are individual-normalized and are therefore preliminary and will change after size normalization.  

### 1. Elevated metabolic rate at 40°C compared to 16°C 

Metabolic rates were significantly higher at 40°C than 16°C.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/metabolism_temperature.png?raw=true)

### 2. No significant differences between spat treatments 

There was no difference in metabolic rates between spat treatments. There is a trend for a difference at control temperature, but this is not significant.  

Note that this may change after we conduct size normalization.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/metabolism_treatment.png?raw=true)

### 3. Lower mortality in treated spat compared to control spat both at 16°C and 40°C 

Mortality was higher in control spat at both temperatures. It is interesting that treated spat had lower mortality. We will see if this relates to size.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/mortality_treatment.png?raw=true)

### 4. High mortality at 40°C compared to 16°C  

As expected, mortality was higher at 40°C with lower risk in treated spat.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/mortality_temperature.png?raw=true)

## Next steps 

Next, we will repeat the procedure at another temperature and conduct size normalization.  

## Notebook images 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb1.jpeg?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb2.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb3.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb4.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb5.jpeg?raw=true)

# 20250129  

Today I was going to process another trial at 35°C, but when I went to start the plate reader it was broken and had a motor error.  

I contacted the company and am moving forward with trying to get repairs and a new plate reader.  

I'll try again next week. I spent the day analyzing yesterday's data and organizing data in the GitHub repo.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb6.jpeg?raw=true)

