---
layout: post
title: Raw sequence downloads and NCBI SRA uploads for M. capitata larval thermal tolerance project
date: '2024-02-21'
categories: Larval_Symbiont_TPC_2023
tags: Bioinformatics Mcapitata Molecular GeneExpression
---

This post details the NCBI Sequence Read Archive upload for my *Montipora capitata* 2023 larval thermal tolerance project. See my [notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#larval-symbiont-tpc-2023) and my [GitHub repo](https://github.com/AHuffmyer/larval_symbiont_TPC) for information on this project.   

# 1. Creating SRA project and sample metadata

I first created an NCBI SRA BioProject and BioSample information in preparation for download and submission. I used the same steps outlined in [my previous notebook post for the Mcap2020 project](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/SRA-Uploads-10-November-2022/) and the [Putnam Lab SRA protocol](https://github.com/Putnam-Lab/Lab_Management/blob/master/Bioinformatics_%26_Coding/Data_Mangament/SRA-Upload_Protocol.md). 

I used the following characteristics to register the BioProject: 

- Multispecies 
- *M. capitata* and associated dinflaggellate endosymbionts in the family Symbiodinaceae
- Raw sequence reads 
- Data will be released in 1 year (March 1 2025) or earlier if published
- Title: *Montipora capitata* larval thermal tolerance
- Description: Influence of parental bleaching tolerance and symbiont species on larval thermal stress response in Montipora capitata corals
- Associated grants: 
	- ID: 2205966; OCE-PRF: Investigating ontogenetic shifts in microbe-derived nutrition in reef building corals; National Science Foundation

*Submission information* 

Submission ID: SUB14259319   
Submitted on Feb 20  
Project ID: PRJNA1078313   

I then generated sample information sheets for sample metadata and SRA metadata.  

The sample metadata can be found on GitHub here using the MIMS.me.host-associated template [here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/NCBI_upload/McapLarval_Tolerance_MIMS.me.host-associated.5.0.xlsx) and sequencing information and metadata [is located on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/sample_rnaseq_metadata.csv).  

I used the following characteristics for BioSample submission:  

- Data released on March 1 2025 
- Under above project ID PRJNA1078313
- Illumina sequencing on NovaSeq X platform 

I uploaded the biosample metadata [to GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/NCBI_upload/Hawaii2023_SRA_metadata.txt).   

I will then finish the upload below after data is downloaded onto the URI server.  

# 2. Download data from Azenta 

## View result statistics and metadata 

I used Azenta's sFTP instructions to view the project results. In their online storage, they provided an .html with an overview of statistics and the R1 and R2 .fastq.gz files for each sample. There are also .md5 files for each .fastq.gz file.  

**Overall statistics**

- number reads = 1,467,898,960
- Yield (Mbases) = 440,370	
- Mean quality scores = 37.88
- % bases over 30 quality score = 89.70 

**More detailed statistics**    

- All samples had >37 mean quality scores 
- 7 samples had less than 10M reads. These were all from random and different treatment groups, so if we end up needing to filter if mapped reads are <5M, we will still have n=5 minimum with >15M read depth for each treatment group. 
- All other samples had 15M-40M reads each 
- 88% bases had quality scores over 30 

| date     | sample | larvae | temperature | symbiont    | parent      | phenotype            | code                    | Barcode Sequence  | # Reads  | Yield (Mbases) | Mean Quality Score | % Bases >= 30 |
|----------|--------|--------|-------------|-------------|-------------|----------------------|-------------------------|-------------------|----------|----------------|--------------------|---------------|
| 20230627 | R59    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+CTTCGCCT | 6002991  | 1801           | 37.96              | 90.11         |
| 20230627 | R107   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+CTTCGCCT | 6719567  | 2016           | 38.15              | 90.98         |
| 20230627 | R91    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+CTTCGCCT | 6934253  | 2080           | 38.01              | 90.36         |
| 20230627 | R83    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+CTTCGCCT | 7497048  | 2249           | 38.06              | 90.57         |
| 20230627 | R67    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+CTTCGCCT | 7665773  | 2300           | 38.07              | 90.62         |
| 20230627 | R99    | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+CTTCGCCT | 8420607  | 2526           | 37.83              | 89.57         |
| 20230627 | R75    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+CTTCGCCT | 9871112  | 2961           | 38.12              | 90.85         |
| 20230627 | R57    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+AGGATAGG | 15232862 | 4570           | 37.7               | 88.88         |
| 20230627 | R55    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+AGGCTATA | 15818670 | 4746           | 37.8               | 89.36         |
| 20230627 | R60    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+TAAGATTA | 16284649 | 4885           | 37.63              | 88.54         |
| 20230627 | R62    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TTCCTCCT+GTCAGTAC | 16675688 | 5003           | 37.64              | 88.59         |
| 20230627 | R56    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+GCCTCTAT | 17457755 | 5238           | 37.58              | 88.32         |
| 20230627 | R58    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+TCAGAGCC | 17987445 | 5396           | 37.73              | 89.03         |
| 20230627 | R61    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TTCCTCCT+ACGTCCTG | 19891057 | 5967           | 37.73              | 89.05         |
| 20230627 | R103   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+AGGCTATA | 21980396 | 6594           | 38.01              | 90.36         |
| 20230627 | R63    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+AGGCTATA | 23828671 | 7149           | 38.01              | 90.33         |
| 20230627 | R87    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+AGGCTATA | 24408673 | 7323           | 38.09              | 90.72         |
| 20230627 | R79    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+AGGCTATA | 25009225 | 7502           | 37.93              | 89.95         |
| 20230627 | R95    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | TGATCACG+AGGCTATA | 27375934 | 8213           | 37.92              | 89.92         |
| 20230627 | R71    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | GGTGATGA+AGGCTATA | 27838736 | 8352           | 37.99              | 90.27         |
| 20230627 | R92    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+TAAGATTA | 28565956 | 8570           | 37.93              | 89.93         |
| 20230627 | R108   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+TAAGATTA | 29304668 | 8791           | 37.95              | 90.04         |
| 20230627 | R105   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+AGGATAGG | 29434310 | 8830           | 37.84              | 89.51         |
| 20230627 | R88    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+GCCTCTAT | 30584889 | 9175           | 37.85              | 89.56         |
| 20230627 | R104   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+GCCTCTAT | 30634968 | 9191           | 37.91              | 89.86         |
| 20230627 | R68    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+TAAGATTA | 30934325 | 9280           | 37.92              | 89.89         |
| 20230627 | R93    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+ACGTCCTG | 31620062 | 9486           | 38.02              | 90.37         |
| 20230627 | R90    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+TCAGAGCC | 31879658 | 9564           | 38.03              | 90.37         |
| 20230627 | R65    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+AGGATAGG | 32099169 | 9630           | 37.78              | 89.27         |
| 20230627 | R84    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+TAAGATTA | 32220467 | 9666           | 37.92              | 89.86         |
| 20230627 | R66    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+TCAGAGCC | 32272569 | 9682           | 37.96              | 90.08         |
| 20230627 | R64    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+GCCTCTAT | 32277087 | 9683           | 37.93              | 89.96         |
| 20230627 | R94    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+GTCAGTAC | 32531274 | 9759           | 37.87              | 89.62         |
| 20230627 | R86    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | AACCTACG+GTCAGTAC | 32691940 | 9808           | 37.76              | 89.14         |
| 20230627 | R76    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+TAAGATTA | 33036284 | 9911           | 37.95              | 90.02         |
| 20230627 | R89    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+AGGATAGG | 33121528 | 9936           | 37.87              | 89.65         |
| 20230627 | R97    | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+AGGATAGG | 33376891 | 10013          | 37.77              | 89.17         |
| 20230627 | R81    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+AGGATAGG | 33493354 | 10048          | 37.78              | 89.2          |
| 20230627 | R102   | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+GTCAGTAC | 33729404 | 10119          | 37.79              | 89.27         |
| 20230627 | R72    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | GGTGATGA+GCCTCTAT | 33950055 | 10185          | 37.81              | 89.34         |
| 20230627 | R70    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+GTCAGTAC | 34280767 | 10284          | 37.88              | 89.7          |
| 20230627 | R100   | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+TAAGATTA | 34566278 | 10370          | 37.79              | 89.32         |
| 20230627 | R74    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+TCAGAGCC | 34922263 | 10476          | 37.96              | 90.07         |
| 20230627 | R106   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+TCAGAGCC | 34929945 | 10479          | 37.94              | 89.97         |
| 20230627 | R73    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+AGGATAGG | 35173243 | 10552          | 37.88              | 89.71         |
| 20230627 | R78    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+GTCAGTAC | 35398354 | 10619          | 37.86              | 89.61         |
| 20230627 | R82    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+TCAGAGCC | 35452563 | 10636          | 37.92              | 89.85         |
| 20230627 | R96    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | TGATCACG+GCCTCTAT | 35882529 | 10765          | 37.78              | 89.22         |
| 20230627 | R98    | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+TCAGAGCC | 35899983 | 10770          | 37.86              | 89.63         |
| 20230627 | R80    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+GCCTCTAT | 36133089 | 10840          | 37.85              | 89.53         |
| 20230627 | R85    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | AACCTACG+ACGTCCTG | 36878406 | 11063          | 37.84              | 89.46         |
| 20230627 | R101   | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+ACGTCCTG | 38457421 | 11538          | 37.79              | 89.29         |
| 20230627 | R69    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+ACGTCCTG | 39517309 | 11856          | 37.9               | 89.77         |
| 20230627 | R77    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+ACGTCCTG | 39746840 | 11924          | 37.98              | 90.17         |

These results are under Azenta project 30-943303755 on an Illumina NovaSeq X at 2x150bp. 
Azenta [FAQs are here](https://web.genewiz.com/raw-data-faqs). Note that we will need to perform filtering and trimming and adapter removal as these are not done by Azenta. 

Full results of the report are [on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/Azenta_30-943303755_Data_Report.html) and I added stats to the sample metadata [here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/sample_rnaseq_metadata.csv). 


## Download data to URI Andromeda 

Prepare folders in Andromeda directory.     

```
#logged into Andromeda 
cd /data/putnamlab/ashuffmyer
mkdir mcap-2023-rnaseq
cd mcap-2023-rnaseq
mkdir scripts
mkdir raw-sequences

```

Now the directory that I want sequences in is `/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences`.    

```
# in andromeda raw-sequences folder 

pwd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences

# log into Azenta sftp as directed by Azenta 
# cd into sequence folder in my project 

#set directory for download
lcd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences

#download all files in sequence folder 
mget *
```

Downloaded to URI Andromeda on Feb 20 2024.    

I then checked for data integrity using md5 checksums.    

```
md5sum *.fastq.gz > checkmd5_20240221.md5
md5sum -c checkmd5_20240221.md5

```

Output was as follows:   

```
R100_R1_001.fastq.gz: OK
R100_R2_001.fastq.gz: OK
R101_R1_001.fastq.gz: OK
R101_R2_001.fastq.gz: OK
R102_R1_001.fastq.gz: OK
R102_R2_001.fastq.gz: OK
R103_R1_001.fastq.gz: OK
R103_R2_001.fastq.gz: OK
R104_R1_001.fastq.gz: OK
R104_R2_001.fastq.gz: OK
R105_R1_001.fastq.gz: OK
R105_R2_001.fastq.gz: OK
R106_R1_001.fastq.gz: OK
R106_R2_001.fastq.gz: OK
R107_R1_001.fastq.gz: OK
R107_R2_001.fastq.gz: OK
R108_R1_001.fastq.gz: OK
R108_R2_001.fastq.gz: OK
R55_R1_001.fastq.gz: OK
R55_R2_001.fastq.gz: OK
R56_R1_001.fastq.gz: OK
R56_R2_001.fastq.gz: OK
R57_R1_001.fastq.gz: OK
R57_R2_001.fastq.gz: OK
R58_R1_001.fastq.gz: OK
R58_R2_001.fastq.gz: OK
R59_R1_001.fastq.gz: OK
R59_R2_001.fastq.gz: OK
R60_R1_001.fastq.gz: OK
R60_R2_001.fastq.gz: OK
R61_R1_001.fastq.gz: OK
R61_R2_001.fastq.gz: OK
R62_R1_001.fastq.gz: OK
R62_R2_001.fastq.gz: OK
R63_R1_001.fastq.gz: OK
R63_R2_001.fastq.gz: OK
R64_R1_001.fastq.gz: OK
R64_R2_001.fastq.gz: OK
R65_R1_001.fastq.gz: OK
R65_R2_001.fastq.gz: OK
R66_R1_001.fastq.gz: OK
R66_R2_001.fastq.gz: OK
R67_R1_001.fastq.gz: OK
R67_R2_001.fastq.gz: OK
R68_R1_001.fastq.gz: OK
R68_R2_001.fastq.gz: OK
R69_R1_001.fastq.gz: OK
R69_R2_001.fastq.gz: OK
R70_R1_001.fastq.gz: OK
R70_R2_001.fastq.gz: OK
R71_R1_001.fastq.gz: OK
R71_R2_001.fastq.gz: OK
R72_R1_001.fastq.gz: OK
R72_R2_001.fastq.gz: OK
R73_R1_001.fastq.gz: OK
R73_R2_001.fastq.gz: OK
R74_R1_001.fastq.gz: OK
R74_R2_001.fastq.gz: OK
R75_R1_001.fastq.gz: OK
R75_R2_001.fastq.gz: OK
R76_R1_001.fastq.gz: OK
R76_R2_001.fastq.gz: OK
R77_R1_001.fastq.gz: OK
R77_R2_001.fastq.gz: OK
R78_R1_001.fastq.gz: OK
R78_R2_001.fastq.gz: OK
R79_R1_001.fastq.gz: OK
R79_R2_001.fastq.gz: OK
R80_R1_001.fastq.gz: OK
R80_R2_001.fastq.gz: OK
R81_R1_001.fastq.gz: OK
R81_R2_001.fastq.gz: OK
R82_R1_001.fastq.gz: OK
R82_R2_001.fastq.gz: OK
R83_R1_001.fastq.gz: OK
R83_R2_001.fastq.gz: OK
R84_R1_001.fastq.gz: OK
R84_R2_001.fastq.gz: OK
R85_R1_001.fastq.gz: OK
R85_R2_001.fastq.gz: OK
R86_R1_001.fastq.gz: OK
R86_R2_001.fastq.gz: OK
R87_R1_001.fastq.gz: OK
R87_R2_001.fastq.gz: OK
R88_R1_001.fastq.gz: OK
R88_R2_001.fastq.gz: OK
R89_R1_001.fastq.gz: OK
R89_R2_001.fastq.gz: OK
R90_R1_001.fastq.gz: OK
R90_R2_001.fastq.gz: OK
R91_R1_001.fastq.gz: OK
R91_R2_001.fastq.gz: OK
R92_R1_001.fastq.gz: OK
R92_R2_001.fastq.gz: OK
R93_R1_001.fastq.gz: OK
R93_R2_001.fastq.gz: OK
R94_R1_001.fastq.gz: OK
R94_R2_001.fastq.gz: OK
R95_R1_001.fastq.gz: OK
R95_R2_001.fastq.gz: OK
R96_R1_001.fastq.gz: OK
R96_R2_001.fastq.gz: OK
R97_R1_001.fastq.gz: OK
R97_R2_001.fastq.gz: OK
R98_R1_001.fastq.gz: OK
R98_R2_001.fastq.gz: OK
R99_R1_001.fastq.gz: OK
R99_R2_001.fastq.gz: OK
```

Azenta provided a .md5 file for each sequence file. I then compared generated checksums to these original files to confirm data integrity and content is the same after the transfer from Azenta. 

```
#bind together all .md5 files provided by Azenta 

cat *.gz.md5 > azenta_original_checksums.md5
```

This provided the following list of the original checksums:  

```
fb2f856ffeaf2337acebada68643ce45  ./R100_R1_001.fastq.gz
38ad7ea4ed9c003f73ba3c29fc7fc83c  ./R100_R2_001.fastq.gz
0f9e5d6157b53737d6b26c6821adfc4d  ./R101_R1_001.fastq.gz
40c1ec83ba83690bf55b7102f1217f61  ./R101_R2_001.fastq.gz
5ffbdd93efdd8368ad071c5db3f18a91  ./R102_R1_001.fastq.gz
617fee9820cb009e9b641cc7aaaa9b98  ./R102_R2_001.fastq.gz
dfefd35c89d5d90ade043685d0f7b849  ./R103_R1_001.fastq.gz
e21941866ddf94d968689dd60c07c846  ./R103_R2_001.fastq.gz
de10a95996f35b9a599d94dca8dd6495  ./R104_R1_001.fastq.gz
50030888aea12e22ee79f47d65006cac  ./R104_R2_001.fastq.gz
cfa5f9cb57df8c95903bd6ed0035067b  ./R105_R1_001.fastq.gz
6749095cf6ec0ca27d6cebed13477e75  ./R105_R2_001.fastq.gz
9ffd4dc02a7b57791b87f5da0f46fbb0  ./R106_R1_001.fastq.gz
7d39bda85955ce9704e3cc7abdb3ce7f  ./R106_R2_001.fastq.gz
090ccb4ca651400e805a936ff920d54b  ./R107_R1_001.fastq.gz
dc7f22d773b311f9f605a9ff5677049e  ./R107_R2_001.fastq.gz
b5d93b1aa771ab2217cbf49ee15d12a4  ./R108_R1_001.fastq.gz
1fa5be76b43fb96630e9f7917ba0d7a9  ./R108_R2_001.fastq.gz
656ed0ade96334eba2f18a34c54afe13  ./R55_R1_001.fastq.gz
a58c4a5d0cf1d928d77b4e269458fe62  ./R55_R2_001.fastq.gz
cdd386cae610becdadf4ffd28a035991  ./R56_R1_001.fastq.gz
627a4b677ea068d66457820d7d84fdd7  ./R56_R2_001.fastq.gz
ee79222483a7095a333b1b89eb5ec0cc  ./R57_R1_001.fastq.gz
52b321e37026ee38551dd2bfe689ca36  ./R57_R2_001.fastq.gz
16c1ac82b21d0b37ed26502f806a0b29  ./R58_R1_001.fastq.gz
d71d8d697af621daf7c06cfa9ac4f778  ./R58_R2_001.fastq.gz
86f57a12b12e647959ddc97b7f3ad5c4  ./R59_R1_001.fastq.gz
f1668762f80250cbfe0acb4936b1f6e9  ./R59_R2_001.fastq.gz
db2b5b1d34316e6f605df732a1474e0d  ./R60_R1_001.fastq.gz
7d56f82a9cc4cf3dd58b1cfb5d3635c0  ./R60_R2_001.fastq.gz
584a05ee25744aeafce4d9931fa877d4  ./R61_R1_001.fastq.gz
d75439fd1d80dec4209ce6d7441dd1e8  ./R61_R2_001.fastq.gz
f7eb8374ea83f381aeeb29b6252d8c2c  ./R62_R1_001.fastq.gz
d0b9bd3d1ec7020399a7deb26f1ea037  ./R62_R2_001.fastq.gz
ee882c33154f14fe92b86ddaabbb7628  ./R63_R1_001.fastq.gz
22bb2b865f92f1daccc8ff7fe6072351  ./R63_R2_001.fastq.gz
9997108124197468b7ede97121ecc9de  ./R64_R1_001.fastq.gz
5bbce1622beacaea94fcc7cd502c0ef2  ./R64_R2_001.fastq.gz
4c6cce887cec546565a5c8ca9167e047  ./R65_R1_001.fastq.gz
4fac3b0355cfcd0711fffe2afa861818  ./R65_R2_001.fastq.gz
f6c497cff568791b2a88c4f90705bc1e  ./R66_R1_001.fastq.gz
a68eaac8298321b81646cdf50dd0f7c8  ./R66_R2_001.fastq.gz
32a7dd3c9dbfc46932ce4cd33d86f7b4  ./R67_R1_001.fastq.gz
9f0b7cf1fa8efe953b9134cda3d97c9a  ./R67_R2_001.fastq.gz
4f81e378fa78ec720a67308c3f09eaca  ./R68_R1_001.fastq.gz
76e328588a3ba9cf4d64638401988f3a  ./R68_R2_001.fastq.gz
6dc239cd5053c9f0f42f7c4a61452bb1  ./R69_R1_001.fastq.gz
ac76754b6340e68ea96a9dde2e453d7c  ./R69_R2_001.fastq.gz
55322d7fed5e8fa3c57a332797759187  ./R70_R1_001.fastq.gz
e5786bcb34f286e1cf799145e65f8a1e  ./R70_R2_001.fastq.gz
10d273a9ddd932678c4450d71c3df2dc  ./R71_R1_001.fastq.gz
9a7997165f024a9f7949d254a5249d13  ./R71_R2_001.fastq.gz
e4f3fdc7576dae75737ed141c121be37  ./R72_R1_001.fastq.gz
57cecc3994764db6b4597957b44d972f  ./R72_R2_001.fastq.gz
d736f67efcc2a3c7516745fc1a3ccd46  ./R73_R1_001.fastq.gz
5c76421457b3901907cf2a9651d6e785  ./R73_R2_001.fastq.gz
220f2c21d6aafc2b3019074ea209e260  ./R74_R1_001.fastq.gz
a9f1143d915eea62467319e68fe87f9a  ./R74_R2_001.fastq.gz
481c8b51b3b1cf424120f841068ce4d7  ./R75_R1_001.fastq.gz
f0e4311f966459b2335873902bd43ba3  ./R75_R2_001.fastq.gz
9076f07b4ad208e2d550f3c94f0c8787  ./R76_R1_001.fastq.gz
22368829e561fd298adee8f7551d3705  ./R76_R2_001.fastq.gz
9d76eb6f1f65b84f8fb8afb09ff935eb  ./R77_R1_001.fastq.gz
b8d368c154eafa4f0fa427954810bf28  ./R77_R2_001.fastq.gz
f97ae96e6c4aa9e476af14470ab7928f  ./R78_R1_001.fastq.gz
bc33760d2d5a22e7f35403d50aff7815  ./R78_R2_001.fastq.gz
be3d7a5d0ed734fd3b1c87273580eaf1  ./R79_R1_001.fastq.gz
d5cdad1d9b5aade2149c5a2645397793  ./R79_R2_001.fastq.gz
808fb96f7cf390540e66ed60539501d7  ./R80_R1_001.fastq.gz
8640e182f98e19e40d76d4b07da103d1  ./R80_R2_001.fastq.gz
2d495d29f472dcf73a70f9633867dd2b  ./R81_R1_001.fastq.gz
ea358d700478d0699e6476e3cab8b928  ./R81_R2_001.fastq.gz
bdbaf3c3b212da61567a1721ce3a8246  ./R82_R1_001.fastq.gz
e8584a5f259adf7117cfafd7b6ce709f  ./R82_R2_001.fastq.gz
199c1d6aa30e4d82dc360ee7b48fc121  ./R83_R1_001.fastq.gz
939bd072fcfa962e9e20936522153ff5  ./R83_R2_001.fastq.gz
718f79e244cb8518bdd3f0689f99cd4c  ./R84_R1_001.fastq.gz
093521ef06042599a19611baa6122e43  ./R84_R2_001.fastq.gz
6f68d6ee2b5d053232fca54ac64a058c  ./R85_R1_001.fastq.gz
08801a798da6edb9dd75f3973d01ed0a  ./R85_R2_001.fastq.gz
c1a0652008f2025f27f26b2637311c63  ./R86_R1_001.fastq.gz
a0572df5065cfbbfb9c5394d3b7bf25e  ./R86_R2_001.fastq.gz
9373699fd3ce5e5de9c4f7cb9256cb59  ./R87_R1_001.fastq.gz
9d0dfd38caca15854aeace535077a6db  ./R87_R2_001.fastq.gz
6ac435c22f801610476e436c7335fee4  ./R88_R1_001.fastq.gz
e4a8a0651e7d956817548fd449868558  ./R88_R2_001.fastq.gz
0edecaaa547e01c7e09b52e3a1630875  ./R89_R1_001.fastq.gz
75562b8b318cdde465aecea9dbbb4a90  ./R89_R2_001.fastq.gz
8fc5fa62c0a901b76b9bf6e61f1fffb3  ./R90_R1_001.fastq.gz
63597112cc2ae5dc914e3bee2a8d4cdc  ./R90_R2_001.fastq.gz
f11e6812f69c1627072ef88103e2a32b  ./R91_R1_001.fastq.gz
8dbae98da0fba664b87c8f4d5717e463  ./R91_R2_001.fastq.gz
b1800bbcd1d33d183fcd371623426c32  ./R92_R1_001.fastq.gz
8f453d98f95e829867149f3c10f5d993  ./R92_R2_001.fastq.gz
78d4b9261e66e9d122af3b10f53a5ea9  ./R93_R1_001.fastq.gz
68f809c166d98cc4376b28c57d09fd9b  ./R93_R2_001.fastq.gz
ca9c487cdb136f12e2006bd2222997f4  ./R94_R1_001.fastq.gz
38ae96c31bd7b536844a1ebebb69df9c  ./R94_R2_001.fastq.gz
34a4412186175b4a99513edbab6065f1  ./R95_R1_001.fastq.gz
54b52ca18af223327f731ef8b490878c  ./R95_R2_001.fastq.gz
5865a8aa32dd61a3e520664f5a185e66  ./R96_R1_001.fastq.gz
ee13781c1e0786819cff94071c779ef0  ./R96_R2_001.fastq.gz
2edae3a28b54c6dc883523c96a3f62fe  ./R97_R1_001.fastq.gz
75d704624b9cbede2ac67c1f138996ca  ./R97_R2_001.fastq.gz
63b0abf30af7e76b87d0bf9dd53492f2  ./R98_R1_001.fastq.gz
48ec3236da746a513c568f7d6be863a5  ./R98_R2_001.fastq.gz
1b4fa2175c2e696932cd41165cd8bd90  ./R99_R1_001.fastq.gz
a2d833a7f784675acc8e5b8ade9aad98  ./R99_R2_001.fastq.gz
```

Then, here is the md5 checksum of the downloaded data on Andromeda:  

```
fb2f856ffeaf2337acebada68643ce45  R100_R1_001.fastq.gz
38ad7ea4ed9c003f73ba3c29fc7fc83c  R100_R2_001.fastq.gz
0f9e5d6157b53737d6b26c6821adfc4d  R101_R1_001.fastq.gz
40c1ec83ba83690bf55b7102f1217f61  R101_R2_001.fastq.gz
5ffbdd93efdd8368ad071c5db3f18a91  R102_R1_001.fastq.gz
617fee9820cb009e9b641cc7aaaa9b98  R102_R2_001.fastq.gz
dfefd35c89d5d90ade043685d0f7b849  R103_R1_001.fastq.gz
e21941866ddf94d968689dd60c07c846  R103_R2_001.fastq.gz
de10a95996f35b9a599d94dca8dd6495  R104_R1_001.fastq.gz
50030888aea12e22ee79f47d65006cac  R104_R2_001.fastq.gz
cfa5f9cb57df8c95903bd6ed0035067b  R105_R1_001.fastq.gz
6749095cf6ec0ca27d6cebed13477e75  R105_R2_001.fastq.gz
9ffd4dc02a7b57791b87f5da0f46fbb0  R106_R1_001.fastq.gz
7d39bda85955ce9704e3cc7abdb3ce7f  R106_R2_001.fastq.gz
090ccb4ca651400e805a936ff920d54b  R107_R1_001.fastq.gz
dc7f22d773b311f9f605a9ff5677049e  R107_R2_001.fastq.gz
b5d93b1aa771ab2217cbf49ee15d12a4  R108_R1_001.fastq.gz
1fa5be76b43fb96630e9f7917ba0d7a9  R108_R2_001.fastq.gz
656ed0ade96334eba2f18a34c54afe13  R55_R1_001.fastq.gz
a58c4a5d0cf1d928d77b4e269458fe62  R55_R2_001.fastq.gz
cdd386cae610becdadf4ffd28a035991  R56_R1_001.fastq.gz
627a4b677ea068d66457820d7d84fdd7  R56_R2_001.fastq.gz
ee79222483a7095a333b1b89eb5ec0cc  R57_R1_001.fastq.gz
52b321e37026ee38551dd2bfe689ca36  R57_R2_001.fastq.gz
16c1ac82b21d0b37ed26502f806a0b29  R58_R1_001.fastq.gz
d71d8d697af621daf7c06cfa9ac4f778  R58_R2_001.fastq.gz
86f57a12b12e647959ddc97b7f3ad5c4  R59_R1_001.fastq.gz
f1668762f80250cbfe0acb4936b1f6e9  R59_R2_001.fastq.gz
db2b5b1d34316e6f605df732a1474e0d  R60_R1_001.fastq.gz
7d56f82a9cc4cf3dd58b1cfb5d3635c0  R60_R2_001.fastq.gz
584a05ee25744aeafce4d9931fa877d4  R61_R1_001.fastq.gz
d75439fd1d80dec4209ce6d7441dd1e8  R61_R2_001.fastq.gz
f7eb8374ea83f381aeeb29b6252d8c2c  R62_R1_001.fastq.gz
d0b9bd3d1ec7020399a7deb26f1ea037  R62_R2_001.fastq.gz
ee882c33154f14fe92b86ddaabbb7628  R63_R1_001.fastq.gz
22bb2b865f92f1daccc8ff7fe6072351  R63_R2_001.fastq.gz
9997108124197468b7ede97121ecc9de  R64_R1_001.fastq.gz
5bbce1622beacaea94fcc7cd502c0ef2  R64_R2_001.fastq.gz
4c6cce887cec546565a5c8ca9167e047  R65_R1_001.fastq.gz
4fac3b0355cfcd0711fffe2afa861818  R65_R2_001.fastq.gz
f6c497cff568791b2a88c4f90705bc1e  R66_R1_001.fastq.gz
a68eaac8298321b81646cdf50dd0f7c8  R66_R2_001.fastq.gz
32a7dd3c9dbfc46932ce4cd33d86f7b4  R67_R1_001.fastq.gz
9f0b7cf1fa8efe953b9134cda3d97c9a  R67_R2_001.fastq.gz
4f81e378fa78ec720a67308c3f09eaca  R68_R1_001.fastq.gz
76e328588a3ba9cf4d64638401988f3a  R68_R2_001.fastq.gz
6dc239cd5053c9f0f42f7c4a61452bb1  R69_R1_001.fastq.gz
ac76754b6340e68ea96a9dde2e453d7c  R69_R2_001.fastq.gz
55322d7fed5e8fa3c57a332797759187  R70_R1_001.fastq.gz
e5786bcb34f286e1cf799145e65f8a1e  R70_R2_001.fastq.gz
10d273a9ddd932678c4450d71c3df2dc  R71_R1_001.fastq.gz
9a7997165f024a9f7949d254a5249d13  R71_R2_001.fastq.gz
e4f3fdc7576dae75737ed141c121be37  R72_R1_001.fastq.gz
57cecc3994764db6b4597957b44d972f  R72_R2_001.fastq.gz
d736f67efcc2a3c7516745fc1a3ccd46  R73_R1_001.fastq.gz
5c76421457b3901907cf2a9651d6e785  R73_R2_001.fastq.gz
220f2c21d6aafc2b3019074ea209e260  R74_R1_001.fastq.gz
a9f1143d915eea62467319e68fe87f9a  R74_R2_001.fastq.gz
481c8b51b3b1cf424120f841068ce4d7  R75_R1_001.fastq.gz
f0e4311f966459b2335873902bd43ba3  R75_R2_001.fastq.gz
9076f07b4ad208e2d550f3c94f0c8787  R76_R1_001.fastq.gz
22368829e561fd298adee8f7551d3705  R76_R2_001.fastq.gz
9d76eb6f1f65b84f8fb8afb09ff935eb  R77_R1_001.fastq.gz
b8d368c154eafa4f0fa427954810bf28  R77_R2_001.fastq.gz
f97ae96e6c4aa9e476af14470ab7928f  R78_R1_001.fastq.gz
bc33760d2d5a22e7f35403d50aff7815  R78_R2_001.fastq.gz
be3d7a5d0ed734fd3b1c87273580eaf1  R79_R1_001.fastq.gz
d5cdad1d9b5aade2149c5a2645397793  R79_R2_001.fastq.gz
808fb96f7cf390540e66ed60539501d7  R80_R1_001.fastq.gz
8640e182f98e19e40d76d4b07da103d1  R80_R2_001.fastq.gz
2d495d29f472dcf73a70f9633867dd2b  R81_R1_001.fastq.gz
ea358d700478d0699e6476e3cab8b928  R81_R2_001.fastq.gz
bdbaf3c3b212da61567a1721ce3a8246  R82_R1_001.fastq.gz
e8584a5f259adf7117cfafd7b6ce709f  R82_R2_001.fastq.gz
199c1d6aa30e4d82dc360ee7b48fc121  R83_R1_001.fastq.gz
939bd072fcfa962e9e20936522153ff5  R83_R2_001.fastq.gz
718f79e244cb8518bdd3f0689f99cd4c  R84_R1_001.fastq.gz
093521ef06042599a19611baa6122e43  R84_R2_001.fastq.gz
6f68d6ee2b5d053232fca54ac64a058c  R85_R1_001.fastq.gz
08801a798da6edb9dd75f3973d01ed0a  R85_R2_001.fastq.gz
c1a0652008f2025f27f26b2637311c63  R86_R1_001.fastq.gz
a0572df5065cfbbfb9c5394d3b7bf25e  R86_R2_001.fastq.gz
9373699fd3ce5e5de9c4f7cb9256cb59  R87_R1_001.fastq.gz
9d0dfd38caca15854aeace535077a6db  R87_R2_001.fastq.gz
6ac435c22f801610476e436c7335fee4  R88_R1_001.fastq.gz
e4a8a0651e7d956817548fd449868558  R88_R2_001.fastq.gz
0edecaaa547e01c7e09b52e3a1630875  R89_R1_001.fastq.gz
75562b8b318cdde465aecea9dbbb4a90  R89_R2_001.fastq.gz
8fc5fa62c0a901b76b9bf6e61f1fffb3  R90_R1_001.fastq.gz
63597112cc2ae5dc914e3bee2a8d4cdc  R90_R2_001.fastq.gz
f11e6812f69c1627072ef88103e2a32b  R91_R1_001.fastq.gz
8dbae98da0fba664b87c8f4d5717e463  R91_R2_001.fastq.gz
b1800bbcd1d33d183fcd371623426c32  R92_R1_001.fastq.gz
8f453d98f95e829867149f3c10f5d993  R92_R2_001.fastq.gz
78d4b9261e66e9d122af3b10f53a5ea9  R93_R1_001.fastq.gz
68f809c166d98cc4376b28c57d09fd9b  R93_R2_001.fastq.gz
ca9c487cdb136f12e2006bd2222997f4  R94_R1_001.fastq.gz
38ae96c31bd7b536844a1ebebb69df9c  R94_R2_001.fastq.gz
34a4412186175b4a99513edbab6065f1  R95_R1_001.fastq.gz
54b52ca18af223327f731ef8b490878c  R95_R2_001.fastq.gz
5865a8aa32dd61a3e520664f5a185e66  R96_R1_001.fastq.gz
ee13781c1e0786819cff94071c779ef0  R96_R2_001.fastq.gz
2edae3a28b54c6dc883523c96a3f62fe  R97_R1_001.fastq.gz
75d704624b9cbede2ac67c1f138996ca  R97_R2_001.fastq.gz
63b0abf30af7e76b87d0bf9dd53492f2  R98_R1_001.fastq.gz
48ec3236da746a513c568f7d6be863a5  R98_R2_001.fastq.gz
1b4fa2175c2e696932cd41165cd8bd90  R99_R1_001.fastq.gz
a2d833a7f784675acc8e5b8ade9aad98  R99_R2_001.fastq.gz
```

I then added these lists to a spreadsheet and checked that the cells matched for original and downloaded files. This gave the following results. Next time, I'll do this in a script, but here I did it manually. I also added in the checksums for data that I downloaded to Mox (detailed below). All files are confirmed and everything looks good!     

| file                 | original_azenta                  | downloaded_andromeda             | downloaded_mox                   | match_andromeda | match_mox |
|----------------------|----------------------------------|----------------------------------|----------------------------------|-----------------|-----------|
| R100_R1_001.fastq.gz | fb2f856ffeaf2337acebada68643ce45 | fb2f856ffeaf2337acebada68643ce45 | fb2f856ffeaf2337acebada68643ce45 | TRUE            | TRUE      |
| R100_R2_001.fastq.gz | 38ad7ea4ed9c003f73ba3c29fc7fc83c | 38ad7ea4ed9c003f73ba3c29fc7fc83c | 38ad7ea4ed9c003f73ba3c29fc7fc83c | TRUE            | TRUE      |
| R101_R1_001.fastq.gz | 0f9e5d6157b53737d6b26c6821adfc4d | 0f9e5d6157b53737d6b26c6821adfc4d | 0f9e5d6157b53737d6b26c6821adfc4d | TRUE            | TRUE      |
| R101_R2_001.fastq.gz | 40c1ec83ba83690bf55b7102f1217f61 | 40c1ec83ba83690bf55b7102f1217f61 | 40c1ec83ba83690bf55b7102f1217f61 | TRUE            | TRUE      |
| R102_R1_001.fastq.gz | 5ffbdd93efdd8368ad071c5db3f18a91 | 5ffbdd93efdd8368ad071c5db3f18a91 | 5ffbdd93efdd8368ad071c5db3f18a91 | TRUE            | TRUE      |
| R102_R2_001.fastq.gz | 617fee9820cb009e9b641cc7aaaa9b98 | 617fee9820cb009e9b641cc7aaaa9b98 | 617fee9820cb009e9b641cc7aaaa9b98 | TRUE            | TRUE      |
| R103_R1_001.fastq.gz | dfefd35c89d5d90ade043685d0f7b849 | dfefd35c89d5d90ade043685d0f7b849 | dfefd35c89d5d90ade043685d0f7b849 | TRUE            | TRUE      |
| R103_R2_001.fastq.gz | e21941866ddf94d968689dd60c07c846 | e21941866ddf94d968689dd60c07c846 | e21941866ddf94d968689dd60c07c846 | TRUE            | TRUE      |
| R104_R1_001.fastq.gz | de10a95996f35b9a599d94dca8dd6495 | de10a95996f35b9a599d94dca8dd6495 | de10a95996f35b9a599d94dca8dd6495 | TRUE            | TRUE      |
| R104_R2_001.fastq.gz | 50030888aea12e22ee79f47d65006cac | 50030888aea12e22ee79f47d65006cac | 50030888aea12e22ee79f47d65006cac | TRUE            | TRUE      |
| R105_R1_001.fastq.gz | cfa5f9cb57df8c95903bd6ed0035067b | cfa5f9cb57df8c95903bd6ed0035067b | cfa5f9cb57df8c95903bd6ed0035067b | TRUE            | TRUE      |
| R105_R2_001.fastq.gz | 6749095cf6ec0ca27d6cebed13477e75 | 6749095cf6ec0ca27d6cebed13477e75 | 6749095cf6ec0ca27d6cebed13477e75 | TRUE            | TRUE      |
| R106_R1_001.fastq.gz | 9ffd4dc02a7b57791b87f5da0f46fbb0 | 9ffd4dc02a7b57791b87f5da0f46fbb0 | 9ffd4dc02a7b57791b87f5da0f46fbb0 | TRUE            | TRUE      |
| R106_R2_001.fastq.gz | 7d39bda85955ce9704e3cc7abdb3ce7f | 7d39bda85955ce9704e3cc7abdb3ce7f | 7d39bda85955ce9704e3cc7abdb3ce7f | TRUE            | TRUE      |
| R107_R1_001.fastq.gz | 090ccb4ca651400e805a936ff920d54b | 090ccb4ca651400e805a936ff920d54b | 090ccb4ca651400e805a936ff920d54b | TRUE            | TRUE      |
| R107_R2_001.fastq.gz | dc7f22d773b311f9f605a9ff5677049e | dc7f22d773b311f9f605a9ff5677049e | dc7f22d773b311f9f605a9ff5677049e | TRUE            | TRUE      |
| R108_R1_001.fastq.gz | b5d93b1aa771ab2217cbf49ee15d12a4 | b5d93b1aa771ab2217cbf49ee15d12a4 | b5d93b1aa771ab2217cbf49ee15d12a4 | TRUE            | TRUE      |
| R108_R2_001.fastq.gz | 1fa5be76b43fb96630e9f7917ba0d7a9 | 1fa5be76b43fb96630e9f7917ba0d7a9 | 1fa5be76b43fb96630e9f7917ba0d7a9 | TRUE            | TRUE      |
| R55_R1_001.fastq.gz  | 656ed0ade96334eba2f18a34c54afe13 | 656ed0ade96334eba2f18a34c54afe13 | 656ed0ade96334eba2f18a34c54afe13 | TRUE            | TRUE      |
| R55_R2_001.fastq.gz  | a58c4a5d0cf1d928d77b4e269458fe62 | a58c4a5d0cf1d928d77b4e269458fe62 | a58c4a5d0cf1d928d77b4e269458fe62 | TRUE            | TRUE      |
| R56_R1_001.fastq.gz  | cdd386cae610becdadf4ffd28a035991 | cdd386cae610becdadf4ffd28a035991 | cdd386cae610becdadf4ffd28a035991 | TRUE            | TRUE      |
| R56_R2_001.fastq.gz  | 627a4b677ea068d66457820d7d84fdd7 | 627a4b677ea068d66457820d7d84fdd7 | 627a4b677ea068d66457820d7d84fdd7 | TRUE            | TRUE      |
| R57_R1_001.fastq.gz  | ee79222483a7095a333b1b89eb5ec0cc | ee79222483a7095a333b1b89eb5ec0cc | ee79222483a7095a333b1b89eb5ec0cc | TRUE            | TRUE      |
| R57_R2_001.fastq.gz  | 52b321e37026ee38551dd2bfe689ca36 | 52b321e37026ee38551dd2bfe689ca36 | 52b321e37026ee38551dd2bfe689ca36 | TRUE            | TRUE      |
| R58_R1_001.fastq.gz  | 16c1ac82b21d0b37ed26502f806a0b29 | 16c1ac82b21d0b37ed26502f806a0b29 | 16c1ac82b21d0b37ed26502f806a0b29 | TRUE            | TRUE      |
| R58_R2_001.fastq.gz  | d71d8d697af621daf7c06cfa9ac4f778 | d71d8d697af621daf7c06cfa9ac4f778 | d71d8d697af621daf7c06cfa9ac4f778 | TRUE            | TRUE      |
| R59_R1_001.fastq.gz  | 86f57a12b12e647959ddc97b7f3ad5c4 | 86f57a12b12e647959ddc97b7f3ad5c4 | 86f57a12b12e647959ddc97b7f3ad5c4 | TRUE            | TRUE      |
| R59_R2_001.fastq.gz  | f1668762f80250cbfe0acb4936b1f6e9 | f1668762f80250cbfe0acb4936b1f6e9 | f1668762f80250cbfe0acb4936b1f6e9 | TRUE            | TRUE      |
| R60_R1_001.fastq.gz  | db2b5b1d34316e6f605df732a1474e0d | db2b5b1d34316e6f605df732a1474e0d | db2b5b1d34316e6f605df732a1474e0d | TRUE            | TRUE      |
| R60_R2_001.fastq.gz  | 7d56f82a9cc4cf3dd58b1cfb5d3635c0 | 7d56f82a9cc4cf3dd58b1cfb5d3635c0 | 7d56f82a9cc4cf3dd58b1cfb5d3635c0 | TRUE            | TRUE      |
| R61_R1_001.fastq.gz  | 584a05ee25744aeafce4d9931fa877d4 | 584a05ee25744aeafce4d9931fa877d4 | 584a05ee25744aeafce4d9931fa877d4 | TRUE            | TRUE      |
| R61_R2_001.fastq.gz  | d75439fd1d80dec4209ce6d7441dd1e8 | d75439fd1d80dec4209ce6d7441dd1e8 | d75439fd1d80dec4209ce6d7441dd1e8 | TRUE            | TRUE      |
| R62_R1_001.fastq.gz  | f7eb8374ea83f381aeeb29b6252d8c2c | f7eb8374ea83f381aeeb29b6252d8c2c | f7eb8374ea83f381aeeb29b6252d8c2c | TRUE            | TRUE      |
| R62_R2_001.fastq.gz  | d0b9bd3d1ec7020399a7deb26f1ea037 | d0b9bd3d1ec7020399a7deb26f1ea037 | d0b9bd3d1ec7020399a7deb26f1ea037 | TRUE            | TRUE      |
| R63_R1_001.fastq.gz  | ee882c33154f14fe92b86ddaabbb7628 | ee882c33154f14fe92b86ddaabbb7628 | ee882c33154f14fe92b86ddaabbb7628 | TRUE            | TRUE      |
| R63_R2_001.fastq.gz  | 22bb2b865f92f1daccc8ff7fe6072351 | 22bb2b865f92f1daccc8ff7fe6072351 | 22bb2b865f92f1daccc8ff7fe6072351 | TRUE            | TRUE      |
| R64_R1_001.fastq.gz  | 9997108124197468b7ede97121ecc9de | 9997108124197468b7ede97121ecc9de | 9997108124197468b7ede97121ecc9de | TRUE            | TRUE      |
| R64_R2_001.fastq.gz  | 5bbce1622beacaea94fcc7cd502c0ef2 | 5bbce1622beacaea94fcc7cd502c0ef2 | 5bbce1622beacaea94fcc7cd502c0ef2 | TRUE            | TRUE      |
| R65_R1_001.fastq.gz  | 4c6cce887cec546565a5c8ca9167e047 | 4c6cce887cec546565a5c8ca9167e047 | 4c6cce887cec546565a5c8ca9167e047 | TRUE            | TRUE      |
| R65_R2_001.fastq.gz  | 4fac3b0355cfcd0711fffe2afa861818 | 4fac3b0355cfcd0711fffe2afa861818 | 4fac3b0355cfcd0711fffe2afa861818 | TRUE            | TRUE      |
| R66_R1_001.fastq.gz  | f6c497cff568791b2a88c4f90705bc1e | f6c497cff568791b2a88c4f90705bc1e | f6c497cff568791b2a88c4f90705bc1e | TRUE            | TRUE      |
| R66_R2_001.fastq.gz  | a68eaac8298321b81646cdf50dd0f7c8 | a68eaac8298321b81646cdf50dd0f7c8 | a68eaac8298321b81646cdf50dd0f7c8 | TRUE            | TRUE      |
| R67_R1_001.fastq.gz  | 32a7dd3c9dbfc46932ce4cd33d86f7b4 | 32a7dd3c9dbfc46932ce4cd33d86f7b4 | 32a7dd3c9dbfc46932ce4cd33d86f7b4 | TRUE            | TRUE      |
| R67_R2_001.fastq.gz  | 9f0b7cf1fa8efe953b9134cda3d97c9a | 9f0b7cf1fa8efe953b9134cda3d97c9a | 9f0b7cf1fa8efe953b9134cda3d97c9a | TRUE            | TRUE      |
| R68_R1_001.fastq.gz  | 4f81e378fa78ec720a67308c3f09eaca | 4f81e378fa78ec720a67308c3f09eaca | 4f81e378fa78ec720a67308c3f09eaca | TRUE            | TRUE      |
| R68_R2_001.fastq.gz  | 76e328588a3ba9cf4d64638401988f3a | 76e328588a3ba9cf4d64638401988f3a | 76e328588a3ba9cf4d64638401988f3a | TRUE            | TRUE      |
| R69_R1_001.fastq.gz  | 6dc239cd5053c9f0f42f7c4a61452bb1 | 6dc239cd5053c9f0f42f7c4a61452bb1 | 6dc239cd5053c9f0f42f7c4a61452bb1 | TRUE            | TRUE      |
| R69_R2_001.fastq.gz  | ac76754b6340e68ea96a9dde2e453d7c | ac76754b6340e68ea96a9dde2e453d7c | ac76754b6340e68ea96a9dde2e453d7c | TRUE            | TRUE      |
| R70_R1_001.fastq.gz  | 55322d7fed5e8fa3c57a332797759187 | 55322d7fed5e8fa3c57a332797759187 | 55322d7fed5e8fa3c57a332797759187 | TRUE            | TRUE      |
| R70_R2_001.fastq.gz  | e5786bcb34f286e1cf799145e65f8a1e | e5786bcb34f286e1cf799145e65f8a1e | e5786bcb34f286e1cf799145e65f8a1e | TRUE            | TRUE      |
| R71_R1_001.fastq.gz  | 10d273a9ddd932678c4450d71c3df2dc | 10d273a9ddd932678c4450d71c3df2dc | 10d273a9ddd932678c4450d71c3df2dc | TRUE            | TRUE      |
| R71_R2_001.fastq.gz  | 9a7997165f024a9f7949d254a5249d13 | 9a7997165f024a9f7949d254a5249d13 | 9a7997165f024a9f7949d254a5249d13 | TRUE            | TRUE      |
| R72_R1_001.fastq.gz  | e4f3fdc7576dae75737ed141c121be37 | e4f3fdc7576dae75737ed141c121be37 | e4f3fdc7576dae75737ed141c121be37 | TRUE            | TRUE      |
| R72_R2_001.fastq.gz  | 57cecc3994764db6b4597957b44d972f | 57cecc3994764db6b4597957b44d972f | 57cecc3994764db6b4597957b44d972f | TRUE            | TRUE      |
| R73_R1_001.fastq.gz  | d736f67efcc2a3c7516745fc1a3ccd46 | d736f67efcc2a3c7516745fc1a3ccd46 | d736f67efcc2a3c7516745fc1a3ccd46 | TRUE            | TRUE      |
| R73_R2_001.fastq.gz  | 5c76421457b3901907cf2a9651d6e785 | 5c76421457b3901907cf2a9651d6e785 | 5c76421457b3901907cf2a9651d6e785 | TRUE            | TRUE      |
| R74_R1_001.fastq.gz  | 220f2c21d6aafc2b3019074ea209e260 | 220f2c21d6aafc2b3019074ea209e260 | 220f2c21d6aafc2b3019074ea209e260 | TRUE            | TRUE      |
| R74_R2_001.fastq.gz  | a9f1143d915eea62467319e68fe87f9a | a9f1143d915eea62467319e68fe87f9a | a9f1143d915eea62467319e68fe87f9a | TRUE            | TRUE      |
| R75_R1_001.fastq.gz  | 481c8b51b3b1cf424120f841068ce4d7 | 481c8b51b3b1cf424120f841068ce4d7 | 481c8b51b3b1cf424120f841068ce4d7 | TRUE            | TRUE      |
| R75_R2_001.fastq.gz  | f0e4311f966459b2335873902bd43ba3 | f0e4311f966459b2335873902bd43ba3 | f0e4311f966459b2335873902bd43ba3 | TRUE            | TRUE      |
| R76_R1_001.fastq.gz  | 9076f07b4ad208e2d550f3c94f0c8787 | 9076f07b4ad208e2d550f3c94f0c8787 | 9076f07b4ad208e2d550f3c94f0c8787 | TRUE            | TRUE      |
| R76_R2_001.fastq.gz  | 22368829e561fd298adee8f7551d3705 | 22368829e561fd298adee8f7551d3705 | 22368829e561fd298adee8f7551d3705 | TRUE            | TRUE      |
| R77_R1_001.fastq.gz  | 9d76eb6f1f65b84f8fb8afb09ff935eb | 9d76eb6f1f65b84f8fb8afb09ff935eb | 9d76eb6f1f65b84f8fb8afb09ff935eb | TRUE            | TRUE      |
| R77_R2_001.fastq.gz  | b8d368c154eafa4f0fa427954810bf28 | b8d368c154eafa4f0fa427954810bf28 | b8d368c154eafa4f0fa427954810bf28 | TRUE            | TRUE      |
| R78_R1_001.fastq.gz  | f97ae96e6c4aa9e476af14470ab7928f | f97ae96e6c4aa9e476af14470ab7928f | f97ae96e6c4aa9e476af14470ab7928f | TRUE            | TRUE      |
| R78_R2_001.fastq.gz  | bc33760d2d5a22e7f35403d50aff7815 | bc33760d2d5a22e7f35403d50aff7815 | bc33760d2d5a22e7f35403d50aff7815 | TRUE            | TRUE      |
| R79_R1_001.fastq.gz  | be3d7a5d0ed734fd3b1c87273580eaf1 | be3d7a5d0ed734fd3b1c87273580eaf1 | be3d7a5d0ed734fd3b1c87273580eaf1 | TRUE            | TRUE      |
| R79_R2_001.fastq.gz  | d5cdad1d9b5aade2149c5a2645397793 | d5cdad1d9b5aade2149c5a2645397793 | d5cdad1d9b5aade2149c5a2645397793 | TRUE            | TRUE      |
| R80_R1_001.fastq.gz  | 808fb96f7cf390540e66ed60539501d7 | 808fb96f7cf390540e66ed60539501d7 | 808fb96f7cf390540e66ed60539501d7 | TRUE            | TRUE      |
| R80_R2_001.fastq.gz  | 8640e182f98e19e40d76d4b07da103d1 | 8640e182f98e19e40d76d4b07da103d1 | 8640e182f98e19e40d76d4b07da103d1 | TRUE            | TRUE      |
| R81_R1_001.fastq.gz  | 2d495d29f472dcf73a70f9633867dd2b | 2d495d29f472dcf73a70f9633867dd2b | 2d495d29f472dcf73a70f9633867dd2b | TRUE            | TRUE      |
| R81_R2_001.fastq.gz  | ea358d700478d0699e6476e3cab8b928 | ea358d700478d0699e6476e3cab8b928 | ea358d700478d0699e6476e3cab8b928 | TRUE            | TRUE      |
| R82_R1_001.fastq.gz  | bdbaf3c3b212da61567a1721ce3a8246 | bdbaf3c3b212da61567a1721ce3a8246 | bdbaf3c3b212da61567a1721ce3a8246 | TRUE            | TRUE      |
| R82_R2_001.fastq.gz  | e8584a5f259adf7117cfafd7b6ce709f | e8584a5f259adf7117cfafd7b6ce709f | e8584a5f259adf7117cfafd7b6ce709f | TRUE            | TRUE      |
| R83_R1_001.fastq.gz  | 199c1d6aa30e4d82dc360ee7b48fc121 | 199c1d6aa30e4d82dc360ee7b48fc121 | 199c1d6aa30e4d82dc360ee7b48fc121 | TRUE            | TRUE      |
| R83_R2_001.fastq.gz  | 939bd072fcfa962e9e20936522153ff5 | 939bd072fcfa962e9e20936522153ff5 | 939bd072fcfa962e9e20936522153ff5 | TRUE            | TRUE      |
| R84_R1_001.fastq.gz  | 718f79e244cb8518bdd3f0689f99cd4c | 718f79e244cb8518bdd3f0689f99cd4c | 718f79e244cb8518bdd3f0689f99cd4c | TRUE            | TRUE      |
| R84_R2_001.fastq.gz  | 093521ef06042599a19611baa6122e43 | 093521ef06042599a19611baa6122e43 | 093521ef06042599a19611baa6122e43 | TRUE            | TRUE      |
| R85_R1_001.fastq.gz  | 6f68d6ee2b5d053232fca54ac64a058c | 6f68d6ee2b5d053232fca54ac64a058c | 6f68d6ee2b5d053232fca54ac64a058c | TRUE            | TRUE      |
| R85_R2_001.fastq.gz  | 08801a798da6edb9dd75f3973d01ed0a | 08801a798da6edb9dd75f3973d01ed0a | 08801a798da6edb9dd75f3973d01ed0a | TRUE            | TRUE      |
| R86_R1_001.fastq.gz  | c1a0652008f2025f27f26b2637311c63 | c1a0652008f2025f27f26b2637311c63 | c1a0652008f2025f27f26b2637311c63 | TRUE            | TRUE      |
| R86_R2_001.fastq.gz  | a0572df5065cfbbfb9c5394d3b7bf25e | a0572df5065cfbbfb9c5394d3b7bf25e | a0572df5065cfbbfb9c5394d3b7bf25e | TRUE            | TRUE      |
| R87_R1_001.fastq.gz  | 9373699fd3ce5e5de9c4f7cb9256cb59 | 9373699fd3ce5e5de9c4f7cb9256cb59 | 9373699fd3ce5e5de9c4f7cb9256cb59 | TRUE            | TRUE      |
| R87_R2_001.fastq.gz  | 9d0dfd38caca15854aeace535077a6db | 9d0dfd38caca15854aeace535077a6db | 9d0dfd38caca15854aeace535077a6db | TRUE            | TRUE      |
| R88_R1_001.fastq.gz  | 6ac435c22f801610476e436c7335fee4 | 6ac435c22f801610476e436c7335fee4 | 6ac435c22f801610476e436c7335fee4 | TRUE            | TRUE      |
| R88_R2_001.fastq.gz  | e4a8a0651e7d956817548fd449868558 | e4a8a0651e7d956817548fd449868558 | e4a8a0651e7d956817548fd449868558 | TRUE            | TRUE      |
| R89_R1_001.fastq.gz  | 0edecaaa547e01c7e09b52e3a1630875 | 0edecaaa547e01c7e09b52e3a1630875 | 0edecaaa547e01c7e09b52e3a1630875 | TRUE            | TRUE      |
| R89_R2_001.fastq.gz  | 75562b8b318cdde465aecea9dbbb4a90 | 75562b8b318cdde465aecea9dbbb4a90 | 75562b8b318cdde465aecea9dbbb4a90 | TRUE            | TRUE      |
| R90_R1_001.fastq.gz  | 8fc5fa62c0a901b76b9bf6e61f1fffb3 | 8fc5fa62c0a901b76b9bf6e61f1fffb3 | 8fc5fa62c0a901b76b9bf6e61f1fffb3 | TRUE            | TRUE      |
| R90_R2_001.fastq.gz  | 63597112cc2ae5dc914e3bee2a8d4cdc | 63597112cc2ae5dc914e3bee2a8d4cdc | 63597112cc2ae5dc914e3bee2a8d4cdc | TRUE            | TRUE      |
| R91_R1_001.fastq.gz  | f11e6812f69c1627072ef88103e2a32b | f11e6812f69c1627072ef88103e2a32b | f11e6812f69c1627072ef88103e2a32b | TRUE            | TRUE      |
| R91_R2_001.fastq.gz  | 8dbae98da0fba664b87c8f4d5717e463 | 8dbae98da0fba664b87c8f4d5717e463 | 8dbae98da0fba664b87c8f4d5717e463 | TRUE            | TRUE      |
| R92_R1_001.fastq.gz  | b1800bbcd1d33d183fcd371623426c32 | b1800bbcd1d33d183fcd371623426c32 | b1800bbcd1d33d183fcd371623426c32 | TRUE            | TRUE      |
| R92_R2_001.fastq.gz  | 8f453d98f95e829867149f3c10f5d993 | 8f453d98f95e829867149f3c10f5d993 | 8f453d98f95e829867149f3c10f5d993 | TRUE            | TRUE      |
| R93_R1_001.fastq.gz  | 78d4b9261e66e9d122af3b10f53a5ea9 | 78d4b9261e66e9d122af3b10f53a5ea9 | 78d4b9261e66e9d122af3b10f53a5ea9 | TRUE            | TRUE      |
| R93_R2_001.fastq.gz  | 68f809c166d98cc4376b28c57d09fd9b | 68f809c166d98cc4376b28c57d09fd9b | 68f809c166d98cc4376b28c57d09fd9b | TRUE            | TRUE      |
| R94_R1_001.fastq.gz  | ca9c487cdb136f12e2006bd2222997f4 | ca9c487cdb136f12e2006bd2222997f4 | ca9c487cdb136f12e2006bd2222997f4 | TRUE            | TRUE      |
| R94_R2_001.fastq.gz  | 38ae96c31bd7b536844a1ebebb69df9c | 38ae96c31bd7b536844a1ebebb69df9c | 38ae96c31bd7b536844a1ebebb69df9c | TRUE            | TRUE      |
| R95_R1_001.fastq.gz  | 34a4412186175b4a99513edbab6065f1 | 34a4412186175b4a99513edbab6065f1 | 34a4412186175b4a99513edbab6065f1 | TRUE            | TRUE      |
| R95_R2_001.fastq.gz  | 54b52ca18af223327f731ef8b490878c | 54b52ca18af223327f731ef8b490878c | 54b52ca18af223327f731ef8b490878c | TRUE            | TRUE      |
| R96_R1_001.fastq.gz  | 5865a8aa32dd61a3e520664f5a185e66 | 5865a8aa32dd61a3e520664f5a185e66 | 5865a8aa32dd61a3e520664f5a185e66 | TRUE            | TRUE      |
| R96_R2_001.fastq.gz  | ee13781c1e0786819cff94071c779ef0 | ee13781c1e0786819cff94071c779ef0 | ee13781c1e0786819cff94071c779ef0 | TRUE            | TRUE      |
| R97_R1_001.fastq.gz  | 2edae3a28b54c6dc883523c96a3f62fe | 2edae3a28b54c6dc883523c96a3f62fe | 2edae3a28b54c6dc883523c96a3f62fe | TRUE            | TRUE      |
| R97_R2_001.fastq.gz  | 75d704624b9cbede2ac67c1f138996ca | 75d704624b9cbede2ac67c1f138996ca | 75d704624b9cbede2ac67c1f138996ca | TRUE            | TRUE      |
| R98_R1_001.fastq.gz  | 63b0abf30af7e76b87d0bf9dd53492f2 | 63b0abf30af7e76b87d0bf9dd53492f2 | 63b0abf30af7e76b87d0bf9dd53492f2 | TRUE            | TRUE      |
| R98_R2_001.fastq.gz  | 48ec3236da746a513c568f7d6be863a5 | 48ec3236da746a513c568f7d6be863a5 | 48ec3236da746a513c568f7d6be863a5 | TRUE            | TRUE      |
| R99_R1_001.fastq.gz  | 1b4fa2175c2e696932cd41165cd8bd90 | 1b4fa2175c2e696932cd41165cd8bd90 | 1b4fa2175c2e696932cd41165cd8bd90 | TRUE            | TRUE      |
| R99_R2_001.fastq.gz  | a2d833a7f784675acc8e5b8ade9aad98 | a2d833a7f784675acc8e5b8ade9aad98 | a2d833a7f784675acc8e5b8ade9aad98 | TRUE            | TRUE      |



Data are now downloaded and integrity confirmed on URI Andromeda.  

## Download data to UW Hyak/Mox


```
#logged into UW Hyak/Mox
cd /gscratch/srlab/ashuff
mkdir mcap-2023-rnaseq
cd mcap-2023-rnaseq
mkdir raw-sequences

``` 

The full directory where I want raw sequences to go is `/gscratch/srlab/ashuff/mcap-2023-rnaseq`.  

```
# in Hyak ashuffm folder 

# log into Azenta sftp as directed by Azenta 
# cd into my project folder

#set directory for download
lcd /gscratch/srlab/ashuff/mcap-2023-rnaseq

#download all files into project folder 

mget *
```

Downloaded on Feb 21 2024. 

I then checked for data integrity using md5 checksums. Azenta provided a .md5 file for each sequence file. See the table above for confirmation of md5 checksums.  

```
srun -p srlab -A srlab --time=1:00:00 --mem=100G --pty /bin/bash

md5sum *.fastq.gz > checkmd5_20240221.md5

md5sum -c checkmd5_20240221.md5  
```

Check sums from data downloaded on Mox is here:  

```
fb2f856ffeaf2337acebada68643ce45  R100_R1_001.fastq.gz
38ad7ea4ed9c003f73ba3c29fc7fc83c  R100_R2_001.fastq.gz
0f9e5d6157b53737d6b26c6821adfc4d  R101_R1_001.fastq.gz
40c1ec83ba83690bf55b7102f1217f61  R101_R2_001.fastq.gz
5ffbdd93efdd8368ad071c5db3f18a91  R102_R1_001.fastq.gz
617fee9820cb009e9b641cc7aaaa9b98  R102_R2_001.fastq.gz
dfefd35c89d5d90ade043685d0f7b849  R103_R1_001.fastq.gz
e21941866ddf94d968689dd60c07c846  R103_R2_001.fastq.gz
de10a95996f35b9a599d94dca8dd6495  R104_R1_001.fastq.gz
50030888aea12e22ee79f47d65006cac  R104_R2_001.fastq.gz
cfa5f9cb57df8c95903bd6ed0035067b  R105_R1_001.fastq.gz
6749095cf6ec0ca27d6cebed13477e75  R105_R2_001.fastq.gz
9ffd4dc02a7b57791b87f5da0f46fbb0  R106_R1_001.fastq.gz
7d39bda85955ce9704e3cc7abdb3ce7f  R106_R2_001.fastq.gz
090ccb4ca651400e805a936ff920d54b  R107_R1_001.fastq.gz
dc7f22d773b311f9f605a9ff5677049e  R107_R2_001.fastq.gz
b5d93b1aa771ab2217cbf49ee15d12a4  R108_R1_001.fastq.gz
1fa5be76b43fb96630e9f7917ba0d7a9  R108_R2_001.fastq.gz
656ed0ade96334eba2f18a34c54afe13  R55_R1_001.fastq.gz
a58c4a5d0cf1d928d77b4e269458fe62  R55_R2_001.fastq.gz
cdd386cae610becdadf4ffd28a035991  R56_R1_001.fastq.gz
627a4b677ea068d66457820d7d84fdd7  R56_R2_001.fastq.gz
ee79222483a7095a333b1b89eb5ec0cc  R57_R1_001.fastq.gz
52b321e37026ee38551dd2bfe689ca36  R57_R2_001.fastq.gz
16c1ac82b21d0b37ed26502f806a0b29  R58_R1_001.fastq.gz
d71d8d697af621daf7c06cfa9ac4f778  R58_R2_001.fastq.gz
86f57a12b12e647959ddc97b7f3ad5c4  R59_R1_001.fastq.gz
f1668762f80250cbfe0acb4936b1f6e9  R59_R2_001.fastq.gz
db2b5b1d34316e6f605df732a1474e0d  R60_R1_001.fastq.gz
7d56f82a9cc4cf3dd58b1cfb5d3635c0  R60_R2_001.fastq.gz
584a05ee25744aeafce4d9931fa877d4  R61_R1_001.fastq.gz
d75439fd1d80dec4209ce6d7441dd1e8  R61_R2_001.fastq.gz
f7eb8374ea83f381aeeb29b6252d8c2c  R62_R1_001.fastq.gz
d0b9bd3d1ec7020399a7deb26f1ea037  R62_R2_001.fastq.gz
ee882c33154f14fe92b86ddaabbb7628  R63_R1_001.fastq.gz
22bb2b865f92f1daccc8ff7fe6072351  R63_R2_001.fastq.gz
9997108124197468b7ede97121ecc9de  R64_R1_001.fastq.gz
5bbce1622beacaea94fcc7cd502c0ef2  R64_R2_001.fastq.gz
4c6cce887cec546565a5c8ca9167e047  R65_R1_001.fastq.gz
4fac3b0355cfcd0711fffe2afa861818  R65_R2_001.fastq.gz
f6c497cff568791b2a88c4f90705bc1e  R66_R1_001.fastq.gz
a68eaac8298321b81646cdf50dd0f7c8  R66_R2_001.fastq.gz
32a7dd3c9dbfc46932ce4cd33d86f7b4  R67_R1_001.fastq.gz
9f0b7cf1fa8efe953b9134cda3d97c9a  R67_R2_001.fastq.gz
4f81e378fa78ec720a67308c3f09eaca  R68_R1_001.fastq.gz
76e328588a3ba9cf4d64638401988f3a  R68_R2_001.fastq.gz
6dc239cd5053c9f0f42f7c4a61452bb1  R69_R1_001.fastq.gz
ac76754b6340e68ea96a9dde2e453d7c  R69_R2_001.fastq.gz
55322d7fed5e8fa3c57a332797759187  R70_R1_001.fastq.gz
e5786bcb34f286e1cf799145e65f8a1e  R70_R2_001.fastq.gz
10d273a9ddd932678c4450d71c3df2dc  R71_R1_001.fastq.gz
9a7997165f024a9f7949d254a5249d13  R71_R2_001.fastq.gz
e4f3fdc7576dae75737ed141c121be37  R72_R1_001.fastq.gz
57cecc3994764db6b4597957b44d972f  R72_R2_001.fastq.gz
d736f67efcc2a3c7516745fc1a3ccd46  R73_R1_001.fastq.gz
5c76421457b3901907cf2a9651d6e785  R73_R2_001.fastq.gz
220f2c21d6aafc2b3019074ea209e260  R74_R1_001.fastq.gz
a9f1143d915eea62467319e68fe87f9a  R74_R2_001.fastq.gz
481c8b51b3b1cf424120f841068ce4d7  R75_R1_001.fastq.gz
f0e4311f966459b2335873902bd43ba3  R75_R2_001.fastq.gz
9076f07b4ad208e2d550f3c94f0c8787  R76_R1_001.fastq.gz
22368829e561fd298adee8f7551d3705  R76_R2_001.fastq.gz
9d76eb6f1f65b84f8fb8afb09ff935eb  R77_R1_001.fastq.gz
b8d368c154eafa4f0fa427954810bf28  R77_R2_001.fastq.gz
f97ae96e6c4aa9e476af14470ab7928f  R78_R1_001.fastq.gz
bc33760d2d5a22e7f35403d50aff7815  R78_R2_001.fastq.gz
be3d7a5d0ed734fd3b1c87273580eaf1  R79_R1_001.fastq.gz
d5cdad1d9b5aade2149c5a2645397793  R79_R2_001.fastq.gz
808fb96f7cf390540e66ed60539501d7  R80_R1_001.fastq.gz
8640e182f98e19e40d76d4b07da103d1  R80_R2_001.fastq.gz
2d495d29f472dcf73a70f9633867dd2b  R81_R1_001.fastq.gz
ea358d700478d0699e6476e3cab8b928  R81_R2_001.fastq.gz
bdbaf3c3b212da61567a1721ce3a8246  R82_R1_001.fastq.gz
e8584a5f259adf7117cfafd7b6ce709f  R82_R2_001.fastq.gz
199c1d6aa30e4d82dc360ee7b48fc121  R83_R1_001.fastq.gz
939bd072fcfa962e9e20936522153ff5  R83_R2_001.fastq.gz
718f79e244cb8518bdd3f0689f99cd4c  R84_R1_001.fastq.gz
093521ef06042599a19611baa6122e43  R84_R2_001.fastq.gz
6f68d6ee2b5d053232fca54ac64a058c  R85_R1_001.fastq.gz
08801a798da6edb9dd75f3973d01ed0a  R85_R2_001.fastq.gz
c1a0652008f2025f27f26b2637311c63  R86_R1_001.fastq.gz
a0572df5065cfbbfb9c5394d3b7bf25e  R86_R2_001.fastq.gz
9373699fd3ce5e5de9c4f7cb9256cb59  R87_R1_001.fastq.gz
9d0dfd38caca15854aeace535077a6db  R87_R2_001.fastq.gz
6ac435c22f801610476e436c7335fee4  R88_R1_001.fastq.gz
e4a8a0651e7d956817548fd449868558  R88_R2_001.fastq.gz
0edecaaa547e01c7e09b52e3a1630875  R89_R1_001.fastq.gz
75562b8b318cdde465aecea9dbbb4a90  R89_R2_001.fastq.gz
8fc5fa62c0a901b76b9bf6e61f1fffb3  R90_R1_001.fastq.gz
63597112cc2ae5dc914e3bee2a8d4cdc  R90_R2_001.fastq.gz
f11e6812f69c1627072ef88103e2a32b  R91_R1_001.fastq.gz
8dbae98da0fba664b87c8f4d5717e463  R91_R2_001.fastq.gz
b1800bbcd1d33d183fcd371623426c32  R92_R1_001.fastq.gz
8f453d98f95e829867149f3c10f5d993  R92_R2_001.fastq.gz
78d4b9261e66e9d122af3b10f53a5ea9  R93_R1_001.fastq.gz
68f809c166d98cc4376b28c57d09fd9b  R93_R2_001.fastq.gz
ca9c487cdb136f12e2006bd2222997f4  R94_R1_001.fastq.gz
38ae96c31bd7b536844a1ebebb69df9c  R94_R2_001.fastq.gz
34a4412186175b4a99513edbab6065f1  R95_R1_001.fastq.gz
54b52ca18af223327f731ef8b490878c  R95_R2_001.fastq.gz
5865a8aa32dd61a3e520664f5a185e66  R96_R1_001.fastq.gz
ee13781c1e0786819cff94071c779ef0  R96_R2_001.fastq.gz
2edae3a28b54c6dc883523c96a3f62fe  R97_R1_001.fastq.gz
75d704624b9cbede2ac67c1f138996ca  R97_R2_001.fastq.gz
63b0abf30af7e76b87d0bf9dd53492f2  R98_R1_001.fastq.gz
48ec3236da746a513c568f7d6be863a5  R98_R2_001.fastq.gz
1b4fa2175c2e696932cd41165cd8bd90  R99_R1_001.fastq.gz
a2d833a7f784675acc8e5b8ade9aad98  R99_R2_001.fastq.gz
```

Output from checksums is here: 

```
R100_R1_001.fastq.gz: OK
R100_R2_001.fastq.gz: OK
R101_R1_001.fastq.gz: OK
R101_R2_001.fastq.gz: OK
R102_R1_001.fastq.gz: OK
R102_R2_001.fastq.gz: OK
R103_R1_001.fastq.gz: OK
R103_R2_001.fastq.gz: OK
R104_R1_001.fastq.gz: OK
R104_R2_001.fastq.gz: OK
R105_R1_001.fastq.gz: OK
R105_R2_001.fastq.gz: OK
R106_R1_001.fastq.gz: OK
R106_R2_001.fastq.gz: OK
R107_R1_001.fastq.gz: OK
R107_R2_001.fastq.gz: OK
R108_R1_001.fastq.gz: OK
R108_R2_001.fastq.gz: OK
R55_R1_001.fastq.gz: OK
R55_R2_001.fastq.gz: OK
R56_R1_001.fastq.gz: OK
R56_R2_001.fastq.gz: OK
R57_R1_001.fastq.gz: OK
R57_R2_001.fastq.gz: OK
R58_R1_001.fastq.gz: OK
R58_R2_001.fastq.gz: OK
R59_R1_001.fastq.gz: OK
R59_R2_001.fastq.gz: OK
R60_R1_001.fastq.gz: OK
R60_R2_001.fastq.gz: OK
R61_R1_001.fastq.gz: OK
R61_R2_001.fastq.gz: OK
R62_R1_001.fastq.gz: OK
R62_R2_001.fastq.gz: OK
R63_R1_001.fastq.gz: OK
R63_R2_001.fastq.gz: OK
R64_R1_001.fastq.gz: OK
R64_R2_001.fastq.gz: OK
R65_R1_001.fastq.gz: OK
R65_R2_001.fastq.gz: OK
R66_R1_001.fastq.gz: OK
R66_R2_001.fastq.gz: OK
R67_R1_001.fastq.gz: OK
R67_R2_001.fastq.gz: OK
R68_R1_001.fastq.gz: OK
R68_R2_001.fastq.gz: OK
R69_R1_001.fastq.gz: OK
R69_R2_001.fastq.gz: OK
R70_R1_001.fastq.gz: OK
R70_R2_001.fastq.gz: OK
R71_R1_001.fastq.gz: OK
R71_R2_001.fastq.gz: OK
R72_R1_001.fastq.gz: OK
R72_R2_001.fastq.gz: OK
R73_R1_001.fastq.gz: OK
R73_R2_001.fastq.gz: OK
R74_R1_001.fastq.gz: OK
R74_R2_001.fastq.gz: OK
R75_R1_001.fastq.gz: OK
R75_R2_001.fastq.gz: OK
R76_R1_001.fastq.gz: OK
R76_R2_001.fastq.gz: OK
R77_R1_001.fastq.gz: OK
R77_R2_001.fastq.gz: OK
R78_R1_001.fastq.gz: OK
R78_R2_001.fastq.gz: OK
R79_R1_001.fastq.gz: OK
R79_R2_001.fastq.gz: OK
R80_R1_001.fastq.gz: OK
R80_R2_001.fastq.gz: OK
R81_R1_001.fastq.gz: OK
R81_R2_001.fastq.gz: OK
R82_R1_001.fastq.gz: OK
R82_R2_001.fastq.gz: OK
R83_R1_001.fastq.gz: OK
R83_R2_001.fastq.gz: OK
R84_R1_001.fastq.gz: OK
R84_R2_001.fastq.gz: OK
R85_R1_001.fastq.gz: OK
R85_R2_001.fastq.gz: OK
R86_R1_001.fastq.gz: OK
R86_R2_001.fastq.gz: OK
R87_R1_001.fastq.gz: OK
R87_R2_001.fastq.gz: OK
R88_R1_001.fastq.gz: OK
R88_R2_001.fastq.gz: OK
R89_R1_001.fastq.gz: OK
R89_R2_001.fastq.gz: OK
R90_R1_001.fastq.gz: OK
R90_R2_001.fastq.gz: OK
R91_R1_001.fastq.gz: OK
R91_R2_001.fastq.gz: OK
R92_R1_001.fastq.gz: OK
R92_R2_001.fastq.gz: OK
R93_R1_001.fastq.gz: OK
R93_R2_001.fastq.gz: OK
R94_R1_001.fastq.gz: OK
R94_R2_001.fastq.gz: OK
R95_R1_001.fastq.gz: OK
R95_R2_001.fastq.gz: OK
R96_R1_001.fastq.gz: OK
R96_R2_001.fastq.gz: OK
R97_R1_001.fastq.gz: OK
R97_R2_001.fastq.gz: OK
R98_R1_001.fastq.gz: OK
R98_R2_001.fastq.gz: OK
R99_R1_001.fastq.gz: OK
R99_R2_001.fastq.gz: OK
```

Everything looks good and all data files were transferred correctly.   

Files are now stored on both URI and UW servers. 

# Transfer data from URI server to NCBI SRA 
 
I then transferred files to NCBI SRA from URI Andromeda. 

I first made a folder with sym links to only the .fastq.gz files.  

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq
mkdir ncbi_upload
cd ncbi_upload

#sym link files
ln -s /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/*gz /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/ncbi_upload
```

The destination of files for upload is now `data/putnamlab/ashuffmyer/mcap-2023-rnaseq/ncbi_upload`.  

I then clicked the "FTP" option for preloaded folder on the NCBI SRA submission page and followed instructions for uploading files.  

```
cd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/ncbi_upload

ftp -i 

open ftp-private.ncbi.nlm.nih.gov

#enter name and password given on SRA webpage

cd uploads/ashuffmyer_gmail.com_bsKvx0RY

mkdir mcap-2023-rnaseq-upload

cd mcap-2023-rnaseq-upload

mput * 

```

This moves all files from my folder on Andromeda to the NCBI upload folder using the FTP.  

The upload to SRA will proceed for each file with messages “transfer complete” when each is uploaded. Keep computer active until all uploads are finished.  

Continue with the submission by selecting the preload folder on SRA once all 108 files registered.   

RNA-Seq sequence files were submitted under SRA SUB14259382. 

I will come back and add the bioaccession values HERE once they are approved.  

All information [added to the Putnam Lab sequence inventory here](https://docs.google.com/spreadsheets/d/1qDGGpLFcmoO-fIFOPSUhPcxi4ErXIq2PkQbxvCCzI40/edit#gid=0).  