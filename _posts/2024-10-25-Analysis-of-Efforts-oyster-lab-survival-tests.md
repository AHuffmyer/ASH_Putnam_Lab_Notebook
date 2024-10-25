---
layout: post
title: Analysis of Efforts BDE oyster lab survival tests
date: '2024-10-25'
categories: RobertsLab_Oysters WSG_USDA
tags: Cgigas Oyster R Survival
---

Today I ran scripts to analyze Efforts B, E, and D survival incubations for oysters in the lab. 

# Overview 

GitHub repositories for this project can be found at the following locations:  

- [Oyster stress hardening](https://github.com/RobertsLab/project-gigas-conditioning/tree/main) 

The script used here can be found on [GitHub at this link](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/code/survival_lab.Rmd).  

In this project, we exposed oyster seed to hardening stressors (sub lethal temperature exopsure) in the spring. We are now conducting lab thermal stress tests to test survivorship. Counterparts of these oysters are deployed at field sites.  

Noah's notebook posts for these incubations [can be found here](https://genefish.wordpress.com/).  

## Results 

I ran Kaplan-Meier survival curves and Cox Proportional Hazards models in the script linked above. Here are the results for each effort.  

For each effort, we exposed treated and control hardening exposure groups to either 18°C or 46°C for hours. Noah measured survival hourly and 24 hours after exposure.  

### Effort B: Daily temperature hardening exposure 

In this effort, we exposed oysters to daily sublethal temperatures for about 6 weeks at Point Whitney (treated) and held others at ambient temperature (control). This was conducted on the 2023 POGS "small" seed. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/surv/KMcurves_EffortB.png?raw=true)

There was no difference in survival between hardening treatments. 

### Effort D: Weekly temperature hardening exposure 

In this effort, we exposed oysters to weekly sublethal temperatures for about 6 weeks at Point Whitney (treated) and held others at ambient temperature (control). This was conducted on the 2023 POGS "small" seed. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/surv/KMcurves_EffortD.png?raw=true)

There was no difference in survival between hardening treatments. 

### Effort E: Weekly temperature and fresh water hardening exposure 

In this effort, we exposed oysters to weekly sublethal temperature or fresh water exposure for about 4 weeks at Point Whitney (treated) and held others at ambient temperature (control). This was conducted on the 2023 POGS "large" seed. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/surv/KMcurves_EffortE.png?raw=true)

There was no difference in survival between hardening treatments. There was lower survival in the temperature group than the freshwater group for both treated and control. This is likely because they were reared in different Heath trays but were in the same stack. There is no difference between control and treated groups within each type of treatment.  

### Comparing efforts at 46°C 

Because the oysters are all from the same stock, I compared treatments between each effort. Note that each group was reared a bit differently, so we cannot separate any differences due soley to treatment. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/surv/KMcurves_EffortsALL.png?raw=true)

As seen above, Effort E temperature group survived less than all other groups. These were reared in a Heath tray in the same stack as the fresh water group, but were one row above. I am not sure what would cause them to have lower survival. Regardless, there was no difference in treated groups compared to their respective controls.  

