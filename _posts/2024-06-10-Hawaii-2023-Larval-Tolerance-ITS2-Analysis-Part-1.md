---
layout: post
title: Hawaii 2023 Larval Tolerance project ITS2 Analysis Part 1
date: '2024-06-10'
categories: Larval_Symbiont_TPC_2023
tags: ITS2 Molecular Mcapitata Bioinformatics
---

This post details download and QC for the ITS2 analysis pipeline for 2023 Hawaii Larval Thermal Tolerance project. 

# Overview 

The preparation of these samples are detailed in posts for [PCR and prep](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/ITS2-amplicon-PCR-and-preparation-for-sequencing-20240326/), [bead clean up](https://github.com/JillAshey/JillAshey_Putnam_Lab_Notebook/blob/master/_posts/2024-05-15-ITS2-Bead-Cleanup-McapDT-POC.md), and [preparation for sequencing](https://github.com/JillAshey/JillAshey_Putnam_Lab_Notebook/blob/master/_posts/2024-05-20-ITS2-Samples-PlateMaps.md). 

Samples were sequenced on 2 x 300 bp sequencing on a MiSeq M00763 at the [URI RI-INBRE Molecular Informatics Core facility](https://web.uri.edu/riinbre/mic/mic-services-and-resources/mic-sequencing-services/). 

The plate maps and metadata were detailed in [Jill's notebook](https://github.com/JillAshey/JillAshey_Putnam_Lab_Notebook/blob/master/_posts/2024-05-20-ITS2-Samples-PlateMaps.md) and have been uploaded to the project [GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/data/its2).      

We will analyze data using SymPortal. The [wiki for SymPortal can be found here](https://github.com/reefgenomics/SymPortal_framework/wiki). SymPortal will perform all quality control filtering of the sequence data and convert the raw sequence data into database objects. There is no need for pre filtering or trimming.
 
## 1. Move sequence files from storage on Andromeda 

Log into Andromeda  

`cd /data/putnamlab/ashuffmyer`

Make a new directory  

`mkdir hawaii_2023_its2`

`cd hawaii_2023_its2` 

`mkdir raw-sequences`  

The directory I want the files to be in is now `/data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences`.  

Navigate to where the files are stored on Andromeda and show directory location.  

`/data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey`  

Copy files into my directory along with .md5 files from the original download.  

```
cd /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences

cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*md5 /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences

```

Now, the file `URI_download.md5` is in my directory.  

| Plate | Well | Sample ID | Sequencing ID | Volume (uL) | Type          | Project    |
|-------|------|-----------|---------------|-------------|---------------|------------|
| 1     | A1   | R55       | HP801         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | B1   | R56       | HP802         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | C1   | R57       | HP803         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | D1   | R58       | HP804         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | E1   | R59       | HP805         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | F1   | R60       | HP806         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | G1   | R61       | HP807         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | H1   | R62       | HP808         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | A2   | R63       | HP809         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | B2   | R64       | HP810         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | C2   | R65       | HP811         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | D2   | R66       | HP812         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | E2   | R67       | HP813         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | F2   | R68       | HP814         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | G2   | R69       | HP815         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | H2   | R70       | HP816         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | A3   | R71       | HP817         | 20          | ITS2 amplicon | A Huffmyer |
| 1     | B3   | R72       | HP818         | 20          | ITS2 amplicon | A Huffmyer |

Next, copy all files that have sequence ID HP810-HP818.  

```
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP801* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP802* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP803* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP804* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP805* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP806* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP807* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP808* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP809* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP810* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP811* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP812* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP813* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP814* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP815* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP816* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP817* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
cp /data/putnamlab/KITT/hputnam/20240603_ITS2_Ashey/*HP818* /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences
```

Generate a new md5 file.  

```
md5sum *.fastq.gz > checkmd5_20240610.md5

md5sum -c checkmd5_20240610.md5
```

Output was as follows:  

```
HP801_S1_L001_R1_001.fastq.gz: OK
HP801_S1_L001_R2_001.fastq.gz: OK
HP802_S13_L001_R1_001.fastq.gz: OK
HP802_S13_L001_R2_001.fastq.gz: OK
HP803_S25_L001_R1_001.fastq.gz: OK
HP803_S25_L001_R2_001.fastq.gz: OK
HP804_S37_L001_R1_001.fastq.gz: OK
HP804_S37_L001_R2_001.fastq.gz: OK
HP805_S49_L001_R1_001.fastq.gz: OK
HP805_S49_L001_R2_001.fastq.gz: OK
HP806_S61_L001_R1_001.fastq.gz: OK
HP806_S61_L001_R2_001.fastq.gz: OK
HP807_S73_L001_R1_001.fastq.gz: OK
HP807_S73_L001_R2_001.fastq.gz: OK
HP808_S85_L001_R1_001.fastq.gz: OK
HP808_S85_L001_R2_001.fastq.gz: OK
HP809_S2_L001_R1_001.fastq.gz: OK
HP809_S2_L001_R2_001.fastq.gz: OK
HP810_S14_L001_R1_001.fastq.gz: OK
HP810_S14_L001_R2_001.fastq.gz: OK
HP811_S26_L001_R1_001.fastq.gz: OK
HP811_S26_L001_R2_001.fastq.gz: OK
HP812_S38_L001_R1_001.fastq.gz: OK
HP812_S38_L001_R2_001.fastq.gz: OK
HP813_S50_L001_R1_001.fastq.gz: OK
HP813_S50_L001_R2_001.fastq.gz: OK
HP814_S62_L001_R1_001.fastq.gz: OK
HP814_S62_L001_R2_001.fastq.gz: OK
HP815_S74_L001_R1_001.fastq.gz: OK
HP815_S74_L001_R2_001.fastq.gz: OK
HP816_S86_L001_R1_001.fastq.gz: OK
HP816_S86_L001_R2_001.fastq.gz: OK
HP817_S3_L001_R1_001.fastq.gz: OK
HP817_S3_L001_R2_001.fastq.gz: OK
HP818_S15_L001_R1_001.fastq.gz: OK
HP818_S15_L001_R2_001.fastq.gz: OK
```

I then compared the checksums between the original download and this transfer.  

All values match.  

```
| sample                         | transfer                          | original                          | match |
|--------------------------------|-----------------------------------|-----------------------------------|-------|
| HP801_S1_L001_R1_001.fastq.gz  | 155fe1769c46c61cb44e941f477f5138  | 155fe1769c46c61cb44e941f477f5138  | TRUE  |
| HP801_S1_L001_R2_001.fastq.gz  | c6e77ed27b3ce14f3259f3fd5ec68a7a  | c6e77ed27b3ce14f3259f3fd5ec68a7a  | TRUE  |
| HP802_S13_L001_R1_001.fastq.gz | 54540f695e4fcb75e75d17a3812df244  | 54540f695e4fcb75e75d17a3812df244  | TRUE  |
| HP802_S13_L001_R2_001.fastq.gz | e48b14bc7a860eaa8a0f87996c228e99  | e48b14bc7a860eaa8a0f87996c228e99  | TRUE  |
| HP803_S25_L001_R1_001.fastq.gz | b2733dfae02af0b37fe33da110d5dc94  | b2733dfae02af0b37fe33da110d5dc94  | TRUE  |
| HP803_S25_L001_R2_001.fastq.gz | 6e4a04b4f6a34320254e034c07c151e3  | 6e4a04b4f6a34320254e034c07c151e3  | TRUE  |
| HP804_S37_L001_R1_001.fastq.gz | eeb8f912a37ff7960fc24ea54f1b5814  | eeb8f912a37ff7960fc24ea54f1b5814  | TRUE  |
| HP804_S37_L001_R2_001.fastq.gz | 48e3931dd08218037139794a7fa7946f  | 48e3931dd08218037139794a7fa7946f  | TRUE  |
| HP805_S49_L001_R1_001.fastq.gz | c4eb066550ceaaf319694afcb60c9085  | c4eb066550ceaaf319694afcb60c9085  | TRUE  |
| HP805_S49_L001_R2_001.fastq.gz | 21fbe1a83419571f87d4239001f78bed  | 21fbe1a83419571f87d4239001f78bed  | TRUE  |
| HP806_S61_L001_R1_001.fastq.gz | 8b3fc5db863500e5a9d5db1e923dc8df  | 8b3fc5db863500e5a9d5db1e923dc8df  | TRUE  |
| HP806_S61_L001_R2_001.fastq.gz | 53ea67cc508fa18e59cb14a23d5716b5  | 53ea67cc508fa18e59cb14a23d5716b5  | TRUE  |
| HP807_S73_L001_R1_001.fastq.gz | f1d5968f749ab5f043c90859d6644cf5  | f1d5968f749ab5f043c90859d6644cf5  | TRUE  |
| HP807_S73_L001_R2_001.fastq.gz | 78cc72eaf72c57350f364f1b74c724a4  | 78cc72eaf72c57350f364f1b74c724a4  | TRUE  |
| HP808_S85_L001_R1_001.fastq.gz | c358d7ff4fd35369dffdc5c6571e1c3a  | c358d7ff4fd35369dffdc5c6571e1c3a  | TRUE  |
| HP808_S85_L001_R2_001.fastq.gz | ab11a1f0f93585a957e4b44138fb1827  | ab11a1f0f93585a957e4b44138fb1827  | TRUE  |
| HP809_S2_L001_R1_001.fastq.gz  | 62ae0e1e58c036eb024efeeb0acf833b  | 62ae0e1e58c036eb024efeeb0acf833b  | TRUE  |
| HP809_S2_L001_R2_001.fastq.gz  | 799cc7eaef0e7469a578a28c06855ede  | 799cc7eaef0e7469a578a28c06855ede  | TRUE  |
| HP810_S14_L001_R1_001.fastq.gz | a124d8b34d7bf30d2a31fef416fb2810  | a124d8b34d7bf30d2a31fef416fb2810  | TRUE  |
| HP810_S14_L001_R2_001.fastq.gz | 25b464be803665601477d3a68d1f27c3  | 25b464be803665601477d3a68d1f27c3  | TRUE  |
| HP811_S26_L001_R1_001.fastq.gz | ba32acae384a652aa183cef2d4f48836  | ba32acae384a652aa183cef2d4f48836  | TRUE  |
| HP811_S26_L001_R2_001.fastq.gz | 27ab28dcadca2620cb804c668b0fa59a  | 27ab28dcadca2620cb804c668b0fa59a  | TRUE  |
| HP812_S38_L001_R1_001.fastq.gz | 2a3e366c5d5213e6097b8f027ba9a944  | 2a3e366c5d5213e6097b8f027ba9a944  | TRUE  |
| HP812_S38_L001_R2_001.fastq.gz | c5d2c4980d2c59b8dda2ee7209439c48  | c5d2c4980d2c59b8dda2ee7209439c48  | TRUE  |
| HP813_S50_L001_R1_001.fastq.gz | ee918481267951be50e7cbada194f7e1  | ee918481267951be50e7cbada194f7e1  | TRUE  |
| HP813_S50_L001_R2_001.fastq.gz | d8c0aabe67e0745d67e52814a0c5a035  | d8c0aabe67e0745d67e52814a0c5a035  | TRUE  |
| HP814_S62_L001_R1_001.fastq.gz | 40e99e87485b650ca25660cbdeb44e87  | 40e99e87485b650ca25660cbdeb44e87  | TRUE  |
| HP814_S62_L001_R2_001.fastq.gz | 150603951008ef7d831069cd38d8bc75  | 150603951008ef7d831069cd38d8bc75  | TRUE  |
| HP815_S74_L001_R1_001.fastq.gz | d2b55ec408e2eeea3fb9aa93262587fa  | d2b55ec408e2eeea3fb9aa93262587fa  | TRUE  |
| HP815_S74_L001_R2_001.fastq.gz | f790fc8ab4bfc50fa716d53c95c77191  | f790fc8ab4bfc50fa716d53c95c77191  | TRUE  |
| HP816_S86_L001_R1_001.fastq.gz | 28b2c59d33942b1795156f70258ce89c  | 28b2c59d33942b1795156f70258ce89c  | TRUE  |
| HP816_S86_L001_R2_001.fastq.gz | 52ae315a36c5afde7cb88192f5976fbb  | 52ae315a36c5afde7cb88192f5976fbb  | TRUE  |
| HP817_S3_L001_R1_001.fastq.gz  | da59bc2cb23d998246d397708f045ec6  | da59bc2cb23d998246d397708f045ec6  | TRUE  |
| HP817_S3_L001_R2_001.fastq.gz  | 441c4daed60182ee7c15c6779fc0e4ab  | 441c4daed60182ee7c15c6779fc0e4ab  | TRUE  |
| HP818_S15_L001_R1_001.fastq.gz | 79c5722cd6e797af0ed840e4e4576fb7  | 79c5722cd6e797af0ed840e4e4576fb7  | TRUE  |
| HP818_S15_L001_R2_001.fastq.gz | 0df639fb9362f29484040d51bf4917e3  | 0df639fb9362f29484040d51bf4917e3  | TRUE  |
```

Files are now ready for QC.  

## 2. Run FastQC and MultiQC on raw sequence files  

Write a script to conduct QC.  

```
cd /data/putnamlab/ashuffmyer/hawaii_2023_its2/
mkdir scripts
cd scripts 

nano raw-qc.sh
```

```
#!/bin/bash
#SBATCH -t 120:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mem=100GB
#SBATCH --account=putnamlab
#SBATCH --export=NONE
#SBATCH --output="qc_raw-%j.out"
#SBATCH --error="qc_raw-%j.err"

# load modules needed
module load fastp/0.19.7-foss-2018b
module load FastQC/0.11.8-Java-1.8
module load MultiQC/1.9-intel-2020a-Python-3.8.2

# fastqc of raw reads
fastqc /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences/*.fastq.gz

#generate multiqc report
multiqc /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences --filename /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences/multiqc_report_raw.html 

echo "Raw MultiQC report generated." $(date)
```

`sbatch raw-qc.sh`  

Job 320310 started at 10:20am on 10 June 2024 and finished after about 10 minutes.  

Move the multiQC file to my computer. This file and all individual QC files are on [GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/data/its2/raw_QC).  Multi  

```
scp ashuffmyer@ssh3.hac.uri.edu:/data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences/multiqc_report_raw.html ~/MyProjects/larval_symbiont_TPC/data/its2/raw_QC

scp ashuffmyer@ssh3.hac.uri.edu:/data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences/\*fastqc.html ~/MyProjects/larval_symbiont_TPC/data/its2/raw_QC

```

View the results in the MultiQC file.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_adapter_content_plot.png?raw=true)  

There is low adapter content, but not zero adapter content.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_overrepresented_sequencesi_plot.png?raw=true)  

There are many overrepresented sequences, which we expect from ITS2 in this species. We expect a dominance of few symbiont sequences.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_per_base_n_content_plot.png?raw=true)  

There is a high percentage of N content in the start of the sequences. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_per_base_sequence_quality_plot.png?raw=true)  

There are quality issues, particularly at the start and end of the sequences.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_per_sequence_gc_content_plot.png?raw=true)  

There is a bit of a weird distribution in GC content. We have seen this [in past QC of ITS2 data](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Mcapitata-Development-ITS2-Analysis-Part-1/).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_per_sequence_quality_scores_plot.png?raw=true)  

There are many sequences with low quality scores.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_sequence_counts_plot.png?raw=true)  

There are many duplicate reads, as we expect.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_sequence_duplication_levels_plot.png?raw=true)  

There is high duplication of sequences, again as we expect.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/its2/raw_qc/fastqc_sequence_length_distribution_plot.png?raw=true)  

There are many small sequences (<60 bp). There are also many sequences at about 300 bp, which is our sequencing length (2x300 bp).  

We will next work on SymPortal submission of these raw files. 









