---
layout: post
title: COTS Project Bioinformatic Analysis
date: '2025-02-22'
categories: COTS-bioinformatics
tags: Bioinformatics GeneExpression 
---

This post details scripts for bioinformatics processing of COTS Project RNAseq data.    

# Lucy's RNAseq COTS data bioinformatics 

## Set up 

Made a folder in Unity at the following location: 

`/work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman`  

Made a `raw_data`, `scripts`, and `trimmed_data` folder.  

## Sym link data 

Sym link the raw data to the new location.  

```
ln -s /project/pi_hputnam_uri_edu/20250107_COTS_LG/*fastq.gz /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_data

```

## Run multiQC on raw data 

Create a script 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/scripts

nano raw_qc.sh

```

Make a script  

```
#!/bin/bash
#SBATCH --job-name=fastqc_raw
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --mem=250G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=24:00:00  # Job time limit
#SBATCH -o slurm-fastqc_raw.out  # %j = job ID
#SBATCH -e slurm-fastqc_raw.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_data

#load modules 
module load uri/main
module load fastqc/0.12.1
module load MultiQC/1.12-foss-2021b

#run fastqc on raw data
fastqc /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_data/*.fastq.gz -o /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_multiqc/

#generate multiqc report
multiqc /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_multiqc/ --filename multiqc_report_raw.html 

echo "Initial QC of raw seq data complete." $(date)
```

Run the job 

```
sbatch raw_qc.sh
```

Started at 15:00 on 2/20/2025

Completed after approx. 24 hours. 

Transfer raw multiQC report to desktop to view. 

I currently don't have access to a GitHub repo for this project, so I'll keep the files on my computer for now.  

In window outside Unity (logged onto my own computer): 

```
scp ashuffmyer_uri_edu@unity.rc.umass.edu:/work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_data/multiqc_report_raw.html /Users/ashuffmyer/MyProjects/COTS-Bioinformatics
```

We have some adapter content and lots of variation in GC histograms.  

## Trim raw sequences 

I made a folder called `trimmed_data` for the trimmed sequence files to go into.  

I will use the following settings for trimming in `fastp`. [Fastp documentation can be found here](https://github.com/OpenGene/fastp).   

- `detect_adapter_for_pe \`
	- This enables auto detection of adapters for paired end data 
- `qualified_quality_phred 30 \`
	- Filters reads based on phred score >=30
- `unqualified_percent_limit 10 \`
	- percents of bases are allowed to be unqualified, set here as 10% 
- `length_required 100 \`
	- Removes reads shorter than 100 bp. We have read lengths of all 150 bp, so this is not a stringent filtration.   
- `cut_right cut_right_window_size 5 cut_right_mean_quality 20`
	- Jill has used this sliding cut window in her script. I am going to leave it out for our trimming and we can re-evaluate the QC to see if we need to implement cutting. 

Write a script.  

```
cd scripts 
nano trimming.sh
```  

Search for fastp module. 

```
module --show_hidden spider fastp
```

Fastp module is `fastp/0.23.2-GCC-11.2.0` on Unity.  

```
#!/bin/bash
#SBATCH --job-name=trimming
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --mem=250G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=10:00:00  # Job time limit
#SBATCH -o slurm-trimming.out  # %j = job ID
#SBATCH -e slurm-trimming.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data

#load modules 
module load uri/main
module load fastqc/0.12.1
module load MultiQC/1.12-foss-2021b
module load fastp/0.23.2-GCC-11.2.0

# Make an array of sequences to trim in raw data directory 

cd /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_data/

array1=($(ls *R1_001.fastq.gz))

echo "Read trimming of adapters started." $(date)

# fastp and fastqc loop 
for i in ${array1[@]}; do
    fastp --in1 ${i} \
        --in2 $(echo ${i}|sed s/_R1/_R2/)\
        --out1 /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data/trim.${i} \
        --out2 //work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data/trim.$(echo ${i}|sed s/_R1/_R2/) \
        --detect_adapter_for_pe \
        --qualified_quality_phred 30 \
        --unqualified_percent_limit 10 \
        --length_required 100 

done

echo "Read trimming of adapters completed." $(date)

```

Run the script. 

```
sbatch trimming.sh
```

Job started at 14:00 on 22 February 2025.  

Job completed at 21:00 on 22 Feb 2025 (~7 hours).  

## MultiQC on trimmed sequences 

Run FastQC and generate a MultiQC report on the trimmed sequences.  

Make a script.  

```
mkdir trimmed_multiqc 

cd scripts 
nano trimmed_qc.sh
``` 

```
#!/bin/bash
#SBATCH --job-name=trimmed_qc
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --mem=250G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=10:00:00  # Job time limit
#SBATCH -o slurm-trim_qc.out  # %j = job ID
#SBATCH -e slurm-trim_qc.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_multiqc

#load modules 
module load uri/main
module load fastqc/0.12.1
module load MultiQC/1.12-foss-2021b

#run fastqc on trimmed data
fastqc /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data/*.fastq.gz -o /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_multiqc/

#generate multiqc report
multiqc /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_multiqc/ --filename multiqc_report_trimmed.html 

echo "Initial QC of trimmed seq data complete." $(date)

```

Run the script. 

```
sbatch trimmed_qc.sh
```

Job started at 21:00 on 22 Feb 2025.  
