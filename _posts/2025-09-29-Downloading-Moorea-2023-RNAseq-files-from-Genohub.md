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

Submitted job 29916491 at 17:00 on Sep 30. 

I'll continue to update this post as I progress with analysis. 

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
 
I'll finalize the submission in the next couple of days and will update this post.  
