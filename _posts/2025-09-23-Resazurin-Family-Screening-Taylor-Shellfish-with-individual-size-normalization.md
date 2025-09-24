---
layout: post
title: Resazurin family screening with Taylor Shelfish with individual size normalization
date: '2025-09-23'
categories: RobertsLab_Oysters 
tags: Resazurin Oyster Cgigas
---

This post details analysis of Taylor Shellfish family resazurin screening using *individual size normalization*.   

# Overview 

See [this previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Family-Screening-Taylor-Shellfish/) that details our resazurin trials for this screening and the protocols that we used and [this post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Family-Screening-Taylor-Shellfish-with-mean-size-normalization/) for my analysis using mean family size normalization.    

Check out our [oyster resazurin landing page](https://robertslab.github.io/resazurin-assay-development/) for more information on the protocol and approach used. 

All data from these trials is located in the [Resazurin Family Screening GitHub repo here](https://github.com/RobertsLab/resazurin-family-screenings).  

Today, I did size normalization on these data using individual size calculations. This provides a more accurate view of metabolic differences and should be the data we proceed with moving forward.  

# tl;dr 

The main conclusion that metabolic rate varies by family is still very strong. However, the order of higher vs lower performing families changed with individual size normalization, suggesting individual size normalization is the most rigorous way to analyze the data.  

# Analysis 

Here are the key results following individual size normalization. **There are strong differences between families, but the results differ from mean family size normalization.**   

Note that in the individual size normalization I remove any well that had 0 or >1 oysters in the images. This only removed ~2,000 of the 13,000 total observations.  

### Individual metabolic rate trajectories 

First, here is a view of individual metabolic rate trajectories using *mean family normalization*.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250923/individual_trajectories.png?raw=true)  

Here is the same plot but with *individual size normalization*.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250923/individual_trajectories_individual-normalized.png?raw=true) 

Notice that the orange family that had the highest rates in the top plot is more similar to the other families, suggesting size effects that are accounted for with indiviudal normalization.  

### Family metabolic rate trajectories 

Here is a view of family metabolic rate trajectories using *mean family normalization*.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250923/family_model_predictions.png?raw=true)  

Here is the same plot but with *individual size normalization*.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250923/family_model_predictions_individual-normalized.png?raw=true) 

Again, notice that the rank order (especially for the family in orange in the top plot) changes with individual size normalization. There is still a similar degree of spread and the absolute values of metabolic rates are similar.  

### Family total metabolic activity 

Here is a view of family total metabolic activity (AUC) using *mean family normalization*.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250923/auc_family.png?raw=true)  

Here is the same plot but with *individual size normalization*.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20250923/auc_family_individual-normalized.png?raw=true) 

- There are some interesting observations in comparing mean family vs individual size normalization. First, family 36 had the highest metabolic rates when looking at mean family size normalization, but is in the middle of the pack following size normalization.  

- The other top performers (families 62 and 77) are still at the top of the ranks and family 72 is now the highest rank after individual size normalization.  

- Families 67, 68, 66, and 53 are all the lowest in both analyses.  

# Take homes 

This comparison demonstrates that results do change when we use mean family size vs individual size for normalization. Individual normalization should be used in the future for all analyses.  

