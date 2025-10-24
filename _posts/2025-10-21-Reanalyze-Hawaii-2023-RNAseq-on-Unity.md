---
layout: post
title: Alignment of Montipora capitata larval thermal tolerance 2023 project on Unity with STAR
date: '2025-10-21'
categories: Larval_Symbiont_TPC_2023
tags: Bioinformatics Mcapitata Molecular GeneExpression
---

This post details alignment (using STAR) and assembly steps of the bioinformatic pipeline for the *Montipora capitata* 2023 larval thermal tolerance project RNAseq. See my [notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#larval-symbiont-tpc-2023) and my [GitHub repo](https://github.com/AHuffmyer/larval_symbiont_TPC) for information on this project.

You can find QC for these files in [this post for original data delivery](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/RNAseq-QC-for-Mcapitata-Larval-Thermal-Tolerance-Project/) and [this post for second data delivery for additional top off sequencing](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Toppoff-Sequencing-RNAseq-QC-for-Mcapitata-Larval-Thermal-Tolerance-Project/).  

Files for this project include RNAseq files for 54 samples. Of these, 13 samples required additional sequencing to meet project deliverables for read depth. These files are denoted with an "s" after the sample prefix in the file name. 

Samples with two rounds of sequencing have been QC'd individually and all files pass QC checks. The files for each sample will be combined/concatenated prior to alignment and assembly.  

I previously did an alignment with Hisat2 and I am redoing this alignment with STAR, which is better suited for higher accuracy.  

See [my previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/RNAseq-QC-for-Mcapitata-Larval-Thermal-Tolerance-Project/) and this [post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Toppoff-Sequencing-RNAseq-QC-for-Mcapitata-Larval-Thermal-Tolerance-Project/) for QC information, which will not be repeated here. I will be using the same trimming script, so I will not be repeating the pre or post trimming QC steps.  

# 1. Trim raw sequence files 

Files are now located on Unity at `/project/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq`. Work will be conducted at `/work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq`.

Sym link files to the new work location. 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq
mkdir raw-sequences

ln -s /project/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/*fastq.gz /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/raw-sequences

```

Sym link reference files. 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq
mkdir refs

ln -s /project/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/Montipora_capitata* /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs
```

Make a new folder for trimmed sequences. 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq
mkdir trimmed-sequences
mkdir scripts
```

Write a script for trimming.  

```
cd scripts
nano trimming.sh
```

Generate a script adapted from Jill Ashey's [script here](https://github.com/JillAshey/JillAshey_Putnam_Lab_Notebook/blob/master/_posts/2024-01-18-RNASeq-Pacuta-Hawaii-2022.md).  

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
	

```
#!/bin/bash
#SBATCH --time=24:00:00  # Job time limit
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --export=NONE
#SBATCH --mem=100GB
#SBATCH --mail-type=BEGIN,END,FAIL #email you when job starts, stops and/or fails
#SBATCH --mail-user=ashuff@uw.edu #your email to send notifications
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/scripts          
#SBATCH -o trimming-%j.out
#SBATCH -e trimming-%j.error

# Load modules needed 
module load uri/main
module load fastqc/0.12.1
module load MultiQC/1.12-foss-2021b
module load fastp/0.23.2-GCC-11.2.0

# Make an array of sequences to trim in raw data directory 

cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/raw-sequences/
array1=($(ls *R1_001.fastq.gz))

echo "Read trimming of adapters started." $(date)

# fastp and fastqc loop 
for i in ${array1[@]}; do
    fastp --in1 ${i} \
        --in2 $(echo ${i}|sed s/_R1/_R2/)\
        --out1 /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/trimmed-sequences/trim.${i} \
        --out2 /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/trimmed-sequences/trim.$(echo ${i}|sed s/_R1/_R2/) \
        --detect_adapter_for_pe \
        --qualified_quality_phred 30 \
        --unqualified_percent_limit 10 \
        --length_required 100 

done

echo "Read trimming of adapters completed." $(date)
```

```
sbatch trimming.sh
```

Job 47070682 submitted on 21 Oct at 9:30am. Completed at 2:00pm. 

```
squeue -u ashuffmyer_uri_edu
```

Trimming completed. 

I then downloaded the fastp.html report to look at trimming information.
  
```
scp ashuffmyer_uri_edu@unity.uri.edu:/work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/raw-sequences/fastp.html ~/MyProjects/larval_symbiont_TPC/data/rna_seq/QC
```

This file can be found [on GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/data/rna_seq/QC/fastp_20240223.html). 

```
General
fastp version:	0.23.2 (https://github.com/OpenGene/fastp)
sequencing:	paired end (150 cycles + 150 cycles)
mean length before filtering:	150bp, 150bp
mean length after filtering:	147bp, 147bp
duplication rate:	15.012106%
Insert size peak:	169
Detected read1 adapter:	AGATCGGAAGAGCACACGTCTGAACTCCAGTCA

Before filtering
total reads:	32.443162 M
total bases:	4.866474 G
Q20 bases:	4.629020 G (95.120605%)
Q30 bases:	4.274311 G (87.831783%)
GC content:	44.578400%

After filtering
total reads:	18.457446 M
total bases:	2.724495 G
Q20 bases:	2.702930 G (99.208472%)
Q30 bases:	2.651206 G (97.309989%)
GC content:	43.743317%

Filtering result
reads passed filters:	18.457446 M (56.891637%)
reads with low quality:	13.754650 M (42.396145%)
reads with too many N:	72 (0.000222%)
reads too short:	230.994000 K (0.711996%)
```

# 2. Concatenate files with multiple sequencing runs 

Concatenate files with two rounds of sequencing. 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/trimmed-sequences

cat trim.R55*R1*.fastq.gz >> trim.R55c_R1_001.fastq.gz #done
cat trim.R55*R2*.fastq.gz >> trim.R55c_R2_001.fastq.gz #done

cat trim.R56*R1*.fastq.gz >> trim.R56c_R1_001.fastq.gz #done
cat trim.R56*R2*.fastq.gz >> trim.R56c_R2_001.fastq.gz #done

cat trim.R57*R1*.fastq.gz >> trim.R57c_R1_001.fastq.gz #done
cat trim.R57*R2*.fastq.gz >> trim.R57c_R2_001.fastq.gz #done

cat trim.R58*R1*.fastq.gz >> trim.R58c_R1_001.fastq.gz #done
cat trim.R58*R2*.fastq.gz >> trim.R58c_R2_001.fastq.gz #done

cat trim.R59*R1*.fastq.gz >> trim.R59c_R1_001.fastq.gz #done
cat trim.R59*R2*.fastq.gz >> trim.R59c_R2_001.fastq.gz #done

cat trim.R60*R1*.fastq.gz >> trim.R60c_R1_001.fastq.gz #done
cat trim.R60*R2*.fastq.gz >> trim.R60c_R2_001.fastq.gz #done

cat trim.R62*R1*.fastq.gz >> trim.R62c_R1_001.fastq.gz #done
cat trim.R62*R2*.fastq.gz >> trim.R62c_R2_001.fastq.gz #done

cat trim.R67*R1*.fastq.gz >> trim.R67c_R1_001.fastq.gz #done
cat trim.R67*R2*.fastq.gz >> trim.R67c_R2_001.fastq.gz #done

cat trim.R75*R1*.fastq.gz >> trim.R75c_R1_001.fastq.gz #done
cat trim.R75*R2*.fastq.gz >> trim.R75c_R2_001.fastq.gz #done

cat trim.R83*R1*.fastq.gz >> trim.R83c_R1_001.fastq.gz #done
cat trim.R83*R2*.fastq.gz >> trim.R83c_R2_001.fastq.gz #done

cat trim.R91*R1*.fastq.gz >> trim.R91c_R1_001.fastq.gz #done
cat trim.R91*R2*.fastq.gz >> trim.R91c_R2_001.fastq.gz #done

cat trim.R99*R1*.fastq.gz >> trim.R99c_R1_001.fastq.gz #done
cat trim.R99*R2*.fastq.gz >> trim.R99c_R2_001.fastq.gz #done

cat trim.R107*R1*.fastq.gz >> trim.R107c_R1_001.fastq.gz #done
cat trim.R107*R2*.fastq.gz >> trim.R107c_R2_001.fastq.gz #done

```

Create a new directory for all final files for alignment. 

```
/work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq
mkdir cat-sequences
cd cat-sequences

ln -s /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/trimmed-sequences/*fastq.gz /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/cat-sequences

```

Remove files that will not be used from the new directory.  

```
#remove the second data delivery files 
rm *s*.fastq.gz

#manually remove the first data delivery files 
rm trim.R55_R*fastq.gz
rm trim.R56_R*fastq.gz
rm trim.R57_R*fastq.gz
rm trim.R58_R*fastq.gz
rm trim.R59_R*fastq.gz
rm trim.R60_R*fastq.gz
rm trim.R62_R*fastq.gz
rm trim.R67_R*fastq.gz
rm trim.R75_R*fastq.gz
rm trim.R83_R*fastq.gz
rm trim.R91_R*fastq.gz
rm trim.R99_R*fastq.gz
rm trim.R107_R*fastq.gz
```

Files are now ready for alignment at `/work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/cat-sequences`. 

We have previously downloaded and format fixed reference files - see details in notebook posts linked above. In order to do alternative splicing analysis in the future, we may need to revise GFF3 formatting.   

# 3. Align reads to genome with STAR 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/scripts

nano align_star_index.sh
```

```
#!/bin/bash
#SBATCH -t 24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --export=NONE
#SBATCH --mem=100GB
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --mail-type=BEGIN,END,FAIL #email you when job starts, stops and/or fails
#SBATCH --mail-user=ashuff@uw.edu #your email to send notifications
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/       
#SBATCH -o align-star-index-%j.out
#SBATCH -e align-star-index-%j.error

# load modules needed
module load uri/main
module load STAR/2.7.11b-GCC-12.3.0

#echo "Building genome reference" $(date)

# index genome 

STAR --runThreadN 6 \
--runMode genomeGenerate \
--genomeDir Mcap_star_index \
--genomeFastaFiles /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/Montipora_capitata_HIv3.assembly.fasta \
--sjdbGTFfile /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/Montipora_capitata_HIv3.genes_fixed.gff3 \
--sjdbGTFtagExonParentTranscript Parent \
--sjdbOverhang 99 \
--genomeSAindexNbases 13

echo "Referece genome indexed." $(date)
```

```
sbatch align_star_index.sh
```

Job 47201148 submitted at 08:30 on Oct 22. Finished after about 15 min.  

Start a scratch directory to generate .bam files. 

```
ws_allocate mcap-rnaseq 30
```

Our scratch directory is `/scratch4/workspace/ashuffmyer_uri_edu-mcap-rnaseq` for 30 days (until Nov 21).  

Change permission. 

```
chmod 777 /scratch4/workspace/ashuffmyer_uri_edu-mcap-rnaseq

#now reads 
drwxrwsrwx 2 ashuffmyer_uri_edu ashuffmyer_uri_edu 2 Oct 22 15:20 .
```

Make a script for alignment. 

```
nano align_star.sh
```


```
#!/bin/bash
#SBATCH -t 7-24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --export=NONE
#SBATCH --mem=200GB
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --mail-type=BEGIN,END,FAIL #email you when job starts, stops and/or fails
#SBATCH --mail-user=ashuff@uw.edu #your email to send notifications
#SBATCH -D /scratch4/workspace/ashuffmyer_uri_edu-mcap-rnaseq/      
#SBATCH -o align-star-%j.out
#SBATCH -e align-star-%j.error

#load modules
echo "Loading programs" $(date)
module load uri/main
module load star/2.7.11a
module load STAR/2.7.11b-GCC-12.3.0

echo "Starting read alignment." $(date)
#loop through all files to align them to genome
for i in /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/cat-sequences/*_R1_001.fastq.gz; do

# Define the corresponding R2 file by replacing _R1_ with _R2_
    r2_file="${i/_R1_001.fastq.gz/_R2_001.fastq.gz}"
    
# Define output prefix CHECK THIS 
    output_prefix="$(basename "${i%_R1_001.fastq.gz}")_"
    
 # Run STAR alignment
    STAR --runMode alignReads \
        --genomeDir /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/Mcap_star_index \
        --runThreadN 8 \
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
sbatch align_star.sh
```

Submitted job 47213157 at 09:15 on Oct 22 and completed 2:00am on Oct 23.  

Obtain mapping percentages. 

```
salloc -c 1

cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/
mkdir bam-files 
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files

ln -s /scratch4/workspace/ashuffmyer_uri_edu-mcap-rnaseq/*.bam /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files 

mv /scratch4/workspace/ashuffmyer_uri_edu-mcap-rnaseq/align-star-47213157.out /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files 

mv /scratch4/workspace/ashuffmyer_uri_edu-mcap-rnaseq/align-star-47213157.error /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files 

module load uri/main
module load samtools/1.19.2

for i in *.bam; do
    echo "${i}" >> mapped_reads_counts
    samtools flagstat ${i} | grep "mapped (" >> mapped_reads_counts
done

```

The output looks like this: 

```
trim.R100_Aligned.sortedByCoord.out.bam
46351600 + 0 mapped (89.96% : N/A)
40062148 + 0 primary mapped (88.56% : N/A)
trim.R101_Aligned.sortedByCoord.out.bam
50768442 + 0 mapped (91.49% : N/A)
44710580 + 0 primary mapped (90.44% : N/A)
trim.R102_Aligned.sortedByCoord.out.bam
44342116 + 0 mapped (89.47% : N/A)
38209940 + 0 primary mapped (87.98% : N/A)
trim.R103_Aligned.sortedByCoord.out.bam
30918840 + 0 mapped (88.27% : N/A)
25973574 + 0 primary mapped (86.34% : N/A)
trim.R104_Aligned.sortedByCoord.out.bam
38923330 + 0 mapped (86.38% : N/A)
34498334 + 0 primary mapped (84.90% : N/A)
trim.R105_Aligned.sortedByCoord.out.bam
38887400 + 0 mapped (90.16% : N/A)
34336102 + 0 primary mapped (89.00% : N/A)
trim.R106_Aligned.sortedByCoord.out.bam
45973826 + 0 mapped (88.77% : N/A)
40760860 + 0 primary mapped (87.51% : N/A)
trim.R107c_Aligned.sortedByCoord.out.bam
33163056 + 0 mapped (90.01% : N/A)
28872384 + 0 primary mapped (88.70% : N/A)
trim.R108_Aligned.sortedByCoord.out.bam
39628162 + 0 mapped (90.00% : N/A)
34879708 + 0 primary mapped (88.80% : N/A)
trim.R55c_Aligned.sortedByCoord.out.bam
24445612 + 0 mapped (89.00% : N/A)
21676022 + 0 primary mapped (87.77% : N/A)
trim.R56c_Aligned.sortedByCoord.out.bam
21918642 + 0 mapped (82.86% : N/A)
19525822 + 0 primary mapped (81.16% : N/A)
trim.R57c_Aligned.sortedByCoord.out.bam
23561368 + 0 mapped (88.08% : N/A)
20880068 + 0 primary mapped (86.75% : N/A)
trim.R58c_Aligned.sortedByCoord.out.bam
24726428 + 0 mapped (88.33% : N/A)
21769660 + 0 primary mapped (86.95% : N/A)
trim.R59c_Aligned.sortedByCoord.out.bam
50734702 + 0 mapped (87.31% : N/A)
45173142 + 0 primary mapped (85.96% : N/A)
trim.R60c_Aligned.sortedByCoord.out.bam
23296180 + 0 mapped (87.14% : N/A)
20666918 + 0 primary mapped (85.73% : N/A)
trim.R61_Aligned.sortedByCoord.out.bam
26344836 + 0 mapped (89.77% : N/A)
22675098 + 0 primary mapped (88.31% : N/A)
trim.R62c_Aligned.sortedByCoord.out.bam
24324264 + 0 mapped (88.13% : N/A)
20959126 + 0 primary mapped (86.48% : N/A)
trim.R63_Aligned.sortedByCoord.out.bam
32134262 + 0 mapped (88.33% : N/A)
28483398 + 0 primary mapped (87.03% : N/A)
trim.R64_Aligned.sortedByCoord.out.bam
42712180 + 0 mapped (89.35% : N/A)
38069446 + 0 primary mapped (88.21% : N/A)
trim.R65_Aligned.sortedByCoord.out.bam
41619646 + 0 mapped (89.59% : N/A)
36920116 + 0 primary mapped (88.42% : N/A)
trim.R66_Aligned.sortedByCoord.out.bam
43871686 + 0 mapped (91.27% : N/A)
39167380 + 0 primary mapped (90.33% : N/A)
trim.R67c_Aligned.sortedByCoord.out.bam
33769298 + 0 mapped (91.35% : N/A)
30163184 + 0 primary mapped (90.42% : N/A)
trim.R68_Aligned.sortedByCoord.out.bam
41042916 + 0 mapped (89.13% : N/A)
36330500 + 0 primary mapped (87.90% : N/A)
trim.R69_Aligned.sortedByCoord.out.bam
52185184 + 0 mapped (87.14% : N/A)
44545526 + 0 primary mapped (85.26% : N/A)
trim.R70_Aligned.sortedByCoord.out.bam
45986680 + 0 mapped (91.06% : N/A)
40811226 + 0 primary mapped (90.04% : N/A)
trim.R71_Aligned.sortedByCoord.out.bam
38882066 + 0 mapped (92.03% : N/A)
34677702 + 0 primary mapped (91.15% : N/A)
trim.R72_Aligned.sortedByCoord.out.bam
44751832 + 0 mapped (90.83% : N/A)
39567226 + 0 primary mapped (89.75% : N/A)
trim.R73_Aligned.sortedByCoord.out.bam
45787094 + 0 mapped (88.06% : N/A)
40210036 + 0 primary mapped (86.62% : N/A)
trim.R74_Aligned.sortedByCoord.out.bam
44709260 + 0 mapped (86.14% : N/A)
39543414 + 0 primary mapped (84.61% : N/A)
trim.R75c_Aligned.sortedByCoord.out.bam
27952218 + 0 mapped (87.80% : N/A)
24668350 + 0 primary mapped (86.40% : N/A)
trim.R76_Aligned.sortedByCoord.out.bam
43142482 + 0 mapped (87.66% : N/A)
38103984 + 0 primary mapped (86.26% : N/A)
trim.R77_Aligned.sortedByCoord.out.bam
51123198 + 0 mapped (86.49% : N/A)
45362626 + 0 primary mapped (85.03% : N/A)
trim.R78_Aligned.sortedByCoord.out.bam
46283608 + 0 mapped (89.70% : N/A)
41148278 + 0 primary mapped (88.57% : N/A)
trim.R79_Aligned.sortedByCoord.out.bam
32980690 + 0 mapped (85.64% : N/A)
27937300 + 0 primary mapped (83.47% : N/A)
trim.R80_Aligned.sortedByCoord.out.bam
47083906 + 0 mapped (89.99% : N/A)
41765252 + 0 primary mapped (88.86% : N/A)
trim.R81_Aligned.sortedByCoord.out.bam
40991410 + 0 mapped (86.27% : N/A)
36468584 + 0 primary mapped (84.82% : N/A)
trim.R82_Aligned.sortedByCoord.out.bam
46336232 + 0 mapped (88.94% : N/A)
40926648 + 0 primary mapped (87.66% : N/A)
trim.R83c_Aligned.sortedByCoord.out.bam
32251026 + 0 mapped (90.13% : N/A)
28320728 + 0 primary mapped (88.91% : N/A)
trim.R84_Aligned.sortedByCoord.out.bam
42900458 + 0 mapped (89.43% : N/A)
37512688 + 0 primary mapped (88.09% : N/A)
trim.R85_Aligned.sortedByCoord.out.bam
48463176 + 0 mapped (90.87% : N/A)
42798346 + 0 primary mapped (89.78% : N/A)
trim.R86_Aligned.sortedByCoord.out.bam
42481162 + 0 mapped (90.95% : N/A)
37797594 + 0 primary mapped (89.95% : N/A)
trim.R87_Aligned.sortedByCoord.out.bam
34440192 + 0 mapped (91.05% : N/A)
30550876 + 0 primary mapped (90.02% : N/A)
trim.R88_Aligned.sortedByCoord.out.bam
40315818 + 0 mapped (86.27% : N/A)
33509048 + 0 primary mapped (83.93% : N/A)
trim.R89_Aligned.sortedByCoord.out.bam
43947028 + 0 mapped (90.49% : N/A)
38957538 + 0 primary mapped (89.40% : N/A)
trim.R90_Aligned.sortedByCoord.out.bam
43620638 + 0 mapped (90.94% : N/A)
38848670 + 0 primary mapped (89.94% : N/A)
trim.R91c_Aligned.sortedByCoord.out.bam
33511132 + 0 mapped (89.97% : N/A)
29077602 + 0 primary mapped (88.62% : N/A)
trim.R92_Aligned.sortedByCoord.out.bam
38864576 + 0 mapped (89.24% : N/A)
33305080 + 0 primary mapped (87.67% : N/A)
trim.R93_Aligned.sortedByCoord.out.bam
44734700 + 0 mapped (81.84% : N/A)
32630094 + 0 primary mapped (76.68% : N/A)
trim.R94_Aligned.sortedByCoord.out.bam
42138752 + 0 mapped (87.55% : N/A)
36643412 + 0 primary mapped (85.95% : N/A)
trim.R95_Aligned.sortedByCoord.out.bam
37114114 + 0 mapped (90.67% : N/A)
32858310 + 0 primary mapped (89.58% : N/A)
trim.R96_Aligned.sortedByCoord.out.bam
45394652 + 0 mapped (84.49% : N/A)
37711206 + 0 primary mapped (81.90% : N/A)
trim.R97_Aligned.sortedByCoord.out.bam
43912004 + 0 mapped (90.51% : N/A)
38289414 + 0 primary mapped (89.27% : N/A)
trim.R98_Aligned.sortedByCoord.out.bam
45800554 + 0 mapped (88.00% : N/A)
40726402 + 0 primary mapped (86.71% : N/A)
trim.R99c_Aligned.sortedByCoord.out.bam
31064742 + 0 mapped (89.48% : N/A)
26291440 + 0 primary mapped (87.81% : N/A)

```

Mapping rates are 1-4% higher than they were with hisat2. Continue to assembly. 

# 3. Assemble and quantify read counts with stringtie 

Write a script for assembly. 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/scripts

nano assembly.sh
```

We will use stringtie for assembly with the following options:  

- `-p 8` for using multiple processors 
- `-e` this option directs StringTie to operate in expression estimation mode; this limits the processing of read alignments to estimating the coverage of the transcripts given with the -G option (hence this option requires -G).
- `-B` This switch enables the output of Ballgown input table files (.ctab) containing coverage data for the reference transcripts given with the -G option. (See the Ballgown documentation for a description of these files.) With this option StringTie can be used as a direct replacement of the tablemaker program included with the Ballgown distribution. If the option -o is given as a full path to the output transcript file, StringTie will write the .ctab files in the same directory as the output GTF.
- `-G` Use a reference annotation file (in GTF or GFF3 format) to guide the assembly process. The output will include expressed reference transcripts as well as any novel transcripts that are assembled. This option is required by options -B, -b, -e, -C.
- `-A` Gene abundances will be reported (tab delimited format) in the output file with the given name.
- `-o` output file name for the merged transcripts GTF (default: stdout)

```
#!/bin/bash
#SBATCH -t 2-24:00:00
#SBATCH -q long #job lasting over 2 days
#SBATCH --nodes=1 --cpus-per-task=8
#SBATCH --export=NONE
#SBATCH --mem=100GB
#SBATCH -p gpu  # Partition
#SBATCH -G 1  # Number of GPUs
#SBATCH --mail-type=BEGIN,END,FAIL #email you when job starts, stops and/or fails
#SBATCH --mail-user=ashuff@uw.edu #your email to send notifications
#SBATCH -D /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files     
#SBATCH -o assemble-%j.out
#SBATCH -e assemble-%j.error

#load modules
echo "Loading programs" $(date)
module load uri/main
module load StringTie/2.2.1-GCC-11.2.0

echo "Assembling transcripts using stringtie" $(date)

cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files

array=($(ls *.bam)) #Make an array of sequences to assemble

for i in ${array[@]}; do 
        sample_name=`echo $i| awk -F [_] '{print $1}'`
	stringtie -p 8 -e -B -G /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/Montipora_capitata_HIv3.genes_fixed.gff3 -A ${sample_name}.gene_abund.tab -o ${sample_name}.gtf ${i}
        echo "StringTie assembly for seq file ${i}" $(date)
done

echo "Assembly for each sample complete " $(date)
```

```
sbatch assembly.sh
```

Job ID 47378630 sumbitted Oct 23 at 17:30, completed 20:30. 

Sym link .gtf files to new directory. 

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/
mkdir gtf-files
cd gtf-files

ln -s /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files/*.gtf /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/gtf-files

ln -s /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/bam-files/*.tab /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/gtf-files
```

# 4. Prepare .gtf files and generate gene count matrix 

Make list of .gtf files. I am doing these steps individually, but can be added to a bash script if desired.  

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/gtf-files

ls *.gtf > gtf_list.txt
```

Merge .gtf's. Use same stringtie settings as above and generate a merged gtf file.   

```
salloc -c 1 

module load uri/main
module load StringTie/2.2.1-GCC-11.2.0

stringtie --merge -e -p 8 -G /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/Montipora_capitata_HIv3.genes_fixed.gff3 -o Mcapitata_merged.gtf gtf_list.txt 

```

Assess the accuracy of the merged assembly with `gffcompare`.

```
module purge
module load uri/main
module load GffCompare/0.12.6-GCC-11.2.0

gffcompare -r /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/Montipora_capitata_HIv3.genes_fixed.gff3 Mcapitata_merged.gtf 

  54384 reference transcripts loaded.
  2 duplicate reference transcripts discarded.
  54384 query transfrags loaded.
```

View the merged file.  

```
less gffcmp.stats 
```

```
# gffcompare v0.12.6 | Command line was:
#gffcompare -r /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/refs/Montipora_capitata_HIv3.genes_fixed.gff3 Mcapitata_merged.gtf
#

#= Summary for dataset: Mcapitata_merged.gtf 
#     Query mRNAs :   54384 in   54185 loci  (36023 multi-exon transcripts)
#            (141 multi-transcript loci, ~1.0 transcripts per locus)
# Reference mRNAs :   54382 in   54185 loci  (36023 multi-exon)
# Super-loci w/ reference transcripts:    54185
#-----------------| Sensitivity | Precision  |
        Base level:   100.0     |   100.0    |
        Exon level:   100.0     |   100.0    |
      Intron level:   100.0     |   100.0    |
Intron chain level:   100.0     |   100.0    |
  Transcript level:    99.9     |    99.9    |
       Locus level:   100.0     |   100.0    |

     Matching intron chains:   36023
       Matching transcripts:   54354
              Matching loci:   54184

          Missed exons:       0/256029  (  0.0%)
           Novel exons:       0/256028  (  0.0%)
        Missed introns:       0/201643  (  0.0%)
         Novel introns:       0/201643  (  0.0%)
           Missed loci:       0/54185   (  0.0%)
            Novel loci:       0/54185   (  0.0%)

 Total union super-loci across all input datasets: 54185 
54384 out of 54384 consensus transcripts written in gffcmp.annotated.gtf (0 discarded as redundant)
```

Make gtf list text file for gene count matrix creation; still in interactive mode. 

```
for filename in trim*.gtf; do echo $filename $PWD/$filename; done > listGTF.txt
```

Download the prepDE.py script from [here](https://github.com/gpertea/stringtie/blob/master/prepDE.py) and put it in the stringtie output folder. 

In a terminal not logged into Unity:  

```
scp ~/MyProjects/larval_symbiont_TPC/scripts/bioinformatics/prepDE.py3 ashuffmyer_uri_edu@unity.uri.edu:/work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/scripts/ 

```

Load python and compile the gene count matrix (still in interactive mode).  

```
cd /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/gtf-files

module load uri/main
module load python/3.12.3

python /work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/scripts/prepDE.py3 -g Mcapitata2023_gene_count_matrix.csv -i listGTF.txt

mv Mcapitata2023_gene_count_matrix.csv Mcapitata2023_gene_count_matrix_STAR.csv
```

We do not have any STRG gene names, which means that fixing the GFF file allowed for correct identification of annotated genes/transcripts.  

Transfer gene count matrix to desktop.  

```
scp ashuffmyer_uri_edu@unity.uri.edu:/work/pi_hputnam_uri_edu/ashuffmyer/hawaii-2023-rnaseq/gtf-files/Mcapitata2023_gene_count_matrix_STAR.csv ~/MyProjects/larval_symbiont_TPC/data/rna_seq

```

