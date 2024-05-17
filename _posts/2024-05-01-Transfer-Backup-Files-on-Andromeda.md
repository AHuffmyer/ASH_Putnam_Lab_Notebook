---
layout: post
title: Transfer raw sequencing data to backup Andromeda folder 
date: '2024-05-01'
categories: Larval_Symbiont_TPC_2023 Mcapitata_EarlyLifeHistory_2020
tags: 16S ITS2 GeneExpression 
---

This post details transfer of raw files for sequencing projects to hputnam KITT folder on Andromeda as an additional backup.   

*Pending HP permissions to use KITT directory*  

Folders:   

**Mcap 2020**  

- TagSeq/ITS2/16S: `20211219_Mcapitata_OntogenyTimeseries` at `/data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries`

**Mcap 2023**  

- RNASeq: `20240220_Mcapitata_LarvalTolerance` at `/data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance`

# Mcap2020

Log onto Andromeda and navigate to hputnam KITT folder where raw data needs to live.    

```
cd /data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries
mkdir tagseq
mkdir ITS2
mkdir 16S 
```

Project folder: `/data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries`

TagSeq: `/data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/tagseq`
ITS2: `/data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/ITS2`
16S: `/data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/16S`

TagSeq file transfer

```
cp /data/putnamlab/ashuffmyer/mcap-2020-tagseq/sequences/*fastq.gz /data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/tagseq

cp /data/putnamlab/ashuffmyer/mcap-2020-tagseq/sequences/md5.original /data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/tagseq
```


ITS2 file transfer 

```
cp /data/putnamlab/ashuffmyer/AH_MCAP_ITS2/raw_data/md5.seqs /data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/ITS2

cp /data/putnamlab/ashuffmyer/AH_MCAP_ITS2/raw_data/*.fastq.gz /data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/ITS2

```

16S file transfer 


```
cp /data/putnamlab/ashuffmyer/AH_MCAP_16S/raw_data/*fastq.gz /data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/16S

cp /data/putnamlab/ashuffmyer/AH_MCAP_16S/raw_data/md5.seqs /data/putnamlab/KITT/hputnam/20211219_Mcapitata_OntogenyTimeseries/16S

```

Done on 17 May 2024.  

# Mcap2023

Log onto Andromeda and navigate to hputnam KITT folder where raw data needs to live.   

Project folder: `/data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance`

RNASeq: `/data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance/rna_seq ` 

RNA-seq file transfer 

```
cd /data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance

mkdir rna_seq #make RNASeq folder 
cd rna_seq

mkdir second_delivery #make a file for second sequence delivery
```

Copy in raw .fastq files 

```
cp /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/*.fastq.gz /data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance/rna_seq

cp /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/second_sequencing/*.fastq.gz /data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance/rna_seq/second_delivery
```

Copy in original .md5 files 

```
cp /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/md5_files/*.md5 /data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance/rna_seq

cp /data/putnamlab/ashuffmyer/mcap-2023-rnaseq/raw-sequences/second_sequencing/md5_files/*.md5 /data/putnamlab/KITT/hputnam/20240220_Mcapitata_LarvalTolerance/rna_seq/second_delivery

```

Done on 17 May 2024.  




