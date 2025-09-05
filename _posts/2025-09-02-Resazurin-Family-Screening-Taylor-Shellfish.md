---
layout: post
title: Resazurin family screening with Taylor Shelfish
date: '2025-09-04'
categories: RobertsLab_Oysters 
tags: Resazurin Oyster Cgigas
---

We are conducting resazurin oyster family screenings with seed provided by Taylor Shellfish. 

# Overview 

We are applying our approach from our past [VIMS work](https://github.com/RobertsLab/vims-resazurin) to screen families of oysters using resazurin assays in collaboration with Taylor Shellfish. These families are going to be outplanted in the field, allowing us to perform correlations between resazurin response and future field performance. 

Check out our [oyster resazurin landing page](https://robertslab.github.io/resazurin-assay-development/) for more information on the protocol and approach used. 

All data from these trials is located in the [Resazurin Family Screening GitHub repo here](https://github.com/RobertsLab/resazurin-family-screenings).  

# Design and preparation 

We are going to conduct an experiment with teh following modifications from our [canonical protocol](https://robertslab.github.io/resazurin-assay-development/).  

- 38 families will be screened in 96-well plates
- We will allocate 1 row as blanks (n=12 per plate) and half of the remaining plate for one family with another family in the other half of the plate (n=42 per family per plate). 
- Incubations will be run for 2-4 h at 40°C with duration determined by live data analysis.
- Size images will be taken for each family with a mean size applied to each family due to uniformity in growth. 
- Plates will be read every hour. 
- Run all families (38 families) using 19 total plates repeated with two batches (n=2 plates per family, n=84 per family). Batches will either be conducted the same day or over two days depending on timing.  

We prepared the following for the assays: 

- Resazurin solution using the calculator on our [oyster resazurin landing page](https://robertslab.github.io/resazurin-assay-development/)
- Labeled all plates (n=17 plates)
- Prepared plate map datasheets
- Set up incubators and protocol on plate reader

Our resazurin recipe was:  

- 1443.48 mL filtered seawater 
- 3.25 mL resazurin stock
- 1.46 mL DMSO
- 14.63 mL antibiotic solution 

We will need to reorder antibiotic solution soon and filter more seawater.  

# Day 1: Running resazurin trials 

Today we ran full trials! 

### Loading seed 

We first loaded seed in 96-well plates with one row (H) as blanks (n=12 per plate) and the rest of the plate split in half to hold two families (columns 1-6 and columns 7-12 split by family). Families were represented in four plates with 38 total plates used today. This totals n=168 seed per family represented in the trials!  

We first loaded half of the plates (n=19) by placing individual seed in wells of the 96 well plates, randomly selecting which families were in each plate. This process took about 2 hours. Seed were held in 15 mL falcon tubes (without seawater) at room temperature prior to loading (overnight and transport to UW).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/pic1.jpeg?raw=true) 

Prior to loading, an image was taken of seed of each family. We will use these images to calculate an average size for each family for normalization. We also attempted photos for individual size normalization as described for Day 2 below.  

After plates were loaded, we added 200 µL of room temperature resazurin solution to all wells. This took about 30 minutes to complete.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/pic3.jpeg?raw=true) 

### Measurements 

Because the loading took some time, we noticed that metabolic activity started right away. By the time all plates were loaded at we took initial readings, metabolic activity was already under way and the fluorescence values were higher than the blanks.  

Therefore, I called the initial reading "T0.5" with subsequent readings as "T1.5" and "T2.5". In the analysis, I use the average values of the blanks from T0.5 to represent T0 for all wells in that plate.  

We read the plates using the standard plate reader protocol with groups of 5 plates staggered to be ready every 10 minutes, similar to our approach at VIMS.  

Plates were then loaded into an incubator set at 40°C until the next measurement.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/pic2.jpeg.png?raw=true) 

Measurements were taken at 1.5 and 2.5 hours. At this point, several of the wells were fully pink, indicating full conversion of the resazurin. Trials were stopped at this point. Samples above 22,000 raw fluorescence values will be oversaturated and will be removed from analysis.  

There were also several oysters that had values that stayed below 1,000. These will also be removed from analysis as these are oysters that were likely dead or out of the water.  

Here are the plates at the end of the trial.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/pic4.jpeg?raw=true) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/pic5.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/pic6.jpeg?raw=true)  

After the trials, we removed the resazurin and did a rinse with seawater. After the rinse, we filled the wells with seawater and placed all plates in the fridge over night. 

Throughout the trial, we measured temperature on a subset of plates using an infrared thermometer.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/temperature_plot_wells.png?raw=true) 

# Day 2: Running repeated trials, taking size images, and sampling for DNA/RNA 

Today we used the plates for repeat trials and sampling for RNA/DNA. Here are the things that we did:  

- Plates 1-19 (half of the plates) were run through the resazurin assay again using the same methods as yesterday. The oysters started cold, so the values were lower than yesterday. We will use this to examine whether response on day 1 correlates with response on day 2.  
- Plates 20-38 (the other half of the plates) were added to an incubator at 40°C for about 2 hours and then sampled for RNA/DNA by adding RNA/DNA sheild directly to the plate after rinsing with fresh water. These were then frozen at -80°C. 
- All plates were also imaged for individual size analysis. We tried doing this by taking a picture from the bottom of the plate. We need to make some sort of rig that makes this easier in the future. 

# Analysis 

I then analyzed the data [using the script here](https://github.com/RobertsLab/resazurin-family-screenings/blob/main/taylor-shellfish-sept2025/code/taylor-shellfish-sept2025.Rmd). 

**Note that the results below are NOT yet size normalized. Some families are bigger than others, so absolute differences in families will change after size normalization. This is just a sneak peak!** 

- There was a wide spread in performance across individual oysters.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/individual_trajectories.png?raw=true) 

- There was a significant differences in metabolism between families with a spread in higher vs lower performers (this may shift with size normalization).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/family_model_predictions.png?raw=true) 

- Total metabolic activity (again, this is not yet size normalized) varied between families with higher and lower performers.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/auc_family.png?raw=true) 

- I then compared the ranks (higher performer = larger numbers, lower performer = lower numbers) of each family between days. Metabolic activity on the first day of measurements was not predictive of family performance on the second day (no significant correlation).    

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/Rplot2.png?raw=true) 

- I then compared the ranks (higher performer = larger numbers, lower performer = lower numbers) of individual oysters on each day. There are a large group of individual oysters where performance on day 1 did correlate with day 2. However, there are oysters with high performance on day 1 and low performance on day 2 or visa versa.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250904/Rplot1.png?raw=true) 

# Next steps 

- Conduct size normalization for full analysis 