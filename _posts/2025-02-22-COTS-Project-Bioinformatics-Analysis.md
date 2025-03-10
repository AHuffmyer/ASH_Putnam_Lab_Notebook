---
layout: post
title: COTS Project Bioinformatic Analysis
date: '2025-03-07'
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

Start a scratch directory to temporarily store the output .bam files. Call the directory `cots` and use for 30 days. It was created on Feb 25th, so we should be prepared to extend or move files we need to keep by March 25th ish.     

```
ws_allocate cots 30

Info: creating workspace.
/scratch3/workspace/ashuffmyer_uri_edu-cots
remaining extensions  : 5
remaining time in days: 30
```

Align all ACR files in the relevant folder to generated index files.  

``` 
cd scripts 
nano acr-align.sh
```

```
#!/bin/bash
#SBATCH --job-name=acr-align
#SBATCH --nodes=1 --cpus-per-task=15
#SBATCH --mem=200G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 7-24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH -o slurm-acr-align.out  # %j = job ID
#SBATCH -e slurm-acr-align.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/acr/

#load modules
echo "Loading programs" $(date)
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

echo "Starting read alignment." $(date)
#loop through all files to align them to genome
for i in /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/acr/*_R1_001.fastq.gz; do

# Define the corresponding R2 file by replacing _R1_ with _R2_
    r2_file="${i/_R1_001.fastq.gz/_R2_001.fastq.gz}"
    
# Define output prefix
    output_prefix="/scratch3/workspace/ashuffmyer_uri_edu-cots/acr/$(basename "${i%_R1_001.fastq.gz}")_"
    
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
        --outFileNamePrefix "$output_prefix"
done

echo "Alignment of Trimmed Seq data complete." $(date)

```   

```
sbatch acr-align.sh
```

This script outputs files in the scratch `acr` directory that I made.  

### Porites 

Align all POR files in the relevant folder using generated genome index.    

``` 
cd scripts 
nano por-align.sh
```

```
#!/bin/bash
#SBATCH --job-name=por-align
#SBATCH --nodes=1 --cpus-per-task=15
#SBATCH --mem=200G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 7-24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH -o slurm-por-align.out  # %j = job ID
#SBATCH -e slurm-por-align.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por/

#load modules
echo "Loading programs" $(date)
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

echo "Starting read alignment." $(date)
#loop through all files to align them to genome
for i in /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por/*_R1_001.fastq.gz; do

# Define the corresponding R2 file by replacing _R1_ with _R2_
    r2_file="${i/_R1_001.fastq.gz/_R2_001.fastq.gz}"
    
# Define output prefix
    output_prefix="/scratch3/workspace/ashuffmyer_uri_edu-cots/por/$(basename "${i%_R1_001.fastq.gz}")_"
    
# Run STAR alignment
    STAR --runMode alignReads \
        --genomeDir /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por/Plutea_index \
        --runThreadN 10 \
        --readFilesCommand zcat \
        --readFilesIn "$i" "$r2_file" \
        --outSAMtype BAM SortedByCoordinate \
        --outSAMunmapped Within \
        --outSAMattributes Standard \
        --outFileNamePrefix "$output_prefix"
done

echo "Alignment of Trimmed Seq data complete." $(date)

```   

```
sbatch por-align
```

This script outputs files in the scratch `por` directory that I made.  

Both jobs started on 27 Feb 2025 at 07:00. Jobs finished on 1 March.    

## Look at mapping results

Obtain mapping percentages after job was completed. Use the `samtools flagstat` function that does a full pass through the input file to calculate and print statistics to stdout. Provides counts for each of 13 categories based primarily on bit flags in the FLAG field. Each category in the output is broken down into QC pass and QC fail, which is presented as "#PASS + #FAIL" followed by a description of the category.

### Porites  

Write a script to generate alignment stats.  

```
cd scripts
nano por-stats.sh
```

```
#!/bin/bash
#SBATCH --job-name=por-stats
#SBATCH --nodes=1 --cpus-per-task=15
#SBATCH --mem=100G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 24:00:00
#SBATCH -o slurm-por-stats.out  # %j = job ID
#SBATCH -e slurm-por-stats.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por/

module load uri/main
module load SAMtools/1.18-GCC-12.3.0

cd /scratch3/workspace/ashuffmyer_uri_edu-cots/por/

# Define output file
output_file="por_alignment_stats.txt"

# Clear the output file if it exists
> "$output_file"

for i in *.bam; do
    echo "${i}" >> "$output_file"
    samtools flagstat "${i}" | grep "mapped (" >> "$output_file"
done
```

```
sbatch por-stats.sh
```

Move file to user directory 

```
cp /scratch3/workspace/ashuffmyer_uri_edu-cots/por/por_alignment_stats.txt /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por/
```

The output looks like this:  

```
trim.16R_Aligned.sortedByCoord.out.bam
3797576 + 0 mapped (2.36% : N/A)
3314780 + 0 primary mapped (2.07% : N/A)
trim.211R_Aligned.sortedByCoord.out.bam
500889612 + 0 mapped (48.39% : N/A)
470004384 + 0 primary mapped (46.80% : N/A)
trim.218R_Aligned.sortedByCoord.out.bam
41218742 + 0 mapped (30.63% : N/A)
39657624 + 0 primary mapped (29.82% : N/A)
trim.225R_Aligned.sortedByCoord.out.bam
866866 + 0 mapped (1.33% : N/A)
753202 + 0 primary mapped (1.15% : N/A)
trim.227R_Aligned.sortedByCoord.out.bam
94051372 + 0 mapped (56.82% : N/A)
87824202 + 0 primary mapped (55.13% : N/A)
trim.235R_Aligned.sortedByCoord.out.bam
40775646 + 0 mapped (26.80% : N/A)
38140810 + 0 primary mapped (25.51% : N/A)
trim.236R_Aligned.sortedByCoord.out.bam
57274150 + 0 mapped (34.73% : N/A)
52577952 + 0 primary mapped (32.82% : N/A)
trim.244R_Aligned.sortedByCoord.out.bam
42584786 + 0 mapped (29.85% : N/A)
40319002 + 0 primary mapped (28.72% : N/A)
trim.253R_Aligned.sortedByCoord.out.bam
58346292 + 0 mapped (38.89% : N/A)
54584348 + 0 primary mapped (37.31% : N/A)
trim.34R_Aligned.sortedByCoord.out.bam
510168 + 0 mapped (0.68% : N/A)
456620 + 0 primary mapped (0.61% : N/A)
trim.43R_Aligned.sortedByCoord.out.bam
2473270 + 0 mapped (1.97% : N/A)
2222166 + 0 primary mapped (1.77% : N/A)
trim.61R_Aligned.sortedByCoord.out.bam
48904270 + 0 mapped (37.72% : N/A)
45761608 + 0 primary mapped (36.17% : N/A)
trim.71R_Aligned.sortedByCoord.out.bam
39363360 + 0 mapped (28.30% : N/A)
36603192 + 0 primary mapped (26.84% : N/A)
trim.76R_Aligned.sortedByCoord.out.bam
49636416 + 0 mapped (32.19% : N/A)
45848520 + 0 primary mapped (30.48% : N/A)
trim.82R_Aligned.sortedByCoord.out.bam
39928236 + 0 mapped (29.10% : N/A)
36333724 + 0 primary mapped (27.19% : N/A)
trim.86R_Aligned.sortedByCoord.out.bam
40120754 + 0 mapped (31.95% : N/A)
38270032 + 0 primary mapped (30.93% : N/A)
```
There is a wide range in mapping. There is a group of samples (n=5) with mapping of 0.5-3%. There are 12 samples mapping at 25% or higher. We need to see if the samples with low mapping are from the "eaten" treatment. We will also compare mapping to the *P. evermanni* genome next.  

### Acropora  

```
cd scripts
nano acr-stats.sh
```

```
#!/bin/bash
#SBATCH --job-name=acr-stats
#SBATCH --nodes=1 --cpus-per-task=15
#SBATCH --mem=100G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 24:00:00
#SBATCH -o slurm-acr-stats.out  # %j = job ID
#SBATCH -e slurm-acr-stats.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/acr/

module load uri/main
module load SAMtools/1.18-GCC-12.3.0

cd /scratch3/workspace/ashuffmyer_uri_edu-cots/acr/

# Define output file
output_file="acr_alignment_stats.txt"

# Clear the output file if it exists
> "$output_file"

for i in *.bam; do
    echo "${i}" >> "$output_file"
    samtools flagstat "${i}" | grep "mapped (" >> "$output_file"
done
```

```
sbatch acr-stats.sh
```

Move file to user directory 

```
cp /scratch3/workspace/ashuffmyer_uri_edu-cots/acr/acr_alignment_stats.txt /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/acr/
```

The output looks like this:  

```
trim.321RA_Aligned.sortedByCoord.out.bam
101145264 + 0 mapped (75.26% : N/A)
91192744 + 0 primary mapped (73.29% : N/A)
trim.331RA_Aligned.sortedByCoord.out.bam
99763802 + 0 mapped (75.29% : N/A)
88927416 + 0 primary mapped (73.09% : N/A)
trim.336R_Aligned.sortedByCoord.out.bam
110716976 + 0 mapped (61.04% : N/A)
100464150 + 0 primary mapped (58.71% : N/A)
trim.370R_Aligned.sortedByCoord.out.bam
34239394 + 0 mapped (24.31% : N/A)
30624808 + 0 primary mapped (22.32% : N/A)
trim.380R_Aligned.sortedByCoord.out.bam
99627838 + 0 mapped (74.16% : N/A)
89986620 + 0 primary mapped (72.16% : N/A)
trim.410R_Aligned.sortedByCoord.out.bam
80526182 + 0 mapped (46.17% : N/A)
73098402 + 0 primary mapped (43.77% : N/A)
trim.414R_Aligned.sortedByCoord.out.bam
87333162 + 0 mapped (38.67% : N/A)
78162862 + 0 primary mapped (36.07% : N/A)
trim.419R_Aligned.sortedByCoord.out.bam
106151016 + 0 mapped (68.00% : N/A)
94939130 + 0 primary mapped (65.52% : N/A)
trim.468R_Aligned.sortedByCoord.out.bam
100108574 + 0 mapped (67.37% : N/A)
90603050 + 0 primary mapped (65.14% : N/A)
trim.497R_Aligned.sortedByCoord.out.bam
3090986 + 0 mapped (2.22% : N/A)
2760422 + 0 primary mapped (1.98% : N/A)
trim.512R_Aligned.sortedByCoord.out.bam
134343958 + 0 mapped (74.83% : N/A)
121579012 + 0 primary mapped (72.91% : N/A)
trim.549R_Aligned.sortedByCoord.out.bam
77332112 + 0 mapped (66.06% : N/A)
69416060 + 0 primary mapped (63.60% : N/A)
trim.568R_Aligned.sortedByCoord.out.bam
74142102 + 0 mapped (53.01% : N/A)
66507248 + 0 primary mapped (50.29% : N/A)
trim.571R_Aligned.sortedByCoord.out.bam
76962776 + 0 mapped (47.52% : N/A)
69663792 + 0 primary mapped (45.04% : N/A)
trim.581R_Aligned.sortedByCoord.out.bam
23750278 + 0 mapped (19.23% : N/A)
21582796 + 0 primary mapped (17.78% : N/A)
trim.586R_Aligned.sortedByCoord.out.bam
87532254 + 0 mapped (68.31% : N/A)
79263634 + 0 primary mapped (66.12% : N/A)

```

There is a pretty wide range in mapping for Acropora as well, but the lowest value is 17%.  

## Run alignment of POR files to the P. lobata/lutea genome 

### Index the genome 

Download the files using the script written by Lucy.    

```
cd refs
mkdir por-ever 
cd por-ever

wget https://www.genoscope.cns.fr/corals/data/Porites_evermanni_v1.annot.gff

wget https://www.genoscope.cns.fr/corals/data/Porites_evermanni_v1.fa
```

Now available as `Porites_evermanni_v1.annot.fa` and `Porites_evermanni_v1.annot.gff` in the `refs/por-ever` folder.  

```
cd scripts 
nano por-ever-genome.sh
```

```
module --show_hidden spider gffread
```

```
#!/bin/bash
#SBATCH --job-name=Pevermanni_index
#SBATCH --nodes=1 --cpus-per-task=10
#SBATCH --mem=100G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --time=24:00:00  # Job time limit
#SBATCH -o slurm-por-ever-genome.out  # %j = job ID
#SBATCH -e slurm-por-ever-genome.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por-ever

#load modules
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0
module load gffread/0.12.7

#convert gff to gtf 
gffread Porites_evermanni_v1.annot.gff -T -o Porites_evermanni_v1.annot.gtf

STAR --runThreadN 6 \
--runMode genomeGenerate \
--genomeDir Pevermanni_index \
--genomeFastaFiles /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por-ever/Porites_evermanni_v1.fa \
--sjdbGTFfile /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por-ever/Porites_evermanni_v1.annot.gtf \
--sjdbGTFtagExonParentTranscript Parent \
--sjdbOverhang 99 \
--genomeSAindexNbases 13
```

```
sbatch por-ever-genome.sh
```

Submitted job on 3 March at 08:00. Finished in about 5 min.   


### Run alignment to the genome to compare alignment rates with P. evermanni genome 

```
cd cots-gorman 
mkdir por-ever 

cd /scratch3/workspace/ashuffmyer_uri_edu-cots/
mkdir por-ever 
```

Align all POR files in the relevant folder using generated P. evermanni genome index.    

``` 
cd scripts 
nano por-ever-align.sh
```

```
#!/bin/bash
#SBATCH --job-name=por-ever-align
#SBATCH --nodes=1 --cpus-per-task=15
#SBATCH --mem=200G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 7-24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH -o slurm-por-ever-align.out  # %j = job ID
#SBATCH -e slurm-por-ever-align.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por-ever/

#load modules
echo "Loading programs" $(date)
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

echo "Starting read alignment." $(date)
#loop through all files to align them to genome
for i in /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por/*_R1_001.fastq.gz; do

# Define the corresponding R2 file by replacing _R1_ with _R2_
    r2_file="${i/_R1_001.fastq.gz/_R2_001.fastq.gz}"
    
# Define output prefix
    output_prefix="/scratch3/workspace/ashuffmyer_uri_edu-cots/por-ever/$(basename "${i%_R1_001.fastq.gz}")_"
    
# Run STAR alignment
    STAR --runMode alignReads \
        --genomeDir /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por-ever/Pevermanni_index \
        --runThreadN 10 \
        --readFilesCommand zcat \
        --readFilesIn "$i" "$r2_file" \
        --outSAMtype BAM SortedByCoordinate \
        --outSAMunmapped Within \
        --outSAMattributes Standard \
        --outFileNamePrefix "$output_prefix"
done

echo "Alignment of Trimmed Seq data complete." $(date)

```   

```
sbatch por-ever-align.sh
```

This script outputs files in the scratch `por-ever` directory that I made.  

Submitted job at 08:00 on March 3, finished on March 4.  


Write a script to generate alignment stats.  

```
cd scripts
nano por-ever-stats.sh
```

```
#!/bin/bash
#SBATCH --job-name=por-ever-stats
#SBATCH --nodes=1 --cpus-per-task=15
#SBATCH --mem=100G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 24:00:00
#SBATCH -o slurm-por-ever-stats.out  # %j = job ID
#SBATCH -e slurm-por-ever-stats.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por-ever/

module load uri/main
module load SAMtools/1.18-GCC-12.3.0

cd /scratch3/workspace/ashuffmyer_uri_edu-cots/por-ever/

# Define output file
output_file="por_ever_alignment_stats.txt"

# Clear the output file if it exists
> "$output_file"

for i in *.bam; do
    echo "${i}" >> "$output_file"
    samtools flagstat "${i}" | grep "mapped (" >> "$output_file"
done
```

```
sbatch por-ever-stats.sh
```

Submitted job on March 4 at 19:00.  

Move file to user directory 

```
cp /scratch3/workspace/ashuffmyer_uri_edu-cots/por-ever/por_ever_alignment_stats.txt /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por-ever/
```

The output looks like this: 

```
trim.16R_Aligned.sortedByCoord.out.bam
9932412 + 0 mapped (6.14% : N/A)
8301790 + 0 primary mapped (5.18% : N/A)
trim.211R_Aligned.sortedByCoord.out.bam
trim.218R_Aligned.sortedByCoord.out.bam
183856720 + 0 mapped (96.73% : N/A)
126776572 + 0 primary mapped (95.33% : N/A)
trim.225R_Aligned.sortedByCoord.out.bam
2907570 + 0 mapped (4.43% : N/A)
2534398 + 0 primary mapped (3.88% : N/A)
trim.227R_Aligned.sortedByCoord.out.bam
171417626 + 0 mapped (84.19% : N/A)
127096866 + 0 primary mapped (79.79% : N/A)
trim.235R_Aligned.sortedByCoord.out.bam
211878608 + 0 mapped (97.26% : N/A)
143543886 + 0 primary mapped (96.01% : N/A)
trim.236R_Aligned.sortedByCoord.out.bam
212095574 + 0 mapped (96.43% : N/A)
152349478 + 0 primary mapped (95.10% : N/A)
trim.244R_Aligned.sortedByCoord.out.bam
188686986 + 0 mapped (94.76% : N/A)
129969490 + 0 primary mapped (92.57% : N/A)
trim.253R_Aligned.sortedByCoord.out.bam
195777140 + 0 mapped (97.66% : N/A)
141597122 + 0 primary mapped (96.80% : N/A)
trim.34R_Aligned.sortedByCoord.out.bam
1022532 + 0 mapped (1.37% : N/A)
783544 + 0 primary mapped (1.05% : N/A)
trim.43R_Aligned.sortedByCoord.out.bam
12644158 + 0 mapped (9.78% : N/A)
8762710 + 0 primary mapped (6.99% : N/A)
trim.61R_Aligned.sortedByCoord.out.bam
156888590 + 0 mapped (91.65% : N/A)
112220878 + 0 primary mapped (88.70% : N/A)
trim.71R_Aligned.sortedByCoord.out.bam
190552052 + 0 mapped (95.40% : N/A)
127166612 + 0 primary mapped (93.26% : N/A)
trim.76R_Aligned.sortedByCoord.out.bam
207617046 + 0 mapped (95.93% : N/A)
141609686 + 0 primary mapped (94.14% : N/A)
trim.82R_Aligned.sortedByCoord.out.bam
198766054 + 0 mapped (98.85% : N/A)
131310576 + 0 primary mapped (98.26% : N/A)
trim.86R_Aligned.sortedByCoord.out.bam
172716586 + 0 mapped (97.33% : N/A)
119000396 + 0 primary mapped (96.17% : N/A)
```

Next, we need to compare alignment to *P. evermanni* vs *P. lutea* for each Porites sample. Sample 211R did not map, so we need to subsample this file and then re run.  

## Subsample 211R  

We need to make a script to: 

1. Subset sequences from trimmed file for sample 211 
2. Re run alignment for 211 trimmed files 
3. Generate stats for all samples again 

### Write a script to subsample 211 trimmed data file 

```
cd scripts

nano seqtk_211R.sh
```

Write the script 

```
#!/bin/bash
#SBATCH --job-name=seqtk_211R
#SBATCH --nodes=1 --cpus-per-task=16
#SBATCH --mem=500G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 7-24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH --time=24:00:00  # Job time limit
#SBATCH -o slurm-seqtk_211R.out  # %j = job ID
#SBATCH -e slurm-seqtk_211R.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data

#load modules 

module load uri/main
module load seqtk/1.4-GCC-12.3.0

#move to scratch directory for temporary work with large files 

cd /scratch3/workspace/ashuffmyer_uri_edu-cots/

cp /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data/trim.211R_R1_001.fastq.gz /scratch3/workspace/ashuffmyer_uri_edu-cots/
cp /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data/trim.211R_R2_001.fastq.gz /scratch3/workspace/ashuffmyer_uri_edu-cots/

echo "Files copied to scratch directory" $(date)

#unzip files
gunzip trim.211R_R1_001.fastq.gz
gunzip trim.211R_R2_001.fastq.gz 

echo "Unzipping complete" $(date)

echo "Starting subsetting" $(date)

## Subsample 90,000,000 paired reads from the 211R sample
seqtk sample -s100 trim.211R_R1_001.fastq 90000000 > trim_sub.211R_R1_001.fastq
seqtk sample -s100 trim.211R_R2_001.fastq 90000000 > trim_sub.211R_R2_001.fastq

echo "Finished subsetting" $(date)

echo "Starting file zipping" $(date)

gzip trim_sub.211R_R1_001.fastq 
gzip trim_sub.211R_R2_001.fastq 

echo "Finished file zipping" $(date)

#copy files back into trimmed sequences directory 
cp /scratch3/workspace/ashuffmyer_uri_edu-cots/trim_sub.211R_R1_001.fastq.gz /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data/

cp /scratch3/workspace/ashuffmyer_uri_edu-cots/trim_sub.211R_R2_001.fastq.gz /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/trimmed_data/

echo "Files copied. Complete." $(date)

```

Run script.  

```
sbatch seqtk_211R.sh
``` 

Job started on 10 March at 07:00. I had to resubmit this job several times - I ran into errors with file copying and symlinking.    







I left off here - will complete the steps elow next.  


Check file size of new files. 

```
cd trimmed_data

ls -lh 
```

File size is now XXXXX. Zipped trimmed files were originally 22 GB (R1) and 24GB (R2) before trimming.    


### Write a script to align 211 subsetting trimmed data to P. evermanni genome 

Sym link file into por file folder 

Now remove previous files that did not run for the full 211R file. 


```
cd scripts

nano align_211R.sh
```

Write the script.  

```
#!/bin/bash
#SBATCH --job-name=211R-por-ever-align
#SBATCH --nodes=1 --cpus-per-task=15
#SBATCH --mem=200G  # Requested Memory
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH -t 7-24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH -o slurm-211R-por-ever-align.out  # %j = job ID
#SBATCH -e slurm-211R-por-ever-align.err  # %j = job ID
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por-ever/

#load modules
echo "Loading programs" $(date)
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

echo "Starting read alignment for 211R." $(date)
#loop through all files to align them to genome
for i in /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por/trim.sub*_R1_001.fastq.gz; do

# Define the corresponding R2 file by replacing _R1_ with _R2_
    r2_file="${i/_R1_001.fastq.gz/_R2_001.fastq.gz}"
    
# Define output prefix
    output_prefix="/scratch3/workspace/ashuffmyer_uri_edu-cots/por-ever/$(basename "${i%_R1_001.fastq.gz}")_"
    
# Run STAR alignment
    STAR --runMode alignReads \
        --genomeDir /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/refs/por-ever/Pevermanni_index \
        --runThreadN 10 \
        --readFilesCommand zcat \
        --readFilesIn "$i" "$r2_file" \
        --outSAMtype BAM SortedByCoordinate \
        --outSAMunmapped Within \
        --outSAMattributes Standard \
        --outFileNamePrefix "$output_prefix"
done

echo "Alignment of 211R Trimmed Seq data complete." $(date)

```   

```
sbatch align_211R.sh
```


Then generate alignment stats for all files again. 

```
sbatch por-ever-stats.sh
```

Submitted job on XXX  

Move file to user directory 

```
cp /scratch3/workspace/ashuffmyer_uri_edu-cots/por-ever/por_ever_alignment_stats.txt /work/pi_hputnam_uri_edu/ashuffmyer/cots-gorman/por-ever/
```

The output looks like this: 

```
xxx
``` 
