---
layout: post
title: USDA Families Resazurin Trials Complete Results
date: '2025-06-15'
categories: RobertsLab_Oysters USDA_Families
tags: Resazurin Oyster Cgigas
---

This post details the results from all completed resazurin trials testing individual oyster stress responses. In these trials we compared metabolic rates of oysters as they go from control to high temperature. 

This work was conducted by Carolyn and Dash in the lab. View their lab [notebooks here](https://genefish.wordpress.com/).  

# Overview 

These trials were conducted based on the protocol available in [my lab notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-Individual-Stress-Testing/) as part of our effort to develop a resazurin based metabolic assay tool for oyster aquaculture. The [GitHub repo for this project is here](https://github.com/RobertsLab/resazurin-assay-development).  

# Trial 

In brief, each trial included the following:  

- Analyzed metabolism via resazurin in n=5 oysters from each of 5 families from the 2024 USDA POGS general excess cohort. Total sample size was n=30 per family. 
- Conducted measurements in both fluorescence and absorbance mode on the new plate reader. 
- Measured resazurin hourly for 3 hours at control temperature (18C) and 3 hours at high temperature (42C). 
- Analyzed oyster size as both length and planar area. 
- Collected images for preliminary color analysis at a later date.     

In my data analysis, I will test the following:  

- Does metabolic rate change between control and high temperature? 
- Is there variation between families in metabolic rates? 
- Is absorbance or fluorescence more appropriate for analysis? 
- Is metabolic rate indicative of mortality? 

Data are uploaded to [GitHub here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families).  

# Data analysis 

The script for this analysis [is on GitHub here](https://github.com/RobertsLab/resazurin-assay-development/blob/main/scripts/usda_families/resazurin-analysis-families.Rmd).  

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

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/fl_family_trajectories.png?raw=true)

- This figure shows resazurin values (size normalized change in fluorescence) over the entire trial. The dotted line indicates the time at which temperature was increased (18C = blue; 42C = red). Each line represents an individual oyster in each of the 5 families (facet).  

- There appears to be a change in the slope of the line after the temperature was increased. 

- It is also interesting to note that there appears to be changes in the magnitude of fluorescence change between families.

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/fl_family_slopes.png?raw=true)

- Next, I examined whether the change in metabolic rate (slope) was different between temperatures and the five families. This plot is showing you the metabolic rate (slope) for each oyster at control (18°C) and high (42°C) temperatures. Each panel and color indicates a family. The lines are connecting the slopes for each individual oyster between the temperatures. A line that decreases from 18-42°C indicates that the oyster reduced metabolic rates under thermal stress. 

- Each family shows some oysters exhibiting metabolic depression (reduced metabolic rates) at high temperature. 

- I ran a statistical test to test the effects of temperature and family on metabolic rates. 

```
Type III Analysis of Variance Table with Satterthwaite's method
                                 Sum Sq Mean Sq NumDF  DenDF   F value
family                           0.0030  0.0008     4 499.65    0.2260
temperature                      0.0064  0.0064     1 940.74    1.9385
time_exposure                    8.5484  8.5484     1 941.33 2576.0791
family:temperature               0.0061  0.0015     4 941.22    0.4627
family:time_exposure             0.0632  0.0158     4 947.55    4.7580
temperature:time_exposure        0.0284  0.0284     1 941.25    8.5703
family:temperature:time_exposure 0.0097  0.0024     4 947.54    0.7272
                                    Pr(>F)    
family                           0.9238240    
temperature                      0.1641576    
time_exposure                    < 2.2e-16 ***
family:temperature               0.7631381    
family:time_exposure             0.0008361 ***
temperature:time_exposure        0.0034991 ** 
family:temperature:time_exposure 0.5734649  
```

- The results show there is a significant effect of family and temperature on metabolic rates (fluorescence value over time). There is no interaction with family x temperature x time, indicating that families did not differentially respond to temperature stress. 

```
 contrast estimate      SE  df t.ratio p.value
 A - B     0.00701 0.00691 132   1.014  0.8484
 A - C     0.02960 0.00689 135   4.293  0.0003
 A - D     0.03074 0.00688 138   4.467  0.0002
 A - E     0.01094 0.00686 136   1.595  0.5034
 B - C     0.02259 0.00700 134   3.227  0.0134
 B - D     0.02373 0.00699 137   3.394  0.0079
 B - E     0.00393 0.00697 135   0.564  0.9800
 C - D     0.00114 0.00697 140   0.163  0.9998
 C - E    -0.01866 0.00696 139  -2.683  0.0617
 D - E    -0.01980 0.00694 141  -2.852  0.0394
 ```

- Family A has higher metabolic rates than most other families and is more variable. Family B also had higher metabolic rates than families C and D. This indicates family variation in metabolism. 

```
 contrast  estimate     SE  df t.ratio p.value
 18C - 42C  -0.0213 0.0035 951  -6.082  <.0001
```

- Metabolic rates increase under high temperature, which is expected. The increase is small but significant.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/fl_family_percent_change.png?raw=true)

- Finally, I calculated the percent change in slopes between control and high temperatures to generate a potential "resilience index". A larger negative percent change would indicate higher survival likelihood based on our previous observations of higher survival in oysters with more metabolic depression.  

- This plot shows the percent change for each oyster in each family. A more negative percent change means there was greater metabolic depression at high temperature.  

- I then ran a test to examine variation in this index between families. There was no difference. 

- Matching our observations above, metabolic rates increased under high temperature (positive change in slope).   

### Is metabolic rate indicative of mortality? 

I then plotted dead oysters (red dots/lines) onto the plots of metabolic rate. We only had a few dead oysters, so this is preliminary evidence only.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/fl_family_slopes_mortality.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/fl_family_percent_change_mortality.png?raw=true)

- There is a trend that higher metabolic rates tend to be shown in oysters that die, but this is not a universal effect and is a small sample size because only a few oysters died.   

- It appears that Family A is more susceptible with more mortality than the other families. This family also has higher and more variable metabolic rates, which is consistent with our previous findings.   

### Is absorbance or fluorescence more appropriate for analysis?

The short answer is that fluorescence appears to be more sensitive with higher resolution. We will further address this as we continue in our trials. Here are the results from the absorbance measurements.   

Note that for absorbance, we expect a decrease in values as metabolism progresses (turning the solution a lighter color). If there is an increase in absorbance, that means that metabolism is reduced.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/ab_family_trajectories.png?raw=true)

- The data is definitely noiser than fluorescence.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/ab_family_slopes.png?raw=true)

- The results show no effects of family, unlike fluorescence data, demonstrating the absorbance results are less sensitive than fluorescence.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/ab_family_percent_change.png?raw=true)

- There is not a strong indication of mortality in the absorbance data either. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/ab_family_slopes_mortality.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/ab_family_percent_change_mortality.png?raw=true)

Finally, I correlated the absorbance and fluorescence values to examine the relationship between the two metrics.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250615/ab_fl_correlation.png?raw=true)

- As expected, the correlation is negative (due to the reciprocal relationship between the two metrics described above). However, the correlation is very noisy. 

- In particular, the absorbance data performs poorly when absorbance is low (near 0), but there is a wider range of values in the fluorescence data. This indicates that the absorbance data has poor performance detecting these small changes in absorbance when values are low.  

# Take Homes 

- There is variation in metabolic rates between families. 
- There is a detectable change in metabolism between control and high temperatures. 
- Fluorescence is more accurate and sensitive to measure changes in metabolism using resazurin.  

