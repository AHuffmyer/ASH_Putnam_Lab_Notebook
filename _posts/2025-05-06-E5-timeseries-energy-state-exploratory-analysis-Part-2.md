---
layout: post
title: E5 timeseries energy state exploratory analysis Part 2
date: '2025-04-24'
categories: E5
tags: R Lipidomics Lipids Metabolomics Multivariate Pocillopora Porites Acropora  
---

This post details preliminary data analysis to identify lipidomic and metabolomic indicators of coral energetic state for the E5 Modeler Team.  

# Overview 

In this analysis, I am using lipidomic and metabolomic signatures in the literature to evaluate whether these signatures can be used in the E5 Modeler Team efforts to model coral energetic state in the E5 project.  

This post focuses on lipid and metabolite indicators. See Kathleen Durkin's [notebook](https://shedurkin.github.io/Roberts-LabNotebook/posts/projects/E5_coral.html) and EPIMAR talk for predication of energetic state gene expression from epigenetic marks.  

# tl;dr 

#### Useful lipidomic indicators:  

- **PUFA:SFA ratio**: Seems to be an interesting indicator of autotrophy/heterotrophy and follows expected seasonal patterns of higher autotrophy in the warm season and higher heterotrophy in the winter season in Porites. 
- **EPA:DHA ratio**: May be a useful indicator of reliance on autotrophy and is higher in Acropora than the other species.   
- **Free fatty acid concentrations**: There may be a few FFAs that are interesting as symbiosis markers that do show some differences across time/species, although results are variable.  
- **FFA:TAG ratio**: Ratio of FFA to TAGs may provide an indicator of relative amount of stored reserves. Ratios are higher in Acropora, indicating more free fatty acids compared to stored reserves.  
- **Total TAG lipids**: The total concentration of TAGs provides an indication of storage lipids. There are lower TAGs in Acropora and TAGs change over time in Porites and Acropora.  
- **Total FFA lipids**: The total concentration of FFAs changes across time and species with higher values in Acropora. In Acropora and Porites, FFAs change seasonally and may provide an indication of active energy availability, nutritional exchange, or lipid catabolism.  

#### Useful metabolomic indicators:  

- **Glucose and glycolysis intermediates**: There are seasonal influences on glucose and glycolysis intermediates. The availability of energy via glucose would be a useful energetic indicator.  
- **Glutamine and glutamate**: These metabolites had seasonal and species influences and may point to shifts in nitrogen metabolism to maintain symbiosis.  
- **SAM (methyl donor)**: There are seasonal shifts in SAM and we may be interested in relationships with epigenetic markers.  

# Lipidomic analyses 

From the literature, I identified several lipid indicators to try:  

1. Concentration of palmitic acid (C16:0) 
	- Higher = more in homologous symbiotic relationship
	- Matthews et al. 2018
2. Concentration of oleic acid (C18:1n-9) 
	- Higher = more in homologous symbiotic relationship
	- Matthews et al. 2018
3. Concentration of linoleic acid (C18:2n-6) 
	- Higher = more in homologous symbiotic relationship
	- Matthews et al. 2018
4. PUFA:SFA ratio 
	- Higher ratio = more feeding/heterotrophy, lower ratio = more autotrophic/starved
	- Kim et al. 2021
5. EPA (20:5n-3):DHA (22:6n-3) ratio 
	- Higher ratio = more autotrophic
	- Kim et al. 2021
6. Concentration of 18:3n-6 
	- Symbiosis marker
	- Radice et al. 2019
7. Concentration of 18:4n-3 
	- Symbiosis marker
	- Radice et al. 2019
8. Concentration of 20:4n-6
	- Heterotrophy marker
	- Radice et al. 2019
9. Total TAG lipids 
	- Total storage lipids in triacylglycerides 
10. Total free fatty acids 
	- Total free fatty acids 
11. Total lipids 
	- Total lipids from all classes 
12. FFA:TAG ratio 
	- Ratio of free fatty acids to triacylglycerides. 
	- Higher ratio indicates more free fatty acids/less storage lipids 

I calculated these metrics and then plotted the change in these markers across time points in the E5 corals.  

Note that in this analysis we only have timepoint 2 (March) data for Porites due to a lack of samples. For Acropora and Pocillopora, we only have timepoints 1 (Jan), 3 (Sept), and 4 (Nov).  

In the plots below, the solid dark lines represent the mean for each species across time with the lighter points and lines representing individual colonies.  

### Concentration of palmitic acid (C16:0)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/palmitic-acid.png?raw=true)

There is a significant effect of time and species on palmitic acid concentrations (as a % of all free fatty acids) and concentrations are higher in Porites. In Porites, palmitic acid is significantly higher at TP2 than TP4.  

Higher concentrations of palmitic acid have been seen in homologous vs heterologous symbiotic associations (see literature notes above), and may indicate higher translocation/nutritional exchange.  

### Concentration of oleic acid (C18:1n-9)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/oleic-acid.png?raw=true)

There is a significant difference between species, with higher concentrations of oleic acid in Porites. There is no difference between time points.  

This metric may not be informative for our use because concentrations are stable across time.  

### Concentration of linoleic acid (C18:2n-6) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/linoleic-acid.png?raw=true) 

There is a significant effect of time and species on linoleic acid concentrations (as a % of all free fatty acids) with concentrations higher in Porites. In Porites, linoleic acid is significantly higher at TP2 than TP4.  

Higher concentrations of linoleic acid have been seen in homologous vs heterologous symbiotic associations (see literature notes above), and may indicate higher translocation/nutritional exchange.  

### PUFA:SFA ratio 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/pufa-sfa.png?raw=true)

I calculated the ratio of PUFA (% composition of total fatty acids) to SFA (% composition of total fatty acids) concentrations. 

A higher ratio of PUFA:SFA may indicate higher contribution of heterotrophic feeding (see literature above).  

There is a significant species x timepoint interaction. The ratio is higher in Pocillopora, indicating it is more heterotrophic/has more feeding activity. In Porites, this ratio appears to increase in Sept-Nov (more feeding) and is lower in Jan-March (more autotrophy).   

### EPA (20:5n-3):DHA (22:6n-3) ratio 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/epa-dha-ratio.png?raw=true) 

There is a significant effect of species, but not time. Values are higher in Acropora, which may indicate a higher reliance on autotrophy.  

### Concentration of 18:3n-6 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/fa-18-3.png?raw=true) 

There is a significant difference between species, but not time, with lower values in Pocillopora. A lower concentration may indicate reduced symbiotic exchange (see literature review above).  

### Concentration of 18:4n-3 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/fa-18-4.png?raw=true) 

There is a significant interactive effect of species x time in 18:4 concentrations. Values are higher at TP1 for Pocillopora.  

### Concentration of 20:4n-6

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/fa-20-4.png?raw=true) 

There is a significant species and timepoint effect. Acropora has higher concentration at TP1 and Porites has a higher concentration at TP4.  

### Total TAG lipids 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/tags.png?raw=true) 

Total concentration of TAG (storage) lipids is affected by species and time. TAG concentrations are higher at TP1 in Acropora, stable in Pocillopora, and higher at TP1 and TP2 in Porites.  

### Total FFAs 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/ffas.png?raw=true) 

There is a significant effect of time and species on total free fatty acid concentrations. FFAs are higher in Acropora at TP3 and are higher in Porites at TP3 as well. There is a reciprocal relationship between FFAs and TAGs and may indicate amount of active energy availability, nutritional exchange, or lipid catabolism.    

### Total lipid pool 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/total-lipid.png?raw=true) 

There is a significant time x species effect on total lipid pools. The total lipid pool includes signaling, membrane, storage, and fatty acid lipids. It may provide an indication of total stored/active lipid availability, but due to the many different types of lipids it is more useful to look at specific classes. Total lipids are higher in Pocillopora and decrease across time. Total lipids are stable in Acropora and Porites. 

### FFA:TAG ratio  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/ffa-tags-ratio.png?raw=true) 

There is a significant effect of species and time on FFA:TAG ratios. The ratio of free fatty acids to triacylglycerides is higher in Acropora, indicating it has more FFA relative to stored lipids and therefore higher levels of lipid breakdown. In contrast, Porites is lower, indicating higher stability of stored lipids. Values are higher (more lipid catabolism) at TP3 in Acropora.  

# Metabolomic analyses 

I will examine the following metabolites that were detected in our samples, passed filtering thresholds, and are potential energy indicators:  

- Glucose (translocation, glycolysis)
- G1P/F1P/F6P (glycolysis)
- G6P (glycolysis)
- Glutamine (nitrogen metabolism)
- Glutamic acid (nitrogen metabolism)
- Glycerol-3-P (central metabolism)
- Inositol (signaling)
- Octanoyl-L-Carnitine (8:0 Carnitine) (fatty acid metabolism)
- S-Adenosylmethionine (SAM) (methylation methyl donor)

### Glucose (translocation, glycolysis)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/glucose.png?raw=true) 

There was a significant species x time effect on glucose concentrations. In Porites, glucose was elevated at TP2 but was stable in Pocillopora. Glucose was elevated in Acropora at TP3. Higher glucose may indicate elevated translocation rates.  

### G1P/F1P/F6P (glycolysis)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/intermediates1.png?raw=true) 

There was a significant species x time effect on glycolysis intermediates including G1P/F1P/F6P. Concentrations were higher at TP1 and TP3 in Acropora with low levels at TP4. Concentrations were lower at TP1 in Pocillopora, but no difference was detected in Porites. Changes may indicate variation in central metabolic rates.  

### G6P (glycolysis)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/intermediates2.png?raw=true) 

No differences were detected in G6P in any species. There are potential outliers to examine.    

### Glutamine (nitrogen metabolism)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/glutamine.png?raw=true) 

There was a significant species x time effect with concentrations of glutamine higher at TP4 in Porites, but stable in Acropora and Pocillopora. This may indicate seasonal changes in nitrogen metabolism in Porites.      

### Glutamic acid (nitrogen metabolism)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/glutamate.png?raw=true) 

There was a significant effect of species but not time on glutamate concentrations. Concentrations were lower in Porites. This may indicate differences in nitrogen metabolism in Porites. 

### Glycerol-3-P (central metabolism)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/glycerol.png?raw=true) 

Glycerol-3-phosphate is involved in central carbon and lipid metabolism. There was a significant effect of species but no time effects. Concentrations were higher in Pocillopora.  

### Inositol (signaling)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/inositol.png?raw=true) 

There was a significant effect of species with higher concentrations in Porites. There are no time effects.  

### Octanoyl-L-Carnitine (8:0 Carnitine) (fatty acid metabolism)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/carnitine.png?raw=true) 

Carnitines can be involved in fatty acid transport during beta oxidation. There was a significant effect of species with far lower values in Pocillopora. No time effects.   

### S-Adenosylmethionine (SAM) (methylation methyl donor)

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/modelers/sam.png?raw=true) 

SAM values were affected by species x time effects with concentrations stable in Porites and Pocillopora. Values were higher at TP1 in Acropora and decreased over time.  
