---
layout: post
title: Revising RDA plots for E5 physiology manuscript
date: '2025-04-07'
categories: E5
tags: Physiology R 
---

Today I revised our redundancy analysis (RDA) plots to better visualize the relationship between physiological metrics and environmental variables.    

# Overview 

We have previously performend [RDA analyses for the E5 project]() to characterize the variance in physiology explained by site, species, and time point. Seasonality played a large role in physiological responses and we therefore wanted to explore the seasonal effects of light and temperature on physiology.  

Today, I visualized the RDA results using `ordiplots`. 

## Code 

The code used to do all of the [RDA analyses can be found here](https://github.com/urol-e5/timeseries/blob/master/time_series_analysis/11_rda_environment_analysis.Rmd).  

Here is an example of code for *Acropora pulchra* host metrics.  

```
acr.comm.rda.host<-rda(acr_data_temp[, c(5,8,10,13,16)] ~ solar_mean + temp_mean, 
                       data=acr_data_temp, scale=TRUE)


pdf("figures/Multivariate/RDA-plots/acr_host_rda.pdf", width=6, height=6)

# Extract site and species scores
site_scores <- scores(acr.comm.rda.host, display = "sites", scaling = 2)
species_scores <- scores(acr.comm.rda.host, display = "species", scaling = 2)
env_scores <- acr.comm.rda.host$CCA$biplot

sample_names <- rownames(site_scores)
group_var <- factor(gsub(".*(TP[1-4]).*", "\\1", sample_names),
                    levels = c("TP1", "TP2", "TP3", "TP4"),
                    labels = c("January", "March", "September", "November"))

# Manually start the plot
plot(site_scores[,1], site_scores[,2], type = "n",
     xlim = c(-3, 2), ylim = c(-3, 3),
     xlab = "RDA1", ylab = "RDA2", main = "ACR Host RDA")

# Ellipses
ordiellipse(site_scores, group = group_var, kind = "sd", 
            conf = 0.95, draw = "polygon", border = NA,
            col = c("orange", "red", "blue", "lightblue"), alpha = 80)

# Species points and labels
points(species_scores[,1], species_scores[,2], pch = 3, col = "black", cex = 1.2)
text(species_scores[,1], species_scores[,2], labels = rownames(species_scores), col = "black", pos = 3, cex = 0.8)

# Environmental vectors
arrows(0, 0, env_scores[,1], env_scores[,2], length = 0.2, col = "black")
text(env_scores[,1], env_scores[,2], labels = rownames(env_scores), col = "black", pos = 3, cex = 0.8)

# Legend
legend("topright", legend = levels(group_var), 
       fill = c("orange", "red", "blue", "lightblue"), bty = "n")

dev.off()
```

## Results 

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/E5_physiology/Fig8_rda.png?raw=true)

**Fig 6. Variance partitioning analysis of temperature and light seasonal effects on physiology.** (A, D, G) Percent variance explained by light (sun icon) and temperature (thermometer icon) in multivariate physiology of the host and symbiont (x-axis) for each genus (A = Acropora pulchra, D = Pocillopora spp., G = Porites spp.) as analyzed using redundancy analyses (RDA). Light and temperature effects on host and symbiont physiology were significant for all genera at P<0.01. RDA plots showing how light (sun icon and associated arrow) and temperature (thermometer icon with associated arrow) drive host (B, E, H) and symbiont (C, F, I) physiology for each genus. + indicates the association of each physiological metric with light and temperature drivers with metrics closer to the arrows being more strongly associated with the respective environmental variable. Metrics in opposite directions of environmental variable arrows are negatively associated and metrics in the same direction as the arrows are positively associated. Metrics that are further from the plot origin have a stronger response to one or more environmental variables. Metrics near or at the plot origin have weak associations with environmental variables. Longer environmental variable arrows indicate the strength of correlations with associated physiological metrics. Ellipses represent 95% confidence intervals (orange = January, red = March, dark blue = September, light blue = November). See legend key for abbreviations of physiological metrics. 

I like these plots because they can tell us a few things including: 

- Which physiological metrics positively or negatively correlate with specific environmental variables? 
- Which environmental variables are most important in driving physiology? 
- Which physiological variables are important in driving the seasonal change in physiology? 
