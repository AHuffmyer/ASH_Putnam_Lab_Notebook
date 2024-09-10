---
layout: post
title: Correcting subsampling error in Mcap2020 ITS2 code and revise results
date: '2024-03-24'
categories: Mcapitata_EarlyLifeHistory_2020
tags: ITS2 R
---  

This post describes correcting code and revising analyses for *Montipora capitata* 2020 developmental time series project.  

# Overview 

While conductings some tests for subsampling thresholds, I found an error in my ITS2 code. The error was that sequence abundance was being transformed to relative abundance prior to subsampling, and therefore it was not subsampling at the desired sequence count threshold. I have corrected this error and have summarised the revised results below.  

Prior to this revision, we saw no change in symbiont communities across development. We can now see significant changes across development, but these changes are very minor and the major ITS2 profiles show only minimal shifts between eggs and later life stages.   

The script for this analysis can be found [on GitHub here](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/blob/master/Mcap2020/Scripts/ITS2/ITS2_strains.Rmd).   

# Subsampling 

The dataset is loaded and formatted as a phyloseq object. 

```
phyloseq-class experiment-level object
otu_table()   OTU Table:         [ 77 taxa and 39 samples ]
sample_data() Sample Data:       [ 39 samples by 4 sample variables ]
tax_table()   Taxonomy Table:    [ 77 taxa by 2 taxonomic ranks ]

```

We have 39 samples and 77 DIV's (taxa) detected. The taxa refers to DIVs. We have 2 "taxonomic ranks", which refer to classification by DIV and classification by "clade", which is at the level of genera of either Cladocopium or Durusdinium.  

I then displayed the number of reads in each sample.  

```
WSH049 WSH046 WSH047 WSH048 WSH052 WSH050 WSH051 WSH056 WSH053 WSH054 WSH055 
 40139  23586  26560  24943  33530  38322  36655  30358  28554  39412  29099 
WSH067 WSH065 WSH066 WSH068 WSH064 WSH061 WSH063 WSH062 WSH069 WSH071 WSH070 
 28385  18660  20729  27558  23212  26645  20934  25255  27587  34275  23199 
WSH072 WSH078 WSH077 WSH079 WSH080 WSH083 WSH081 WSH084 WSH082 WSH075 WSH076 
 30530  27465  27625  27259  10551  32705  41857  28159  31364  27662  32939 
WSH073 WSH074 WSH060 WSH058 WSH059 
 23421  24076  58846  25766  27251 
```

There are >10,000 sequences in all samples. We will subsampling all samples by randomly selecting reads to the read depth of the sample with the lowest read depth of 10,551. 

I then used subsampling in phyloseq to achieve even read depth with replacement.  

```
GP1_rare<-rarefy_even_depth(GP1, sample.size = min(sample_sums(GP1)),
  rngseed = TRUE, replace = TRUE, trimOTUs = TRUE, verbose = TRUE)
```

After this, all samples are at the same read depth. 

```
WSH049 WSH046 WSH047 WSH048 WSH052 WSH050 WSH051 WSH056 WSH053 WSH054 WSH055 
 10551  10551  10551  10551  10551  10551  10551  10551  10551  10551  10551 
WSH067 WSH065 WSH066 WSH068 WSH064 WSH061 WSH063 WSH062 WSH069 WSH071 WSH070 
 10551  10551  10551  10551  10551  10551  10551  10551  10551  10551  10551 
WSH072 WSH078 WSH077 WSH079 WSH080 WSH083 WSH081 WSH084 WSH082 WSH075 WSH076 
 10551  10551  10551  10551  10551  10551  10551  10551  10551  10551  10551 
WSH073 WSH074 WSH060 WSH058 WSH059 
 10551  10551  10551  10551  10551 
```

No OTU's were removed, which means that all OTU's present before subsampling are still present. 

# DIV-level analysis 

### Community composition  

I first plotted an NMDS of symbiont community composition at the DIV level using Bray-Curtis dissimilarity.  

```
Call:
metaMDS(comm = veganifyOTU(physeq), distance = distance) 

global Multidimensional Scaling using monoMDS

Data:     wisconsin(sqrt(veganifyOTU(physeq))) 
Distance: bray 

Dimensions: 2 
Stress:     0.2166622 
Stress type 1, weak ties
Best solution was repeated 2 times in 20 tries
The best solution was from try 15 (random start)
Scaling: centring, PC rotation, halfchange scaling 
Species: expanded scores based on ‘wisconsin(sqrt(veganifyOTU(physeq)))’ 
```

The stress value is 0.216, which is a poor goodness of fit. Stress is a measure of the degree to which the distance between samples in reduced dimensional space (usually 2-dimensions) corresponds with the actual multivariate distance between the samples. Therefore, a lower stress value is considered a "better fit".  

![](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/blob/master/Mcap2020/Figures/ITS2/nmds_its2_div.jpeg?raw=true)   

I then ran a PERMANOVA on DIV community composition using `perm1<-adonis2(phyloseq::distance(GP1_rare, method="bray") ~ lifestage,
       data = metadata)`.  
       
```
Permutation test for adonis under reduced model
Terms added sequentially (first to last)
Permutation: free
Number of permutations: 999

adonis2(formula = phyloseq::distance(GP1_rare, method = "bray") ~ lifestage, data = metadata)
          Df SumOfSqs      R2      F Pr(>F)    
lifestage  9 0.090070 0.48981 2.9868  0.001 ***
Residual  28 0.093818 0.51019                  
Total     37 0.183888 1.00000    
```

This PERMANOVA indicates that there is significant variation in symbiont communities between lifestages. 

Next, I plotted a heat map of each DIV, filtering out any DIV with <1% relative abundance.  

There were 77 OTU's present before filtering and 11 after filtering. A majority of DIVs detected were rare. 

```
GP1_prune = filter_taxa(GP1_rare, function(x) mean(x) > 0.01, TRUE)
GP1_prune   
```

Run an NMDS again and plotted.  

```
Call:
metaMDS(comm = veganifyOTU(physeq), distance = distance) 

global Multidimensional Scaling using monoMDS

Data:     wisconsin(sqrt(veganifyOTU(physeq))) 
Distance: bray 

Dimensions: 2 
Stress:     0.2166622 
Stress type 1, weak ties
Best solution was repeated 2 times in 20 tries
The best solution was from try 17 (random start)
Scaling: centring, PC rotation, halfchange scaling 
Species: expanded scores based on ‘wisconsin(sqrt(veganifyOTU(physeq)))’ 
```

Stress value is still 0.216.  

![](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/blob/master/Mcap2020/Figures/ITS2/nmds_its2_div_filtered.jpeg?raw=true) 

There is not strong separation between lifestages. The egg life stages has particularly low variability within this stage compared to other life stages.  

A PERMANOVA reveals significant separation by lifestage as before.

```
Permutation test for adonis under reduced model
Terms added sequentially (first to last)
Permutation: free
Number of permutations: 999

adonis2(formula = phyloseq::distance(GP1_prune, method = "bray") ~ lifestage, data = metadata_filt)
          Df SumOfSqs     R2      F Pr(>F)    
lifestage  9 0.090218 0.4896 2.9843  0.001 ***
Residual  28 0.094051 0.5104                  
Total     37 0.184269 1.0000                  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

Next, I generated a heatmap of relative abundance data for each DIV as a mean of each lifestage (n=3-4 replicates per lifestage).   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/ITS2/20240429_heatmap.png?raw=true) 

This shows that the dominant taxa is C13 with D1 as the next most abundant DIV. Two of the later lifestages, larvae (231 hpf) and attached recruits (183 hpf) appear to have higher C31 abundance that may be driving the multivariate differences in lifestages.  

I also generated a heatmap of all DIV's, but this wasn't very helpful because of the many rare DIV's.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/ITS2/20240429_heatmap_allDIV.png?raw=true)   

### Diversity metrics 

I also visualized alpha diversity (the number of DIV's present) in each sample using Shannon and Inverse Simpson methods.  

![](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/blob/master/Mcap2020/Figures/ITS2/alpha_rare.png?raw=true) 

I tested for differences in these metrics across lifestages using anova tests.  

```
> summary(aov(Shannon~lifestage, data=alpha_rare))
            Df  Sum Sq  Mean Sq F value Pr(>F)
lifestage    9 0.03871 0.004301   1.173   0.35
Residuals   28 0.10271 0.003668               
> summary(aov(InvSimpson~lifestage, data=alpha_rare))
            Df Sum Sq Mean Sq F value Pr(>F)
lifestage    9  2.398  0.2665   1.408  0.232
Residuals   28  5.297  0.1892               
> summary(aov(Chao1~lifestage, data=alpha_rare))
            Df Sum Sq Mean Sq F value Pr(>F)
lifestage    9  78.98   8.776   1.582  0.169
Residuals   28 155.33   5.548   
```

There was no significant change in alpha diversity across lifestages. This indicates that there is not a reducion or increase in the number of DIV's present in early life stages across development.  

Finally, I ran DESEQ2 analyses to look for differential DIV's at adjusted p<0.05 and log2fold change >1.  

```
library("DESeq2")
diagdds = phyloseq_to_deseq2(GP1_rare_untransformed, ~ lifestage)
diagdds = DESeq(diagdds, test="Wald", fitType="parametric")

res = results(diagdds, cooksCutoff = FALSE)
alpha = 0.05
l2fc = 1
sigtab = res[which(res$padj < alpha), ]
sigtab = sigtab[which(sigtab$log2FoldChange > abs(l2fc)), ]

sigtab = cbind(as(sigtab, "data.frame"), as(tax_table(GP1_rare_untransformed)[rownames(sigtab), ], "matrix"))
head(sigtab)
```

There are two differential taxa.  

```
      baseMean log2FoldChange    lfcSE     stat       pvalue         padj
C3     36.09249      18.769396 2.953328 6.355339 2.079679e-10 1.601353e-08
1378_C 22.07770       7.059482 2.035066 3.468919 5.225560e-04 1.341227e-02
          DIV clade
C3         C3     C
1378_C 1378_C     C
```

These taxa are rare, with <1% relative abundance. 

Finally, I plotted relative abundance of these taxa across lifestages.  

```
df<-as.data.frame(GP1_rare@otu_table)

deg_list <- rownames(sigtab)
degs<-df[,which(names(df) %in% deg_list)]
degs$lifestage<-metadata_filt$lifestage[match(rownames(df), rownames(metadata_filt))]

```

![](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/blob/master/Mcap2020/Figures/ITS2/deg_taxa.png?raw=true)  

The results for change in relative abundance (y) are highly variable. Due to the low abundance and variable change (no clear direction across development), we will not analyze further. The one interesting thing is that these taxa are not present in the eggs, and this is likely why is it detected as differential in the analysis.  

# Profile-level analysis 

After DIV analyses, I then analyzed ITS2 communities at the level of ITS2 type profile. 

I calculated relative abundance and plotted across life stages.  

Mean relative abundance across all lifestages was:  

```
  its2_type_profile        mean     sd     n      se
  <chr>                   <dbl>  <dbl> <int>   <dbl>
1 C31-C17d-C21-C21ac-C31a 0.560 0.0260    38 0.00422
2 D1-D4-D6-D1ab-D17d      0.440 0.0260    38 0.00422

means
# A tibble: 10 × 3
   lifestage                     C31-C17d-C21-C21ac-C3…¹ `D1-D4-D6-D1ab-D17d`
   <fct>                                           <dbl>                <dbl>
 1 Egg (1 hpf)                                     0.530                0.470
 2 Embryo (5 hpf)                                  0.547                0.453
 3 Embryo (38 hpf)                                 0.537                0.463
 4 Embryo (65 hpf)                                 0.571                0.429
 5 Larvae (93 hpf)                                 0.581                0.419
 6 Larvae (163 hpf)                                0.576                0.424
 7 Larvae (231 hpf)                                0.586                0.414
 8 Metamorphosed Polyp (231 hpf)                   0.567                0.433
 9 Attached Recruit (183 hpf)                      0.545                0.455
10 Attached Recruit (231 hpf)                      0.560                0.440
# ℹ abbreviated name: ¹​`C31-C17d-C21-C21ac-C31a`
```

I also ran an ANOVA on relative abundance across lifestages.  

```
> model<-rel_abund_data %>% 
+   select(its2_type_profile, Abundance, lifestage, Sample)%>%
+   spread(its2_type_profile, Abundance)%>%
+   aov(`C31-C17d-C21-C21ac-C31a`~lifestage, data=.);summary(model)
            Df  Sum Sq   Mean Sq F value  Pr(>F)   
lifestage    9 0.01336 0.0014841   3.563 0.00464 **
Residuals   28 0.01166 0.0004166                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

Post hoc results are:  

```
emm<-emmeans(model, ~lifestage)

cld(emm, Letters=letters)
 lifestage                     emmean     SE df lower.CL upper.CL .group
 Egg (1 hpf)                    0.530 0.0102 28    0.509    0.551  a    
 Embryo (38 hpf)                0.537 0.0102 28    0.516    0.558  ab   
 Attached Recruit (183 hpf)     0.545 0.0102 28    0.524    0.566  ab   
 Embryo (5 hpf)                 0.547 0.0102 28    0.526    0.568  ab   
 Attached Recruit (231 hpf)     0.560 0.0118 28    0.536    0.584  ab   
 Metamorphosed Polyp (231 hpf)  0.567 0.0118 28    0.543    0.591  ab   
 Embryo (65 hpf)                0.571 0.0102 28    0.550    0.592  ab   
 Larvae (163 hpf)               0.576 0.0102 28    0.555    0.597  ab   
 Larvae (93 hpf)                0.581 0.0102 28    0.560    0.602   b   
 Larvae (231 hpf)               0.586 0.0102 28    0.565    0.607   b  
```

The only differences are between eggs and 231 hpf larvae (p=0.0173) and eggs and 93 hpf larvae (p=0.0420).  

Here is a plot of abundance across lifestages.  

![](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/blob/master/Mcap2020/Figures/ITS2/profile_plot_means.png?raw=true)   

There are minor shifts in ITS2 profiles. There is not an overall pattern of change or increase/decrease in members of the symbiont community across development.  

I then looked at C:D ratio, which is a metric used frequently in *Montipora capitata* which is typically dominated by Cladocopium and Durusdinium.  

```
ratio_plot<-rel_abund_data%>%
  dplyr::select(its2_type_profile, Abundance, lifestage, Sample)%>%
  pivot_wider(values_from = Abundance, names_from = its2_type_profile)%>%
  mutate(ratio=`C31-C17d-C21-C21ac-C31a`/`D1-D4-D6-D1ab-D17d`)%>%
  group_by(lifestage)%>%
  summarise(mean=mean(ratio, na.rm=TRUE), se=sd(ratio, na.rm=TRUE)/sqrt(length(ratio)))%>%
  
  ggplot(aes(x=lifestage, y=mean))+
  geom_point()+
  geom_errorbar(aes(ymin=mean-se, ymax=mean+se), width=0.2)+
  ylab("C:D Ratio")+
  ylim(0,2)+
  xlab("Life Stage")+
  theme_classic()+
  theme(
    text=element_text(color="black", size=12), 
    axis.text=element_text(color="black", size=11), 
    axis.text.x=element_text(angle=45, hjust=1, vjust=1), 
    axis.title=element_text(face="bold")
  );ratio_plot
```

I ran an ANOVA on this data.  

```
model<-rel_abund_data%>%
  dplyr::select(its2_type_profile, Abundance, lifestage, Sample)%>%
  pivot_wider(values_from = Abundance, names_from = its2_type_profile)%>%
  mutate(ratio=`C31-C17d-C21-C21ac-C31a`/`D1-D4-D6-D1ab-D17d`)%>%
  aov(ratio~lifestage, data=.);summary(model)
```

There was significant variation in C:D ratio across development.  

```
            Df Sum Sq Mean Sq F value Pr(>F)  
lifestage    9 0.3829 0.04254   3.056 0.0112 *
Residuals   28 0.3897 0.01392                 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1  
```

There is only one significant pairwise difference between eggs and one later lifestage in larvae.  

```
Egg (1 hpf) - Larvae (231 hpf)                              -0.3104 0.0834 28  -3.721  0.0254

 lifestage                     emmean     SE df lower.CL upper.CL .group
 Egg (1 hpf)                     1.13 0.0590 28     1.01     1.25  a    
 Embryo (38 hpf)                 1.16 0.0590 28     1.04     1.28  ab   
 Attached Recruit (183 hpf)      1.20 0.0590 28     1.08     1.32  ab   
 Embryo (5 hpf)                  1.21 0.0590 28     1.09     1.33  ab   
 Attached Recruit (231 hpf)      1.28 0.0681 28     1.14     1.42  ab   
 Metamorphosed Polyp (231 hpf)   1.31 0.0681 28     1.17     1.45  ab   
 Embryo (65 hpf)                 1.33 0.0590 28     1.21     1.45  ab   
 Larvae (163 hpf)                1.37 0.0590 28     1.24     1.49  ab   
 Larvae (93 hpf)                 1.39 0.0590 28     1.27     1.51  ab   
 Larvae (231 hpf)                1.44 0.0590 28     1.32     1.56   b  
```

This is what the C:D ratios look like:  

![](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/blob/master/Mcap2020/Figures/ITS2/CD_ratio_means.png?raw=true)    

There is perhaps a trend for increased C:D ratios in larvae, but this is variable and not significant. This matches the slight increase in C31 relative abundance in the heatmaps above.  