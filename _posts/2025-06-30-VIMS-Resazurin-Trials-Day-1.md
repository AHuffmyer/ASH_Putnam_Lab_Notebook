---
layout: post
title: VIMS Resazurin Trials Day 1
date: '2025-06-30'
categories: VIMS_Oysters
tags: Resazurin Oyster 
---

This post details resazurin trials at VIMS. Today, we conducted temperature testing and prepared supplies for full trials. We used data from today to make decisions about trial conditions tomorrow.  

# VIMS Resazurin Trials  

## 20250630: Temperature testing day 

Today we ran trials in which we tested resazurin response of 3 oyster lines (*Crassostrea virginica*) to 40°C, 42°C, 44°C, and 46°C.  

The lines we tested are related to the families we will test later this week and are as follows: 

- "Lola": Selected for low salinity 
- "Henry": Selected for high/moderate salinity 
- "Wild": Wildtype   

### Resazurin solutions 

We first prepared resazurin solutions using our base [resazurin protocol](https://robertslab.github.io/resources/Lab-Protocols/) and protocols used in our [resazurin optimization work](https://github.com/RobertsLab/resazurin-assay-development).   

We will use the following sampling design for today:  

- 8 plates * 48 wells = 384 wells x 1.5 mL 
- Equals 576 mL working solution required at a minimum (round to 700 mL)

We rounded up to 700 mL to have enough for any additional testing.  

**Concentrated stock solution for the week: 30 mL**    

- 30 µL DMSO 
- 1.5 g Resazurin 
- 30 mL DI water 

**Working resazurin solution: 700 mL**   

- 692 mL filtered seawater 
- 1556 µL resazurin concentrated stock solution 
- 700 µL DMSO 
- 7 mL antibiotic solution 

### Preparing oysters 

After solutions were made, we prepared our 8 48-well plates with the following samples in each plate:  

- n=8 blanks (column 1)
- n=21 oysters from each family (two rows of 7 for each family) 

Family row location was varied between the plates. This way, each family was exposed in different rows across plates. Blanks were always in column 1.  

Prior to loading the plates, we took a picture for size as we do in our standard protocol. I then measured shell length in ImageJ.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250630/pic1.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250630/pic2.jpeg?raw=true)  

### Plate reader and incubators

We then took initial measurements on the plate reader with the following specifications:  

- Model: Varioskan Lux 
- Manufacturer: ThermoScientific 
- Excitation 530
- Emmission 590
- Read from top 
- Band width = 12 
- Read with SkanIt software 

The initial resazurin readings were low (<100), which may be due to the instrument or the batch of working solution. I'll check these values again with new solution in the morning with our new round.  

Samples were then loaded (n=2 plates per temperature) into drying ovens at 40, 42, 44, and 46°C. The timer was then started and plates were re-read on the plate reader every hour.  

During the measurements, I kept getting an error randomly that said "waste wells not loaded". I called the manufacturer and they had me uncheck the "check plate and pipette priming" setting in the instrument settings to stop this error.  

### Resazurin rsults 

I analyzed the resazurin data as I normally do by conducting blank correction and size normalization. All data and code and figures are at our [GitHub repo here](https://github.com/RobertsLab/vims-resazurin/tree/main).  

The raw data for each sample looked like this:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250630/individual_trajectories.png?raw=true)

And here are summary models for trajectories grouped by family and temperature:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250630/family_trajectories.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250630/family_trajectories_temp.png?raw=true)

There are a few things I noticed right off the bat: 

- At the higher temperatures, resazurin fluorescence crashes after time point 2. This corresponded with our observations of very rapid changes in the resazurin solution to a bright pink color. This happened much more quickly than we expected (described below). This is likely due to oversaturation and completion of the resazurin reaction. Metabolic rates exceeded the capacity of resazurin to detect differences at high metabolic rates.  
- This crash occured only at >42°C and did not seem to occur at 40°C. 
- There were some interesting family level differences in which the H family had higher initial metabolic rates, leading to a more rapid crash. 
- There is a dip in fluorescence at timepoint 2. This is when the plate reader started having errors and plates sat out for a bit longer than other time points.  

The over saturation of resazurin may be due to one or more of the following reasons: 

- Higher baseline metabolic rates in these oysters is very possible because they are at a high seasonal baseline temperature and feeding rate. 
- Species differences in metabolism are also possible. 
- The 48 well plate volume is likely low for the size of oysters we used. A larger volume may be required for this size of oyster. 

Here are some images showing the stark changes in color.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250630/pic3.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250630/pic4.jpeg?raw=true)  
 
Overall, our biggest findings were: 

- 40°C is the most appropriate temperature to use because we did not exceed the capacity of the resazurin reaction
- There are statistical differences in metabolism between lines. 
- We will adjust our approach for tomorrow based on what we learned (see below).  

### Additional testing 

We also ran some quick tests with oysters at 4°C, 21°C, and 36°C to see if baseline metabolism is indeed higher than we typically see in our oysters and if a lower temperature may be more appropriate. 

We saw lower metabolic rates at 4°C. Rates were also lower at 21°C and 36°C, but still higher than we see in our trials, indicating there may be higher baseline metabolic rates here at VIMS.  

After 1 h, we put the 4°C plate at 49°C for an additional hour, to see if increased temperature raised metabolic rates. Indeed, rates did increase (visually via increasing pink in the solution) after 1 h at 40°C, but not as high of an increase as our primary trial at 40°C today.  

We decided to use this tomorrow to our advantage by adding chilled resazurin to the plates to slow the initial increase in metabolic rates at 40°C, allowing us to distinguish between any family level differences.  

### Mortality

Only one oyster died from all of the samples. This was lower than expected, but may be due to higher tolerance due to species, seasonality, or duration.  

### Decisions for tomorrow 

We made the following decisions for tomorrow. 

- Run n-50 families with n=28 oysters per family spread across the two main phenotypes (high salinity, low/moderate salinity), resulting in n=25 families per phenotype 
- Run assays in 24 well plates with n=4 blanks per plate and n=20 oysters per plate 
- Plates will have n=4 replicates of randomly selected families per plate 
- 70 total plates will be required to fit all replicates and we will try to run 22-24 per day 
- Load plates in cold resazurin and extend trials up to 6 hours as needed 
- Run assays at 40°C 

### Resazurin solution for tomorrow 

I made a new batch of 1.5 L (1,500 mL) working solution and stirred the concentrated solution again before adding.   

**Working resazurin solution: 1,500 mL**   

- 1,481 mL filtered seawater  
- 3,330 µL resazurin concentrated stock solution (made above) 
- 1,498 µL DMSO 
- 15 mL antibiotic solution 

### Preparing data for tomorrow 

I then prepared the scripts, empty dataframes, and plate maps for tomorrow. All data is available on our GitHub [here](https://github.com/RobertsLab/vims-resazurin/tree/main).   