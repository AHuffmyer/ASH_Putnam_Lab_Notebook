---
layout: post
title: USDA Families Resazurin Trials 13 May 2025
date: '2025-05-13'
categories: RobertsLab_Oysters USDA_Families
tags: Resazurin Oyster Cgigas
---

This post details activities of a resazurin trial from 13 May 2025 and preliminary data analysis. In this trial we are comparing metabolic rates of oysters as they go from control to high temperature. 

This work was conducted by Carolyn and Dash in the lab. View their lab [notebooks here](https://genefish.wordpress.com/).  

# Overview 

This trial was conducted based on the protocol available in [my lab notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-Individual-Stress-Testing/) as part of our effort to develop a resazurin based metabolic assay tool for oyster aquaculture. The [GitHub repo for this project is here](https://github.com/RobertsLab/resazurin-assay-development).  

# Trial 

In brief, the trial included the following:  

- Analyzed metabolism via resazurin in n=5 oysters from each of 5 families from the 2024 USDA POGS general excess cohort. 
- Conducted measurements in both fluorescence and absorbance mode on the new plate reader. 
- Measured resazurin hourly for 3 hours at control temperature (18C) and 4 hours at high temperature (42C). 
- Analyzed oyster size as both length and planar area. 
- Collected images for preliminary color analysis at a later date.  

Trials will continue over the next several weeks.  

In my preliminary data analysis, I will test the following:  

- Does metabolic rate change between control and high temperature? 
- Is there variation between families in metabolic rates? 
- Is absorbance or fluorescence more appropriate for analysis? 

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

I will summarize the results here that our relevant to our main questions.  

### Does metabolic rate change between control and high temperature? Is there variation between families in metabolic rates?  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250513/fl_plot1.png?raw=true)

- This figure shows resazurin values (size normalized change in fluorescence) over the entire trial. The dotted line indicates the time at which temperature was increased (18C = blue; 42C = red). Each line represents an individual oyster in each of the 5 families (facet).  

- There appears to be a change in the slope of the line after the temperature was increased. This is what we would expect given that previous trials shows oysters enter metabolic depression at 40-42°C. 

- It is also interesting to note that there appears to be changes in the magnitude of fluorescence change between families.

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/fl_plot3.png?raw=true)

- Next, I examined whether the change in metabolic rate (slope) was different between temperatures and the five families. This plot is showing you the metabolic rate (slope) for each oyster at control (18°C) and high (42°C) temperatures. Each panel and color indicates a family. The lines are connecting the slopes for each individual oyster between the temperatures. A line that decreases from 18-42°C indicates that the oyster reduced metabolic rates under thermal stress. 

- Each family shows a majority of oysters exhibiting metabolic depression (reduced metabolic rates) at high temperature. I ran a statistical test to test the effects of temperature and family on metabolic rates. 

- For most of the oysters, metabolic rates decrease under high temperature. There are a few, however, lthat increase slightly. Based on our [previous experiments](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-data-analysis-and-size-normalization-for-10K-seed-project/), we would expect these to be more sensitive to stress. Capacity for metabolic depression appears to be positively correlated with survival in our previous tests.  

- The results show there was a significant effect of family and temperature on metabolic rates. Interestingly, a couple families had significant differences in metabolic rate between temperatures (family A and D), but the others did not.  

- Families A and D did show the steepest slope changes in the figure above.   


![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250513/fl_plot4.png?raw=true)

- Finally, I calculated the percent change in slopes between control and high temperatures to generate a potential "resilience index". A larger negative percent change would indicate higher survival likelihood based on our previous observations of higher survival in oysters with more metabolic depression.  

- This plot shows the percent change for each oyster in each family. A more negative percent change means there was greater metabolic depression at high temperature.  

- I then ran a test to examine variation in this index between families.  

- The two highest values on this plot were from oysters that did die by the end of the trial.  


### Is absorbance or fluorescence more appropriate for analysis?

The short answer is that fluorescence appears to be more appropriate with higher resolution. We will further address this as we continue in our trials. Here are the results from the absorbance measurements.   

Note that for absorbance, we expect a decrease in values as metabolism progresses (turning the solution a lighter color). If there is an increase in absorbance, that means that metabolism is reduced.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250513/ab_plot1.png?raw=true)

- The data is definitely noiser than fluorescence, but we can see a trend for increased metabolic rate at control temperature (decreasing slope) and decreased metabolic rate at high temperature (increasing slope).   

- As we saw in the fluorescence data, there are different slopes between temperatures. At high temperature, metabolic rates are lower (higher values) and at control temperature, rates are higher (lower values).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250513/ab_plot3.png?raw=true)

- There is similar agreement between families and perhaps less variation within family as measured by absorbance. 

- All families show decreased metabolic rates (higher values) at high temperature on average.  

- The results show an effect of temperature, but not family. The fluorescence values did show a significant effect of family, which is likely due to lower resolution of the absorbance method.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250513/ab_plot4.png?raw=true)

- This plot looks similar to our fluorescence results, but with less variation between families.  

- As in the fluorescence data, there was no effect of family on percent change in metabolic rates. We will revisit this as we gather more data.  

# Next steps

- Conduct additional trials to increase sample size 
- Analyze data after each trial and evaluate results 

