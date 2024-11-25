---
layout: post
title: Sanger sequencing preps for Moorea 2023 project at URI
date: '2024-11-24'
categories: Moorea_SymbioticExchange_2023 Protocol
tags: Extraction Molecular PCR Processing Protocol RFLP Sanger 
---

During my visit to URI this past week I processed DNA for Sanger sequencing for species identification. These samples are from the [2023 Moorea Symbiotic Exchange project](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023). In this project, I collected corals of three genera (*Acropora, Pocillopora, Porites*) including adults and recruits. We used Sanger sequencing and RFLP to identify species within these genera.  

# Overview 

gDNA extractions were [completed by Pierrick](https://github.com/PierrickHarnay/PierrickHarnay_Notebook/blob/master/_posts/2024-09-26-gDNA-extraction-corals-samples-Mo'orea-field-trip-2023.md) in October 2024.  

My samples were included on Plate 38, 39, 40, and 41 of extractions. Sample metadata is [on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/dna_metadata.xlsx). 

# Protocols used 

- [gDNA extraction](https://github.com/PierrickHarnay/PierrickHarnay_Notebook/blob/master/_posts/2024-09-26-gDNA-extraction-corals-samples-Mo'orea-field-trip-2023.md)
- [Pocillopora mtORF PCR](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/POR-POR-Species-ID-Sanger-and-RFLP-Protocol/)
- [Pocillopora Histone PCR and RFLP](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/POR-POR-Species-ID-Sanger-and-RFLP-Protocol/)
- [Porites H2 PCR](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/POR-POR-Species-ID-Sanger-and-RFLP-Protocol/)
- [Gel electrophoresis](https://emmastrand.github.io/EmmaStrand_Notebook/Gel-Electrophoresis-Protocol/) 
- [PCR product cleaning](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/protocol_zr-96_dna_clean-up_kit.pdf) 
- [Sanger sequencing at URI](https://web.uri.edu/riinbre/mic/)

Sequencing metadata sheets and all images of gels and associated data is on my [GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/tree/main/data/dna).  

## Pocillopora 

To identify cryptic species in *Pocillopora* we use mtORF sequencing as described in [Johnston et al. 2018](https://peerj.com/articles/4355/) and [Burgess et al. 2024](https://pubmed.ncbi.nlm.nih.gov/39100752/). Our protocol for this is described [here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/POR-POR-Species-ID-Sanger-and-RFLP-Protocol/).  

mtORF sequencing identifies haplotypes. For haplotype 1a (*grandis/meandrina* complex), we then conduct an RFLP analysis as described in [Johnston et al. 2018](https://peerj.com/articles/4355/) to distinguish between these two species. This week I finished up preparing samples for sequencing and ran an RFLP. 

### mtORF sequencing 

#### 20241117

Today I prepared the following samples for mtORF sequencing. These were previously extracted by Pierrick, amplified for mtORF, and the PCR product was cleaned. The plate maps and gels for these samples [can be found on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/tree/main/data/dna/extractions). 

- All plate 39 Huffmyer Pocillopora samples: Wells A1-C8
- Samples in wells F2, F5, F8, F11, and H11 in plate 38 that failed sequencing in a previous run 

I'll post another notebook post detailing sequencing analyses.  

Protocol:  

1. Dilute forward primer (FatP6.1) to 3.2 uM by adding 33.3 uL of 10 uM primer to 66.6 uL of nuclease-free water 
2. Add 8 uL of water to each strip tube for samples 
3. Add 2 uL of sample clean PCR product to respective tube
4. Add 2 uL of F primer at 3.2 uM to each tube  

Sample information is as follows for samples labeled with "Project 1" for sequencer.      

| Sample ID | Sample Name | Template Type | PCR Template Volume (µl) | H20 Volume (µl) | Primer Volume (µl) | Primer Name | Notes                                        |
|-----------|-------------|---------------|--------------------------|-----------------|--------------------|-------------|----------------------------------------------|
| AH001     | 183         | PCR           | 2                        | 8               | 2                  | FatP6.1     | Strip tubes; water and primer already added  |
| AH002     | 184         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH003     | 185         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH004     | 186         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH005     | 187         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH006     | 188         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH007     | 189         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH008     | 190         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH009     | 191         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH010     | 194         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH011     | 195         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH012     | 196         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH013     | 197         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH014     | 198         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH015     | 199         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH016     | 200         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH017     | 201         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH018     | 202         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH019     | 203         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH020     | 204         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH021     | 205         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH022     | 206         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH023     | 207         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH024     | 208         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH025     | 209         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH026     | 210         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH027     | 211         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH028     | 212         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH029     | 213         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH030     | 214         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH031     | 228         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH032     | 229         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH033     | 114         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH034     | 117         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH035     | 120         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH036     | 123         | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |
| AH037     | 53          | PCR           | 2                        | 8               | 2                  | FatP6.1     |                                              |

#### 20241120

These were dropped off for sequencing on 20241120 by Hollie.  

### Poc Histone RFLP

#### 20241117

I performed an RFLP using the protocol linked above for all *Pocillopora* samples. I decided to run all samples before all have been sequenced because I will not have enough time to get back sequences and analyze before I leave URI. By performing RFLP on all samples, I will have the RFLP data to reference after analyzing sequences and will not require someone to run the RFLP for me later.    

I ran the RFLP on 81 total samples.    

I prepared the master mix.  

| Component                   | 1 rxn   | 100 rxns |
|-----------------------------|---------|----------|
| EmeraldAmp 2X TT master mix | 12.55uL | 1255ul   |
| POC Histone F primer (10uM) | 0.32uL  | 32uL     |
| POC Histone R primer (10uM) | 0.32uL  | 32uL     |
| Ultrapure H20               | 10.8uL  | 1080uL   |
| Template DNA               | 1uL  |  |
|                             | 25uL    | 23.99uL  |

I ran all reactions in a 96 well PCR plate. Added 24 uL of master mix to each well. Then added 1 uL of template DNA of each sample. 1uL of water was used in one well as a control. 

I used a positive control as sample #159 from Hollie. There was very little DNA left in the tube.  

The PCR protocol was as follows:  

- 94°C for 60 sec [1 cycle]
- 94°C for 30 sec, 53°C for 30 sec, 72°C for 60 sec [30 cycles]
- 72°C for 5 min [1 cycle]
- Hold at 4°C 

After PCR, samples were stored in the fridge overnight.  

The plate map is included below. Note that the sample ID's in this plate map are the well numbers of the gDNA from the original gDNA plate. The plate map for gDNA (below) will have the tube ID for this project. 

![POC histone plate map](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/rflp/poc-histone-pcr-map.png?raw=true)

![gDNA plate map 38 39](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/extractions/extraction_gDNA_plate038_plate039.jpeg?raw=true)

#### 20241118 

After PCR for POC Histone, I ran the restriction enzyme incubation for the RFLP. I generated a master mix containing rCutSmart Buffer (1.9 uL per reaction) and XholRE (1.9 uL per reaction) with 15 uL of PCR product in each reaction. 

I added the enzymes and sample to each well with 15 uL of water added to a new negative control (NEG2).  

Samples were incubated at 37°C for 1 hr then at 65°C for 20 minutes with a 10°C hold in a thermocycler.  

Samples were loaded in the same positions as the PCR plate. Sample ID in the plate maps indicates the well from the original gDNA plate. 

![rflp plate map](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/rflp/rflp-map.png?raw=true)  

I then looked at the PCR product on a gel after the enzyme incubation. If there is a cut, the sample is *P. grandis* and if the enzyme does not cut, the sample is *P. meandrina*. We are only going to use this to look at samples where the mtORF sequence analysis indicates that the sample is in haplotype 1a (grandis/meandrina complex) as described in linked protocols and papers above.  

I ran 2% gels for 90 min at 80V to view the results. I ran this with 3 gels to fit all samples.  

![RFLP gel 1](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/rflp/20241118_rflp1.jpeg?raw=true)

![RFLP gel 2](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/rflp/20241118_rflp2.jpeg?raw=true)

![RFLP gel 3](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/rflp/20241118_rflp3.jpeg?raw=true)

There are some samples that cut very clearly and negative controls have no bands. The positive control did not show up, which is not surprising because of the very low DNA available in the original tube. There are also some higher bands in some samples. The variation in banding pattern may not be surprising because we ran this on all samples, which may contain several *Pocillopora* species and not just *P. meandrina* and *P. grandis*, as it was designed for.  

We will use these results to look for the cut bands for samples identified at haplotype 1a when sequence data is returned. The RFLP bands will not be used to identify species other than *grandis/meandrina*.  

The enzyme did successfully cut and for samples we had preliminary sequences for, it did cut only those identified as haplotype 1a.  

## Porites 

### H2-H4 sequencing 

#### 20241117

Hollie prepared the *Porites* H2-H4 PCR for all *Porites* samples using the protocol linked above. The gDNA for these samples came from plates 40-41 of gDNA extractions. 

![gDNA plate map 40 41](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/extractions/extraction_gDNA_plate040_plate041.jpeg?raw=true)

We PCR'd samples from Plate 40 B6-H12 and 41 A1-A2 for 81 total samples. 

| Component                   | 1 rxn  | 90 rxns |
|-----------------------------|--------|---------|
| EmeraldAmp 2X TT master mix | 12.6uL | 1134ul  |
| zH2AH4f F primer (10uM)     | 0.3uL  | 27uL    |
| zH2FrR R primer (10uM)      | 0.3uL  | 27uL    |
| Ultrapure H20               | 10.8uL | 972uL   |
| Template DNA                | 1uL    |         |
|                             | 25uL   | 23.99uL |

The PCR protocol was as follows:  

- 96°C for 120 sec [1 cycle]
- 96°C for 20 sec, 58.5°C for 20 sec, 72°C for 90 sec, 72°C for 90 sec [34 cycles]
- 72°C for 5 min [1 cycle]
- Hold at 4°C 

We used a negative control of water only and used a positive control of a *P. evermanni* sample (JA 491 9/25/24).  

Samples held overnight on the thermocycler at 4°C. We will run a gel in the morning.  

The plate map sample ID refers to the tube ID, which is also what is recorded in the gDNA plate maps above. 

![POR H2 plate map](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/por-H2-PCR-map.png?raw=true)

#### 20241118 

I ran a gel to look at the PCR product. I ran a 1.5% gel at 80V for 90 min with 3uL of sample. I also added a 1Kb ladder. I ran three gels to fit all samples.   

![POR H2 gel 1](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/POR-H2_gel1.jpeg?raw=true)

![POR H2 gel 2](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/POR-H2_gel2.jpeg?raw=true)

![POR H2 gel 3](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/POR-H2_gel2.jpeg?raw=true)

Notes from gel results:  

- Wells A1, A2, A9-A12 had no bands 
- All other samples have a clear band 
- No bands in negative control 
- Postive control has a clear band 

I then looked at the gDNA data and some of the samples had >10 ng/uL DNA concentrations, which is about twice of that of samples that amplified well. I then decided to re-amplify the samples that failed with half of the template DNA, because the DNA concentration may have been too high for this PCR reaction.  

I therefore did another round of PCR for the failed samples with 1/2 of the DNA volume (0.5 uL of DNA and 0.5 uL of water in each reaction).  

| Component                   | 1 rxn  | 10 rxns |
|-----------------------------|--------|---------|
| EmeraldAmp 2X TT master mix | 12.55uL | 125.54ul  |
| zH2AH4f F primer (10uM)     | 0.32uL  | 3.2uL    |
| zH2FrR R primer (10uM)      | 0.32uL  | 3.2uL    |
| Ultrapure H20               | 11.3uL | 113uL   |
| Template DNA                | 0.5uL    |         |
|                             | 25uL   | 23.99uL |

I used sample #75, which was a sample that successfully amplified yesterday as a postive control. Water was used as a negative control.  

If these successfully amplify, they will replace the failed sample wells in the PCR product plate to submit for sequencing.  

Ran in strip tubes labeled with tube ID. PCR ran as described above and I'll run a gel in the morning.  

#### 20241119 

I ran a 1.5% gel at 80V for 90min with 4uL of sample to check PCR product from samples that were re-done yesterday. There are 6 samples and 1 positive control, with 1 negative control.  

![POR H2 re do gel](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/POR-H2_gel4.jpeg?raw=true) 

All samples have bands - reducing DNA input worked! Positive control amplified and negative controls have no band.  

We can now proceed with cleaning and preparation for sequencing for all *Porites* samples.  

I then performed a clean up using the [Zymo Zr-96 DNA clean up kit]() in a 96-well plate format. Note that during this process I removed the failed samples from the first round of PCR and replaced with the successful PCR product for those samples.  

1. Add 100 uL DNA binding buffer to PCR samples, mix, and transfer to silicon A plate on top of column plate. 
2. Centrifuge at 3000g for 5 min. 
3. Add 300 uL of DNA wash buffer (100% ethanol added to buffer by HP) to each well. 
4. Centrifugure at 3000g for 5 min. 
5. Add 300 uL of DNA wash buffer again to each well. 
4. Centrifugure at 3000g for 5 min. 
5. Add 40uL nuclease free water to each well. 
6. Transfer the silicon A plate to the elution plate. 
7. Centrifuge at 3000g for 3 min. 
8. DNA is now in the elution plate. 
9. Store at -20°C until sending for sequencing. 
10. Discard of waste in hood.  

I then assembled boxes for each of the projects for sequencing. Project 1 is the *Pocillopora* mtORF samples as described above. Project 2 is the *Porites* H2 samples. To prepare for sequencing, I added the plate of clean DNA to a labeled box and added in a tube of 3.2 uM primers. We decided to sequence both the forward and reverse due to the length of the sequence desired.  

Sample information is as follows for sequencing. Sample name is a running number for Janet and the Sample Name includes the actual tube ID that I will use to track to sample.  

Forward primer:  

| Sample ID | Sample Name      | Well | Template Type | PCR Template Volume (µl) | H20 Volume (µl) | Primer Volume (µl) | Primer Name | Notes                                                                                                                      |
|-----------|------------------|------|---------------|--------------------------|-----------------|--------------------|-------------|----------------------------------------------------------------------------------------------------------------------------|
| AH038     | 68               | A1   | PCR           |                          |                 |                    | zH2AH4f     | |
| AH039     | 124              | B1   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH040     | 138              | C1   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH041     | 81               | D1   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH042     | 225              | E1   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH043     | 232              | F1   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH044     | 251              | G1   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H1   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH045     | 69               | A2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH046     | 127              | B2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH047     | 140              | C2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH048     | 83               | D2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH049     | 226              | E2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH050     | 233              | F2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH051     | 192              | G2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H2   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH052     | 71               | A3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH053     | 128              | B3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH054     | 141              | C3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH055     | 86               | D3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH056     | 244              | E3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH057     | 234              | F3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH058     | 215              | G3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H3   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH059     | 75               | A4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH060     | 129              | B4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH061     | 65               | C4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH062     | 87               | D4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH063     | 245              | E4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH064     | 235              | F4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH065     | 217              | G4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H4   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH066     | 80               | A5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH067     | 130              | B5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH068     | 67               | C5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH069     | 90               | D5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH070     | 246              | E5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH071     | 238              | F5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH072     | 218              | G5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H5   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH073     | 82               | A6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH074     | 131              | B6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH075     | 70               | C6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH076     | 91               | D6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH077     | 247              | E6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH078     | 239              | F6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH079     | 220              | G6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H6   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH080     | 84               | A7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH081     | 132              | B7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH082     | 72               | C7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH083     | 92               | D7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH084     | 193              | E7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH085     | 240              | F7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH086     | 237              | G7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H7   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH087     | 85               | A8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH088     | 133              | B8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH089     | 73               | C8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH090     | 93               | D8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH091     | 216              | E8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH092     | 242              | F8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH093     | 241              | G8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H8   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH094     | 88               | A9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH095     | 134              | B9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH096     | 74               | C9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH097     | 221              | D9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH098     | 219              | E9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH099     | 243              | F9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH100     | 66               | G9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H9   | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH101     | 94               | A10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH102     | 135              | B10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH103     | 76               | C10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH104     | 222              | D10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH105     | 227              | E10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH106     | 248              | F10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH107     | Positive-JA491   | G10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| EMPTY     | NA               | H10  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH108     | 95               | A11  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH109     | 136              | B11  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH110     | 77               | C11  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH111     | 223              | D11  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH112     | 230              | E11  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH113     | 249              | F11  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| NA        | negative-control | G11  | PCR           |                          |                 |                    | zH2AH4f     | PCR negative, do not sequence                                                                                              |
| EMPTY     | NA               | H11  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH115     | 96               | A12  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH116     | 137              | B12  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH117     | 79               | C12  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH118     | 224              | D12  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH119     | 231              | E12  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| AH120     | 250              | F12  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |
| NA        | negative-control | G12  | PCR           |                          |                 |                    | zH2AH4f     | PCR negative, do not sequence                                                                                              |
| EMPTY     | NA               | H12  | PCR           |                          |                 |                    | zH2AH4f     |                                                                                                                            |

Reverse primer:   

| Sample ID | Sample Name      | Well | Template Type | PCR Template Volume (µl) | H20 Volume (µl) | Primer Volume (µl) | Primer Name | Notes                                                                                                                      |
|-----------|------------------|------|---------------|--------------------------|-----------------|--------------------|-------------|----------------------------------------------------------------------------------------------------------------------------|
| AH121     | 68               | A1   | PCR           |                          |                 |                    | zH4Fr       |  |
| AH122     | 124              | B1   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH123     | 138              | C1   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH124     | 81               | D1   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH125     | 225              | E1   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH126     | 232              | F1   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH127     | 251              | G1   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H1   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH128     | 69               | A2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH129     | 127              | B2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH130     | 140              | C2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH131     | 83               | D2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH132     | 226              | E2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH133     | 233              | F2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH134     | 192              | G2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H2   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH135     | 71               | A3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH136     | 128              | B3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH137     | 141              | C3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH138     | 86               | D3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH139     | 244              | E3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH140     | 234              | F3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH141     | 215              | G3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H3   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH142     | 75               | A4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH143     | 129              | B4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH144     | 65               | C4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH145     | 87               | D4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH146     | 245              | E4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH147     | 235              | F4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH148     | 217              | G4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H4   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH149     | 80               | A5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH150     | 130              | B5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH151     | 67               | C5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH152     | 90               | D5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH153     | 246              | E5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH154     | 238              | F5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH155     | 218              | G5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H5   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH156     | 82               | A6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH157     | 131              | B6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH158     | 70               | C6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH159     | 91               | D6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH160     | 247              | E6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH161     | 239              | F6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH162     | 220              | G6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H6   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH162     | 84               | A7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH163     | 132              | B7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH164     | 72               | C7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH165     | 92               | D7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH166     | 193              | E7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH167     | 240              | F7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH168     | 237              | G7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H7   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH169     | 85               | A8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH170     | 133              | B8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH171     | 73               | C8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH172     | 93               | D8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH173     | 216              | E8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH174     | 242              | F8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH175     | 241              | G8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H8   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH176     | 88               | A9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH177     | 134              | B9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH178     | 74               | C9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH179     | 221              | D9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH180     | 219              | E9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH181     | 243              | F9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH182     | 66               | G9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H9   | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH183     | 94               | A10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH184     | 135              | B10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH185     | 76               | C10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH186     | 222              | D10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH187     | 227              | E10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH188     | 248              | F10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH189     | Positive-JA491   | G10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| EMPTY     | NA               | H10  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH190     | 95               | A11  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH191     | 136              | B11  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH192     | 77               | C11  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH193     | 223              | D11  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH194     | 230              | E11  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH195     | 249              | F11  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| NA        | negative-control | G11  | PCR           |                          |                 |                    | zH4Fr       | PCR negative, do not sequence                                                                                              |
| EMPTY     | NA               | H11  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH196     | 96               | A12  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH197     | 137              | B12  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH198     | 79               | C12  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH199     | 224              | D12  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH200     | 231              | E12  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| AH201     | 250              | F12  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |
| NA        | negative-control | G12  | PCR           |                          |                 |                    | zH4Fr       | PCR negative, do not sequence                                                                                              |
| EMPTY     | NA               | H12  | PCR           |                          |                 |                    | zH4Fr       |                                                                                                                            |

#### 20241120  

Samples were dropped off by Hollie today for sequencing.  

## Acropora

### Primer selection 

#### 20241119 

Hollie and I talked about primers to use for *Acropora* species identification. Hollie previously tested some of the primers from the literature. We already have primers in the lab for PaxC (PaxC_intron_FP1 forward and PaxC_intron_RP1 reverse) and CRf/CO3r (CRF forward and CO3r reverse) so we will try those first.  

PaxC_intron:  

- [van Oppen et al. 2004](https://link.springer.com/article/10.1007/s00227-003-1188-3) and [van Oppen et al. 2001](https://academic.oup.com/mbe/article/18/7/1315/992341)

CRf/CO3r:  

- Mitochondrial putative control (933+ bp) + 83 bp of cytochrome oxidase III as in [Vollmer et al. 2002](https://www.science.org/doi/full/10.1126/science.1069524)

### CRf/CO3r sequencing and PaxC_intron_FP1/PaxC_intron_RP1 sequencing 

I prepared two PCR plates - one for each primer set.  

CRf/CO3r:  

| Component                   | 1 rxn   | 95 rxns   |
|-----------------------------|---------|-----------|
| EmeraldAmp 2X TT master mix | 12.55uL | 1192.25ul |
| Ultrapure H20               | 10.80uL | 1026uL    |
| CRf primer (10uM)           | 0.32uL  | 30.4uL    |
| CO3r primer (10uM)          | 0.32uL  | 30.4uL    |
| Template DNA                | 1uL     |           |
|                             | 25uL    | 23.99uL   |

PaxC:  

| Component                   | 1 rxn   | 95 rxns   |
|-----------------------------|---------|-----------|
| EmeraldAmp 2X TT master mix | 12.55uL | 1192.25ul |
| Ultrapure H20               | 10.80uL | 1026uL    |
| PaxC FP1 primer (10uM)      | 0.32uL  | 30.4uL    |
| PaxC RP1 primer (10uM)      | 0.32uL  | 30.4uL    |
| Template DNA                | 1uL     |           |
|                             | 25uL    | 23.99uL   |


I used a Becker Heatwave *Acropora pulchra* as a postive control (no. 24 gDNA from 10/29/23). Water was used as a negative control in both plates.  

The plate maps for these PCRs shows the sample ID as the well from the original gDNA plates. 

![acr PCR map](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/acr-PCR-map.png?raw=true) 

The gDNA plates are as follows.  

![plate 39 gDNA map](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/extractions/extraction_gDNA_plate038_plate039.jpeg?raw=true) 
![plate 40 gDNA map](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/extractions/extraction_gDNA_plate040_plate041.jpeg?raw=true) 

The PCR protocol ("acro" on Putnam account) was the same for both primers.  

- 95°C for 3 min [1 cycle]
- 94°C for 30 sec, 53°C for 30 sec, 72°C for 1 min [35 cycles] *melting temperature chosen based on lowest primer melting temperature* 
- 72°C for 5 min 
- Hold at 4°C 

[Images of primer labels can be found on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/tree/main/data/dna/pcr/acr-primers).  

I then ran a gel for the CRf/CO3r primer set. I'll run the PaxC gel in the morning. 

I ran PCR product on a 2% gel for 90 min at 80V.  

![acr CRf gel 1](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/ACR-CRf_gel1.jpeg?raw=true)
![acr CRf gel 2](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/ACR-CRf_gel2.jpeg?raw=true)
![acr CRf gel 3](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/ACR-CRf_gel3.jpeg?raw=true)

The gel product looks really nice. No bands in the negative controls, a band in the positive control. There are bands present for all other samples. Sample AH229 had a faint band - but this is ok because it was an accidental *Pocillopora* sample that was included! All other samples look good.  

#### 20241120 

I ran a gel for the PaxC PCR products using a 2% gel at 80V for 90 min.  

![acr PaxC gel 1](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/ACR-PaxC_gel1.jpeg?raw=true)
![acr PaxC gel 2](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/ACR-PaxC_gel2.jpeg?raw=true)
![acr PaxC gel 3](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/pcr/ACR-PaxC_gel3.jpeg?raw=true)

Notes from the gel results:  

- Very interesting! The bands all have varying lengths. It will be interesting to see if band length corresponds with species after sequencing analyses.  
- Negative controls have no bad, the positive control as a band. 
- Interestingly, the *Pocillopora* sample amplified well (we won't be analyzing this sample since it was an accidental inclusion).  

All samples look good, we will move forward with cleaning and sample preps.  

I cleaned the plates following the same protocol as above for *Porites* samples. Sample plates were labeled and stored in the freezer with one plate for each primer set.  

We will sequence both the forward and reverse for the CRf/CO3r primer set and only the forward for the PaxC (smaller length).  

Sample info is as follows.  

CRf/CO3r forward:  

| Sample ID | Sample Name | Well | Template Type | PCR Template Volume (µl) | H20 Volume (µl) | Primer Volume (µl) | Primer Name |
|-----------|-------------|------|---------------|--------------------------|-----------------|--------------------|-------------|
| AH202     | 13          | A1   | PCR           |                          |                 |                    | CRf         |
| AH203     | 102         | B1   | PCR           |                          |                 |                    | CRf         |
| AH204     | 4           | C1   | PCR           |                          |                 |                    | CRf         |
| AH205     | 26          | D1   | PCR           |                          |                 |                    | CRf         |
| AH206     | 151         | E1   | PCR           |                          |                 |                    | CRf         |
| AH207     | 163         | F1   | PCR           |                          |                 |                    | CRf         |
| AH208     | 171         | G1   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H1   | PCR           |                          |                 |                    | CRf         |
| AH209     | 14          | A2   | PCR           |                          |                 |                    | CRf         |
| AH210     | 103         | B2   | PCR           |                          |                 |                    | CRf         |
| AH211     | 6           | C2   | PCR           |                          |                 |                    | CRf         |
| AH212     | 27          | D2   | PCR           |                          |                 |                    | CRf         |
| AH213     | 152         | E2   | PCR           |                          |                 |                    | CRf         |
| AH214     | 164         | F2   | PCR           |                          |                 |                    | CRf         |
| AH215     | 172         | G2   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H2   | PCR           |                          |                 |                    | CRf         |
| AH216     | 15          | A3   | PCR           |                          |                 |                    | CRf         |
| AH217     | 104         | B3   | PCR           |                          |                 |                    | CRf         |
| AH218     | 7           | C3   | PCR           |                          |                 |                    | CRf         |
| AH219     | 30          | D3   | PCR           |                          |                 |                    | CRf         |
| AH220     | 153         | E3   | PCR           |                          |                 |                    | CRf         |
| AH221     | 166         | F3   | PCR           |                          |                 |                    | CRf         |
| AH222     | 174         | G3   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H3   | PCR           |                          |                 |                    | CRf         |
| AH223     | 16          | A4   | PCR           |                          |                 |                    | CRf         |
| AH224     | 105         | B4   | PCR           |                          |                 |                    | CRf         |
| AH225     | 8           | C4   | PCR           |                          |                 |                    | CRf         |
| AH226     | 31          | D4   | PCR           |                          |                 |                    | CRf         |
| AH227     | 154         | E4   | PCR           |                          |                 |                    | CRf         |
| AH228     | 168         | F4   | PCR           |                          |                 |                    | CRf         |
| AH229     | 175         | G4   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H4   | PCR           |                          |                 |                    | CRf         |
| AH230     | 20          | A5   | PCR           |                          |                 |                    | CRf         |
| AH231     | 106         | B5   | PCR           |                          |                 |                    | CRf         |
| AH232     | 9           | C5   | PCR           |                          |                 |                    | CRf         |
| AH233     | 32          | D5   | PCR           |                          |                 |                    | CRf         |
| AH234     | 155         | E5   | PCR           |                          |                 |                    | CRf         |
| AH235     | 173         | F5   | PCR           |                          |                 |                    | CRf         |
| AH236     | 177         | G5   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H5   | PCR           |                          |                 |                    | CRf         |
| AH237     | 28          | A6   | PCR           |                          |                 |                    | CRf         |
| AH238     | 107         | B6   | PCR           |                          |                 |                    | CRf         |
| AH239     | 12          | C6   | PCR           |                          |                 |                    | CRf         |
| AH240     | 142         | D6   | PCR           |                          |                 |                    | CRf         |
| AH241     | 156         | E6   | PCR           |                          |                 |                    | CRf         |
| AH242     | 176         | F6   | PCR           |                          |                 |                    | CRf         |
| AH243     | 229         | G6   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H6   | PCR           |                          |                 |                    | CRf         |
| AH244     | 29          | A7   | PCR           |                          |                 |                    | CRf         |
| AH245     | 108         | B7   | PCR           |                          |                 |                    | CRf         |
| AH246     | 17          | C7   | PCR           |                          |                 |                    | CRf         |
| AH247     | 143         | D7   | PCR           |                          |                 |                    | CRf         |
| AH248     | 157         | E7   | PCR           |                          |                 |                    | CRf         |
| AH249     | 178         | F7   | PCR           |                          |                 |                    | CRf         |
| AH250     | 1           | G7   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H7   | PCR           |                          |                 |                    | CRf         |
| AH251     | 97          | A8   | PCR           |                          |                 |                    | CRf         |
| AH252     | 109         | B8   | PCR           |                          |                 |                    | CRf         |
| AH253     | 18          | C8   | PCR           |                          |                 |                    | CRf         |
| AH254     | 144         | D8   | PCR           |                          |                 |                    | CRf         |
| AH255     | 158         | E8   | PCR           |                          |                 |                    | CRf         |
| AH256     | 148         | F8   | PCR           |                          |                 |                    | CRf         |
| AH257     | 5           | G8   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H8   | PCR           |                          |                 |                    | CRf         |
| AH258     | 98          | A9   | PCR           |                          |                 |                    | CRf         |
| AH259     | 110         | B9   | PCR           |                          |                 |                    | CRf         |
| AH260     | 19          | C9   | PCR           |                          |                 |                    | CRf         |
| AH261     | 145         | D9   | PCR           |                          |                 |                    | CRf         |
| AH262     | 159         | E9   | PCR           |                          |                 |                    | CRf         |
| AH263     | 150         | F9   | PCR           |                          |                 |                    | CRf         |
| AH264     | 10          | G9   | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H9   | PCR           |                          |                 |                    | CRf         |
| AH265     | 99          | A10  | PCR           |                          |                 |                    | CRf         |
| AH266     | 111         | B10  | PCR           |                          |                 |                    | CRf         |
| AH267     | 21          | C10  | PCR           |                          |                 |                    | CRf         |
| AH268     | 146         | D10  | PCR           |                          |                 |                    | CRf         |
| AH269     | 160         | E10  | PCR           |                          |                 |                    | CRf         |
| AH270     | 165         | F10  | PCR           |                          |                 |                    | CRf         |
| AH271     | 11          | G10  | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H10  | PCR           |                          |                 |                    | CRf         |
| AH272     | 100         | A11  | PCR           |                          |                 |                    | CRf         |
| AH273     | 2           | B11  | PCR           |                          |                 |                    | CRf         |
| AH274     | 24          | C11  | PCR           |                          |                 |                    | CRf         |
| AH275     | 147         | D11  | PCR           |                          |                 |                    | CRf         |
| AH276     | 161         | E11  | PCR           |                          |                 |                    | CRf         |
| AH277     | 167         | F11  | PCR           |                          |                 |                    | CRf         |
| NA        | NA          | G11  | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H11  | PCR           |                          |                 |                    | CRf         |
| AH278     | 101         | A12  | PCR           |                          |                 |                    | CRf         |
| AH279     | 3           | B12  | PCR           |                          |                 |                    | CRf         |
| AH280     | 25          | C12  | PCR           |                          |                 |                    | CRf         |
| AH281     | 149         | D12  | PCR           |                          |                 |                    | CRf         |
| AH282     | 162         | E12  | PCR           |                          |                 |                    | CRf         |
| AH283     | 170         | F12  | PCR           |                          |                 |                    | CRf         |
| AH284     | Becker24    | G12  | PCR           |                          |                 |                    | CRf         |
| EMPTY     | NA          | H12  | PCR           |                          |                 |                    | CRf         |

CRf/CO3r reverse:  

| Sample ID | Sample Name | Well | Template Type | PCR Template Volume (µl) | H20 Volume (µl) | Primer Volume (µl) | Primer Name |
|-----------|-------------|------|---------------|--------------------------|-----------------|--------------------|-------------|
| AH285     | 13          | A1   | PCR           |                          |                 |                    | CO3r        |
| AH286     | 102         | B1   | PCR           |                          |                 |                    | CO3r        |
| AH287     | 4           | C1   | PCR           |                          |                 |                    | CO3r        |
| AH288     | 26          | D1   | PCR           |                          |                 |                    | CO3r        |
| AH289     | 151         | E1   | PCR           |                          |                 |                    | CO3r        |
| AH290     | 163         | F1   | PCR           |                          |                 |                    | CO3r        |
| AH291     | 171         | G1   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H1   | PCR           |                          |                 |                    | CO3r        |
| AH292     | 14          | A2   | PCR           |                          |                 |                    | CO3r        |
| AH293     | 103         | B2   | PCR           |                          |                 |                    | CO3r        |
| AH294     | 6           | C2   | PCR           |                          |                 |                    | CO3r        |
| AH295     | 27          | D2   | PCR           |                          |                 |                    | CO3r        |
| AH296     | 152         | E2   | PCR           |                          |                 |                    | CO3r        |
| AH297     | 164         | F2   | PCR           |                          |                 |                    | CO3r        |
| AH298     | 172         | G2   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H2   | PCR           |                          |                 |                    | CO3r        |
| AH299     | 15          | A3   | PCR           |                          |                 |                    | CO3r        |
| AH300     | 104         | B3   | PCR           |                          |                 |                    | CO3r        |
| AH301     | 7           | C3   | PCR           |                          |                 |                    | CO3r        |
| AH302     | 30          | D3   | PCR           |                          |                 |                    | CO3r        |
| AH303     | 153         | E3   | PCR           |                          |                 |                    | CO3r        |
| AH304     | 166         | F3   | PCR           |                          |                 |                    | CO3r        |
| AH305     | 174         | G3   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H3   | PCR           |                          |                 |                    | CO3r        |
| AH306     | 16          | A4   | PCR           |                          |                 |                    | CO3r        |
| AH307     | 105         | B4   | PCR           |                          |                 |                    | CO3r        |
| AH308     | 8           | C4   | PCR           |                          |                 |                    | CO3r        |
| AH309     | 31          | D4   | PCR           |                          |                 |                    | CO3r        |
| AH310     | 154         | E4   | PCR           |                          |                 |                    | CO3r        |
| AH311     | 168         | F4   | PCR           |                          |                 |                    | CO3r        |
| AH312     | 175         | G4   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H4   | PCR           |                          |                 |                    | CO3r        |
| AH313     | 20          | A5   | PCR           |                          |                 |                    | CO3r        |
| AH314     | 106         | B5   | PCR           |                          |                 |                    | CO3r        |
| AH315     | 9           | C5   | PCR           |                          |                 |                    | CO3r        |
| AH316     | 32          | D5   | PCR           |                          |                 |                    | CO3r        |
| AH317     | 155         | E5   | PCR           |                          |                 |                    | CO3r        |
| AH318     | 173         | F5   | PCR           |                          |                 |                    | CO3r        |
| AH319     | 177         | G5   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H5   | PCR           |                          |                 |                    | CO3r        |
| AH320     | 28          | A6   | PCR           |                          |                 |                    | CO3r        |
| AH321     | 107         | B6   | PCR           |                          |                 |                    | CO3r        |
| AH322     | 12          | C6   | PCR           |                          |                 |                    | CO3r        |
| AH323     | 142         | D6   | PCR           |                          |                 |                    | CO3r        |
| AH324     | 156         | E6   | PCR           |                          |                 |                    | CO3r        |
| AH325     | 176         | F6   | PCR           |                          |                 |                    | CO3r        |
| AH326     | 229         | G6   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H6   | PCR           |                          |                 |                    | CO3r        |
| AH327     | 29          | A7   | PCR           |                          |                 |                    | CO3r        |
| AH328     | 108         | B7   | PCR           |                          |                 |                    | CO3r        |
| AH329     | 17          | C7   | PCR           |                          |                 |                    | CO3r        |
| AH330     | 143         | D7   | PCR           |                          |                 |                    | CO3r        |
| AH331     | 157         | E7   | PCR           |                          |                 |                    | CO3r        |
| AH332     | 178         | F7   | PCR           |                          |                 |                    | CO3r        |
| AH333     | 1           | G7   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H7   | PCR           |                          |                 |                    | CO3r        |
| AH334     | 97          | A8   | PCR           |                          |                 |                    | CO3r        |
| AH335     | 109         | B8   | PCR           |                          |                 |                    | CO3r        |
| AH336     | 18          | C8   | PCR           |                          |                 |                    | CO3r        |
| AH337     | 144         | D8   | PCR           |                          |                 |                    | CO3r        |
| AH338     | 158         | E8   | PCR           |                          |                 |                    | CO3r        |
| AH339     | 148         | F8   | PCR           |                          |                 |                    | CO3r        |
| AH340     | 5           | G8   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H8   | PCR           |                          |                 |                    | CO3r        |
| AH341     | 98          | A9   | PCR           |                          |                 |                    | CO3r        |
| AH342     | 110         | B9   | PCR           |                          |                 |                    | CO3r        |
| AH343     | 19          | C9   | PCR           |                          |                 |                    | CO3r        |
| AH344     | 145         | D9   | PCR           |                          |                 |                    | CO3r        |
| AH345     | 159         | E9   | PCR           |                          |                 |                    | CO3r        |
| AH346     | 150         | F9   | PCR           |                          |                 |                    | CO3r        |
| AH347     | 10          | G9   | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H9   | PCR           |                          |                 |                    | CO3r        |
| AH348     | 99          | A10  | PCR           |                          |                 |                    | CO3r        |
| AH349     | 111         | B10  | PCR           |                          |                 |                    | CO3r        |
| AH350     | 21          | C10  | PCR           |                          |                 |                    | CO3r        |
| AH351     | 146         | D10  | PCR           |                          |                 |                    | CO3r        |
| AH352     | 160         | E10  | PCR           |                          |                 |                    | CO3r        |
| AH353     | 165         | F10  | PCR           |                          |                 |                    | CO3r        |
| AH354     | 11          | G10  | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H10  | PCR           |                          |                 |                    | CO3r        |
| AH355     | 100         | A11  | PCR           |                          |                 |                    | CO3r        |
| AH356     | 2           | B11  | PCR           |                          |                 |                    | CO3r        |
| AH357     | 24          | C11  | PCR           |                          |                 |                    | CO3r        |
| AH358     | 147         | D11  | PCR           |                          |                 |                    | CO3r        |
| AH359     | 161         | E11  | PCR           |                          |                 |                    | CO3r        |
| AH360     | 167         | F11  | PCR           |                          |                 |                    | CO3r        |
| NA        | NA          | G11  | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H11  | PCR           |                          |                 |                    | CO3r        |
| AH361     | 101         | A12  | PCR           |                          |                 |                    | CO3r        |
| AH362     | 3           | B12  | PCR           |                          |                 |                    | CO3r        |
| AH363     | 25          | C12  | PCR           |                          |                 |                    | CO3r        |
| AH364     | 149         | D12  | PCR           |                          |                 |                    | CO3r        |
| AH365     | 162         | E12  | PCR           |                          |                 |                    | CO3r        |
| AH366     | 170         | F12  | PCR           |                          |                 |                    | CO3r        |
| AH367     | Becker24    | G12  | PCR           |                          |                 |                    | CO3r        |
| EMPTY     | NA          | H12  | PCR           |                          |                 |                    | CO3r        |

PaxC forward:  

| Sample ID | Sample Name | Well | Template Type | PCR Template Volume (µl) | H20 Volume (µl) | Primer Volume (µl) | Primer Name     |
|-----------|-------------|------|---------------|--------------------------|-----------------|--------------------|-----------------|
| AH368     | 13          | A1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH369     | 102         | B1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH370     | 4           | C1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH371     | 26          | D1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH372     | 151         | E1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH373     | 163         | F1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH374     | 171         | G1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H1   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH375     | 14          | A2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH376     | 103         | B2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH377     | 6           | C2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH378     | 27          | D2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH379     | 152         | E2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH380     | 164         | F2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH381     | 172         | G2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H2   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH382     | 15          | A3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH383     | 104         | B3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH384     | 7           | C3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH385     | 30          | D3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH386     | 153         | E3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH387     | 166         | F3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH388     | 174         | G3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H3   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH389     | 16          | A4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH390     | 105         | B4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH391     | 8           | C4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH392     | 31          | D4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH393     | 154         | E4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH394     | 168         | F4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH395     | 175         | G4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H4   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH396     | 20          | A5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH397     | 106         | B5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH398     | 9           | C5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH399     | 32          | D5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH400     | 155         | E5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH401     | 173         | F5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH402     | 177         | G5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H5   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH403     | 28          | A6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH404     | 107         | B6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH405     | 12          | C6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH406     | 142         | D6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH407     | 156         | E6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH408     | 176         | F6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH409     | 11          | G6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H6   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH410     | 29          | A7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH411     | 108         | B7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH412     | 17          | C7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH413     | 143         | D7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH414     | 157         | E7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH415     | 178         | F7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH416     | 229         | G7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H7   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH417     | 97          | A8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH418     | 109         | B8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH419     | 18          | C8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH420     | 144         | D8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH421     | 158         | E8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH422     | 148         | F8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH423     | 1           | G8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H8   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH424     | 98          | A9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH425     | 110         | B9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH426     | 19          | C9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH427     | 145         | D9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH428     | 159         | E9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH429     | 150         | F9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH430     | 5           | G9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H9   | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH431     | 99          | A10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH432     | 111         | B10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH433     | 21          | C10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH434     | 146         | D10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH435     | 160         | E10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH436     | 165         | F10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH437     | 10          | G10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H10  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH438     | 100         | A11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH439     | 2           | B11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH440     | 24          | C11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH441     | 147         | D11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH442     | 161         | E11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH443     | 167         | F11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| NA        | NA          | G11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H11  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH444     | 101         | A12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH445     | 3           | B12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH446     | 25          | C12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH447     | 149         | D12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH448     | 162         | E12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH449     | 170         | F12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| AH450     | Becker24    | G12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |
| EMPTY     | NA          | H12  | PCR           |                          |                 |                    | PaxC_intron_FP1 |

#### 20241120 

Hollie dropped these off for sequencing.  

# Next steps 

Next, I will analyze sequences to determine species ID and then decide how to proceed with sample processing for RNA and/or metabolomics.  