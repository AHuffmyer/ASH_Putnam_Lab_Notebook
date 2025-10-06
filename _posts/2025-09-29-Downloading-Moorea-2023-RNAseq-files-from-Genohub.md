---
layout: post
title: Download and QC of Moorea 2023 RNAseq files with NCBI upload
date: '2025-09-30'
categories: Moorea_SymbioticExchange_2023
tags: Bioinformatics GeneExpression Molecular Data_Management
---

Today I downloaded RNAseq files from GenoHub for the 2023 Moorea Symbiotic Exchange project. 

# Overview 

I submitted samples for RNA sequencing to examine differences in thermal responses between lifestages of *Pocillopora* and *Acropora* corals from the 2023 Moorea Symbiotic Exchange project.  

Here are relevant links to past posts and data for this project:  

- [Project GitHub](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023)
- [RNA extraction protocol](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Coral-RNA-Extraction-Protocol-for-2023-Moorea-Project/)
- [RNA extraction log](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/RNA-Extraction-Log-for-2023-Moorea-Project/) 

Here are links to relevant metadata and sample inventories: 

- [Sequencing sample inventory](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/genohub_Huffmyer_coral_RNA_samples.xlsx)
- [Biological/experimental sample metadata](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/rna_samples_extractions.xlsx)
- [Sequencer QC files](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/sequencer-files/GH6303829_RNA_QC_-_2025-08-18_-_10.36.49.docx)
- [Description of sequencing methods](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/sequencer-files/Methods_Stranded_mRNA_Watchmaker_Genomics_20241127.docx)
- [Sequencer Qubit data](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/sequencer-files/QubitData_08-18-2025_11-29-36.csv)

# Data download 

## UW Hyak 

- Logged onto hyak and made a new directory here for raw sequences: `/gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences`
- Started an interactive job `salloc --partition=ckpt-all --cpus-per-task=1 --mem=10G --time=2:00:00` 
- `module load coenv/aws/2.17.32`
- `aws configure` and added log in data from Genohub 
- Used Genohub commands to access S3 storage bucket 
- Downloaded all files from Genohub bucket to `/gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences`
- The sequences are identified by sample name. Some samples had a second round of sequencing, identified by a different "S###" identifier. 

Here is the complete list of files on Genohub's server.  

```
2025-09-26 15:26:50  202011072 10_S131_R1_001.fastq.gz
2025-09-26 15:26:59         57 10_S131_R1_001.fastq.gz.md5
2025-09-26 15:26:50  201982551 10_S131_R2_001.fastq.gz
2025-09-26 15:26:53         57 10_S131_R2_001.fastq.gz.md5
2025-09-23 09:28:24  653257028 10_S921_R1_001.fastq.gz
2025-09-23 09:28:35         57 10_S921_R1_001.fastq.gz.md5
2025-09-23 09:28:26  650495077 10_S921_R2_001.fastq.gz
2025-09-23 09:28:36         57 10_S921_R2_001.fastq.gz.md5
2025-09-26 15:28:52 1882339016 11_S132_R1_001.fastq.gz
2025-09-26 15:29:00         57 11_S132_R1_001.fastq.gz.md5
2025-09-26 15:28:52 1886049965 11_S132_R2_001.fastq.gz
2025-09-26 15:29:04         57 11_S132_R2_001.fastq.gz.md5
2025-09-23 09:31:35     750780 11_S922_R1_001.fastq.gz
2025-09-23 09:31:39         57 11_S922_R1_001.fastq.gz.md5
2025-09-23 09:31:38     745609 11_S922_R2_001.fastq.gz
2025-09-23 09:31:42         57 11_S922_R2_001.fastq.gz.md5
2025-09-26 15:28:30  266677507 12_S133_R1_001.fastq.gz
2025-09-26 15:28:40         57 12_S133_R1_001.fastq.gz.md5
2025-09-26 15:28:31  267506631 12_S133_R2_001.fastq.gz
2025-09-26 15:28:34         57 12_S133_R2_001.fastq.gz.md5
2025-09-23 09:30:59  878736005 12_S923_R1_001.fastq.gz
2025-09-23 09:31:09         57 12_S923_R1_001.fastq.gz.md5
2025-09-23 09:31:00  874927620 12_S923_R2_001.fastq.gz
2025-09-23 09:31:13         57 12_S923_R2_001.fastq.gz.md5
2025-09-26 15:28:41  266283704 13_S134_R1_001.fastq.gz
2025-09-26 15:28:46         57 13_S134_R1_001.fastq.gz.md5
2025-09-26 15:28:43  265393778 13_S134_R2_001.fastq.gz
2025-09-26 15:28:55         57 13_S134_R2_001.fastq.gz.md5
2025-09-23 09:31:23  863456553 13_S924_R1_001.fastq.gz
2025-09-23 09:31:33         57 13_S924_R1_001.fastq.gz.md5
2025-09-23 09:31:24  857992096 13_S924_R2_001.fastq.gz
2025-09-23 09:31:36         57 13_S924_R2_001.fastq.gz.md5
2025-09-26 15:28:52  356402661 14_S135_R1_001.fastq.gz
2025-09-26 15:29:04         57 14_S135_R1_001.fastq.gz.md5
2025-09-26 15:28:55  353909175 14_S135_R2_001.fastq.gz
2025-09-26 15:29:06         57 14_S135_R2_001.fastq.gz.md5
2025-09-23 09:31:39 1123658157 14_S925_R1_001.fastq.gz
2025-09-23 09:31:48         57 14_S925_R1_001.fastq.gz.md5
2025-09-23 09:31:41 1114020857 14_S925_R2_001.fastq.gz
2025-09-23 09:31:54         57 14_S925_R2_001.fastq.gz.md5
2025-09-26 15:28:49  287539135 15_S136_R1_001.fastq.gz
2025-09-26 15:28:52         57 15_S136_R1_001.fastq.gz.md5
2025-09-26 15:28:49  285499626 15_S136_R2_001.fastq.gz
2025-09-26 15:29:01         57 15_S136_R2_001.fastq.gz.md5
2025-09-23 09:31:27  885160874 15_S926_R1_001.fastq.gz
2025-09-23 09:31:38         57 15_S926_R1_001.fastq.gz.md5
2025-09-23 09:31:32  876632244 15_S926_R2_001.fastq.gz
2025-09-23 09:31:42         57 15_S926_R2_001.fastq.gz.md5
2025-09-26 15:27:49  279202783 16_S137_R1_001.fastq.gz
2025-09-26 15:27:58         57 16_S137_R1_001.fastq.gz.md5
2025-09-26 15:27:49  279103137 16_S137_R2_001.fastq.gz
2025-09-26 15:27:52         57 16_S137_R2_001.fastq.gz.md5
2025-09-23 09:29:53  884175492 16_S927_R1_001.fastq.gz
2025-09-23 09:30:05         57 16_S927_R1_001.fastq.gz.md5
2025-09-23 09:29:56  879388652 16_S927_R2_001.fastq.gz
2025-09-23 09:30:09         57 16_S927_R2_001.fastq.gz.md5
2025-09-26 15:28:46  315521999 17_S138_R1_001.fastq.gz
2025-09-26 15:28:49         57 17_S138_R1_001.fastq.gz.md5
2025-09-26 15:28:46  314173216 17_S138_R2_001.fastq.gz
2025-09-26 15:28:58         57 17_S138_R2_001.fastq.gz.md5
2025-09-23 09:31:24  988629472 17_S928_R1_001.fastq.gz
2025-09-23 09:31:35         57 17_S928_R1_001.fastq.gz.md5
2025-09-23 09:31:28  982460035 17_S928_R2_001.fastq.gz
2025-09-23 09:31:35         57 17_S928_R2_001.fastq.gz.md5
2025-09-26 15:26:18  233732621 18_S139_R1_001.fastq.gz
2025-09-26 15:26:20         57 18_S139_R1_001.fastq.gz.md5
2025-09-26 15:26:23  233198925 18_S139_R2_001.fastq.gz
2025-09-26 15:26:32         57 18_S139_R2_001.fastq.gz.md5
2025-09-23 09:27:31  713013275 18_S929_R1_001.fastq.gz
2025-09-23 09:27:42         57 18_S929_R1_001.fastq.gz.md5
2025-09-23 09:27:43  707972377 18_S929_R2_001.fastq.gz
2025-09-23 09:27:53         57 18_S929_R2_001.fastq.gz.md5
2025-09-26 15:27:30  291295888 19_S140_R1_001.fastq.gz
2025-09-26 15:27:39         57 19_S140_R1_001.fastq.gz.md5
2025-09-26 15:27:31  288288058 19_S140_R2_001.fastq.gz
2025-09-26 15:27:40         57 19_S140_R2_001.fastq.gz.md5
2025-09-23 09:29:36  813380202 19_S930_R1_001.fastq.gz
2025-09-23 09:29:45         57 19_S930_R1_001.fastq.gz.md5
2025-09-23 09:29:37  802567667 19_S930_R2_001.fastq.gz
2025-09-23 09:29:48         57 19_S930_R2_001.fastq.gz.md5
2025-09-26 15:28:22  268090339 1_S122_R1_001.fastq.gz
2025-09-26 15:28:34         56 1_S122_R1_001.fastq.gz.md5
2025-09-26 15:28:25  267398177 1_S122_R2_001.fastq.gz
2025-09-26 15:28:28         56 1_S122_R2_001.fastq.gz.md5
2025-09-23 09:30:44  802810789 1_S912_R1_001.fastq.gz
2025-09-23 09:30:57         56 1_S912_R1_001.fastq.gz.md5
2025-09-23 09:30:47  798365190 1_S912_R2_001.fastq.gz
2025-09-23 09:31:00         56 1_S912_R2_001.fastq.gz.md5
2025-09-26 15:27:55  349562101 20_S141_R1_001.fastq.gz
2025-09-26 15:28:04         57 20_S141_R1_001.fastq.gz.md5
2025-09-26 15:27:58  345927239 20_S141_R2_001.fastq.gz
2025-09-26 15:28:01         57 20_S141_R2_001.fastq.gz.md5
2025-09-23 09:29:59 1040531009 20_S931_R1_001.fastq.gz
2025-09-23 09:30:09         57 20_S931_R1_001.fastq.gz.md5
2025-09-23 09:29:59 1025659742 20_S931_R2_001.fastq.gz
2025-09-23 09:30:09         57 20_S931_R2_001.fastq.gz.md5
2025-09-26 15:27:40  256644872 21_S142_R1_001.fastq.gz
2025-09-26 15:27:44         57 21_S142_R1_001.fastq.gz.md5
2025-09-26 15:27:39  255983049 21_S142_R2_001.fastq.gz
2025-09-26 15:27:51         57 21_S142_R2_001.fastq.gz.md5
2025-09-23 09:29:50  799436215 21_S932_R1_001.fastq.gz
2025-09-23 09:29:59         57 21_S932_R1_001.fastq.gz.md5
2025-09-23 09:29:51  794774614 21_S932_R2_001.fastq.gz
2025-09-23 09:29:59         57 21_S932_R2_001.fastq.gz.md5
2025-09-26 15:27:21  240606837 22_S143_R1_001.fastq.gz
2025-09-26 15:27:23         57 22_S143_R1_001.fastq.gz.md5
2025-09-26 15:27:21  239686313 22_S143_R2_001.fastq.gz
2025-09-26 15:27:29         57 22_S143_R2_001.fastq.gz.md5
2025-09-23 09:29:18  750930588 22_S933_R1_001.fastq.gz
2025-09-23 09:29:31         57 22_S933_R1_001.fastq.gz.md5
2025-09-23 09:29:22  745635127 22_S933_R2_001.fastq.gz
2025-09-23 09:29:33         57 22_S933_R2_001.fastq.gz.md5
2025-09-26 15:28:34  224308588 23_S144_R1_001.fastq.gz
2025-09-26 15:28:36         57 23_S144_R1_001.fastq.gz.md5
2025-09-26 15:28:37  223237425 23_S144_R2_001.fastq.gz
2025-09-26 15:28:41         57 23_S144_R2_001.fastq.gz.md5
2025-09-23 09:31:05  797039603 23_S934_R1_001.fastq.gz
2025-09-23 09:31:18         57 23_S934_R1_001.fastq.gz.md5
2025-09-23 09:31:08  788952748 23_S934_R2_001.fastq.gz
2025-09-23 09:31:26         57 23_S934_R2_001.fastq.gz.md5
2025-09-26 15:28:58  306311416 24_S145_R1_001.fastq.gz
2025-09-26 15:29:07         57 24_S145_R1_001.fastq.gz.md5
2025-09-26 15:28:58  303526222 24_S145_R2_001.fastq.gz
2025-09-26 15:29:01         57 24_S145_R2_001.fastq.gz.md5
2025-09-23 09:31:41  936752300 24_S935_R1_001.fastq.gz
2025-09-23 09:31:53         57 24_S935_R1_001.fastq.gz.md5
2025-09-23 09:31:44  924052120 24_S935_R2_001.fastq.gz
2025-09-23 09:31:54         57 24_S935_R2_001.fastq.gz.md5
2025-09-26 15:27:34  259143906 25_S146_R1_001.fastq.gz
2025-09-26 15:27:43         57 25_S146_R1_001.fastq.gz.md5
2025-09-26 15:27:37  258337178 25_S146_R2_001.fastq.gz
2025-09-26 15:27:39         57 25_S146_R2_001.fastq.gz.md5
2025-09-23 09:29:38  862182039 25_S936_R1_001.fastq.gz
2025-09-23 09:29:47         57 25_S936_R1_001.fastq.gz.md5
2025-09-23 09:29:41  854528149 25_S936_R2_001.fastq.gz
2025-09-23 09:29:50         57 25_S936_R2_001.fastq.gz.md5
2025-09-26 15:28:10  299761772 26_S147_R1_001.fastq.gz
2025-09-26 15:28:13         57 26_S147_R1_001.fastq.gz.md5
2025-09-26 15:28:13  297416514 26_S147_R2_001.fastq.gz
2025-09-26 15:28:21         57 26_S147_R2_001.fastq.gz.md5
2025-09-23 09:30:24  918422035 26_S937_R1_001.fastq.gz
2025-09-23 09:30:36         57 26_S937_R1_001.fastq.gz.md5
2025-09-23 09:30:23  909286653 26_S937_R2_001.fastq.gz
2025-09-23 09:30:36         57 26_S937_R2_001.fastq.gz.md5
2025-09-26 15:27:19  317092560 27_S148_R1_001.fastq.gz
2025-09-26 15:27:28         57 27_S148_R1_001.fastq.gz.md5
2025-09-26 15:27:19  314588951 27_S148_R2_001.fastq.gz
2025-09-26 15:27:31         57 27_S148_R2_001.fastq.gz.md5
2025-09-23 09:29:18  957305963 27_S938_R1_001.fastq.gz
2025-09-23 09:29:27         57 27_S938_R1_001.fastq.gz.md5
2025-09-23 09:29:17  946780102 27_S938_R2_001.fastq.gz
2025-09-23 09:29:31         57 27_S938_R2_001.fastq.gz.md5
2025-09-26 15:26:50  286768653 28_S149_R1_001.fastq.gz
2025-09-26 15:26:59         57 28_S149_R1_001.fastq.gz.md5
2025-09-26 15:26:50  286349169 28_S149_R2_001.fastq.gz
2025-09-26 15:26:58         57 28_S149_R2_001.fastq.gz.md5
2025-09-23 09:28:16  923973131 28_S939_R1_001.fastq.gz
2025-09-23 09:28:23         57 28_S939_R1_001.fastq.gz.md5
2025-09-23 09:28:16  920574071 28_S939_R2_001.fastq.gz
2025-09-23 09:28:24         57 28_S939_R2_001.fastq.gz.md5
2025-09-26 15:26:09  334129360 29_S150_R1_001.fastq.gz
2025-09-26 15:26:09         57 29_S150_R1_001.fastq.gz.md5
2025-09-26 15:26:09  333229683 29_S150_R2_001.fastq.gz
2025-09-26 15:26:09         57 29_S150_R2_001.fastq.gz.md5
2025-09-23 09:27:12 1078548940 29_S940_R1_001.fastq.gz
2025-09-23 09:27:21         57 29_S940_R1_001.fastq.gz.md5
2025-09-23 09:27:12 1072744348 29_S940_R2_001.fastq.gz
2025-09-23 09:27:22         57 29_S940_R2_001.fastq.gz.md5
2025-09-26 15:26:50  312110698 2_S123_R1_001.fastq.gz
2025-09-26 15:26:59         56 2_S123_R1_001.fastq.gz.md5
2025-09-26 15:26:50  309261122 2_S123_R2_001.fastq.gz
2025-09-26 15:26:59         56 2_S123_R2_001.fastq.gz.md5
2025-09-23 09:28:06  960723270 2_S913_R1_001.fastq.gz
2025-09-23 09:28:23         56 2_S913_R1_001.fastq.gz.md5
2025-09-23 09:28:12  949954952 2_S913_R2_001.fastq.gz
2025-09-23 09:28:21         56 2_S913_R2_001.fastq.gz.md5
2025-09-26 15:26:32  631382195 30_S151_R1_001.fastq.gz
2025-09-26 15:26:44         57 30_S151_R1_001.fastq.gz.md5
2025-09-26 15:26:35  631177701 30_S151_R2_001.fastq.gz
2025-09-26 15:26:44         57 30_S151_R2_001.fastq.gz.md5
2025-09-23 09:27:53 1976118285 30_S941_R1_001.fastq.gz
2025-09-23 09:28:02         57 30_S941_R1_001.fastq.gz.md5
2025-09-23 09:27:53 1970809265 30_S941_R2_001.fastq.gz
2025-09-23 09:28:02         57 30_S941_R2_001.fastq.gz.md5
2025-09-26 15:27:09  329272825 31_S152_R1_001.fastq.gz
2025-09-26 15:27:19         57 31_S152_R1_001.fastq.gz.md5
2025-09-26 15:27:09  329234104 31_S152_R2_001.fastq.gz
2025-09-26 15:27:19         57 31_S152_R2_001.fastq.gz.md5
2025-09-23 09:28:48 1055454535 31_S942_R1_001.fastq.gz
2025-09-23 09:29:06         57 31_S942_R1_001.fastq.gz.md5
2025-09-23 09:28:54 1051723014 31_S942_R2_001.fastq.gz
2025-09-23 09:29:05         57 31_S942_R2_001.fastq.gz.md5
2025-09-26 15:28:19  345274632 32_S153_R1_001.fastq.gz
2025-09-26 15:28:28         57 32_S153_R1_001.fastq.gz.md5
2025-09-26 15:28:19  341163227 32_S153_R2_001.fastq.gz
2025-09-26 15:28:31         57 32_S153_R2_001.fastq.gz.md5
2025-09-23 09:30:33 1043423919 32_S943_R1_001.fastq.gz
2025-09-23 09:30:42         57 32_S943_R1_001.fastq.gz.md5
2025-09-23 09:30:35 1026463015 32_S943_R2_001.fastq.gz
2025-09-23 09:30:48         57 32_S943_R2_001.fastq.gz.md5
2025-09-26 15:26:44  290139935 33_S154_R1_001.fastq.gz
2025-09-26 15:26:47         57 33_S154_R1_001.fastq.gz.md5
2025-09-26 15:26:47  288989963 33_S154_R2_001.fastq.gz
2025-09-26 15:26:58         57 33_S154_R2_001.fastq.gz.md5
2025-09-23 09:28:02  914358842 33_S944_R1_001.fastq.gz
2025-09-23 09:28:16         57 33_S944_R1_001.fastq.gz.md5
2025-09-23 09:28:07  909381908 33_S944_R2_001.fastq.gz
2025-09-23 09:28:16         57 33_S944_R2_001.fastq.gz.md5
2025-09-26 15:28:28  276051752 34_S155_R1_001.fastq.gz
2025-09-26 15:28:31         57 34_S155_R1_001.fastq.gz.md5
2025-09-26 15:28:28  274356940 34_S155_R2_001.fastq.gz
2025-09-26 15:28:41         57 34_S155_R2_001.fastq.gz.md5
2025-09-23 09:30:48  822290222 34_S945_R1_001.fastq.gz
2025-09-23 09:30:59         57 34_S945_R1_001.fastq.gz.md5
2025-09-23 09:30:57  815663538 34_S945_R2_001.fastq.gz
2025-09-23 09:31:08         57 34_S945_R2_001.fastq.gz.md5
2025-09-26 15:27:40  286374528 35_S156_R1_001.fastq.gz
2025-09-26 15:27:49         57 35_S156_R1_001.fastq.gz.md5
2025-09-26 15:27:40  284795688 35_S156_R2_001.fastq.gz
2025-09-26 15:27:49         57 35_S156_R2_001.fastq.gz.md5
2025-09-23 09:29:44  879499541 35_S946_R1_001.fastq.gz
2025-09-23 09:29:56         57 35_S946_R1_001.fastq.gz.md5
2025-09-23 09:29:46  873601324 35_S946_R2_001.fastq.gz
2025-09-23 09:29:57         57 35_S946_R2_001.fastq.gz.md5
2025-09-26 15:26:38  372766750 36_S157_R1_001.fastq.gz
2025-09-26 15:26:41         57 36_S157_R1_001.fastq.gz.md5
2025-09-26 15:26:41  370882226 36_S157_R2_001.fastq.gz
2025-09-26 15:26:50         57 36_S157_R2_001.fastq.gz.md5
2025-09-23 09:27:53 1087001803 36_S947_R1_001.fastq.gz
2025-09-23 09:28:03         57 36_S947_R1_001.fastq.gz.md5
2025-09-23 09:27:53 1077963884 36_S947_R2_001.fastq.gz
2025-09-23 09:28:06         57 36_S947_R2_001.fastq.gz.md5
2025-09-26 15:28:04  285625695 37_S158_R1_001.fastq.gz
2025-09-26 15:28:07         57 37_S158_R1_001.fastq.gz.md5
2025-09-26 15:28:07  282292944 37_S158_R2_001.fastq.gz
2025-09-26 15:28:10         57 37_S158_R2_001.fastq.gz.md5
2025-09-23 09:30:11  831669283 37_S948_R1_001.fastq.gz
2025-09-23 09:30:24         57 37_S948_R1_001.fastq.gz.md5
2025-09-23 09:30:14  821392226 37_S948_R2_001.fastq.gz
2025-09-23 09:30:24         57 37_S948_R2_001.fastq.gz.md5
2025-09-26 15:27:40 1887207722 38_S159_R1_001.fastq.gz
2025-09-26 15:27:49         57 38_S159_R1_001.fastq.gz.md5
2025-09-26 15:27:39 1885030462 38_S159_R2_001.fastq.gz
2025-09-26 15:27:52         57 38_S159_R2_001.fastq.gz.md5
2025-09-23 09:29:47        746 38_S949_R1_001.fastq.gz
2025-09-23 09:29:50         57 38_S949_R1_001.fastq.gz.md5
2025-09-23 09:29:47        724 38_S949_R2_001.fastq.gz
2025-09-23 09:29:50         57 38_S949_R2_001.fastq.gz.md5
2025-09-26 15:27:29  203004182 39_S160_R1_001.fastq.gz
2025-09-26 15:27:30         57 39_S160_R1_001.fastq.gz.md5
2025-09-26 15:27:28  202741281 39_S160_R2_001.fastq.gz
2025-09-26 15:27:39         57 39_S160_R2_001.fastq.gz.md5
2025-09-23 09:29:30  732953151 39_S950_R1_001.fastq.gz
2025-09-23 09:29:44         57 39_S950_R1_001.fastq.gz.md5
2025-09-23 09:29:33  730396508 39_S950_R2_001.fastq.gz
2025-09-23 09:29:46         57 39_S950_R2_001.fastq.gz.md5
2025-09-26 15:26:09  300231191 3_S124_R1_001.fastq.gz
2025-09-26 15:26:09         56 3_S124_R1_001.fastq.gz.md5
2025-09-26 15:26:09  297343002 3_S124_R2_001.fastq.gz
2025-09-26 15:26:09         56 3_S124_R2_001.fastq.gz.md5
2025-09-23 09:27:12  871154353 3_S914_R1_001.fastq.gz
2025-09-23 09:27:21         56 3_S914_R1_001.fastq.gz.md5
2025-09-23 09:27:12  861099928 3_S914_R2_001.fastq.gz
2025-09-23 09:27:22         56 3_S914_R2_001.fastq.gz.md5
2025-09-26 15:28:01  323927035 40_S161_R1_001.fastq.gz
2025-09-26 15:28:10         57 40_S161_R1_001.fastq.gz.md5
2025-09-26 15:28:01  321904613 40_S161_R2_001.fastq.gz
2025-09-26 15:28:13         57 40_S161_R2_001.fastq.gz.md5
2025-09-23 09:30:05  959725567 40_S951_R1_001.fastq.gz
2025-09-23 09:30:18         57 40_S951_R1_001.fastq.gz.md5
2025-09-23 09:30:06  952033449 40_S951_R2_001.fastq.gz
2025-09-23 09:30:21         57 40_S951_R2_001.fastq.gz.md5
2025-09-26 15:26:18  253011608 41_S162_R1_001.fastq.gz
2025-09-26 15:26:20         57 41_S162_R1_001.fastq.gz.md5
2025-09-26 15:26:18  252117752 41_S162_R2_001.fastq.gz
2025-09-26 15:26:20         57 41_S162_R2_001.fastq.gz.md5
2025-09-23 09:27:31  779194290 41_S952_R1_001.fastq.gz
2025-09-23 09:27:42         57 41_S952_R1_001.fastq.gz.md5
2025-09-23 09:27:31  774392844 41_S952_R2_001.fastq.gz
2025-09-23 09:27:42         57 41_S952_R2_001.fastq.gz.md5
2025-09-26 15:29:01  362284115 42_S163_R1_001.fastq.gz
2025-09-26 15:29:13         57 42_S163_R1_001.fastq.gz.md5
2025-09-26 15:29:04  360602460 42_S163_R2_001.fastq.gz
2025-09-26 15:29:06         57 42_S163_R2_001.fastq.gz.md5
2025-09-23 09:31:48 1138848440 42_S953_R1_001.fastq.gz
2025-09-23 09:32:00         57 42_S953_R1_001.fastq.gz.md5
2025-09-23 09:31:50 1129654684 42_S953_R2_001.fastq.gz
2025-09-23 09:32:07         57 42_S953_R2_001.fastq.gz.md5
2025-09-26 15:27:02  312427427 43_S164_R1_001.fastq.gz
2025-09-26 15:27:14         57 43_S164_R1_001.fastq.gz.md5
2025-09-26 15:27:05  311959430 43_S164_R2_001.fastq.gz
2025-09-26 15:27:16         57 43_S164_R2_001.fastq.gz.md5
2025-09-23 09:28:39  922783741 43_S954_R1_001.fastq.gz
2025-09-23 09:28:53         57 43_S954_R1_001.fastq.gz.md5
2025-09-23 09:28:45  920414635 43_S954_R2_001.fastq.gz
2025-09-23 09:28:59         57 43_S954_R2_001.fastq.gz.md5
2025-09-26 15:27:29  288444293 44_S165_R1_001.fastq.gz
2025-09-26 15:27:39         57 44_S165_R1_001.fastq.gz.md5
2025-09-26 15:27:28  286181750 44_S165_R2_001.fastq.gz
2025-09-26 15:27:39         57 44_S165_R2_001.fastq.gz.md5
2025-09-23 09:29:24  876365211 44_S955_R1_001.fastq.gz
2025-09-23 09:29:36         57 44_S955_R1_001.fastq.gz.md5
2025-09-23 09:29:31  867795464 44_S955_R2_001.fastq.gz
2025-09-23 09:29:39         57 44_S955_R2_001.fastq.gz.md5
2025-09-26 15:28:21  292987120 45_S166_R1_001.fastq.gz
2025-09-26 15:28:24         57 45_S166_R1_001.fastq.gz.md5
2025-09-26 15:28:22  292087105 45_S166_R2_001.fastq.gz
2025-09-26 15:28:24         57 45_S166_R2_001.fastq.gz.md5
2025-09-23 09:30:41  899719347 45_S956_R1_001.fastq.gz
2025-09-23 09:30:54         57 45_S956_R1_001.fastq.gz.md5
2025-09-23 09:30:44  894983870 45_S956_R2_001.fastq.gz
2025-09-23 09:30:53         57 45_S956_R2_001.fastq.gz.md5
2025-09-26 15:26:09  270808221 46_S167_R1_001.fastq.gz
2025-09-26 15:26:09         57 46_S167_R1_001.fastq.gz.md5
2025-09-26 15:26:09  270144351 46_S167_R2_001.fastq.gz
2025-09-26 15:26:09         57 46_S167_R2_001.fastq.gz.md5
2025-09-23 09:27:12  854634298 46_S957_R1_001.fastq.gz
2025-09-23 09:27:25         57 46_S957_R1_001.fastq.gz.md5
2025-09-23 09:27:12  850548721 46_S957_R2_001.fastq.gz
2025-09-23 09:27:21         57 46_S957_R2_001.fastq.gz.md5
2025-09-26 15:27:57  329397321 47_S168_R1_001.fastq.gz
2025-09-26 15:28:01         57 47_S168_R1_001.fastq.gz.md5
2025-09-26 15:27:57  328855813 47_S168_R2_001.fastq.gz
2025-09-26 15:28:10         57 47_S168_R2_001.fastq.gz.md5
2025-09-23 09:30:03 1010118838 47_S958_R1_001.fastq.gz
2025-09-23 09:30:12         57 47_S958_R1_001.fastq.gz.md5
2025-09-23 09:30:03 1005423262 47_S958_R2_001.fastq.gz
2025-09-23 09:30:15         57 47_S958_R2_001.fastq.gz.md5
2025-09-26 15:28:21  369327078 48_S169_R1_001.fastq.gz
2025-09-26 15:28:30         57 48_S169_R1_001.fastq.gz.md5
2025-09-26 15:28:21  366096713 48_S169_R2_001.fastq.gz
2025-09-26 15:28:31         57 48_S169_R2_001.fastq.gz.md5
2025-09-23 09:30:42 1161330687 48_S959_R1_001.fastq.gz
2025-09-23 09:30:50         57 48_S959_R1_001.fastq.gz.md5
2025-09-23 09:30:41 1147526684 48_S959_R2_001.fastq.gz
2025-09-23 09:30:53         57 48_S959_R2_001.fastq.gz.md5
2025-09-26 15:27:19  271963220 49_S170_R1_001.fastq.gz
2025-09-26 15:27:28         57 49_S170_R1_001.fastq.gz.md5
2025-09-26 15:27:19  270444508 49_S170_R2_001.fastq.gz
2025-09-26 15:27:29         57 49_S170_R2_001.fastq.gz.md5
2025-09-23 09:29:15  817728308 49_S960_R1_001.fastq.gz
2025-09-23 09:29:24         57 49_S960_R1_001.fastq.gz.md5
2025-09-23 09:29:18  811658705 49_S960_R2_001.fastq.gz
2025-09-23 09:29:27         57 49_S960_R2_001.fastq.gz.md5
2025-09-26 15:28:13  368146858 4_S125_R1_001.fastq.gz
2025-09-26 15:28:22         56 4_S125_R1_001.fastq.gz.md5
2025-09-26 15:28:16  365405091 4_S125_R2_001.fastq.gz
2025-09-26 15:28:19         56 4_S125_R2_001.fastq.gz.md5
2025-09-23 09:30:26 1102879277 4_S915_R1_001.fastq.gz
2025-09-23 09:30:39         56 4_S915_R1_001.fastq.gz.md5
2025-09-23 09:30:29 1090096121 4_S915_R2_001.fastq.gz
2025-09-23 09:30:41         56 4_S915_R2_001.fastq.gz.md5
2025-09-26 15:27:09  294580496 50_S171_R1_001.fastq.gz
2025-09-26 15:27:21         57 50_S171_R1_001.fastq.gz.md5
2025-09-26 15:27:11  293493398 50_S171_R2_001.fastq.gz
2025-09-26 15:27:23         57 50_S171_R2_001.fastq.gz.md5
2025-09-23 09:28:56  942227203 50_S961_R1_001.fastq.gz
2025-09-23 09:29:05         57 50_S961_R1_001.fastq.gz.md5
2025-09-23 09:28:56  936688078 50_S961_R2_001.fastq.gz
2025-09-23 09:29:14         57 50_S961_R2_001.fastq.gz.md5
2025-09-26 15:26:59  294530870 51_S172_R1_001.fastq.gz
2025-09-26 15:27:11         57 51_S172_R1_001.fastq.gz.md5
2025-09-26 15:27:02  292906744 51_S172_R2_001.fastq.gz
2025-09-26 15:27:05         57 51_S172_R2_001.fastq.gz.md5
2025-09-23 09:28:36  866464210 51_S962_R1_001.fastq.gz
2025-09-23 09:28:46         57 51_S962_R1_001.fastq.gz.md5
2025-09-23 09:28:35  859915939 51_S962_R2_001.fastq.gz
2025-09-23 09:28:45         57 51_S962_R2_001.fastq.gz.md5
2025-09-26 15:26:18  287786913 52_S173_R1_001.fastq.gz
2025-09-26 15:26:20         57 52_S173_R1_001.fastq.gz.md5
2025-09-26 15:26:18  287061007 52_S173_R2_001.fastq.gz
2025-09-26 15:26:20         57 52_S173_R2_001.fastq.gz.md5
2025-09-23 09:27:32  819250901 52_S963_R1_001.fastq.gz
2025-09-23 09:27:42         57 52_S963_R1_001.fastq.gz.md5
2025-09-23 09:27:31  815599341 52_S963_R2_001.fastq.gz
2025-09-23 09:27:42         57 52_S963_R2_001.fastq.gz.md5
2025-09-26 15:26:23  332130478 53_S174_R1_001.fastq.gz
2025-09-26 15:26:32         57 53_S174_R1_001.fastq.gz.md5
2025-09-26 15:26:29  327414234 53_S174_R2_001.fastq.gz
2025-09-26 15:26:38         57 53_S174_R2_001.fastq.gz.md5
2025-09-23 09:27:54  984470057 53_S964_R1_001.fastq.gz
2025-09-23 09:28:02         57 53_S964_R1_001.fastq.gz.md5
2025-09-23 09:27:54  968782918 53_S964_R2_001.fastq.gz
2025-09-23 09:28:02         57 53_S964_R2_001.fastq.gz.md5
2025-09-26 15:27:44 1765868056 54_S175_R1_001.fastq.gz
2025-09-26 15:27:52         57 54_S175_R1_001.fastq.gz.md5
2025-09-26 15:27:44 1759719215 54_S175_R2_001.fastq.gz
2025-09-26 15:27:57         57 54_S175_R2_001.fastq.gz.md5
2025-09-26 15:27:14  319809749 55_S176_R1_001.fastq.gz
2025-09-26 15:27:16         57 55_S176_R1_001.fastq.gz.md5
2025-09-26 15:27:16  318254839 55_S176_R2_001.fastq.gz
2025-09-26 15:27:29         57 55_S176_R2_001.fastq.gz.md5
2025-09-23 09:29:06  982958681 55_S966_R1_001.fastq.gz
2025-09-23 09:29:17         57 55_S966_R1_001.fastq.gz.md5
2025-09-23 09:29:08  975883160 55_S966_R2_001.fastq.gz
2025-09-23 09:29:24         57 55_S966_R2_001.fastq.gz.md5
2025-09-26 15:26:29  290253182 56_S177_R1_001.fastq.gz
2025-09-26 15:26:38         57 56_S177_R1_001.fastq.gz.md5
2025-09-26 15:26:29  290296568 56_S177_R2_001.fastq.gz
2025-09-26 15:26:41         57 56_S177_R2_001.fastq.gz.md5
2025-09-23 09:27:53  917489787 56_S967_R1_001.fastq.gz
2025-09-23 09:28:02         57 56_S967_R1_001.fastq.gz.md5
2025-09-23 09:27:53  916184588 56_S967_R2_001.fastq.gz
2025-09-23 09:28:02         57 56_S967_R2_001.fastq.gz.md5
2025-09-26 15:26:50  318364886 57_S178_R1_001.fastq.gz
2025-09-26 15:27:02         57 57_S178_R1_001.fastq.gz.md5
2025-09-26 15:26:53  317691683 57_S178_R2_001.fastq.gz
2025-09-26 15:27:02         57 57_S178_R2_001.fastq.gz.md5
2025-09-23 09:28:33 1019196290 57_S968_R1_001.fastq.gz
2025-09-23 09:28:44         57 57_S968_R1_001.fastq.gz.md5
2025-09-23 09:28:35 1013994907 57_S968_R2_001.fastq.gz
2025-09-23 09:28:44         57 57_S968_R2_001.fastq.gz.md5
2025-09-26 15:28:37  502643900 58_S179_R1_001.fastq.gz
2025-09-26 15:28:49         57 58_S179_R1_001.fastq.gz.md5
2025-09-26 15:28:41  504007280 58_S179_R2_001.fastq.gz
2025-09-26 15:28:52         57 58_S179_R2_001.fastq.gz.md5
2025-09-23 09:31:14 1544192904 58_S969_R1_001.fastq.gz
2025-09-23 09:31:23         57 58_S969_R1_001.fastq.gz.md5
2025-09-23 09:31:18 1533897578 58_S969_R2_001.fastq.gz
2025-09-23 09:31:27         57 58_S969_R2_001.fastq.gz.md5
2025-09-26 15:29:07  290857999 59_S180_R1_001.fastq.gz
2025-09-26 15:29:10         57 59_S180_R1_001.fastq.gz.md5
2025-09-26 15:29:06  287868541 59_S180_R2_001.fastq.gz
2025-09-26 15:29:19         57 59_S180_R2_001.fastq.gz.md5
2025-09-23 09:31:57  873067476 59_S970_R1_001.fastq.gz
2025-09-23 09:32:07         57 59_S970_R1_001.fastq.gz.md5
2025-09-23 09:31:57  862418652 59_S970_R2_001.fastq.gz
2025-09-23 09:32:09         57 59_S970_R2_001.fastq.gz.md5
2025-09-26 15:26:56  341306791 5_S126_R1_001.fastq.gz
2025-09-26 15:26:58         56 5_S126_R1_001.fastq.gz.md5
2025-09-26 15:26:58  339970740 5_S126_R2_001.fastq.gz
2025-09-26 15:27:09         56 5_S126_R2_001.fastq.gz.md5
2025-09-23 09:28:36  960779525 5_S916_R1_001.fastq.gz
2025-09-23 09:28:44         56 5_S916_R1_001.fastq.gz.md5
2025-09-23 09:28:35  955164543 5_S916_R2_001.fastq.gz
2025-09-23 09:28:45         56 5_S916_R2_001.fastq.gz.md5
2025-09-26 15:28:10  275424908 60_S181_R1_001.fastq.gz
2025-09-26 15:28:19         57 60_S181_R1_001.fastq.gz.md5
2025-09-26 15:28:10  274786130 60_S181_R2_001.fastq.gz
2025-09-26 15:28:13         57 60_S181_R2_001.fastq.gz.md5
2025-09-23 09:30:17  878885544 60_S971_R1_001.fastq.gz
2025-09-23 09:30:30         57 60_S971_R1_001.fastq.gz.md5
2025-09-23 09:30:21  874297882 60_S971_R2_001.fastq.gz
2025-09-23 09:30:30         57 60_S971_R2_001.fastq.gz.md5
2025-09-26 15:27:09  129017626 61_S182_R1_001.fastq.gz
2025-09-26 15:27:11         57 61_S182_R1_001.fastq.gz.md5
2025-09-26 15:27:09  128728252 61_S182_R2_001.fastq.gz
2025-09-26 15:27:11         57 61_S182_R2_001.fastq.gz.md5
2025-09-23 09:28:53  398343545 61_S972_R1_001.fastq.gz
2025-09-23 09:29:05         57 61_S972_R1_001.fastq.gz.md5
2025-09-23 09:28:56  395952830 61_S972_R2_001.fastq.gz
2025-09-23 09:29:05         57 61_S972_R2_001.fastq.gz.md5
2025-09-26 15:29:01  301018226 62_S183_R1_001.fastq.gz
2025-09-26 15:29:09         57 62_S183_R1_001.fastq.gz.md5
2025-09-26 15:29:01  299125684 62_S183_R2_001.fastq.gz
2025-09-26 15:29:10         57 62_S183_R2_001.fastq.gz.md5
2025-09-23 09:31:44  910832277 62_S973_R1_001.fastq.gz
2025-09-23 09:31:53         57 62_S973_R1_001.fastq.gz.md5
2025-09-23 09:31:48  902094943 62_S973_R2_001.fastq.gz
2025-09-23 09:32:00         57 62_S973_R2_001.fastq.gz.md5
2025-09-26 15:28:41  250580530 63_S184_R1_001.fastq.gz
2025-09-26 15:28:43         57 63_S184_R1_001.fastq.gz.md5
2025-09-26 15:28:41  250170713 63_S184_R2_001.fastq.gz
2025-09-26 15:28:43         57 63_S184_R2_001.fastq.gz.md5
2025-09-23 09:31:17  799150275 63_S974_R1_001.fastq.gz
2025-09-23 09:31:28         57 63_S974_R1_001.fastq.gz.md5
2025-09-23 09:31:17  795282825 63_S974_R2_001.fastq.gz
2025-09-23 09:31:29         57 63_S974_R2_001.fastq.gz.md5
2025-09-26 15:29:10  293025625 64_S185_R1_001.fastq.gz
2025-09-26 15:29:13         57 64_S185_R1_001.fastq.gz.md5
2025-09-26 15:29:09  291386174 64_S185_R2_001.fastq.gz
2025-09-26 15:29:21         57 64_S185_R2_001.fastq.gz.md5
2025-09-23 09:32:00  916698527 64_S975_R1_001.fastq.gz
2025-09-23 09:32:11         57 64_S975_R1_001.fastq.gz.md5
2025-09-23 09:32:03  909202203 64_S975_R2_001.fastq.gz
2025-09-23 09:32:11         57 64_S975_R2_001.fastq.gz.md5
2025-09-26 15:26:10  320509250 6_S127_R1_001.fastq.gz
2025-09-26 15:26:09         56 6_S127_R1_001.fastq.gz.md5
2025-09-26 15:26:09  320058676 6_S127_R2_001.fastq.gz
2025-09-26 15:26:09         56 6_S127_R2_001.fastq.gz.md5
2025-09-23 09:27:12  968902635 6_S917_R1_001.fastq.gz
2025-09-23 09:27:23         56 6_S917_R1_001.fastq.gz.md5
2025-09-23 09:27:12  965482495 6_S917_R2_001.fastq.gz
2025-09-23 09:27:21         56 6_S917_R2_001.fastq.gz.md5
2025-09-26 15:28:30  325918787 7_S128_R1_001.fastq.gz
2025-09-26 15:28:43         56 7_S128_R1_001.fastq.gz.md5
2025-09-26 15:28:34  325573961 7_S128_R2_001.fastq.gz
2025-09-26 15:28:37         56 7_S128_R2_001.fastq.gz.md5
2025-09-23 09:31:04  997026820 7_S918_R1_001.fastq.gz
2025-09-23 09:31:15         56 7_S918_R1_001.fastq.gz.md5
2025-09-23 09:31:03  994968921 7_S918_R2_001.fastq.gz
2025-09-23 09:31:17         56 7_S918_R2_001.fastq.gz.md5
2025-09-26 15:26:09  332323651 8_S129_R1_001.fastq.gz
2025-09-26 15:26:09         56 8_S129_R1_001.fastq.gz.md5
2025-09-26 15:26:09  333288034 8_S129_R2_001.fastq.gz
2025-09-26 15:26:09         56 8_S129_R2_001.fastq.gz.md5
2025-09-23 09:27:12 1070272911 8_S919_R1_001.fastq.gz
2025-09-23 09:27:21         56 8_S919_R1_001.fastq.gz.md5
2025-09-23 09:27:12 1068752075 8_S919_R2_001.fastq.gz
2025-09-23 09:27:21         56 8_S919_R2_001.fastq.gz.md5
2025-09-26 15:27:52  410127602 9_S130_R1_001.fastq.gz
2025-09-26 15:28:01         56 9_S130_R1_001.fastq.gz.md5
2025-09-26 15:27:52  410076073 9_S130_R2_001.fastq.gz
2025-09-26 15:27:55         56 9_S130_R2_001.fastq.gz.md5
2025-09-23 09:29:59 1174407278 9_S920_R1_001.fastq.gz
2025-09-23 09:30:10         56 9_S920_R1_001.fastq.gz.md5
2025-09-23 09:29:59 1171948638 9_S920_R2_001.fastq.gz
2025-09-23 09:30:09         56 9_S920_R2_001.fastq.gz.md5
```

We will concatenate all files for each sample, removing those that are flagged for issues in QC.  

The files are: 

```
find . -type f | wc -l
```

508 files (64 samples * 2 read directions * 2 file types = 256 * 2 sequencing runs = 512 files (one sample - Sample 54 - didn't need resequencing, so there are 4 fewer files) 

- I then concatenated all .md5 files made by the sequencer and made my own .md5 file 

```
cat *.gz.md5 > genohub_original_checksums.md5

md5sum *.fastq.gz > checkmd5_20250929.md5

cmp genohub_original_checksums.md5 checkmd5_20250929.md5
```

All files match check sums, transfer completed successfully. 

# QC on raw sequences 

```
cd /gscratch/srlab/ashuff/moorea-2023-rnaseq
mkdir scripts
cd scripts

nano qc_raw.sh
```

```
#!/bin/bash
#SBATCH --job-name=rawQC
#SBATCH --mail-type=ALL
#SBATCH --mail-user=ashuff@uw.edu

#SBATCH --account=srlab
#SBATCH --partition=cpu-g2-mem2x
#SBATCH --nodes=1
#SBATCH --mem=250G
#SBATCH --time=01-12:00:00 # Max runtime in DD-HH:MM:SS format.

#SBATCH --chdir=/gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences
#SBATCH --export=all
#SBATCH -o raw-qc-%j.out
#SBATCH -e raw-qc-%j.error

# load modules needed

module load kawaldorflab/fastqc/0.12.1
module load kawaldorflab/multiqc/1.15

# fastqc of raw reads
fastqc *.fastq.gz

#generate multiqc report
multiqc ./ --filename multiqc_report_raw.html 

echo "Raw MultiQC report generated." $(date)
```

Run the script. 

```
sbatch qc_raw.sh
```

Submitted as job 29898292 at 20:30 on 29 September, took about 12 hours. 

Copy the MultiQC HTML to my computer. 

```
scp ashuff@klone.hyak.uw.edu:/gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences/multiqc_report_raw.html ~/MyProjects/moorea_symbiotic_exchange_2023/data/rna/QC

#copy single fastQC for troubleshooting
scp ashuff@klone.hyak.uw.edu:/gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences/raw_fastqc/11_S922_R1_001_fastqc.html ~/MyProjects/moorea_symbiotic_exchange_2023/data/rna/QC

scp ashuff@klone.hyak.uw.edu:/gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences/raw_fastqc/38_S949_R1_001_fastqc.html ~/MyProjects/moorea_symbiotic_exchange_2023/data/rna/QC
```

Move .fastqc files to a QC folder to keep folder organized.  

```
mkdir raw_fastqc
mv *fastqc.html raw_fastqc
mv *fastqc.zip raw_fastqc
mv multiqc* raw_fastqc
```

The raw sequence MultiQC [report can be found on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/tree/main/data/rna/QC/multiqc_report_raw.html).  

Here are some things I noticed from the raw QC:  

- Samples did need a second round of sequencing to reach ~15M total reads or more. This is not surprising given lower than ideal RIN and concentrations of RNA from QC reports. There does not appear to be any pattern by species or lifestage.  

| Sample   Name  | %   Dups     | %   GC    | M   Seqs   | Species     | Lifestage |
|----------------|--------------|-----------|------------|-------------|-----------|
| 10_S131_R1_001 |    41.00%    |    43%    |    3       | Pocillopora | Larvae    |
| 10_S131_R2_001 |    37.40%    |    43%    |    3       | Pocillopora | Larvae    |
| 10_S921_R1_001 |    50.00%    |    43%    |    9.1     | Pocillopora | Larvae    |
| 10_S921_R2_001 |    46.00%    |    44%    |    9.1     | Pocillopora | Larvae    |
| 11_S132_R1_001 |    58.30%    |    43%    |    27.9    | Pocillopora | Recruit   |
| 11_S132_R2_001 |    53.90%    |    43%    |    27.9    | Pocillopora | Recruit   |
| 11_S922_R1_001 |    5.20%     |    43%    |    0       | Pocillopora | Recruit   |
| 11_S922_R2_001 |    4.50%     |    43%    |    0       | Pocillopora | Recruit   |
| 12_S133_R1_001 |    62.20%    |    44%    |    3.9     | Acropora    | Larvae    |
| 12_S133_R2_001 |    58.30%    |    45%    |    3.9     | Acropora    | Larvae    |
| 12_S923_R1_001 |    76.00%    |    44%    |    12.3    | Acropora    | Larvae    |
| 12_S923_R2_001 |    73.20%    |    45%    |    12.3    | Acropora    | Larvae    |
| 13_S134_R1_001 |    32.30%    |    42%    |    3.9     | Pocillopora | Larvae    |
| 13_S134_R2_001 |    29.40%    |    43%    |    3.9     | Pocillopora | Larvae    |
| 13_S924_R1_001 |    37.80%    |    42%    |    11.9    | Pocillopora | Larvae    |
| 13_S924_R2_001 |    34.40%    |    43%    |    11.9    | Pocillopora | Larvae    |
| 14_S135_R1_001 |    40.90%    |    43%    |    5.2     | Pocillopora | Recruit   |
| 14_S135_R2_001 |    36.00%    |    43%    |    5.2     | Pocillopora | Recruit   |
| 14_S925_R1_001 |    48.90%    |    43%    |    15.5    | Pocillopora | Recruit   |
| 14_S925_R2_001 |    43.90%    |    44%    |    15.5    | Pocillopora | Recruit   |
| 15_S136_R1_001 |    40.20%    |    42%    |    4.2     | Pocillopora | Larvae    |
| 15_S136_R2_001 |    36.50%    |    43%    |    4.2     | Pocillopora | Larvae    |
| 15_S926_R1_001 |    48.30%    |    43%    |    12.2    | Pocillopora | Larvae    |
| 15_S926_R2_001 |    44.00%    |    43%    |    12.2    | Pocillopora | Larvae    |
| 16_S137_R1_001 |    44.80%    |    42%    |    4.1     | Pocillopora | Larvae    |
| 16_S137_R2_001 |    41.10%    |    42%    |    4.1     | Pocillopora | Larvae    |
| 16_S927_R1_001 |    54.60%    |    42%    |    12.2    | Pocillopora | Larvae    |
| 16_S927_R2_001 |    50.90%    |    43%    |    12.2    | Pocillopora | Larvae    |
| 17_S138_R1_001 |    39.30%    |    41%    |    4.7     | Pocillopora | Recruit   |
| 17_S138_R2_001 |    34.30%    |    42%    |    4.7     | Pocillopora | Recruit   |
| 17_S928_R1_001 |    47.40%    |    42%    |    13.6    | Pocillopora | Recruit   |
| 17_S928_R2_001 |    42.40%    |    42%    |    13.6    | Pocillopora | Recruit   |
| 18_S139_R1_001 |    46.20%    |    42%    |    3.4     | Acropora    | Larvae    |
| 18_S139_R2_001 |    42.80%    |    42%    |    3.4     | Acropora    | Larvae    |
| 18_S929_R1_001 |    56.50%    |    42%    |    9.7     | Acropora    | Larvae    |
| 18_S929_R2_001 |    53.40%    |    43%    |    9.7     | Acropora    | Larvae    |
| 19_S140_R1_001 |    36.80%    |    43%    |    4.2     | Pocillopora | Larvae    |
| 19_S140_R2_001 |    33.80%    |    43%    |    4.2     | Pocillopora | Larvae    |
| 19_S930_R1_001 |    43.50%    |    43%    |    11      | Pocillopora | Larvae    |
| 19_S930_R2_001 |    39.80%    |    44%    |    11      | Pocillopora | Larvae    |
| 1_S122_R1_001  |    42.40%    |    41%    |    4       | Acropora    | Larvae    |
| 1_S122_R2_001  |    38.40%    |    42%    |    4       | Acropora    | Larvae    |
| 1_S912_R1_001  |    50.50%    |    42%    |    11      | Acropora    | Larvae    |
| 1_S912_R2_001  |    46.60%    |    43%    |    11      | Acropora    | Larvae    |
| 20_S141_R1_001 |    42.00%    |    42%    |    5       | Acropora    | Larvae    |
| 20_S141_R2_001 |    37.80%    |    43%    |    5       | Acropora    | Larvae    |
| 20_S931_R1_001 |    50.00%    |    42%    |    14.1    | Acropora    | Larvae    |
| 20_S931_R2_001 |    46.00%    |    43%    |    14.1    | Acropora    | Larvae    |
| 21_S142_R1_001 |    40.60%    |    42%    |    3.8     | Acropora    | Larvae    |
| 21_S142_R2_001 |    36.60%    |    42%    |    3.8     | Acropora    | Larvae    |
| 21_S932_R1_001 |    48.20%    |    42%    |    11      | Acropora    | Larvae    |
| 21_S932_R2_001 |    44.20%    |    43%    |    11      | Acropora    | Larvae    |
| 22_S143_R1_001 |    41.60%    |    42%    |    3.5     | Pocillopora | Larvae    |
| 22_S143_R2_001 |    38.80%    |    43%    |    3.5     | Pocillopora | Larvae    |
| 22_S933_R1_001 |    49.90%    |    43%    |    10.3    | Pocillopora | Larvae    |
| 22_S933_R2_001 |    46.80%    |    43%    |    10.3    | Pocillopora | Larvae    |
| 23_S144_R1_001 |    34.80%    |    42%    |    3.2     | Acropora    | Recruit   |
| 23_S144_R2_001 |    29.90%    |    43%    |    3.2     | Acropora    | Recruit   |
| 23_S934_R1_001 |    44.90%    |    43%    |    10.9    | Acropora    | Recruit   |
| 23_S934_R2_001 |    39.80%    |    44%    |    10.9    | Acropora    | Recruit   |
| 24_S145_R1_001 |    40.90%    |    41%    |    4.5     | Acropora    | Larvae    |
| 24_S145_R2_001 |    37.90%    |    42%    |    4.5     | Acropora    | Larvae    |
| 24_S935_R1_001 |    49.60%    |    42%    |    12.8    | Acropora    | Larvae    |
| 24_S935_R2_001 |    46.40%    |    42%    |    12.8    | Acropora    | Larvae    |
| 25_S146_R1_001 |    46.40%    |    42%    |    3.8     | Pocillopora | Recruit   |
| 25_S146_R2_001 |    42.10%    |    43%    |    3.8     | Pocillopora | Recruit   |
| 25_S936_R1_001 |    59.50%    |    43%    |    11.9    | Pocillopora | Recruit   |
| 25_S936_R2_001 |    55.90%    |    44%    |    11.9    | Pocillopora | Recruit   |
| 26_S147_R1_001 |    38.00%    |    42%    |    4.4     | Pocillopora | Larvae    |
| 26_S147_R2_001 |    34.70%    |    43%    |    4.4     | Pocillopora | Larvae    |
| 26_S937_R1_001 |    46.20%    |    43%    |    12.5    | Pocillopora | Larvae    |
| 26_S937_R2_001 |    42.40%    |    43%    |    12.5    | Pocillopora | Larvae    |
| 27_S148_R1_001 |    38.20%    |    42%    |    4.6     | Acropora    | Recruit   |
| 27_S148_R2_001 |    33.80%    |    42%    |    4.6     | Acropora    | Recruit   |
| 27_S938_R1_001 |    46.50%    |    42%    |    13.1    | Acropora    | Recruit   |
| 27_S938_R2_001 |    42.30%    |    43%    |    13.1    | Acropora    | Recruit   |
| 28_S149_R1_001 |    34.90%    |    43%    |    4.2     | Acropora    | Recruit   |
| 28_S149_R2_001 |    30.30%    |    43%    |    4.2     | Acropora    | Recruit   |
| 28_S939_R1_001 |    44.00%    |    43%    |    12.7    | Acropora    | Recruit   |
| 28_S939_R2_001 |    39.40%    |    44%    |    12.7    | Acropora    | Recruit   |
| 29_S150_R1_001 |    39.40%    |    43%    |    4.9     | Acropora    | Recruit   |
| 29_S150_R2_001 |    35.20%    |    43%    |    4.9     | Acropora    | Recruit   |
| 29_S940_R1_001 |    48.60%    |    43%    |    14.8    | Acropora    | Recruit   |
| 29_S940_R2_001 |    44.60%    |    44%    |    14.8    | Acropora    | Recruit   |
| 2_S123_R1_001  |    39.10%    |    44%    |    4.6     | Acropora    | Recruit   |
| 2_S123_R2_001  |    35.20%    |    44%    |    4.6     | Acropora    | Recruit   |
| 2_S913_R1_001  |    48.30%    |    44%    |    13.1    | Acropora    | Recruit   |
| 2_S913_R2_001  |    44.50%    |    45%    |    13.1    | Acropora    | Recruit   |
| 30_S151_R1_001 |    49.60%    |    42%    |    9.4     | Pocillopora | Larvae    |
| 30_S151_R2_001 |    46.80%    |    43%    |    9.4     | Pocillopora | Larvae    |
| 30_S941_R1_001 |    59.60%    |    42%    |    27.4    | Pocillopora | Larvae    |
| 30_S941_R2_001 |    56.70%    |    43%    |    27.4    | Pocillopora | Larvae    |
| 31_S152_R1_001 |    39.70%    |    41%    |    4.9     | Pocillopora | Recruit   |
| 31_S152_R2_001 |    34.80%    |    42%    |    4.9     | Pocillopora | Recruit   |
| 31_S942_R1_001 |    48.80%    |    42%    |    14.7    | Pocillopora | Recruit   |
| 31_S942_R2_001 |    43.80%    |    42%    |    14.7    | Pocillopora | Recruit   |
| 32_S153_R1_001 |    41.80%    |    43%    |    5       | Acropora    | Recruit   |
| 32_S153_R2_001 |    38.30%    |    43%    |    5       | Acropora    | Recruit   |
| 32_S943_R1_001 |    50.60%    |    43%    |    14.1    | Acropora    | Recruit   |
| 32_S943_R2_001 |    47.40%    |    44%    |    14.1    | Acropora    | Recruit   |
| 33_S154_R1_001 |    38.60%    |    42%    |    4.3     | Pocillopora | Recruit   |
| 33_S154_R2_001 |    34.10%    |    43%    |    4.3     | Pocillopora | Recruit   |
| 33_S944_R1_001 |    47.00%    |    42%    |    12.5    | Pocillopora | Recruit   |
| 33_S944_R2_001 |    42.50%    |    43%    |    12.5    | Pocillopora | Recruit   |
| 34_S155_R1_001 |    41.40%    |    42%    |    4.1     | Pocillopora | Larvae    |
| 34_S155_R2_001 |    39.00%    |    43%    |    4.1     | Pocillopora | Larvae    |
| 34_S945_R1_001 |    48.70%    |    42%    |    11.3    | Pocillopora | Larvae    |
| 34_S945_R2_001 |    45.60%    |    43%    |    11.3    | Pocillopora | Larvae    |
| 35_S156_R1_001 |    38.00%    |    43%    |    4.2     | Acropora    | Recruit   |
| 35_S156_R2_001 |    33.90%    |    43%    |    4.2     | Acropora    | Recruit   |
| 35_S946_R1_001 |    46.20%    |    43%    |    12      | Acropora    | Recruit   |
| 35_S946_R2_001 |    41.80%    |    43%    |    12      | Acropora    | Recruit   |
| 36_S157_R1_001 |    41.40%    |    43%    |    5.5     | Acropora    | Recruit   |
| 36_S157_R2_001 |    36.00%    |    43%    |    5.5     | Acropora    | Recruit   |
| 36_S947_R1_001 |    49.50%    |    43%    |    14.9    | Acropora    | Recruit   |
| 36_S947_R2_001 |    44.30%    |    44%    |    14.9    | Acropora    | Recruit   |
| 37_S158_R1_001 |    39.10%    |    42%    |    4.2     | Acropora    | Larvae    |
| 37_S158_R2_001 |    37.00%    |    42%    |    4.2     | Acropora    | Larvae    |
| 37_S948_R1_001 |    46.10%    |    42%    |    11.3    | Acropora    | Larvae    |
| 37_S948_R2_001 |    43.90%    |    43%    |    11.3    | Acropora    | Larvae    |
| 38_S159_R1_001 |    59.90%    |    43%    |    28.2    | Pocillopora | Larvae    |
| 38_S159_R2_001 |    57.80%    |    43%    |    28.2    | Pocillopora | Larvae    |
| 38_S949_R1_001 |    0.00%     |    41%    |    0       | Pocillopora | Larvae    |
| 38_S949_R2_001 |    0.00%     |    42%    |    0       | Pocillopora | Larvae    |
| 39_S160_R1_001 |    39.70%    |    41%    |    3       | Acropora    | Larvae    |
| 39_S160_R2_001 |    36.70%    |    42%    |    3       | Acropora    | Larvae    |
| 39_S950_R1_001 |    50.10%    |    41%    |    10.1    | Acropora    | Larvae    |
| 39_S950_R2_001 |    47.10%    |    42%    |    10.1    | Acropora    | Larvae    |
| 3_S124_R1_001  |    41.60%    |    41%    |    4.5     | Acropora    | Larvae    |
| 3_S124_R2_001  |    38.90%    |    42%    |    4.5     | Acropora    | Larvae    |
| 3_S914_R1_001  |    49.20%    |    42%    |    11.9    | Acropora    | Larvae    |
| 3_S914_R2_001  |    46.50%    |    42%    |    11.9    | Acropora    | Larvae    |
| 40_S161_R1_001 |    42.00%    |    42%    |    4.8     | Acropora    | Larvae    |
| 40_S161_R2_001 |    39.50%    |    42%    |    4.8     | Acropora    | Larvae    |
| 40_S951_R1_001 |    50.70%    |    42%    |    13.1    | Acropora    | Larvae    |
| 40_S951_R2_001 |    48.20%    |    42%    |    13.1    | Acropora    | Larvae    |
| 41_S162_R1_001 |    39.10%    |    41%    |    3.8     | Acropora    | Larvae    |
| 41_S162_R2_001 |    36.00%    |    42%    |    3.8     | Acropora    | Larvae    |
| 41_S952_R1_001 |    47.00%    |    42%    |    10.7    | Acropora    | Larvae    |
| 41_S952_R2_001 |    43.80%    |    43%    |    10.7    | Acropora    | Larvae    |
| 42_S163_R1_001 |    44.80%    |    43%    |    5.3     | Acropora    | Recruit   |
| 42_S163_R2_001 |    39.80%    |    44%    |    5.3     | Acropora    | Recruit   |
| 42_S953_R1_001 |    53.60%    |    43%    |    15.7    | Acropora    | Recruit   |
| 42_S953_R2_001 |    49.00%    |    44%    |    15.7    | Acropora    | Recruit   |
| 43_S164_R1_001 |    36.50%    |    42%    |    4.6     | Acropora    | Recruit   |
| 43_S164_R2_001 |    32.50%    |    43%    |    4.6     | Acropora    | Recruit   |
| 43_S954_R1_001 |    43.40%    |    43%    |    12.5    | Acropora    | Recruit   |
| 43_S954_R2_001 |    39.00%    |    43%    |    12.5    | Acropora    | Recruit   |
| 44_S165_R1_001 |    37.30%    |    43%    |    4.2     | Acropora    | Recruit   |
| 44_S165_R2_001 |    33.70%    |    44%    |    4.2     | Acropora    | Recruit   |
| 44_S955_R1_001 |    45.80%    |    43%    |    11.9    | Acropora    | Recruit   |
| 44_S955_R2_001 |    42.20%    |    44%    |    11.9    | Acropora    | Recruit   |
| 45_S166_R1_001 |    45.80%    |    41%    |    4.4     | Pocillopora | Larvae    |
| 45_S166_R2_001 |    42.80%    |    42%    |    4.4     | Pocillopora | Larvae    |
| 45_S956_R1_001 |    54.80%    |    42%    |    12.4    | Pocillopora | Larvae    |
| 45_S956_R2_001 |    52.20%    |    42%    |    12.4    | Pocillopora | Larvae    |
| 46_S167_R1_001 |    32.50%    |    45%    |    3.9     | Acropora    | Recruit   |
| 46_S167_R2_001 |    29.30%    |    46%    |    3.9     | Acropora    | Recruit   |
| 46_S957_R1_001 |    41.00%    |    45%    |    11.7    | Acropora    | Recruit   |
| 46_S957_R2_001 |    37.30%    |    46%    |    11.7    | Acropora    | Recruit   |
| 47_S168_R1_001 |    37.60%    |    42%    |    4.9     | Pocillopora | Recruit   |
| 47_S168_R2_001 |    33.30%    |    43%    |    4.9     | Pocillopora | Recruit   |
| 47_S958_R1_001 |    45.90%    |    43%    |    13.9    | Pocillopora | Recruit   |
| 47_S958_R2_001 |    41.30%    |    43%    |    13.9    | Pocillopora | Recruit   |
| 48_S169_R1_001 |    40.40%    |    42%    |    5.3     | Acropora    | Larvae    |
| 48_S169_R2_001 |    35.90%    |    43%    |    5.3     | Acropora    | Larvae    |
| 48_S959_R1_001 |    49.30%    |    42%    |    15.9    | Acropora    | Larvae    |
| 48_S959_R2_001 |    45.20%    |    43%    |    15.9    | Acropora    | Larvae    |
| 49_S170_R1_001 |    36.70%    |    43%    |    4       | Pocillopora | Larvae    |
| 49_S170_R2_001 |    34.30%    |    43%    |    4       | Pocillopora | Larvae    |
| 49_S960_R1_001 |    43.90%    |    43%    |    11.2    | Pocillopora | Larvae    |
| 49_S960_R2_001 |    41.00%    |    44%    |    11.2    | Pocillopora | Larvae    |
| 4_S125_R1_001  |    52.20%    |    41%    |    5.4     | Pocillopora | Larvae    |
| 4_S125_R2_001  |    47.00%    |    42%    |    5.4     | Pocillopora | Larvae    |
| 4_S915_R1_001  |    61.30%    |    42%    |    15.2    | Pocillopora | Larvae    |
| 4_S915_R2_001  |    56.40%    |    42%    |    15.2    | Pocillopora | Larvae    |
| 50_S171_R1_001 |    39.90%    |    43%    |    4.3     | Acropora    | Recruit   |
| 50_S171_R2_001 |    35.90%    |    43%    |    4.3     | Acropora    | Recruit   |
| 50_S961_R1_001 |    48.50%    |    43%    |    13      | Acropora    | Recruit   |
| 50_S961_R2_001 |    44.30%    |    44%    |    13      | Acropora    | Recruit   |
| 51_S172_R1_001 |    38.40%    |    43%    |    4.3     | Pocillopora | Larvae    |
| 51_S172_R2_001 |    36.00%    |    43%    |    4.3     | Pocillopora | Larvae    |
| 51_S962_R1_001 |    45.30%    |    43%    |    11.8    | Pocillopora | Larvae    |
| 51_S962_R2_001 |    42.40%    |    43%    |    11.8    | Pocillopora | Larvae    |
| 52_S173_R1_001 |    37.60%    |    42%    |    4.2     | Pocillopora | Larvae    |
| 52_S173_R2_001 |    35.10%    |    43%    |    4.2     | Pocillopora | Larvae    |
| 52_S963_R1_001 |    44.50%    |    43%    |    11.1    | Pocillopora | Larvae    |
| 52_S963_R2_001 |    41.50%    |    43%    |    11.1    | Pocillopora | Larvae    |
| 53_S174_R1_001 |    38.90%    |    43%    |    4.8     | Acropora    | Recruit   |
| 53_S174_R2_001 |    35.60%    |    43%    |    4.8     | Acropora    | Recruit   |
| 53_S964_R1_001 |    47.30%    |    43%    |    13.3    | Acropora    | Recruit   |
| 53_S964_R2_001 |    43.50%    |    43%    |    13.3    | Acropora    | Recruit   |
| 54_S175_R1_001 |    58.20%    |    43%    |    26      | Acropora    | Recruit   |
| 54_S175_R2_001 |    54.80%    |    44%    |    26      | Acropora    | Recruit   |
| 55_S176_R1_001 |    38.90%    |    41%    |    4.7     | Pocillopora | Recruit   |
| 55_S176_R2_001 |    34.60%    |    42%    |    4.7     | Pocillopora | Recruit   |
| 55_S966_R1_001 |    47.40%    |    42%    |    13.5    | Pocillopora | Recruit   |
| 55_S966_R2_001 |    42.80%    |    42%    |    13.5    | Pocillopora | Recruit   |
| 56_S177_R1_001 |    41.40%    |    41%    |    4.3     | Acropora    | Larvae    |
| 56_S177_R2_001 |    38.70%    |    42%    |    4.3     | Acropora    | Larvae    |
| 56_S967_R1_001 |    50.50%    |    42%    |    12.7    | Acropora    | Larvae    |
| 56_S967_R2_001 |    47.60%    |    42%    |    12.7    | Acropora    | Larvae    |
| 57_S178_R1_001 |    38.10%    |    42%    |    4.7     | Acropora    | Larvae    |
| 57_S178_R2_001 |    34.30%    |    42%    |    4.7     | Acropora    | Larvae    |
| 57_S968_R1_001 |    45.90%    |    42%    |    14      | Acropora    | Larvae    |
| 57_S968_R2_001 |    41.50%    |    43%    |    14      | Acropora    | Larvae    |
| 58_S179_R1_001 |    22.60%    |    42%    |    7.4     | Pocillopora | Larvae    |
| 58_S179_R2_001 |    21.00%    |    43%    |    7.4     | Pocillopora | Larvae    |
| 58_S969_R1_001 |    28.30%    |    43%    |    21.4    | Pocillopora | Larvae    |
| 58_S969_R2_001 |    26.50%    |    44%    |    21.4    | Pocillopora | Larvae    |
| 59_S180_R1_001 |    38.00%    |    42%    |    4.2     | Acropora    | Recruit   |
| 59_S180_R2_001 |    33.70%    |    43%    |    4.2     | Acropora    | Recruit   |
| 59_S970_R1_001 |    46.00%    |    42%    |    11.9    | Acropora    | Recruit   |
| 59_S970_R2_001 |    41.40%    |    43%    |    11.9    | Acropora    | Recruit   |
| 5_S126_R1_001  |    44.70%    |    42%    |    5       | Pocillopora | Larvae    |
| 5_S126_R2_001  |    41.90%    |    43%    |    5       | Pocillopora | Larvae    |
| 5_S916_R1_001  |    51.60%    |    42%    |    13.1    | Pocillopora | Larvae    |
| 5_S916_R2_001  |    48.60%    |    43%    |    13.1    | Pocillopora | Larvae    |
| 60_S181_R1_001 |    40.50%    |    41%    |    4.1     | Acropora    | Larvae    |
| 60_S181_R2_001 |    37.50%    |    42%    |    4.1     | Acropora    | Larvae    |
| 60_S971_R1_001 |    48.90%    |    42%    |    12.2    | Acropora    | Larvae    |
| 60_S971_R2_001 |    46.00%    |    42%    |    12.2    | Acropora    | Larvae    |
| 61_S182_R1_001 |    28.80%    |    43%    |    1.9     | Pocillopora | Recruit   |
| 61_S182_R2_001 |    24.70%    |    43%    |    1.9     | Pocillopora | Recruit   |
| 61_S972_R1_001 |    35.30%    |    43%    |    5.4     | Pocillopora | Recruit   |
| 61_S972_R2_001 |    30.70%    |    44%    |    5.4     | Pocillopora | Recruit   |
| 62_S183_R1_001 |    35.90%    |    42%    |    4.4     | Pocillopora | Recruit   |
| 62_S183_R2_001 |    31.40%    |    42%    |    4.4     | Pocillopora | Recruit   |
| 62_S973_R1_001 |    44.10%    |    42%    |    12.4    | Pocillopora | Recruit   |
| 62_S973_R2_001 |    39.70%    |    43%    |    12.4    | Pocillopora | Recruit   |
| 63_S184_R1_001 |    38.70%    |    42%    |    3.7     | Acropora    | Larvae    |
| 63_S184_R2_001 |    35.70%    |    42%    |    3.7     | Acropora    | Larvae    |
| 63_S974_R1_001 |    47.20%    |    42%    |    11      | Acropora    | Larvae    |
| 63_S974_R2_001 |    44.00%    |    42%    |    11      | Acropora    | Larvae    |
| 64_S185_R1_001 |    46.90%    |    41%    |    4.3     | Acropora    | Recruit   |
| 64_S185_R2_001 |    42.50%    |    42%    |    4.3     | Acropora    | Recruit   |
| 64_S975_R1_001 |    56.30%    |    42%    |    12.6    | Acropora    | Recruit   |
| 64_S975_R2_001 |    52.60%    |    42%    |    12.6    | Acropora    | Recruit   |
| 6_S127_R1_001  |    44.50%    |    43%    |    4.7     | Acropora    | Recruit   |
| 6_S127_R2_001  |    39.10%    |    43%    |    4.7     | Acropora    | Recruit   |
| 6_S917_R1_001  |    54.10%    |    43%    |    13.3    | Acropora    | Recruit   |
| 6_S917_R2_001  |    48.80%    |    44%    |    13.3    | Acropora    | Recruit   |
| 7_S128_R1_001  |    35.50%    |    45%    |    4.8     | Pocillopora | Larvae    |
| 7_S128_R2_001  |    31.90%    |    45%    |    4.8     | Pocillopora | Larvae    |
| 7_S918_R1_001  |    42.80%    |    45%    |    13.7    | Pocillopora | Larvae    |
| 7_S918_R2_001  |    38.50%    |    45%    |    13.7    | Pocillopora | Larvae    |
| 8_S129_R1_001  |    40.80%    |    42%    |    4.9     | Acropora    | Larvae    |
| 8_S129_R2_001  |    37.50%    |    43%    |    4.9     | Acropora    | Larvae    |
| 8_S919_R1_001  |    49.70%    |    42%    |    14.9    | Acropora    | Larvae    |
| 8_S919_R2_001  |    46.30%    |    43%    |    14.9    | Acropora    | Larvae    |
| 9_S130_R1_001  |    41.70%    |    41%    |    6       | Acropora    | Larvae    |
| 9_S130_R2_001  |    37.60%    |    42%    |    6       | Acropora    | Larvae    |
| 9_S920_R1_001  |    48.60%    |    42%    |    16      | Acropora    | Larvae    |
| 9_S920_R2_001  |    44.10%    |    42%    |    16      | Acropora    | Larvae    |

- There are four files per samples and the second round of sequencing was needed to obtain at least 15M sequences. There is high duplication, which is not uncommon in RNA datasets. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic1.png?raw=true)

- Quality scores were high on average. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic2.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic3.png?raw=true)

- GC sequence content looks odd with one particular sample. We will look at this again following trimming and we will see how the sequences perform in alignment.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic4.png?raw=true)

- N content looks good. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic5.png?raw=true)

- Duplication varies with some samples showing higher duplication. This is not uncommon with RNA seqeuences especially if there was low RNA concentration and it was sequenced deeply. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic6.png?raw=true)

- Sample 38 had high overrepresented sequences. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic7.png?raw=true)

- Adapters are present and will be removed in trimming.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Moorea2023/20250930/pic8.png?raw=true)

Samples `11_S922_R1_001` (no reads) and `38_S949_R1_001` (bad GC and quality) fail QC checks, so instead we will use the other sequence run files for these samples (11_S132 and 38_S159).  

# Trimming raw sequences 

I first ran a step to trim adapters from sequences. I will then generate another QC report to look at the results before making other trimming decisions.  

I first moved the .md5 files to an md5 folder to keep  only .fastq files in `raw-sequences`. 

```
cd raw-sequences
mkdir md5_files

mv *.md5 md5_files
```

Make a new folder for trimmed sequences. 

```
cd ../
mkdir trimmed-sequences
```

Find the program in the Roberts Lab bioinformatics container. 

```
cd /gscratch/srlab/containers/
ls -la 
```

Most recent container is `srlab-R4.4-bioinformatics-container-f050784.sif`.  

```
cd scripts
nano trim_adapters_slurm.sh
``` 

I will use the following settings for trimming in `fastp`. [Fastp documentation can be found here](https://github.com/OpenGene/fastp).   

- `detect_adapter_for_pe \`
	- This enables auto detection of adapters for paired end data 
- `qualified_quality_phred 30 \`
	- Filters reads based on phred score >=30
- `unqualified_percent_limit 10 \`
	- percents of bases are allowed to be unqualified, set here as 10% 
- `length_required 100 \`
	- Removes reads shorter than 100 bp. We have read lengths of 150 bp. 
- `cut_right cut_right_window_size 5 cut_right_mean_quality 20`
	- Jill used this sliding cut window in her script. I am going to leave it out for now and evaluate the QC to see if we need to implement cutting.  

Make the job script. 

```
nano trim_adapters_slurm.sh
```

```
#!/bin/bash
#SBATCH --job-name=adapter-trimming
#SBATCH --mail-type=ALL
#SBATCH --mail-user=ashuff@uw.edu

#SBATCH --account=srlab
#SBATCH --partition=cpu-g2-mem2x
#SBATCH --nodes=1
#SBATCH --mem=250G
#SBATCH --time=02-12:00:00 # Max runtime in DD-HH:MM:SS format.

#SBATCH --chdir=/gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences
#SBATCH --export=all
#SBATCH -o adapter-trim-%j.out
#SBATCH -e adapter-trim-%j.error

# load modules 
module load apptainer

apptainer exec \
--home $PWD \
--bind /mmfs1/home/ \
--bind /mmfs1/gscratch/ \
--bind /gscratch/ \
/gscratch/srlab/containers/srlab-R4.4-bioinformatics-container-f050784.sif \
/gscratch/srlab/ashuff/moorea-2023-rnaseq/scripts/trim_adapters.sh
```

Make the script for the specific job. 

```
nano trim_adapters.sh
#save file 
chmod +x trim_adapters.sh
```

```
#!/bin/bash

# This script is designed to be called by a SLURM script which
# runs this script across an array of HPC nodes.

# Make an array of sequences to trim in raw data directory 
cd /gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences/
array1=($(ls *R1_001.fastq.gz))

echo "Read trimming of adapters started." $(date)

# fastp and fastqc loop 
for i in ${array1[@]}; do
    fastp --in1 ${i} \
        --in2 $(echo ${i}|sed s/_R1/_R2/)\
        --out1 /gscratch/srlab/ashuff/moorea-2023-rnaseq/trimmed-sequences/trim.${i} \
        --out2 /gscratch/srlab/ashuff/moorea-2023-rnaseq/trimmed-sequences/trim.$(echo ${i}|sed s/_R1/_R2/) \
        --detect_adapter_for_pe \
        --qualified_quality_phred 30 \
        --unqualified_percent_limit 10 \
        --length_required 100 

done

echo "Read trimming of adapters completed." $(date)
```

```
sbatch trim_adapters_slurm.sh
``` 

Submitted job 29916491 at 17:00 on Sep 30. Job finished Oct 1. 

# QC of trimmed sequences 


```
cd scripts

nano qc_trimmed.sh
```

```
#!/bin/bash
#SBATCH --job-name=trimmedQC
#SBATCH --mail-type=ALL
#SBATCH --mail-user=ashuff@uw.edu

#SBATCH --account=srlab
#SBATCH --partition=cpu-g2-mem2x
#SBATCH --nodes=1
#SBATCH --mem=250G
#SBATCH --time=01-12:00:00 # Max runtime in DD-HH:MM:SS format.

#SBATCH --chdir=/gscratch/srlab/ashuff/moorea-2023-rnaseq/trimmed-sequences
#SBATCH --export=all
#SBATCH -o trimmed-qc-%j.out
#SBATCH -e trimmed-qc-%j.error

# load modules needed

module load kawaldorflab/fastqc/0.12.1
module load kawaldorflab/multiqc/1.15

# fastqc of raw reads
fastqc trim*.fastq.gz

#generate multiqc report
multiqc ./ --filename multiqc_report_trimmed.html 

echo "Trimmed MultiQC report generated." $(date)
```

Run the script. 

```
sbatch qc_trimmed.sh
```

Job 29937034 on Oct 1 at 09:28, completed at 12:00.  

Move to my computer. 

```
scp ashuff@klone.hyak.uw.edu:/gscratch/srlab/ashuff/moorea-2023-rnaseq/trimmed-sequences/multiqc_report_trimmed.html ~/MyProjects/moorea_symbiotic_exchange_2023/data/rna/QC
```  

I then examined the QC information for the trimmed sequences. The MultiQC for the trimmed sequences can be [found here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/tree/main/data/rna/QC/multiqc_report_trimmed.html).  

Here are the stats for all files following trimming.  

| Sample Name         | % Dups | % GC | M Seqs |
|---------------------|--------|------|--------|
| trim.10_S131_R1_001 |  39.8% |  42% |  2.6   |
| trim.10_S131_R2_001 |  36.5% |  42% |  2.6   |
| trim.10_S921_R1_001 |  48.6% |  42% |  7.5   |
| trim.10_S921_R2_001 |  44.9% |  42% |  7.5   |
| trim.11_S132_R1_001 |  57.5% |  42% |  24.4  |
| trim.11_S132_R2_001 |  53.3% |  42% |  24.4  |
| trim.11_S922_R1_001 |  3.3%  |  42% |  0.0   |
| trim.11_S922_R2_001 |  3.0%  |  42% |  0.0   |
| trim.12_S133_R1_001 |  60.6% |  42% |  3.3   |
| trim.12_S133_R2_001 |  58.1% |  43% |  3.3   |
| trim.12_S923_R1_001 |  75.3% |  42% |  9.7   |
| trim.12_S923_R2_001 |  73.9% |  42% |  9.7   |
| trim.13_S134_R1_001 |  32.0% |  42% |  3.5   |
| trim.13_S134_R2_001 |  29.2% |  42% |  3.5   |
| trim.13_S924_R1_001 |  37.7% |  42% |  9.8   |
| trim.13_S924_R2_001 |  34.4% |  42% |  9.8   |
| trim.14_S135_R1_001 |  40.3% |  42% |  4.7   |
| trim.14_S135_R2_001 |  35.4% |  43% |  4.7   |
| trim.14_S925_R1_001 |  48.4% |  43% |  12.9  |
| trim.14_S925_R2_001 |  43.4% |  43% |  12.9  |
| trim.15_S136_R1_001 |  39.3% |  42% |  3.8   |
| trim.15_S136_R2_001 |  35.8% |  42% |  3.8   |
| trim.15_S926_R1_001 |  47.5% |  42% |  10.2  |
| trim.15_S926_R2_001 |  43.3% |  42% |  10.2  |
| trim.16_S137_R1_001 |  43.8% |  41% |  3.5   |
| trim.16_S137_R2_001 |  40.2% |  41% |  3.5   |
| trim.16_S927_R1_001 |  53.5% |  41% |  9.9   |
| trim.16_S927_R2_001 |  50.3% |  41% |  9.9   |
| trim.17_S138_R1_001 |  38.5% |  41% |  4.2   |
| trim.17_S138_R2_001 |  33.6% |  41% |  4.2   |
| trim.17_S928_R1_001 |  46.5% |  41% |  11.3  |
| trim.17_S928_R2_001 |  41.8% |  41% |  11.3  |
| trim.18_S139_R1_001 |  45.1% |  41% |  3.0   |
| trim.18_S139_R2_001 |  41.9% |  41% |  3.0   |
| trim.18_S929_R1_001 |  55.4% |  41% |  7.9   |
| trim.18_S929_R2_001 |  52.6% |  41% |  7.9   |
| trim.19_S140_R1_001 |  36.0% |  43% |  3.8   |
| trim.19_S140_R2_001 |  33.1% |  43% |  3.8   |
| trim.19_S930_R1_001 |  42.6% |  43% |  9.2   |
| trim.19_S930_R2_001 |  39.1% |  43% |  9.2   |
| trim.1_S122_R1_001  |  41.4% |  41% |  3.5   |
| trim.1_S122_R2_001  |  37.6% |  41% |  3.5   |
| trim.1_S912_R1_001  |  49.4% |  41% |  9.2   |
| trim.1_S912_R2_001  |  45.9% |  41% |  9.2   |
| trim.20_S141_R1_001 |  41.4% |  42% |  4.5   |
| trim.20_S141_R2_001 |  37.2% |  42% |  4.5   |
| trim.20_S931_R1_001 |  49.4% |  42% |  11.6  |
| trim.20_S931_R2_001 |  45.5% |  42% |  11.6  |
| trim.21_S142_R1_001 |  39.9% |  41% |  3.4   |
| trim.21_S142_R2_001 |  36.1% |  42% |  3.4   |
| trim.21_S932_R1_001 |  47.5% |  42% |  9.2   |
| trim.21_S932_R2_001 |  43.7% |  42% |  9.2   |
| trim.22_S143_R1_001 |  40.5% |  42% |  3.1   |
| trim.22_S143_R2_001 |  37.9% |  42% |  3.1   |
| trim.22_S933_R1_001 |  48.6% |  42% |  8.5   |
| trim.22_S933_R2_001 |  45.9% |  42% |  8.5   |
| trim.23_S144_R1_001 |  33.6% |  42% |  2.9   |
| trim.23_S144_R2_001 |  28.9% |  42% |  2.9   |
| trim.23_S934_R1_001 |  43.6% |  42% |  9.0   |
| trim.23_S934_R2_001 |  38.6% |  42% |  9.0   |
| trim.24_S145_R1_001 |  39.9% |  41% |  4.0   |
| trim.24_S145_R2_001 |  37.1% |  41% |  4.0   |
| trim.24_S935_R1_001 |  48.7% |  41% |  10.6  |
| trim.24_S935_R2_001 |  45.5% |  41% |  10.6  |
| trim.25_S146_R1_001 |  44.9% |  41% |  3.3   |
| trim.25_S146_R2_001 |  40.8% |  42% |  3.3   |
| trim.25_S936_R1_001 |  58.2% |  41% |  9.6   |
| trim.25_S936_R2_001 |  54.8% |  42% |  9.6   |
| trim.26_S147_R1_001 |  37.3% |  42% |  3.9   |
| trim.26_S147_R2_001 |  34.2% |  42% |  3.9   |
| trim.26_S937_R1_001 |  45.5% |  42% |  10.5  |
| trim.26_S937_R2_001 |  42.0% |  42% |  10.5  |
| trim.27_S148_R1_001 |  37.4% |  42% |  4.2   |
| trim.27_S148_R2_001 |  33.1% |  42% |  4.2   |
| trim.27_S938_R1_001 |  45.8% |  42% |  11.1  |
| trim.27_S938_R2_001 |  41.6% |  42% |  11.1  |
| trim.28_S149_R1_001 |  34.1% |  43% |  3.8   |
| trim.28_S149_R2_001 |  29.6% |  43% |  3.8   |
| trim.28_S939_R1_001 |  43.1% |  43% |  10.7  |
| trim.28_S939_R2_001 |  38.6% |  43% |  10.7  |
| trim.29_S150_R1_001 |  38.5% |  42% |  4.4   |
| trim.29_S150_R2_001 |  34.5% |  43% |  4.4   |
| trim.29_S940_R1_001 |  47.7% |  42% |  12.3  |
| trim.29_S940_R2_001 |  44.1% |  43% |  12.3  |
| trim.2_S123_R1_001  |  38.2% |  43% |  4.0   |
| trim.2_S123_R2_001  |  34.5% |  44% |  4.0   |
| trim.2_S913_R1_001  |  47.3% |  43% |  10.7  |
| trim.2_S913_R2_001  |  43.7% |  44% |  10.7  |
| trim.30_S151_R1_001 |  49.0% |  42% |  8.4   |
| trim.30_S151_R2_001 |  46.4% |  42% |  8.4   |
| trim.30_S941_R1_001 |  59.0% |  42% |  23.0  |
| trim.30_S941_R2_001 |  56.6% |  42% |  23.0  |
| trim.31_S152_R1_001 |  38.9% |  41% |  4.4   |
| trim.31_S152_R2_001 |  34.1% |  41% |  4.4   |
| trim.31_S942_R1_001 |  47.9% |  41% |  12.4  |
| trim.31_S942_R2_001 |  43.1% |  41% |  12.4  |
| trim.32_S153_R1_001 |  40.9% |  42% |  4.3   |
| trim.32_S153_R2_001 |  37.3% |  43% |  4.3   |
| trim.32_S943_R1_001 |  49.8% |  42% |  11.4  |
| trim.32_S943_R2_001 |  46.6% |  43% |  11.4  |
| trim.33_S154_R1_001 |  37.7% |  42% |  3.8   |
| trim.33_S154_R2_001 |  33.4% |  42% |  3.8   |
| trim.33_S944_R1_001 |  46.0% |  42% |  10.5  |
| trim.33_S944_R2_001 |  41.6% |  42% |  10.5  |
| trim.34_S155_R1_001 |  40.7% |  42% |  3.8   |
| trim.34_S155_R2_001 |  38.5% |  42% |  3.8   |
| trim.34_S945_R1_001 |  47.9% |  42% |  9.6   |
| trim.34_S945_R2_001 |  45.3% |  42% |  9.6   |
| trim.35_S156_R1_001 |  37.3% |  42% |  3.8   |
| trim.35_S156_R2_001 |  33.2% |  43% |  3.8   |
| trim.35_S946_R1_001 |  45.5% |  43% |  10.2  |
| trim.35_S946_R2_001 |  41.1% |  43% |  10.2  |
| trim.36_S157_R1_001 |  40.3% |  42% |  4.8   |
| trim.36_S157_R2_001 |  35.0% |  42% |  4.8   |
| trim.36_S947_R1_001 |  48.5% |  42% |  12.2  |
| trim.36_S947_R2_001 |  43.4% |  42% |  12.2  |
| trim.37_S158_R1_001 |  38.5% |  41% |  3.8   |
| trim.37_S158_R2_001 |  36.6% |  42% |  3.8   |
| trim.37_S948_R1_001 |  45.5% |  42% |  9.5   |
| trim.37_S948_R2_001 |  43.7% |  42% |  9.5   |
| trim.38_S159_R1_001 |  59.6% |  42% |  25.2  |
| trim.38_S159_R2_001 |  57.6% |  42% |  25.2  |
| trim.38_S949_R1_001 |  0.0%  |  44% |  0.0   |
| trim.38_S949_R2_001 |  0.0%  |  41% |  0.0   |
| trim.39_S160_R1_001 |  38.7% |  41% |  2.7   |
| trim.39_S160_R2_001 |  36.1% |  41% |  2.7   |
| trim.39_S950_R1_001 |  49.2% |  41% |  8.6   |
| trim.39_S950_R2_001 |  46.7% |  41% |  8.6   |
| trim.3_S124_R1_001  |  40.8% |  41% |  4.0   |
| trim.3_S124_R2_001  |  38.2% |  41% |  4.0   |
| trim.3_S914_R1_001  |  48.4% |  41% |  9.9   |
| trim.3_S914_R2_001  |  45.9% |  42% |  9.9   |
| trim.40_S161_R1_001 |  41.4% |  41% |  4.3   |
| trim.40_S161_R2_001 |  39.0% |  42% |  4.3   |
| trim.40_S951_R1_001 |  50.0% |  41% |  11.0  |
| trim.40_S951_R2_001 |  48.0% |  42% |  11.0  |
| trim.41_S162_R1_001 |  38.2% |  41% |  3.4   |
| trim.41_S162_R2_001 |  35.2% |  41% |  3.4   |
| trim.41_S952_R1_001 |  46.0% |  41% |  8.9   |
| trim.41_S952_R2_001 |  43.0% |  41% |  8.9   |
| trim.42_S163_R1_001 |  43.8% |  43% |  4.7   |
| trim.42_S163_R2_001 |  38.9% |  43% |  4.7   |
| trim.42_S953_R1_001 |  52.9% |  43% |  13.0  |
| trim.42_S953_R2_001 |  48.3% |  43% |  13.0  |
| trim.43_S164_R1_001 |  35.7% |  42% |  4.1   |
| trim.43_S164_R2_001 |  31.9% |  42% |  4.1   |
| trim.43_S954_R1_001 |  42.5% |  42% |  10.3  |
| trim.43_S954_R2_001 |  38.6% |  42% |  10.3  |
| trim.44_S165_R1_001 |  36.6% |  43% |  3.7   |
| trim.44_S165_R2_001 |  33.1% |  43% |  3.7   |
| trim.44_S955_R1_001 |  44.9% |  43% |  9.9   |
| trim.44_S955_R2_001 |  41.5% |  43% |  9.9   |
| trim.45_S166_R1_001 |  45.2% |  41% |  3.9   |
| trim.45_S166_R2_001 |  42.5% |  42% |  3.9   |
| trim.45_S956_R1_001 |  54.2% |  41% |  10.5  |
| trim.45_S956_R2_001 |  51.9% |  42% |  10.5  |
| trim.46_S167_R1_001 |  31.6% |  45% |  3.5   |
| trim.46_S167_R2_001 |  28.7% |  45% |  3.5   |
| trim.46_S957_R1_001 |  40.1% |  45% |  9.7   |
| trim.46_S957_R2_001 |  36.6% |  45% |  9.7   |
| trim.47_S168_R1_001 |  36.6% |  42% |  4.3   |
| trim.47_S168_R2_001 |  32.4% |  42% |  4.3   |
| trim.47_S958_R1_001 |  45.0% |  42% |  11.6  |
| trim.47_S958_R2_001 |  40.6% |  42% |  11.6  |
| trim.48_S169_R1_001 |  39.5% |  41% |  4.7   |
| trim.48_S169_R2_001 |  35.0% |  42% |  4.7   |
| trim.48_S959_R1_001 |  48.5% |  41% |  12.9  |
| trim.48_S959_R2_001 |  44.3% |  42% |  12.9  |
| trim.49_S170_R1_001 |  36.1% |  43% |  3.6   |
| trim.49_S170_R2_001 |  33.8% |  43% |  3.6   |
| trim.49_S960_R1_001 |  43.2% |  43% |  9.5   |
| trim.49_S960_R2_001 |  40.5% |  43% |  9.5   |
| trim.4_S125_R1_001  |  51.6% |  41% |  4.8   |
| trim.4_S125_R2_001  |  46.6% |  41% |  4.8   |
| trim.4_S915_R1_001  |  60.9% |  41% |  12.6  |
| trim.4_S915_R2_001  |  56.4% |  41% |  12.6  |
| trim.50_S171_R1_001 |  39.3% |  43% |  3.9   |
| trim.50_S171_R2_001 |  35.5% |  43% |  3.9   |
| trim.50_S961_R1_001 |  47.9% |  43% |  11.0  |
| trim.50_S961_R2_001 |  44.0% |  43% |  11.0  |
| trim.51_S172_R1_001 |  37.7% |  42% |  3.9   |
| trim.51_S172_R2_001 |  35.5% |  43% |  3.9   |
| trim.51_S962_R1_001 |  44.5% |  43% |  9.9   |
| trim.51_S962_R2_001 |  41.9% |  43% |  9.9   |
| trim.52_S173_R1_001 |  37.0% |  42% |  3.8   |
| trim.52_S173_R2_001 |  34.6% |  42% |  3.8   |
| trim.52_S963_R1_001 |  43.6% |  42% |  9.3   |
| trim.52_S963_R2_001 |  41.0% |  43% |  9.3   |
| trim.53_S174_R1_001 |  38.1% |  42% |  4.3   |
| trim.53_S174_R2_001 |  34.9% |  43% |  4.3   |
| trim.53_S964_R1_001 |  46.5% |  42% |  11.1  |
| trim.53_S964_R2_001 |  42.9% |  43% |  11.1  |
| trim.54_S175_R1_001 |  57.7% |  43% |  23.0  |
| trim.54_S175_R2_001 |  54.5% |  43% |  23.0  |
| trim.55_S176_R1_001 |  38.1% |  41% |  4.2   |
| trim.55_S176_R2_001 |  33.7% |  41% |  4.2   |
| trim.55_S966_R1_001 |  46.5% |  41% |  11.3  |
| trim.55_S966_R2_001 |  42.1% |  41% |  11.3  |
| trim.56_S177_R1_001 |  40.8% |  41% |  3.9   |
| trim.56_S177_R2_001 |  38.2% |  41% |  3.9   |
| trim.56_S967_R1_001 |  49.8% |  41% |  10.8  |
| trim.56_S967_R2_001 |  47.3% |  41% |  10.8  |
| trim.57_S178_R1_001 |  37.3% |  42% |  4.2   |
| trim.57_S178_R2_001 |  33.6% |  42% |  4.2   |
| trim.57_S968_R1_001 |  45.0% |  42% |  11.6  |
| trim.57_S968_R2_001 |  41.0% |  42% |  11.6  |
| trim.58_S179_R1_001 |  21.4% |  42% |  6.2   |
| trim.58_S179_R2_001 |  19.6% |  42% |  6.2   |
| trim.58_S969_R1_001 |  26.6% |  42% |  17.0  |
| trim.58_S969_R2_001 |  24.7% |  42% |  17.0  |
| trim.59_S180_R1_001 |  37.2% |  42% |  3.8   |
| trim.59_S180_R2_001 |  32.9% |  42% |  3.8   |
| trim.59_S970_R1_001 |  45.2% |  42% |  10.0  |
| trim.59_S970_R2_001 |  40.7% |  42% |  10.0  |
| trim.5_S126_R1_001  |  44.0% |  42% |  4.5   |
| trim.5_S126_R2_001  |  41.5% |  42% |  4.5   |
| trim.5_S916_R1_001  |  50.9% |  42% |  10.9  |
| trim.5_S916_R2_001  |  48.3% |  42% |  10.9  |
| trim.60_S181_R1_001 |  39.7% |  41% |  3.7   |
| trim.60_S181_R2_001 |  37.0% |  41% |  3.7   |
| trim.60_S971_R1_001 |  48.1% |  41% |  10.2  |
| trim.60_S971_R2_001 |  45.4% |  41% |  10.2  |
| trim.61_S182_R1_001 |  27.7% |  43% |  1.7   |
| trim.61_S182_R2_001 |  23.9% |  43% |  1.7   |
| trim.61_S972_R1_001 |  34.1% |  43% |  4.5   |
| trim.61_S972_R2_001 |  29.6% |  43% |  4.5   |
| trim.62_S183_R1_001 |  34.9% |  41% |  3.9   |
| trim.62_S183_R2_001 |  30.4% |  42% |  3.9   |
| trim.62_S973_R1_001 |  43.2% |  42% |  10.3  |
| trim.62_S973_R2_001 |  38.8% |  42% |  10.3  |
| trim.63_S184_R1_001 |  37.9% |  41% |  3.3   |
| trim.63_S184_R2_001 |  35.1% |  41% |  3.3   |
| trim.63_S974_R1_001 |  46.4% |  41% |  9.2   |
| trim.63_S974_R2_001 |  43.5% |  41% |  9.2   |
| trim.64_S185_R1_001 |  46.4% |  41% |  3.8   |
| trim.64_S185_R2_001 |  42.0% |  41% |  3.8   |
| trim.64_S975_R1_001 |  56.0% |  41% |  10.4  |
| trim.64_S975_R2_001 |  52.4% |  41% |  10.4  |
| trim.6_S127_R1_001  |  43.7% |  42% |  4.1   |
| trim.6_S127_R2_001  |  38.5% |  43% |  4.1   |
| trim.6_S917_R1_001  |  53.4% |  42% |  11.0  |
| trim.6_S917_R2_001  |  48.4% |  43% |  11.0  |
| trim.7_S128_R1_001  |  34.7% |  44% |  4.3   |
| trim.7_S128_R2_001  |  31.3% |  45% |  4.3   |
| trim.7_S918_R1_001  |  41.9% |  44% |  11.4  |
| trim.7_S918_R2_001  |  38.0% |  45% |  11.4  |
| trim.8_S129_R1_001  |  40.1% |  42% |  4.4   |
| trim.8_S129_R2_001  |  37.0% |  42% |  4.4   |
| trim.8_S919_R1_001  |  48.9% |  42% |  12.4  |
| trim.8_S919_R2_001  |  45.8% |  42% |  12.4  |
| trim.9_S130_R1_001  |  40.9% |  41% |  5.4   |
| trim.9_S130_R2_001  |  37.0% |  41% |  5.4   |
| trim.9_S920_R1_001  |  47.8% |  41% |  13.3  |
| trim.9_S920_R2_001  |  43.8% |  41% |  13.3  |


Files to remove from analysis due to few/no reads and failed QC:    

```
#no reads/few reads after trimming

trim.11_S922_R1_001 
trim.11_S922_R2_001

trim.38_S949_R1_001 
trim.38_S949_R2_001 

```

Other reads will need to be concatenated to include both first and second rounds of sequencing. For some samples, the first round produced more sequences while in others the second round produced more. So we are not able to only select one round of sequencing.  

Other QC checks passed. Adapters were removed, quality is high, and GC content looks good when we remove the samples I listed above.  

Here are some stats: 

- Trimming removed between 0.5-5 M sequences. 
- GC content was reduced 1-2% afer trimming.
- Duplication was reduced 0-2 after triming. 
- Total sequences when totalling both sequencing runs ranged from ~ 6M at the lowest to >25 M at the highest. 

The sample with low (<10 M seqs) are:  
- 61 (POC recruit) 

All other samples have above 10 M seqs with ~ 15 M seqs as the average.  

It is likely we will need to remove sample 61 as an outlier in analysis.  

Move files to QC directory. 

```
cd trimmed-sequences
mkdir trimmed_fastqc

mv *fastqc.html trimmed_fastqc
mv *fastqc.zip trimmed_fastqc
mv multiqc_report_trimmed.html trimmed_fastqc
```

# Test blast files 

I then blasted some of the sequences (3 per file) by hand to check if they mapped to corals as expected. This will serve as a species QC check until I do alignments to reference genomes.   

ACR recruit: Sample 2 `2_S913_R1_001` - Hits = Acropora! 

```
GTCGGTTCAC+ACACCTGTAT
GGGACCGGATCCACTTGGACCAGTTGGTGCAGGTTCTGTTGTAGAGCTCGTCATGGAAGAATTAGCAGCGCCCATACCATGTGACATAGACATCATGGACTCTGAGCTTGCCATATCCATCACCGTCGTCATCGTGCT
#no similarity found

GTCGGTTCAC+ACACCTGTAT
TTCTGCTTTTCGCTTGTCGATCTCCTTACACTTTGCCAGCACAGCAGGCTTTCCCTCTGGAAGGTTCTTGAAGAGAACTCGTGCCATTGCATCTGCCCAGCCCTCATTCACACGTCCAATGTTATCTGCAGTCTTCATATTTTCC
#Acropora palmata, digitifera, hyacinthus 

GTCGGTTCAC+ACACCTGTAT
TGCGGTGTGTCTGACTACTTTTTACATATCAACAATAATAAAGTCATTAAGGAAAAACTAAAGCCAATGAAATGCTTTTTTTTAAAGCAGAACTGAAAATAGAAAAATTATTTCTAGTTTTTAATAATAAAAAATTCATGAAGTTTTTGGC
#Acropora digitifera, spicifera, muricata, hyacinthus
```

ACR larvae: Sample 1 `1_S912_R1_001` - Hits = Acropora! 

```
TCCTAGTATC+ATAGCTTGTG
GCTAGAATGATTACATGTTCAAAGTTAAAAATTCCATTTCACCCTTGTTATTGATAACAATGAAAAAAAAAAGTCAAGGGGCTTCACATTTGTTCAAATGTAAAGCGAGCTTGACAACCCAAAAAACAAATCCTCCTTCC
# Acropora loripes, hyacinthus, digitifera

TCCTAGTATC+ATAGCTTGTG
AGAGAATATGCTGTCATCAGCTTACGGAGACCTAATACACGATCCATTAATGGCTGATGGAGTGGAGCCTGTTGGATCGAAAGCATGGTTTACCAGAAACGGGATCAGGTGAGTTAACCACCTTCATACGTTCTACAATAGTTTTATTGCA
# Acropora spathulata, loripes, hyacinthus, palmata 

TCCTAGTATC+ATAGCTTGTG
CGGGCAAATTGGAAGGCTAACAAAGCAAAAGTGCGTGTTATATTGACTTAAGAAGATAAAACAAAGCGTGCAGTCAAAGCGAGGATGACAGAGGCGGCAGTATACGATCCTCCGCATTTCGCAGCCGAAGCTGTTGTGGCTGGGGGGTCTG
# Acropora hyacinthus, spicifera, loripes
```

POC recruit: Sample 11 `11_S132_R1_001` - Hits = Pocillopora! 

```
AAGACGTATG+CCAGATGCCA
GCTGGCTAGGCAACATTTAAAACCTTACACGAGAAACCGAAACAAAAACTACCTTTGGACTATATAACTTAAATCTATCAAATGCTACACCGTACAGGCCTACTCTCAACGAGATTAAACACAATTAAGTTATCAGAACATTAATAAAATG
#Pocillopora grandis, verrucosa, damicornis

AAGACGTATG+CCAGATGCCA
ATCCAGCTTAATAAAACAAAAGTTGTATTCTATGATAATTTCTGCCGTTCCAGTGTGCAGCTCAGTTACATTTAGACACCAAGCATGCAGTGTTGTTGGATTTTTTAAGAACACATAAGAAACAAGAAAAACACCACCTGCAGTTTTCCTC
#Pocillopora damicornis, verrucosa, grandis

AAGACGTATG+CCAGATGCCA
ACTGTGCTCAAGCATTCCTCAGGAGATTTTTATCCTCGGTAAAACGGAGATGTTCTGAATGAAACTTCAGCTGCTGTCATATAACTGTGAAGCTCCAATATGATGAGTTCATTGTCTCCTGTCCGTAACCATG
#Pocillopora damicornis, grandis, verrucosa 
```

POC larvae: Sample 10 `10_S921_R1_001` - Hits =  

```
ACCAGCATCT+CAAGGTATGT
GCACTAATTTCTTTTGTCATTTTTAAGCCATCGGTATACAATACAGGTAACAGCTTCTTCTCTTTCAGTTTCTCGAGTGTTTCCTTGTCATCCCGTAAGTCAAG
#Pocillopora damicornis, grandis, verrucosa

ACCAGCATCT+CAAGGTATGT
GCCAAGAAGCTGGAAATCTTAAATTAAATAAGTTTGTCTGAGAATGTCAGGATTAATATCCACAACTCTTGTTTAACCAAAGCTTGAGGAAACAGCTAAACTGAACGTTAGAGGTGCAAAGTTCTTCAATTCAGGAAGGTGTGGCGGCAAC
#Pocillopora damicornis, grandis 

ACCAGCATCT+CAAGGTATGT
CCTGAATTGTATTGGCCTTGCTTTCTTCGCTCCGTGATTGGTCGAGGCATTTTCTCAATAACCTCTCCACCAATCATATGCAGTTTTCCCGCACTTCAGGCAGGGAGTTTCCATTGGCTCCTTGAAACATTTTCTTCGGCTCTGATTGGCT
#Pocillopora verrucosa, damicornis, grandis 
```

# Submitting sequences to NCBI 

I first created an NCBI SRA BioProject and BioSample information in preparation for download and submission. I used the same steps outlined in my [previous notebook post for the Hawaii2023](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Mcapitata-Larval-Thermal-Tolerance-Project-NCBI-upload/) project and the [Putnam Lab SRA protocol](https://github.com/Putnam-Lab/Lab_Management/blob/master/Bioinformatics_%26_Coding/Data_Mangament/SRA-Upload_Protocol.md).

- Created new BioProject 
- Release date Oct 1 2026
- Title: Thermal sensitivity across coral larval and recruit life stages
- Description: Examination of thermal stress responses in two coral species across larval and recruit life stages collected in Moorea, French Polynesia in 2023
- Grants: ID: 2205966; OCE-PRF: Investigating ontogenetic shifts in microbe-derived nutrition in reef building corals; National Science Foundation
- Package: MIMS environmental/meta host-associated 

The BioProject information file [can be found here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/ncbi/MIMS.me.host-associated.6.0.xlsx). 

I then completed the SRA upload metadata file, which [can be found here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/ncbi/SRA_metadata.xlsx). 
 
Requested FTP upload folder.  

Create symlinks to the files I want to upload and follow NCBI FTP instructions.  

```
cd /gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences

mkdir ncbi_upload
cd ncbi_upload

ln -s /gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences/*fastq.gz /gscratch/srlab/ashuff/moorea-2023-rnaseq/raw-sequences/ncbi_upload

salloc --partition=ckpt-all --cpus-per-task=1 --mem=10G --time=2:00:00

sftp subftp@sftp-private.ncbi.nlm.nih.gov

#entered credentials

mkdir rnaseq_20251001

mput *
```

All information was added to the [Putnam Lab sequence inventory here](https://docs.google.com/spreadsheets/d/1qDGGpLFcmoO-fIFOPSUhPcxi4ErXIq2PkQbxvCCzI40/edit?gid=0#gid=0).  

254 Files were uploaded under SUB15678460.   

Sequencing inventory including all accession numbers is [available on GitHub here](https://github.com/AHuffmyer/moorea_symbiotic_exchange_2023/blob/main/data/rna/sequencing_inventory.xlsx).  


  