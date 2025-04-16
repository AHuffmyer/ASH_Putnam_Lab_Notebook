---
layout: post
title: Resazurin Metabolic Assays Protocol for Individual Stress Testing
date: '2025-04-15'
categories: Protocol RobertsLab_Oysters
tags: Cgigas Oyster Protocol Resazurin Survival
---

This post details the working protocol for test of individual oyster response to stress using the resazurin metabolic assay. This protocol is used to measure change in metabolism from ambient to high temperature in individual seed.  

## Overview 

This protocol is written for 15-20 mm oyster seed, but can be adapted for other life stages. This protocol is based on [previous protocols for conducting measurements of small <6 mm seed in 96 well plates](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/). 

We are using this protocol to test metabolic response of seed oysters to thermal stress across multiple families to examine genetic variation in responses. We will be conducting assays on individual oysters at ambient temperature followed by high temperature to characterize individual metabolic response to stress.  

# Materials and preparing solutions

For this protocol, we expect to analyze 45-50 samples (20 mL volume per sample) in each batch. Resazurin working stock will be made fresh for each batch. Concentrated stock solution will be made once prior to trials. 

### 1. Stock resazurin solution 

To make the resazurin stock solution (10 mL) mix the following. We will use this solution for multiple trials.  

- [0.5 g resazurin salt](https://www.thermofisher.com/order/catalog/product/R12204)
- 10 mL DI water
- 10 µL DMSO

Store in a dark fridge or freezer.  

### 2. Working resazurin solution

To prepare the working solution of resazurin, prepare the following to make 2,000 mL prior to each batch of trials. Generate into two 1,000 mL bottles. This volume includes a little extra to allow for errors in pipetting.  

- 1,973 mL seawater DI water with Instant Ocean adjusted to approx. 25 ppt) 
- 4,440 µL (4.44 mL) resazurin stock solution
- 2,000 µL (2 mL) DMSO
- 20 mL antibiotic solution [100x Penn/Strep & 100x Fungizone](https://us.vwr.com/store/product/4648458/null) - this should be frozen in a dark freezer and defrosted before use

Use one of the bottles (1,000 mL) to fill sample cups (see below). Place the other in an incubator heated to 42°C to pre heat for the stress trial.  

### 3. Supplies 

- Two Hobo TidbiT [temperature loggers](https://www.onsetcomp.com/products/data-loggers/mx2203)
	- One placed in incubator and one placed on counter at room temperature 
- Plastic cups with lids (25 mL volume)
- Paper towels and bench paper/pads 
- Tweezers, transfer pipettes, and forceps 
- Dissecting microscope 
- Spectrophotometer plate reader (Agilent Synergy HTX) and software 
- Filter sets for fluorescence measurements of 528/20 excitation and 590/20 emission 
- 25 mL glass seriological pipette and dispenser 
- Scale bar/ruler
- 96 well plate 

For this experiment, we pre-labeled cups with the family ID ("A", "B", "C", and so on) and a serial number. For example, "A1", "A2", and so on. 

We will be conducting measurements on n=8 oysters per family per batch. We have five families, which will total to 40 oysters measured per batch. Five blanks will be run as well. 

Measurements will be repeated over 3-4 batches to obtain final replication of n=24-32 per family. 

# Protocol

On each day, we will measure metabolic response of oysters first at ambient temperature (3 h; 18-20°C) and then at high temperature (3 h; 42°C).  

We will measure metabolic rate every hour and assess survival at each hour.  

After the ambient period, resazurin will be refreshed using the pre heated working solution and measurements will start again.  

## Schedule 

Each day, the schedule will be as follows. 

08:00-09:00: Prepare solutions, load oysters in cups, take size images   
09:00: Ambient Time 0 measurement   
10:00: Ambient Time 1 measurement   
11:00: Ambient Time 2 measurement  
12:00: Ambient Time 3 measurement   
12:00-13:00: Refresh resazurin and start high temperature exposure
13:00: High Time 0 measurement
14:00: High Time 1 measurement
15:00: High Time 2 measurement
16:00: High Time 3 measurement

*We will test this schedule and see if we need to extend the time. From previous tests, differences were clearly seen by 3 hours of incubation.*  

## Load and prepare samples  

Before starting, set the incubator at 18°C and pre heat the solution in another incubator set at 42°C as listed above.   

1. Prepare seed for assays by getting some seed from each family bag in the FTR tank room. We will need n=8 seed per family within the 15-20 mm size range. 
2. Place the seed next to their empty cups with their specified ID such that the oyster and the ID can be seen next to each other.
3. Take an image with a scale bar/ruler included that includes all of the oysters next to their ID cups. We will use the images to measure size for size normalization.  
4. Prepare n=5 cups for blank samples ("blank 1", "blank 2", and so on). 
5. Write the sample IDs in the notebook. 
6. Move the oysters into their respective cups. 
7. Fill each cup with 20 mL of resazurin working stock at ambient temperature using a seriological pipette. Rinse the pipette with DI water after this step. 
8. Move the oysters to the incubator set at 18°C and stack in random order. Proceed with the first measurement (next step below). 
9. Ensure a temperature logger is in the incubator. 

## Measurements at 18°C 

1. Turn on the computer and plate reader. Open the plate reader software. 
2. Use the "oyster-resazurin" protocol in the software to take a measurement of each sample at T0 before starting the incubation. 
3. Take a sample of 250-280 µL out of each cup using a transfer pipette and add to a 96 well plate. Rinse the pipette with DI water between samples. Put sample A1 in well A1 and so on.  
4. As a sample is taken, observe whether the oyster is dead or alive. If the oyster is dead, record this in the notebook in a table that includes the time point and a list of the ID's of dead oysters. Even if the oyster is dead, collect a resazurin sample.
5. After collecting a sample from each cup, move the cups back into the incubator in random order to randomize position in the incubator. 
6. Record the sample locations in the notebook. 
7. Put the plate on the loading platform. 
8. Collect and export readings as directed in the plate software. 
9. Save the file as: `YYYYMMDD_18C_T0.xlsx`. For example, `20250128_18C_T0.xlsx`. 
10. Save the data to a flash drive and add to GitHub at the [location here under the Data directory](https://github.com/RobertsLab/oyster-resazurin). 
11. Record the time of the measurement. Rinse the 96 well palte thoroughly. 
12. Repeat at 1, 2, and 3 hours of incubation at 18°C.

## Measurements at 42°C 

1. After the T3 measurement is taken in the ambient condition, we will now conduct the measurements at 42°C. 
2. Empty the resazurin from each cup into a hazardous waste collection container and rinse with seawater briefly to remove most resazurin residue. Keep the oyster in their cup! 
3. Change the incubator to 42°C.
4. Now fill each cup with the pre heated 42°C resazurin solution. 
5. Immediately take a sample for the T0 measurement of the high temperature phase using the same protocol as described above for taking measurements. Survival does not need to be assessed at this time point. 
6. Collect and export readings as directed in the plate software. 
7. Save the file as: `YYYYMMDD_42C_T0.xlsx`. For example, `20250128_42C_T0.xlsx`. 
8. After collecting a sample from each cup, move the cups back into the incubator now at 42°C in random order to randomize position in the incubator. 
9. Record the time that samples were taken and added back into the incubator. 
10. Save the data to a flash drive and add to GitHub at the [location here under the Data directory](https://github.com/RobertsLab/oyster-resazurin). 
11. Record the time of the measurement. Rinse the 96 well plate thoroughly. 
12. Repeat at 1, 2, and 3 hours of incubation at 42°C.

## Clean up

After the final T3 measurement and survival assessment at 42°C, the trial is completed. Follow these steps for clean up.  

- Empty the resazurin from each cup into a hazardous waste collection container. 
- Place oysters in a large tripour or cup and then put them in the compost. 
- Rinse all cups, pipettes, and plates thoroughly. 
- Save all data to the zip drive and upload to GitHub or send to Ariana. 
- Ensure all data is recorded in the notebook including time of trials, survival, and any relevant notes. 
- Take images of the notebook and upload to GitHub or send to Ariana 

## Assemble data 

- Survival: A data sheet with columns for date, temperature, time point, oyster ID, family ID, and relevant notes 
- Metadata: A data sheet with columns for date, temperature, oyster ID, family ID, and relevant notes

Scripts for analysis will be available on GitHub [here](https://github.com/RobertsLab/oyster-resazurin). 

# Notes 

Protocol is in progress and will be revised as testing is conducted.  

