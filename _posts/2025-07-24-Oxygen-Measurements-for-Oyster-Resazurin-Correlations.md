---
layout: post
title: Oxygen measurements for oyster oxgen-resazurin correlations 
date: '2025-07-24'
categories: RobertsLab_Oysters 
tags: Resazurin Oyster Cgigas
---

This post details activities of oxygen measurements of ~70 oysters to calculate respiration rates via oxygen evolution to correlate to resazurin responses measured earlier this week. 

This work was conducted by Mac Gavery in the lab today.

**Spoiler alert! Most of the oysters were found to be dead at the end of the trials and likely were empty shells for both resazurin and oxygen measurements. This was confirmed by showing that respiration rates were the same for "samples" as they were for empty shells. On the bright side, we developed protocols for using the oxygen sensors and it worked great! We will repeat this experiment with new oysters in a couple weeks.**   

# Overview 

We measured respiration rates at 27°C over a 1 hour period using oxygen evolution. See set up and the protocol notes below for details.  

Data were uploaded to [GitHub here](https://github.com/RobertsLab/resazurin-assay-development/tree/main/data/oxygen-correlation).  

Our goal is to correlate respiration rates with resazurin fluorescence.  

See my [previous post on resazurin measurements here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Measurements-for-Oyster-Oxygen-Correlations/) from earlier this week.    

# Experimental set up

There were about 65-70 surviving oysters from the resazurin trials this week for us to meausure respiration on today. We are using the following equipment:  

- Loligo 24 well plates with PreSens sensor spots (SP PSt5 2334-01) with SDR 1319 and MicroResp software

Our protocol was as follows:  

- Prepare 0.2 µm filtered seawater and add bubbler to achieve 100% air saturation before trials 
- Hydrate sensors in FSW for 20-30 min 
- Set incubator at 27°C 
- Collect oxygen measurements every 15 seconds. Set to mmol oxygen per L, salinity to 25 psu, temperature to 27°C, chamber volume at 1700 µL, and no treatment data added into the software. 
- Recorded oyster location in the plate map (n=16-20 oysters per plate, n=4-8 blanks per plate)
- Fill wells with FSW 
- Remove any bubbles using transfer pipette
- Add oysters to respective well. If oysters were found dead once they were in the well the shell was removed.  Rinse oysters in FSW before adding to the plate. 
- Fill well with FSW completely full and overflowing slightly. 
- Seal with plate sealer. 
- Start a run and export data once run is complete after 1 h. 

In each plate, we had live oysters ("sample") clean FSW blanks ("clean blank"), dirty FSW blanks (had a shell dipped in and then removed; "dirty blank"), and in some plates we also ran empty shell blanks ("shell blank"). We will look at the signals from each of these.  

Plates were rinsed with DI water between runs.  

We will repeat this protocol in the future on our next round of runs.  

# Mortality problems 

At the end of the trial we opened up all oysters (even if their shells were closed) and found they were dead with no tissue inside. 

Note that I also saw lower than expected resazurin measurements earlier this week with values <1 when typical resazurin values are in the range of 2-8 size normalized fluorescence. 

![](https://github.com/RobertsLab/resazurin-assay-development/blob/main/figures/oxygen-correlation/individual_trajectories.png?raw=true)    

It is most likely that we picked up background signal of microbial communities on the shells driving these low value responses.  

Today, we tested changes in oxygen measured in clean filtered seawater blanks, FSW blanks that had a shell dipped in the water ("dirty"), and blanks that contained an empty shell. I then compared the rates for each of these to oysters we thought were alive/oyster that were closed. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20270724/plot.png?raw=true)    

This shows that signals from confirmed empty shells are very similar to samples we thought were live oysters. There is one potential live oyster with the lowest value (highest oxygen consumption). The signals are likely due to respiration occuring in the microbial community.  

I extracted respiration rates using localized linear regressions using the code [here](https://github.com/RobertsLab/resazurin-assay-development/blob/main/scripts/oxygen-correlation/oxygen-rate-extraction.Rmd).  

Finally, I ran a correlation using our resazurin and oxygen data - **although this is likely signals from empty shells and shouldn't be used.** 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20270724/plot2.png?raw=true)   

There does appear to be a positive correlation, suggesting more microbial activity on larger shells could be driving the differences. 

*It's important to note that both resazurin responses and oxygen responses are small, we would expect to see larger values for live oysters.*  

In the future we will need to add empty shells as controls/blanks for resazurin and oxygen measurements. 