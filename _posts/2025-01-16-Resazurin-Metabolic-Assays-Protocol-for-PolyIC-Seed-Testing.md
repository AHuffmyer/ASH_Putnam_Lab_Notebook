---
layout: post
title: Resazurin Metabolic Assays Protocol for PolyIC Seed Testing
date: '2025-01-29'
categories: PolyIC_Larvae Protocol RobertsLab_Oysters
tags: Cgigas Oyster Protocol Resazurin Survival
---

This post details the working protocol for PolyIC Seed tests using the resazurin metabolic assay. 

## Overview 

This protocol is written for small oyster spat, but can be adapted for other life stages. This protocol is based on [previous work in the lab by Colby E.](https://genefish.wordpress.com/author/colbyelvrum/) and as used in our analysis of 10K Seed [metabolic response](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Initial-analysis-of-resazurin-metabolic-rates-in-10K-Seed-oysters/) and [survival tests](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/10K-seed-resazurin-survival-analysis/). 

We are using this protocol to test metabolic response of seed oysters to thermal stress that are from parents that were immune primed using [PolyIC exposure](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#polyic-larvae). 

We will expose "treated" and "control" spat to our thermal stress protocol and will measure metabolic response using resazurin and will track survival in a 96-well plate format.  

# Materials and preparing solutions

### 1. Stock resazurin solution 

To make the resazurin stock solution (10 mL) mix the following. We will use this solution for multiple trials.  

- [0.5 g resazurin salt](https://www.thermofisher.com/order/catalog/product/R12204)
- 10 mL DI water
- 10 µL DMSO

Store in a dark fridge or freezer.  

### 2. Working resazurin solution

To prepare the working solution of resazurin, prepare the following. 

This recipe is to make 150 mL of working stock. To run a single 96 well plate, approximately 35 mL will be required (allowing for extra for re-do's or errors). For example, if we are running 4 total plates, we will need to prepare 4 x 35 mL = 150 mL of working solution. Increase if more is required.  

- 148 mL seawater (DI water with Instant Ocean adjusted to 23-25 ppt) 
- 333 µL resazurin stock solution
- 150 µL DMSO
- 1.5 mL antibiotic solution [100x Penn/Strep & 100x Fungizone](https://us.vwr.com/store/product/4648458/null) - this should be frozen in a dark freezer 

Store at 4°C in dark fridge.  

### 3. Supplies 

- Two Hobo TidbiT [temperature loggers](https://www.onsetcomp.com/products/data-loggers/mx2203)
	- One placed in incubator and one placed on counter at room temperature 
- 96 well culture plates 
- Paper towels and bench paper/pads 
- Tweezers, transfer pipettes, and forceps 
- Dissecting microscope 
- Spectrophotometer plate reader (Perkin Elmer 1420 Multilabel Counter Victor3) and software version 3.00  
- P1000 pipette
- Scale bar/ruler
- Scale (mg measurements)  

Label plates with identifying number (e.g. "Plate 1", "Plate 2").  

# Protocol

We will be conducting measurements at several temperatures. Each day of measurements, we will run a control (benchtop, ~16°C) and an elevated temperature (25°C, 30°C, 35°C, 40°C).

## Schedule 

Each day, the schedule will be as follows. Note that this schedule is written for a single person. If there are 2-3 people, the time frame for loading and assessing survival will be shorter than written here. 

08:00-09:00: Load plates with oysters, take size images, and load resazurin solution   
09:00: Time 0 measurement   
10:00: Time 1 measurement   
11:00: Time 2 measurement  
12:00: Time 3 measurement  
13:00: Time 4 measurement  
14:00: Time 5 measurement  
14:00-16:00: Survival assessments and clean up   

## Load and prepare samples  

Before starting, set the incubator at the desired temperature.  

1. Prepare spat for assays by getting some spat from each silo in the FTR tank room. Label a cup with the appropriate label (control-1, control-2, treated-4, or treated-5). These are from duplicate tanks. We will want to keep these separate and track the tanks they came from during the measurements. 
2. Add spat into labeled 96 well plate (with a plate number and "resazurin") from each replicate tank placing them on the bottom of each well. For example, in one plate add control-1 and control-2 spat. Generate four plates as outlined below. We will leave 12 wells open for blank measurements in each plate.   
	- Here is how the plates will be structured each day: 
		- *Plate 1*: 16°C (control temperature) with control spat. Wells A1-D6 = control-1; wells D7-E6 = blanks; wells E7-H12 = control-2
		- *Plate 2*: 16°C (control temperature) with treated spat. Wells A1-D6 = treated-4; wells D7-E6 = blanks; wells E7-H12 = treated-5
		- *Plate 3*: Elevated temperaure with control spat. Wells A1-D6 = control-1; wells D7-E6 = blanks; wells E7-H12 = control-2 
		- *Plate 4*: Elevated temperature with treated spat. Wells A1-D6 = treated-4; wells D7-E6 = blanks; wells E7-H12 = treated-5
3. Write the location of wells on a plate map for each plate. 
4. Take images of each plate. This can be done by flipping the plate upside down gently with oysters stuck on the bottom of the well. If oysters stay on the bottom of the well, then an image can be taken from the bottom with a scale bar and label in the image. See an example below.  
5. If the oysters do not stick to the well, take several images across the plate from the top view to view each oyster in each well for size analysis. 
6. Fill each well with 300 µL of resazurin working stock at ambient temperature (in FTR 213 fridge). 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/size.jpg?raw=true)  

## Measurements 

1. Turn on the computer and plate reader. Open the plate reader software. 
2. Use the "Colby resazurin" protocol in the software to take a measurement of each plate at T0 before starting the incubation. 
3. Put the plate on the loading platform. 
4. Click Start
5. After the data is collected, press the spreadsheet icon. 
6. Click File > Export
7. Save the file as: `YYYYMMDD_TemperatureTreatment_Plate#_T0.xlsx`. For example, `20250128_40C_Plate#_T3.xlsx`. 
8. Save the data to a flash drive and add to GitHub at the [location here](https://github.com/RobertsLab/polyIC-larvae/tree/main/data/resazurin/plate-files/). 
9. Record the time of the measurement.
10. Move the elevated temperature plates to the incubator and record the incubator temperature. 
11. Repeat at 1, 2, 3, 4, and 5 hours of incubation.

Here is an example of the plate maps.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb3.jpeg?raw=true)

## Survival measurements 

1. After the 5 hour incubation, finish the last measurement and then bring the plates to FTR 213 for survival measurements. 
2. Prepare  96 well plate maps for survival data recording for each plate. 
3. In the plate maps, you will record which wells have dead oysters. Use an "X" to mark wells with dead oysters. 
4. Start with high temperature plates. 
4. Use tweezers to take each oyster out and examine in a petri dish filled with DI water under a dissecting scope. Determine if the oyster is dead by placing the cup side of the oyster down and gently taping/moving the shell. If the shell is open and remains open after tapping, the oyster is dead. If the shell is closed tight or closes after tapping, the oyster is alive. 
5. Record any notes of oysters that were damaged by the tweezers or if you noticed 2 oysters in a well. 
6. After examining each oyster, move it to a beaker. Discard oysters in the compost bin after the measurements are done. 
7. Repeat for all plates. 
8. Discard of resazurin in the appropriate waste bin. Empty plates into a tripour and pour into a labeled waste container. 
9. Rise plates thoroughly in the sink and allow to dry for the next day. 

Here is an example of the data sheet in the notebook.  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20250128/nb5.jpeg?raw=true)

# Data sheets 

Data will be stored on GitHub. Links are available below.  

[Size image folder](https://github.com/RobertsLab/polyIC-larvae/tree/main/data/resazurin/images/)  
[Size data sheet](https://github.com/RobertsLab/polyIC-larvae/blob/main/data/resazurin/size.csv)  
[Resazurin plate reader files](https://github.com/RobertsLab/polyIC-larvae/tree/main/data/resazurin/plate_files)  
[Resazurin plate metadata](https://github.com/RobertsLab/polyIC-larvae/blob/main/data/resazurin/metadata/metadata.xlsx)  
[Survival data](https://github.com/RobertsLab/polyIC-larvae/blob/main/data/resazurin/survival.csv)  

Scripts for analysis are available on GitHub [here](https://github.com/RobertsLab/polyIC-larvae/blob/main/scripts/resazurin/resazurin-analysis.Rmd) and figures of results are [available here](https://github.com/RobertsLab/polyIC-larvae/tree/main/figures/resazurin).    

# Notes 

Protocol is in progress and will be revised as testing is conducted.  

