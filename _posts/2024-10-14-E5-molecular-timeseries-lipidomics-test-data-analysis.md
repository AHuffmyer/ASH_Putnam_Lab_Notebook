---
layout: post
title: E5 molecular timeseries lipidomics test data analysis
date: '2024-10-14'
categories: E5
tags: Lipidomics Lipids R
---

This post details analysis of test lipidomic samples for the E5 timeseries molecular project.  

## Overview 

In this project, we are conducting multi-omics analyses of the E5 physiology time series project. We sent 4 test samples to the University of Washington Northwest Metabolomics Center in Seattle to see if lipidomics and metabolomics is feasible for these samples.  

Lipidomics data has been returned and in this post I detail my overview analysis of this data to see if the data are of high quality and are as we expect for coral samples.  

The repository [for this project is on GitHub](https://github.com/urol-e5/timeseries_molecular) and the script used to produce the results described here [can be found on GitHub using this direct link](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/03-test-lipidomics.Rmd).  

The test samples included 2 Acropora (ACR-140 and ACR-165), 1 Pocillopora (POC-68) and 1 Porites (POR-70). All are from the Manava site. 79 and 140 are from time point 2 and 165 and 68 are from time point 1.  

We submitted these samples for targeted lipidomic analyses at UW and they provided the data, which is [on GitHub here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/data/lipidomics/test_QuantLipids.xlsx).  

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

I will focus on data types 1-4 in this post. The fatty acid data was also analyzed, but it requires more in depth analyses to gain biologically meaningful results given the complexity of fatty acids in each lipid class.  

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

## 1. Lipid species concentration

I conducted the following steps to prepare the data for analysis:  

1. Subtract the concentration (units of `nmol.g_plasma` normalized to internal standards) in the blank sample from the concentration of every lipid species in the biological samples. 
2. Remove lipids that were not quantified in *all* biological samples. We cannot distinguish a lipid being completely absent (true 0) with lipids that were below the detection limit, but are still technically present. Therefore, we remove any lipid that does not have a quantifiable value in every sample.  
3. Remove any lipid that had a value <0 after blank correction.  
4. Normalize to sample volume and tissue input using the formula: `nmol.g_plasma.mg_tissue=(nmol.g_plasma.corr*25)/tissue.mg)`. Concentration is now in units of nmol lipid per mg tissue.  

After the filtering steps, we retained 581 lipids out of 1561. This is expected and within the range of lipids detected in my other lipidomics projects. 

I then plotted a stacked bar plot to show the concentration of all lipids for each sample.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/total_lipid_content.png?raw=true) 

There is variation in total lipid by sample, which is expected. It is interesting that the two Acropora samples vary quite a bit. I expect total lipid content to be variable by time point, site, and colony ID.  

Because there was more total lipid in ACR 140, this drives variation between ACR lipid concentrations seen in plots below. However, lipid profiles (lipid compositions) are highly similar. I have detailed this below.  

Note that concentrations are in number of lipid molecules per weight of tissue - lipids here are not measured in weight but rather by number of molecules.  

I then plotted a PCA of multivariate lipid compositions. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/lipid_species_quant_pca.png?raw=true) 

You can see variation in lipid concentrations between all samples (including the variation between the two Acropora samples).  

## 2. Lipid species composition within class  

I then manually generated lipid species composition for each class from the filtered data produced in Step 1. 

This was calculated by extracting the class prefix from each lipid and calculating the sum of all lipids for each class for each sample. Then, each individual lipid was divided by the total lipid in its respective class. This generated percent composition for each lipid within its class.  

I first looked at a PCA.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/lipid_species_comp_pca.png?raw=true) 

I found that the lipid profiles of percent composition were very similar between Acropora samples and quite divergent between each species (ACR, POC, and POR are all separated here).  

This makes sense and is a good QC check for us.  

I then wanted to look in more detail at specific classes of lipids, specifically the Free Fatty Acids and Triacylglycerols, since these are important for energy metabolism.  

### Free fatty acids 

I separated fatty acids into saturated, monounsaturated, and polyunsaturated groups and looked at percent composition of each FFA within these groups.  

Saturated free fatty acids:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/saturated-free-fatty-acid_composition.png?raw=true) 

- Pocillopora had lower saturated free fatty acids than the other species

Monounsaturated free fatty acids:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/mono-unsaturated-free-fatty-acid_composition.png?raw=true) 

- Pocillopora again had lower MUFA than the other species. 
- Porites had more MUFA overall. 
- Pocillopora has more FA 18:1 and 24:1 than other species specifically.  

Polyunsaturated free fatty acids:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/poly-unsaturated-free-fatty-acid_composition.png?raw=true) 

- Interesting! Pocillopora had higher PUFA than the other species. 
- Porites noteably has fewer of 20:4, which is arachidonic acid. This is a common coral FFA. 
- Pocillopora has more 22:6 (DHA), which has been proposed as a marker of Durusdinium. This matches with species composition of these corals.  

### Triacylglycerides

I also looked at TG's by the saturation of their constituent fatty acids.  

Saturated TGs:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/saturated-TG-fatty-acid_composition.png?raw=true) 

- Lower saturated fatty acids in TGs in Porites. 

Monounsaturated TGs:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/mono-unsaturated-TG-fatty-acid_composition.png?raw=true)

- More MUFA fatty acid TGs in Porites.  

Polyunsaturated TGs:  
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/poly-unsaturated-TG-fatty-acid_composition.png?raw=true)

- Less PUFA fatty acids in TGs in Pocillopora.  

These results show we are possibly detecting some changes in the stored lipid pool in our species, which is interesting! 

## 3. Class concentration 

I conducted the following steps to prepare the data for analysis:  

1. Subtract the concentration (units of `nmol.g_plasma` normalized to internal standards) in the blank sample from the concentration of every class concentration in the biological samples. 
2. Remove classes that were not quantified in *all* biological samples. We cannot distinguish a class being completely absent (true 0) with classes that were below the detection limit, but are still technically present. Therefore, we remove any class that does not have a quantifiable value in every sample. This removed classes LacCER, HexCER, and LPS.  
3. Remove any class that had a value <0 after blank correction.  
4. Normalize to sample volume and tissue input using the formula: `nmol.g_plasma.mg_tissue=(nmol.g_plasma.corr*25)/tissue.mg)`. Concentration is now in units of nmol class lipid per mg tissue.  

After the filtering steps, we retained 15 classes out of 20. 

I then plotted a PCA to look at multivariate class composition.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/class_quant_pca.png?raw=true) 

Acropora samples were divergent again, due to higher lipid content in ACR 140.  

I also viewed the absolute concentration of each lipid class.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/class_concentration.png?raw=true) 

From this, we can see that FFAs and TGs are most abundant. PC and PE are the next most abundant. 

I then allowed `scales = "free"` to get a closer look at the less abundant lipids.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/class_concentration-free-scales.png?raw=true) 

There was variation in class concentration across samples.  

We can test specific hypotheses about lipid concentration in this project as it relates to physiology and sequencing data.  

## 4. Class composition 

I next calculated the percent composition of each class by calculating the total lipid as a sum of all class concentrations for each sample. Then, the concentration of each class was divided by the total to get composition (x 100 to get a percent).  

I first viewed as overall composition of each class.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/class_composition_overall.png?raw=true)

- Pocillopora  has more TG and Porites has fewer TG by composition. There are also fewer PC in Pocillopora and more PE in Porites. 

I viewed a PCA of lipid class profiles.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/class_composition_pca.png?raw=true)

As seen before, Acropora lipid profiles are very similar with Pocillopora and Porites samples clearly separated.  

I then viewed the composition of lipids in each class for each sample.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/class_composition.png?raw=true)

- FFAs and TGs were the highest percent composition followed by PE and PC

I also viewed with free scales to look at the less abundant lipids. 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_molecular/lipids/test/class_composition-free-scales.png?raw=true)

## Overall findings 

- Lipid composition is as expected for coral samples and we could detect a decent number of lipid species in our samples. 
- There are hints and interesting differences between coral species, but we will need to wait on full analysis to look at this further. 
- TGs and FFAs comprise 10-40% of the total lipid pool, which is an expected range.  
- FFA's characterized in corals previously (e.g., arachadonic acid) show up in our data. 

I think we should give the green light to proceed with full lipidomics analysis of all samples.  

We need to analyze the metabolomic tests to confirm that the metabolomic analyses worked as well. We are waiting on this data and it should be available shortly.  

## To do for full round of the analysis 

- Submit PBQC samples (at least n=3) as a QC check for full analysis. This way, we will only keep lipids that are present in the samples as well as the PBQC to validate quantification.  
- Quantify tissue input as we did for this round. 
- Keep samples as cold as possible during the entire process of preparation.  
- Full analysis will include unsupervised and supervised approaches to characterize lipid profiles across species, site, and time. 
