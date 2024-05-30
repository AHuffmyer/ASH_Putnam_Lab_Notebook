---
layout: post
title: Hatchery field day at Point Whitney May 29 2024
date: '2024-05-29'
categories: RobertsLab_Oysters WSG_USDA
tags: Husbandry Growth Oyster ExperimentalDesign
---

This post details activities at Point Whitney from 29 May 2024 trip.  

# Overview 

Today we went to Point Whitney to prepare tanks for larval work. Except for the LCO project, all oysters are in a common upweller tank or in outdoor trays in preparation for outplanting. We are not doing weekly stressors. 

### Weekly probe measurements

AH took weekly measurements at 13:30. The only tank with oysters running is the left tank. Fluorometry measurements were 11 in this tank. We did not do any pulse feeding. 

*Environmental measurements*  

| tank         | date    | time  | temp.C | pH.nbs | sal.psu | initials | notes      |
|--------------|---------|-------|--------|--------|---------|----------|------------|
| left         | 5/29/24 | 13:30 | 15.2   | 8.08   | 29.87   | ash      | tank       |
| left         | 5/29/24 | 13:30 | 15.2   | 8.09   | 29.85   | ash      | silo       |
| left         | 5/29/24 | 13:30 | 15.1   | 8.1    | 29.87   | ash      | silo       |
| outdoor_tray | 5/29/24 | 13:30 | 14.7   | 7.87   | 29.01   | ash      | large POGS |
| outdoor_tray | 5/29/24 | 13:30 | 14.7   | 7.87   | 29      | ash      | large POGS |
| outdoor_tray | 5/29/24 | 13:30 | 14.6   | 7.74   | 28.95   | ash      | small POGS |
| outdoor_tray | 5/29/24 | 13:30 | 14.6   | 7.75   | 28.94   | ash      | small POGS |

### Re doing larval system 

Today we raised the table and header tank and reorganized the system for ease of access and to simplify the system. Next week will will add air into the system. We started 6 HUDLs to see how we like the flow. We used the higher flow gray water tips for this round.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/pic2.jpeg?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/pic3.jpeg?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/pic4.jpeg?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/pic5.jpeg?raw=true)

### Field outplant planning 

AH made an overview document for outplanting [that can be found here](https://github.com/RobertsLab/project-gigas-conditioning/blob/main/planning/summer-2024-outplanting.md).  

### Download Apex temperature data 

We need to download all Apex data for our tank system. 

- T3-tmp is the treated daily hardening tank
- Tmp is the feed bucket
- Tmp-44 is the control tank

I figured out how to download in batches. This needs to be done when you are on the same internet as the Apex system (local access).  

1. Access log through URL `http://IPADDRESS//cgi-bin/datalog.xml?sdate=240101&days=15`. Note that I had memory issues if I did more than 15 days. If you have many probes, you will likely have to do fewer numbers of days per download. Change the `IPADDRESS` and the start date `sdate` and number of days `days` for what you need. 

2. Wait until it loads to the XML format as pictured here. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/apex1.png?raw=true)

3. Copy all text with Command + A then Command + C. Paste the text into a text editor (I use VS Code). 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/apex2.png?raw=true)

4. Remove the top line that starts with "This XML file....". It should now look like this: 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/apex3.png?raw=true)

5. Now save as a .xml file to your computer. Next, open the .xml file using Excel. The file should look like this:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/apex4.png?raw=true)

6. Resave in whatever format you need. Name by date range and repeat until all data has been saved. Now it can be loaded into R for analysis and plotting.  

## Notebook images 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/nb1.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/nb2.jpeg?raw=true)  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/wsg_usda/20240529/nb3.jpeg?raw=true)
 
## To do next week 

- Download remaining Apex data 
- Add air lines to HUDLs and reconfigure air system 
- Receive new POGS seed sent from Neil (arrival June 4th) 
- Prepare stocks for Westcott outplanting (and give to Steven if possible to deliver) 
- Weekly probe measurements 
- Start broodstock stress 
