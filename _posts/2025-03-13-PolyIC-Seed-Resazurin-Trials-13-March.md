---
layout: post
title: Poly IC Resazurin Trials 13 March 2025
date: '2025-03-13'
categories: RobertsLab_Oysters PolyIC_Larvae
tags: Resazurin Oyster Cgigas
---

This post details results from resazurin trials of PolyIC seed from 13 March 2025 run at control (20°C) and high (36°C) temperatures. This is a continuation of trials I ran earlier this week.     

# Overview 

I have set up the MERLABs FLx800 plate reader for the resazurin assay and ran the first round of full resazurin trials today [using our resazurin protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/).  

All data and scripts are in our [GitHub repository](https://github.com/RobertsLab/polyIC-larvae).  

These trials were run on spat brought back from the hatchery last week ("batch 2").  

This run is repeating the [protocol I used a few days ago](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Trials-10-March/) and will be repeated 2x per week at different temperatures.  

## Proceedure 

Plate 1: 20°C (counter top, control)  
Plate 2: 36°C (incubator) 

Each plate had a temperature logger with it.  

I prepared oysters using the following plate map.  

- Wells A1-A3 = blanks
- Wells A4-B12 = treated tank 4
- Wells C1-C3 = blanks
- Wells C4-D12 = control tank 1
- Wells E1-E3 = blanks
- Wells E4-F12 = treated tank 5
- Wells G1-G3 = blanks
- Wells G4-H12 = control tank 2

I set oysters on the lid of the plates and took an image with a scale bar for size. I used ImageJ to measure max length of each oyster for analysis.  

Size for plate 1:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250313/20250313_plate1.jpeg?raw=true)  

Size for plate 2:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250313/20250313_plate2.jpeg?raw=true)  

I then measured resazurin fluorescence hourly from 10:30-2:30 for a 4 hour incubation.  

Noah then performed survival measurements after the 4 hour incubation.  

## Data analysis 

I analyzed data using scripts available on the [GitHub repository](https://github.com/RobertsLab/polyIC-larvae) and as described previously in my [past notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Jan-28-and-29-2025/).  

Resazurin fluoresence was standardized to initial values, blank corrected, and size normalized.  

I then examined effects of time, temperature, and spat treatment on resazurin fluorescence as a proxy for metabolic rate.  

## Results 

There was a significant effect of temperature on metabolic rates and metabolic rate was significantly different between spat treatments. 

Metabolic rates were higher at 36°C than 20°C, indicative of metabolic stimulation under elevated moderate temperatures. Metabolic rates were lower at 40°C.   
 
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250313/batch2_metabolism_temperature.png?raw=true)

The difference in metabolic rates between 20°C and 36°C was greater for treated spat than for control spat. This was also true for 40°C trials from a couple days ago.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250313/batch2_metabolism_temperature_treatment.png?raw=true) 

Treated spat had higher metabolic rates at 36°C than control spat. Metabolic rates were depressed at 40°C.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250313/batch2_metabolism_treatment.png?raw=true)

# Next steps 

On Tuesday of next week, I will run another trial at control and 42°C and again Thursday at 38°C and 20°C.  