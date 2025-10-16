---
layout: post
title: 10K outplant growth analysis 
date: '2025-10-16'
categories: 10K_Seed WSG_USDA
tags: Cgigas Growth Oyster R 
---

Outplant Growth and Survival Analysis - 10K Seed Hardening Project

**Author:** AS Huffmyer  
**Date:** October 16, 2025  
**Assessment Date:** August 20, 2025

## Project Summary

The 10K Seed Hardening Project investigates the effects of different hardening treatments on Pacific oyster (*Crassostrea gigas*) seed resilience and performance. This project aims to evaluate whether pre-conditioning juvenile oysters through various stress exposures (temperature, salinity, immune challenge) can improve their survival and growth when outplanted to field conditions.

### Experimental Design

Approximately 10,000 Pacific oyster seed were divided into five treatment groups:

1. **Control** - No hardening treatment
2. **35C** - Temperature hardening (35°C exposure)
3. **FW** - Fresh water (salinity stress) hardening
4. **FW 35C** - Combined fresh water and temperature hardening
5. **Immune** - Immune challenge hardening with PolyIC exposure

Each treatment group consisted of 6 replicate bags, with 150 oysters initially placed in each bag. Oysters were outplanted to field conditions and monitored for growth and survival over time.

## Growth Analysis

### Methods

Growth measurements were conducted on August 20, 2025. For each bag, multiple oysters were measured for:

- Length (mm) 
- Width (mm)

Since depth measurements were not available, volume was estimated using a polynomial regression model trained on data from the same source population at Goose Point, where length, width, and depth were all measured. The volume calculation used the ellipsoid formula:

**Volume = (4/3) × π × (length/2) × (width/2) × (depth/2)**

The polynomial model was:

```
volume ~ poly(length.mm, 2) + poly(width.mm, 2) + length.mm:width.mm
```

### Data Quality Control

Several quality control steps were implemented:

- Bag 12 was identified as an outlier (much larger than all others) and removed from analysis
- Observations resulting in negative predicted volumes were removed (n<10)
- Upper outliers (volume > 125,000 mm³) were filtered out
- Standardized residuals were checked for outliers (threshold = 3 standard deviations)

### Statistical Analysis

A linear mixed effects model was used to test for treatment effects on growth:

```
model <- lmer(sqrt(Predicted_Volume_Poly) ~ treatment + (1|purple.tag:treatment), data=data)
```

The model included:

- **Fixed effect:** Treatment group
- **Random effect:** Bag nested within treatment (purple.tag:treatment)
- **Transformation:** Square root transformation of predicted volume to improve normality

### Results

![Growth by Bag](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20251016/bag_sizes.png?raw=true)
*Figure 1: Predicted oyster volume (mm³) by bag, colored by treatment. Each point represents an individual oyster measurement.*

![Growth by Treatment](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20251016/treatment_sizes.png?raw=true)
*Figure 2: Predicted oyster volume (mm³) by treatment group. No significant differences were observed among treatments.*

**Key Finding:** There were *no significant differences in growth* among treatment groups (p > 0.05). All hardening treatments resulted in similar final sizes when outplanted to field conditions. 

The lack of treatment effects suggests that:

1. The hardening treatments did not negatively impact growth capacity
2. Field conditions may override any growth advantages conferred by pre-conditioning
3. All oysters, regardless of treatment, were able to grow similarly under field conditions

## Interpretation and Conclusions

### Growth Performance

The absence of treatment effects on growth is both informative and encouraging:

1. **No negative impacts:** None of the hardening treatments resulted in reduced growth, suggesting that the stress exposures did not cause lasting growth depression or metabolic costs that persisted into the field phase.

2. **Field conditions dominate:** Once deployed to the field, environmental conditions (food availability, water quality, temperature) appear to be the primary drivers of growth, superseding any metabolic or physiological differences induced by pre-conditioning.

3. **Phenotypic plasticity:** Pacific oysters demonstrated growth plasticity, with all groups converging on similar growth trajectories regardless of their conditioning history.

### Next steps

- Normalize size to initial outplant sizes (pending AH image analysis)
- Conduct winter/spring assessments in 2026

## Data and Code Availability

All data and code [are available on our repo](https://github.com/RobertsLab/10K-seed-Cgigas).  