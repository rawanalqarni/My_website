---
title: Example car dataset
author: Rawan ALQarni
date: '2022-01-27'
slug: example-car-dataset
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2022-01-27T21:32:50Z'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


~~Rawan~~ 




```r
library(dplyr)
## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
library(ggplot2)

summary(cars)
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
fit <- lm(dist ~ speed, data = cars)
fit
## 
## Call:
## lm(formula = dist ~ speed, data = cars)
## 
## Coefficients:
## (Intercept)        speed  
##     -17.579        3.932
```

```r

sp2 <- ggplot(cars,aes(dist, speed, color=speed))+ geom_point()
sp2+scale_color_gradientn(colours = rainbow(5))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />



