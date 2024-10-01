---
layout: post
title: Preliminary lipidomic and metabolomics analysis of Hawaii 2023 larval data
date: '2024-09-30'
categories: Larval_Symbiont_TPC_2023 Analysis
tags: Lipidomics Lipids Metabolomics Mcapitata Multivariate R
---

This post details preliminary analysis of lipidomic and metabolomic data for the Hawaii 2023 larval project. 

# Overview 

This post details lipidomic and metabolomic analyses for the *Montipora capitata* 2023 larval thermal tolerance project in collaboration with Jennifer Matthews at University of Technology Sydney. See my [notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#larval-symbiont-tpc-2023) and my [GitHub repo](https://github.com/AHuffmyer/larval_symbiont_TPC) for information on this project.

Lipidomics and metabolomic data were obtained through a tri-extraction process to obtain lipid, metabolite, and protein pellet fractions from each sample. Lipids were analyzed using LCMS at University of Technology Sydney and metabolomics were analyzed via GCMS at Metabolomics Australia. See my [notebook posts](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/categoryview/#larval-symbiont-tpc-2023) for more information.      Protein was quantified using a Bradford assay and is used for normalization.  

Files for this project include lipidomic and metabolomic files for 54 samples. There are three parental phenotypes of these larvae: Nonbleached (from parents that were unbleached during a previous heat wave), bleached (from parents that bleached and recovered during a previous heat wave), and wildtype (collected as a sample from the natural reef). Those from bleached parents are dominated by *Cladocopium* symbionts and those from wildtype and non-bleached colonies have a mixture of *Cladocopium* and *Durusdinium* symbionts.  

Each of these parental phenotypes was exposed to 27, 30, or 33°C for 4 hour incubations at 9 days post fertilization. We have n=6 biological replicates per parental phenotype x temperature treatment. There are no random effects, temperature exposures were conducted in water baths in sealed glass jars with n=1 per temperature. Environmental data for these incubations is in my [GitHub repo](https://github.com/AHuffmyer/larval_symbiont_TPC).  

At this point in analysis, metabolomics data has undergone initial QC checks and curation (J. Matthews) and lipidomics data underwent peak identification and selection in MS-DIAL (A. Huffmyer). All data are available on [GitHub here](https://github.com/AHuffmyer/larval_symbiont_TPC/tree/main/data/lipids_metabolites).  

In this analysis, I analyzed peak area as well as peak height for lipidomics. The entry below contains full scripts for peak area as well as metabolomic analyses. I have added a section for analysis of peak height that just contains the output figures so that we can view any differences between results for height and area.  

# Lipidomics (Peak area)  

```
library(tidyverse)
library(stringr)
library(readxl)
library(purrr)
library(lubridate)
library(ggplot2)
library(broom)
library(cowplot)
library(mixOmics)
library(dplyr)
library(conflicted)
library(vegan)
library(factoextra)
library(ggfortify)
library(RVAideMemoire)
library(ComplexHeatmap)
library(circlize)
library(tibble)
library(viridis)
library(genefilter)
library(UpSetR)

conflict_prefer("select", winner = "dplyr", quiet = FALSE)
conflict_prefer("filter", winner = "dplyr", quiet = FALSE)
conflict_prefer("rename", winner = "dplyr", quiet = FALSE)
```

## Data manipulation and preparations  

### Read in all files 

```
metadata<-read_csv("data/lipids_metabolites/lipidomics/lipid_metadata_huffmyer_matthews.csv")%>%
  rename(sample=`Vial ID`, tube=`Sample ID`, temperature=Temperature, parent=Parent, type=Type, code=Code)

neg_values<-read_csv("data/lipids_metabolites/lipidomics/processed-data/negative/PeakValues_2_2024_09_19_13_01_19.csv")

neg_peaks<-read_csv("data/lipids_metabolites/lipidomics/processed-data/negative/PeakMaster_2_2024_09_19_13_01_19.csv")

pos_values<-read_csv("data/lipids_metabolites/lipidomics/processed-data/positive/PeakValues_3_2024_09_19_14_24_22.csv")

pos_peaks<-read_csv("data/lipids_metabolites/lipidomics/processed-data/positive/PeakMaster_3_2024_09_19_14_24_22.csv")

protein<-read_csv("data/lipids_metabolites/protein/protein_calculated.csv")
```

Add protein data to the metadata.   

```
metadata$protein.ug<-protein$`Total Protein ug`[match(metadata$tube, protein$Sample)]
```

### Negative polarity 

How many peaks do we have?    

```
length(neg_peaks$`Metabolite name`)
```

162 peaks.   

Reformat data set to be useful.    

```
neg_peaks<-neg_peaks%>% 
  rename(peak=`Alignment ID`, lipid=`Metabolite name`, formula=Formula, ontology=Ontology)%>%
  select(peak, lipid, formula, ontology)%>%
  mutate(polarity=c("negative"))

neg_values<-neg_values%>%
  rename(peak=ID, file=File, group=Class, area=Area)%>%
  select(peak, file, group, area)%>%
  mutate(polarity=c("negative"))
```

The data frames now contain `peak` for a unique ID for each lipid detected. This number links the two files. Area represents the normalized area after internal standard normalization. We will further normalize this value below.    

Format the file names to correspond to sample IDs in the metadata sheet.   

```
#first remove the test PBQCs ran at 5 and 10 uL loadings and keep the standard 20 uL loading 
neg_values<-neg_values%>%
  filter(!file=="20240423_Mcap_neg_PBQC1_5")%>%
  filter(!file=="20240423_Mcap_neg_PBQC1_10")%>%
  mutate(file = str_replace(file, "20240423_Mcap_neg_PBQC1_20", "20240423_Mcap_neg_PBQC1"))%>%
  mutate(sample = str_extract(file, "[^_]+$"))
```

Merge together all data from the metadata, values, and peak information to one data frame.   

```
negative<-left_join(metadata, neg_values)

negative<-negative%>%
  select(sample, temperature, parent, type, peak, area, polarity, protein.ug)

negative<-left_join(negative, neg_peaks)
```

Next, add together the values for all peaks identified as the same metabolite for each sample.     

How many lipids have multiple peaks detected separately?   

```
length(unique(neg_peaks$lipid))
```

There are 162 unique lipids and 162 peaks.   

Show an example of entries that have the same metabolite with multiple peak IDs.   

```{r}
filtered_values <- negative %>%
  group_by(lipid) %>%
  filter(n_distinct(peak) > 1) %>%
  ungroup()%>%
  arrange(lipid, sample, peak)

print(filtered_values)
```

No duplicates.   

Remove PBQC's and Blanks. These were used for peak identification in MSDIAL and are not used downstream at this point.   

```
negative<-negative%>%
  filter(!type=="PBQC")%>%
  filter(!type=="Blank")
```

Conduct internal standard normalization.   

Peak 16879 (PG) will be used as our internal standard for negative polarity. It was consistent across samples (low fold change), high signal:noise, and is mid-retention.   

```
#pull out the name of the standard being used 
standard_neg<-neg_peaks%>%
  filter(peak=="16879")%>%
  pull(lipid)

#make a dataframe of standard values for each sample
standard_neg_value<-negative%>%
  filter(lipid %in% standard_neg)%>%
  group_by(sample)%>%
  summarise(area_standard=mean(area, na.rm=TRUE))

#add this value back into the master data frame for corrections. 
negative<-negative%>%
  left_join(.,standard_neg_value)
```

Remove sample 1, it did not have a signal for the internal standard and was an outlier in MS-DIAL analysis.   

```
negative<-negative%>%
  filter(!sample=="1")
```

Normalize to the standard.   

```
negative<-negative%>%
   mutate(area_corrected=area/area_standard)
```

Finally, normalize corrected area to sample protein. This will generate units of area / ug protein.   

```
negative<-negative%>%
  mutate(area_normalized=area_corrected/protein.ug)
```

Repeat for positive polarity.    

### Positive polarity 

How many peaks do we have?    

```
length(pos_peaks$`Metabolite name`)
```

896 peaks   

Reformat data set to be useful.    

```
pos_peaks<-pos_peaks%>% 
  rename(peak=`Alignment ID`, lipid=`Metabolite name`, formula=Formula, ontology=Ontology)%>%
  select(peak, lipid, formula, ontology)%>%
  mutate(polarity=c("positive"))

pos_values<-pos_values%>%
  rename(peak=ID, file=File, group=Class, area=Area)%>%
  select(peak, file, group, area)%>%
  mutate(polarity=c("positive"))
```

The data frames now contain `peak` for a unique ID for each lipid detected. This number links the two files. Area represents the normalized area after internal standard normalization. We will further normalize this value below.    

Format the file names to correspond to sample IDs in the metadata sheet.   

```
pos_values<-pos_values%>%
  mutate(sample = str_extract(file, "[^_]+$"))
```

Merge together all data from the metadata, values, and peak information to one data frame.   

```
positive<-left_join(metadata, pos_values)

positive<-positive%>%
  select(sample, temperature, parent, type, peak, area, polarity, protein.ug)

positive<-left_join(positive, pos_peaks)
```

Next, add together the values for all peaks identified as the same metabolite for each sample.     

How many lipids have multiple peaks detected separately?   

```
length(unique(pos_peaks$lipid))
```

There are 896 unique lipids and 896 total peaks.   

Show an example of entries that have the same metabolite with multiple peak IDs.   

```
filtered_values <- positive %>%
  group_by(lipid) %>%
  filter(n_distinct(peak) > 1) %>%
  ungroup()%>%
  arrange(lipid, sample, peak)

print(filtered_values)

levels(factor(filtered_values$lipid))
```

No duplicates    

Remove PBQC's and blanks. These were used for peak identification in MSDIAL and are not used downstream at this point.     

```
positive<-positive%>%
  filter(!type=="PBQC")%>%
  filter(!type=="Blank")
```

Conduct internal standard normalization.   

Peak 19029 (LPC) will be used as our internal standard for positive polarity. It was consistent across samples (low fold change), high signal:noise, and was the best representative from standards.   

```
#pull out the name of the standard being used 
standard_pos<-pos_peaks%>%
  filter(peak=="19029")%>%
  pull(lipid)

#make a dataframe of standard values for each sample
standard_pos_value<-positive%>%
  filter(lipid %in% standard_pos)%>%
  group_by(sample)%>%
  summarise(area_standard=mean(area, na.rm=TRUE))

#add this value back into the master data frame for corrections. 
positive<-positive%>%
  left_join(.,standard_pos_value)
```

Remove sample 1, it did not have a signal for the internal standard for negative and therefore is being removed from the data analysis.   

```
positive<-positive%>%
  filter(!sample=="1")
```

Normalize to the standard.   

```
positive<-positive%>%
   mutate(area_corrected=area/area_standard)
```

Finally, normalize corrected area to sample protein. This will generate units of area / ug protein.    

```
positive<-positive%>%
  mutate(area_normalized=area_corrected/protein.ug)
```

### Join polarities together 

Join positive and negative datasets.    

```
length(negative$lipid)
length(positive$lipid)

lipids<-rbind(negative, positive)

length(lipids$lipid)
```

Select polarity for peaks in both polarity files. How many lipids are detected at both polarities?   

```
filtered_values <- lipids %>%
  group_by(lipid) %>%
  filter(n_distinct(polarity) > 1) %>%
  ungroup()%>%
  arrange(lipid, sample, polarity)

print(filtered_values)

length(unique(filtered_values$lipid))
```

There are 40 lipids detected at both polarities. Select the polarity with the highest Area value for each lipid.   

```
# Identify lipids with both polarities
lipids_with_both_polarities <- lipids %>%
  group_by(lipid) %>%
  filter(n_distinct(polarity) == 2)

# Filter lipids with both polarities to keep only the one with the highest area
filtered_lipids <- lipids_with_both_polarities %>%
  group_by(lipid) %>%
  filter(area == max(area)) %>%
  ungroup()

# Identify lipids that only have one entry (i.e., do not have both polarities)
single_entry_lipids <- lipids %>%
  group_by(lipid) %>%
  filter(n_distinct(polarity) == 1) %>%
  ungroup()

# Combine filtered lipids and single entry lipids
lipids_filtered <- bind_rows(filtered_lipids, single_entry_lipids)
```

How many lipids are detected at both polarities?   

```
filtered_values <- lipids_filtered %>%
  group_by(lipid) %>%
  filter(n_distinct(polarity) > 1) %>%
  ungroup()%>%
  arrange(lipid, sample, polarity)

print(filtered_values)

length(unique(filtered_values$lipid))
```

Duplicates are now taken care of.     

View ontology categories available.    

```
levels(as.factor(lipids_filtered$ontology))

ontology<-lipids_filtered%>%ungroup()%>%select(lipid, formula, ontology)%>%unique()
```

 [1] "AHexCAS"    "AHexCer"    "AHexCS"     "AHexSIS"    "ASM"        "BileAcid"   "BMP"       
 [8] "BRSE"       "CAR"        "CASE"       "CE"         "Cer_ADS"    "Cer_AP"     "Cer_AS"    
[15] "Cer_EOS"    "Cer_HDS"    "Cer_HS"     "Cer_NDS"    "Cer_NP"     "Cer_NS"     "CL"        
[22] "CoQ"        "DCAE"       "DEGSE"      "DG"         "DGDG"       "DMPE"       "EtherDG"   
[29] "EtherDGDG"  "EtherLPC"   "EtherLPE"   "EtherMGDG"  "EtherOxPC"  "EtherPC"    "EtherPE"   
[36] "EtherPI"    "EtherPS"    "EtherTG"    "FA"         "FAHFA"      "HBMP"       "HexCer_AP" 
[43] "HexCer_HS"  "HexCer_NDS" "KDCAE"      "KLCAE"      "LCAE"       "LPC"        "LPE"       
[50] "MG"         "MGDG"       "NAE"        "NAGly"      "NAGlySer"   "NAOrn"      "NATau"     
[57] "Others"     "OxFA"       "OxPC"       "OxPE"       "OxPI"       "OxTG"       "PA"        
[64] "PC"         "PE"         "PEtOH"      "PG"         "PI"         "PMeOH"      "PS"        
[71] "SHexCer"    "SISE"       "SL"         "SM"         "SSulfate"   "ST"         "TG"        
[78] "TG_EST"     "Unknown"    "VAE"  


```
lipids_filtered<-lipids_filtered%>%
  select(sample, lipid, temperature, parent, type, polarity, formula, ontology, area_normalized)%>%
  group_by(sample, lipid, temperature, parent, type, formula, ontology)%>%
  summarize(area_normalized=sum(area_normalized, na.rm=TRUE))
```

Remove standard lipids - those with (d7) or (d9) in the lipid name.      

```
lipid_d7 <- lipids_filtered %>%
  filter(str_detect(lipid, "\\(d7\\)"))%>%
  pull(lipid)%>%
  unique()

lipid_d9 <- lipids_filtered %>%
  filter(str_detect(lipid, "\\(d9\\)"))%>%
  pull(lipid)%>%
  unique()

lipids_filtered<-lipids_filtered%>%
  filter(!lipid %in% lipid_d7)%>%
  filter(!lipid %in% lipid_d9)
```

```
length(unique(lipids_filtered$lipid))
```

We have 1009 lipids in our dataset.    

```
hist(lipids_filtered$area_normalized)
```

Plot and remove outliers.   

```
wide_data<-lipids_filtered%>%
  ungroup()%>%
  select(!formula)%>%
  select(!ontology)%>%
  select(!type)%>%
  pivot_wider(names_from=lipid, values_from=area_normalized)
```

```
wide_data%>%
    select_if(~ any(is.na(.)))
```

Replace NA's with 0's.   

```
wide_data[is.na(wide_data)] <- 0

wide_data%>%
    select_if(~ any(is.na(.)))
```

```
str(wide_data)

wide_data$temperature<-factor(wide_data$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

wide_data$parent<-factor(wide_data$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

All characteristics are renamed.    

```
scaled.pca<-prcomp(wide_data[c(4:1012)], scale=TRUE, center=TRUE) 
```

```
pca1<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  geom_text(aes(x = PC1, y = PC2, label = sample), vjust = -0.5)+
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="none",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));pca1
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/plot1.png?raw=true)  

Sample 23 and 48 are outliers, remove.   

``` 
wide_data<-wide_data%>%
  filter(!sample=="23")%>%
  filter(!sample=="48")

lipids_filtered<-lipids_filtered%>%
  filter(!sample=="23")%>%
  filter(!sample=="48")
```

## Conduct filtering for relative standard deviation 

Make a dataframe that calculates the standard deviation of the lipid area within treatment relative to the mean (i.e., how "large" is the SD).    

The formula will be:    

(sd*100)/mean   

Calculate for each lipid in each treatment group.   

```
test<-lipids_filtered%>%
  group_by(lipid, parent, temperature)%>%
  summarise(RSD=(sd(area_normalized)*100)/mean(area_normalized))

length(unique(test$lipid))

hist(test$RSD)
```

1009 lipids before   

Keep lipids with RSD <40 in all treatments.    

```
test <- test %>%
  group_by(lipid, parent, temperature) %>%
  filter(all(RSD < 40))

length(unique(test$lipid))

keep_list<-c(unique(test$lipid))

hist(test$RSD)
```

843 metabolites after   

Filter the filtered dataset to include the selected metabolites after RSD filtering.    

```
lipids_filtered<-lipids_filtered%>%
  filter(lipid %in% keep_list)
```

We are now ready to conduct analysis!   

Export the processed dataset.   

```
write_csv(lipids_filtered, file="output/lipids_metabolites/lipids/processed_lipids.csv")
```

## Unsupervised analysis 

Conduct a PCA and PERMANOVA to examine the effects of parent and temperature on the lipidome of coral larvae.   

Convert to wide format.   

```
wide_data<-lipids_filtered%>%
  ungroup()%>%
  select(!formula)%>%
  select(!ontology)%>%
  select(!type)%>%
  pivot_wider(names_from=lipid, values_from=area_normalized)
```

```
wide_data%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.  

```
str(wide_data)

wide_data$temperature<-factor(wide_data$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

wide_data$parent<-factor(wide_data$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

All characteristics are renamed.    

View scree plot.   

```
scaled.pca<-prcomp(wide_data[c(4:846)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot  

```
# scale data
vegan <- scale(wide_data[c(4:846)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = wide_data, method='eu')
permanova
```  

adonis2(formula = vegan ~ parent * temperature, data = wide_data, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)  
parent              2     3925 0.09312 2.5252  0.024 *
temperature         2     3346 0.07938 2.1525  0.050 *
parent:temperature  4     2238 0.05309 0.7198  0.703  
Residual           42    32642 0.77441                
Total              50    42150 1.00000      

Because the unsupervised analysis shows an effect of parent and temperature, we will proceed with PLSDA analyses to look at VIPS that separate parent groups and those that separate temperature groups.  


```
pca1<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  geom_text(aes(x = PC1, y = PC2, label = sample), vjust = -0.5)+
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="none",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));pca1
```

```
pca2<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca2

pca2b<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="temperature", loadings=FALSE,  colour="temperature", shape="parent", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_fill_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_shape_manual(name="Parental Phenotype", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca2b

pca_plots<-plot_grid(pca2, pca2b, ncol=2, nrow=1)

ggsave(pca_plots, filename="figures/lipids/pca_plots.jpeg", width=12, height=4)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_plots.jpeg?raw=true)

## Supervised PLS-DA analysis  

Script modified from the following tutorial: https://mixomicsteam.github.io/Bookdown/plsda.html  

### PLS-DA for the effect of parent 

Generate a PLS-DA and plot.   

```
#assigning datasets 
X <- wide_data

levels(as.factor(X$parent))
levels(as.factor(X$temperature))

Y <- as.factor(X$parent) #select treatment names
Y

X<-X[4:846] #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y, ncomp=2) # 1 Run the method with ncomp = groups -1
plotIndiv(MyResult.plsda)    # 2 Plot the samples

cols<-c("gray", "orange", "brown4") 

pdf("figures/lipids/PLSDA_Parent.pdf", width=9, height=6)
plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
dev.off() 

plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/PLSDA_Parent.png?raw=true)

Set model to determine VIPs.    

```
MyResult.plsda_parent <- plsda(X,Y, ncomp=2) 

#view plsda model again
plotIndiv(MyResult.plsda_parent, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent", ellipse = TRUE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

Extract VIP scores to view the lipids that are most highly differentiating lipids.         

```
#extract
parent_VIP <- PLSDA.VIP(MyResult.plsda_parent, graph=TRUE)

parent_VIP_DF <- as.data.frame(parent_VIP[["tab"]])
parent_VIP_DF

# Converting row names to column
parent_VIP_table <- rownames_to_column(parent_VIP_DF, var = "Lipid")

#filter for VIP > 1
parent_VIP_1 <- parent_VIP_table %>% 
  filter(VIP >= 1)

write_csv(parent_VIP_1, "output/lipids_metabolites/lipids/Overall_Parent_VIPs.csv")

#plot
parent_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Lipid,VIP,sum))) +
  geom_point() +
  ylab("Lipid") +
  xlab("VIP Score") +
  ggtitle("Parent VIP") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))


pdf("figures/lipids/Parent_VIP.pdf", width=10, height=35)
parent_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Lipid,VIP,sum))) +
  geom_point() +
  ylab("Lipid") +
  xlab("VIP Score") +
  ggtitle("Parent VIP > 1") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))
dev.off() 
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/Parent_VIP.jpg?raw=true)

These lipids represent VIPs that drive separation between parent groups.    

Make a list of vips.   

```
parent_vip_list<-parent_VIP_1%>%
  pull(Lipid)
```

There are 237 VIPs.   

Plot heatmap of z score of lipids across life stages.    

```
# Filter data for VIP metabolites
parent_vip_data <- lipids_filtered %>% filter(lipid %in% parent_vip_list)

# Convert counts to Z-scores
parent_vip_data <- parent_vip_data %>% 
  group_by(lipid) %>% 
  mutate(z_score = scale(area_normalized)) %>% 
  group_by(parent, lipid)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- parent_vip_data %>% 
  dplyr::select(lipid, parent, z_score) %>%
  spread(key = parent, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "lipid"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = FALSE,
  show_column_names = TRUE,
  #row_split=6,
  row_title = "Lipid", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/lipids/VIP_heatmap_parent.pdf", width=4, height=6)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/VIP_parent_heatmap.png?raw=true)

There are sets of lipids that are elevated in bleached parent larvae and another set that are elevated in nonbleached and wildtype. This is interesting because larvae from bleached parents are dominated by *Cladocopium* and larvae from nonbleached and wildtype parents have a mixture of *Cladocopium* and *Durusdinium* symbionts.  

View a PCA of these VIPs.   

```
vip_data_parent<-lipids_filtered%>%
  ungroup()%>%
  select(!formula)%>%
  select(!ontology)%>%
  select(!type)%>%
  filter(lipid %in% parent_vip_list)%>%
  pivot_wider(names_from=lipid, values_from=area_normalized)
```

```
vip_data_parent%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.    

```
vip_data_parent$temperature<-factor(vip_data_parent$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

vip_data_parent$parent<-factor(vip_data_parent$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

All characteristics are renamed.  

```
scaled.pca<-prcomp(vip_data_parent[c(4:240)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot

```
# scale data
vegan <- scale(vip_data_parent[c(4:240)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = vip_data_parent, method='eu')
permanova
```

adonis2(formula = vegan ~ parent * temperature, data = vip_data_parent, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)    
parent              2   2631.8 0.22209 7.0674  0.001 ***
temperature         2    874.1 0.07376 2.3472  0.031 *  
parent:temperature  4    524.0 0.04422 0.7035  0.753    
Residual           42   7820.1 0.65993                  
Total              50  11850.0 1.00000         

```
pca_vip_parent1<-ggplot2::autoplot(scaled.pca, data=vip_data_parent, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_parent1

pca_vip_parent2<-ggplot2::autoplot(scaled.pca, data=vip_data_parent, frame.colour="temperature", loadings=FALSE,  colour="temperature", shape="parent", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_fill_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_shape_manual(name="Parental Phenotype", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_parent2

pca_plots_parent<-plot_grid(pca_vip_parent1, pca_vip_parent2, ncol=2, nrow=1)

ggsave(pca_plots_parent, filename="figures/lipids/pca_plots_parentVIP.jpeg", width=12, height=4)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_plots_parentVIP.jpeg?raw=true)

### PLS-DA for the effect of temperature 

Generate a PLS-DA and plot. 
 
```
#assigning datasets 
X <- wide_data

levels(as.factor(X$parent))
levels(as.factor(X$temperature))

Y <- as.factor(X$temperature) #select treatment names
Y

levels(Y)

X<-X[4:846] #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y, ncomp=2) # 1 Run the method with ncomp = groups -1
plotIndiv(MyResult.plsda)    # 2 Plot the samples

cols<-c("blue", "orange", "red") 

pdf("figures/lipids/PLSDA_Temperature.pdf", width=9, height=6)
plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
dev.off() 

plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/PLSDA_Temperature.png?raw=true)

Set model to determine VIPs.  

```
MyResult.plsda_temperature <- plsda(X,Y, ncomp=2) 

#view plsda model again
plotIndiv(MyResult.plsda_temperature, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = TRUE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

Extract VIP scores for the most highly differentiating lipids.         

```
#extract
temperature_VIP <- PLSDA.VIP(MyResult.plsda_temperature, graph=TRUE)

temperature_VIP_DF <- as.data.frame(temperature_VIP[["tab"]])
temperature_VIP_DF

# Converting row names to column
temperature_VIP_table <- rownames_to_column(temperature_VIP_DF, var = "Lipid")

#filter for VIP > 1
temperature_VIP_1 <- temperature_VIP_table %>% 
  filter(VIP >= 1)

write_csv(temperature_VIP_1, "output/lipids_metabolites/lipids/Overall_Temperature_VIPs.csv")

#plot
temperature_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Lipid,VIP,sum))) +
  geom_point() +
  ylab("Lipid") +
  xlab("VIP Score") +
  ggtitle("Temperature VIP") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))


pdf("figures/lipids/Temperature_VIP.pdf", width=10, height=35)
temperature_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Lipid,VIP,sum))) +
  geom_point() +
  ylab("Lipid") +
  xlab("VIP Score") +
  ggtitle("Temperature VIP > 1") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))
dev.off() 
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/PLSDA_Temperature.png?raw=true)

These lists represent VIPs that drive separation between temperature groups.    

Make a list of vips.   

```
temperature_vip_list<-temperature_VIP_1%>%
  pull(Lipid)
```

There are 251 VIPs. 

Plot heatmap of z score of lipids across temperature.  

```
# Filter data for VIP metabolites
temperature_vip_data <- lipids_filtered %>% filter(lipid %in% temperature_vip_list)

# Convert counts to Z-scores
temperature_vip_data <- temperature_vip_data %>% 
  group_by(lipid) %>% 
  mutate(z_score = scale(area_normalized)) %>% 
  group_by(temperature, lipid)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

temperature_vip_data$temperature<-factor(temperature_vip_data$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- temperature_vip_data %>% 
  dplyr::select(lipid, temperature, z_score) %>%
  spread(key = temperature, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "lipid"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = FALSE,
  show_column_names = TRUE,
  #row_split=6,
  row_title = "Lipid", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/lipids/VIP_heatmap_temperature.pdf", width=4, height=6)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/VIP_heatmap_temperature.png?raw=true)

View a PCA of these VIPs.   

```
vip_data_temperature<-lipids_filtered%>%
  ungroup()%>%
  select(!formula)%>%
  select(!ontology)%>%
  select(!type)%>%
  filter(lipid %in% temperature_vip_list)%>%
  pivot_wider(names_from=lipid, values_from=area_normalized)
```

```
vip_data_temperature%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.    

```
vip_data_temperature$temperature<-factor(vip_data_temperature$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

vip_data_temperature$parent<-factor(vip_data_temperature$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

All characteristics are renamed.  

```
scaled.pca<-prcomp(vip_data_temperature[c(4:254)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot

```
# scale data
vegan <- scale(vip_data_temperature[c(4:254)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = vip_data_temperature, method='eu')
permanova
```

adonis2(formula = vegan ~ parent * temperature, data = vip_data_temperature, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)  
parent              2   1228.1 0.09786 2.7436  0.018 *
temperature         2   1219.2 0.09715 2.7237  0.021 *
parent:temperature  4    702.6 0.05598 0.7848  0.681  
Residual           42   9400.1 0.74901                
Total              50  12550.0 1.00000          

```
pca_vip_temperature1<-ggplot2::autoplot(scaled.pca, data=vip_data_temperature, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_temperature1

pca_vip_temperature2<-ggplot2::autoplot(scaled.pca, data=vip_data_temperature, frame.colour="temperature", loadings=FALSE,  colour="temperature", shape="parent", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_fill_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_shape_manual(name="Parental Phenotype", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_temperature2

pca_plots_temperature<-plot_grid(pca_vip_temperature1, pca_vip_temperature2, ncol=2, nrow=1)

ggsave(pca_plots_temperature, filename="figures/lipids/pca_plots_temperatureVIP.jpeg", width=12, height=4)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_plots_temperatureVIP.jpeg?raw=true)

### PLS-DA for the effect of parent x temperature 

Next, run a PLSDA to look for VIPs between groups (combination of parent x temperature treatment).  

Generate a PLS-DA and plot.   

```
#assigning datasets 
X <- wide_data

levels(as.factor(X$parent))
levels(as.factor(X$temperature))

X$code<-paste(X$parent, X$temperature)

levels(as.factor(X$code))

Y <- as.factor(X$code) #select treatment names
Y

levels(Y)

X<-X[4:846] #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y, ncomp=5) # 1 Run the method with ncomp = groups -1
plotIndiv(MyResult.plsda)    # 2 Plot the samples

cols<-c("orange", "orange3", "orange4", "brown", "brown2", "brown4", "gray", "lightgray", "darkgray") 

pdf("figures/lipids/PLSDA_interaction.pdf", width=9, height=6)
plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
dev.off() 

plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/PLSDA_Interaction.png?raw=true)

Set model to determine VIPs.  

```
MyResult.plsda_interaction <- plsda(X,Y, ncomp=5) 

#view plsda model again
plotIndiv(MyResult.plsda_interaction, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent x Temperature", ellipse = TRUE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

Extract VIP scores of the most highly differentiating lipids.       

```
#extract
interaction_VIP <- PLSDA.VIP(MyResult.plsda_interaction, graph=TRUE)

interaction_VIP_DF <- as.data.frame(interaction_VIP[["tab"]])
interaction_VIP_DF

# Converting row names to column
interaction_VIP_table <- rownames_to_column(interaction_VIP_DF, var = "Lipid")

#filter for VIP > 1
interaction_VIP_1 <- interaction_VIP_table %>% 
  filter(VIP >= 1)

write_csv(interaction_VIP_1, "output/lipids_metabolites/lipids/Overall_Interaction_VIPs.csv")

#plot
interaction_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Lipid,VIP,sum))) +
  geom_point() +
  ylab("Lipid") +
  xlab("VIP Score") +
  ggtitle("Parent x Temperature VIP") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))


pdf("figures/lipids/Interaction_VIP.pdf", width=10, height=35)
interaction_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Lipid,VIP,sum))) +
  geom_point() +
  ylab("Lipid") +
  xlab("VIP Score") +
  ggtitle("Interaction VIP > 1") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))
dev.off() 
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/Interaction_VIP.png?raw=true)

These lists represent VIPs that drive separation between parent x temperature groups.  

Make a list of vips.   

```
interaction_vip_list<-interaction_VIP_1%>%
  pull(Lipid)
```

There are 302 VIPs. 

Plot heatmap of z score of lipids across groups.   

```
# Filter data for VIP metabolites
interaction_vip_data <- lipids_filtered %>% filter(lipid %in% interaction_vip_list)

# Convert counts to Z-scores
interaction_vip_data <- interaction_vip_data %>% 
  mutate(code=paste(parent, temperature))%>%
  group_by(lipid) %>% 
  mutate(z_score = scale(area_normalized)) %>% 
  group_by(code, lipid)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()


interaction_vip_data$code<-factor(interaction_vip_data$code, levels=c("Bleached Ambient", "Bleached Moderate (+3)", "Bleached High (+6)", "Nonbleached Ambient", "Nonbleached Moderate (+3)", "Nonbleached High (+6)", "Wildtype Ambient", "Wildtype Moderate (+3)", "Wildtype High (+6)"))

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- interaction_vip_data %>% 
  dplyr::select(lipid, code, z_score) %>%
  spread(key = code, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "lipid"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = FALSE,
  show_column_names = TRUE,
  #row_split=4,
  row_title = "Lipid", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/lipids/VIP_heatmap_parentxtemperature.pdf", width=6, height=8)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/VIP_heatmap_parentxtemperature.png?raw=true)

View a PCA of these VIPs.   

```
vip_data_interaction<-lipids_filtered%>%
  ungroup()%>%
  select(!formula)%>%
  select(!ontology)%>%
  select(!type)%>%
  filter(lipid %in% interaction_vip_list)%>%
  pivot_wider(names_from=lipid, values_from=area_normalized)
```

```
vip_data_interaction%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.    

```
vip_data_interaction$temperature<-factor(vip_data_interaction$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

vip_data_interaction$parent<-factor(vip_data_interaction$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

All characteristics are renamed.    

```
scaled.pca<-prcomp(vip_data_interaction[c(4:305)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot  

```
# scale data
vegan <- scale(vip_data_interaction[c(4:305)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = vip_data_interaction, method='eu')
permanova
```

adonis2(formula = vegan ~ parent * temperature, data = vip_data_interaction, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)    
parent              2   2779.5 0.18407 5.8640  0.001 ***
temperature         2   1432.4 0.09486 3.0220  0.002 ** 
parent:temperature  4    934.5 0.06189 0.9858  0.450    
Residual           42   9953.7 0.65918                  
Total              50  15100.0 1.00000    

No interactive effect detected.  

View a PCA with treatment code information. 

```
test_wide_data<-vip_data_interaction%>%
  mutate(code=paste(parent, temperature))

levels(as.factor(test_wide_data$code))

pca_interaction3<-ggplot2::autoplot(scaled.pca, data=test_wide_data, frame.colour="code", loadings=FALSE,  colour="code", shape="code", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Group", values=c("orange", "orange", "orange", "brown4", "brown4", "brown4", "gray", "gray", "gray"))+
  scale_fill_manual(name="Group", values=c("orange", "orange", "orange", "brown4", "brown4", "brown4", "gray", "gray", "gray"))+
  scale_shape_manual(name="Group", values=c(19,17,15,19,17,15,19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_interaction3

ggsave(pca_interaction3, filename="figures/lipids/pca_all_groups_interactionVIP.jpeg", width=8, height=6)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_all_groups_interactionVIP.jpeg?raw=true)

## Output lists for LION 

Conduct analysis in [LION](lipidontology.com) for lipid enrichment. This site is similar to metaboanalyst for enrichment analysis of lipids.    

List of parental VIPS 

```
parent_vip_list%>%
  as.data.frame()%>%
  write_csv(., file="output/lipids_metabolites/lipids/parent_VIP_list.csv")
```

No significant enrichment of any terms on LION.   

List of temperature VIPS

```
temperature_vip_list%>%
  as.data.frame()%>%
  write_csv(., file="output/lipids_metabolites/lipids/temperature_VIP_list.csv")
```

No significant enrichment of any terms on LION.   

List of interaction VIPS

```
interaction_vip_list%>%
  as.data.frame()%>%
  write_csv(., file="output/lipids_metabolites/lipids/interaction_VIP_list.csv")
```

Full list of lipids as the background.  

```
full_lipid_list<-lipids_filtered%>%
  pull(lipid)%>%
  unique()%>%
  as.data.frame()%>%
  write_csv(., file="output/lipids_metabolites/lipids/full_lipid_list.csv")
```

What is the overlap in these lists? Create an upset plot. 

```{r}
# Combine lists into a data frame indicating membership (1 = present, 0 = absent)
upset_data <- data.frame(
  Lipid = unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)),
  List1 = as.numeric(unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)) %in% parent_vip_list),
  List2 = as.numeric(unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)) %in% temperature_vip_list),
  List3 = as.numeric(unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)) %in% interaction_vip_list)
)

# Convert data to the format needed for the UpSetR plot
plot_data <- upset_data[, -1]
row.names(plot_data) <- upset_data$Lipid
names(plot_data) <- c("Parent", "Temperature", "PxT Group")

# Generate the UpSet plot
pdf("figures/lipids/upset_VIP_list_overlap.pdf", width=6, height=6)
upset(plot_data, sets = c("Parent", "Temperature", "PxT Group"), keep.order = TRUE)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/upset1.png?raw=true)

## Plot enriched lipids in the group VIP data set 

Results from LION:   

- No enrichment in parent list   
- No enrichment in temperature list   
- fatty acids [FA], and fatty acids and conjugates [FA01] enriched in group specific list   

### Ontology FA  

Add in lipid ontology values.   

```
lipids$ontology<-ontology$ontology[match(lipids$lipid, ontology$lipid)]
```

Plot a heatmap of the target ontologies in the group VIP list (parent x temperature).   

Fatty acids  

```
# Filter data for VIP metabolites
FA_data <- lipids %>% 
  filter(lipid %in% interaction_vip_list) %>% 
  filter(ontology=="FA") %>% 
  droplevels()

length(unique(FA_data$lipid))
#16 fatty acids in this data set 

# Convert counts to Z-scores
FA_data <- FA_data %>% 
  group_by(lipid) %>% 
  mutate(z_score = scale(area_normalized)) %>% 
  group_by(parent, temperature, lipid)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- FA_data %>% 
  mutate(code=paste(parent, temperature))%>%
  mutate(code=factor(code, levels=c("Bleached Ambient", "Bleached Moderate (+3)", "Bleached High (+6)", "Nonbleached Ambient", "Nonbleached Moderate (+3)", "Nonbleached High (+6)", "Wildtype Ambient", "Wildtype Moderate (+3)", "Wildtype High (+6)")))%>%
  dplyr::select(lipid, code, z_score) %>%
  spread(key = code, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "lipid"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = TRUE,
  show_column_names = TRUE,
  #row_split=split,
  #column_split=heatmap_data$code,
  row_title = "Fatty Acids", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/lipids/fatty_acids_VIP.pdf", width=6, height=8)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/fatty_acids_VIP.png?raw=true)

This is interesting! There are several FAs that are elevated in moderate temperature in nonbleached and wildtype larvae, but not in bleached parent larvae. There is one FA, 28:7 that is depleted in bleached larvae and another, FA22:6 that is depleted in bleached larave at high temperature. THere are also several fatty acids that are strongly elevated in nonbleached larvae at moderate temperatures.  

No other categories were enriched in the VIP lists.  

# Lipidomics (Peak height) 

The analysis followed the same steps as done for area above. Here are the figures for analysis of height. Results are largely the same. I also produced an upset plot below to look at VIP overlap between the two analyses.  

I will have to decide which data set to use.  

## Parental effects 

PCA of parent and treatment effects:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_plots_height.jpeg?raw=true)  

PLSDA of parent effects:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/PLSDA_Parent_height.png?raw=true)

VIPs of parent effects:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/Parent_VIP_height.png?raw=true)

Heatmap of parent VIPs:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/VIP_heatmap_parent_height.png?raw=true)

PCA of parent VIPs:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_plots_parentVIP_height.png?raw=true)

## Temperature effects

PLSDA of temperature effects:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/PLSDA_Temperature_height.png?raw=true)

VIPs of temperature effects:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/Temperature_VIP_height.png?raw=true)

Heatmap of temperature VIPs:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/VIP_heatmap_temperature_height.png?raw=true)

PCA of temperature VIPs:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_plots_temperatureVIP_height.jpeg?raw=true)

## Treatment group effects 

PLSDA of group effects:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/PLSDA_Interaction_height.png?raw=true)

VIPs of group effects:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/Interaction_VIP_height.png?raw=true)

Heatmap of group VIPs:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/VIP_heatmap_parentxtemperature_height.png?raw=true)

PCA of group VIPs:  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/pca_plots_interactionVIP_height.jpeg?raw=true)

## LION enrichment 

Enrichment results from LION: 

- No enrichment in parent list 
- polyunsaturated fatty acid enriched in temperature list (LION:0002967)
- diacylglycerols [GL0201], diradylglycerols [GL02], and lipid-mediated signalling enriched in group-specific list 

View lipids related to the enriched categories in the interaction/group specific VIP data set from the LION results.    

Monounsaturated fatty acids, fatty acids, fatty acids and conjugates, Fatty acid with less than 2 double bonds = ontology FA   

Diacylglycerols = ontology DG  

Lipid-mediated signalling = could include ontology PC, PS, PG, PA, PE (glycerophospholipids), contains Cer (ceramides)  

Add in lipid ontology values.   

```
lipids$ontology<-ontology$ontology[match(lipids$lipid, ontology$lipid)]
```

Plot a heatmap of the target ontologies in the group VIP list (parent x temperature).   

Fatty acids  

```
# Filter data for VIP metabolites
FA_data <- lipids %>% 
  filter(lipid %in% interaction_vip_list) %>% 
  filter(ontology=="FA") %>% 
  droplevels()

length(unique(FA_data$lipid))
#14 fatty acids in this data set 

# Convert counts to Z-scores
FA_data <- FA_data %>% 
  group_by(lipid) %>% 
  mutate(z_score = scale(height_normalized)) %>% 
  group_by(parent, temperature, lipid)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- FA_data %>% 
  mutate(code=paste(parent, temperature))%>%
  mutate(code=factor(code, levels=c("Bleached Ambient", "Bleached Moderate (+3)", "Bleached High (+6)", "Nonbleached Ambient", "Nonbleached Moderate (+3)", "Nonbleached High (+6)", "Wildtype Ambient", "Wildtype Moderate (+3)", "Wildtype High (+6)")))%>%
  dplyr::select(lipid, code, z_score) %>%
  spread(key = code, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "lipid"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = TRUE,
  show_column_names = TRUE,
  #row_split=split,
  #column_split=heatmap_data$code,
  row_title = "Fatty Acids", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/lipids/fatty_acids_VIP_height.pdf", width=6, height=8)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/fatty_acids_VIP_height.png?raw=true)

Diacylglycerols  

```
# Filter data for VIP metabolites
DG_data <- lipids %>% 
  filter(lipid %in% interaction_vip_list) %>% 
  filter(ontology=="DG") %>% 
  droplevels()

length(unique(DG_data$lipid))
#61 DGs in this data set 

# Convert counts to Z-scores
DG_data <- DG_data %>% 
  group_by(lipid) %>% 
  mutate(z_score = scale(height_normalized)) %>% 
  group_by(parent, temperature, lipid)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- DG_data %>% 
  mutate(code=paste(parent, temperature))%>%
  mutate(code=factor(code, levels=c("Bleached Ambient", "Bleached Moderate (+3)", "Bleached High (+6)", "Nonbleached Ambient", "Nonbleached Moderate (+3)", "Nonbleached High (+6)", "Wildtype Ambient", "Wildtype Moderate (+3)", "Wildtype High (+6)")))%>%
  dplyr::select(lipid, code, z_score) %>%
  spread(key = code, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "lipid"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = TRUE,
  show_column_names = TRUE,
  #row_split=7,
  row_title = "DGs", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/lipids/diacylglycerols_VIP_height.pdf", width=6, height=14)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/diacylglycerols_VIP_height.png?raw=true)

Lipid mediated signaling  

```
# Filter data for VIP metabolites
signal_data <- lipids %>% 
  filter(lipid %in% interaction_vip_list) %>% 
  filter(ontology %in% c("PC", "PS", "PG", "PA", "PE") | grepl("Cer", ontology))%>%
  droplevels()

length(unique(signal_data$lipid))
#64 lipids in this data set 

# Convert counts to Z-scores
signal_data <- signal_data %>% 
  group_by(lipid) %>% 
  mutate(z_score = scale(height_normalized)) %>% 
  group_by(parent, temperature, lipid)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- signal_data %>% 
  mutate(code=paste(parent, temperature))%>%
  mutate(code=factor(code, levels=c("Bleached Ambient", "Bleached Moderate (+3)", "Bleached High (+6)", "Nonbleached Ambient", "Nonbleached Moderate (+3)", "Nonbleached High (+6)", "Wildtype Ambient", "Wildtype Moderate (+3)", "Wildtype High (+6)")))%>%
  dplyr::select(lipid, code, z_score) %>%
  spread(key = code, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "lipid"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = TRUE,
  show_column_names = TRUE,
  #row_split=7,
  row_title = "Lipid Signaling", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/lipids/lipid_signaling_VIP_height.pdf", width=6, height=14)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/lipid_signaling_VIP_height.png?raw=true)

There were more functions enriched in the height data set.  

## Compare height and area VIP lists 

Plot upset plot for each factor tested between height and area to see overlap in VIP sets.       

Load in parent VIP lists.    

```
list1<-read_csv("output/lipids_metabolites/lipids/parent_VIP_list.csv")%>%pull(.)
list2<-read_csv("output/lipids_metabolites/lipids/parent_VIP_list_height.csv")%>%pull(.)

# Combine lists into a data frame indicating membership (1 = present, 0 = absent)
upset_data <- data.frame(
  Lipid = unique(c(list1, list2)),
  List1 = as.numeric(unique(c(list1, list2)) %in% list1),
  List2 = as.numeric(unique(c(list1, list2)) %in% list2))

# Convert data to the format needed for the UpSetR plot
plot_data <- upset_data[, -1]
row.names(plot_data) <- upset_data$Lipid
names(plot_data) <- c("Area", "Height")

# Generate the UpSet plot
upset(plot_data, sets = c("Area", "Height"), keep.order = TRUE)
```

There were 218 shared, 31 unique to height, 19 unique lipids to area data sets for parent VIPs. 

Load in temperature VIP lists.   

```
list1<-read_csv("output/lipids_metabolites/lipids/temperature_VIP_list.csv")%>%pull(.)
list2<-read_csv("output/lipids_metabolites/lipids/temperature_VIP_list_height.csv")%>%pull(.)

# Combine lists into a data frame indicating membership (1 = present, 0 = absent)
upset_data <- data.frame(
  Lipid = unique(c(list1, list2)),
  List1 = as.numeric(unique(c(list1, list2)) %in% list1),
  List2 = as.numeric(unique(c(list1, list2)) %in% list2))

# Convert data to the format needed for the UpSetR plot
plot_data <- upset_data[, -1]
row.names(plot_data) <- upset_data$Lipid
names(plot_data) <- c("Area", "Height")

# Generate the UpSet plot
upset(plot_data, sets = c("Area", "Height"), keep.order = TRUE)
```
There were 213 shared, 38 unique to height, 38 unique to area data sets for temperature VIPs.   

Group VIP lists.   

```
list1<-read_csv("output/lipids_metabolites/lipids/interaction_VIP_list.csv")%>%pull(.)
list2<-read_csv("output/lipids_metabolites/lipids/interaction_VIP_list_height.csv")%>%pull(.)

# Combine lists into a data frame indicating membership (1 = present, 0 = absent)
upset_data <- data.frame(
  Lipid = unique(c(list1, list2)),
  List1 = as.numeric(unique(c(list1, list2)) %in% list1),
  List2 = as.numeric(unique(c(list1, list2)) %in% list2))

# Convert data to the format needed for the UpSetR plot
plot_data <- upset_data[, -1]
row.names(plot_data) <- upset_data$Lipid
names(plot_data) <- c("Area", "Height")

# Generate the UpSet plot
upset(plot_data, sets = c("Area", "Height"), keep.order = TRUE)
```

There were 266 shared, 39 unique to height, 36 unique to area data sets for group VIPs.  

In each set, most lipids are shared by area and height data sets. There are some small differences.  

# Metabolomics 

```
library(tidyverse)
library(stringr)
library(readxl)
library(purrr)
library(lubridate)
library(ggplot2)
library(broom)
library(cowplot)
library(mixOmics)
library(dplyr)
library(conflicted)
library(vegan)
library(factoextra)
library(ggfortify)
library(RVAideMemoire)
library(ComplexHeatmap)
library(circlize)
library(tibble)
library(viridis)
library(genefilter)
library(UpSetR)

conflict_prefer("select", winner = "dplyr", quiet = FALSE)
conflict_prefer("filter", winner = "dplyr", quiet = FALSE)
conflict_prefer("rename", winner = "dplyr", quiet = FALSE)
```

## Data manipulation and preparations  

### Read in all files 

```
metadata<-read_csv("data/lipids_metabolites/metabolomics/Metabolite_Metadata_Huffmyer_Matthews.csv")%>%rename(tube=`Sample ID`, type=Type, temperature=Temperature, parent=Symbiont)

metabolites<-read_xlsx("data/lipids_metabolites/metabolomics/metabolite_data.xlsx")

protein<-read_csv("data/lipids_metabolites/protein/protein_calculated.csv")
```

Add protein data to the metadata.   

```
metadata$protein.ug<-protein$`Total Protein ug`[match(metadata$tube, protein$Sample)]
```

### Format data and conduct protein normalization 

Format the metabolite data frame in long format to obtain sample names and normalize to total protein.   

```
metabolites<-metabolites%>%
  pivot_longer(names_to = "file", values_to = "area", cols=c(2:74))%>%
  mutate(tube = str_extract(file, "(?<=_)[^_]+(?=_)"))%>%
  mutate(tube = str_remove_all(tube, "M"))

levels(as.factor(metabolites$tube))
levels(as.factor(metadata$tube))
```

Merge in metadata.  

```
metadata<-metadata%>%
  select(tube, type, temperature, parent, protein.ug)

metabolites<-left_join(metabolites, metadata)%>%select(!file)
```

Replace NA's with 0's in the metabolite data frame.   

```
metabolites %>%
  filter(is.na(area)) %>%
  nrow()
# there are many NA's, which need to be replaced with 0

metabolites <- metabolites %>%
  mutate(area = replace_na(area, 0))

metabolites %>%
  filter(is.na(area)) %>%
  nrow()
```

Filter out PBQC samples and blank samples, these were used for peak identification and are not needed now.   

```
metabolites<-metabolites%>%
  filter(!type=="PBQC")%>%
  filter(!type=="Blank")
```

Now perform internal standard normalization to C13 sorbitol average for each sample corrections.   

```
#make a dataframe of sorbitol values for each sample
sorbitol<-metabolites%>%
  filter(Name=="13C6-Sorbitol")%>%
  group_by(tube)%>%
  summarise(area_sorbitol=mean(area, na.rm=TRUE))

#add this value back into the master data frame for corrections. 
metabolites<-metabolites%>%
  left_join(.,sorbitol)
```

Normalize area by dividing area by sorbitol internal standard  value for each sample's peak value for each metabolite.   

```
metabolites<-metabolites%>%
  mutate(area_corrected=area/area_sorbitol)
```

Filter to only keep metabolites that have a detectable value in every sample.   

Check for outliers at this point.   

```
wide_data<-metabolites%>%
  ungroup()%>%
  select(!type)%>%
  select(!protein.ug)%>%
  select(!area_sorbitol)%>%
  select(!area)%>%
  pivot_wider(names_from=Name, values_from=area_corrected)
```

```
wide_data%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.  

```
#remove any column with all 0's 
wide_data <- wide_data[, colSums(wide_data != 0) > 0]

wide_data<-wide_data%>%
  select(!`13C6-Sorbitol`)

scaled.pca<-prcomp(wide_data[c(4:374)], scale=TRUE, center=TRUE) 
```

```
pca1<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  geom_text(aes(x = PC1, y = PC2, label = tube), vjust = -0.5)+
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="none",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));pca1
```

No outliers before removing metabolites for presence/absence filtering. 

Keep only metabolites detected in all samples.  

```
# Find metabolites that have a value > 0 for every tube
filtered_data <- metabolites %>%
  group_by(Name) %>%
  filter(all(area_corrected > 0))

# do this instead by keeping those present at >0 value in at least 90% of samples 
#threshold <- 0.9  # 90% threshold

#filtered_data <- metabolites %>%
#  group_by(Name) %>%
#  filter(mean(area_corrected > 0) >= threshold)

# Find metabolites that were removed (i.e., have any value <= 0)
removed_metabolites <- metabolites %>%
  filter(!(Name %in% filtered_data$Name)) %>%
  distinct(Name)

kept_metabolites <- metabolites %>%
  filter((Name %in% filtered_data$Name)) %>%
  distinct(Name)
```

57 metabolites are kept after all samples >0 filtering, 569 were removed. This makes sense, the data frame kept all metabolites in the library so we expect far fewer to be retained.   

66 are kept with filtering for >0 value in 90% of samples. Keeping more stringent filtering for now to keep metabolites that have detectable value in every sample.   

Normalize corrected area to sample protein. This will generate units of normalized peak area / ug protein.   

```
filtered_data<-filtered_data%>%
  mutate(area_normalized=area_corrected/protein.ug)
```

```
hist(filtered_data$area_normalized)

quantile(filtered_data$area_normalized)
```

```
length(unique(filtered_data$Name))
```

Filter out internal standards if present still.   

```
filtered_data<-filtered_data%>%
  filter(!Name=="13C5,15N1-Valine")%>%
  filter(!Name=="13C6-Sorbitol")
```

We are now ready for anaylsis! 

Plot to detect outliers.     

```
wide_data<-filtered_data%>%
  ungroup()%>%
  select(!type)%>%
  select(!protein.ug)%>%
  select(!area_sorbitol)%>%
  select(!area_corrected)%>%
  select(!area)%>%
  pivot_wider(names_from=Name, values_from=area_normalized)
```

```
wide_data%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.    

```
wide_data$temperature<-factor(wide_data$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

wide_data$parent<-factor(wide_data$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

```
scaled.pca<-prcomp(wide_data[c(4:58)], scale=TRUE, center=TRUE) 
```

```
pca1<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=FALSE, loadings.label.size=5, loadings.label.vjust=-1, size=5) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  geom_text(aes(x = PC1, y = PC2, label = tube), vjust = -0.5)+
  theme_classic()+
   theme(legend.text = element_text(size=18), 
         legend.position="none",
        plot.background = element_blank(),
        legend.title = element_text(size=18, face="bold"), 
        axis.text = element_text(size=18), 
        axis.title = element_text(size=18,  face="bold"));pca1
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/pca1.png?raw=true)

Sample L107 is a clear outlier. Remove.  

```
filtered_data<-filtered_data%>%
  filter(!tube=="L107")

wide_data<-wide_data%>%
  filter(!tube=="L107")
```

# Conduct filtering for relative standard deviation 

Make a dataframe that calculates the standard deviation of the metabolite area within treatment relative to the mean (i.e., how "large" is the SD).    

The formula will be:    

(sd*100)/mean   

Calculate for each metabolite in each treatment group.   

```
test<-filtered_data%>%
  group_by(Name, parent, temperature)%>%
  summarise(RSD=(sd(area_normalized)*100)/mean(area_normalized))

length(unique(test$Name))

hist(test$RSD)
```

55 metabolites before.  

Remove metabolites that have RSD < threshold in all treatments (RSD values are high, come back to this). I am using a threshold of 150. RSD values are high for this data set.    

```
test <- test %>%
  group_by(Name) %>%
  filter(all(RSD < 150))

length(unique(test$Name))

keep_list<-c(unique(test$Name))
```

48 metabolites after filtering.  

Filter the filtered dataset to include the selected metabolites after RSD filtering.    

```
filtered_data<-filtered_data%>%
  filter(Name %in% keep_list)
```

## Unsupervised analysis 

Conduct a PCA and PERMANOVA to examine the effects of parent and temperature on the metabolome of coral larvae.

Convert to wide format.   

```
wide_data<-filtered_data%>%
  ungroup()%>%
  select(!type)%>%
  select(!protein.ug)%>%
  select(!area_sorbitol)%>%
  select(!area_corrected)%>%
  select(!area)%>%
  pivot_wider(names_from=Name, values_from=area_normalized)
```

```
wide_data%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.  

```
wide_data$temperature<-factor(wide_data$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

wide_data$parent<-factor(wide_data$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

```
scaled.pca<-prcomp(wide_data[c(4:51)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot  

```
# scale data
vegan <- scale(wide_data[c(4:51)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = wide_data, method='eu')
permanova
```

adonis2(formula = vegan ~ parent * temperature, data = wide_data, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)  
parent              2    79.95 0.03203 0.8623  0.483  
temperature         2    74.27 0.02975 0.8009  0.542  
parent:temperature  4   301.81 0.12092 1.6274  0.087 .
Residual           44  2039.97 0.81730                
Total              52  2496.00 1.00000      

No effect of parent or temperature.      

```
pca2<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca2

pca2b<-ggplot2::autoplot(scaled.pca, data=wide_data, frame.colour="temperature", loadings=FALSE,  colour="temperature", shape="parent", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_fill_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_shape_manual(name="Parental Phenotype", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca2b

pca_plots<-plot_grid(pca2, pca2b, ncol=2, nrow=1)

ggsave(pca_plots, filename="figures/metabolites/pca_plots.jpeg", width=12, height=4)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/pca_plots.jpeg?raw=true)

No clear differences by parent or treatment.  

## Supervised PLS-DA analysis  

Script modified from the following tutorial: https://mixomicsteam.github.io/Bookdown/plsda.html

### PLS-DA for the effect of parent 

Generate a PLS-DA and plot.    

```
#assigning datasets 
X <- wide_data

levels(as.factor(X$parent))
levels(as.factor(X$temperature))

Y <- as.factor(X$parent) #select treatment names
Y

X<-X[4:51] #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y, ncomp=2) # 1 Run the method with ncomp = groups -1
plotIndiv(MyResult.plsda)    # 2 Plot the samples

cols<-c("gray", "orange", "brown4") 

pdf("figures/metabolites/PLSDA_Parent.pdf", width=9, height=6)
plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
dev.off() 

plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/PLSDA_Parent.png?raw=true)

Set model to determine VIPs.  

```{r, message=FALSE}
MyResult.plsda_parent <- plsda(X,Y, ncomp=2) 

#view plsda model again
plotIndiv(MyResult.plsda_parent, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent", ellipse = TRUE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

Extract VIP scores for metabolites that are most highly differentiating.       

```
#extract
parent_VIP <- PLSDA.VIP(MyResult.plsda_parent, graph=TRUE)

parent_VIP_DF <- as.data.frame(parent_VIP[["tab"]])
parent_VIP_DF

# Converting row names to column
parent_VIP_table <- rownames_to_column(parent_VIP_DF, var = "Metabolite")

#filter for VIP > 1
parent_VIP_1 <- parent_VIP_table %>% 
  filter(VIP >= 1)

write_csv(parent_VIP_1, "output/lipids_metabolites/metabolites/Overall_Parent_VIPs.csv")

#plot
parent_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Metabolite,VIP,sum))) +
  geom_point() +
  ylab("Metabolite") +
  xlab("VIP Score") +
  ggtitle("Parent VIP") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))


pdf("figures/metabolites/Parent_VIP.pdf", width=4, height=4)
parent_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Metabolite,VIP,sum))) +
  geom_point() +
  ylab("Metabolite") +
  xlab("VIP Score") +
  ggtitle("Parent VIP > 1") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))
dev.off() 
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/Parent_VIP.png?raw=true)

These lists represent VIPs that drive separation between parent groups. We can extract VIPs, but given that there is no significant separation in unsupervised analyses we will likely not move forward with more detailed analyses.     

Make a list of vips.   

```
parent_vip_list<-parent_VIP_1%>%
  pull(Metabolite)
```

There are 15 VIPs. 

Plot heatmap of z score of lipids across life stages.  

```
# Filter data for VIP metabolites
parent_vip_data <- filtered_data %>% filter(Name %in% parent_vip_list)

# Convert counts to Z-scores
parent_vip_data <- parent_vip_data %>% 
  group_by(Name) %>% 
  mutate(z_score = scale(area_normalized)) %>% 
  group_by(parent, Name)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- parent_vip_data %>% 
  dplyr::select(Name, parent, z_score) %>%
  spread(key = parent, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "Name"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = TRUE,
  show_column_names = TRUE,
  #row_split=3,
  row_title = "Metabolite", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/metabolites/VIP_heatmap_parent.pdf", width=5, height=6)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/VIP_heatmap_parent.png?raw=true)  

The variation appears to be mainly due to differences between wildtype and the other two phenotypes. This may have to do with genotypic diversity or the method of collection/location of population.  

View PCA for VIPs.  

```
vip_data_parent<-filtered_data%>%
  ungroup()%>%
  select(!type)%>%
  select(!protein.ug)%>%
  select(!area_sorbitol)%>%
  select(!area_corrected)%>%
  select(!area)%>%
  filter(Name %in% parent_vip_list)%>%
  pivot_wider(names_from=Name, values_from=area_normalized)
```

```
vip_data_parent%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.    

```
vip_data_parent$temperature<-factor(vip_data_parent$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

vip_data_parent$parent<-factor(vip_data_parent$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

```
scaled.pca<-prcomp(vip_data_parent[c(4:18)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot  

```
# scale data
vegan <- scale(vip_data_parent[c(4:18)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = vip_data_parent, method='eu')
permanova
```

adonis2(formula = vegan ~ parent * temperature, data = vip_data_parent, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)
parent              2    44.50 0.05705 1.5591  0.182
temperature         2    22.15 0.02839 0.7760  0.548
parent:temperature  4    85.45 0.10955 1.4969  0.159
Residual           44   627.91 0.80501              
Total              52   780.00 1.00000      

No difference in VIPs by parent or temperature.  

```
pca_vip_parent1<-ggplot2::autoplot(scaled.pca, data=vip_data_parent, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_parent1

pca_vip_parent2<-ggplot2::autoplot(scaled.pca, data=vip_data_parent, frame.colour="temperature", loadings=FALSE,  colour="temperature", shape="parent", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_fill_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_shape_manual(name="Parental Phenotype", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_parent2

pca_plots_parent<-plot_grid(pca_vip_parent1, pca_vip_parent2, ncol=2, nrow=1)

ggsave(pca_plots_parent, filename="figures/metabolites/pca_plots_parentVIP.jpeg", width=12, height=4)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/pca_plots_parentVIP.jpeg?raw=true)

### PLS-DA for the effect of temperature 

Generate a PLS-DA and plot.  
  
```
#assigning datasets 
X <- wide_data

levels(as.factor(X$parent))
levels(as.factor(X$temperature))

Y <- as.factor(X$temperature) #select treatment names
Y

levels(Y)

X<-X[4:51] #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y, ncomp=2) # 1 Run the method with ncomp = groups -1
plotIndiv(MyResult.plsda)    # 2 Plot the samples

cols<-c("blue", "orange", "red") 

pdf("figures/metabolites/PLSDA_Temperature.pdf", width=9, height=6)
plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
dev.off() 

plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/PLSDA_Temperature.png?raw=true)

Set model to determine VIPs.    
 
```
MyResult.plsda_temperature <- plsda(X,Y, ncomp=2) 

#view plsda model again
plotIndiv(MyResult.plsda_temperature, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = TRUE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

Extract VIP scores of differentiating metabolites.     

```
#extract
temperature_VIP <- PLSDA.VIP(MyResult.plsda_temperature, graph=TRUE)

temperature_VIP_DF <- as.data.frame(temperature_VIP[["tab"]])
temperature_VIP_DF

# Converting row names to column
temperature_VIP_table <- rownames_to_column(temperature_VIP_DF, var = "Metabolite")

#filter for VIP > 1
temperature_VIP_1 <- temperature_VIP_table %>% 
  filter(VIP >= 1)

write_csv(temperature_VIP_1, "output/lipids_metabolites/metabolites/Overall_Temperature_VIPs.csv")

#plot
temperature_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Metabolite,VIP,sum))) +
  geom_point() +
  ylab("Metabolite") +
  xlab("VIP Score") +
  ggtitle("Temperature VIP") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))


pdf("figures/metabolites/Temperature_VIP.pdf", width=4, height=4)
temperature_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Metabolite,VIP,sum))) +
  geom_point() +
  ylab("Metabolite") +
  xlab("VIP Score") +
  ggtitle("Temperature VIP > 1") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))
dev.off() 
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/Temperature_VIP.png?raw=true)

These lists represent VIPs that drive separation between temperature groups.    

Make a list of vips.  

```
temperature_vip_list<-temperature_VIP_1%>%
  pull(Metabolite)
```
There are 10 VIPs.   

Plot heatmap of z score of metabolites across temperature.    

```
# Filter data for VIP metabolites
temperature_vip_data <- filtered_data %>% filter(Name %in% temperature_vip_list)

# Convert counts to Z-scores
temperature_vip_data <- temperature_vip_data %>% 
  group_by(Name) %>% 
  mutate(z_score = scale(area_normalized)) %>% 
  group_by(temperature, Name)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

temperature_vip_data$temperature<-factor(temperature_vip_data$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- temperature_vip_data %>% 
  dplyr::select(Name, temperature, z_score) %>%
  spread(key = temperature, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "Name"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = TRUE,
  show_column_names = TRUE,
  #row_split=3,
  row_title = "Metabolite", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/metabolites/VIP_heatmap_temperature.pdf", width=6, height=7)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/VIP_heatmap_temperature.png?raw=true)  

View PCA for VIPs.  

```
vip_data_temperature<-filtered_data%>%
  ungroup()%>%
  select(!type)%>%
  select(!protein.ug)%>%
  select(!area_sorbitol)%>%
  select(!area_corrected)%>%
  select(!area)%>%
  filter(Name %in% temperature_vip_list)%>%
  pivot_wider(names_from=Name, values_from=area_normalized)
```

```
vip_data_temperature%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.  

```
vip_data_temperature$temperature<-factor(vip_data_temperature$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

vip_data_temperature$parent<-factor(vip_data_temperature$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

```
scaled.pca<-prcomp(vip_data_temperature[c(4:13)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot  

```
# scale data
vegan <- scale(vip_data_temperature[c(4:13)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = vip_data_temperature, method='eu')
permanova
```

adonis2(formula = vegan ~ parent * temperature, data = vip_data_temperature, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)
parent              2    28.82 0.05543 1.5314  0.208
temperature         2    23.68 0.04553 1.2579  0.263
parent:temperature  4    53.42 0.10273 1.4192  0.203
Residual           44   414.08 0.79630              
Total              52   520.00 1.00000   

No significant effects.  

```
pca_vip_temperature1<-ggplot2::autoplot(scaled.pca, data=vip_data_temperature, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_temperature1

pca_vip_temperature2<-ggplot2::autoplot(scaled.pca, data=vip_data_temperature, frame.colour="temperature", loadings=FALSE,  colour="temperature", shape="parent", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_fill_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_shape_manual(name="Parental Phenotype", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_temperature2

pca_plots_temperature<-plot_grid(pca_vip_temperature1, pca_vip_temperature2, ncol=2, nrow=1)

ggsave(pca_plots_temperature, filename="figures/metabolites/pca_plots_temperatureVIP.jpeg", width=12, height=4)
```
![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/pca_plots_temperatureVIP.jpeg?raw=true)

### PLS-DA for the effect of parent x temperature groups   

Generate a PLS-DA and plot.  
  
```
#assigning datasets 
X <- wide_data

levels(as.factor(X$parent))
levels(as.factor(X$temperature))

X$code<-paste(X$parent, X$temperature)

levels(as.factor(X$code))

Y <- as.factor(X$code) #select treatment names
Y

levels(Y)

X<-X[4:51] #pull only data columns

# run PLSDA 
MyResult.plsda <- plsda(X,Y, ncomp=5) # 1 Run the method with ncomp = groups -1
plotIndiv(MyResult.plsda)    # 2 Plot the samples

cols<-c("orange", "orange3", "orange4", "brown", "brown2", "brown4", "gray", "lightgray", "darkgray") 

pdf("figures/metabolites/PLSDA_interaction.pdf", width=9, height=6)
plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
dev.off() 

plotIndiv(MyResult.plsda, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Temperature", ellipse = FALSE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/PLSDA_interaction.jpeg?raw=true)

Set model to determine VIPs.  

```
MyResult.plsda_interaction <- plsda(X,Y, ncomp=5) 

#view plsda model again
plotIndiv(MyResult.plsda_interaction, col=cols, ind.names = FALSE, legend=TRUE, legend.title = "Parent x Temperature", ellipse = TRUE, title="", style = "graphics", centroid=FALSE, point.lwd = 2, cex=2)
```

Extract VIP scores.        

```
#extract
interaction_VIP <- PLSDA.VIP(MyResult.plsda_interaction, graph=TRUE)

interaction_VIP_DF <- as.data.frame(interaction_VIP[["tab"]])
interaction_VIP_DF

# Converting row names to column
interaction_VIP_table <- rownames_to_column(interaction_VIP_DF, var = "Metabolite")

#filter for VIP > 1
interaction_VIP_1 <- interaction_VIP_table %>% 
  filter(VIP >= 1)

write_csv(interaction_VIP_1, "output/lipids_metabolites/metabolites/Overall_Interaction_VIPs.csv")

#plot
interaction_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Metabolite,VIP,sum))) +
  geom_point() +
  ylab("Metabolite") +
  xlab("VIP Score") +
  ggtitle("Parent x Temperature VIP") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))


pdf("figures/metabolites/Interaction_VIP.pdf", width=4, height=4)
interaction_VIP_1 %>%
  arrange(VIP) %>%
  ggplot( aes(x = VIP, y = reorder(Metabolite,VIP,sum))) +
  geom_point() +
  ylab("Metabolite") +
  xlab("VIP Score") +
  ggtitle("Interaction VIP > 1") +
  theme_bw() + theme(panel.border = element_rect(linetype = "solid", color = "black"), panel.grid.major = element_blank(), #Makes background theme white
                     panel.grid.minor = element_blank(), axis.line = element_line(colour = "black"))
dev.off() 
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/Interaction_VIP.jpeg?raw=true)

These lists represent VIPs that drive separation between parent x temperature groups.  

Make a list of vips.   

```
interaction_vip_list<-interaction_VIP_1%>%
  pull(Metabolite)
```
There are 13 VIPs.   

Plot heatmap of z score of lipids across groups.     

```
# Filter data for VIP metabolites
interaction_vip_data <- filtered_data %>% filter(Name %in% interaction_vip_list)

# Convert counts to Z-scores
interaction_vip_data <- interaction_vip_data %>% 
  mutate(code=paste(parent, temperature))%>%
  group_by(Name) %>% 
  mutate(z_score = scale(area_normalized)) %>% 
  group_by(code, Name)%>%
  summarise(z_score = mean(z_score, na.rm=TRUE))%>%
  ungroup()

interaction_vip_data$code<-factor(interaction_vip_data$code, levels=c("Bleached Ambient", "Bleached Moderate (+3)", "Bleached High (+6)", "Nonbleached Ambient", "Nonbleached Moderate (+3)", "Nonbleached High (+6)", "Wildtype Ambient", "Wildtype Moderate (+3)", "Wildtype High (+6)"))

# Pivot data to have metabolites as rows and lifestage as columns
heatmap_data <- interaction_vip_data %>% 
  dplyr::select(Name, code, z_score) %>%
  spread(key = code, value = z_score)

# Convert to matrix and set rownames
heatmap_matrix <- as.matrix(column_to_rownames(heatmap_data, var = "Name"))

# Create the heatmap
heatmap <- Heatmap(
  heatmap_matrix,
  name = "Z-score",
  col = inferno(10),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = TRUE,
  show_column_names = TRUE,
  #row_split=4,
  row_title = "Metabolite", 
  column_names_gp =  gpar(fontsize = 12, border=FALSE),
  column_names_rot = 45,
  row_gap = unit(1, "mm"), 
  border = TRUE,
  row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)
)

# Draw the heatmap
draw(heatmap)

dev.off()
pdf("figures/metabolites/VIP_heatmap_parentxtemperature.pdf", width=8, height=8)
draw(heatmap)
dev.off()
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/VIP_heatmap_parentxtemperature.jpeg?raw=true)  

View PCA for VIPs.  

```
vip_data_interaction<-filtered_data%>%
  ungroup()%>%
  select(!type)%>%
  select(!protein.ug)%>%
  select(!area_sorbitol)%>%
  select(!area_corrected)%>%
  select(!area)%>%
  filter(Name %in% interaction_vip_list)%>%
  pivot_wider(names_from=Name, values_from=area_normalized)
```

```
vip_data_interaction%>%
    select_if(~ any(is.na(.)))
```

No columns have NA's.    

```
vip_data_interaction$temperature<-factor(vip_data_interaction$temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)"))

vip_data_interaction$parent<-factor(vip_data_interaction$parent, levels=c("Wildtype", "Bleached", "Nonbleached"))
```

```
scaled.pca<-prcomp(vip_data_interaction[c(4:16)], scale=TRUE, center=TRUE) 

fviz_eig(scaled.pca)
```

Prepare a PCA plot  

```
# scale data
vegan <- scale(vip_data_interaction[c(4:16)])

# PerMANOVA 
permanova<-adonis2(vegan ~ parent*temperature, data = vip_data_interaction, method='eu')
permanova
```

adonis2(formula = vegan ~ parent * temperature, data = vip_data_interaction, method = "eu")
                   Df SumOfSqs      R2      F Pr(>F)  
parent              2    28.06 0.04151 1.1445  0.332  
temperature         2    27.70 0.04098 1.1299  0.315  
parent:temperature  4    80.85 0.11960 1.6489  0.099 .
Residual           44   539.39 0.79791                
Total              52   676.00 1.00000               

No effects are significant.  

```
pca_vip_interaction1<-ggplot2::autoplot(scaled.pca, data=vip_data_interaction, frame.colour="parent", loadings=FALSE,  colour="parent", shape="temperature", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_fill_manual(name="Parental Phenotype", values=c("Wildtype"="gray", "Bleached"="orange", "Nonbleached"="brown4"))+
  scale_shape_manual(name="Temperature", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_interaction1

pca_vip_interaction2<-ggplot2::autoplot(scaled.pca, data=vip_data_interaction, frame.colour="temperature", loadings=FALSE,  colour="temperature", shape="parent", loadings.label.colour="black", loadings.colour="black", loadings.label=FALSE, frame=TRUE, loadings.label.size=5, loadings.label.vjust=-1, size=4) + 
  scale_color_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_fill_manual(name="Temperature", values=c("blue", "orange", "red"))+
  scale_shape_manual(name="Parental Phenotype", values=c(19,17,15))+
  theme_classic()+
   theme(legend.text = element_text(size=14), 
         legend.position="right",
        plot.background = element_blank(),
        legend.title = element_text(size=14, face="bold"), 
        axis.text = element_text(size=14), 
        axis.title = element_text(size=14,  face="bold"));pca_vip_interaction2

pca_plots_interaction<-plot_grid(pca_vip_interaction1, pca_vip_interaction2, ncol=2, nrow=1)

ggsave(pca_plots_interaction, filename="figures/metabolites/pca_plots_interactionVIP.jpeg", width=12, height=4)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/pca_plots_interactionVIP.jpeg?raw=true)

## Plot mean values of VIPs 

Plot parent x temperature group VIPs as a mean to see if differences are present.   

```
interaction_means<-filtered_data%>%
  ungroup()%>%
  select(!type)%>%
  select(!protein.ug)%>%
  select(!area_sorbitol)%>%
  select(!area_corrected)%>%
  select(!area)%>%
  filter(Name %in% interaction_vip_list)%>%
  
  group_by(Name, parent, temperature)%>%
  summarise(mean=mean(area_normalized, na.rm=TRUE), se=sd(area_normalized, na.rm=TRUE)/sqrt(length(area_normalized)))%>%
  mutate(temperature=factor(temperature, levels=c("Ambient", "Moderate (+3)", "High (+6)")))%>%
  
  ggplot(aes(x = temperature, y = mean, color = parent)) +
  facet_wrap(~Name, scales="free")+
  geom_line(aes(group=parent), position=position_dodge(0.3))+
  geom_point(position=position_dodge(0.3))+
  geom_errorbar(aes(ymin=mean-se, ymax=mean+se), width=0, position=position_dodge(0.3))+
  scale_color_manual(values=c("orange", "brown4", "gray"))+
  labs(x = "",y = "Mean Normalized Area") +
  theme_classic()+
  theme(
    axis.text.x=element_text(angle=45, vjust=1, hjust=1)
  );interaction_means

ggsave(interaction_means, filename="figures/metabolites/interaction_VIP_means.jpeg", width=12, height=12)
```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/interaction_VIP_means.jpeg?raw=true)

What is the overlap in these lists? Create an upset plot. 

```
# Combine lists into a data frame indicating membership (1 = present, 0 = absent)
upset_data <- data.frame(
  Name = unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)),
  List1 = as.numeric(unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)) %in% parent_vip_list),
  List2 = as.numeric(unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)) %in% temperature_vip_list),
  List3 = as.numeric(unique(c(parent_vip_list, temperature_vip_list, interaction_vip_list)) %in% interaction_vip_list)
)

# Convert data to the format needed for the UpSetR plot
plot_data <- upset_data[, -1]
row.names(plot_data) <- upset_data$Name
names(plot_data) <- c("Parent", "Temperature", "PxT Group")

# Generate the UpSet plot
pdf("figures/metabolites/upset_VIP_list_overlap.pdf", width=6, height=6)
upset(plot_data, sets = c("Parent", "Temperature", "PxT Group"), keep.order = TRUE)
dev.off()

```

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/Hawaii2023/omics/metabolomics/upset.png?raw=true)

No signature of changes in metabolome by parent or temperature treatment. 

# Next steps  

- Determine whether to use area or height data sets for analysis.  
- Dig into lipid results! 
- Decide on use for metabolomics results given low metabolites kept after filtering.  