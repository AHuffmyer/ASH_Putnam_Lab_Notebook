---
layout: post
title: RNA Extraction Log for 2023 Moorea Project
date: '2025-07-21'
categories: Moorea_SymbioticExchange_2023
tags: Molecular Extraction GeneExpression
---

This post details RNA extractions for the 2023 Moorea Symbiotic Exchange project, which contains both larval and recruit samples.  

# RNA Extraction Protocol 

The protocol for this work is documented in my [notebook here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Coral-RNA-Extraction-Protocol-for-2023-Moorea-Project/).  

The sample metadata [is on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/rna_samples.xlsx).  

### 20250613 

Today I transferred tissue samples for all larval samples ("A" and "P" sample IDs) from original screw cap cryovials into 1.5 mL safe-lock RNA free Eppendorf tubes. 

- Thaw samples on ice (stored in RNA/DNA shield)
- Transfer to new tube labeled with sequencing ID 
- Add new RNA/DNA shield to bring total volume to 1000 µL. Use the new RNA/DNA shield to ensure all material is gathered from the original tube. 
- Stored at -80°C. 

I'll next complete this process for the remaining recruit samples. 

### 20250708 

Today I finished transfering all samples to the 1.5 mL labeled safe-lock tubes as done on 20250613. I transferred any tissue or skeletal samples along with the liquid for each tube. 

I then froze all samples at -80°C.  

Next I processed the 10 test samples (T1-T10) through bead beating and RNA extraction. I used the protocol described [in my notebook here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Coral-RNA-Extraction-Protocol-for-2023-Moorea-Project/). For these test samples, I only processed RNA and did not proceed with DNA extraction.  

The bead beating and extraction process was as described in the protocol. I ran the bead beater at the 8 setting (medium-high) for 2 minutes and then spun down all samples at 2000g for 1 min in a centrifuge. 500 µL were then transferred to a new 1.5 mL tube for RNA extraction. Extra material and beads were frozen as well for back ups.  

After extraction (took about 2.5 hrs to do 10 samples), I quantified concentration using the Qubit.  

Here are the results:  

| T1  |   | ACR-A6  | 1  | Acropora    | Adult   | 800 ul shield | T1 RNA | 20250708 | 80 | 58.6 | 4688  | rna extraction only |
|-----|---|---------|----|-------------|---------|---------------|--------|----------|----|------|-------|---------------------|
| T2  |   | ACR-A7  | 2  | Acropora    | Adult   | 500 uL shield | T2 RNA | 20250708 | 80 | 28.4 | 2272  | rna extraction only |
| T3  |   | ACR-A8  | 3  | Acropora    | Larvae  | 500 uL shield | T3 RNA | 20250708 | 80 | 28.6 | 2288  | rna extraction only |
| T4  |   | ACR-A9  | 4  | Acropora    | Larvae  | 500 uL shield | T4 RNA | 20250708 | 80 | 25.6 | 2048  | rna extraction only |
| T5  |   | ACR-A10 | 5  | Acropora    | Adult   | 800 ul shield | T5 RNA | 20250708 | 80 | 34   | 2720  | rna extraction only |
| T6  |   | POC-A6  | 33 | Pocillopora | Adult   | 500 uL shield | T5 RNA | 20250708 | 80 | 67.6 | 5408  | rna extraction only |
| T7  |   | POC-A7  | 34 | Pocillopora | Adult   | 500 uL shield | T6 RNA | 20250708 | 80 | 39.4 | 3152  | rna extraction only |
| T8  |   | POC-R13 | 47 | Pocillopora | Recruit | 500 uL shield | T7 RNA | 20250708 | 80 | 39.4 | 3152  | rna extraction only |
| T9  |   | POC-R15 | 49 | Pocillopora | Recruit | 500 uL shield | T8 RNA | 20250708 | 80 | 59.4 | 4752  | rna extraction only |
| T10 |   | POC-R18 | 52 | Pocillopora | Recruit | 500 uL shield | T9 RNA | 20250708 | 80 | 9.14 | 731.2 | rna extraction only |

Concentrations look good! In general RNA concentration is 20-50 ng/µL and I do not see any issues with a particular species or lifestage. I will move forward with processing the samples for sequencing.  

Each day, I will thaw the samples on ice (n=10 per batch), bead beat, aliquot, extract RNA and DNA, and quantify RNA.  

### 20250714

Today I extracted 10 samples for RNA and gDNA using the protocol linked above with no modifications.  

I quantified using the Qubit after extractions.  

RNA concentrations look good in general. The lowest concentration samples were the Pocillopora larvae, especially those from the 33C treatment.  

Batch 1:  

| seq_id | sample_id | tube_id | genus       | lifestage | Treatment | rna.tube | dna.tube | date.extracted | extraction.batch | total.volume.ul | concentration.ng.ul | total.rna.ng |
|--------|-----------|---------|-------------|-----------|----------:|----------|----------|----------------|------------------|-----------------|---------------------|--------------|
| 1      | A7        | A7      | Acropora    | Larvae    | 30        | 1 RNA    | 1 gDNA   | 20250714       | 1                | 80              | 22.8                | 1824         |
| 2      | ACR-R22   | 24      | Acropora    | Recruit   |        30 | 2 RNA    | 2 gDNA   | 20250714       | 1                | 80              | 11.2                | 896          |
| 3      | A10       | A10     | Acropora    | Larvae    | 30        | 3 RNA    | 3 gDNA   | 20250714       | 1                | 80              | 24.8                | 1984         |
| 4      | P15       | P15     | Pocillopora | Larvae    | 33        | 4 RNA    | 4 gDNA   | 20250714       | 1                | 80              | 4.6                 | 368          |
| 5      | P16       | P16     | Pocillopora | Larvae    | 33        | 5 RNA    | 5 gDNA   | 20250714       | 1                | 80              | 6.84                | 547.2        |
| 6      | ACR-R24   | 26      | Acropora    | Recruit   |        27 | 6 RNA    | 6 gDNA   | 20250714       | 1                | 80              | 19.1                | 1528         |
| 7      | P5        | P5      | Pocillopora | Larvae    | 27        | 7 RNA    | 7 gDNA   | 20250714       | 1                | 80              | 11.2                | 896          |
| 8      | A6        | A6      | Acropora    | Larvae    | 27        | 8 RNA    | 8 gDNA   | 20250714       | 1                | 80              | 20                  | 1600         |
| 9      | A2        | A2      | Acropora    | Larvae    | 27        | 9 RNA    | 9 gDNA   | 20250714       | 1                | 80              | 24.4                | 1952         |
| 10     | P6        | P6      | Pocillopora | Larvae    | 27        | 10 RNA   | 10 gDNA  | 20250714       | 1                | 80              | 10.6                | 848          |

I will proceed with another batch on Wednesday. Today, I bead beat and aliquoted samples 11-30 in preparation for future extractions.   

### 20250716

Today I extracted 20 samples (2 batches of 10) for RNA and gDNA using the protocol linked above with no modifications. Aakriti completed gDNA extraction steps following lysis. All samples stored in -80°C.   

I quantified using the Qubit after extractions.  

RNA concentrations look good in general. The lowest concentration samples were the Pocillopora larvae, consistent with previous extractions. The facility requires >200 ng total RNA, so we are above this limit so far.    

Batch 2:  

| seq_id | sample_id | tube_id | genus       | lifestage | Treatment | rna.tube | dna.tube | date.extracted | extraction.batch | total.volume.ul | concentration.ng.ul | total.rna.ng |
|--------|-----------|---------|-------------|-----------|----------:|----------|----------|----------------|------------------|-----------------|---------------------|--------------|
| 11     | POC-R19   | 53      | Pocillopora | Recruit   |        33 | 11 RNA   | 11 gDNA  | 20250716       | 2                | 80              | 57                  | 4560         |
| 12     | A4        | A4      | Acropora    | Larvae    | 27        | 12 RNA   | 12 gDNA  | 20250716       | 2                | 80              | 27.8                | 2224         |
| 13     | P12       | P12     | Pocillopora | Larvae    | 30        | 13 RNA   | 13 gDNA  | 20250716       | 2                | 80              | 6.66                | 532.8        |
| 14     | POC-R11   | 45      | Pocillopora | Recruit   |        27 | 14 RNA   | 14 gDNA  | 20250716       | 2                | 80              | 32.2                | 2576         |
| 15     | P9        | P9      | Pocillopora | Larvae    | 30        | 15 RNA   | 15 gDNA  | 20250716       | 2                | 80              | 7.48                | 598.4        |
| 16     | P18       | P18     | Pocillopora | Larvae    | 33        | 16 RNA   | 16 gDNA  | 20250716       | 2                | 80              | 8.68                | 694.4        |
| 17     | POC-R28   | 62      | Pocillopora | Recruit   |        27 | 17 RNA   | 17 gDNA  | 20250716       | 2                | 80              | 22.2                | 1776         |
| 18     | A17       | A17     | Acropora    | Larvae    | 33        | 18 RNA   | 18 gDNA  | 20250716       | 2                | 80              | 32.2                | 2576         |
| 19     | P3        | P3      | Pocillopora | Larvae    | 27        | 19 RNA   | 19 gDNA  | 20250716       | 2                | 80              | 6.04                | 483.2        |
| 20     | A9        | A9      | Acropora    | Larvae    | 30        | 20 RNA   | 20 gDNA  | 20250716       | 2                | 80              | 32.2                | 2576         |

Batch 3:  

| seq_id | sample_id | tube_id | genus       | lifestage | Treatment | rna.tube | dna.tube | date.extracted | extraction.batch | total.volume.ul | concentration.ng.ul | total.rna.ng |
|--------|-----------|---------|-------------|-----------|----------:|----------|----------|----------------|------------------|-----------------|---------------------|--------------|
| 21     | A16       | A16     | Acropora    | Larvae    | 33        | 21 RNA   | 21 gDNA  | 20250716       | 3                | 80              | 48.2                | 3856         |
| 22     | P4        | P4      | Pocillopora | Larvae    | 27        | 22 RNA   | 22 gDNA  | 20250716       | 3                | 80              | 8.08                | 646.4        |
| 23     | ACR-R28   | 30      | Acropora    | Recruit   |        30 | 23 RNA   | 23 gDNA  | 20250716       | 3                | 80              | 8.88                | 710.4        |
| 24     | A3        | A3      | Acropora    | Larvae    | 27        | 24 RNA   | 24 gDNA  | 20250716       | 3                | 80              | 42.6                | 3408         |
| 25     | POC-R26   | 60      | Pocillopora | Recruit   |        30 | 25 RNA   | 25 gDNA  | 20250716       | 3                | 80              | 7.22                | 577.6        |
| 26     | P14       | P14     | Pocillopora | Larvae    | 33        | 26 RNA   | 26 gDNA  | 20250716       | 3                | 80              | 7.92                | 633.6        |
| 27     | ACR-R29   | 31      | Acropora    | Recruit   |        27 | 27 RNA   | 27 gDNA  | 20250716       | 3                | 80              | 27                  | 2160         |
| 28     | ACR-R18   | 20      | Acropora    | Recruit   |        30 | 28 RNA   | 28 gDNA  | 20250716       | 3                | 80              | 17.5                | 1400         |
| 29     | ACR-R25   | 27      | Acropora    | Recruit   |        30 | 29 RNA   | 29 gDNA  | 20250716       | 3                | 80              | 29.2                | 2336         |
| 30     | P10       | P10     | Pocillopora | Larvae    | 30        | 30 RNA   | 30 gDNA  | 20250716       | 3                | 80              | 7.5                 | 600          |

We are using the following equipment:  

- Eppendorf Centrifuge 5424
- Qubit High Sensitivity RNA 3.0 Fluoreometer and reagents 
- Bullet Blender bead beater Gold + 

Batches 1-3 used the following kit: Zymo Quick RNA DNA MiniPrep Plus Kit (cat no. D7003, Lot no. 225465).     

I will proceed with another batch on Friday using a new kit (Lot no. 225465). I prepped the reagents today.    

### 20250718

Today I extracted 14 samples (1 batch of 14) for RNA and gDNA using the protocol linked above with no modifications. Aakriti completed gDNA extraction steps following lysis. All samples stored in -80°C.   

I quantified using the Qubit after extractions.  

RNA concentrations look good in general. The lowest concentration samples were the Pocillopora larvae, consistent with previous extractions.   

Batch 4:  

| seq_id | sample_id | tube_id | genus       | lifestage | Treatment | rna.tube | dna.tube | date.extracted | extraction.batch | total.volume.ul | concentration.ng.ul | total.rna.ng |
|--------|-----------|---------|-------------|-----------|----------:|----------|----------|----------------|------------------|-----------------|---------------------|--------------|
| 31     | POC-R21   | 55      | Pocillopora | Recruit   |        27 | 31 RNA   | 31 gDNA  | 20250718       | 4                | 80              | 22.4                | 1792         |
| 32     | ACR-R15   | 17      | Acropora    | Recruit   |        33 | 32 RNA   | 32 gDNA  | 20250718       | 4                | 80              | 19.4                | 1552         |
| 33     | POC-R14   | 48      | Pocillopora | Recruit   |        33 | 33 RNA   | 33 gDNA  | 20250718       | 4                | 80              | 46.6                | 3728         |
| 34     | P7        | P7      | Pocillopora | Larvae    | 30        | 34 RNA   | 34 gDNA  | 20250718       | 4                | 80              | 6.98                | 558.4        |
| 35     | ACR-R27   | 29      | Acropora    | Recruit   |        27 | 35 RNA   | 35 gDNA  | 20250718       | 4                | 80              | 24.6                | 1968         |
| 36     | ACR-R12   | 14      | Acropora    | Recruit   |        33 | 36 RNA   | 36 gDNA  | 20250718       | 4                | 80              | 12.7                | 1016         |
| 37     | A8        | A8      | Acropora    | Larvae    | 30        | 37 RNA   | 37 gDNA  | 20250718       | 4                | 80              | 38.2                | 3056         |
| 38     | P8        | P8      | Pocillopora | Larvae    | 30        | 38 RNA   | 38 gDNA  | 20250718       | 4                | 80              | 7.42                | 593.6        |
| 39     | A15       | A15     | Acropora    | Larvae    | 33        | 39 RNA   | 39 gDNA  | 20250718       | 4                | 80              | 28.4                | 2272         |
| 40     | A13       | A13     | Acropora    | Larvae    | 33        | 40 RNA   | 40 gDNA  | 20250718       | 4                | 80              | 28.6                | 2288         |
| 41     | A12       | A12     | Acropora    | Larvae    | 30        | 41 RNA   | 41 gDNA  | 20250718       | 4                | 80              | 35.8                | 2864         |
| 42     | ACR-R19   | 21      | Acropora    | Recruit   |        33 | 42 RNA   | 42 gDNA  | 20250718       | 4                | 80              | 15.7                | 1256         |
| 43     | ACR-R17   | 19      | Acropora    | Recruit   |        27 | 43 RNA   | 43 gDNA  | 20250718       | 4                | 80              | 16.7                | 1336         |
| 44     | ACR-R14   | 16      | Acropora    | Recruit   |        30 | 44 RNA   | 44 gDNA  | 20250718       | 4                | 80              | 17.9                | 1432         |

Batch 4 used a new kit with the following information: Zymo Quick RNA DNA MiniPrep Plus Kit (cat no. D7003, Lot no. 225465).     

### 20250721

Today I extracted 10 samples (1 batch of 10) for RNA and gDNA using the protocol linked above with no modifications. Aakriti completed gDNA extraction steps following lysis. All samples stored in -80°C.   

I quantified using the Qubit after extractions.  

RNA concentrations look good in general. The lowest concentration samples were the Pocillopora larvae, consistent with previous extractions.   

Batch 5:  

| seq_id | box       | sample_id | tube_id | genus       | lifestage | preservation  | Treatment | rna.tube | dna.tube | date.extracted | extraction.batch | total.volume.ul | concentration.ng.ul | total.rna.ng | notes |
|--------|-----------|-----------|---------|-------------|-----------|---------------|----------:|----------|----------|----------------|------------------|-----------------|---------------------|--------------|-------|
| 45     | Box 4     | P13       | P13     | Pocillopora | Larvae    | 250 uL shield | 33        | 45 RNA   | 45 gDNA  | 20250721       | 5                | 80              | 8.34                | 667.2        |       |
| 46     | RNA Box 1 | ACR-R30   | 32      | Acropora    | Recruit   | 500 uL shield |        33 | 46 RNA   | 46 gDNA  | 20250721       | 5                | 80              | 32.4                | 2592         |       |
| 47     | RNA Box 2 | POC-R12   | 46      | Pocillopora | Recruit   | 500 uL shield |        30 | 47 RNA   | 47 gDNA  | 20250721       | 5                | 80              | 26.8                | 2144         |       |
| 48     | Box 4     | A14       | A14     | Acropora    | Larvae    | 500 uL shield | 33        | 48 RNA   | 48 gDNA  | 20250721       | 5                | 80              | 51.4                | 4112         |       |
| 49     | Box 4     | P2        | P2      | Pocillopora | Larvae    | 250 uL shield | 27        | 49 RNA   | 49 gDNA  | 20250721       | 5                | 80              | 10                  | 800          |       |
| 50     | RNA Box 1 | ACR-R13   | 15      | Acropora    | Recruit   | 500 uL shield |        33 | 50 RNA   | 50 gDNA  | 20250721       | 5                | 80              | 22.6                | 1808         |       |
| 51     | Box 4     | P1        | P1      | Pocillopora | Larvae    | 250 uL shield | 27        | 51 RNA   | 51 gDNA  | 20250721       | 5                | 80              | 6.06                | 484.8        |       |
| 52     | Box 4     | P17       | P17     | Pocillopora | Larvae    | 250 uL shield | 33        | 52 RNA   | 52 gDNA  | 20250721       | 5                | 80              | 5.34                | 427.2        |       |
| 53     | RNA Box 1 | ACR-R23   | 25      | Acropora    | Recruit   | 500 uL shield |        27 | 53 RNA   | 53 gDNA  | 20250721       | 5                | 80              | 26.4                | 2112         |       |
| 54     | RNA Box 1 | ACR-R11   | 13      | Acropora    | Recruit   | 500 uL shield |        27 | 54 RNA   | 54 gDNA  | 20250721       | 5                | 80              | 13.8                | 1104         |       | 

