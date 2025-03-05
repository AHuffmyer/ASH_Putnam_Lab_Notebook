---
layout: post
title: Analysis of DNA sequences for species identification in the Moorea 2023 project
date: '2025-03-05'
categories: Moorea_SymbioticExchange_2023
tags: Bioinformatics Molecular Sanger
---

This post details analysis of DNA sequences for species identification of *Pocillopora*, *Porites*, and *Acropora* samples from the Moorea 2023 project.  

# Protocols and resources used 

- [Sanger sequencing of samples for this project completed at URI](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Sanger-sequencing-preps-at-URI/)
- [Protocol for Porites and Pocillopora Sanger sequencing](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/POR-POR-Species-ID-Sanger-and-RFLP-Protocol/)
- [Pocillopora reference sequences](https://osf.io/5geqd/)
- [Johnston et al. 2018](https://peerj.com/articles/4355/)

All data and trees shown below are available [on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023).  

# Steps for analysis 

1. For each species, I loaded in the sequence files to Geneious using the URI license. 
2. I then downloaded reference sequences for Pocillopora (linked above) and used NCBI search in Geneious to download sequences for the other species and markers.  
3. For markers with sequencing only in the forward direction (Pocillopora mtORF and Acropora PaxC), I proceeded directly to multiple alignment using MUSCLE. 
4. For sequences with forward and reverse sequences (Porites H2/H4 and Acropora mt Control Region) I first generated a consensus sequence for each sample. Consensus sequences were then aligned using multiple alignment with MUSCLE. 
5. I then examined the sequences and edited if necessary. I removed the first ~20bp on the start and end of the sequence and manually called discrepancies in base pairs calls ("N"'s) when possible.  
6. I then generated a neighbor-joining consensus tree from the alignment with bootstrapping. 
7. For each species, I then used the generated tree for species designations. Details on how I did this are listed for each species below.  

All generated trees are described below and can be found on [GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/tree/main/output/dna).  

# Pocillopora

The reference sequences provide well established reference to konwn species. We used these reference sequences to identify species of our samples based on clustering in the tree. 

For samples identified as Haplotype 1, we conducted an RFLP to distinguish between *P. meandrina* and *P. grandis*, documented [in this post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Sanger-sequencing-preps-at-URI/).

The tree below includes notations for species calls based on the sequencing. Note that for *Pocillopora*, I have also recorded RFLP results for final species identification provided below.  

My worksheet for identifying species [can be found here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/sequences/POC/20241119_Huffmyer_GSC_Project1_POC.csv).  

Here is the generated tree:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/poc-tree.png?raw=true)  

Species clusters were fairly clear in this tree. Annotations show species corresponding with haplotype as described in [Johnston et al. 2018](https://peerj.com/articles/4355/).  

## Total species identification 

| POCILLOPORA  | Adults | Recruits |
|--------------|--------|----------|
| meandrina    | 13     | 25       |
| verrucosa    | 5      | 0        |
| grandis      | 3      | 2        |
| tuahiniensis | 4      | 3        |
| effusa       | 0      | 7        |
| acuta        | 0      | 2        |

# Acropora

Acropora species ID was more challenging that Pocillopora. For the PaxC intron marker and the mitochondrial control region, I used the NCBI search tool to download other sequences in Acropora species. However, the clustering was not particularly clear, and I decided to use photographs of the adult corals from our collection to try to map species ID onto the trees using the photo identification. 

Note that I have added annotation for species identification using my best guess of species based on photographs. I am confident in my identification of *hyacinthus* and *cytherea*, but less confident in the other species. Because we are most interested in *hyacinthus/cytherea* samples, we really care about those that are *hyacinthus/cytherea* and those that are a different species.  

My worksheet for identifying species [can be found here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/sequences/ACR/Assembled-ACR-results.csv).  

First, here is the generated mitochondrial control region tree:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/acr-mtCR-tree.png?raw=true) 

In this tree, I highlighted samples that were adults and added a notation for the species identification from the photograph. Notations with a question mark are my best guess based on the photograph. There were a few that I could not identify from the photograph, marked as "unsure".  

The boxes show clusters based on mapping the photo identifications on the trees. *Hyacinthus* and *cytherea* group together, with other clusters that appear to be *retusa*, *globiceps* or similar, *lutkeni/pulchra*, and unknown. 

Sequences with an NCBI identification and a species are based on reference sequences from NCBI.  

Second, here is the generated PaxC intron tree:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/acr-paxC-tree.png?raw=true) 

The main distinguishing factor for PaxC is the length - some samples had a shorter length while others were longer. All *hyacinthus/cytherea* samples had the short variant of the PaxC marker, where as other species had the long variant.  

Clustering was unclear and length seems to be the only interesting factor with this marker.  

I then determined samples to be *hyacinthus/cytherea* if they grouped with respective clusters in the mtCR tree AND had a short PaxC sequence. 

Other species were determined by the respective clusters in the mtCR tree and having a long PaxC sequence. There were a couple exceptions of species that were not *hyacinthus/cytherea* with short PaxC sequences, which happened to be those I couldn't identify from pictures.  

## Total species identification 

| ACROPORA            | Adults | Recruits |
|---------------------|--------|----------|
| lutkeni/pulchra     | 5      | 4        |
| hyacinthus/cytherea | 13     | 21       |
| retusa              | 4      | 8        |
| unknown             | 2      | 5        |
| globiceps           | 3      | 1        |

# Porites 

Porities identifications were completed by comparing to *P. lobata/lutea* and *P. evermanni* reference sequences provided to me from Hollie's E5 comparison. NCBI identifiers can be seen in the tree.  

In some of the samples, a consensus sequence could not be generated. I therefore tested whether using only the forward sequence would be sufficient for identification (samples denoted with "F"). I did this for those that did not have a consensus sequence and also did this for those that did have a consensus sequence to compare to. Clustering was the same for using only the forward sequence or using the whole consensus sequence.  

My worksheet for identifying species [can be found here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/sequences/POR/20241119_Huffmyer_GSC_Project2_POR_forward.csv).  

Here is the H2/H4 tree:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/por-tree.png?raw=true) 

Only a small number of samples clustered with the *P. evermanni* references, with the remainder being either *P. lobata* or *P. lutea*. We will need to use further analyses to distinguish between these if desired. We think it is possible that the lower cluster and upper lobata/lutea clusters could represent the different species.  

We are not likely to use *Porites* samples in processing, so this is on the back burner for now.  

## Total species identification 

| PORITES      | Adults | Recruits |
|--------------|--------|----------|
| evermanni    | 2      | 3        |
| lobata lutea | 25     | 36       |

# Final species designations 

Final species identifications and associated metadata for all samples are recorded [on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/dna/dna_metadata.xlsx).  

# Project planning and next steps 

We will move forward with a project to test the following questions: 

- How does temperature influence metabolism and symbiotic nutritional exchange in *Acropora*, a horizontally transmitting species, and *Pocillopora*, a vertically transmitting species? 
- Does temperature differentially affect metabolism and symbiotic nutritional exchange in these species? 

We have obtain physiological metrics and metabolic rates of these corals. We will pursue additional RNA sequencing and processing samples for stable isotope metabolic flux analysis.  

Here are the samples that we will use:  

| POC vs ACR RNA and Metabolomics |    |                              |                        |    |                     |
|---------------------------------|----|------------------------------|------------------------|----|---------------------|
|                                 |    |                              |                        |    |                     |
| RNA                             |    |                              | Metabolomics           |    |                     |
| P. mea recruit                  | 10 | n=3-4 per treatment          | P. mea recruit         | 15 | n=4-5 per treatment |
| P. mea larvae                   | 18 | n=6 per treatment            | P. mea larvae          | 18 | n=6 per sample      |
| A. spp mix  recruit             | 18 | mixed species to get n=6 per | A. hya/A. ret  recruit | 16 | n=5-6 per treatment |
| A. hya larvae                   | 18 | n=6 per treatment            | Dark Controls          | 8  | n=4 per species     |
|                                 |    |                              |                        |    |                     |
| Total                           | 64 |                              | Total                  | 57 |                     |

We will focus on *P. meandrina* for *Pocillopora* samples. We also took samples of larvae of this species, allowing us to make larvae and recruit life stage comparisons.  

We will focus on a mixture of *Acropora* species, but aiming for most samples to be *A. hyacinthus*. We also collected larvae of *A. hyacinthus*, allowing us to compare larvae and recruits.  

Sampling will include stable isotope samples for *Pocillopora* (because the larvae have symbionts) and RNA sequencing for *Pocillopora* and *Acropora* larvae.  

We will conduct RNA sequencing and stable isotope metabolomics on recruits of *Pocillopora* and *Acropora*.  

I am now working on obtaining quotes for this work to begin processing.  