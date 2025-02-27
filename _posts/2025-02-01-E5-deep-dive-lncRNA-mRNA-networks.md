---
layout: post
title: E5 deep dive lncRNA mRNA correlation networks
date: '2025-02-04'
categories: E5
tags: Integration Molecular R 
---

This post details mRNA-lncRNA correlation network analyses for the E5 deep dive expression project.   

# Overview 

This analysis is in response to [this GitHub issue](https://github.com/urol-e5/deep-dive-expression/issues/26) to examine correlations between mRNA and lncRNA expression in the [E5 deep dive expression project](https://github.com/urol-e5/deep-dive-expression).    

## Where to find the input data  

### Acropora pulchra 

[mRNA](https://github.com/urol-e5/deep-dive-expression/blob/main/D-Apul/output/07-Apul-Hisat/Apul-gene_count_matrix.csv)   

[lncRNA](https://github.com/urol-e5/deep-dive-expression/blob/main/D-Apul/output/19-Apul-lncRNA-matrix/Apul-lncRNA-counts.txt)   

### Porites evermanni 

[mRNA](https://github.com/urol-e5/deep-dive-expression/blob/main/E-Peve/output/06-Peve-Hisat/Peve-gene_count_matrix.csv)   

[lncRNA](https://github.com/urol-e5/deep-dive-expression/blob/main/E-Peve/output/07-Peve-lncRNA-matrix/Peve-lncRNA-counts.txt)   

### Pocillopora tuahiniensis  

[mRNA](https://github.com/urol-e5/deep-dive-expression/blob/main/F-Ptuh/output/06-Ptuh-Hisat/Ptuh-gene_count_matrix.csv)   

[lncRNA](https://github.com/urol-e5/deep-dive-expression/blob/main/F-Ptuh/output/08-Ptuh-lncRNA-matrix/Peve-lncRNA-counts.txt)  

## Where to find scripts 

[Acropora pulchra mRNA-lncRNA correlation script](https://github.com/urol-e5/deep-dive-expression/blob/main/D-Apul/code/21-Apul-mRNA-lncRNA-correlation-networks.Rmd)  

[Pocillopora tuahiniensis mRNA-lncRNA correlation script](https://github.com/urol-e5/deep-dive-expression/blob/main/F-Ptuh/code/10-Ptuh-mRNA-lncRNA-correlation-networks.Rmd)  

[Porites evermanni mRNA-lncRNA correlation script](https://github.com/urol-e5/deep-dive-expression/blob/main/E-Peve/code/09-Peve-mRNA-lncRNA-correlation-networks.Rmd)  

## Where to view the full results 

I have generated GitHub markdown documents for these analyses and will summarize the key results and figures below. To view the full script and detailed results, visit the links below.  

[Acropora pulchra results]()  

[Pocillopora tuahiniensis results]()  

[Porites evermanni results]()  

# Key results and interesting take aways  

I used WGCNA correlation network analyses to investigate correlations between mRNA and lncRNA in each species and then viewed relationships bewteen modules of coexpressed mRNA-lncRNAs using correlation networks. We can qualitatively compare these networks between species.  

## *Acropora pulchra*  

- **Co-expression of mRNAs and lncRNAs are co-expressed:** There were co-expressed modules of mRNA and lncRNAs. Rather than perform pairwise comparisons between many features, WGCNA identifies groups of co-expressed features. All modules contained both mRNAs and lncRNAs (indicating co-expression of features in each module) with varying numbers of lncRNAs contained in each module.  

Each of these modules can be examined for functional enrichment to determine biological functions involved in co-expression of mRNAs and lncRNAs.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/ap2.png?raw=true)  

Some modules contain more features than others and most are a majority mRNAs with some variability in the number of lncRNAs in each module.  

The expression of each module for each sample can be seen below. We will use module expression values to conduct module-module correlations next.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/ap3.png?raw=true)

- **Correlation of mRNA and lncRNA co-expression modules:** There were several modules that were correlated. These correlations represent groups of co-expressed mRNAs and lncRNAs that are either negatively or positively correlated to other groups of co-expressed features.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/ap4.png?raw=true)  

This correlation matrix shows significantly correlated modules. Note that there is one pair of modules that are positively correlated while the others are negatively correlated. 

Here is an example of two correlated modules and their expression values.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/ap5.png?raw=true)

- **Correlation network of mRNA-lncRNA co-expression:** I then generated a correlation network to show the relationships between modules of co-expressed mRNA and lncRNAs.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/ap1.png?raw=true)  

In this plot, the color shows the proportion of features in the module that are lncRNAs. The lines indicate significant correlations (r>0.6; p<0.05) with gray lines indicating negative correlations and black lines indicating positive correlations.  

There are a few interesting take-aways:  

- There are 4 groups of modules that are significantly related and may interact with each other. 
- Most of the modules are not correlated to other modules, indicating potential unique regulatory mechanisms.  

## *Porites evermanni*  

- **Co-expression of mRNAs and lncRNAs are co-expressed:** There were co-expressed modules of mRNA and lncRNAs. Rather than perform pairwise comparisons between many features, WGCNA identifies groups of co-expressed features. All modules contained both mRNAs and lncRNAs (indicating co-expression of features in each module) with varying numbers of lncRNAs contained in each module.  

Each of these modules can be examined for functional enrichment to determine biological functions involved in co-expression of mRNAs and lncRNAs.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pe2.png?raw=true)  

Some modules contain more features than others and most are a majority mRNAs with some variability in the number of lncRNAs in each module.  

The expression of each module for each sample can be seen below. We will use module expression values to conduct module-module correlations next.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pe3.png?raw=true)

- **Correlation of mRNA and lncRNA co-expression modules:** There were several modules that were correlated. These correlations represent groups of co-expressed mRNAs and lncRNAs that are either negatively or positively correlated to other groups of co-expressed features.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pe4.png?raw=true)  

This correlation matrix shows significantly correlated modules. Note that there is one pair of modules that are positively correlated while the others are negatively correlated. There are more significant correlations than we saw in *A. pulchra*.  

Here is an example of two correlated modules and their expression values.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pe5.png?raw=true)

- **Correlation network of mRNA-lncRNA co-expression:** I then generated a correlation network to show the relationships between modules of co-expressed mRNA and lncRNAs.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pe1.png?raw=true)  

In this plot, the color shows the proportion of features in the module that are lncRNAs. The lines indicate significant correlations (r>0.6; p<0.05) with gray lines indicating negative correlations and black lines indicating positive correlations.  

There are a few interesting take-aways:  

- There are 4 groups of modules that are significantly related and may interact with each other. There are more modules that are significantly correlated than we saw in *A. pulchra*.  
- Most of the modules are not correlated to other modules, indicating potential unique regulatory mechanisms.  
- Most correlations between modules are negative, as seen in *A. pulchra*.  

## *Pocillopora tuahiniensis*  

- **Co-expression of mRNAs and lncRNAs are co-expressed:** There were co-expressed modules of mRNA and lncRNAs. Rather than perform pairwise comparisons between many features, WGCNA identifies groups of co-expressed features. All modules contained both mRNAs and lncRNAs (indicating co-expression of features in each module) with varying numbers of lncRNAs contained in each module.  

Each of these modules can be examined for functional enrichment to determine biological functions involved in co-expression of mRNAs and lncRNAs.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pt2.png?raw=true)  

Some modules contain more features than others and most are a majority mRNAs with some variability in the number of lncRNAs in each module.  

The expression of each module for each sample can be seen below. We will use module expression values to conduct module-module correlations next.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pt3.png?raw=true)

- **Correlation of mRNA and lncRNA co-expression modules:** There were several modules that were correlated. These correlations represent groups of co-expressed mRNAs and lncRNAs that are either negatively or positively correlated to other groups of co-expressed features.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pt4.png?raw=true)  

This correlation matrix shows significantly correlated modules. Note that there are four pairs of modules that are positively correlated while the others are negatively correlated. There are more significant correlations than we saw in the other species and more positively correlated modules.  

Note that the r values are all 0.9 in the matrix above. This is because I ran a spearman correlation (due to low sample size). The way the r values are claculated are rank based, and with only n=5 samples, the number of possible ranks are very small. There are few unique rank orderings, so this leads to a small set of possible r values. We will need to think about our limitations using correlation analyses with only n=5.  

Here is an example of two correlated modules and their expression values.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pt5.png?raw=true)

- **Correlation network of mRNA-lncRNA co-expression:** I then generated a correlation network to show the relationships between modules of co-expressed mRNA and lncRNAs.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_deepdive/20250204/pt1.png?raw=true)  

In this plot, the color shows the proportion of features in the module that are lncRNAs. The lines indicate significant correlations (r>0.6; p<0.05) with gray lines indicating negative correlations and black lines indicating positive correlations.  

There are a few interesting take-aways:  

- There are more significant correlations between modules than the other two species. 
- There are 4 groups of modules that are significantly related and may interact with each other. One of these networks is large, with 8 modules significantly correlated.     

## Next steps 

The next steps will be to perform functional enrichment to look at biological functions in co-expressed features and modules.  