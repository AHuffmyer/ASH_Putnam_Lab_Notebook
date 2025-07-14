---
layout: post
title: RNA Extraction Log for 2023 Moorea Project
date: '2025-07-08'
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
