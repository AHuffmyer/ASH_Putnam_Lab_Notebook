---
layout: post
title: Data entry for Goose Point data and schedule for incubations
date: '2024-09-13'
categories: RobertsLab_Oysters WSG_USDA
tags: Cgigas Growth Oyster Survival DataEntry
---

This post details protocol for data entry for Goose Point oyster growth data from 9/9/2024 and schedule for oyster survival incubations next week. 

# Overview

Today I met with Madeline to go over data entry, GitHub use, and incubations for oyster survival.  

## 1. GitHub and R project use 

Madeline and I went through the protocols for using GitHub. Madeline now has access to the Roberts Lab GitHub resources repository as well as the 10K Seed and Oyster Hardening GitHub repositories.  

[10K Seed project repo](https://github.com/RobertsLab/10K-seed-Cgigas)      
[Oyster hardening repo](https://github.com/RobertsLab/project-gigas-conditioning)    

We downloaded GitHub desktop and integrated GitHub push/pull capacity with Madeline's RStudio. Here are the best practices that we talked through:  

1. Always PULL from the repo before changing any files.   
2. PUSH any changes to the repo once you are finished with a task.   
3. Write a detailed COMMIT message to explain the changes that were made.  
4. Add any questions, issues, concerns to a GitHub ISSUE within the repo. 

Madeline successfully cloned the repositories and pushed/pulled practice changes to the files that we will be working on.  

Madeline will also use the data we enter to practice R skills!  

## 2. Data entry protocol 

We will be entering growth data from the 9/9/2024 Goose Point field collection. Here is the protocol we will follow: 

1. PULL from the [Oyster hardening repo](https://github.com/RobertsLab/project-gigas-conditioning) before beginning work. 
2. Open the [growth data sheet](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/data/outplanting/GoosePoint/growth_GoosePoint.csv). 
3. Open the folder with [images of data notebooks](https://github.com/RobertsLab/project-gigas-conditioning/tree/main/data/outplanting/GoosePoint/size/20240909). 
4. Open the first image that hasn't been completed yet (more on this below). 
5. Enter the `field_cattle_tag` number in the first column. This is the "bag" number in the notebook images. 
6. Enter the `date` as "20240909". 
7. Enter a running number for each oyster measurement entered. This will be a running number from 1 to 100 for each bag (the total number of oysters should be 100 or very close to 100). 
8. Enter the `length.mm`, `width.mm`, and `depth.mm` in the next three columns. In the notebook, we recorded these values without decimal places for efficiency. For example, a value of `1345` is `13.45` and a value of `975` or `0975` indicates `9.75`. Enter the measurement with these decimal places. 
9. If you have questions or are unsure of the value in the notebook, add a `notes` column to the end of the data sheet and record any notes there. Ariana can go back and check these values. 
10. If the oyster is recorded as dead with an "*" next to the value, enter the measurements as you normally would AND put a `1` in the `dead` column. All other live oysters will get a `0` in the `dead` column. 
11. If the oyster is denoted as "stuck together" in the notebook, enter a `1` in the `stuck` column. All other normal oysters will get a `0` in the `stuck` column. 
12. Finally, record the image name next to each measurement so that, if needed, we can QC the measurements easily. 
13. Save the file. 
14. Rename any images that have been completed with "_done" at the end of the image name. Keep the original image numbers in the file name. 
15. ADD, COMMIT, and PUSH the saved files to GitHub. 
16. Repeat until all are completed! 

After data is entered, we will make some scripts to graph the data - this will be a great opportunity for Madeline to practice plotting and coding in R.  

## 3. Schedule for next week 

Next week, we are going to try increasing heat stress for survival incubation trials. Here is the schedule we agreed on:  

- MONDAY: Run a new incubation trial of 45°C for 5 hrs. Use n=20 oysters in individual cups each from bags 1 and 20 at treatment (in the incubator) and control (on the bench/counter). This will be control and temperature hardened oysters from Effort E. This will result in oysters from each hardening treatment at a control and stress treatment incubation. Place all oysters on the middle shelf and randomize their location between the bags. Add pre-heated water to the cups at the start of the incubation. Record survival every hour and at the end of the incubation. At the end of the incubation, place all individuals on the bench at room temperature and clearly mark those that were stressed vs control. 
- TUESDAY: Check survival from Monday and record any mortality. We will chat about results at this point and decide what to do next. 
- WEDNESDAY: Run a new trial either to 1) change the protocol again to try to get desired mortality or 2) keep the protocol from yesterday if it worked well and test a new group. 
- THURSDAY: Check mortality from Wednesday's trial. 