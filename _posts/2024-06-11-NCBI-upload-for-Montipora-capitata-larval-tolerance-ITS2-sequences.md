---
layout: post
title: NCBI upload for Montipora capitata larval tolerance ITS2 sequences
date: '2024-06-11'
categories: Larval_Symbiont_TPC_2023
tags: ITS2 Mcapitata Molecular
---

This post details NCBI SRA upload of ITS2 sequences for the *Montipora capitata* larval thermal tolerance project on June 11 2024.  

Files will be uploaded to existing BioProject PRJNA1078313 on NCBI SRA.  

The metadata for these files is located on the project [GitHub repo](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/data/its2). 

I created a new folder for the NCBI upload and sym linked the files for upload.  

```
cd /data/putnamlab/ashuffmyer/hawaii_2023_its2

mkdir ncbi_its2_upload
cd ncbi_its2_upload

ln -s /data/putnamlab/ashuffmyer/hawaii_2023_its2/raw-sequences/*fastq.gz /data/putnamlab/ashuffmyer/hawaii_2023_its2/ncbi_its2_upload
```

I followed FTP instructions and make a new upload folder on NCBI called `its2_20240611`.  

ITS2 sequence files were submitted under SRA SUB14527350.

All information added to the Putnam Lab [sequence inventory here](https://docs.google.com/spreadsheets/d/1qDGGpLFcmoO-fIFOPSUhPcxi4ErXIq2PkQbxvCCzI40/edit#gid=521605465).
