---
layout: post
title: USDA Families Resazurin Trials 3 June 2025
date: '2025-06-03'
categories: RobertsLab_Oysters USDA_Families
tags: Resazurin Oyster Cgigas
---

This post details activities of a resazurin trial from 3 June 2025 and preliminary data analysis. In this trial we are comparing metabolic rates of oysters as they go from control to high temperature. 

This work was conducted by Carolyn and Dash in the lab. View their lab [notebooks here](https://genefish.wordpress.com/).  

# Overview 

This trial was conducted based on the protocol available in [my lab notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-Individual-Stress-Testing/) as part of our effort to develop a resazurin based metabolic assay tool for oyster aquaculture. The [GitHub repo for this project is here](https://github.com/RobertsLab/resazurin-assay-development).  

# Trial 

In brief, the trial included the following:  

- Analyzed metabolism via resazurin in n=5 oysters from each of 5 families from the 2024 USDA POGS general excess cohort. 
- Conducted measurements in both fluorescence and absorbance mode on the new plate reader. 
- Measured resazurin hourly for 3 hours at control temperature (18C) and 3 hours at high temperature (42C). 
- Analyzed oyster size as both length and planar area. 
- Collected images for preliminary color analysis at a later date.  

Trials will continue through next week (one more round).    

In my preliminary data analysis, I will test the following:  

- Does metabolic rate change between control and high temperature? 
- Is there variation between families in metabolic rates? 
- Is absorbance or fluorescence more appropriate for analysis? 
- Is metabolic rate indicative of mortality? 

Data from today were uploaded to [GitHub here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families).  

# Data analysis 

The script for this analysis [is on GitHub here](https://github.com/RobertsLab/resazurin-assay-development/blob/main/scripts/usda_families/resazurin-analysis-families.Rmd) and will be updated with each round of analysis and trials.  

### Data preparation and calculations 

I prepared the data using the following steps. These steps were repeated for both absorbance and fluorescence readings.    

- Read in data from 96 well plate format 
- Read in sample metadata 
- Normalize all signals to the initial time point signal
- Subtract blank signals from each oyster sample signal
- Size normalize signals based on planar area from each oyster 

This generates a metric of resazurin signal per unit size that is blank corrected.   

I then conducted analysis using the following steps. 

- Separate readings by temperature phase: 0-3 hours was conducted at control temperature; 3-6 hours was conducted at high temperature
- Normalize control readings to the 0 hour timepoint (first timepoint at high temperature)
- Normalize high readings to the 3 hour timepoint (first timepoint at high temperature)

This generates a metric of resazurin signal per unit size that is calculated for each temperature phase. 

A higher value for fluorescence indicates more metabolic activity, because the solution fluoresces more as the reaction progresses.   

In contrast, a lower value for absorbance indicates more metabolic activity, because the solution lightens in color as the reaction progresses.   

I then plotted the data and examined preliminary simple linear models to compare rates between temperature phases for each oyster.  

Finally, I plotted metabolic rate by mortality status at the end of the trial.  

I will summarize the results here that our relevant to our main questions.  

### Does metabolic rate change between control and high temperature? Is there variation between families in metabolic rates?  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/fl_family_trajectories.png?raw=true)

- This figure shows resazurin values (size normalized change in fluorescence) over the entire trial. The dotted line indicates the time at which temperature was increased (18C = blue; 42C = red). Each line represents an individual oyster in each of the 5 families (facet).  

- There appears to be a change in the slope of the line after the temperature was increased. 

- It is also interesting to note that there appears to be changes in the magnitude of fluorescence change between families.

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/fl_family_slopes.png?raw=true)

- Next, I examined whether the change in metabolic rate (slope) was different between temperatures and the five families. This plot is showing you the metabolic rate (slope) for each oyster at control (18°C) and high (42°C) temperatures. Each panel and color indicates a family. The lines are connecting the slopes for each individual oyster between the temperatures. A line that decreases from 18-42°C indicates that the oyster reduced metabolic rates under thermal stress. 

- Each family shows some oysters exhibiting metabolic depression (reduced metabolic rates) at high temperature. I ran a statistical test to test the effects of temperature and family on metabolic rates. 

- The results show there Family A has higher metabolic rates than all other families and is more variable. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/fl_family_percent_change.png?raw=true)

- Finally, I calculated the percent change in slopes between control and high temperatures to generate a potential "resilience index". A larger negative percent change would indicate higher survival likelihood based on our previous observations of higher survival in oysters with more metabolic depression.  

- This plot shows the percent change for each oyster in each family. A more negative percent change means there was greater metabolic depression at high temperature.  

- I then ran a test to examine variation in this index between families. There was no difference.  

### Is metabolic rate indicative of mortality? 

I then plotted dead oysters (red dots/lines) onto the plots of metabolic rate.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/fl_family_slopes_mortality.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250630/fl_family_percent_change_mortality.png?raw=true)

There is a trend that higher metabolic rates tend to be shown in oysters that die, but this is not a universal effect and is a small sample size because only a few oysters died.   

It appears that Family A is more susceptible with more mortality than the other families. This family also has higher and more variable metabolic rates, which is consistent with our previous findings.  

### Is absorbance or fluorescence more appropriate for analysis?

The short answer is that fluorescence appears to be more sensitive with higher resolution. We will further address this as we continue in our trials. Here are the results from the absorbance measurements.   

Note that for absorbance, we expect a decrease in values as metabolism progresses (turning the solution a lighter color). If there is an increase in absorbance, that means that metabolism is reduced.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/ab_family_trajectories.png?raw=true)

- The data is definitely noiser than fluorescence.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/ab_family_slopes.png?raw=true)

- The results show no effects of family, unlike fluorescence data, demonstrating the absorbance results are less sensitive than fluorescence.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/ab_family_percent_change.png?raw=true)

- There is not a strong indication of mortality in the absorbance data either. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/ab_family_slopes_mortality.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250603/ab_family_percent_change_mortality.png?raw=true)

# Next steps

- Conduct additional trials to increase sample size 
- Analyze data after each trial and evaluate results 

