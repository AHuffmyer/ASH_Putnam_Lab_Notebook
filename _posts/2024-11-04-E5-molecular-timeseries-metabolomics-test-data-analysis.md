---
layout: post
title: E5 molecular timeseries metabolomics test data analysis
date: '2024-11-04'
categories: E5
tags: Metabolomics
---

This post details analysis of test metabolomic samples for the E5 timeseries molecular project.  

## Overview 

In this project, we are conducting multi-omics analyses of the E5 physiology time series project. We sent 4 test samples to the University of Washington Northwest Metabolomics Center in Seattle to see if lipidomics and metabolomics is feasible for these samples.  

Lipid data were successful and we can characterize about 500 lipids. We will now look at the metabolites data.  

Lipidomics data has been returned and in this post I detail my overview analysis of this data to see if the data are of high quality and are as we expect for coral samples.  

The repository [for this project is on GitHub](https://github.com/urol-e5/timeseries_molecular) and the script used to produce the results described here [can be found on GitHub using this direct link](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/scripts/04-test-metabolomics.Rmd).  

The test samples included 2 Acropora (ACR-140 and ACR-165), 1 Pocillopora (POC-68) and 1 Porites (POR-70). All are from the Manava site. 79 and 140 are from time point 2 and 165 and 68 are from time point 1.  

We submitted these samples for targeted metabolomics analyses at UW and they provided the data, which is [on GitHub here](https://github.com/urol-e5/timeseries_molecular/blob/main/M-multi-species/data/metabolomics/test_Metabolites.xlsx).  

## Metabolites measured 

The purpose of test metabolomics is to just see which metabolites can be measured. Here are the steps I took to filter and determine which metabolites we were able to measure. 

1. Load in metabolite abundance data 
2. Keep only metabolites with a quantifiable value in all samples. We cannot separate an NA from a true 0, as some metabolites may be present but just below the detection limit. 
3. Sample concentration was normalized to the internal standard and then to tissue input estimates. 

Following these steps, we were left with 64 metabolites. The UW facility said that the signals were weak and this is fewer metabolites than they normally measure. There was also not enough tissue to obtain a protein concentration value either.  

Here are the metabolites that we were able to characterize:  

- "Alanine"                                
- "Cadaverine"                           
- "Choline"                                
- "Serine"                                
- "Cytosine"                               
- "Leucine /D-Norleucine"                 
- "Ornithine"                              
- "Asparagine"                            
- "Aspartic Acid"                          
- "Tyramine"                              
- "Trigonelline"                           
- "4-Guanidinobutanoate"                  
- "Glutamine"      
- "Lysine"                                
- "Glutamic acid"                          
- "Guanine"                               
- "Histidine"                              
- "Tryptamine"                            
- "Carnitine"                              
- "Phenylalanine"                         
- "Methionine Sulfoxide"                  
- "Arginine"                              
- "N6-Trimethyllysine"                     
- "Caffeine"                              
- "Dimethylarginine (A/SDMA)"              
- "Tryptophan"                            
- "2'-Deoxycytidine"                       
- "Cytidine"                              
- "Uridine"                               
- "isoValerylcarnitine"                   
- "Glycerophosphocholine"                  
- "Hexanoyl-L-Carnitine (6:0 Carnitine)"  
- "Deoxyguanosine"                         
- "Adenosine"                             
- "Inosine"                                
- "Oleamide"                              
- "Retinol"                                
- "Octanoyl-L-Carnitine (8:0 Carnitine)"  
- "5'-Methylthioadenosine"                 
- "GMP"                                   
- "Myristoyl-L-Carnitine (14:0 Carnitine)" 
- "Riboflavin"                            
- "Cholecalciferol"                        
- "Ergocalciferol"                        
- "S-Adenosylmethionine (SAM)"            
- "PAF C-16"                              
- "Nicotinic Acid"                         
- "Adenine"                               
- "Hypoxanthine"                           
- "Phenylacetic Acid"                     
- "Acetylphosphate"                       
- "Hydrocinnamic Acid"                    
- "Xanthine"                               
- "Glycerol-3-P"                          
- "Inositol"                               
- "Azelaic Acid"                          
- "Erythrose-4-Phosphate"                  
- "Pantothenate"                          
- "Ribose-5-P"                             
- "Biotin"                                
- "G1P/F1P/F6P"                            
- "Linoleic Acid"                         
- "IMP"                                    
- "Adenylosuccinate"  

Of these, glutamine, glutamate (glutamic acid), and SAM are obvious metabolites of interest.  

Importantly, central metabolism and core metabolites including glucose, pyruvate, acetyl CoA, lactate, and alpha-ketoglutarate were not high enough to be measured. 

## Outcome 

Overall, the signals were weak and we are missing many core metabolties. Here is a potential path forward:   

- Submit samples with more tissue input. Run all samples for lipids. Have the facility run 5-10 samples to start for metabolomics and see what the data look like at that point. If the data look good, we go forward with all samples for metabolites. If it doesn't look good, we only do lipids. 

