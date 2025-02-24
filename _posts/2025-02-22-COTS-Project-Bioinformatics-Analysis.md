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

The file can be [viewed on GitHub here](https://github.com/lmgorman/CoTS-RNAseq/blob/main/output/seq_qc/multiqc_report_raw.html).  

In window outside Unity (logged onto my own computer): 

```
scp ashuffmyer_uri_edu@unity.rc.umass.edu:/work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/raw_data/multiqc_report_raw.html /Users/ashuffmyer/MyProjects/CoTS-RNAseq/output/seq_qc
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
#SBATCH --time=24:00:00  # Job time limit
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

Job started at 11:15 on 23 Feb 2025, ended at about 24 h.  

Copy file to computer. 

```
scp ashuffmyer_uri_edu@unity.rc.umass.edu:/work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_multiqc/multiqc_report_trimmed.html /Users/ashuffmyer/MyProjects/CoTS-RNAseq/output/seq_qc

```

Adapters are removed, quality scores are high. QC content is weird, but likely due to symbiont and microbial reads seen in Hollie's tests of alignment previously. We will see what happens at the alignment steps.  

The file can be [viewed on GitHub here](https://github.com/lmgorman/CoTS-RNAseq/blob/main/output/seq_qc/multiqc_report_trimmed.html). 

## Make species specific lists of samples 

Make a list of Acropora and Porites files. 

```
nano acr-list.txt

497R_R1_001.fastq.gz
568R_R1_001.fastq.gz
410R_R1_001.fastq.gz
549R_R1_001.fastq.gz
414R_R1_001.fastq.gz
321RA_R1_001.fastq.gz
581R_R1_001.fastq.gz
370R_R1_001.fastq.gz
419R_R1_001.fastq.gz
331RA_R1_001.fastq.gz
336R_R1_001.fastq.gz
512R_R1_001.fastq.gz
380R_R1_001.fastq.gz
571R_R1_001.fastq.gz
586R_R1_001.fastq.gz
468R_R1_001.fastq.gz
497R_R2_001.fastq.gz
568R_R2_001.fastq.gz
410R_R2_001.fastq.gz
549R_R2_001.fastq.gz
414R_R2_001.fastq.gz
321RA_R2_001.fastq.gz
581R_R2_001.fastq.gz
370R_R2_001.fastq.gz
419R_R2_001.fastq.gz
331RA_R2_001.fastq.gz
336R_R2_001.fastq.gz
512R_R2_001.fastq.gz
380R_R2_001.fastq.gz
571R_R2_001.fastq.gz
586R_R2_001.fastq.gz
468R_R2_001.fastq.gz

```

```
nano por-list.txt

225R_R1_001.fastq.gz
235R_R1_001.fastq.gz
43R_R1_001.fastq.gz
211R_R1_001.fastq.gz
218R_R1_001.fastq.gz
34R_R1_001.fastq.gz
227R_R1_001.fastq.gz
16R_R1_001.fastq.gz
61R_R1_001.fastq.gz
86R_R1_001.fastq.gz
236R_R1_001.fastq.gz
244R_R1_001.fastq.gz
82R_R1_001.fastq.gz
71R_R1_001.fastq.gz
253R_R1_001.fastq.gz
76R_R1_001.fastq.gz
225R_R2_001.fastq.gz
235R_R2_001.fastq.gz
43R_R2_001.fastq.gz
211R_R2_001.fastq.gz
218R_R2_001.fastq.gz
34R_R2_001.fastq.gz
227R_R2_001.fastq.gz
16R_R2_001.fastq.gz
61R_R2_001.fastq.gz
86R_R2_001.fastq.gz
236R_R2_001.fastq.gz
244R_R2_001.fastq.gz
82R_R2_001.fastq.gz
71R_R2_001.fastq.gz
253R_R2_001.fastq.gz
76R_R2_001.fastq.gz
```

Make a folder for Acropora and Porites files.  

```
mkdir acr
mkdir por
```

Sym link Acropora files to the `acr` folder.  

```
while IFS= read -r filename; do
    trimmed_file="trimmed_data/trim.$filename"
    if [[ -f "$trimmed_file" ]]; then
        ln -s "$(realpath "$trimmed_file")" "acr/trim.$filename"
    else
        echo "Warning: $trimmed_file not found" >&2
    fi
done < acr-list.txt
```

Sym link Porites files to the `por` folder.  

```
while IFS= read -r filename; do
    trimmed_file="trimmed_data/trim.$filename"
    if [[ -f "$trimmed_file" ]]; then
        ln -s "$(realpath "$trimmed_file")" "por/trim.$filename"
    else
        echo "Warning: $trimmed_file not found" >&2
    fi
done < por-list.txt
```

## Alignment to genomes 

### Index genomes 

Copy genome fasta files to my directory to prevent permission issues. 

```
mkdir refs 
mkdir por
mkdir acr

cp /work/pi_hputnam_uri_edu/refs/Plutea_genome/plut_final_2.1.fasta /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por

cp /work/pi_hputnam_uri_edu/refs/Plutea_genome/plut2v1.1.genes.gff3 /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por

cp /work/pi_hputnam_uri_edu/refs/Ahyacinthus_genome/Ahyacinthus_genome_V1/Ahyacinthus.chrsV1.fasta /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/acr

cp /work/pi_hputnam_uri_edu/refs/Ahyacinthus_genome/Ahyacinthus_genome_V1/Ahyacinthus.coding.gff3 /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/acr
```

Index Porites genome 

```
cd scripts 
nano por-genome.sh

#!/bin/bash
#SBATCH --job-name=STAR_genome_index_Plutea
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --mem=100G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=24:00:00  # Job time limit
#SBATCH -o slurm-por-genome.out  # %j = job ID
#SBATCH -e slurm-por-genome.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por

#load modules
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

STAR --runThreadN 6 \
--runMode genomeGenerate \
--genomeDir Plutea_index \
--genomeFastaFiles /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por/plut_final_2.1.fasta \
--sjdbGTFfile /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por/plut2v1.1.genes.gff3 \
--sjdbGTFtagExonParentTranscript Parent \
--sjdbOverhang 99 \
--genomeSAindexNbases 13

sbatch por-genome.sh
```

Index Acropora genome 

```
cd scripts 
nano acr-genome.sh

#!/bin/bash
#SBATCH --job-name=STAR_genome_index_Ahya
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --mem=100G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=24:00:00  # Job time limit
#SBATCH -o slurm-acr-genome.out  # %j = job ID
#SBATCH -e slurm-acr-genome.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/acr

module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

STAR --runThreadN 6 \
--runMode genomeGenerate \
--genomeDir Ahya_index \
--genomeFastaFiles /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/acr/Ahyacinthus.chrsV1.fasta \
--sjdbGTFfile /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/acr/Ahyacinthus.coding.gff3 \
--sjdbGTFtagExonParentTranscript Parent \
--sjdbOverhang 99

sbatch acr-genome.sh
```

Jobs started at 12:00 24 February 2025. Jobs finished after about 10-30 min.  

### Acropora 

Align all ACR files in the relevant folder to generated index files.  

``` 
cd scripts 
nano acr-align.sh
```

```
#!/bin/bash
#SBATCH --job-name=acr-align
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --mem=250G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=24:00:00  # Job time limit
#SBATCH -o slurm-acr-align.out  # %j = job ID
#SBATCH -e slurm-acr-align.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/acr

#load modules
echo "Loading programs" $(date)
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

echo "Starting read alignment." $(date)
#loop through all files to align them to genome
for i in /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/acr/*_R1_001.fastq.gz; do

# Define the corresponding R2 file by replacing _R1_ with _R2_
    r2_file="${i/_R1_001.fastq.gz/_R2_001.fastq.gz}"
    
    # Run STAR alignment
    STAR --runMode alignReads \
        --genomeDir /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/acr/Ahya_index \
        --runThreadN 10 \
        --readFilesCommand zcat \
        --readFilesIn "$i" "$r2_file" \
        --outSAMtype BAM SortedByCoordinate \
        --outSAMunmapped Within \
        --outSAMattributes Standard \
        --genomeSAindexNbases 13 \
        --outFileNamePrefix "${i%.fastq.gz}"
done

echo "Alignment of Trimmed Seq data complete." $(date)

```   

```
sbatch acr-align.sh
```


### Porites 

Align all POR files in the relevant folder using generated genome index.    

``` 
cd scripts 
nano por-align.sh
```

```
#!/bin/bash
#SBATCH --job-name=por-align
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --mem=250G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=24:00:00  # Job time limit
#SBATCH -o slurm-por-align.out  # %j = job ID
#SBATCH -e slurm-por-align.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por

#load modules
echo "Loading programs" $(date)
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

echo "Starting read alignment." $(date)
#loop through all files to align them to genome
for i in /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por/*_R1_001.fastq.gz; do

# Define the corresponding R2 file by replacing _R1_ with _R2_
    r2_file="${i/_R1_001.fastq.gz/_R2_001.fastq.gz}"
    
    # Run STAR alignment
    STAR --runMode alignReads \
        --genomeDir /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por/Plutea_index \
        --runThreadN 10 \
        --readFilesCommand zcat \
        --readFilesIn "$i" "$r2_file" \
        --outSAMtype BAM SortedByCoordinate \
        --outSAMunmapped Within \
        --outSAMattributes Standard \
        --outFileNamePrefix "${i%.fastq.gz}"
done

echo "Alignment of Trimmed Seq data complete." $(date)

```   

```
sbatch por-align
```

Both jobs started on 24 Feb 2025 at 14:00.  