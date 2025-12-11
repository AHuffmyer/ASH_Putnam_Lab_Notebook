---
layout: post
title: Westcott oyster survival and resazurin lab testing
date: '2025-12-11'
categories: RobertsLab_Oysters WSG_USDA
tags: Oyster Cgigas Survival R Resazurin
---

This post details the results of survival and resazurin lab testing on Westcott oysters. 

## Overview 

These oysters were exposed to either daily or weekly thermal hardening back in 2024 and were outplanted at Westcott for 18 months. This week, we brought back some of the outplants for lab testing.    

We conducted testing of the following: 

- Survival under acute stress 
- Resazurin metabolic response in gill tissue
- Sampling for glycogen (pending work by Sam, not described here)  

See [my previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Plan-for-Westcott-Oyster-Survival-Testing/) here for the experimental design of these tests.  

The GitHub repo containing the data and scripts used here can be found [at this link](https://github.com/RobertsLab/project-gigas-conditioning/tree/main).

Figures: 

- [Resazurin figures](https://github.com/RobertsLab/project-gigas-conditioning/tree/main/figures/resazurin)
- [Survival figures](https://github.com/RobertsLab/project-gigas-conditioning/tree/main/figures/survival/westcott_lab_survival)  

Code:  

- [Resazurin code](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/code/reazurin/westcott-resazurin.Rmd)
- [Survival code](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/code/lab_testing_westcott/westcott_lab_survival.Rmd)  

Groups tested: 

- Weekly thermal hardening experiment: control and treated
- Daily thermal hardening experiment: control and treated

Both of these groups were outplanted at Westcott. The daily experiment was conducted as +10°C daily for 2 months with oysters outplanted at 50 oysters per bag. The weekly experiment was conducted as +10°C weekly for 3 months with oysters outplanted at 100 oysters per bag. 

Oysters were brought back as subsets from field deployed oysters at Westcott on December 4th. We sampled an even number of oysters from each outplant bag (n=4-8 per bag depending on the experiment) and stored them at FTR until testing this week (December 9-11).  

## tl;dr

1. Survival under acute stress was higher in weekly thermal primed oysters 
2. Metabolic rates in gill tissue was higher in daily thermal primed oysters 

### Acute stress survival assays 

Refer to my post linked above for full notes on experimental design. Here is a brief summary.  

- n=10 oysters from each experimental group exposed to control temperature (22°C)
- n=20 oysters from each experimental group exposed to high temperature (33°C) in large incubator
- All oysters were in plastic cups wijth ~8 oz of seawater
- Control oysters were set on the table at room temperature and high oysters were exposed in a large standing incubator
- Oyster survival was recorded for every individual at the start of the experiment (9am Dec 9) and noon and 4pm each day following (Dec 9-10)
- The experiment ended during the morning of Dec 11 with only 5 reminaing oysters alive. 
- Data were analyzed using Kaplain Meier Survivorship Curves and Cos Proportional Hazards models.  

Our thermal stress protocol worked really well! Mortality was not too rapid and allowed us to distinguish treatments (see below). In the future, we can continue to use exposure to 33°C for ~48 h for survival assays. This was accomplished by setting the large incubator to 37°C and starting with water at the ambient temperature in the cold room.  

#### Results 

There were some interesting results in our survival data!  

##### Daily thermal hardening experiment  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20251211/daily-hardening.png?raw=true)

In oysters that experienced daily hardening, there was no significant difference in survival at either 22°C (no mortality) or 33°C. As expected, there was high mortality at 33°C. Hardening did not affect acute stress survival in the daily hardening group.  

##### Weekly thermal hardening experiment  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20251211/weekly-hardening.png?raw=true)

In oysters that experienced weekly hardening, hardened oysters survived more than control oysters! Survival was approximately 15% higher in treated oysters starting at about the 31 h measurement. This is exciting because we see a treatment phenotype >18 months after hardening and the difference was significant. There is a positive effect of weekly thermal hardening on acute stress tolerance.  

It is interesting to note that we see a difference in survival in an acute lab-based stress assay in contrast to the field survival. In the field, there was no major heat wave event but there was a small, but significant (<5%) decrease in survival in weekly treated oysters. This suggests that thermal hardening provides benefits under acute stress conditions, but not typical field conditions.  

### Acute stress resazurin metabolic assays 

I used the following procedure to conduct resazurin metabolic assays on gill tissue:  

- I selected n=9-10 oysters per group for assays from the FTR tanks
- Oysters were opened and I clipped a 30-70 mg sample of gill tissue
- Plates were filled with room temperature resazurin solution as in our core [resazurin protocols](oyster.pink)
- A sample of gill tissue from each individual was added to plate 1 (control) and plate 2 (high)
- Glycogen samples (gill tissue samples) were taken from the same individual oysters by Sam
- Weight (mg) of tissue was recorded in plate maps 
- Individuals were oriented in the same well in each of the two plates 
- Samples were loaded in 48 well plates with 1 mL of resazurin solution 
- There was a total of n=9-10 per experimental group per temperature with samples from the same individual in plate 1 and plate 2 
- Plates were read at 0, 1, 2, 3, and 4 hours on the plate reader 
- After time 0, plate 2 was placed in the benchtop incubator set at 35°C while plate 1 was placed at room temperature
-  Temperature was recorded in a subset of wells at each time point using an infrared digital thermometer 
-  Plates were rinsed after the trials 

Temperatures were 22°C at room temperature and 33°C at high temperature in the wells.  

THe protocol worked well! It took about 1 hour to load plates prior to readings. Resazurin was loaded before so that the tissue was not sitting dry in the plates. 

#### Results 

Resazurin metabolic rates also show some interesting differences between treatments! 

I analyzed the data using my typical workflow. Here are the individual metabolic trajectories for all oysters:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20251211/westcott_group_trajectories.png?raw=true)

There appear to be some differences between groups as well as temperatures. I then calculated total metabolic activity as area under the curve (AUC) to compare between groups and tempratures. 

Individual data points:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20251211/westcott_dots_AUC.png?raw=true)

Group means:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20251211/westcott_mean_AUC.png?raw=true)

These plots show acute stress temperature on the X axis (incubator temperature) and the colors show priming. Treated/primed oysters are in orange and the control/unprimed oysters are in gray.  

I ran linear models to evalulate these differences. There was a **significant effect of hardening treatment in the daily experiment**. There was no effect of hardening treatment in the weekly experiment. Overall, metabolic rates were elevated at 33°C compared to 22°C and rates were higher in daily primed oysters.  

This may suggest that hardening alters metabolic or energetic state in oysters and it is exciting that this effect is still present >18 months following hardening. 

We will think more about how this may relate to acute survival results above.     

## Next steps 

- Conduct glycogen assays on the same individuals that were measured for resazurin
- Sample remaining oysters (n=10 per group) for additional assays of interest (qPCR, other assays)  
- Outline potential Westcott hardening paper 

