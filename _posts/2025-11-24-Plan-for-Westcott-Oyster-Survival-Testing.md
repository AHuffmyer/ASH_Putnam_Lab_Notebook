---
layout: post
title: Plan for Westcott Oyster Survival Testing
date: '2025-11-24'
categories: RobertsLab_Oysters WSG_USDA
tags: Oyster Cgigas
---

This post details a plan for running acute stress survival assays in the lab the week of December 8-12 from Westcott oysters.  

I'll continue to edit this post as we revise our plan and add data.   

## Overview 

Our goal is to subsample oysters from the treatments at Westcott and conduct acute survival testing in the lab at UW. This post describes the plan for survival testing. We may also try to do some resazurin work on gill tissue, but this will be pending some tests that I will run on Nov 26th to confirm we can use gill tissue for resazurin tests. 

Our hypothesis is that hardened oysters will have higher survival under thermal stress than control oysters.  

## General Timeline 

Below is a general timeline of events. I have specific information for scheduling below.  

- November 26: Ariana put together supplies and conduct resazurin tests. 
- December 2: Assessment at Westcott and taking subsamples for transport
- December 3-4: Grace L. transport Westcott oysters to UW 
- December 4: Noah O. receives oysters in the lab at UW and makes sure tanks are running and oysters are fed 
- December 8: Experiment set up and preparation 
- December 9-11: Survival testing
- December 12: Clean up and storage of any remaining samples

People available to help: Steven, Christina, Noah O., Ariana (Dec 10-11 only) 

## Sample size and groups to transport

We have the following treatments at Westcott:  

- "Daily" hardening - control and treated 
- "Weekly" hardening - control and treated 

In the field, we have seen lower growth in the treated oysters from both hardening efforts.  

In order to conduct survival testing, we will do a reciprocal exposure of each group to high and control tempratures for 48-72 h. Therefore, we need enough samples of each group to do these measurements.  

Sample size: n=30 oysters from each group  

This will provide us with n=10 at control and n=20 at high for each group. In total we will transport 120 oysters. We rarely observe mortality in controls in these tests, so that is why sample size will be smaller at control than high.   

## Treatments 

Reciprocal exposure of each treatment group to either:  

- Control (room temperature, ~20°C)
- High (in large incubator, ~38°C)

Treatments will continue until significant mortality (<30% survival at high temperature) is reached, which will likely be within 48-72 h from our previous tests. 

Oysters will be placed in individual cups or bowls with seawater and placed in the incubator during the tests. Water changes can be performed every 24 h using water that is pre warmed in the incubator.  

## Measurements 

1. Survival: We will measure survival of each oyster as "alive" or "dead" at multiple time points over 2-3 days
2. Tissue samples: In all control oysters and oysters that survive the stress trial, we will sample gill tissue and freeze for glycogen assays or other testing. 
3. Resazurin *potential*: Depending on results from a trial before the sampling, we may run resazurin on a subset of gill tissue. Ariana will determine this closer to time.  

I have premade a spreadsheet on [GitHub](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/data/survival/survival_testing_Dec25/westcott_survival_testing.csv).  

The format will look something like this:  

| date     | site     | group          | temperature | oyster_id | 0     | 4     | 8     | 24    | 28    | 32    | 48    | 52    | 56    | tube_id |
|----------|----------|----------------|-------------|-----------|-------|-------|-------|-------|-------|-------|-------|-------|-------|---------|
| 20251209 | Westcott | Daily Treated  | control     | 1         | alive | alive | alive | alive | alive | alive | alive | alive | alive | 1       |
| 20251209 | Westcott | Daily Control  | high        | 2         | alive | alive | alive | alive | dead  | dead  | dead  | dead  | dead  | NA      |
| 20251209 | Westcott | Weekly Treated | high        | 3         | alive | alive | alive | alive | alive | alive | alive | alive | alive | 3       |

## Sampling 

After the stress trial is complete, we will sample gill tissue from all remaining live oysters. This will likely be all control oysters as well as a subset of oysters that survived the high temperature exposure.  

- Oysters will be removed from the cup, opened, and gill tissue will be cut. 
- Tissue will be placed in snap cap tubes and frozen either on dry ice or liquid nitrogen
- Label tubes with date, "Westcott", and a running number
- Record the oyster cup number that goes into each tube. We will use this to track metadata
- Place in a freezer box in the -80°C freezer

Pending resazurin results, we may also use some gill tissue from these issues to run resazurin trials. Ariana will plan this out based on how trials go on Nov 26th.  

## Full protocol 

Survial trials: 

- Remove oysters from tank
- Place individual oysters into labeled numbered cups
- Fill cups with freshly made instant ocean seawater (ambient temperature) at salinity 20-25 psu
- Record the treatment group that corresponds to each cup number and write a datasheet that has columns for "cup", "group", "temperature treatment", and columns for each time measurement (e.g., 0, 4, 8, 24, 28, 32, 48, 52, 56 hrs)
- Place n=10 oysters from each group on the counter at room temperature (control)
- Place n=20 oysters from each group in the incubator set at 38°C (high)
- Record "alive" for all oysters at Time 0 (when the incubation starts) and record the time oysters were added to the cups. 
- Use a temperature probe to record the temperature in n=5 control and n=5 high cups 
- About 4 hours later, look at all the oysters and record if they are alive or dead. Record "alive" or "dead" for each oyster on the data sheet under a column labeled "4 hours"
- Repeat at 8 hours later (end of Day 1)
- Repeat this process for Day 2 - record survival for each oyster in the morning, mid-day, and evening under columns for the appropriate time period. At each assessment record the time and temperature measurements. 
- Once survival at high treatment is at about 30% or below, the trial can be stopped. 

Sampling: 

- Remove oyster from cups, open, and take a sample fo gill tissue. 
- Place tissue in snap cap tubes and freeze either on dry ice or in liquid nitrogen
- Label tubes with date, "Westcott", and the cup number of the oyster (e.g., 20251210 Westcott 1, 20251210 Westcott 2, etc)
- Record the oyster cup number that goes into each tube. We will use this to track metadata
- Place in a freezer box in the -80°C freezer

Resazurin: 

- If we proceed with resazurin, we will use an additional sample of gill tissue from control oysters to run in resazurin plates at control and high temperatures for each individual  
- This would include n=1 sample at high and n=1 sample at control for each oyster, totaling n=10 per group per temperature 

## To do at Westcott & supplies 

- Conduct final assessment of survival 
- Collect a sample of n=30 oysters from each group
- Add oysters to labeled seed bags with group informatin
- Transport to UW and place in tanks in FTR

## To do at UW & supplies 

- Prepare batches of Instant Ocean seawater (freshly made) and place in incubator and at room temp to use for water changes 
- Prepare tubes, freezer boxes 
- Prepare data sheets 
- Prepare cups and counter space/incubator space 
- Prepare dissection tools for sampling
- Conduct resazurin trial on Nov 26th 