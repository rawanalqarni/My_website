---
title: Diamonds EDA
author: Rawan ALQarni
date: '2022-01-27'
slug: diamonds-eda
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2022-01-27T13:35:11Z'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

## Diamonds EDA




```r
data(diamonds)
table1 <- as_tibble(diamonds)
```

## 9.2: How many diamonds with a clarity of category “IF” are present in the data-set?


```r
sum_of_IF <- sum(diamonds$clarity == "IF")
sum_of_IF
```

```
## [1] 1790
```


## 9.3: What fraction of the total do they represent?


```r
total <- count(diamonds)
fraction_of_clarity <- (sum_of_IF / total)
```



## 9.5: What is the cheapest diamond price overall?

```r
cheapest_diamond <- min(diamonds$price)
```

## 9.6: What is the range of diamond prices?

```r
range(diamonds$price) 
```

```
## [1]   326 18823
```

## 9.7:  What is the average diamond price in each category of cut and color?
- for color

```r
diamonds %>% 
  group_by(color = color) %>%
  summarise(Average_Price = mean(price))
```

```
## # A tibble: 7 × 2
##   color Average_Price
##   <ord>         <dbl>
## 1 D             3170.
## 2 E             3077.
## 3 F             3725.
## 4 G             3999.
## 5 H             4487.
## 6 I             5092.
## 7 J             5324.
```
- for cut 

```r
diamonds %>% 
  group_by(cut = cut) %>%
  summarise(Average_Price = mean(price))
```

```
## # A tibble: 5 × 2
##   cut       Average_Price
##   <ord>             <dbl>
## 1 Fair              4359.
## 2 Good              3929.
## 3 Very Good         3982.
## 4 Premium           4584.
## 5 Ideal             3458.
```


### Data Vis 

```r
library(ggplot2)
ggplot(diamonds, aes(cut))+geom_bar()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />



