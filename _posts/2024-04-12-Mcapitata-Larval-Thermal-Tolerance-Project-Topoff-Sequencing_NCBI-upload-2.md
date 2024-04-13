---
layout: post
title: Raw sequence downloads and NCBI SRA uploads for M. capitata larval thermal tolerance project top off sequencing second upload
date: '2024-04-12'
categories: Larval_Symbiont_TPC_2023
tags: Bioinformatics Mcapitata Molecular GeneExpression
---

This post details the NCBI Sequence Read Archive upload for my *Montipora capitata* 2023 larval thermal tolerance project. See my [notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#larval-symbiont-tpc-2023) and my [GitHub repo](https://github.com/AHuffmyer/larval_symbiont_TPC) for information on this project. 

I previously uploaded and QC'd data from our first round of sequencing in this project [with downloads detailed here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Mcapitata-Larval-Thermal-Tolerance-Project-NCBI-upload/) and [QC information here](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/RNAseq-QC-for-Mcapitata-Larval-Thermal-Tolerance-Project/).   

Due to low read depth in some samples (13 samples at <20M reads, 7 samples at <15M reads), Azenta performed top off sequencing to meet deliverables for this project. Azenta added additional sequencing for 13 total samples. These samples were distributed across treatments.  

In the first round of sequencing (Feb 2024), samples were sequenced on one lane of a NovaX-25B instrument. The top off samples were also performed on one lane of the same instrument on 4/11/2024. 

This post details downloads and SRA uploads for the new sequencing files. 

# 1. Download top off data from Azenta 

## View result statistics and metadata 

I used Azenta's sFTP instructions to view the project results. In their online storage, they provided an .html with an overview of statistics and the R1 and R2 .fastq.gz files for each sample. There are also .md5 files for each .fastq.gz file.  

```
sftp ashuff_uw@sftp.genewiz.com
#entered password from email

lcd MyProjects/larval_symbiont_TPC/data/rna_seq
cd 30-943303755
cd second_delivery
mget Azenta_30-943303755_Data_Report.html
```

I renamed this file to have "_second" on the title and retained the original file for the first round of sequencing with the file name `Azenta_30-943303755_Data_Report.html`. 

The file for the second sequencing report [is on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/Azenta_30-943303755_Data_Report_second.html).  

**Overall statistics**

- Number of samples = 13 
- number reads = 145,090,062
- Yield (Mbases) = 43,525	
- Mean quality scores = 38.00
- % bases over 30 quality score = 90.00

**More detailed statistics**   

I downloaded the report statistics [available on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/report_statistics_second.csv) as well as the [full report](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/Azenta_30-943303755_Data_Report_second.html) and added this data to the [sample metadata on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/sample_rnaseq_metadata.csv).   

- All samples had >37 mean quality scores 
- Yeild (Mbases) ranged from ~500-10,000 per sample with an additional 1.6-33 million reads per sample. 
- 89+% bases had quality scores over 30 

Here is the summary of the second round of sequencing:  

| Project      | Sample ID | Barcode Sequence  | # Reads  | Yield (Mbases) | Mean Quality Score | % Bases >= 30 |
|--------------|-----------|-------------------|----------|----------------|--------------------|---------------|
| 30-943303755 | R107      | AAGCGACT+CTTCGCCT | 17533977 | 5260           | 38                 | 90.31         |
| 30-943303755 | R55       | TTCCTCCT+AGGCTATA | 2876549  | 863            | 38.01              | 90.43         |
| 30-943303755 | R56       | TTCCTCCT+GCCTCTAT | 1661094  | 498            | 37.94              | 90.06         |
| 30-943303755 | R57       | TTCCTCCT+AGGATAGG | 3287827  | 986            | 38.08              | 90.73         |
| 30-943303755 | R58       | TTCCTCCT+TCAGAGCC | 1373160  | 412            | 38.09              | 90.78         |
| 30-943303755 | R59       | TTCCTCCT+CTTCGCCT | 33684113 | 10105          | 37.96              | 90.15         |
| 30-943303755 | R60       | TTCCTCCT+TAAGATTA | 2530567  | 759            | 38                 | 90.36         |
| 30-943303755 | R62       | TTCCTCCT+GTCAGTAC | 2313143  | 694            | 38.01              | 90.39         |
| 30-943303755 | R67       | TGCTTGCT+CTTCGCCT | 17430173 | 5229           | 37.9               | 89.84         |
| 30-943303755 | R75       | GGTGATGA+CTTCGCCT | 11054008 | 3316           | 37.99              | 90.27         |
| 30-943303755 | R83       | AACCTACG+CTTCGCCT | 17612372 | 5284           | 37.76              | 89.18         |
| 30-943303755 | R91       | GGATCTGA+CTTCGCCT | 17511498 | 5253           | 38.03              | 90.44         |
| 30-943303755 | R99       | TGATCACG+CTTCGCCT | 16221581 | 4866           | 37.47              | 87.83         |

I then added this data to the sample metadata and generated a column for total reads combined from first and second sequencing.  

| date     | sample | larvae | temperature | symbiont    | parent      | phenotype            | code                    | Barcode Sequence  | # Reads  | Yield (Mbases) | Mean Quality Score | % Bases >= 30 | ncbi_bioproject | ncbi_accession | resequenced | second_barcode    | second_reads | second_yield | second_mean_quality | second_bases_30 | second_ncbi_bioproject | second_ncbi_accession | total_reads |
|----------|--------|--------|-------------|-------------|-------------|----------------------|-------------------------|-------------------|----------|----------------|--------------------|---------------|-----------------|----------------|-------------|-------------------|--------------|--------------|---------------------|-----------------|------------------------|-----------------------|-------------|
| 20230627 | R100   | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+TAAGATTA | 34566278 | 10370          | 37.79              | 89.32         | PRJNA1078313    | SAMN40082680   |             |                   |              |              |                     |                 |                        |                       | 34566278    |
| 20230627 | R101   | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+ACGTCCTG | 38457421 | 11538          | 37.79              | 89.29         | PRJNA1078313    | SAMN40082681   |             |                   |              |              |                     |                 |                        |                       | 38457421    |
| 20230627 | R102   | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+GTCAGTAC | 33729404 | 10119          | 37.79              | 89.27         | PRJNA1078313    | SAMN40082682   |             |                   |              |              |                     |                 |                        |                       | 33729404    |
| 20230627 | R103   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+AGGCTATA | 21980396 | 6594           | 38.01              | 90.36         | PRJNA1078313    | SAMN40082683   |             |                   |              |              |                     |                 |                        |                       | 21980396    |
| 20230627 | R104   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+GCCTCTAT | 30634968 | 9191           | 37.91              | 89.86         | PRJNA1078313    | SAMN40082684   |             |                   |              |              |                     |                 |                        |                       | 30634968    |
| 20230627 | R105   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+AGGATAGG | 29434310 | 8830           | 37.84              | 89.51         | PRJNA1078313    | SAMN40082685   |             |                   |              |              |                     |                 |                        |                       | 29434310    |
| 20230627 | R106   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+TCAGAGCC | 34929945 | 10479          | 37.94              | 89.97         | PRJNA1078313    | SAMN40082686   |             |                   |              |              |                     |                 |                        |                       | 34929945    |
| 20230627 | R107   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+CTTCGCCT | 6719567  | 2016           | 38.15              | 90.98         | PRJNA1078313    | SAMN40082687   | yes         | AAGCGACT+CTTCGCCT | 17533977     | 5260         | 38                  | 90.31           |                        |                       | 24253544    |
| 20230627 | R108   | 50     | 33          | Wildtype    | Wildtype    | Wildtype             | Wildtype_33             | AAGCGACT+TAAGATTA | 29304668 | 8791           | 37.95              | 90.04         | PRJNA1078313    | SAMN40082688   |             |                   |              |              |                     |                 |                        |                       | 29304668    |
| 20230627 | R55    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+AGGCTATA | 15818670 | 4746           | 37.8               | 89.36         | PRJNA1078313    | SAMN40082635   | yes         | TTCCTCCT+AGGCTATA | 2876549      | 863          | 38.01               | 90.43           |                        |                       | 18695219    |
| 20230627 | R56    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+GCCTCTAT | 17457755 | 5238           | 37.58              | 88.32         | PRJNA1078313    | SAMN40082636   | yes         | TTCCTCCT+GCCTCTAT | 1661094      | 498          | 37.94               | 90.06           |                        |                       | 19118849    |
| 20230627 | R57    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+AGGATAGG | 15232862 | 4570           | 37.7               | 88.88         | PRJNA1078313    | SAMN40082637   | yes         | TTCCTCCT+AGGATAGG | 3287827      | 986          | 38.08               | 90.73           |                        |                       | 18520689    |
| 20230627 | R58    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+TCAGAGCC | 17987445 | 5396           | 37.73              | 89.03         | PRJNA1078313    | SAMN40082638   | yes         | TTCCTCCT+TCAGAGCC | 1373160      | 412          | 38.09               | 90.78           |                        |                       | 19360605    |
| 20230627 | R59    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+CTTCGCCT | 6002991  | 1801           | 37.96              | 90.11         | PRJNA1078313    | SAMN40082639   | yes         | TTCCTCCT+CTTCGCCT | 33684113     | 10105        | 37.96               | 90.15           |                        |                       | 39687104    |
| 20230627 | R60    | 50     | 27          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_27 | TTCCTCCT+TAAGATTA | 16284649 | 4885           | 37.63              | 88.54         | PRJNA1078313    | SAMN40082640   | yes         | TTCCTCCT+TAAGATTA | 2530567      | 759          | 38                  | 90.36           |                        |                       | 18815216    |
| 20230627 | R61    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TTCCTCCT+ACGTCCTG | 19891057 | 5967           | 37.73              | 89.05         | PRJNA1078313    | SAMN40082641   |             |                   |              |              |                     |                 |                        |                       | 19891057    |
| 20230627 | R62    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TTCCTCCT+GTCAGTAC | 16675688 | 5003           | 37.64              | 88.59         | PRJNA1078313    | SAMN40082642   | yes         | TTCCTCCT+GTCAGTAC | 2313143      | 694          | 38.01               | 90.39           |                        |                       | 18988831    |
| 20230627 | R63    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+AGGCTATA | 23828671 | 7149           | 38.01              | 90.33         | PRJNA1078313    | SAMN40082643   |             |                   |              |              |                     |                 |                        |                       | 23828671    |
| 20230627 | R64    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+GCCTCTAT | 32277087 | 9683           | 37.93              | 89.96         | PRJNA1078313    | SAMN40082644   |             |                   |              |              |                     |                 |                        |                       | 32277087    |
| 20230627 | R65    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+AGGATAGG | 32099169 | 9630           | 37.78              | 89.27         | PRJNA1078313    | SAMN40082645   |             |                   |              |              |                     |                 |                        |                       | 32099169    |
| 20230627 | R66    | 50     | 27          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_27    | TGCTTGCT+TCAGAGCC | 32272569 | 9682           | 37.96              | 90.08         | PRJNA1078313    | SAMN40082646   |             |                   |              |              |                     |                 |                        |                       | 32272569    |
| 20230627 | R67    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+CTTCGCCT | 7665773  | 2300           | 38.07              | 90.62         | PRJNA1078313    | SAMN40082647   | yes         | TGCTTGCT+CTTCGCCT | 17430173     | 5229         | 37.9                | 89.84           |                        |                       | 25095946    |
| 20230627 | R68    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+TAAGATTA | 30934325 | 9280           | 37.92              | 89.89         | PRJNA1078313    | SAMN40082648   |             |                   |              |              |                     |                 |                        |                       | 30934325    |
| 20230627 | R69    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+ACGTCCTG | 39517309 | 11856          | 37.9               | 89.77         | PRJNA1078313    | SAMN40082649   |             |                   |              |              |                     |                 |                        |                       | 39517309    |
| 20230627 | R70    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | TGCTTGCT+GTCAGTAC | 34280767 | 10284          | 37.88              | 89.7          | PRJNA1078313    | SAMN40082650   |             |                   |              |              |                     |                 |                        |                       | 34280767    |
| 20230627 | R71    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | GGTGATGA+AGGCTATA | 27838736 | 8352           | 37.99              | 90.27         | PRJNA1078313    | SAMN40082651   |             |                   |              |              |                     |                 |                        |                       | 27838736    |
| 20230627 | R72    | 50     | 27          | Wildtype    | Wildtype    | Wildtype             | Wildtype_27             | GGTGATGA+GCCTCTAT | 33950055 | 10185          | 37.81              | 89.34         | PRJNA1078313    | SAMN40082652   |             |                   |              |              |                     |                 |                        |                       | 33950055    |
| 20230627 | R73    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+AGGATAGG | 35173243 | 10552          | 37.88              | 89.71         | PRJNA1078313    | SAMN40082653   |             |                   |              |              |                     |                 |                        |                       | 35173243    |
| 20230627 | R74    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+TCAGAGCC | 34922263 | 10476          | 37.96              | 90.07         | PRJNA1078313    | SAMN40082654   |             |                   |              |              |                     |                 |                        |                       | 34922263    |
| 20230627 | R75    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+CTTCGCCT | 9871112  | 2961           | 38.12              | 90.85         | PRJNA1078313    | SAMN40082655   | yes         | GGTGATGA+CTTCGCCT | 11054008     | 3316         | 37.99               | 90.27           |                        |                       | 20925120    |
| 20230627 | R76    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+TAAGATTA | 33036284 | 9911           | 37.95              | 90.02         | PRJNA1078313    | SAMN40082656   |             |                   |              |              |                     |                 |                        |                       | 33036284    |
| 20230627 | R77    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+ACGTCCTG | 39746840 | 11924          | 37.98              | 90.17         | PRJNA1078313    | SAMN40082657   |             |                   |              |              |                     |                 |                        |                       | 39746840    |
| 20230627 | R78    | 50     | 30          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_30 | GGTGATGA+GTCAGTAC | 35398354 | 10619          | 37.86              | 89.61         | PRJNA1078313    | SAMN40082658   |             |                   |              |              |                     |                 |                        |                       | 35398354    |
| 20230627 | R79    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+AGGCTATA | 25009225 | 7502           | 37.93              | 89.95         | PRJNA1078313    | SAMN40082659   |             |                   |              |              |                     |                 |                        |                       | 25009225    |
| 20230627 | R80    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+GCCTCTAT | 36133089 | 10840          | 37.85              | 89.53         | PRJNA1078313    | SAMN40082660   |             |                   |              |              |                     |                 |                        |                       | 36133089    |
| 20230627 | R81    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+AGGATAGG | 33493354 | 10048          | 37.78              | 89.2          | PRJNA1078313    | SAMN40082661   |             |                   |              |              |                     |                 |                        |                       | 33493354    |
| 20230627 | R82    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+TCAGAGCC | 35452563 | 10636          | 37.92              | 89.85         | PRJNA1078313    | SAMN40082662   |             |                   |              |              |                     |                 |                        |                       | 35452563    |
| 20230627 | R83    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+CTTCGCCT | 7497048  | 2249           | 38.06              | 90.57         | PRJNA1078313    | SAMN40082663   | yes         | AACCTACG+CTTCGCCT | 17612372     | 5284         | 37.76               | 89.18           |                        |                       | 25109420    |
| 20230627 | R84    | 50     | 30          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_30    | AACCTACG+TAAGATTA | 32220467 | 9666           | 37.92              | 89.86         | PRJNA1078313    | SAMN40082664   |             |                   |              |              |                     |                 |                        |                       | 32220467    |
| 20230627 | R85    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | AACCTACG+ACGTCCTG | 36878406 | 11063          | 37.84              | 89.46         | PRJNA1078313    | SAMN40082665   |             |                   |              |              |                     |                 |                        |                       | 36878406    |
| 20230627 | R86    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | AACCTACG+GTCAGTAC | 32691940 | 9808           | 37.76              | 89.14         | PRJNA1078313    | SAMN40082666   |             |                   |              |              |                     |                 |                        |                       | 32691940    |
| 20230627 | R87    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+AGGCTATA | 24408673 | 7323           | 38.09              | 90.72         | PRJNA1078313    | SAMN40082667   |             |                   |              |              |                     |                 |                        |                       | 24408673    |
| 20230627 | R88    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+GCCTCTAT | 30584889 | 9175           | 37.85              | 89.56         | PRJNA1078313    | SAMN40082668   |             |                   |              |              |                     |                 |                        |                       | 30584889    |
| 20230627 | R89    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+AGGATAGG | 33121528 | 9936           | 37.87              | 89.65         | PRJNA1078313    | SAMN40082669   |             |                   |              |              |                     |                 |                        |                       | 33121528    |
| 20230627 | R90    | 50     | 30          | Wildtype    | Wildtype    | Wildtype             | Wildtype_30             | GGATCTGA+TCAGAGCC | 31879658 | 9564           | 38.03              | 90.37         | PRJNA1078313    | SAMN40082670   |             |                   |              |              |                     |                 |                        |                       | 31879658    |
| 20230627 | R91    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+CTTCGCCT | 6934253  | 2080           | 38.01              | 90.36         | PRJNA1078313    | SAMN40082671   | yes         | GGATCTGA+CTTCGCCT | 17511498     | 5253         | 38.03               | 90.44           |                        |                       | 24445751    |
| 20230627 | R92    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+TAAGATTA | 28565956 | 8570           | 37.93              | 89.93         | PRJNA1078313    | SAMN40082672   |             |                   |              |              |                     |                 |                        |                       | 28565956    |
| 20230627 | R93    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+ACGTCCTG | 31620062 | 9486           | 38.02              | 90.37         | PRJNA1078313    | SAMN40082673   |             |                   |              |              |                     |                 |                        |                       | 31620062    |
| 20230627 | R94    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | GGATCTGA+GTCAGTAC | 32531274 | 9759           | 37.87              | 89.62         | PRJNA1078313    | SAMN40082674   |             |                   |              |              |                     |                 |                        |                       | 32531274    |
| 20230627 | R95    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | TGATCACG+AGGCTATA | 27375934 | 8213           | 37.92              | 89.92         | PRJNA1078313    | SAMN40082675   |             |                   |              |              |                     |                 |                        |                       | 27375934    |
| 20230627 | R96    | 50     | 33          | Cladocopium | Bleached    | Bleached_Cladocopium | Bleached_Cladocopium_33 | TGATCACG+GCCTCTAT | 35882529 | 10765          | 37.78              | 89.22         | PRJNA1078313    | SAMN40082676   |             |                   |              |              |                     |                 |                        |                       | 35882529    |
| 20230627 | R97    | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+AGGATAGG | 33376891 | 10013          | 37.77              | 89.17         | PRJNA1078313    | SAMN40082677   |             |                   |              |              |                     |                 |                        |                       | 33376891    |
| 20230627 | R98    | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+TCAGAGCC | 35899983 | 10770          | 37.86              | 89.63         | PRJNA1078313    | SAMN40082678   |             |                   |              |              |                     |                 |                        |                       | 35899983    |
| 20230627 | R99    | 50     | 33          | Mixed       | Nonbleached | Nonbleached_Mixed    | Nonbleached_Mixed_33    | TGATCACG+CTTCGCCT | 8420607  | 2526           | 37.83              | 89.57         | PRJNA1078313    | SAMN40082679   | yes         | TGATCACG+CTTCGCCT | 16221581     | 4866         | 37.47               | 87.83           |                        |                       | 24642188    |

All samples now have >18 M reads ranging from ~18M to ~35M. This is great! It increased read depth for the samples that previously had <12M reads.  

## Download sequences to URI server 

Prepare folders in Andromeda directory.     

```
#logged into Andromeda 
cd /data/putnamlab/ashuffmyer
cd mcap-2023-rnaseq
cd raw-sequences
mkdir second_sequencing
```

Now the directory that I want sequences in is `/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/second_sequencing`.    

```
# in andromeda second_sequencing folder 

pwd 
/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/second_sequencing

# log into Azenta sftp while logged into Andromeda as directed by Azenta and navigate to sequence folder 00_fastq

#set directory for download
lcd /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/second_sequencing

#download all files in sequence folder 
mget *
```

Downloaded to URI Andromeda on April 12 2024.    

I then checked for data integrity using md5 checksums.    

```
md5sum *.fastq.gz > checkmd5_20240412.md5
md5sum -c checkmd5_20240412.md5

```

Output was as follows:   

```
R107_R1_001.fastq.gz: OK
R107_R2_001.fastq.gz: OK
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
R62_R1_001.fastq.gz: OK
R62_R2_001.fastq.gz: OK
R67_R1_001.fastq.gz: OK
R67_R2_001.fastq.gz: OK
R75_R1_001.fastq.gz: OK
R75_R2_001.fastq.gz: OK
R83_R1_001.fastq.gz: OK
R83_R2_001.fastq.gz: OK
R91_R1_001.fastq.gz: OK
R91_R2_001.fastq.gz: OK
R99_R1_001.fastq.gz: OK
R99_R2_001.fastq.gz: OK
```

Azenta provided a .md5 file for each sequence file. I then compared generated checksums to these original files to confirm data integrity and content is the same after the transfer from Azenta. 

```
#bind together all .md5 files provided by Azenta 

cat *.gz.md5 > azenta_original_checksums_second.md5
```

This provided the following list of the original checksums:  

```
less azenta_original_checksums_second.md5

6ee514bdc1bacfb591629a3edf82bcd4  ./R107_R1_001.fastq.gz
3d6b00afc053f2eb770f83c5830f699e  ./R107_R2_001.fastq.gz
16aba2b715ceeb2c2f3a79dbc855e768  ./R55_R1_001.fastq.gz
14e32d9fccd55fd81a3468a4fd5766d1  ./R55_R2_001.fastq.gz
7fbb16f6f920e4a58431cd91e14b912b  ./R56_R1_001.fastq.gz
80bd51fd98400da3fb91b1182252f577  ./R56_R2_001.fastq.gz
aa6b3bd9617c10ca9b504309cc1ff047  ./R57_R1_001.fastq.gz
c52e1d5d9c7caa2189eaedf81944dfc7  ./R57_R2_001.fastq.gz
027601dabbfc6e48829efe5b931e6e05  ./R58_R1_001.fastq.gz
b1e2605ec7c35ad3c25ad7ab03cb4d2f  ./R58_R2_001.fastq.gz
a62cfd57273efefcfc4076e66cb5da1c  ./R59_R1_001.fastq.gz
dfa3021694fd5ddc01c387eafe34a0d9  ./R59_R2_001.fastq.gz
ba4c0c4c8dede34d546ee7ec5293553b  ./R60_R1_001.fastq.gz
dbf68528a9ee3428d378275c83458b24  ./R60_R2_001.fastq.gz
9ae8dc138d0df94197388b9da117f658  ./R62_R1_001.fastq.gz
96e1b11cb6e60415dc6f69fb4849651e  ./R62_R2_001.fastq.gz
0d2b1896bf85c0800a3a3482f34c90ca  ./R67_R1_001.fastq.gz
cbb14c976024dfe72a7561415284c41a  ./R67_R2_001.fastq.gz
f16d7330659ba6e310ade50cdae10b8c  ./R75_R1_001.fastq.gz
9b9ab52837950fdd8108d22e29f9bc31  ./R75_R2_001.fastq.gz
429bf4b34de618d97ec2213e658477c2  ./R83_R1_001.fastq.gz
77cf8a4c7d2db0bf83280df1254b335a  ./R83_R2_001.fastq.gz
86064e95efa1d802371fbda68d2be0ca  ./R91_R1_001.fastq.gz
3e24c0fdad691020cd1140e3a90bea3f  ./R91_R2_001.fastq.gz
c117e340bd9bef1bc295b6e86f6bfcbb  ./R99_R1_001.fastq.gz
0ebb0438dde06d989cfd79a5616ef9a3  ./R99_R2_001.fastq.gz
```

Then, here is the md5 checksum of the downloaded data on Andromeda:  

```
less checkmd5_20240412.md5

6ee514bdc1bacfb591629a3edf82bcd4  R107_R1_001.fastq.gz
3d6b00afc053f2eb770f83c5830f699e  R107_R2_001.fastq.gz
16aba2b715ceeb2c2f3a79dbc855e768  R55_R1_001.fastq.gz
14e32d9fccd55fd81a3468a4fd5766d1  R55_R2_001.fastq.gz
7fbb16f6f920e4a58431cd91e14b912b  R56_R1_001.fastq.gz
80bd51fd98400da3fb91b1182252f577  R56_R2_001.fastq.gz
aa6b3bd9617c10ca9b504309cc1ff047  R57_R1_001.fastq.gz
c52e1d5d9c7caa2189eaedf81944dfc7  R57_R2_001.fastq.gz
027601dabbfc6e48829efe5b931e6e05  R58_R1_001.fastq.gz
b1e2605ec7c35ad3c25ad7ab03cb4d2f  R58_R2_001.fastq.gz
a62cfd57273efefcfc4076e66cb5da1c  R59_R1_001.fastq.gz
dfa3021694fd5ddc01c387eafe34a0d9  R59_R2_001.fastq.gz
ba4c0c4c8dede34d546ee7ec5293553b  R60_R1_001.fastq.gz
dbf68528a9ee3428d378275c83458b24  R60_R2_001.fastq.gz
9ae8dc138d0df94197388b9da117f658  R62_R1_001.fastq.gz
96e1b11cb6e60415dc6f69fb4849651e  R62_R2_001.fastq.gz
0d2b1896bf85c0800a3a3482f34c90ca  R67_R1_001.fastq.gz
cbb14c976024dfe72a7561415284c41a  R67_R2_001.fastq.gz
f16d7330659ba6e310ade50cdae10b8c  R75_R1_001.fastq.gz
9b9ab52837950fdd8108d22e29f9bc31  R75_R2_001.fastq.gz
429bf4b34de618d97ec2213e658477c2  R83_R1_001.fastq.gz
77cf8a4c7d2db0bf83280df1254b335a  R83_R2_001.fastq.gz
86064e95efa1d802371fbda68d2be0ca  R91_R1_001.fastq.gz
3e24c0fdad691020cd1140e3a90bea3f  R91_R2_001.fastq.gz
c117e340bd9bef1bc295b6e86f6bfcbb  R99_R1_001.fastq.gz
0ebb0438dde06d989cfd79a5616ef9a3  R99_R2_001.fastq.gz

```

I then added these lists to a spreadsheet and checked that the cells matched for original and downloaded files. The file [is on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/checksums_azenta_andromeda_second_sequencing.xlsx).   

I also added in the checksums for data that I downloaded to Mox (detailed below). All files are confirmed and everything looks good!     

| file                 | original_azenta                  | downloaded_andromeda             | downloaded_mox                   | match_andromeda | match_mox |
|----------------------|----------------------------------|----------------------------------|----------------------------------|-----------------|-----------|
| R107_R1_001.fastq.gz | 6ee514bdc1bacfb591629a3edf82bcd4 | 6ee514bdc1bacfb591629a3edf82bcd4 | 6ee514bdc1bacfb591629a3edf82bcd4 | TRUE            | TRUE      |
| R107_R2_001.fastq.gz | 3d6b00afc053f2eb770f83c5830f699e | 3d6b00afc053f2eb770f83c5830f699e | 3d6b00afc053f2eb770f83c5830f699e | TRUE            | TRUE      |
| R55_R1_001.fastq.gz  | 16aba2b715ceeb2c2f3a79dbc855e768 | 16aba2b715ceeb2c2f3a79dbc855e768 | 16aba2b715ceeb2c2f3a79dbc855e768 | TRUE            | TRUE      |
| R55_R2_001.fastq.gz  | 14e32d9fccd55fd81a3468a4fd5766d1 | 14e32d9fccd55fd81a3468a4fd5766d1 | 14e32d9fccd55fd81a3468a4fd5766d1 | TRUE            | TRUE      |
| R56_R1_001.fastq.gz  | 7fbb16f6f920e4a58431cd91e14b912b | 7fbb16f6f920e4a58431cd91e14b912b | 7fbb16f6f920e4a58431cd91e14b912b | TRUE            | TRUE      |
| R56_R2_001.fastq.gz  | 80bd51fd98400da3fb91b1182252f577 | 80bd51fd98400da3fb91b1182252f577 | 80bd51fd98400da3fb91b1182252f577 | TRUE            | TRUE      |
| R57_R1_001.fastq.gz  | aa6b3bd9617c10ca9b504309cc1ff047 | aa6b3bd9617c10ca9b504309cc1ff047 | aa6b3bd9617c10ca9b504309cc1ff047 | TRUE            | TRUE      |
| R57_R2_001.fastq.gz  | c52e1d5d9c7caa2189eaedf81944dfc7 | c52e1d5d9c7caa2189eaedf81944dfc7 | c52e1d5d9c7caa2189eaedf81944dfc7 | TRUE            | TRUE      |
| R58_R1_001.fastq.gz  | 027601dabbfc6e48829efe5b931e6e05 | 027601dabbfc6e48829efe5b931e6e05 | 027601dabbfc6e48829efe5b931e6e05 | TRUE            | TRUE      |
| R58_R2_001.fastq.gz  | b1e2605ec7c35ad3c25ad7ab03cb4d2f | b1e2605ec7c35ad3c25ad7ab03cb4d2f | b1e2605ec7c35ad3c25ad7ab03cb4d2f | TRUE            | TRUE      |
| R59_R1_001.fastq.gz  | a62cfd57273efefcfc4076e66cb5da1c | a62cfd57273efefcfc4076e66cb5da1c | a62cfd57273efefcfc4076e66cb5da1c | TRUE            | TRUE      |
| R59_R2_001.fastq.gz  | dfa3021694fd5ddc01c387eafe34a0d9 | dfa3021694fd5ddc01c387eafe34a0d9 | dfa3021694fd5ddc01c387eafe34a0d9 | TRUE            | TRUE      |
| R60_R1_001.fastq.gz  | ba4c0c4c8dede34d546ee7ec5293553b | ba4c0c4c8dede34d546ee7ec5293553b | ba4c0c4c8dede34d546ee7ec5293553b | TRUE            | TRUE      |
| R60_R2_001.fastq.gz  | dbf68528a9ee3428d378275c83458b24 | dbf68528a9ee3428d378275c83458b24 | dbf68528a9ee3428d378275c83458b24 | TRUE            | TRUE      |
| R62_R1_001.fastq.gz  | 9ae8dc138d0df94197388b9da117f658 | 9ae8dc138d0df94197388b9da117f658 | 9ae8dc138d0df94197388b9da117f658 | TRUE            | TRUE      |
| R62_R2_001.fastq.gz  | 96e1b11cb6e60415dc6f69fb4849651e | 96e1b11cb6e60415dc6f69fb4849651e | 96e1b11cb6e60415dc6f69fb4849651e | TRUE            | TRUE      |
| R67_R1_001.fastq.gz  | 0d2b1896bf85c0800a3a3482f34c90ca | 0d2b1896bf85c0800a3a3482f34c90ca | 0d2b1896bf85c0800a3a3482f34c90ca | TRUE            | TRUE      |
| R67_R2_001.fastq.gz  | cbb14c976024dfe72a7561415284c41a | cbb14c976024dfe72a7561415284c41a | cbb14c976024dfe72a7561415284c41a | TRUE            | TRUE      |
| R75_R1_001.fastq.gz  | f16d7330659ba6e310ade50cdae10b8c | f16d7330659ba6e310ade50cdae10b8c | f16d7330659ba6e310ade50cdae10b8c | TRUE            | TRUE      |
| R75_R2_001.fastq.gz  | 9b9ab52837950fdd8108d22e29f9bc31 | 9b9ab52837950fdd8108d22e29f9bc31 | 9b9ab52837950fdd8108d22e29f9bc31 | TRUE            | TRUE      |
| R83_R1_001.fastq.gz  | 429bf4b34de618d97ec2213e658477c2 | 429bf4b34de618d97ec2213e658477c2 | 429bf4b34de618d97ec2213e658477c2 | TRUE            | TRUE      |
| R83_R2_001.fastq.gz  | 77cf8a4c7d2db0bf83280df1254b335a | 77cf8a4c7d2db0bf83280df1254b335a | 77cf8a4c7d2db0bf83280df1254b335a | TRUE            | TRUE      |
| R91_R1_001.fastq.gz  | 86064e95efa1d802371fbda68d2be0ca | 86064e95efa1d802371fbda68d2be0ca | 86064e95efa1d802371fbda68d2be0ca | TRUE            | TRUE      |
| R91_R2_001.fastq.gz  | 3e24c0fdad691020cd1140e3a90bea3f | 3e24c0fdad691020cd1140e3a90bea3f | 3e24c0fdad691020cd1140e3a90bea3f | TRUE            | TRUE      |
| R99_R1_001.fastq.gz  | c117e340bd9bef1bc295b6e86f6bfcbb | c117e340bd9bef1bc295b6e86f6bfcbb | c117e340bd9bef1bc295b6e86f6bfcbb | TRUE            | TRUE      |
| R99_R2_001.fastq.gz  | 0ebb0438dde06d989cfd79a5616ef9a3 | 0ebb0438dde06d989cfd79a5616ef9a3 | 0ebb0438dde06d989cfd79a5616ef9a3 | TRUE            | TRUE      |


Data are now downloaded and integrity confirmed on URI Andromeda.  

## Download data to UW Hyak/Mox

```
#logged into UW Hyak/Mox
cd /gscratch/srlab/ashuff/mcap-2023-rnaseq
mkdir second_sequencing
cd second_sequencing

``` 

The full directory where I want raw sequences to go is `/gscratch/srlab/ashuff/mcap-2023-rnaseq/second_sequencing `.  

```
# in Hyak ashuffm folder 

# log into Azenta sftp as directed by Azenta 
# cd into my project folder

#set directory for download
lcd /gscratch/srlab/ashuff/mcap-2023-rnaseq/second_sequencing

#download all files into project folder 

mget *
```

Downloaded on 13 April 2024. 

I then checked for data integrity using md5 checksums. Azenta provided a .md5 file for each sequence file. See the table above for confirmation of md5 checksums.  

```
srun -p srlab -A srlab --time=1:00:00 --mem=100G --pty /bin/bash

md5sum *.fastq.gz > checkmd5_20240413.md5

md5sum -c checkmd5_20240413.md5  
```

Check sums from data downloaded on Mox is here:  

```
less checkmd5_20240413.md5

6ee514bdc1bacfb591629a3edf82bcd4  R107_R1_001.fastq.gz
3d6b00afc053f2eb770f83c5830f699e  R107_R2_001.fastq.gz
16aba2b715ceeb2c2f3a79dbc855e768  R55_R1_001.fastq.gz
14e32d9fccd55fd81a3468a4fd5766d1  R55_R2_001.fastq.gz
7fbb16f6f920e4a58431cd91e14b912b  R56_R1_001.fastq.gz
80bd51fd98400da3fb91b1182252f577  R56_R2_001.fastq.gz
aa6b3bd9617c10ca9b504309cc1ff047  R57_R1_001.fastq.gz
c52e1d5d9c7caa2189eaedf81944dfc7  R57_R2_001.fastq.gz
027601dabbfc6e48829efe5b931e6e05  R58_R1_001.fastq.gz
b1e2605ec7c35ad3c25ad7ab03cb4d2f  R58_R2_001.fastq.gz
a62cfd57273efefcfc4076e66cb5da1c  R59_R1_001.fastq.gz
dfa3021694fd5ddc01c387eafe34a0d9  R59_R2_001.fastq.gz
ba4c0c4c8dede34d546ee7ec5293553b  R60_R1_001.fastq.gz
dbf68528a9ee3428d378275c83458b24  R60_R2_001.fastq.gz
9ae8dc138d0df94197388b9da117f658  R62_R1_001.fastq.gz
96e1b11cb6e60415dc6f69fb4849651e  R62_R2_001.fastq.gz
0d2b1896bf85c0800a3a3482f34c90ca  R67_R1_001.fastq.gz
cbb14c976024dfe72a7561415284c41a  R67_R2_001.fastq.gz
f16d7330659ba6e310ade50cdae10b8c  R75_R1_001.fastq.gz
9b9ab52837950fdd8108d22e29f9bc31  R75_R2_001.fastq.gz
429bf4b34de618d97ec2213e658477c2  R83_R1_001.fastq.gz
77cf8a4c7d2db0bf83280df1254b335a  R83_R2_001.fastq.gz
86064e95efa1d802371fbda68d2be0ca  R91_R1_001.fastq.gz
3e24c0fdad691020cd1140e3a90bea3f  R91_R2_001.fastq.gz
c117e340bd9bef1bc295b6e86f6bfcbb  R99_R1_001.fastq.gz
0ebb0438dde06d989cfd79a5616ef9a3  R99_R2_001.fastq.gz

```

Output from checksums is here: 

```
R107_R1_001.fastq.gz: OK
R107_R2_001.fastq.gz: OK
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
R62_R1_001.fastq.gz: OK
R62_R2_001.fastq.gz: OK
R67_R1_001.fastq.gz: OK
R67_R2_001.fastq.gz: OK
R75_R1_001.fastq.gz: OK
R75_R2_001.fastq.gz: OK
R83_R1_001.fastq.gz: OK
R83_R2_001.fastq.gz: OK
R91_R1_001.fastq.gz: OK
R91_R2_001.fastq.gz: OK
R99_R1_001.fastq.gz: OK
R99_R2_001.fastq.gz: OK

```

Everything looks good and all data files were transferred correctly.   

Files are now stored on both URI and UW servers. 

# 2. Rename files to indicate they are from the second sequencing 

Add an "s" after the sample ID to indicate that samples are from the second round of sequencing, so that they can be uploaded to NCBI and will remain distinct from first round of sequencing.  

I am performing this in Andromeda, becuase this is where I am going to conduct analyses and upload to NCBI.  

```
/data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/second_sequencing 

for file in *; do
    # Check if the file name contains ".gz" and an underscore
    if [[ $file == *".gz"* && $file == *_* ]]; then
        # Extract the part of the file name before the first underscore
        prefix="${file%%_*}"
        # Extract the part of the file name after the first underscore
        suffix="${file#*_}"
        # Rename the file by adding "s" after the prefix
        mv "$file" "${prefix}s_$suffix"
        echo "Renamed: $file to ${prefix}s_$suffix"
    fi
done

``` 

This changed the files to the following names:  

```
azenta_original_checksums_second.md5  R60s_R1_001.fastq.gz.md5
checkmd5_20240412.md5                 R60s_R2_001.fastq.gz
md5_files                             R60s_R2_001.fastq.gz.md5
R107s_R1_001.fastq.gz                 R62s_R1_001.fastq.gz
R107s_R1_001.fastq.gz.md5             R62s_R1_001.fastq.gz.md5
R107s_R2_001.fastq.gz                 R62s_R2_001.fastq.gz
R107s_R2_001.fastq.gz.md5             R62s_R2_001.fastq.gz.md5
R55s_R1_001.fastq.gz                  R67s_R1_001.fastq.gz
R55s_R1_001.fastq.gz.md5              R67s_R1_001.fastq.gz.md5
R55s_R2_001.fastq.gz                  R67s_R2_001.fastq.gz
R55s_R2_001.fastq.gz.md5              R67s_R2_001.fastq.gz.md5
R56s_R1_001.fastq.gz                  R75s_R1_001.fastq.gz
R56s_R1_001.fastq.gz.md5              R75s_R1_001.fastq.gz.md5
R56s_R2_001.fastq.gz                  R75s_R2_001.fastq.gz
R56s_R2_001.fastq.gz.md5              R75s_R2_001.fastq.gz.md5
R57s_R1_001.fastq.gz                  R83s_R1_001.fastq.gz
R57s_R1_001.fastq.gz.md5              R83s_R1_001.fastq.gz.md5
R57s_R2_001.fastq.gz                  R83s_R2_001.fastq.gz
R57s_R2_001.fastq.gz.md5              R83s_R2_001.fastq.gz.md5
R58s_R1_001.fastq.gz                  R91s_R1_001.fastq.gz
R58s_R1_001.fastq.gz.md5              R91s_R1_001.fastq.gz.md5
R58s_R2_001.fastq.gz                  R91s_R2_001.fastq.gz
R58s_R2_001.fastq.gz.md5              R91s_R2_001.fastq.gz.md5
R59s_R1_001.fastq.gz                  R99s_R1_001.fastq.gz
R59s_R1_001.fastq.gz.md5              R99s_R1_001.fastq.gz.md5
R59s_R2_001.fastq.gz                  R99s_R2_001.fastq.gz
R59s_R2_001.fastq.gz.md5              R99s_R2_001.fastq.gz.md5
R60s_R1_001.fastq.gz                  raw_fastqc
```

The files that were sequenced in the second round now contain "s" after the sample name. 

# 3. Upload to NCBI project 

Project ID: PRJNA1078313   

I used the same information on uploading for this project [detailed in my previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Mcapitata-Larval-Thermal-Tolerance-Project-NCBI-upload/) and the [Putnam Lab SRA protocol](https://github.com/Putnam-Lab/Lab_Management/blob/master/Bioinformatics_%26_Coding/Data_Mangament/SRA-Upload_Protocol.md)..  

The sample metadata can be found on GitHub here using the MIMS.me.host-associated template [here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/NCBI_upload/McapLarval_Tolerance_MIMS.me.host-associated.5.0.xlsx) and sequencing information and metadata [is located on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/sample_rnaseq_metadata.csv).  

I uploaded the biosample metadata [to GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/NCBI_upload/Hawaii2023_SRA_metadata.txt). In this file I updated the design notes to include: "Standard mRNA-seq from libraries prepared using Zymo Quick MiniPrep Plus DNA/RNA extraction kit at the Putnam Lab (University of Rhode Island) with sequencing at Azenta Life Sciences. Additional top off sequencing performed by Azenta Life Sciences with additional sequencing files denoted with "s" following sample name prefix." I added the new sequence file names into filename 3 and 4 columns. 




AH WORKING HERE 




## Transfer data from URI server to NCBI SRA 
 
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