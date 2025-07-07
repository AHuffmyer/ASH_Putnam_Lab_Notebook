---
layout: post
title: VIMS Resazurin Trials Day 4
date: '2025-07-03'
categories: VIMS_Oysters
tags: Resazurin Oyster 
---

This post details resazurin trials at VIMS from Day 4. Today we ran full family trials with temperature exposure in the same way we ran trials the last two days. 

# Overview 

We are visiting VIMS this week to conduct resazurin trials of *C. virginica* oyster families that are going to be outplanted into the field. We are going to sample oysters from 50 families with half of them representing families selected for low salinity performance and the other half selected for high salinity performance.   

There are predicted performance values and traits we can use to correlate to the resazurin data and once they are deployed in the field, we can use the observed performance data as well. This will allow us to test whether metabolic rate corresponds to performance.   

*Scroll to the bottom for take home points!*  

## Protocol 

See my [notebook post from yesterday](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/VIMS-Resazurin-Trials-Day-3/) for the procedure that we followed today and yesterday's results. This post provides an update on activities today and new results! I also provide a summary of some additional analyses using the full dataset.   

Today we following this procedure:  

- Allocate oysters to designated 24-well plates based on our [plate maps](https://github.com/RobertsLab/vims-resazurin/tree/main/output/plate-maps). We loaded 22 total plates with a total of 440 oysters! 
- Take an image for [size analysis](https://github.com/RobertsLab/vims-resazurin/tree/main/data/size). Analyze size using ImageJ. 
- Complete metadata [for today's trials](https://github.com/RobertsLab/vims-resazurin/blob/main/data/trial_metadata.xlsx).  
- Add chilled resazurin solution to each well at a volume of 2.5 mL. 
- Take initial T0 measurements.
- Add plates to the incubator at 40°C. Start the timer for exposures. 
- Stagger plates to read n=4 plates every 10 minutes for continuous plate reading throughout the day. 
- Take hourly measurements at T1 and T2. 
- After T2, put plates into the fridge at 4°C to start chilling the plates. Plates were added to the fridge at 2.5 hours of exposure. 
- Take measurements at T3 and T4. 
- Following T4, remove oysters from the plates and clean. 
- After each timepoint, we uploaded the [plate files here](https://github.com/RobertsLab/vims-resazurin/tree/main/data/plate-files/20250703).
- Throughout, record temperature in the wells using infrared thermometer.  

We were able to stay at VIMS for the morning before we had to leave for our flight. The VIMS crew continued measurements after we left.  

## Observations from today 

Here are some of our observations and troubleshooting during the trials today.  

- The resazurin solution started out at a similar temperature today as it did yesterday. This is because the plates were filled last night and then kept in the fridge all night. Fluorescence values were similar to yesterday. 
- We still saw the gradient in temperature within plates like we did yesterday. Future trials should include rotation of the plates. We had a replicate of every family in each row of the plate, so we can account for this statistically.  
- Temperatures followed the same pattern today as they did yesterday, with the exception of starting colder today.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/temps.png?raw=true)

- There were some oysters that were quite small and others that were quite large. I recommend using the mid range of sizes for future trials (>10mm and less than 18mm) for 24-well plates. Here is the size distrution.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/size.png?raw=true)

## Critical considerations for grower recommendations and implementation of the resazurin assay 

This project is an exciting first view into the potential for the resazurin metabolic assay to predict/relate to field performance. Previously, we have been able to relate metabolic response to temperature to predict mortality under very short term (<4 h) conditions. In order for us to implement this assay and make recommendations for growers, it is critical that we collect more data that directly relates metabolic response in an assay to field performance.  

**Currently, we do not yet have the data we need to make recommendations to growers on which stocks may be more tolerant or have higher performance based on the response to the resazurin assay.** This is because there is a lot of context required to understand whether or not a higher or lower metabolic rate is advantageous or signals differential performance. To be able to make these conclusions, we must continue collecting data in projects like this effort at VIMS that will allow us to directly relate metabolic response to field performance.  

Whether or not a higher metabolic response is indicative of higher or lower performance/tolerance is going to depend heavily on the environmental history of the stocks tested, the stress profile used during the test and how this stress relates to environmental history, and the lifestage of stocks tested. We currently do not have enough information or data to make overarching statements or make general expectations for the directionality of metabolic response and anticipated performance.  

This project is an exciting step that will bring us closer to understanding how the resazurin assay may inform grower stock selection or hardening efforts. **Continued data collection is absolutely essential before we can make recommendations to growers.**  

## Results from today 

I used the same approach as my previous resazurin analyses to produce change in size-normalized fluorescence over time for each oyster. Here are our preliminary results using all data collected this week! 

- Metabolic rate was significantly different between families. There is a high degree of variability between families. I have shown these data as metabolic rates over time as well as ridge plots showing the distrubition.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/family_model_predictions.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/family_model_predictions_panel.png?raw=true)

```
model<-full_data%>%
  lmer((value)^(1/3) ~ family * timepoint + (1|well:plate:date) + (1|plate) + (1|date), data=.)
  
Type III Analysis of Variance Table with Satterthwaite's method
                  Sum Sq Mean Sq NumDF  DenDF    F value    Pr(>F)    
family              7.88    0.16    49 1141.8     4.6744 < 2.2e-16 ***
timepoint        1801.71  450.43     4 4788.8 13088.2823 < 2.2e-16 ***
family:timepoint   30.74    0.16   196 4788.8     4.5567 < 2.2e-16 ***
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/ridge_family.png?raw=true)

- Metabolic responses for each family were fairly consistent between all days of measurements.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/family_trajectories_date.png?raw=true)

- Metabolic rates also differed by phenotype. Families bred for survival in low salinity environments had higher metabolic rates. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/phenotype_model_predictions.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/ridge_phenotype.png?raw=true)

```
model<-full_data%>%
  lmer((value)^(1/3) ~ timepoint * phenotype + (1|well:plate:date) + (1|plate) + (1|date) + (1|family), data=.)
  
Type III Analysis of Variance Table with Satterthwaite's method
                     Sum Sq Mean Sq NumDF  DenDF    F value    Pr(>F)    
timepoint           1805.55  451.39     4 4980.6 11567.7357 < 2.2e-16 ***
phenotype              0.10    0.10     1   47.1     2.6053    0.1132    
timepoint:phenotype    1.29    0.32     4 4980.7     8.2364 1.286e-06 ***
  
```

- I also calculated metabolic activity as total metabolic activity over the entire trial. This was calculated as Area Under the Curve (AUC). Total AUC shows significant varition between families as well as between phenotypes. This metric will be used to conduct correlations with predicted performance metrics below.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/auc_family.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/auc_phenotype.png?raw=true)

- I used unsupervised clustering analyses to identify cluster of oysters that displayed similar metabolic responses. Clustering analysis identified three groups that are expected: low metabolic activity, medium metabolic activity, and high metabolic activity. The first plot shows these metabolic patterns. The second plot shows the proportion of oysters in each family that were assigned to each cluster. Some families show a majority of oysters in one cluster, but most families have a mixture. This shows high variability in metabolism within family. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/cluster_fluorescence_patterns.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/cluster_assignments_family.png?raw=true)

- To quantify variability within and between families, I first calculated repeatability (i.e., intra-class correlations). If the value is close to 0, most variation is within families with very little family structure. This would indicate that metabolic response is more influenced by individual response or the environment than family. As the value approaches 1, more variability is attributed to family and would suggest oysters from the same family exhibit the same response. We found that the value is 0.111, indicating most variation is within families with little family structure. 

```
repeat_model <- rpt(value ~ timepoint + (1|family), grname = "family", data = full_data, datatype = "Gaussian")

summary(repeat_model)

R = 0.111

```

- In a similar approach, I calculated heritability estimates. As in the repeatability analysis, a low H2 value indicates low heritability and that variation is largely within family rather than between. Values >0.5 indicate stronger heritability of the response. We found that the H2 value was 0.0056, indicating low family structure and high variability in metabolism within family. 

```
heritability <- (MS_family - MS_resid) / (MS_family + (k - 1) * MS_resid)  # k = number of replicates
heritability

H2 = 0.0056

```

## Correlation with predicted traits 

We obtained data from Jess on the predicted trait values for each family based on their breeding program calculations. Here, I conducted correlations to the following traits: 

- Predicted survival in a high salinity environment
- Predicted survival in a low salinity environment
- Predicted weight in a high salinity environment 
- Predicted weight in a low salinity environment 

From our discussions with the VIMS team, we would expect higher disease prevalence in high salinity environments. Lower salinity environments are considered psu <15 where as high salinity is considered psu >18. Theses designations of "low" and "high" are relative based on the environments that are being selected. We can discuss the implications of these environments with the VIMS team to learn more about how metabolism might relate to these environments.  

Note that these are predicted performance values, not actual measured values. We will learn more about actual performance over the next year!  

### Correlation of AUC (total metabolic activity) with traits

I used the area under the curve (total metabolic activity) to correlate to the four traits listed above. For all of these analyses, I conducted correlations for each phenotype group. In these plots, you will see the metabolic activity on the X axis and the predicted performance metric on the Y axis. The plots are faceted by each breeding group/phenotype  - either those selected for high salinity or low salinity environments. If there is a positive relationship, that indicates that families with higher metabolic rate have higher predicted performance. If there is a negative relationship, it means that families with lower metabolic rate have higher predicted performance.  

Data were calculated as an average for each family. Correlations were tested with Pearson correlations.   

- Metabolic activity ~ survival in high salinity: **No relationship**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/auc_survival-high-salinity.png?raw=true)

- Metabolic activity ~ survival in low salinity: **There is a positive relationship between metabolic activity and predicted survival in low salinity environments but ONLY for the low salinity selected families.** There is a trend for a negative relationship in the high salinity selected families, but this is not significant.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/auc_survival-low-salinity.png?raw=true)

- Metabolic activity ~ weight in high salinity: **No relationship**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/auc_weight-high-salinity.png?raw=true)

- Metabolic activity ~ weight in low salinity: **No relationship**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/auc_weight-low-salinity.png?raw=true)

### Correlation of metabolic rate under the first hour of stress with traits

I next used the same approach to conduct correlations with predicted traits but with another metric of metabolism - metabolic rate under the first hour of the incubation. This was the period of time in which the oysters were first exposed to temperature stress and when temperatures were elevated from ~ 10°C to ~ 35°C within 1 hour. The purpose of this is to test whether stress response is indicative of predicted performance.  

Data were calculated and analysed as I described above. 

- Metabolic activity under stress ~ survival in high salinity: **No relationship**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/stress-rate_survival-high-salinity.png?raw=true)

- Metabolic activity under stress ~ survival in low salinity: **There is a positive relationship between metabolic activity under stress and predicted survival in low salinity environments but ONLY for the low salinity selected families.**   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/stress-rate_survival-low-salinity.png?raw=true)

- Metabolic activity under stress ~ weight in high salinity: **No relationship**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/stress-rate_weight-high-salinity.png?raw=true)

- Metabolic activity under stress ~ weight in low salinity: **No relationship**  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/vims/20250703/stress-rate_weight-low-salinity.png?raw=true)

# Take home points 

- It is critical that we consider environmental context and gather more data to be able to make recommendations to growers using this assay. We do not yet have enough information to be able to relate higher or lower metabolic activity with expected performance on a broad scale. These results will need to be highly contextualized by the specific population/family/species/environment/stress.  

- We characterized differences in metabolic responses between families and saw differences in metabolic responses between phenotypes (i.e., those selected for either high or low salinity environments). Families selected for low salinity environments had higher metabolic rates.  

- Families selected for low salinity with higher metabolic rates had higher predicted survival in low salinity environments.  

- Next steps include relating metabolic rates to actual performance data collected in the field (fall 2026).  