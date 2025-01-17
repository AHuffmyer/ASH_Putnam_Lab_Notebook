---
layout: post
title: PolyIC Seed Resazurin Assay Testing
date: '2025-01-16'
categories: PolyIC_Larvae Protocol RobertsLab_Oysters
tags: Cgigas Oyster Protocol Resazurin Survival
---

This post details methods testing from this week for PolyIC seed resazurin testing. 

# Overview 

We are running resazurin metabolic assays in the lab to test thermal tolerance of seed from the [PolyIC immune hardening project](https://github.com/RobertsLab/polyIC-larvae). In this project, we exposed broodstock to PolyIC immune challenges and then reared their offspring through larval stages and now we have seed in the lab. Our goal is to evaluate whether parental immune challenge elicits differences in thermal tolerance in seed.  

More information can be found on the [project GitHub](https://github.com/RobertsLab/polyIC-larvae) and in [my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#polyic-larvae).   

The in progress protocol for this work can be found in [my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/) and will be continually updated as we conduct assays.  

We have [previously used resazurin assays](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-data-analysis-and-size-normalization-for-10K-seed-project/) to characterize effects of thermal stress in oysters and found strong differences in metabolic rate under stress as well as significant differences in metabolic sensitivity and mortality. We will adapt these assays for use here to examine thermal tolerance in our PolyIC seed.  

Undergraduate researchers in our lab are working on this project and [are posting daily notebook posts on our lab blog here](https://genefish.wordpress.com/).  

# Where to view the full results 

The full statistical analysis is available in a published GitHub markdown document [that can be accessed here](https://github.com/RobertsLab/polyIC-larvae/blob/main/scripts/resazurin/resazurin-testing.md).  

In this post, I'll highlight the key results.   

# Tank conditions 

Oysters are being maintained in FTR tanks at 10°C with daily pH, salinity, and temperature measurements. Oysters are fed weekly with Reed Mariculture Shellfish Diet 1800 (batch 24188). A water change (parital, 5 gal) is performed weekly and salinity is maintained at 23-25 psu. These measurements are recorded in the tank room lab notebook and entered into the GitHub repo linked above.  

We also have temperature loggers in the FTR tanks (one in each tank) as well as a temperature logger at control and high resazurin temperatures.  

# Preparing solutions 

I prepared solutions [as written in the protocol in my notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Metabolic-Assays-Protocol-for-PolyIC-Seed-Testing/). I ordered new antibiotic solution.   

# Test 1. Volume gradients 

#### Method 

We first ran a test with control spat (no parental PolyIC) at control and high temperatures with a gradient of resazurin solution volumes: 100, 200, and 300 µL. The purpose of this test was to determine what volume is appropriate for measuring small spat.  

See undergraduate notebook links above for specific details on our daily activities.  

We added a single spat into wells A1-A8, B1-B8, and C1-C8 (n=8 spat per row). We then added 100 µL of resazurin working solution to row A, 200 µL to row B, and 300 µL to row C. We repeated this process for two total plates. Wells 9-12 in each row contained blanks.   

One plate went to control temperature treatment (countertop, 16°C) and the other went into the incubator at 42°C.  

We took resazurin measurements on the plate reader at 0, 1, 2, 3, and 4 hours. This part was very fast and easy because the whole plate was read on the plate reader and oysters do not need to be removed.  

I then analyzed the data using [this script in the GitHub repo](https://github.com/RobertsLab/polyIC-larvae/blob/main/scripts/resazurin/resazurin-testing.Rmd).  

#### Results

We identified 300 µL as the appropriate volume. Metabolic rates declined at 100 µL, likely due to low water volume and oxygen depletion. Rates were highly variable at 200 µL. Rates increased with time at 300 µL.  

As expected, metabolic rates were different between temperature treatments. In previous work we have also seen depressed metabolism at 42°C using this assay. This is a confirmation that we see the same result.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20251116-res/plot1.png?raw=true)

As I noted above, 100 µL volume is too little - metabolic rates between temperatures were not different as expected and rates did not increase with time in the control temperature as we would also expect.  

200 µL volume is highly variable, especially at the control temperature.  

300 µL is the selected volume we will use. The metabolic rates at control temperature continue to increase, we see a decrease in rates at 42°C, and variability is low.  

# Test 2. Survivorship and size assessments

### Survival 

After the resazurin test described above, we examined the spat for mortality. We wanted to see if we could reliably call an oyster as alive vs dead under the microscope. Since the spat are small, we needed to confirm this was possible.  

In the protocol, we will assess mortality after the final resazurin measurement time point. After the measurement, we will move the spat to a new, clean plate with seawater and leave overnight to check for delayed mortality the next morning.  

We did find that we could clearly see which spat were dead by placing in a dish under a dissecting scope with the cup size down (flat size up). We then used tweezers to probe the shell. If the shell was open and remained open when probed or touched repeatedly, it was very obviously dead. Live oysters kept a closed shell or closed the shell when probed.  

Unfortunately, there was almost complete mortality after 4h exposure to 42°C. This is not a surprise, because small spat may be more sensitive than the larger seed we have used before. I'll run a test to see if a lower temperature is more appropriate to avoid total mortality.  

Survival measurements will likely take the most time during this protocol. We will need to try a full scale trial with full plates and test our timing.    

### Size 

We tried out a few methods for assessing size for rate normalization. We have a few options:  

- Place oysters in position on the lid of the 96 well plate and image for size with ImageJ
- Place oysters in position in the well of the 96 well plate, genlty invert, image from the bottom, and measure size with ImageJ
- Weigh the oysters as they are placed into the plate and normalize to weight instead 

I would like to try the weighing option to see if this would be feasible or if it would take too much time. If it is too time consuming, we can try the imaging options.  

# Test 3. Temperature treatment 

### Method 

Finally, I ran a test at a lower temperature to see if we could reduce mortality while still being able to distinguish response to temperature.  

To do this, I added n=12 control spat and n=12 treated spat into each of two plates with one plate at 16°C and one plate at 36°C. I decreased the temperature on the incubator to be consistent at 36°C. I added n=6 blank wells per plate. 

I then ran the assay using our protocol linked above and measured resazurin fluorescence at 0, 1, 2, 3, 4, and 5 hours. 

### Results 

The results were really interesting! I found that in contrast to response at 42°C as discussed above, metabolic rates were higher at 36°C than 16°C. This makes sense - as oysters exceed their thermal maxima, they may exhibit metabolic depression. However, at lower temperatures, elevated temperature will increase metabolic rate.  

First, we found that metabolic rates were higher at 36°C - but only in treated spat! Very interesting.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20251116-res/plot2.png?raw=true)

Plotted another way, I found that treated spat had higher metabolic rates than control spat at both 16°C, and more pronouced at 36°C. Neat!  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/analysis/20251116-res/plot3.png?raw=true)

This makes me want to try a few things: 

- Look at response to other stressors (PolyIC/viral, mechanical)
- Look at resazurin response for these oysters at multiple temperatures (e.g., 16°C, 26°C, 36°C, and 42°C). This would allow us to see if the spat have different thermal performance across a range of temperatures (i.e., thermal performance curves).   
- Repeat with higher replication to see if treatment differences are real.  
