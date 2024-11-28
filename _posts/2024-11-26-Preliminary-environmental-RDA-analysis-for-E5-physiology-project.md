---
layout: post
title: Preliminary environmental RDA analysis for E5 physiology project
date: '2024-11-26'
categories: E5
tags: Physiology R Environmental
---

Today I ran redundancy (RDA) and variance partioning analyses to examine environmental drivers of physiology across seasons for the [E5 Coral Project](https://e5coral.org/). he GitHub repository for this paper [can be found here](https://github.com/urol-e5/timeseries/tree/master). Check out the `README` for this project in the GitHub repository for more information.  

# Overview 

In [previous analyses](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Revising-E5-timeseries-results-restructuring-figures-and-filtering-univariate-data/) and using [this script](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/7_variance_partitioning.Rmd) we show that coral and symbiont physiology of three species in Moorea is strongly responsive to seasonal time point. 

![](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/Figures/Multivariate/VarPart_Panel.png?raw=true)

Today, I ran an analysis to examine the effect of specific environmental variables across seasons on host and symbiont physiology that may explain this response to season.  

The scripts used in this post can be found on GitHub:  

- [Environmental data preparations and summary](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/timeseries_environmental_data_analyses.Rmd) 
- [RDA analysis](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/11_rda_environment_analysis.Rmd)  

## Environmental data sources 

In [previous analyses](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/E5-Environmental-Timeseries-Analysis-Part-3/) I described the environmental data collected as part of this project during the 2020 sampling time series. From this data, we are using the most reliable and comprehensively characterized metric of temperature.  

Because we want to examine other seasonal variation in the environment in addition to temperature, I pulled solar radiation (light) and rainfall data from the [Moorea LTER data archive](https://portal.edirepository.org/nis/home.jsp). I downloaded data from the following source: 

```
Moorea Coral Reef LTER, L. Washburn, and A. Brooks. 2024. MCR LTER: Coral Reef: Gump Station Meteorological Data, ongoing since 2006 ver 50. Environmental Data Initiative. https://doi.org/10.6073/pasta/c41e5ef3131a9328ccee7097a4221f7a (Accessed 2024-11-26).
```

At the DOI in the citation, there is R code to download and load this data. I used this script to read in the data. 

I chose this data set because there is data from the 2020 time frame in which we conducted this study. Note that light is measured as solar radiation (kWhm-2) rather than in water PAR measurements. PAR measurements available from the MCR as a time series are only available starting in 2021. Therefore, I chose solar radiation as a proxy for light across seasons.  

This data set also captures cumulative rainfall (mm) that I examined as well.  

Overall, **I will use temperature data collected during our study and I will use solar and rain data from the MCR source cited above as environmental characteristics across 2020 seasons.**  

## Environmental characteristics data analysis  

I then calculated metrics of temperature, solar radiation, and rainfall to use in our analysis using [this script](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/timeseries_environmental_data_analyses.Rmd).  

First, I calculated mean and standard deviation of each metric for respective time points. 

- mean_solar_rad_kwpm2 (sunlight) - mean and standard deviation 
- cumulative_rainfall_mm (rainfall) - mean and standard deviation 
- temperature - mean and standard deviation 

We will use mean and standard deviation of temperature from 

I first used the MCR data to generate timepoint average light and rainfall as the 4 weeks leading up to the end of the sampling period. 

- Timepoint 1 sampling = 28 Dec 2019 - 14 Jan 2020 [use data from 15 Dec-15 Jan]
- Timepoint 2 sampling = 27 Feb - 14 March [use data from 15 Feb-14 Mar] - no data available for this range for solar and rainfall, using April 1-30 for light and rain fall, using correct range for temp 
- Timepoint 3 sampling = 8 Sept - 28 Sept [use data from 29 Aug-28 Sept]
- Timepoint 4 sampling = 30 Oct - 21 Nov [use data from 22 Oct-21 Nov]

Data of the mean value for each day were then averaged across the respective date period.  

I then ran a loop to calculate temperatures for each time period and the data were then scaled so that each metric is bound between -1 and 1 to account for differences in scale of each metric. The data are on [GitHub here](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/Output/scaled_environment_characteristics_RDA.csv).  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/Seasonal_averages.png?raw=true) 

As seen in previous analyses, temperature peaks in the March time point and is lowest in September. Solar radiation increases from Jan-September and is highest in September, with lowest radiation in November. Rainfall was highest in January and November, but was highly variable.  

## Testing for metric autocorrelation 

Before running RDA analyses, I tested for correlation between the calculated environmental characteristics.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/cor-plot.png?raw=true) 

This plot shows the r values from the correlation and I also generated p-values from the correlation matrix. There was significant autocorrelation between the mean and standard devation of each metric. Further, there was significant negative correlation between rainfall and solar radiation (more rainfall in winter season). Therefore, I selected the following metrics that were not already correlated to each other:  

- Mean solar radiation 
- Mean temperature 

These two metrics are then used in RDA analyses to correlate with host and symbiont physiology in each species. 

## RDA and variance partioning analyses  

The full script for this analysis [can be found here](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/11_rda_environment_analysis.Rmd). Here, I will present the highlights.  

I then ran an RDA and variance partioning analysis for each species for both host and symbiont physiology. Here is an example of that script and the type of results we get.  

```
names(acr_data_temp)
acr.comm.rda.host<-rda(acr_data_temp[, c(5,8,11,13,18)] ~ solar_mean + temp_mean, 
                       data=acr_data_temp, scale=TRUE)

##Check model significance
anova(acr.comm.rda.host)
# Permutation test for rda under reduced model
# Permutation: free
# Number of permutations: 999
# 
# Model: rda(formula = acr_data_temp[, c(5, 8, 11, 13, 18)] ~ solar_mean + temp_mean, data = acr_data_temp, scale = TRUE)
#          Df Variance      F Pr(>F)    
# Model     2   0.9276 9.2249  0.001 ***
# Residual 81   4.0724       

plot(acr.comm.rda.host)

##Check variance explained by model
(summary(acr.comm.rda.host)$constr.chi/summary(acr.comm.rda.host)$tot.chi)*100
#18.55% of the variance in host physiology is constrained by metrics 

RsquareAdj(acr.comm.rda.host)$adj.r.squared*100
#Adjusted: The model explains 16.54% of the variation in host physiology 

summary(acr.comm.rda.host)$cont
#RDA1 explains 15.16% of total variance 
#RDA2 explains 3.39% of total variance 

summary(acr.comm.rda.host)$concont
#RDA1 explains 81.73% of constrained variance 
#RDA2 explains 18.27% of constrained variance 

anova(acr.comm.rda.host, by="terms")

# Permutation test for rda under reduced model
# Terms added sequentially (first to last)
# Permutation: free
# Number of permutations: 999
# 
# Model: rda(formula = acr_data_temp[, c(5, 8, 11, 13, 18)] ~ solar_mean + temp_mean, data = acr_data_temp, scale = TRUE)
#            Df Variance       F Pr(>F)    
# solar_mean  1   0.6557 13.0412  0.001 ***
# temp_mean   1   0.2719  5.4087  0.002 ** 
# Residual   81   4.0724  

##Run varpart function with responses and explanatory variables of interest
varpart_acr.comm.rda.host<-varpart(acr_data_temp[, c(5,8,11,13,18)], acr_data_temp$temp_mean, acr_data_temp$solar_mean)

##Check variance explained by individual fractions
varpart_acr.comm.rda.host$part
# Partition table:
#                      Df R.squared Adj.R.squared Testable
# [a+c] = X1            1   0.03190       0.02009     TRUE
# [b+c] = X2            1   0.08411       0.07294     TRUE
# [a+b+c] = X1+X2       2   0.11719       0.09539     TRUE
# Individual fractions                                    
# [a] = X1|X2           1                 0.02245     TRUE
# [b] = X2|X1           1                 0.07529     TRUE
# [c]                   0                -0.00235    FALSE
# [d] = Residuals                         0.90461    FALSE

acr_host_temp<-2.25
acr_host_light<-7.53

```

These results produce information including model significance, model term significance, variance constrained by terms, and variance in multivariate physiology explained by terms. Note thatt in the partition table example above, `X1|X2` indicates the variance explained by temperature accounting for light and `X2|X1` indicates the variance explained by light accounting for temperature.   

Here is a table with all model results for analysis of all species and fractions.  

**Table S15.** Redundancy analysis of variance constrained in symbiont and host physiology by environmental characteristics. p-values of each term from redundancy analysis (RDA) and model analysis of variance constrained in host and symbiont of each genus by host and symbiont responses with holobiont identity included in models for each biological level (Fig 3A). Main effects were mean light (solar radiance in kWh m-2) and mean temperature (°C) that were scaled for analysis. Gray boxes indicate no significant effect. p-values shown for effects where p<0.05.		
	
| Biological Level   | Genus       | Main Effect | DF | Variance | F     | p value |
|--------------------|-------------|-------------|----|----------|-------|---------|
|   Host Responses   |   Acropora  | light       |  1 |     0.66 | 13.04 |   0.001 |
|                    |             | temperature |  1 |     0.27 |  5.41 |   0.002 |
|                    | Pocillopora | light       |  1 |     0.01 |  9.73 |   0.001 |
|                    |             | temperature |  1 |     0.01 | 19.00 |   0.001 |
|                    |   Porites   | light       |  1 |     0.01 |  4.18 |   0.007 |
|                    |             | temperature |  1 |     0.05 | 30.58 |   0.001 |
| Symbiont Responses |   Acropora  | light       |  1 |     0.30 | 52.63 |   0.001 |
|                    |             | temperature |  1 |     0.11 | 18.75 |   0.001 |
|                    | Pocillopora | light       |  1 |     0.19 | 38.58 |   0.001 |
|                    |             | temperature |  1 |     0.06 | 12.04 |   0.001 |
|                    |   Porites   | light       |  1 |     0.11 | 20.67 |   0.001 |
|                    |             | temperature |  1 |     0.05 |  9.17 |   0.001 |

**Main result**: The main result is that light and temperature significantly correlated with host and symbiont physiology in all species.  

The ordination plots below show the loadings of light and temperature metrics on multivariate physiology.   

*Acropora*  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/acr-host.png?raw=true) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/acr-sym.png?raw=true) 

*Pocillopora*  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/poc-host.png?raw=true) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/poc-sym.png?raw=true) 

*Porites*  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/por-host.png?raw=true) 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/por-sym.png?raw=true) 

From this, I used the variance partitioning results to see *how much* of an affect each metric had on physiology. I extracted the variance explained by light and temperature from each RDA/variance partioning analysis and asembled them into one plot.  

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_Environmental/rda/environment_variance_partioning.png?raw=true) 

Cool! From this we can see that symbiont physiology is much more affected by light than temperature *and* that symbiont physiology is more affected by light than the host. There are specific specific patterns that are also interesting. Specifically, *Acropora* symbionts were the most affected by light of all the species and in *Porites*, the host was more impacted by temperature than light - the opposite patterns was observed in *Pocillopora* and *Acropora*.  

These analyses will go into our physiology manuscript. I think it raises some interesting points about the responsiveness of symbionts to light. In species with more flexible associations (*Acropora* and *Pocillopora*) with symbiont taxa, the symbiont physiology was more responsive to light across seasons than in *Porites*, which has high fidelity associations with one symbiont taxa.  

