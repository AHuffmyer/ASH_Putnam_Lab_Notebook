---
layout: post
title: Resazurin trials on gill tissue
date: '2025-11-26'
categories: RobertsLab_Oysters 
tags: Resazurin Oyster Cgigas
---

Today I ran some resazurin trails on gill tissue in the lab. Christina helped conduct these trials. 

# Overview 

Check out our [oyster resazurin landing page](https://robertslab.github.io/resazurin-assay-development/) for more information on the protocol and approach used in these trials. 

All data from these trials is located in the [Resazurin Family Screening GitHub repo here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/testing).  

Today, I used our standard protocol to run trials on gill tissue samples. This would allow us to run samples on larger adults without putting the entire animal in a large cup. 

# tl;dr 

- We ran trials on gill tissue from 4 individuals of varying sizes of tissue in varying volumes. We were able to determine optimal ranges of tissue sizes and plate well volumes. Ideal tissue size is approx. 50 mg in a 48 well plate.   

- We also saw strong variation in rates between individuals. 

- We also observed very strong temperature responses with much higher rates in tissue at elevated temperature (~35°C).   

# Protocol overview 

We dissected oysters today and took samples of gill tissue. We then added small, medium, and large samples from each individual to either control (20°C) or high (35°C) temperatures. We put samples from 2 animals in 96 well plates (smaller samples) and from another 2 animals in 48 well plates (larger samples). Sample readings in resazurin were read every hour for 3 h. Blanks (n=3) were included in each plate.  

Fluorescence signals were normalized to mg of tissue. See our [oyster resazurin landing page](https://robertslab.github.io/resazurin-assay-development/) for the full protocol.  

# Results 

The script to analyze these data is here [on GitHub](https://github.com/RobertsLab/resazurin-assay-development/blob/main/scripts/testing/gill-tissue.Rmd).  

Our overall observations were:  

- Larger tissue samples had higher signals
- In 96 well plates, the optimal sample is <50 mg
- In 48 well plates, the optimal sample is approx. 50 mg
- There was strong individual variation in metabolic rates of gill tissue 
- There was a strong temperature response with higher metabolic rates in gill tissue at elevated temperature (35C compared to 21C) 

Here is a plot of the metabolic rates.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20251126/plot1.png?raw=true)

From this, we can see that the smaller tissue sizes showed continual increases in signals. This shows that the larger tissue samples reached oversaturation. There was a difference in temperature in the magnitude of signal, which was more clear in the 48-well plates.  
 
# Take homes 

- We can measure signals in gill tissue
- Optimal tissue size is about 50 mg of gill tissue in a 48 well plate 
- There is a difference by temperature 