---
layout: post
title: ITS2 amplicon sequencing preparation protocol
date: '2025-04-01'
categories: Protocol
tags: PCR ITS2 Molecular
---

This protocol details sample preparation for next-generation sequencing of ITS2 amplicons for identification of Symbiodiniaceae in reef-building coral samples.  

Updated 8 April 2025  

# Overview 

This protocol amplifies the ITS2 region for amplicon sequencing to identify Symbiodiniaceae community composition in coral tissue samples (adult or larvae).  

In this protocol, we conduct the first PCR to amplify DNA and then submit to the sequencing facility for additional PCR and NGS sequencing.    

# Equipment and supplies 

*Equipment and supplies:*  

- [2X Phusion Mastermix](https://www.neb.com/en-us/products/m0531-phusion-high-fidelity-pcr-master-mix-with-hf-buffer)
- Forward primer (*ITSintfor2*) at 100uM: TCG TCG GCA GCG TCA GAT GTG TAT AAG AGA CAG GAA TTG CAG AAC TCC GTG
- Reverse primer (*ITS2_Reverse*) at 100uM: GTC TCG TGG GCT CGG AGA TGT GTA TAA GAG ACA GGG GAT CCA TAT GCT TAA GTT CAG CGG GT
- Loading Dye [NEB 6X Purple Loading Dye NEB Cat # B7024S](https://www.neb.com/en-us/products/b7024-gel-loading-dye-purple-6x)        
- Gel Stain [Biotium GelGreen Nucleic Acid Gel Stain, 10,000X in Water Fisher Cat NC9728313](https://www.fishersci.com/shop/products/gel-green-stain-5ml/NC9728313#?keyword=NC9728313)
- DNA Ladder [1kb Gel ladder](https://github.com/hputnam/Putnam_Lab_Notebook/blob/master/images/NEB_1kb_Ladder_N3232S.png?raw=true) and [100bp gel ladder](https://www.neb.com/en-us/products/n3231-100-bp-dna-ladder)
- Agarose and 1XTAE buffer for gel electrophoresis 
- Ultrapure water 
- PCR strip tubes and 1.5 mL tubes 

*Sample preparation:* 

This protocol requires extracted and quantified gDNA. We typically preserve samples in [Zymo Research DNA/RNA Shield R1100-250](https://github.com/hputnam/Putnam_Lab_Notebook/blob/master/images/Zymo_r1100-250_dna_rna_shield.pdf) and extract DNA and RNA with the [Zymo Research Quick-DNA™ Miniprep Plus Kit D4069](https://github.com/hputnam/Putnam_Lab_Notebook/blob/master/images/d4068_d4069_quick-dna_miniprep_plus_kit.pdf).   

*Additional protocols:*  

- This protocol is based on Emma Strand's [ITS2 protocol](https://emmastrand.github.io/EmmaStrand_Notebook/ITS2-Sequencing-Protocol/). 
- This protocol will also use the Putnam Lab [gel electrophoresis protocol](https://github.com/Putnam-Lab/Lab_Management/blob/master/Lab_Resources/DNA_RNA-protocols/Agarose-Gel-Protocol.md) and [Qubit protocol](https://emmastrand.github.io/EmmaStrand_Notebook/Qubit-Protocol/).

# Preparation and labeling tubes 

Before starting this protocol, prepare the following:  

- Sample list. If you are performing preparations in multiple batches/PCRs, randomize the sample order for processing to remove batch effects confounding with treatment groups.  
	- Prepare list of appropriate controls. Controls will include a **negative control** (input = nuclease-free water) and a **positive control** (gDNA from a previously successfully amplified and sequenced sample). These controls should be included in each batch. If you perform preparations over multiple days, include a positive and negative control each day or with each PCR.  
	
- Label 3 strip PCR tubes per sample (including negative and positive controls) ["Sample ID"] *Note that you can use 96 well plates for PCR, but I prefer strip tubes to reduce risk of pipetting errors and mixing up samples. Strip tubes are easier to track individual samples.*  
- Label 1 1.5 mL tube per sample for gDNA dilution (including positive control) ["Sample ID - Diluted gDNA"]
- Label 1 1.5 mL tube per sample for pooled PCR product (including negative and positive controls) ["Sample ID - Pooled PCR"]

# 1. Sample Dilution 

First, dilute gDNA from each of your samples to a consistent concentration. In this protocol, we dilute to 3-4 ng/uL. ITS2 amplifies readily, so input DNA into the first PCR can be at a low concentration. We dilute samples so that there is reduced bias towards high DNA concentration samples during PCR and subsequent sequencing. 

1. Calculate the volume required to obtain 3-4 ng/uL gDNA in a 10 uL total volume. In this protocol, we diluted to 4 ng/uL with the exception of samples that had <4 ng/uL available concentration. For these samples, we diluted to 3 ng/uL and  increased the input volume to obtain the same total input DNA for PCR for all samples (detailed below).  
2. Thaw gDNA on ice. 
3. Place nuclease free water on ice and thaw if necessary. 
4. Aliquot the required volume of water for each sample into the labeled gDNA dilution 1.5 mL tubes. 
5. For each sample, aliquot the required volume of gDNA into the labeled gDNA dilution 1.5 mL tubes. Use a separate pipette tip for each sample. 
6. After adding gDNA and water to each tube, vortex for ~5 sec and then spin down. 
7. Samples can be stored at -20°C until proceeding to the next step.  

This is now our template gDNA. This is a good pause point and can be done as far in advance as needed for your workflow.  

*Here is an example table of dilution calcuations.*  

| Sample | Group       | Treatment | Qubit Conc (ng/uL) | Nanodrop Conc (ng/uL) | DNA for dilution (uL) | Water | Total volume | Final Concentration (ng/ul) | Notes                                                                    |
|--------|-------------|-----------|--------------------|-----------------------|-----------------------|-------|--------------|-----------------------------|--------------------------------------------------------------------------|
| R55    | Cladocopium | Ambient   |               14.3 | NA                    |                   2.8 |   7.2 |           10 |                           4 |                                                                          |
| R57    | Cladocopium | Ambient   |              14.45 |                  15.8 |                   2.8 |   7.2 |           10 |                           4 |                                                                          |
| R58    | Cladocopium | Ambient   |              15.05 |                   8.4 |                   2.7 |   7.3 |           10 |                           4 |                                                                          |
| R70    | Wildtype    | Ambient   |               14.3 |                     3 |                   2.8 |   7.2 |           10 |                           4 |                                                                          |
| R68    | Wildtype    | Ambient   |               4.34 |                   9.7 |                   9.2 |   0.8 |           10 |                           4 |                                                                          |
| R69    | Wildtype    | Ambient   | 3                 |                     3 |                  10.0 |   0.0 |           10 |                           3 | adjust master mix to obtain 4ng input|

# 2. Master mix calculations  

### Calculate number of reactions  

Next, prepare master mix for the desired input volume of DNA. In this protocol, we diluted gDNA to 4 ng/uL for all samples except for 2 that had concentrations of 3 ng/uL. We therefore generated master mix to adjust the total DNA input to add a total of 4 ng of every sample into the PCR. Adjust this master mix volume if necessary to meet your dilution requirements and number of samples.   

Master mix will be prepared to allow for triplicate reactions for each sample. Reactions are performed in triplicate to 1) increase total PCR product for each sample, 2) provide individual replicates in the case that some reactions do not amplify or do not amplify well and 3) increase diversity of sequences amplified by chance.   

1. Prepare master mix components. Thaw the Phusion Master Mix on ice. Thaw the primers on ice. If required, hydrate lyophilized primers if not done so already. Thaw nuclease-free water on ice. Thaw diluted gDNA samples on ice. 
2. Calculate the volume of each regent needed by first calculating the total number of reactions requires. The total reactions will be the number of samples (including positive and negative control) x 3. Include an extra 10% volume for pipetting error.  

*Here is an example of calculating total reactions:*  

| Reactions:           |    |
|----------------------|----|
| Samples (AH)         | 17 |
| Samples (JA)         |  2 |
| Positive (Mcap2020)  |  1 |
| Negative (1 uL H20)  |  1 |
| TOTAL SAMPLES        | 21 |
| TOTAL REACTIONS      | 63 |
|                      |    |
| Added for 10% error  | 69 |

### Prepare master mix   

**Master mix - samples with 4 ng/uL diluted gDNA concentration:**  

Prepare master mix. These calculations are based on 25 uL reaction volume. Increase proportions to 33 uL if desired.  

This table shows the per reaction volume of each component multiplied by total number of reactions from the example above.  

1. First, label a 1.5-2 mL tube for the master mix (or a larger volume if required). 
2. Next, label two 1.5 mL tubes for dilution of primers (if required). This protocol calls for 10uM primers. Primers are often received or hydrated at 100uM. To dilute, add 1 part primer to 9 parts nuclease free water. For example, to make 10 total uL of primer stock, add 1 uL of the respective primer to 9 uL of H20. See the table below for the calculation for total primer stock needed to make the master mix. Do this for both the F and R primer separately. Vortex and spin down after diluting.  
3. Add in volume of Ultra pure/ nuclease free H20 required to the master mix tube. Use pipettes that are accurate for the volume required. For example, in the table below, we added 727.7 uL of water to the master mix. For this, I added 700 uL of water with a 100-1000 uL pipette, then 27 uL of water with a 20-200 uL pipette, followed by 0.7 uL of water with a 0.1-2 uL pipette.  
4. Next, add in the volume of Phusion Mastermix required.  
5. Finally, add in the 10uM F and R primers using the same approach above for pipetting for accuracy. 
6. The master mix is now ready to add to each reaction tube. After adding to each reaction tube, we will add the template gDNA.  

*Example master mix calculations:*  

| Component            | Per Rxn (uL) | Rxns | Total Volume (uL) |
|----------------------|--------------|------|-------------------|
| 2X Phusion Mastermix |         12.5 |   69 |             866.3 |
| F primer (10uM)      |          0.5 |   69 |              34.7 |
| R primer (10uM)      |          0.5 |   69 |              34.7 |
| Ultra Pure H20       |         10.5 |   69 |             727.7 |
| DNA (4ng)            |            1 |      |                   |
| TOTAL RXN VOLUME     |           25 |      |                   |

For this master mix, we will add 24 uL of master mix to each tube and 1 uL of template DNA (see below).  

**Master mix - samples with 3 ng/uL diluted gDNA concentration:**   

If you have samples with less than the target input template DNA concentration, you will adjust to increase the input DNA volume to meet the target and balance the total PCR volume by reduing water input. 

1. Make a separate master mix for low concentration samples using the table below as an example. Repeat the steps outlined above for calculating the number of reactions and mixing the master mix.  

*Here is an example of master mix for low concentration samples:*  

| Component            | Per Rxn (uL) | Rxns | Total Volume (uL) |
|----------------------|--------------|------|-------------------|
| 2X Phusion Mastermix |         12.5 |    7 |              82.5 |
| F primer (10uM)      |          0.5 |    7 |               3.3 |
| R primer (10uM)      |          0.5 |    7 |               3.3 |
| Ultra Pure H20       |        10.17 |    7 |            67.122 |
| DNA (3ng)            |         1.33 |      |                   |
| TOTAL RXN VOLUME     |           25 |      |                   |

For this master mix, we will add 23.67 uL of master mix to each tube and 1.33 uL of template DNA (see below).  

### Add master mix to reaction tubes and add template DNA  

1. Add 24 uL of master mix to each labeled reaction strip tube (or adjust master mix volume depending on sample concentration as described above).  
2. Add 1 uL of template diluted gDNA (or adjust template gDNA volume depending on sample concentration as described above). 
3. Briefly vortex each set of strip tubes to mix and spin down briefly.  
4. Strip tubes are now ready for PCR.  
5. Keep tubes on ice. 

# 3. PCR 

Conduct PCR to amplify the ITS2 region. Load all reaction tubes (including negative and positive controls) to the thermocycler. Run the following program (pre-programmed "ITS2" PCR protocol in the PPP lab):    

- 1 cycle at 95°C for 3 min 
- 35 cycles at 95°C for 30 sec, 52°C for 30 sec, and 72°C for 30 sec
- 1 cycle at 72°C for 2 min
- Hold at 4°C

PCR will run for approx. 1.5 hours. Samples will stay at 4°C on the thermocycler until running a QC gel or store samples at 4°C in fridge overnight.

This is a good stopping point if needed.   

# 4. Gel for PCR product QC 

To QC PCR product, run an electrophoresis gel of all reactions.   

### Prepare 2% agarose gel 
1. Prepare a 2% gel for a gel side appropriate for the number of reactions (1 reaction per well). 
2. Mix 1X TAE buffer and agarose (2% v/w) in a flask and heat for 1 minute, swirling every 20 sec (caution, it will be hot). After heating, let cool briefly then add 1uL GelGreen. Swirl to mix. 
3. Pour into gel box with comb and allow to set. Remove comb and set the gel with wells towards the black electrode and running towards the red electrode. 
4. Allow to fully set for about 30 min. 
5. Pour 1X TAE buffer to cover the gel with a thin layer of buffer. 

### Load samples into the gel 
1. Prepare a strip of parafilm and purple loading dye. 
2. Add ~3 dots of 1 uL of purple loading dye onto the parafilm. 
3. To each dot, ad 3uL of PCR product from each reaction. Then, pipette up and down gently to mix and then move 4 uL of product with loading dye into a well on the gel. 
4. Repeat this until all samples are loaded. I recommend doing this in sets of 3 to prevent the loading dye form drying on the parafilm. 
5. Include a 100Kb DNA ladder and a smaller DNA ladder (300bp or 100bp) to each well row. Add loading dye in the same way as described for samples unless already included with the ladder. 
6. Record a map of sample and well locations in your notebook. 

### Run gel 
1. After samples are loaded, run the gel at 80V-100V and 100 amps for 1.5 hours. We want to run this gel for a longer run time to draw out the smaller DNA fragment sizes to check for primer dimers and contamination. 
2. Store remaining PCR product at 4°C. 

### QC the gel

Here are some of the characteristics to look for in your gel and what they might mean:  

- Double bands: This could indicate contamination. If double bands are present in negative, positive, and sample controls, this is a sign of contamination. If you believe contamination is present, proceed with identifying the source. For example, you may want to start with a new water source and run a new PCR with a new master mix and new sample dilutions. If contamination is still present, it could indicate contamination in the master mix or primers. 
- Primer dimer: If you see a second band at about 100bp, this is likely due to primer dimer. If so, you will also see this in the negative control. If you have primer dimer, this can be removed using a size selection bead cleanup before sequencing your original PCR product.  
- No or weak amplification: If no band is present, samples may not have amplified. Return to your protocol in detail and ensure you have the correct reagents, primers, and dilutions. This could be due to incorrect master mix concentrations, old or bad primers, incorrect primers, incorrect PCR protocol, or several other reasons. 
- No amplificaiton or double banding in only *some* of the technical replicate reactions for each sample. In your QC, if you see that some replicates of some samples did not amplify or have problems, note this and you will decide whether or not to include them in the PCR pool for sequencing.  

Repeat troubleshooting and PCR as necessary.  

# 5. Pooling PCR product 

After QC'ing gel, we will pool PCR product from the technical replicates for each sample. 

1. Remove PCR product from the 4°C fridge and place on ice. 
2. Prepare the pre-labeled 1.5 mL labeled tubes for PCR product. 
3. Pool all PCR product from replicate reactions for each sample into one pooled PCR produt tube for each sample. You will have approx. 70 uL of total product.  
4. Store pooled product at 4°C until preparing for submission to sequencing facility. Store original and diluted gDNA at -20°C.  

# 6. Preparation and tube labeling for sequencing 

1. Label new PCR strip tubes according to sequencing facility instructions. For submission at RI-INBRE, we label samples with initials and ascending sample number (e.g., AH 1-18) in strip tubes. 
2. Aliquot volume of PCR product into each strip tube for each sample according to sequencing facility instructions. 
3. Store at 4°C until submission for next-generation sequencing. 

# 7. Clean up (if required)

If primer dimers are present or size specific clean up is required, see [this protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/KAPA-bead-clean-up-protocol-for-removal-of-primer-dimers-from-PCR-product/) for more information.  

# 8. Information on library preparation and sequencing 

We conducted sequencing at the [URI RI-INBRE core facility](https://web.uri.edu/riinbre/mic/). The following information contains the protocols used for library preparation.  

The indices and adapters used are for the Nextera, Illumina Prep, and Illumina PCR Kits and are located at [Illumina's website here](https://support-docs.illumina.com/SHARE/AdapterSequences/Content/SHARE/AdapterSeq/Nextera/SequencesNextera_Illumina.htm). 
 
25 µL of each sample was then submitted to the RI-INBRE Molecular Informatics Core at the University of Rhode Island (Kingston, RI, USA) for the index PCR and sequencing. PCR products from the first round of PCR were cleaned with NucleoMag beads (Takara Bio, San Jose, CA, USA) and then visualized by agarose gel electrophoresis. A second round of PCR (50 ng of template DNA, 8 cycles) was performed to attach Nextera XT indices and adapters using the Illumina Nextera XT® Index Kit (Illumina, San Diego, CA, USA) and Phusion HiFi PCR master mix (New England Biolabs, Ipswich, MA, USA, Catalog #Cat #M0531S). PCR products from the second PCR were cleaned with NucleoMag beads and analyzed by agarose gel electrophoresis. Selected samples were confirmed on an Agilent BioAnalyzer DNA1000 chip (Santa Clara, CA, USA). Quantification was performed on all samples prior to pooling using a Qubit fluorometer (Invitrogen, Carlsbad, CA, USA), and the final pooled library was quantified by qPCR in a Roche LightCycler480 with the KAPA Biosystems Illumina Kit (Roche Sequencing, Indianapolis, IN, USA). Samples were sequenced on an Illumina MiSeq platform (Illumina, San Diego, CA, USA) using 2x300 bp paired-end sequencing at the RI-INBRE Molecular Informatics Core (University of Rhode Island, Kingston, RI, USA).  

The PCR protocols for the index and adapter PCR are below:   

Set up 1 50-µl reaction per sample:   

- 25 µl Phusion mastermix 
- 5 µl Nextera 7xx primer 
- 5 µl Nextera 5xx primer 
- 50 ng template DNA (or 7 ul) 
- H2O to 50 µl 

Perform PCR on thermal cycler:   

- 72°C for 3 minutes 
- 98°C for 30 seconds 
- 8-12 cycles of  
- 98°C for 10 seconds 
- 63°C for 30 seconds 
- 72°C for 3 minutes 
- Hold at 4°C 

The protocol for library preparation can be found on OSF [at the link here](https://osf.io/fq7ry).  

