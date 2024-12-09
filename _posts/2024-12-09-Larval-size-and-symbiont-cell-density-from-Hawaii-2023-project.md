---
layout: post
title: Larval size and symbiont cell density from Hawaii 2023 project
date: '2024-12-09'
categories: Larval_Symbiont_TPC_2023
tags: CellDensity LarvalSize Mcapitata Physiology
---

This post details processing of Hawaii 2023 project samples for larval size and symbiont cell density.  

For more information on this project, see the [GitHub repo here](https://github.com/AHuffmyer/larval_symbiont_TPC) and my previous [notebook posts here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#larval-symbiont-tpc-2023). 

The samples used for this processing were collected in June/July 2023 across larval development in larvae from previously bleached, non-bleached, or wildtype parents. The samples contained 30-50 larvae each in 200 uL Z-fix. There are 90 total samples.  

Sample [metadata is available here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/sample_metadata/cell_density_size_sample_metadata.xlsx).   

I will update this post as I go through the processing and finish samples.   

Data sheets used are here:  

- [Larval counts](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/size_cells/larval_counts.csv)
- [Larval size](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/size_cells/larval_size.csv)
- [Cell density](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/size_cells/cell-density.csv)

## Larval size processing

- Jill previously took images of the fixed larvae with all larvae in the sample contained in the images. The images have a scale bar.  
- These images are stored on Google Drive. 
- My next step is to measure the planar area of each larvae in all images. 
- We will use this for larval size as a response and as a normalizer for cell density. 

**TO DO: I need to measure size of larvae in the images.** 

## Preparation of samples for cell density 

The working protocol for isolation of symbiont cells is as follows:  

- Remove as much Z-fix as possible from each tube and discard (~200 uL) 
- Add 200 uL of DI water 
- Centrifuge for 3 minutes at 3,000g
- Remove the liquid host fraction and discard (this sample was fixed, so we are not able to keep it for any other assays)  
- Freeze pellet at -80°C until processing for cell counts  

#### DATE LOG OF PROCESSING HERE 

## Cell density 

- Thaw sample on ice 
- Resuspend in 100 uL DI water and mix with pipette 
- Homogenize briefly with pipette
- Count cells on hemocytometer in 6 replicates per sample using the [Putnam Lab protocol](https://github.com/Putnam-Lab/Lab_Management/blob/master/Lab_Resources/Physiology_Protocols/Cell_Density-Protocol.md)  
- Cell density will be calculated in R and normalized to number of larvae (cells per larvae) and larval size (cells per unit planar surface area).  

#### DATE LOG OF PROCESSING HERE 