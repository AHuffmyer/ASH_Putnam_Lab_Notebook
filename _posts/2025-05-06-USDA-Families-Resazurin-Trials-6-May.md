---
layout: post
title: USDA Families Resazurin Trials 6 May 2025
date: '2025-05-09'
categories: RobertsLab_Oysters USDA_Families
tags: Resazurin Oyster Cgigas
---

This post details activities of a resazurin trial from 6 May 2025 and preliminary data analysis. In this trial we are comparing metabolic rates of oysters as they go from control to high temperature. 

# Overview 

This trial was conducted based on the protocol available in [my lab notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-Individual-Stress-Testing/) as part of our effort to develop a resazurin based metabolic assay tool for oyster aquaculture. The [GitHub repo for this project is here](https://github.com/RobertsLab/resazurin-assay-development).  

# Trial

Dash and Carolyn conducted the protocol today. You can see their notes from today in their notebooks posts [here](https://genefish.wordpress.com/2025/05/07/resazurin-trials-plate-reading-reference-photos-05-06/) and [here](https://genefish.wordpress.com/2025/05/06/practice-resazurin-test-plate-reader-protocol/).  

In brief, we did the following:  

- Analyzed metabolism via resazurin in n=5 oysters from each of 5 families from the 2024 USDA POGS general excess cohort. 
- Conducted measurements in both fluorescence and absorbance mode on the new plate reader. 
- Measured resazurin hourly for 3 hours at control temperature (18C) and three hours at high temperature (42C). 
- Analyzed oyster size as both length and planar area. 
- Collected images for preliminary color analysis at a later date.  

Trials will continue over the next several weeks.  

In my preliminary data analysis, I will test the following:  

- Does metabolic rate change between control and high temperature? 
- Is there variation between families in metabolic rates? 
- Is absorbance or fluorescence more appropriate for analysis? 

Data from today were uploaded to [GitHub here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/usda-families).  

# Data analysis 

The script for this analysis [is on GitHub here]() and will be updated with each round of analysis and trials.  

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

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/fl_plot1.png?raw=true)

- This figure shows resazurin values (size normalized change in fluorescence) over the entire trial. The dotted line indicates the time at which temperature was increased (18C = blue; 42C = red). Each line represents an individual oyster in each of the 5 families (facet).  

- There appears to be a change in the slope of the line after the temperature was increased. This is what we would expect given that previous trials shows oysters enter metabolic depression at 40-42°C. 

- It is also interesting to note that There appears to be changes in the magnitude of fluorescence change between families. Variance is low, especially for a small sample size in this initial trial.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/fl_plot2.png?raw=true)

- I then generated a linear model for each individual oyster during the control and high temperature phases. This plot shows the slope of change in fluorescence (i.e., metabolic rate) over the time of incubation. Note we ran out of time to do the final high temperature measurement, but that will be included in future trials. 

- For most of the oysters, metabolic rates decrease under high temperature. There are a few, however, like B1, A5, and D5, that do not appear to change. Based on our [previous experiments](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-data-analysis-and-size-normalization-for-10K-seed-project/), we would expect these to be more sensitive to stress. Capacity for metabolic depression appears to be positively correlated with survival in our previous tests.  

- Some oysters appear to decrease metabolic rates more than others at high temperature (e.g., A1, C1).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/fl_plot3.png?raw=true)

- Next, I examined whether the change in metabolic rate (slope) was different between temperatures and the five families. This plot is showing you the metabolic rate (slope) for each oyster at control (18°C) and high (42°C) temperatures. Each panel and color indicates a family. The lines are connecting the slopes for each individual oyster between the temperatures. A line that decreases from 18-42°C indicates that the oyster reduced metabolic rates under thermal stress. 

- Each family shows a majority of oysters exhibiting metabolic depression (reduced metabolic rates) at high temperature. I ran a statistical test to test the effects of temperature and family on metabolic rates.  

```
model1<-slopes%>%
  lm(slope ~ family * temperature, data=.)

summary(model1)
anova(model1)
qqPlot(model1$residuals)

emm<-emmeans(model1, ~temperature|family)
pairs(emm)

```

- The results show there was a significant effect of family and temperature on metabolic rates. Interestingly, a couple families had significant differences in metabolic rate between temperatures (family A and D), but the others did not.  

- Families A and D did show the steepest slope changes in the figure above.   

```
Analysis of Variance Table

Response: slope
                   Df     Sum Sq    Mean Sq F value    Pr(>F)    
family              4 0.00042503 0.00010626  3.6282 0.0129498 *  
temperature         1 0.00051564 0.00051564 17.6064 0.0001467 ***
family:temperature  4 0.00006528 0.00001632  0.5572 0.6949292    
Residuals          40 0.00117147 0.00002929

family = A:
 contrast  estimate      SE df t.ratio p.value
 18C - 42C  0.00768 0.00342 40   2.243  0.0305

family = B:
 contrast  estimate      SE df t.ratio p.value
 18C - 42C  0.00349 0.00342 40   1.019  0.3141

family = C:
 contrast  estimate      SE df t.ratio p.value
 18C - 42C  0.00506 0.00342 40   1.479  0.1469

family = D:
 contrast  estimate      SE df t.ratio p.value
 18C - 42C  0.01012 0.00342 40   2.956  0.0052

family = E:
 contrast  estimate      SE df t.ratio p.value
 18C - 42C  0.00577 0.00342 40   1.685  0.0999
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/fl_plot4.png?raw=true)

- Finally, I calculated the percent change in slopes between control and high temperatures to generate a potential "resilience index". A larger negative percent change would indicate higher survival likelihood based on our previous observations of higher survival in oysters with more metabolic depression.  

- This plot shows the percent change for each oyster in each family. A more negative percent change means there was greater metabolic depression at high temperature.  

- I then ran a test to examine variation in this index between families.  

```
model2<-slope_summary%>%
  lm(percent_change ~ family, data=.)

summary(model2)
anova(model2)
qqPlot(model2$residuals)

emm<-emmeans(model2, ~family)
pairs(emm)
```

- Results show no significant difference between families. We will revisit this analysis as this may be due to low sample size. As we gather more data we may see differences. 

```
Analysis of Variance Table

Response: percent_change
          Df Sum Sq Mean Sq F value Pr(>F)
family     4   4126  1031.4  0.5673 0.6892
Residuals 20  36360  1818.0    

 contrast estimate SE df t.ratio p.value
 A - B      -19.25 27 20  -0.714  0.9509
 A - C        6.42 27 20   0.238  0.9992
 A - D       17.98 27 20   0.667  0.9613
 A - E       -9.71 27 20  -0.360  0.9961
 B - C       25.67 27 20   0.952  0.8730
 B - D       37.23 27 20   1.381  0.6464
 B - E        9.54 27 20   0.354  0.9964
 C - D       11.56 27 20   0.429  0.9924
 C - E      -16.13 27 20  -0.598  0.9738
 D - E      -27.69 27 20  -1.027  0.8400
```

### Is absorbance or fluorescence more appropriate for analysis?

The short answer is that fluorescence appears to be more appropriate with higher resolution. We will further address this as we continue in our trials. Here are the results from the absorbance measurements.   

Note that for absorbance, we expect a decrease in values as metabolism progresses (turning the solution a lighter color). If there is an increase in absorbance, that means that metabolism is reduced.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/ab_plot1.png?raw=true)

- The data is definitely noiser than fluorescence, but we can see a trend for increased metabolic rate at control temperature (decreasing slope) and decreased metabolic rate at high temperature (increasing slope).   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/ab_plot2.png?raw=true)

- As we saw in the fluorescence data, there are different slopes between temperatures. At high temperature, metabolic rates are lower (higher values) and at control temperature, rates are higher (lower values).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/ab_plot3.png?raw=true)

- There is similar agreement between families and perhaps less variation within family as measured by absorbance. 

- All families show decreased metabolic rates (higher values) at high temperature.  

- I ran a statistical test to examine effects of temperature and family on absorbance measurements.  

```
Analysis of Variance Table

Response: slope
                   Df     Sum Sq    Mean Sq F value    Pr(>F)    
family              4 6.1060e-09 1.5270e-09  0.4660    0.7602    
temperature         1 2.2076e-07 2.2076e-07 67.3893 4.125e-10 ***
family:temperature  4 2.1990e-08 5.4980e-09  1.6782    0.1740    
Residuals          40 1.3103e-07 3.2760e-09       
```

- The results show an effect of temperature, but not family. The fluorescence values did show a significant effect of family, which is likely due to lower resolution of the absorbance method.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/usda/20250509/ab_plot4.png?raw=true)

- This plot looks similar to our fluorescence results, but with less variation between families.  

- As in the fluorescence data, there was no effect of family on percent change in metabolic rates. We will revisit this as we gather more data.  

```
Analysis of Variance Table

Response: percent_change
          Df     Sum Sq  Mean Sq F value Pr(>F)
family     4  271302372 67825593  0.9976 0.4319
Residuals 20 1359774471 67988724   
```

# Next steps

- Conduct additional trials to increase sample size 
- Run trials for at least 1 h longer 
- Analyze data after each trial and evaluate results 

