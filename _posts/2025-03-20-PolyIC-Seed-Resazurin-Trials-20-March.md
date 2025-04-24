---
layout: post
title: Poly IC Resazurin Trials 20 March 2025
date: '2025-04-20'
categories: RobertsLab_Oysters PolyIC_Larvae
tags: Resazurin Oyster Cgigas
---

This post details results from resazurin trials of PolyIC seed from 20 March 2025 run at control (20°C) and high (38°C) temperatures. This is the last resazurin run for these samples.    

# Overview 

I have set up the MERLABs FLx800 plate reader for the resazurin assay and ran the first round of full resazurin trials today [using our resazurin protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/).  

All data and scripts are in our [GitHub repository](https://github.com/RobertsLab/polyIC-larvae).  

This run is repeating the [protocol I used a few days ago](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Trials-10-March/) and will be repeated 2x per week at different temperatures.  

## Proceedure 

Plate 1: 20°C (counter top, control)  
Plate 2: 38°C (incubator) 

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
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250320/20250320_plate1.jpeg?raw=true)  

Size for plate 2:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250320/20250320_plate2.jpeg?raw=true)  

I then measured resazurin fluorescence hourly from 10:00-2:00 for a 4 hour incubation.  

I then performed survival measurements after the 4 hour incubation.  

## Data analysis 

I analyzed data using scripts available on the [GitHub repository](https://github.com/RobertsLab/polyIC-larvae) and as described previously in my [past notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/PolyIC-Seed-Resazurin-Jan-28-and-29-2025/).  

Resazurin fluoresence was standardized to initial values, blank corrected, and size normalized.  

I then examined effects of time, temperature, and spat treatment on resazurin fluorescence as a proxy for metabolic rate.  

## Results 

There was a significant effect of temperature on metabolic rates and metabolic rate was significantly different between spat treatments. 

Metabolic rates peaked at 36°C and decreased as temperature increased with metabolic depression seen at 40-42°C.  

Metabolic rate tracks as expected over a thermal performance curve. Metabolic rates were elevated at 36°C (metabolic stimulation), started reducing at 38°C and were lower at 40-42°C (metabolic depression) compared to control (20°C).  
 
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250320/batch2_metabolism_temperature.png?raw=true)

Metabolic rates increased at 36°C and decreased at 40-42°C in both control and treated spat. There is more separation in rates between temperatures in treated spat.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250320/batch2_metabolism_temperature_treatment.png?raw=true) 

Metabolic rate was different between control and treated spat at 36°C (higher in treated) and 40°C (higher in control). The other temperatures were not significantly different. You can see the changes in rates across temperatures performs as expected with decreasing metabolic rate as temperatures increased past 36°C.     

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/polyic/20250320/batch2_metabolism_treatment.png?raw=true)
   