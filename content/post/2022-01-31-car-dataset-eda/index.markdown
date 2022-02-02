---
title: Car Dataset EDA
author: Rawan ALQarni
date: '2022-01-31'
slug: car-dataset-eda
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2022-01-31T10:22:54Z'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


1- import dataset 

```r
summary(cars)
```

```
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```


##روان 

2- data vis 

```r
library(ggplot2)
vis <- ggplot(cars , aes(speed,dist, color=speed))+geom_point()
vis+scale_color_gradientn(colors=rainbow(5))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />

