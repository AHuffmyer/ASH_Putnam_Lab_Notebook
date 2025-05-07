---
layout: post
title: Resazurin Metabolic Assays Protocol for Individual Stress Testing
date: '2025-05-07'
categories: Protocol RobertsLab_Oysters
tags: Cgigas Oyster Protocol Resazurin Survival
---

This post details the working protocol for test of individual oyster response to stress using the resazurin metabolic assay. This protocol is used to measure change in metabolism from ambient to high temperature in individual seed.  

## Overview 

This protocol is written for 15-20 mm oyster seed, but can be adapted for other life stages. This protocol is based on [previous protocols for conducting measurements of small <6 mm seed in 96 well plates](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/). 

We are using this protocol to test metabolic response of seed oysters to thermal stress across multiple families to examine genetic variation in responses. We will be conducting assays on individual oysters at ambient temperature followed by high temperature to characterize individual metabolic response to stress.

*This protocol requires the use of GitHub for data storage and entry. GitHub details are not discussed in this protocol.*    

# Materials and preparing solutions

For this protocol, we expect to analyze 30-45 samples (10 mL volume per sample) in each batch. Resazurin working stock will be made fresh for each batch. Concentrated stock solution will be made once prior to trials. 

### 1. Stock resazurin solution 

To make the resazurin stock solution (approx. 20 mL) mix the following. We will use this solution for multiple trials.  

- [1.0 g resazurin salt](https://www.thermofisher.com/order/catalog/product/R12204)
- 20 mL DI water
- 20 µL DMSO

Store in a dark fridge or freezer.  

### 2. Working resazurin solution

To prepare the working solution of resazurin, prepare the following to make 500 mL prior to each batch of trials. This volume includes a little extra to allow for errors in pipetting and testing additional samples.  

- 194 mL seawater DI water with Instant Ocean adjusted to approx. 25 ppt) 
- 1,110 µL (1.11 mL) resazurin stock solution
- 500 µL (0.5 mL) DMSO
- 5 mL antibiotic solution [100x Penn/Strep & 100x Fungizone](https://us.vwr.com/store/product/4648458/null) - this should be frozen in a dark freezer and defrosted before use

### 3. Supplies 

- Two Hobo TidbiT [temperature loggers](https://www.onsetcomp.com/products/data-loggers/mx2203)
	- One placed in incubator and one placed on counter at room temperature 
- Plastic cups with lids (25 mL volume)
- Paper towels and bench paper/pads 
- Tweezers, transfer pipettes, and forceps 
- Dissecting microscope 
- Spectrophotometer plate reader (Agilent Synergy HTX) and Gen5 software 
- Filter sets for fluorescence measurements of 528/20 or 530/20 excitation and 590/20 emission 
- 25 mL glass seriological pipette and automated dispenser 
- Scale bar/ruler
- 96 well plate 
- Temperature probe 
- Color scale

For this experiment, we pre-labeled cups with the family ID ("A", "B", "C", and so on) and a serial number. For example, "A1", "A2", and so on. 

We will be conducting measurements on n=5-8 oysters per family per batch. We have five families, which will total to 25-40 oysters measured per batch. Five blanks will be run as well. 

Measurements will be repeated over 5-8 batches to obtain final replication of n=40-50 per family. 

# Protocol

On each day, we will measure metabolic response of oysters first at ambient temperature (3 h; incubator at 18°C) and then at high temperature (3 h; incubator set at 44°C to reach water temperatures of 41-42°C).  

We will measure metabolic rate every hour and assess survival at the end of the trial.  

## Schedule 

Each day, the schedule will be as follows. 

08:00-09:00: Prepare solutions, load oysters in cups, take size images, set incubator to 18°C    
09:00: Ambient Time 0 measurement   
10:00: Ambient Time 1 measurement   
11:00: Ambient Time 2 measurement  
12:00: Ambient Time 3 measurement   
12:00: Start high temperature exposure, set incubator to 42°C  
13:00: High Time 4 measurement   
14:00: High Time 5 measurement  
15:00: High Time 6 measurement  
16:00: High Time 7 measurement (preferred, but can be removed if time is limiting)    

## Load and prepare samples  

Before starting, set the incubator at 18°C. 

1. Prepare seed for assays by getting some seed from each family bag in the FTR tank room. We will need n=5-8 seed per family within the 15-20 mm size range. 
2. Place the seed next to their empty cups with their specified ID such that the oyster and the ID can be seen next to each other.
3. Take an image with a scale bar/ruler included that includes all of the oysters next to their ID cups. We will use the images to measure size for size normalization.  
4. Prepare n=5 cups for blank samples ("blank 1", "blank 2", and so on). 
5. Write the sample IDs in the notebook. 
6. Move the oysters into their respective cups. 
7. Fill each cup with 10 mL of resazurin working stock at ambient temperature using a seriological pipette. Rinse the pipette with DI water after this step. 
8. Move the oysters to the incubator set at 18°C and stack in random order. Proceed with the first measurement (next step below). 
9. Ensure a temperature logger is in the incubator. 
10. Record the time incubation is started and the time each measurement was taken. 

## Measurements at 18°C (T0-T3) 

Measurements at each time point will include fluorescence, absorbance, and color images. 

### Fluorescence 

1. Turn on the computer and plate reader. Open the plate reader software. 
2. Select the `oyster_resazurin_fluorescence` protocol. 
3. Take a sample of 250-280 µL out of each cup using a transfer pipette and add to a 96 well plate. Rinse the pipette with DI water between samples. Put sample A1 in well A1 and so on. Add blanks to row H (e.g., blank 1 in H1, blank 2 in H2, and so on). 
4. Put the oysters back in the incubator after taking a sample. Put them back in a random order to rearrange their position. 
5. Take the lid off of the plate and load the plate with well A1 aligned with "A1" on the instrument plate loader. 
6. Uncheck "use lid" and click "OK". 
7. Click "Read new" (green arrow). 
8. Save the experiment for the whole day as `YYYYMMDD_usda_fluorescence` in the `Desktop > resazurin_oyster` folder. The experiment does not need to be saved at each time point. The experiment file contains the protocol used for the entire day. Only the individual plate readings need to be exported at each time point (see below).  
9. Once the plate is read, click "Export" > "Yes" > "File Export" > "Plate" > "Automatic" > "OK". *Note:* sometimes the plate reader will try to export to an Excel file on the first save. If this happens, click the "modify export" icon and delete the PowerExport option. Then repeat the Export process in this step. 
10. Save the text file as `YYYYMMDD_usda_fluorescence_T#`. For example, `20250506_usda_fluorescence_T0`. Save to the same folder `Desktop > resazurin_oyster`. 
11. Record the time, sample information, plate information, and any relevant notes in the notebook. 

### Absorbance 

1. Keep the plate in the plate reader.
2. Click the "New Task" icon. 
3. Select the `oyster_resazurin_absorbance` protocol. 
4. Click "Read new". 
5. Uncheck "use lid" and click "OK". 
6. Save the experiment for the whole day as `YYYYMMDD_usda_absorbance` in the `Desktop > resazurin_oyster` folder. The experiment does not need to be saved at each time point. The experiment file contains the protocol used for the entire day. Only the individual plate readings need to be exported at each time point (see below). 
7. Once the plate is read, click "Export" > "Yes" > "File Export" > "Plate" > "Automatic" > "OK". *Note:* sometimes the plate reader will try to export to an Excel file on the first save. If this happens, click the "modify export" icon and delete the PowerExport option. Then repeat the Export process in this step. 
8. Save the text file as `YYYYMMDD_usda_absorbance_T#`. For example, `20250506_usda_absorbance_T0`. Save to the same folder `Desktop > resazurin_oyster`. 
9. Copy all text files for fluorescence and absorbance onto a zip drive.  
10. Record the time, sample information, plate information, and any relevant notes in the notebook. 

### Color images 

1. Take an image of the plate on a white piece of paper with even lighting. Turn the lights off or move to a location where light is even on the plate. Place the color standard ruler next to the plate. Add a note to the image that includes the date and time point. 
2. Rinse the plate well and set aside until the next measurement. 

Repeat these steps at 1 h (T1), 2 h (T2), and 3 h (T3) of incubation at 18°C.  

Record all times, notes, and sample information in the notebook. 

## Measurements at 42°C (T0-T3) 

After samples are taken at the 3 h (T3) time point, increase the incubator temperature to 44°C. This will increase water temperature to 41-42°C during the incubation.    

Repeat the steps above for fluorescence, absorbance, and color measurements at 4 h (T4), 5 h (T5), 6 h (T6), and 7 h (T7) of incubation at high temperature. If time is short for the day, the 7 h measurement can be skipped. Record the 7 h time point if possible.       

At the end of the day, save all of the data to a flash drive and add the text files to GitHub at the [location here under the Data directory](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families). 

## Final mortality measurements 

1. At the end of the incubation, following the T7 measurements, empty the resazurin from each cup into a waste collection container. 
2. Look at each oyster to see if the shell is open. If the shell is open and does not close when you probe the shell, the oyster is dead. 
3. Record whether each oyster is dead or alive in the notebook and record the oyster ID with each survival assessment. 
4. Enter survival data at the GitHub [location here under the Data directory](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families). Include a column for date, sample ID, and mortality. If the oyster is alive, enter mortality as a `0` and if the oyster is dead, enter mortality as a `1`.  

## Clean up

- Place oysters in a large tripour or cup and then put them in the compost. 
- Rinse all cups, pipettes, and plates thoroughly. 
- Save all data to the zip drive and upload to GitHub [here under the Data directory](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families) 
- Ensure all data is recorded in the notebook including time of trials, survival, and any relevant notes. 
- Take images of the notebook and post in a notebook post 

## Assemble data 

- Survival: A data sheet with columns for date, oyster ID, family ID, mortality (0 = alive; 1 = dead) and relevant notes 
- Sample Metadata: A data sheet with columns for date, oyster ID, family ID, and relevant notes
- Temperature Metadata: A data sheet with columns for date, timepoint, temperature, and relevant notes
- Resazurin fluorescence: All data files for fluorescence measurements during the trial (T0-T7 "fluorescence .txt" files). 
- Resazurin absorbance: All data files for absorbance measurements during the trial (T0-T7 "absorbance .txt" files). 
- Photos for color analysis: All images for color analysis. 
- Photos for size analysis: All images for size analysis.
- Size: Size data for each oyster (see below).   

Upload data to the [GitHub repository](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families).  

## Analyze oyster size on ImageJ 

Analyze oyster size on ImageJ using the following protocol.   

1. Download ImageJ. 
2. Open the size photograph for the day from the [GitHub repository](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families).  
3. Set the scale to 1 cm (10 mm) on the ruler standard in the image. Use mm units. 
4. Open the data spreadsheet. This will have a column for date, sample, length, and area. 
5. Use the line measurement tool to measure the longest axis length for each oyster. Enter the data as length in mm. 
6. Use the freehand tool to outline the planar area of each oyster. Enter the data as area in mm^2.  
7. Complete for each oyster. 
8. Upload data to the [GitHub repository](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families). 

# Notes 

Protocol is in progress and will be revised as testing is conducted.  

