---
layout: post
title: Poly IC Resazurin Trials 10 March 2025
date: '2025-03-10'
categories: RobertsLab_Oysters PolyIC_Larvae
tags: Resazurin Oyster Cgigas
---

This post details results from resazurin trials of PolyIC seed from 10 March 2025 run at control (20°C) and high (40°C) temperatures.  

# Overview 

I have set up the MERLABs FLx800 plate reader for the resazurin assay and ran the first round of full resazurin trials today [using our resazurin protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/).  

All data and scripts are in our [GitHub repository](https://github.com/RobertsLab/polyIC-larvae).  

These trials were run on spat brought back from the hatchery last week ("batch 2").  

## Proceedure 

Plate 1: 20°C (counter top, control)  
Plate 2: 40°C (incubator) 

Each plate had a temperature logger with it.  

I prepared oysters using the following plate map.  

- Wells A1-A3 = blanks
- Wells A4-B12 = control tank 1
- Wells C1-C3 = blanks
- Wells C4-D12 = treated tank 4
- Wells E1-E3 = blanks
- Wells E4-F12 = control tank 2
- Wells G1-G3 = blanks
- Wells G4-H12 = treated tank 5

I set oysters on the lid of the plates and took an image with a scale bar for size. I used ImageJ to measure max length of each oyster for analysis.  

Size for plate 1:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250310/20240310_plate1.jpeg?raw=true)  

Size for plate 2:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250310/20240310_plate2.jpeg?raw=true)  

I then measured resazurin fluorescence hourly from 10:30-2:30 for a 4 hour incubation.  

Noah then performed survival measurements after the 4 hour incubation.  

## Data analysis 

I analyzed data using scripts available on the [GitHub repository](https://github.com/RobertsLab/polyIC-larvae) and as described previously in my [past notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Jan-28-and-29-2025/).  

Resazurin fluoresence was standardized to initial values, blank corrected, and size normalized.  

I then examined effects of time, temperature, and spat treatment on resazurin fluorescence as a proxy for metabolic rate.  

## Results 

There was a significant effect of temperature on metabolic rates and metabolic rate was significantly different between spat treatments. 

Metabolic rates were lower at 40°C than 20°C, indicative of metabolic depression under high stress. 
 
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250310/batch2_metabolism_temperature.png?raw=true)

THe difference in metabolic rates between 20°C and 40°C was greater for treated spat than for control spat.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250310/batch2_metabolism_temperature_treatment.png?raw=true) 

Treated spat had lower metabolic rates at 40°C than control spat.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250310/batch2_metabolism_treatment.png?raw=true)

# Next steps 

On Thursday of this week, I will run another trial at control and 36°C.  

Noah O. is collecting survival data today and I will analyze mortality as a function of survival after this is completed.  