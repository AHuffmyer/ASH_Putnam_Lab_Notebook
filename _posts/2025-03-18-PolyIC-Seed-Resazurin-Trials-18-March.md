---
layout: post
title: Poly IC Resazurin Trials 18 March 2025
date: '2025-03-18'
categories: RobertsLab_Oysters PolyIC_Larvae
tags: Resazurin Oyster Cgigas
---

This post details results from resazurin trials of PolyIC seed from 18 March 2025 run at control (20°C) and high (42°C) temperatures. This is a continuation of trials I ran earlier this week.     

# Overview 

I have set up the MERLABs FLx800 plate reader for the resazurin assay and ran the first round of full resazurin trials today [using our resazurin protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/).  

All data and scripts are in our [GitHub repository](https://github.com/RobertsLab/polyIC-larvae).  

This run is repeating the [protocol I used a few days ago](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Trials-10-March/) and will be repeated 2x per week at different temperatures.  

## Proceedure 

Plate 1: 20°C (counter top, control)  
Plate 2: 42°C (incubator) 

Each plate had a temperature logger with it.  

I prepared oysters using the following plate map.  

- Wells A1-A3 = blanks
- Wells A4-B12 = control tank 2
- Wells C1-C3 = blanks
- Wells C4-D12 = treated tank 5
- Wells E1-E3 = blanks
- Wells E4-F12 = control tank 1
- Wells G1-G3 = blanks
- Wells G4-H12 = treated tank 4

I set oysters on the lid of the plates and took an image with a scale bar for size. I used ImageJ to measure max length of each oyster for analysis.  

Size for plate 1:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250318/20250318_plate1.jpeg?raw=true)  

Size for plate 2:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250318/20250318_plate2.jpeg?raw=true)  

I then measured resazurin fluorescence hourly from 10:00-2:00 for a 4 hour incubation.  

I then performed survival measurements after the 4 hour incubation.  

## Data analysis 

I analyzed data using scripts available on the [GitHub repository](https://github.com/RobertsLab/polyIC-larvae) and as described previously in my [past notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Jan-28-and-29-2025/).  

Resazurin fluoresence was standardized to initial values, blank corrected, and size normalized.  

I then examined effects of time, temperature, and spat treatment on resazurin fluorescence as a proxy for metabolic rate.  

## Results 

There was a significant effect of temperature on metabolic rates and metabolic rate was significantly different between spat treatments. 

Metabolic rates were lower at 42°C than 20°C, indicative of metabolic depression under elevated moderate temperatures that we have seen before. 

Metabolic rate tracks as expected over a thermal performance curve. Metabolic rates were elevated at 36°C (metabolic stimulation) and lower at 40 and even lower at 42°C (metabolic depression) compared to control (20°C). 

I'll run a trial at 38°C next.   
 
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250318/batch2_metabolism_temperature.png?raw=true)

Metabolic rates decreased at 42°C in both control and treated spat. There is more separation in rates between temperatures in treated spat.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250318/batch2_metabolism_temperature_treatment.png?raw=true) 

Treated spat had higher metabolic rates at 36°C than control spat. Metabolic rates were depressed at 40°C and were lower in treated spat. Metabolic rates decreased at 42°C in both treated and control spat.    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250318/batch2_metabolism_treatment.png?raw=true)

Metabolic rates were significantly different between treated and control spat at 36 and 40°C, but not at 20°C or 42°C.  

# Next steps 

On Tuesday of this week, I will run another trial at control and 38°C to wrap up this resazurin experiment.    