---
layout: post
title: New Guava Muse instrument set up and testing
date: '2025-09-17'
categories: RobertsLab_Oysters
tags: FlowCytometry
---

This post details set up and testing of the replacement Cytek Guava Muse instrument. 

# Overview 

Our last instrument was defective and did not pass system checks. Cytek sent a new instrument that I set up and tested today. 

# Set up 

I followed steps in the [Guava manual](https://github.com/RobertsLab/GuavaMuse/blob/main/sop/Muse.Micro.System.User.s.Guide.pdf) to set up the instrument. Here are the steps I followed:  

- Inserted and secured the flow cell 
- Attached the flow cell tubing 
- Filled the cleaning bottle with Instrument Cleaning Fluid (ICF) ~ 20 mL
- Filled the waste bottle with bleach (10 mL)
- Connected waste and cleaning bottle tubing
- Powered on the system
- Logged into the instrument as Admin with password 1234
- Set date and time 
- Perform complete system clean with ICF fluid and DI water 

# System check

I then performed system checks on the instrument. For each kit, I brought the components to room temperature before mixing. Each replicate was made with 20 µL beads and 380 µL dilution fluid. I followed this procedure for preparing the check kit: 

- Bring components to room temperature 
- Mix the beads with a P1000 pipette for about 30 seconds (gentle mixing) 
- Add 20 µL of beads to a new Guava Muse 1.5 mL tube 
- Add 280 µL of dilution fluid to the tube
- Mix with the P1000 pipette for about 15 seconds 
- Select the system check kit option on the Muse 
- Add the check kit information 
- Load sample, mix right before loading 
- The check kit reads the same sample three times. Between each reading, mix with pipette for about 15 sec

## Check kit attempt #1 

- Lot GA4200063812324 (previously opened) 
- Expires 8/9/2027
- Code ASSVW99F
- Expected 51000 particles/mL

Failed on all three reps (all values were high). I then made a new replicate. The second attempt also failed (all values were high).  

I then made samples from a new test kit. 

## Check kit attempt #2 

- Lot GA420006387125 (new) 
- Expires 4/3/2028
- Code ATSVW99G
- Expected 52000 particles/mL

Failed the first, second, and third attempts. The first attempt had one replicate reading that was OK, the others were all too low. 

I made one last attempt with another kit after another cleaning. 

## System clean 

I then ran another complete clean on the instrument. After the cleaning, I tried another check kit.  

## Check kit attempt #3 

- Lot GA4200063882123 (new) 
- Expries 8/9/2027
- Code ASSVW99F
- Expected 51000 particles/mL

This round passed! Values were 49800-48100 particles/mL. The CV was 1.93%.  

I think the pass may have been due to the second cleaning. Next time I'll try doing two cleanings before reading values or running samples.   
