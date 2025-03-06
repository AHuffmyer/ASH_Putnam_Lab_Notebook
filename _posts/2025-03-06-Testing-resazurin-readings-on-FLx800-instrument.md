---
layout: post
title: Testing resazurin readings on FLx800 instrument
date: '2025-03-06'
categories: RobertsLab_Oysters
tags: Resazurin Oyster Cgigas
---

This post details my tests and set up for resazurin readings on the FLx800 instrument in the MERLAB. 

# Overview 

Our plate reader broke last month, so we have been preparing and testing readings on an FLx800 fluorescence plate reader in the MERLAB.  

We have a new plate reader ordered, so I am testing the MERLAB instrument until then.  

## Setting up the instrument and installing filters 

I installed filters that were sent to us from the Agilent agent. The plate reader has an Excitation filter wheel ("EX") and an Emission filter wheel ("EM").  

I added the 528/20 filter to the Excitation wheel and the 590/20 wheel to the Emission wheel using the user manual instructions. 

The "/20" is the band width. So for example, the 528 filter will capture wavelengths +/- 20. Our target is to use excitation of 530 and emission of 590, so these filters will work well.  

I labeled the filter sets on the wheel itself and then added the filter sets to the software.  

## Writing protocols 

I then generated a new protocol for the instrument with the following steps:  

1. Load plate in 
2. Read in fluorescence mode with endpoint readings at excitation 528/20 and emission at 590/20 using Filter Set 1 with optics position at the bottom and sensitivity at 35. 
3. Unload plate out 

I validated the protocol and all steps were confirmed.  

## Test measurements on 20250305

I tried to run some tests today, but the monitor broke when I tried to run a plate. Tomorrow I will replace the monitor and try again.  

## Test measurements on 20250306 

Today I replaced the monitor on the instrument and it worked! I read a test plate with row A as blanks with resazurin and row B as oysters (old PolyIC batch) with resazurin as done previously for our protocols.  

Initial readings were around 600-700. The Renquist et al. protocol we are using suggests to adjust the gain such that readings are around 500. Our previous measurements were much higher - this value seems more appropriate. I will leave the settings as is and see how the values change over the day. 

I expect that values will increase in wells with oysters and stay stable in wells without oysters. 

The plate is being left on the counter at 16-20°C in the MERLAB.  

I loaded the plate with A1 in the upper left corner. This is the correct orientation.  

In order to save the data, you first save the "experiment" and then after reading the plate you can "export" data into a .txt format that contains the data.  

Here are the results from today:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/resazurin/rest-res-plot.png?raw=true)

The blanks remained stable and all of the oysters wells increased over time as expected. The well with the highest fluorescence values was visually the most pink as well. 

The tests worked well! Next week I'll start experimental runs on the new batch of PolyIC seed at control and elevated temperatures.  