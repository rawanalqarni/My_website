---
title: Palmer Penguins EDA
author: Rawan ALQarni
date: '2022-01-27'
slug: palmer-penguins-eda
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2022-01-27T12:57:08Z'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

<img src="dataset-thumbnail.png" width="80%" style="display: block; margin: auto;" />

### Palmer Penguins Dataset EDA 
- This notebook include: 
  - Introduction
  - EDA 
  - Data Visualization 
  - Conclusion





### Introduction

#### Palmer Penguins Dataset

Palmer penguin data were collected and made available by Dr. Kristen Gorman and the Palmer Station, Antarctica LTER, a member of the Long Term Ecological Research Network.

The dataset veriables :
- species: penguin species (Chinstrap, Adélie, or Gentoo)
- culmen_length_mm: culmen length (mm)
- culmen_depth_mm: culmen depth (mm)
- flipper_length_mm: flipper length (mm)
- body_mass_g: body mass (g)
- island: island name (Dream, Torgersen, or Biscoe) in the Palmer Archipelago (Antarctica)
- sex: penguin sex

Sourse of the dataset:
- built-in dataset in r


Objectives: 
- Ehowcase my technical skill 
- Explore Palmer Penguins dataset (EDA phase)
- Practice my EDA and Visualization skills in python 


### Exploratory Data Analysis

1- import the library and the palmer penguins dataset 


```r
install.packages("palmerpenguins")
```

```
## Installing package into '/usr/local/lib/R/site-library'
## (as 'lib' is unspecified)
```





2- Create a tibble for the penguins dataset 

```r
df_penguins <- as_tibble(penguins)
```

3- Explore the dataset

```r
head(df_penguins)
```

```
## # A tibble: 6 × 8
##   species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
##   <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
## 1 Adelie  Torge…           39.1          18.7              181        3750 male 
## 2 Adelie  Torge…           39.5          17.4              186        3800 fema…
## 3 Adelie  Torge…           40.3          18                195        3250 fema…
## 4 Adelie  Torge…           NA            NA                 NA          NA <NA> 
## 5 Adelie  Torge…           36.7          19.3              193        3450 fema…
## 6 Adelie  Torge…           39.3          20.6              190        3650 male 
## # … with 1 more variable: year <int>
```



```r
# How many rows and colums and what are there datatypes?
glimpse(df_penguins)
```

```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
## $ sex               <fct> male, female, female, NA, female, male, female, male…
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
```

### Data Visualization


```r
ggplot(df_penguins, aes(species, fill=species)) + geom_bar()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

