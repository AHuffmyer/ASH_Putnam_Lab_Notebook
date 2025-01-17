---
layout: post
title: Resazurin Metabolic Assays Protocol for PolyIC Seed Testing
date: '2025-01-16'
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

This recipe is to make 150 mL of working stock. To run a single 96 well plate, approximately 35 mL will be required (allowing for extra for re-do's or errors). For example, if we are running 4 total plates, we will need to prepare 4 x 35 mL = 150 mL of working solution. 

- 148 mL seawater (DI water with Instant Ocean adjusted to 23-25 ppt) 
- 333 µL resazurin stock solution
- 150 µL DMSO
- 1.5 mL antibiotic solution [100x Penn/Strep & 100x Fungizone](https://us.vwr.com/store/product/4648458/null) - this should be frozen in a dark freezer 

Store at 4°C in dark fridge.  

### 3. Seawater 

Prepare clean seawater solution using DI water and set on the bench at the control temperature. This will be used for survival incubations at the end of the day after resazurin measurements. 

### 4. Supplies 

- Two Hobo TidbiT [temperature loggers](https://www.onsetcomp.com/products/data-loggers/mx2203)
	- One placed in incubator and one placed on counter at room temperature 
- 96 well culture plates 
- Paper towels and bench paper/pads 
- Tweezers, transfer pipettes, and forceps 
- Dissecting microscope 
- Spectrophotometer plate reader(Perkin Elmer 1420 Multilabel Counter Victor3) and software version 3.00  
- P1000 pipette
- Scale bar/ruler
- Scale (mg measurements)  

Label plates with identifying number (e.g. "High Plate 1", "Control Plate 2").  

# Protocol

## Resazurin measurements 

1. Prepare spat for assays by getting some spat from each silo in the FTR tank room. Label a cup with the appropriate label (control-1, control-2, treated-4, or treated-5). These are from duplicate tanks. We will want to keep these separate and track the tanks they came from during the measurements. 
2. Add 40 spat into a labeled 96 well plate (with a plate number and "resazurin") from the same treatment group from each replicate. For example, in one plate add 40 wells of control-1 spat and 40 wells of control -2 spat. Repeat to generate 1 plates per treatment group per temperature (control or high) for a total of 4 96 well plates. Leave at least 10 random wells open for blank measurements. 
	- Here is how the plates can be structured: 
		- *Control Plate 1*: 40 wells of control-1 seed and 40 wells of control-2 seed; 10 blank wells. Set at control temperature. 
		- *Control Plate 2*: 40 wells of treated-1 seed and 40 wells of treated-2 seed; 10 blank wells. Set at control temperature. 
		- *High Plate 1*: 40 wells of control-1 seed and 40 wells of control-2 seed; 10 blank wells. Set at high temperature. 
		- *High Plate 2*: 40 wells of treated-1 seed and 40 wells of treated-2 seed; 10 blank wells. Set at high temperature. 
3. Prepare the scale. Plate the 96 well plate on the scale and zero the meaurement. 
4. Place seed individually on the 96 well plate after dabbing on a towel to remove excess water, recording the weight of each spat as you load into the plate (in mg) in the notebook. Record the plate number and the well number with the weight. Zero the scale after each spat is added and repeat until all spat are loaded. Load without water. 
2. Record plate number and position of samples in each plate. Assign plate number as `YYYYMMDD_TemperatureTreatment_Plate#`. For example, `20250110_High_Plate1`. Label plates consecutively, such that there is never a repeat of "plate 1". Record which spat treatment group is in each plate and which wells contain oysters and which contain blanks. 
3. Fill each well with 300 µL of resazurin working stock at ambient temperature. 
4. Take a reading for each plate on the plate reader (initial time point). Save the file as `YYYYMMDD_TemperatureTreatment_Plate#_T0.xlsx`. 
5. Put the plates in respective temperature treatments. Put High plates at the 42°C incubation and Control plates at the countertop (~16°C). 
6. After 1, 2, 3, 4, and 5 hours, read the plates again on the plate reader. Record the name of data files as `YYYYMMDD_TemperatureTreatment_Plate#_Time1.xlsx` and so on. 
7. Save the data to a flash drive and add to GitHub at the [location here](https://github.com/RobertsLab/polyIC-larvae/tree/main/data/resazurin/plate-files/). 

## Survival measurements 

1. After the 5 hour incubation, prepare new labeled plates with the same plate identification numbers and a label with "survival". 
2. Move the samples in the identical positions from the resazurin plate to the survival plate. In the transfer, gently rinse each spat by dunking into a clean seawater from the respective temperature to remove excess resazurin solution. Do this in a petri dish or cup. 
3. While transfering, assess if the oyster is dead (gaping open with no movement when shell is probed) or alive (closed). Use a microscope if needed. It is clear to see when the oysters have died by probing the flat portion of the shell to see if it remains gaping after being touched. 
4. Record the plate, well number, and whether the oyster is dead or alive in the notebook or in a data sheet. You can do this by just making a list of the wells with either dead or alive spat. 
5. Fill the wells of the survival plate with 300 µL seawater from the benchtop temperature. 
6. Place all plates in the cold room on the table over night. 
7. The next day (~24 h since start of resazurin measurements), record survival again as in step 4 above.
8. All oysters will be discarded after this time point.  

# Data sheets 

Ariana will add links to data sheets here after initial trials. 

# Notes 

Protocol is in progress and will be revised as testing is conducted.  

