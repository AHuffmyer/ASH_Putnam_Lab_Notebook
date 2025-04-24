---
layout: post
title: Resazurin Metabolic Assays Protocol for Steven's VIMS trip
date: '2025-04-20'
categories: Protocol RobertsLab_Oysters
tags: Cgigas Oyster Protocol Resazurin Survival
---

This post details the working protocol for test of individual oyster response to stress using the resazurin metabolic assay. This protocol is meant for demonstration purposes only.   

## Overview 

This protocol is based on [previous protocols for conducting measurements of small <6 mm seed in 96 well plates](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/) and [15-20mm seed in plastic cups](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-Individual-Stress-Testing/). 

# Materials and preparing solutions

Concentrated stock solution will be made prior to trials and shipped to VIMS. The recipe is below for reference. Only the "working solution" will need to be made.  

### 1. Stock resazurin solution 

Stock solution recipe for 20 mL volume:  

- [1.0 g resazurin salt](https://www.thermofisher.com/order/catalog/product/R12204)
- 20 mL DI water
- 20 µL DMSO

Store in a dark fridge or freezer.  

### 2. Working resazurin solution

To prepare the working solution of resazurin, prepare the following to make 500 mL of solution. Adjust if more or less volume is needed. 

- 493 mL filtered seawater or DI water with Instant Ocean adjusted to approx. 25 ppt 
- 1,110 µL resazurin stock solution (pre made, above)
- 500 µL DMSO

An antibiotic solution is typically added for experimental trials. For demonstration purposes this is removed. 

### 3. Supplies 

- Plastic cups with lids 
	- Aim for 1 mL volume for each mm in oyster length
	- For example, a 20 mm oyster will need to be in 20 mL of solution
- Paper towels and bench paper/pads 
- Spectrophotometer plate reader (Agilent Synergy HTX) and software 
- Filter sets for fluorescence measurements of 528/20 excitation and 590/20 emission 
- 25 mL glass seriological pipette and dispenser 
- P1000 pipette 
- 96 well plate 
- Incubator

# Protocol

For this demonstration, conduct hourly measurements or photographs of oysters in resazurin at a control and high (>40°C) temperature. 

Volume are set lower than typically used to elicit a stronger visual change in color. Fluorescence measurements will be the best way to detect differences that you cannot see visually. 

Include 1-2 samples that are blanks (no oysters) as reference. 

## Schedule 

Here is an example schedule. 

- 09:00-10:00: Prepare solutions, load oysters in cups 
- 10:00: Time 0 initial measurement   
- 11:00: Time 1 measurement   
- 12:00: Time 2 measurement  
- 13:00: Time 3 measurement  
- 14:00: Time 4 measurement  
- 15:00: Time 5 measurement (optional, but preferred if possible)
- 15:30-16:00: Clean up 

*If taking photographs of the resazurin for looking at color changes by eye, do incubations for 5 h. If measuring using fluorescence, you can see differences by 3-4 h.*  

## Load and prepare samples  

Before starting, set the incubator at desired high temperature (40-42°C). Control oysters will be set on counter/lab bench at 16-20°C. If needed, use an incubator to cool controls below 20°C.     

1. Prepare oysters for assays and try to use oysters in a similar size range.   
2. Place oysters in a labeled cup, beaker, or tripour.  
3. Prepare n=1-2 cups for blank samples that will have solution, but no oysters, for reference. 
4. Fill each cup with the required volume of resazurin working stock. Oysters should be completely submerged at a volume of approximately 0.5 mL per mm length. For example, a 20 mm oyster could be in 10 mL of water as long as it is submerged. 
5. Proceed with the first measurement (next step below). 

## Measurements 

1. Turn on the computer and plate reader. Open the plate reader software. 
2. Program a protocol to measure resazurin according to the plate reader specifications. 
	- Excitation = 528 or 530 nm (band pass 20-30 nm)
	- Emission = 590 nm (band pass 20-30 nm)
	- Read measurements from top
	- End point measurements 
	- Fluorescence mode 
3. Take a 280-300µL sample from each oyster cup and place into a 96-well plate. Track the sample ID that goes into each well and the temperature treatment. 
4. Use the protocol in the software to take a measurement of the plate (initial measurement; T0) before starting the incubation. Load plate without lid into the plate reader. 
5. Save the data file in a .txt, .csv, or .xlsx format as `YYYYMMDD_##C_T0.txt`. For example, `20250128_18C_T0_fl.txt` or `20250128_42C_T0.txt`. Be sure to include the date, temperature, and time point in the output file.   
6. As a sample is taken, observe whether the oyster is dead or alive. If the oyster is dead, record this in the notebook in a table that includes the time point and a list of the ID's of dead oysters. 
7. After collecting a sample from each cup, move the oysters back into the incubator.
8. Rinse the 96 well plate thoroughly. 
9. Repeat at 1, 2, 3, 4, and 5 (optional) hours of incubation.
10. If doing a demonstration, you can visualize the change over time using Excel or by comparison of photographs. 

If desired, example scripts can be found on [GitHub here for the PolyIC project](https://github.com/RobertsLab/polyIC-larvae/blob/main/scripts/resazurin/resazurin-analysis-batch2.Rmd). 

## Clean up

After the final measurement and survival assessment at 4 or 5 h exposure the trial is completed. Follow these steps for clean up.  

- Empty the resazurin from each cup into a hazardous waste collection container. 
- Place oysters in a large tripour or cup and then put them in the compost. 
- Rinse all cups, pipettes, and plates thoroughly. 
- Save all data to the zip drive and upload to GitHub or proceed with analysis in R.  
- Ensure all data is recorded in the notebook including time of trials, survival, and any relevant notes. 

## Assemble data 

If using R scripts for analysis, prepare the following data sets:  

- Metadata: A data sheet with columns for date, temperature, oyster ID, and relevant notes
- Plate reader files 

# Notes 

Protocol is in progress and will be revised as testing is conducted.  

