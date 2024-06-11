---
layout: post
title: Oyster survival testing for lifestage carryover project
date: '2024-06-11'
categories: RobertsLab_Oysters WSG_USDA
tags: Husbandry Growth Oyster ExperimentalDesign
---

# Planning for Survivorship measurements 

Experimental design and project planning notes for lifestage carry over project survival analysis.  

The GitHub for this [project is here](https://github.com/RobertsLab/project-gigas-carryover/tree/main/lifestage_carryover).  

A visual overview of this project is available below.  

![](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/overview.png?raw=true)

We are focusing on the lifestage carryover project (LCO) for this planning.  

## Question of interest and hypothesis

*Does stress hardening increase temperature tolerance in oysters and does this effect vary by lifestage?*   

- **Hypothesis 1**: Stress hardening will increase survival times at elevated temperatures. 
- **Hypothesis 2**: The magnitude of sress hardening effects will vary by lifestage (i.e., between adults, seed, and spat). 

## General approach

Measure survival of individuals at high and control temperatures for oysters that were hardened and those that were not.  

## Overview of survivorship curve approaches 

There are three general statistical approaches that I am going to look into: Kaplan Meier, Cox proportional hazards, and logisitc regression.  

### Kaplan Meier approach

[A practical guide to understanding Kaplan-Meier curves (Rich et al. 2014)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3932959/):  

- Kaplan-Meier curves help us deal with missing observations in survival analyses. This can also be used when there are no missing observations to estimate survival and calculate survivorship metrics. 
- Time-to-event measurements including the Kaplan-Meier are used when there is a beginning and end point for all subjects within the course of the study (i.e., all subjects are alive at the start and are measured at the end of the study). 
- Subject is characterized by three variables: 1) time (serial time during study, i.e. no. of days), 2) their status at the end of the serial time - censored or event occurred, and 3) the study group they are in. 
	- For example, at the end of the study, a subject may be given a status of 1, meaning that the event (e.g., "mortality") occurred or a status of 0, meaning that the event did not occur and that individual was measured (i.e., censored and stayed alive).  
	- All individuals will therefore have a value of 0 at the beginning of the study, and could either have a value of 1 or 0 at each subsequent time point. 
	- "Censoring means the total survival time for that subject cannot be accurately determined. This can happen when something negative for the study occurs, such as the subject drops out, is lost to follow-up, or required data is not available or, conversely, something good happens, such as the study ends before the subject had the event of interest occur, i.e., they survived at least until the end of the study, but there is no knowledge of what happened thereafter."
- The interval is the time duration of known survival - this is terminated by mortality or the event of interest. Known survival can only be determined by the occurrance of the mortality event. Censored subjects are indicated as tick marks in the interval - they do not terminate the interval because the event has not yet occurrred/the subject is still alive. 
- For our use, since all oysters will be measured during the entire study, 1 indicates the event (mortality occurred) and 0 indicates the event did not occur (alive). 
- Further statistical tests can be analyzed using log rank tests or hazard ratios.  

[Understanding survival analysis: Kaplan-Meier estimate (Goel et al. 2010)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3059453/): 

- KM method involves computing probabilities of occurrence of the event at a point in time and multiplying these successive probabilities by any earlier probabilities to get a final survivorship estimate. 
	- Calculated as (number of subjects living at the start - number of subjects died) / (number of subjects living at the start)
	- Subjects who have died, dropped out, or moved are not counted as "at risk" - subjects censored do not count in the denominator. 
	- Log rank tests compare the groups statistically after generating curves.  

**Take home**: KM curves are useful to incorporate censoring and are non-parametric. I need to determine whether this is the most powerful statistical tool we can use, given that we will not need to incorporate censoring in our study (we will measure every individual across the study).  

### Cox proportional hazards model

[McLernon et al. 2022](https://www.acpjournals.org/doi/abs/10.7326/M22-0844?journalCode=aim)  

[Vasudeo Deo et al. 2021](https://link.springer.com/article/10.1007/s12055-020-01108-7)  

- Cox Prop. Hazards (CPH) models provide estimates of covariate effects
- First, report observed estimates using Kaplan Meier method
- Follow this by a statistical test (log rank) test to see if the curves are different. Log rank test is a test of whole survival estimates, not survival functionas at a particular time. 
- Next, look at effects of predictors using CPH models. 
- Log rank test can only assess effect of one variable at a time, does not allow for multiple predictors
- The log rank test also does not quantify the difference, it just tells you if it is different 
- CPH accounts for these limitations 
- Estimates a hazard ratio, which is the event rate at a given time in one group relative to another. 
- Hazard ratio is analogous to the odds ratio in multiple logistic regression analsyes. It is the ratio of the total number of observed to expected events in two independent comparison groups. For example, HR = (observed/expected in females) / (observed/expected in males) to compare male and females. 
- CPH examines association with multiple predictors while accounting for confounding. 
- Assumptions of CPH models 
	- Hazard ratio remains constant; if this is violated, alternative analyses such as accelerated failure or milestone analyses can be used. 
	- Independence of survival times between individuals (survival of one individual does not depend on another). This also applies to logistic regression. 
	- Censoring is uninformative about the outcome of interest - it is important that those who have been censored have the same risk as those who continued to be followed. 
- Steps in analysis 
	- Create a null hypothesis
	- Derive survival estimates from Kaplan meier 
	- Log rank test for difference in survival curves 
	- Check CPH assumptions 
	- Employ CPH model if assumptions are met 

### Logistic regressions

[Abbott 1985](https://academic.oup.com/aje/article/121/3/465/61440?login=true#google_vignette)  

[Liang et al. 2020](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7475459/)  

[Efron 1987](https://www.tandfonline.com/doi/abs/10.1080/01621459.1988.10478612)   

Logistic regression allows for parametric regression modeling on censored data to obtain estimates and standard errors. 

In a logistic regression, the response is transformed using a `logit` transformation, which is the log of the odds of an event occurring. Covariates can be continuous or categorical. 

Logistic models can overcome a limitation of the Cox model where time is in discrete categories, which generates a large number of ties. 

I used logistic regressions for survival in a previous paper [in a coral project here](https://link.springer.com/article/10.1007/s00338-021-02074-1).  

### Take homes 

A common approach is to use Kaplan Meier to visualize the curves, a log-rank test to compare the curves statistically, and the cox proportional hazards model to describe the effect of variables on survival.  

An alterative approach is to use logistic regression models, following workflows that are similar to mixed effect models for statistical comparisons.  

## Example survival studies in oysters 

[Kantzow et al. 2016](https://www.int-res.com/abstracts/aei/v8/p419-428/): Effect of water temperature on mortality of Pacific oysters *Crassostrea gigas* associated with microvariant ostreid herpesvirus 1 (OsHV-1 uVar)  

- Measured mortality by tmperature and viral exposure 
- Inspected individual oysters every 12 h for mortality. Removed dead osyters. Considered dead when open, non responsive to disturbance of tank, and did not retract mantle when probed with 22 gauge needle. 
- Survival time = number of hours from viral exposure to mortality. 
- Used Kaplan Meier survivorship curves for each factor (temperature and viral exposure). Curves compared using Mantel-Cox log-rank test. Combined groups together when they were not significantly different. 
- Censored alive individuals at the end of sampling. 
- Cox proportional hazards model with shared frailty was used to account for clustering using Breslow method for tied events. 
- Assessed Cox model graphically using scaled Schoenfeld residuals.
- Goodness of fit tests confirmed by Nelson-Aalen cumulative hazard and Cox-Snell residue plots. 
- Showed figures for K-M curves.  

[Petton et al. 2013](https://www.int-res.com/abstracts/aei/v3/n3/p257-273/): Temperature influence on pathogen transmission and subsequent mortalities in juvenile Pacific oysters *Crassostrea gigas*  

- Effects of temperature and viral exposure on survival
- Chi square tests for proportion survivorship 

[Ding et al. 2020](https://www.frontiersin.org/articles/10.3389/fmars.2020.00399/full): The Phenotypic and the Genetic Response to the Extreme High Temperature Provides New Insight Into Thermal Tolerance for the Pacific Oyster *Crassostrea gigas*  

- Assessed tolerance as survival rate after an acute stress. n=300 oysters 
- Classified survival by oysters being open or closed. Removed dead oysters. 
- Used Kaplan Meier anaysis with log-rank tests. 

[Pruett et al. 2021](https://www.sciencedirect.com/science/article/pii/S0022098121001052?casa_token=x4rU-Zn1MxkAAAAA:q0xMVy6GeO8523CCuMLw6BtFr2dVaRpT1zxTmPsqO9ZGUot1u03Nug-STgRO77qXj8PDPKyo): Effects of flood-associated stressors on growth and survival of early life stage oysters (*Crassostrea virginica*)

- Larval survival analyzed by generalized linear model with binomial error and logit link (logistic regression). Measured as number alive and number dead larvae. 

[McCarty et al. 2023](https://www.sciencedirect.com/science/article/pii/S0044848623004623?casa_token=36OqFkou7CAAAAAA:D3WfN19E3S4rP33t8F1dqCLEQQ1UroK2zPrDASSwv3Tn7_LKCmefW9P8FhVUmLl-OYTxhNr6): Evaluating a short vs. long-term progeny test and investigating physiology associated with survival in extreme low salinity for the eastern oyster *Crassostrea virginica*  

- Weekly survival assessments. Used as a correlate to growth, not analyzed using survival curves. 

[Agnew et al. 2020](https://www.mdpi.com/2076-0817/9/12/1057): Differential Mortality and High Viral Load in Naive Pacific Oyster Families Exposed to OsHV-1 Suggests Tolerance Rather than Resistance to Infection  

- Survival analysis of daily mortality counts over time. Juvenile oysters (n=20) in petri dishes from each of 20 families for exposure testing. 
- Checked daily for mortality by looking for gaping that did not close when probed. Counted number of live and dead oysters. 
- Used Kaplan Meier survival curves with log-rank chi-square tests and Cox proportional hazards ratios to look at differences in survival of oysters from different viral treatments. 

[Li et al. 2022](https://www.sciencedirect.com/science/article/pii/S0044848621014721?casa_token=M-nsB1o-0iIAAAAA:_qP4drwZyh8nO8a7xG1dVayfN8kD1zGSSd1Agp7ioNVy_KhT1VPUhKSyXAkgW3U-gMoUJFaQ): Effects of salinity and temperature on growth and survival of diploid and tetraploid larvae of the Pacific oyster, *Crassostrea gigas*  

- Used diploid and tetraploid animals. Tetraploids produced from eggs from triploids fertilized with sperm from diploid animals. 
- Measured accumulative survival rate (ASR) over time points across multiple days. Calculated as ASR (%) = (number of larvae alive at timepoint)/(number of larvae alive at the start) * 100% 
- No details in statistical analyses provided. Looks like ANOVA or similar analyses on proportion data.  

[Pereira et al. 2020](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0228527): Can prior exposure to stress enhance resilience to ocean warming in two oyster species?  

- Conducted lab heat shock, then outplanted to the field. n=60 shocked and 60 control of each species across 6 baskets 
- Measured growth and condition at the end of the field deployment for 7 months 
- Survival measured by counting number of live oysters in each basket. Assessed percentage with ANOVA. 
- Little effect of heat shock, but minor improvement in survival 

[Alter et al. 2023](https://www.sciencedirect.com/science/article/pii/S0044848622012923): Consequences of thermal history for growth, development and survival during metamorphosis and settlement for the European flat oyster

- t-test used to test for survival analysis between temperature conditions at one time point 

**Take homes**: 

- There are a variety of ways to analyze survival in the literature. Kaplan Meier curve approaches are most common when tracking individual survivorship and this is often paired with model approaches. In osme cases, logistic regressions are used in place of K-M curves.  
- Mortality is most often assessed by gaping and lack of response to stimulation 

## Proposed experimental design fr survival testing  

**Goal**: Expose LCO oysters to elevated temperature and monitor time to mortality in individuals 

**Exposure and holding design**: 

- Transport oysters from Point Whitney and hold in seawater at UW in the Roberts Lab 
- Place oysters in individual containers to track survival of individuals (sample size below). We can use glass jars for adults, 6 well plates for seed, and 24 well plates for spat. We should try to make the approximate volume of organism to water equal across the life stages. Each container will need to be labeled. These can even be labeled with running numbers or a code ("A-1" for adult 1, etc).  
- Oysters will be exposed in benchtop incubators. Ideally we have two incubators - one running at control temprature (~17°C) and one at high (temperature treatment described below). If we only have access to one, we can keep the control oysters on the counter top. 
- Water should be exchanged every 24 hrs. We will need access to seawater (preferrably filtered).  
- Conduct in reciprocal exposure such that each hardening treatment (treated or control) is exposed to each test treatment (control or high). This allows us to control for any effects of transport, holding, or monitoring on survival.

**Temperature treatments**:  

- We want to expose oysters to temperatures that induce mortality but allow for any variation in tolerance to be exposed. We don't want the temperature to be too hot such that any tolerance from hardening is overwhelmed. 
- The best way to do this would be to do some preliminary tests. We have some extra seed at Point Whitney and it may work best to do some preliminary survival measurements at 32°C-36°C to determine a temperature that results in mortality in 2-4 days. 

**Survival measurements**:  

- We will have individuals isolated in plates/jars in incubators at a high and control temperature individually labeled. 
- At set time intervals, we will assess all individuals as alive or dead (gaping open shell that does not respond to stimulation from poking/probing). 
- This time interval should be multiple times per day within reasonable working hours. For example, we could assess at 9am and 5pm daily until we reach <20% mortality or, ideally, total mortality in the high temperature treatment. 
- We will want to have at least 1 person each day to monitor survival, but ideally we would have 2 people at each time point to facilitate the speed of assessments.  
- Before starting the exposures, we will measure the length, width, and depth of each animal to add size as a covariate.  
- Record on data sheet as "alive" or "dead". AH will then code this to numerical values during statistical analyses. This will prevent any mix ups in recording survival during the exposures.  
- Ideally we run all tests at one time. If we have too many oysters/not enough space, we could run in two successive trials (e.g., run half in week 1 and half in week 2). We have limited sample size, so this should be avoided.  

**Statistical testing** 

- Visualize survival curves using Kaplan-Meier curves
- Test for statistical differences in curves with log-rank tests 
- Use a CPH model to test for main effects
- Also use a logistic regression approach to compare statistical approaches and develop a survival tutorial for the lab 
- Generate lethal temperature, LT50, and other values of interest  

Statistical models will be structured in general as follows: 

This can be run to compare life stages using `survival ~ hardening treatment * testing treatment * lifestage + size`.  Or we can run models subsetted by each life stage using `survival ~ hardening treatment * testing treatment + size`. 

## Replication 

### Adults 

Adults: n=15 animals per treatment per family; n=3 families

- n=45 animals treated; n=45 animals control 
- n=7-8 animals control-high
- n=7-8 animals treated-high
- n=7-8 animals control-control
- n=7-8 animals treated-control

### Seed 

Seed: n=18 animals per treatment

- n=18 animals treated; n=18 animals control
- n=9 animals control-high
- n=9 animals treated-high
- n=9 animals control-control
- n=9 animals treated-control

### Spat 

Spat: Hundreds per treatment available 

- n=50-100 animals control-high
- n=50-100 animals treated-high
- n=50-100 animals control-control
- n=50-100 animals treated-control

## Target dates 

Could be run anytime from late June throughout July. Oysters held at Point Whitney until then.  

## Data recording 

We will need to record the following data:  

- Dead or alive category for each labeled individual at each time point 
- Temperature measurements of a subset of containers at each time point 
- Metadata for launching n=3 temperature loggers in each temperature in water container 
- Metadata for animal ID numbers 

Mortality = Gaping open/open shell with no response when stimulated (probing with large needle or probe)  

## Equipment required 

- Incubator
- Jars for adults 
- 24-well plates for spat; 6- well plates for seed 
- Seawater
- Markers to label jars/plates 
- Bubbler for holding
- Jar to hold refil water at incubator temperature 
- Temperature loggers (n=6 total loggers, n=3 per temperature) - currently at Point Whitney 
- Dissecting scope to look for gaping in the smallest spat 
- Seawater (filtered if possible)  

## Next steps 

1. Go over design with the lab 
2. Identify people to help with taking surival measurements 
3. Test temperature treatments
4. Prepare for holding oysters
5. Transport from Point Whitney to UW 