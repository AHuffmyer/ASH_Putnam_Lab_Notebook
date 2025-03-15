---
layout: post
title: E5 molecular timeseries lipidomics preliminary analysis 
date: '2025-03-15'
categories: E5
tags: Lipidomics Lipids R
---

This post details analysis of lipidomic samples for the E5 timeseries molecular project.  

## Overview 

In this project, we are conducting multi-omics analyses of the E5 physiology time series project. We obtained data from the following groups:  

- Acropora from TP1, TP3, and TP4 
- Pocillopora from TP1, TP3, and TP4 
- Pocillopora from TP1, TP2, TP3, and TP4 

We selected these samples to obtain n=5 samples per site per species (n=10 per time point per species). Because of low sample size at TP2 for Acropora and Pocillopora, that time point was not analyzed.

*Note that in this preliminary analysis I am looking at the effects of species and time point, which were the strongest drivers of physiology. I will return to this analysis to see if site has an influence as well.*  

Samples were processed at the University of Washington Northwest Metabolomics Center in Seattle.  

Lipidomics data has been returned and in this post I detail my overview analysis of this data to see if the data are of high quality and are as we expect for coral samples.  

The repository [for this project is on GitHub](https://github.com/urol-e5/timeseries_molecular) and the script used to produce the results described here [can be found on GitHub using this direct link](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/06-lipidomics-analysis.Rmd).   

We submitted these samples for targeted lipidomic analyses at UW and they provided the data, which is [on GitHub here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/data/lipidomics/2025-02-05_Roberts_Test_coral.xlsx) and [here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/data/lipidomics/2025-02-20_Roberts_QuantLipidData.xlsx).  

Note that the facility first analyzed 5 samples to ensure the protocol worked properly. Those 5 samples are read into the script and combined with the full data set - the analyses are reproducible between batch runs.  

Here, I'll describe the main findings of this preliminary analysis so we can develop hypotheses to test using this dataset.  

## Analysis steps 

There were several data files sent to us with different lipidomic results.  

1. **Lipid species concentration**: Absolute concentrations of each lipid species detected.   
2. **Lipid species composition**: Percent composition of each lipid *within* its own class. 
	- Percent composition = lipid species concentration / sum of all lipids within the same class * 100   
3. **Class concentration**: Absolute concentration of each lipid class detected (20 total possible classes).   
4. **Class composition**: Percent composition of each class relative to all lipid classes.  
	- Percent composition = class concentration / sum of all classes (AKA total lipid) * 100
5. **Fatty acid concentration**: Sum of absolute concentration of all fatty acids that are the same in one lipid class.
	- For example: PC(FA18:1) is the sum of the concentrations of PC(12:0/18:1), PC(14:0/18:1), PC(15:0/18:1), etc for all PC(x/18:1)  
	- Free fatty acids are characterized separately as their own class in the above data sets ("FFA's"). These fatty acids are those that are part of other types of lipids (e.g., TG's or DG's).  
6. **Fatty acid composition**: Percent composition of each fatty acids relative to the sum of all fatty acids in its class. 
	- Percent composition =	fatty acid  concentration /sum of all fatty acids concentration from that class * 100

I have analyzed all of these data sets and used manually calculated composition data generated after I conducted filtering and blank normalization for absolute concentration. 

There were 700-1000 lipids detected in our samples prior to any filtering or QC.    

## Definitions  

Lipids are denoted by a letter code for the class followed by structural annotation. For example, a free fatty acid with 18 carbons and no double bonds is notated as "FA 18:0".  

Here are the possible lipid classes:  

1. **CE: Cholesterol esters**: long chain fatty acid with hydroxyl group
	- Functions: transport, detoxification
2. **Cer d18:0 and 18:1: Ceramides**: sphingolipids, consisting of a long chain fatty acid and sphingosine 
	- Functions: cell signaling, membrane structure
3. **DG: Diacylglycerols**: glycerol backbone with 2 fatty acid chains
	- Functions: signal transduction, immune function, cell membrane structure, lipid metabolism
4. **FFA: Free fatty acid**: hydrocarbon chain with carboxy group
	- Functions: primary energy source and metabolized via beta oxidation when glucose is scarce; can be released from TG's, also used in signaling; in corals can be translocated from the symbiont
	- Types: Saturated (no double bonds; 18:0); monounsaturated (1 double bond; 18:1); polyunsaturated (2+ double bonds; 18:3)
5. **HexCER: Hexosylceramides**: ceramide backbone with neutral sugar; *not present in our data*  
	- Functions: neural and brain function
6. **LPC: Lyso-phosphatidyl-cholines**: glycerol with fatty acid and choline
	- Functions: mediate oxidative stress, plasma lipid
7. **LPE: Lyso-phosphatidyl-ethanolamines**: ethanolamine head and glycerophosphoric acid with fatty acid
	- Functions: membrane function, regulation, oxidative phosphorylation
8. **LPG: Lyso-phosphatidyl-glycerol**: glycerol with fatty acid and phosphoglycerol
	- Functions: signaling, cell proliferation, cell survival
9. **LPI: Lyso-phosphatidyl-inositol**: glycerol backbone with fatty acid and phosphoinositol
	- Functions: cell proliferation, signaling
10. **LPS: Lipopolysaccharides**: lipid domain with core oligosaccharide and polysaccharide; *not present in our data*  
	- Functions: outer membrane components in bacteria
11. **LacCER: Lactosylceramides**: ceramide backbone with a lactose including a fatty acid; *not present in our data*  
	- Functions: cell process, immune process, precursor lipid
12. **PA: phosphatic acids**: glycerol backbone with two fatty acid chains 
	- Functions: cell signaling, metabolic regulation, precuror to TG's, pH sensing 
13. **PC: phosphatidylcholines**: glycerol backbone with two fatty acids and phosphoryl choline group 
	- Functions: facilitates TG transport for metabolism, membrane structure 
14. **PE: phosphatidylethanolamines**: glycerol backbone with two fatty acid chains and phosphate group
	- Functions: protein assembly, chaperone, mitochondrial stability
15. **PG: phosphatidyl glycerols**: glycerol-3-phosphate bonded to two fatty acids	
	- Functions: membrane fluidity 
16. **PI: phosphoinositides**: glycerol backbone with 2 fatty acids and myo-inositol ring
	- Functions: signaling and ion transport
17. **PS: phosphoserines**: serine with phosphate group, carboxyl group, and hydroxyl group
	- Functions: cell division, transduction, signaling, homeostasis
18. **SM: sphingomyelin**: sphingosine backbone, fatty acid, and phosphocholine
	- Functions: membrane structure, cholesterol metabolism, ion channels
19. **TG: triacylglycerides**: glycerol backbone with 3 fatty acids
	- Types: simple/monoacid (one type of FA; rare), diacid (two types of FAs), triacid (three types of FAs), and mixed (2-3 types of FAs; most common)
	- Saturation: Also characterized by the saturation of consituent fatty acids including saturated, monounsaturated, and polyunsaturated as detailed above.  
	- Functions: energy storage 

*Note that after filtering, some of these classes were not analzyed. Only classes and lipids that were quantified in all samples were kept in the dataset. This is because we cannot distinguish a true 0 concentration from a concentration that is just below detection limits of the instruments.*  

## 1. Lipid species concentration

I conducted the following steps to prepare the data for analysis:  

1. Subtract the concentration (units of `nmol.g_plasma` normalized to internal standards) in the blank sample from the concentration of every lipid species in the biological samples. 
2. Remove lipids that were not quantified in *all* biological samples. We cannot distinguish a lipid being completely absent (true 0) with lipids that were below the detection limit, but are still technically present. Therefore, we remove any lipid that does not have a quantifiable value in every sample.  
3. Remove any lipid that had a value <0 after blank correction.  
4. Remove any lipid that was not measured in pooled biological control samples (PBQCs). There were no lipids removed in this step.  
4. Normalize to sample volume and tissue input using the formula: `nmol.g_plasma.ug_protein=(nmol.g_plasma.corr*25)/protein.ug)`. Concentration is now in units of nmol lipid per ug protein (measured using BCA assay).  

After the filtering steps, we retained 467 lipids. This is expected and within the range of lipids detected in my other lipidomics projects. 

### **Total lipid concentration**: sum of all lipid concentrations in each sample 

There is a significant effect of species and there is a decrease in total lipid content over time in Pocillopora. Porites and Acropora total lipid is stable over time.  

Pocillopora had more total lipid content than Acropora and Porites at TP1. Values are not different during other time points. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/total-lipid.png?raw=true)

### **Lipid quantification of individual lipid species**: concentration of each individual lipid in the sample normalized to total protein 

There is a significant difference in multivariate lipid concentration profiles between species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-pca.png?raw=true)

There is no significant difference in multivariate lipid concentration profiles between time points in Acropora. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-acr-pca.png?raw=true)

There was a significant effect of time in Pocillopora. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-poc-pca.png?raw=true)

There was no effect of time in Porites.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-por-pca.png?raw=true)

### **Differential lipids between species**: Identifying lipids that are different in concentration between species using supervised methods 

A heatmap of concentration of individual lipid species shows that groups of lipids are higher in each species. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-heatmap.png?raw=true)

Lipid profiles are different between each species. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-plsda.png?raw=true)

The lipids driving these differences (Variable of Importance in Projection, VIPs) include primarily TG lipids as well as some FFAs.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-vips.png?raw=true)

Because there was a difference in lipid profiles in Pocillopora between time points (above), I looked at VIPs driving these differences. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-poc-plsda.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-poc-vips.png?raw=true)

There are some LCP, FFA, TG, and DG lipidscontributing to differences between time points. TGs account for most VIPs. 

The abundance plots of these VIPs show some lipids are higher at particular time points. For example, FFA 22:2 is higher at TP3 than other time points in Pocillopora.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-quant-poc-vips-plots.png?raw=true)

## 2. Lipid species composition within class  

I then manually generated lipid species composition for each class from the filtered data produced in Step 1. 

This was calculated by extracting the class prefix from each lipid and calculating the sum of all lipids for each class for each sample. Then, each individual lipid was divided by the total lipid in its respective class. This generated percent composition for each lipid within its class.  

### **Lipid composition of individual lipid species as relative abundance in their respective class**

Lipid composition is highly differential between species.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-comp-pca.png?raw=true)

There was no effect of time point on multivariate lipid composition in any species. However, just because the multivariate response does not change does not mean that no individual lipid changes over time. We will use other methods to see if individual lipids do change over time in a subsequent analysis.    

Acropora: 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-comp-acr-pca.png?raw=true)

Pocillopora:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-comp-poc-pca.png?raw=true)

Porites:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-comp-por-pca.png?raw=true)

### **Identify differential lipids as percent composition between species**

There are groups of lipids that have greater relative contribution to their respective class in each species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-comp-heatmap.png?raw=true)

I used PLSDA analyses to identify VIP lipids. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-comp-plsda.png?raw=true)

TG lipids made up the majority of VIP lipids that distinguish between species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/lipid-comp-vip.png?raw=true)

### **Examine percent composition of types of free fatty acids (FFAs) between species and timepoints**  

I separated fatty acids into saturated, monounsaturated, and polyunsaturated groups and looked at percent composition of each FFA within these groups.  

#### **Polyunsaturated fatty acids**  

There are significant differences in composition of PUFAs between species and there is a significant change in PUFA composition over time within all three species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/pufa-comp-plot.png?raw=true)

#### **Monounsaturated fatty acids**  

There are significant differences in composition of MUFAs between species but there is no change over time within any species. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/mufa-comp-plot.png?raw=true)

#### **Saturated fatty acids**  

There are significant differences in composition of saturated FAs between species there is a significant change in saturated FA composition over time within all three species. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/sfa-comp-plot.png?raw=true)

### **Examine percent composition of types of triacylglyceride (TG) fatty acids between species and timepoints**  

Triacylglyceride lipids (TGs) contain fatty acids. I also looked at the composition of different types of constituent fatty acids in TG lipids.   

#### **Polyunsaturated TGs**  

There are significant differences in composition of TG PUFAs between species but there is no change over time within any species. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/pufaTG-comp-plot.png?raw=true)

#### **Monounsaturated TGs**  

There are significant differences in composition of TG MUFAs between species and there is a significant effect of time only in Pocillopora.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/mufaTG-comp-plot.png?raw=true)

#### **Saturated TGs**  

There are significant differences in composition of TG saturated FAs between species but there is no change over time within any species. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/satTG-comp-plot.png?raw=true)

We can dig deeper into these results to look at specific types of FFAs and TGs and their role in metabolism.  

## 3. Total concentration of each lipid class 

I conducted the following steps to prepare the data for analysis:  

1. Subtract the concentration (units of `nmol.g_plasma` normalized to internal standards) in the blank sample from the concentration of every class concentration in the biological samples. 
2. Remove classes that were not quantified in *all* biological samples. We cannot distinguish a class being completely absent (true 0) with classes that were below the detection limit, but are still technically present. Therefore, we remove any class that does not have a quantifiable value in every sample. 
3. Remove any class that had a value <0 after blank correction.  
4. Normalize to sample volume and tissue input using the formula: `nmol.g_plasma.ug_protein=(nmol.g_plasma.corr*25)/protein.ug)`. Concentration is now in units of nmol class lipid per ug protein.  

After the filtering steps, we retained 6 classes out of 20. This is expected and included the most abundant classes such at TGs, FFAs, and DGs.  

### **Total class concentration**: sum of all class concentrations in each sample 

There is a significant difference in multivariate class concentrations between species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-quant-pca.png?raw=true)

There was no significant difference across time points in multivariate class concentrations for any species. This does not mean that some individual classes may be changing and we can use different model approaches to examine each individual class.  

Acropora:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-quant-acr-pca.png?raw=true)

Pocillopora:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-quant-poc-pca.png?raw=true)

Porites:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-quant-por-pca.png?raw=true)

### **Examine differential class composition over time and between species**  

I then viewed the concentration of each class over time for each species.  

There are significant differences in TG and FFA concentrations between species. There is no difference in the other classes. This makes sense with the lipid concentration and composition results from above. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-quant-plot.png?raw=true)

- In Acropora, there was no difference in lipid class concentration over time.  

- In Pocillopora, there was significantly higher TG concentration at TP1 than at TP3 and TP4 (decreases in TG over time).  

- In Porites, there was no difference in lipid class concentration over time.  

## 4. Composition of each class as a proportion of total lipids 

I next calculated the percent composition of each class by calculating the total lipid as a sum of all class concentrations for each sample. Then, the concentration of each class was divided by the total to get composition (x 100 to get a percent).  

### **Class composition of total lipids** 

There was a significant difference in multivariate class composition between species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-comp-pca.png?raw=true)

Acropora: 

There is no effect of time in Acropora.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-comp-acr-pca.png?raw=true)

Pocillopora: 

There was an effect of time in Pocillopora.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-comp-poc-pca.png?raw=true)

Porites:  

There was no effect of time in Porites.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-comp-por-pca.png?raw=true)

### **Examine differential classes** 

I then plotted the percent composition of each class over time for each species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/class-comp-plot.png?raw=true)

There was a significant difference in the percent composition of TGs and FFAs to the total lipid pool between species. 

In Acropora, there was a significant change in TGs and FFAs over time. TGs are higher in TP1 than in TP3 and TP4. FFAs are lower at TP1 than at TP3 and TP4. There may be a shift in energy storage (TGs) and free fatty acids between TP1 and TP3/TP4. FFAs are 50-60% of total lipids while TGs are 20-40% of total lipids in Acropora.  

There was also a significant difference over time for TGs and FFAs in Pocillopora, and the results are similar to what we see in Acropora.  

Specifically, TGs are higher in TP1 than in TP3 and TP4. FFAs are lower at TP1 than at TP3 and TP4. There may also be a shift in energy storage (TGs) and free fatty acids between TP1 and TP3/TP4. FFAs are 30-50% of total lipids while TGs are 40-60% of total lipids in Pocillopora.   

Finally, there was a significant effect of time in Porites. TGs are higher at TP1 and TP2 than TP3. TGs are higher at TP2 than TP4, but are lower at TP3 than TP4. TGs are higher during warmer periods, and lower during cooler periods, especially at TP3. FFAs are lower at TP1 and TP2 than at TP3. TP2 was also lower than TP4. FFAs are lower in the warm period and higher in the cool period.  

These results will be interesting for us to examine further.  

## 4. Concentration of constituent fatty acids in each lipid class 

Most lipid classes contain 1 or more fatty acids in their structure. This analysis examines the composition of the fatty acids within each lipid class. See analysis descriptions at the top of this post for further explanation.  

I conducted the following steps to prepare the data for analysis:  

1. Subtract the concentration (units of `nmol.g_plasma` normalized to internal standards) in the blank sample from the concentration of every fatty acid type in the biological samples. 
2. Remove fatty acides that were not quantified in *all* biological samples. We cannot distinguish a fatty acid being completely absent (true 0) with fatty acids that were below the detection limit, but are still technically present. Therefore, we remove any compound that does not have a quantifiable value in every sample.  
3. Remove any fatty acid that had a value <0 after blank correction.  
4. Remove any fatty acid that was not measured in pooled biological control samples (PBQCs). There were no fatty acids removed in this step.  
4. Normalize to sample volume and tissue input using the formula: `nmol.g_plasma.ug_protin=(nmol.g_plasma.corr*25)/protein.ug)`. Concentration is now in units of nmol lipid per ug protein (measured using BCA assay).  

After the filtering steps, we retained 72 individual fatty acids to examine. 

### **Fatty acid concentration of individual fatty acids**

There was a significant difference in multivariate fatty acid concentrations between species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-quant-pca.png?raw=true)

There was no effect of time on multivariate fatty acid profiles in Acropora, Pocillopora, or Porites.  

Acropora:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-quant-acr-pca.png?raw=true)

Pocillopora:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-quant-poc-pca.png?raw=true)

Porites:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-quant-por-pca.png?raw=true)

### **Identify differential fatty acids driving species differences**

I then used a PLSDA and VIP analysis to identify lipids that contribute to differences between species fatty acid profiles.  

There are groups of fatty acids that are higher in each species.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-quant-heatmap.png?raw=true)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-quant-plsda.png?raw=true)

Fatty acids contributing to differences between species include fatty acids within CE, TG, LPC, Cer, TG, and free FAs. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-quant-vips.png?raw=true)

## 4. Composition of constituent fatty acids in each lipid class 

I then calculated the percent composition of each individual fatty acid to the total pool of fatty acids in the respective class.   

### **Fatty acid percent composition**  

There is a significant effect of species on percent composition of fatty acids.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-pca.png?raw=true)

There was no effect of time on fatty acid percent composition in Acropora. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-acr-pca.png?raw=true)

There was a significant effect of time on fatty acid percent composition in Pocillopora.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-poc-pca.png?raw=true) 

There was a significant effect of time on fatty acid percent composition in Porites.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-por-pca.png?raw=true) 

### **Examine differential percent composition of fatty acids** 

Differential fatty acids contributing to differences between species include fatty acids contained in TG, DG, free FA, and LPC lipids.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-plsda.png?raw=true) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-vips.png?raw=true)

Because unsupervised analyses showed differences in composition between timepoints in Pocillopora and Porites, I examined differential fatty acids in these species.  

In Pocillopora, differential fatty acids included those contained in TG, free FAs, CE, DG, and LPC lipids.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-plsda-poc.png?raw=true) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-vips-poc.png?raw=true)

These fatty acids look like this when we view concentration:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-poc-vips-plot.png?raw=true)

In Porites, differential fatty acids also included those contained in TG, free FAs, CE, DG, and LPC lipids.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-por-plsda.png?raw=true) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-vips-por.png?raw=true)

These fatty acids look like this when we view concentration:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/20250315/fa-comp-vips-por-plot.png?raw=true)

## Overall findings 

- Triacylglycerides and Fatty acids drive the strong differences that we see between species 
- Each species has a different response to time in their lipid profiles
- The composition of TGs and FFAs changes between species and across time 

## Next steps

- Conduct analyses to examine the changes we saw in FFA and TGs in species over time 
- Examine if there are site effects 
- Conduct SAM analyses or ML analyses to look at differential lipids  
