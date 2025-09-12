---
layout: post
title: Resazurin family screening with Taylor Shelfish with mean family size normalization
date: '2025-09-12'
categories: RobertsLab_Oysters 
tags: Resazurin Oyster Cgigas
---

This post details analysis of Taylor Shellfish family resazurin screening. 

# Overview 

See [this previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Family-Screening-Taylor-Shellfish/) that details our resazurin trials for this screening and the protocols that we used.  

Check out our [oyster resazurin landing page](https://robertslab.github.io/resazurin-assay-development/) for more information on the protocol and approach used. 

All data from these trials is located in the [Resazurin Family Screening GitHub repo here](https://github.com/RobertsLab/resazurin-family-screenings).  

Today, I did size normalization on these data using mean family size calculations. **We will follow this up with individual size normalization after image analysis is complete.** This does provide a more accurate view of metabolic differences between families.  

# Analysis 

Here are the key results following size normalization. **There are strong differences between families.**   

### 1. Metabolic rates were different between families 

There was a wide spread in metabolic rates between families, with some families showing high activity and some showing lower activity. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250912/family_model_predictions.png?raw=true)  

It will be very exciting to run correlations between metabolic rates and predicted and/or actual performance data provided by Taylor Shellfish.  

### 2. Total metabolic activity was different between families 

There was a wide spread in total metabolic activity between families, with some families showing high activity and some showing lower activity. Family 36 was the highest, and families 66 and 67 were the lowest.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250912/auc_family.png?raw=true)  

It will be very exciting to run correlations between metabolic activity and predicted and/or actual performance data provided by Taylor Shellfish.  

### 3. Oysters do not consistently respond the same to stress upon repeat exposure 

After our initial day of resazurin trials, we saved half of the oysters and re-exposed them to the same exposures on the next day to see if the first day response reflects the second day response.  

We found that, on the whole, oysters are variable in their responses.  

First, I ran an analysis to correlate the rank-order of oyster metabolic rates for each individual and found that there were a large number of oysters that respond similarly (near the red 1:1 line).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250912/Rplot01.png?raw=true) 

However, there are groups that are high performers on one day, but not on the other (upper left and lower right quadrants).  

It will be interesting to follow this up with individual size normalization to see if these results stay consistent.  

On a family level, performance on day 1 was not predictive of performance on day 2. Results were highly variable.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250912/Rplot.png?raw=true) 