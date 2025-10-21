---
layout: post
title: Resazurin EBV correlations for Taylor Shellfish screening
date: '2025-10-21'
categories: RobertsLab_Oysters 
tags: Resazurin Oyster Cgigas
---

This post details analysis of Taylor Shellfish family resazurin screening  metrics with estimated breeding values (EBV) of performance.   

# Overview 

See [this previous post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Family-Screening-Taylor-Shellfish/) that details our resazurin trials for this screening and the protocols that we used and [this post](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Resazurin-Family-Screening-Taylor-Shellfish-with-individual-size-normalization/) for my analysis generating resazurin metrics.    

Check out our [oyster resazurin landing page](https://robertslab.github.io/resazurin-assay-development/) for more information on the protocol and approach used. 

All data from these trials is located in the [Resazurin Family Screening GitHub repo here](https://github.com/RobertsLab/resazurin-family-screenings).  

Today, I ran correlations etween EBV values and resazurin metrics.   

# tl;dr 

There is no significant correlation between any resazurin metric or any performance/EBV value.   

![](https://github.com/AHuffmyer/ASH_Putnam_Lab_Notebook/blob/master/images/NotebookImages/oysters/resazurin/20251021/ebv-metrics-correlation-heatmap.png?raw=true)

Here is a key for the EBV metrics: 

- `HI`=shell height index family value
- `MYW`=meat yield family value
- `POMS`=lab survival after OsHV-1 challenge family value
- `SUR`=field survival family value
- `SUR_LIVE`=survival, live individual value
- `WI`=shell width family value
- `WTT`=Wet weight family value
- `WTT_SEL`=Wet weight individual value

Here is a key for the resazurin metrics: 

- `day1_slope`=metabolic rate on day 1 of testing
- `day1_auc`=total metabolism on day 1 of testing
- `day2_slope`=metabolic rate on day 2 of testing
- `day2_auc`=total metabolism on day 2 of testing
- `cumulative_mean_slope`=mean of metabolic rate from days 1 and 2
- `cumulative_mean_auc`=mean of total metabolism from days 1 and 2
- `day1_vs_day2_ratio_slope`=ratio of metabolic rate from day 1 to day 2
- `day1_vs_day2_ratio_auc`=ratio of total metabolism from day 1 to day 2
- `day1_vs_day2_perc_change_slope`=percent change of metabolic rate from day 1 to day 2
- `day1_vs_day2_perc_change_auc`=percent change of total metabolism from day 1 to day 2

There is a trend for a positive correlation between survival in lab tests following OsHV-1 exposure and metabolism. There is also a trend for a negative correlation between field survival (family and individual) and metabolism. However, these are not significant and weak correlations.  

## Code 

Here is the code I used to generate this plot. 

This script conducts correlations between EBV values and resazurin metrics from individual size normalization.  

#### Set up 

Set up workspace, set options, and load required packages.   
 
```
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE)
```

Load libraries. 

```
library(MASS) 
library(tidyverse)
library(ggplot2)
library(readxl)
library(cowplot)
library(lme4)
library(lmerTest)
library(car)
library(effects)
library(emmeans)
library(tools)
library(pracma)
library(rptR)
library(fda)
library(nls2)
library(purrr)
library(broom)
library(pheatmap)
library(vegan)
library(ggridges)
library(ggpubr)
library(broom.mixed)
```

#### Load data 

```
ebv<-read_xlsx(path="taylor-shellfish-sept2025/data/ebv/TAY_EBV_Report.xlsx")%>%select(FAM_ID, HI, MYW, POMS, SUR, SUR_LIVE, WI, WTT, WTT_SEL)%>%mutate(family = str_extract(FAM_ID, "\\d{2}$"))
head(ebv)
fams1<-c(ebv$family)
```

- HI=shell height index family value
- MYW=meat yield family value
- POMS=lab survival after OsHV-1 challenge family value
- SUR=field survival family value
- SUR_LIVE=survival, live individual value
- WI=shell width family value
- WTT=Wet weight family value
- WTT_SEL=Wet weight individual value

Load in resazurin data

```
index<-read_csv("taylor-shellfish-sept2025/output/taylor_screening_metrics_individual-normalized.csv")%>%mutate(family=as.character(family))
head(index)

fams2<-c(index$family)
```

Check family list

```
setdiff(fams1, fams2)
setdiff(fams2, fams1)
intersect(fams1, fams2)
```

Both are the same. We have 38 families total. 

Merge datasets.

```
data<-left_join(ebv, index)%>%select(family, everything())%>%select(!FAM_ID)
```

##### Correlation between performance and resazurin metrics 

Make a list of resazurin metrics.
 
```
list<-colnames(data[10:19])
```

##### HI: shell height index family value

```
data%>%
  select(HI, all_of(list))%>%
  pivot_longer(-HI, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(HI, value, use = "pairwise.complete.obs"),
    p_value = cor.test(HI, value)$p.value
  )
```

No significant correlations. 

##### MYW: meat yield family value

```
data%>%
  select(MYW, all_of(list))%>%
  pivot_longer(-MYW, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(MYW, value, use = "pairwise.complete.obs"),
    p_value = cor.test(MYW, value)$p.value
  )
```

No significant correlations. 

###### POMS: lab survival after OsHV-1 challenge family value

```
data%>%
  select(POMS, all_of(list))%>%
  filter(!is.na(POMS))%>%
  pivot_longer(-POMS, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(POMS, value, use = "pairwise.complete.obs"),
    p_value = cor.test(POMS, value)$p.value
  )
```

No significant correlations. 

##### SUR: field survival family value

```
data%>%
  select(SUR, all_of(list))%>%
  pivot_longer(-SUR, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(SUR, value, use = "pairwise.complete.obs"),
    p_value = cor.test(SUR, value)$p.value
  )
```

No significant correlations. 

##### SUR_LIVE: survival, live individual value

```
data%>%
  select(SUR_LIVE, all_of(list))%>%
  pivot_longer(-SUR_LIVE, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(SUR_LIVE, value, use = "pairwise.complete.obs"),
    p_value = cor.test(SUR_LIVE, value)$p.value
  )
```

No significant correlations. 

##### WI: shell width family value

```
cor_results<-data%>%
  select(WI, all_of(list))%>%
  pivot_longer(-WI, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(WI, value, use = "pairwise.complete.obs"),
    p_value = cor.test(WI, value)$p.value
  );cor_results
```

No significant correlations. 

##### WTT: Wet weight family value

```
data%>%
  select(WTT, all_of(list))%>%
  pivot_longer(-WTT, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(WTT, value, use = "pairwise.complete.obs"),
    p_value = cor.test(WTT, value)$p.value
  )
```

No significant correlations. 

##### WTT_SEL=Wet weight individual value

```
data%>%
  select(WTT_SEL, all_of(list))%>%
  pivot_longer(-WTT_SEL, names_to = "variable", values_to = "value") %>%
  group_by(variable) %>%
  summarize(
    cor = cor(WTT_SEL, value, use = "pairwise.complete.obs"),
    p_value = cor.test(WTT_SEL, value)$p.value
  )
```

No significant correlations. 

### View all correlations together 

```
ebv_list<-c(colnames(data[2:9]))
```

```
# Select the relevant columns
df <- data %>%
  select(all_of(c(list, ebv_list)))

# Compute correlation matrix (for plotting color scale)
cor_mat <- cor(df, use = "pairwise.complete.obs")

# Compute p-values for each pair (list × ebv_list)
p_mat <- matrix(NA, nrow = length(list), ncol = length(ebv_list),
                dimnames = list(list, ebv_list))

for (i in list) {
  for (j in ebv_list) {
    test <- cor.test(df[[i]], df[[j]], use = "pairwise.complete.obs")
    p_mat[i, j] <- test$p.value
  }
}

# Extract only cross correlations
cross_cor <- cor_mat[list, ebv_list, drop = FALSE]
cross_p <- p_mat[list, ebv_list, drop = FALSE]

# Convert to tidy (long) format for ggplot
cross_cor_long <- as.data.frame(cross_cor) %>%
  rownames_to_column(var = "resazurin_var") %>%
  pivot_longer(-resazurin_var, names_to = "ebv_var", values_to = "correlation")

cross_p_long <- as.data.frame(cross_p) %>%
  rownames_to_column(var = "resazurin_var") %>%
  pivot_longer(-resazurin_var, names_to = "ebv_var", values_to = "p_value")

# Join correlation + p-value data frames
cross_long <- left_join(cross_cor_long, cross_p_long,
                        by = c("resazurin_var", "ebv_var"))

# Plot heatmap with p-values overlaid
plot1<-ggplot(cross_long, aes(x = ebv_var, y = resazurin_var, fill = correlation)) +
  geom_tile(color = "white") +
  geom_text(aes(label = paste0("p=", round(p_value, 2))), size = 3) +
  scale_fill_gradient2(
    low = "blue", mid = "white", high = "red",
    midpoint = 0, limits = c(-1, 1),
    name = "Pearson r"
  ) +
  theme_minimal(base_size = 14) +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    panel.grid = element_blank()
  ) +
  labs(
    x = "EBV variables",
    y = "Resazurin variables",
    title = "Cross-correlation heatmap (List × EBV, p-values labeled)"
  );plot1

ggsave(plot1, filename="taylor-shellfish-sept2025/figures/ebv-metrics-correlation-heatmap.png", width=10, height=6)
```

No significant correlations. 

