---
layout: post
title: PolyIC Seed Resazurin Assay January 28 and 29 2025
date: '2025-03-07'
categories: PolyIC_Larvae RobertsLab_Oysters
tags: Cgigas Oyster Molecular GeneExpression
---

This post details plans for an RNAseq experiment for the PolyIC project.  

# Overview 

We would like to process samples from the [PolyIC immune hardening project](https://github.com/RobertsLab/polyIC-larvae) for RNAseq. In this project, we exposed broodstock to PolyIC immune challenges and then reared their offspring through larval stages and now we have seed in the lab. Our goal is to examine gene expression across development to 1) examine treatment group differences and 2) establish and early life history time series RNA resource. 

More information can be found on the [project GitHub](https://github.com/RobertsLab/polyIC-larvae) and in [my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#polyic-larvae).   

We have observed differential phenotypes in these seed as differences in metabolic rate and survival under thermal stress.  

Undergraduate researchers in our lab are working on this project and [are posting daily notebook posts on our lab blog here](https://genefish.wordpress.com/).  

# Samples available 

| Treatment | Lifestage | Age      | Replicate-Tanks-per-Treatment | Total-Samples |
|-----------|-----------|----------|-------------------------------|---------------|
| Control   | Larvae    | 3 days   | n=1                           | 15            |
| Treated   | Larvae    | 3 days   | n=1                           | 15            |
| Control   | Larvae    | 8 days   | n=2-3                         | 15            |
| Treated   | Larvae    | 8 days   | n=2-3                         | 15            |
| Control   | Larvae    | 14 days  | n=2-3                         | 15            |
| Treated   | Larvae    | 14 days  | n=2-3                         | 15            |
| Control   | Spat      | 60 days  | n=3                           | 15            |
| Treated   | Spat      | 60 days  | n=3                           | 15            |
| Control   | Seed      | 135 days | n=2                           | 20            |
| Treated   | Seed      | 135 days | n=2                           | 20            |
|           |           |          | TOTAL                         | 160           |

We have an upper limit of 160 total samples. I requested a quote for this maximum number. A few samples have gone to Lauren for RNA extraction testing, so the final number that will be submitted will be 140-150 samples.  

# RNAseq method choices 

The sequencing choices we made are:  

- Standard RNAseq (non-stranded)
- PolyA selection library prep (to be performed by facility)
- 30M paired end reads per sample
- 160 total samples 
- Value service package with 23-28 day turn around 
- Sample type will be extracted total RNA 
- 2x150 bp sequencing 
- >85% of bases at >Q30 

The facility recommends RNA in nuclease free water on dry ice. Recommended quantity is >2 ug with required quantity of at least 250 ng.  

Sample quality should be RIN >6.0, >50ng/uL concentration, A260/A280 of 1.8-2.2 and DNA free.  

Prefers samples sent in a 96 well plate format.  

# Pricing 

I have a quote from Genewiz/Azenta for 160 samples, including a 15% discount for a total of `$26,061` at `$163` per sample.  

If we submit 140-150 samples, the final price will be approximately `$22,820 - $24,450`. 

We may also submit fewer samples than this if some samples fail in extraction or QC.  

# Time line 

- RNA extraction and sample preparation: complete by end of April 2025 
- Sample sent to facility by early May 2025
- Data would be received by early June 2025 

I will order Zymo Quick RNA/DNA Miniprep Plus kits for extraction.  

# Analytical goals 

Our goals will be to test the following hypotheses:  

- There will be differences in expression of immmune related genes in offspring from parental PolyIC treatments, with strongest effects seen in the youngest life stages.  
- There will be functional shifts in gene expression across development related to developmental needs (i.e., differentiation and growth in larvae; calcification and feeding in spat).  

We will use differential gene expression analyses and correlation network analyses to test these hypotheses.  

Further, we can use RNAseq SNP-calling to examine differences in genetic diversity in treatment groups throughout development.  

