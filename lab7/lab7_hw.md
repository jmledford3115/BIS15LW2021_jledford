---
title: "Lab 7 Homework"
author: "Please Add Your Name Here"
date: "2021-01-31"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

## Data
**1. For this homework, we will use two different data sets. Please load `amniota` and `amphibio`.**  

`amniota` data:  
Myhrvold N, Baldridge E, Chan B, Sivam D, Freeman DL, Ernest SKM (2015). “An amniote life-history
database to perform comparative analyses with birds, mammals, and reptiles.” _Ecology_, *96*, 3109.
doi: 10.1890/15-0846.1 (URL: https://doi.org/10.1890/15-0846.1).

```r
amniota <- readr::read_csv("data/amniota.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   common_name = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

`amphibio` data:  
Oliveira BF, São-Pedro VA, Santos-Barrera G, Penone C, Costa GC (2017). “AmphiBIO, a global database
for amphibian ecological traits.” _Scientific Data_, *4*, 170123. doi: 10.1038/sdata.2017.123 (URL:
https://doi.org/10.1038/sdata.2017.123).

```r
amphibio <- readr::read_csv("data/amphibio.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   id = col_character(),
##   Order = col_character(),
##   Family = col_character(),
##   Genus = col_character(),
##   Species = col_character(),
##   Seeds = col_logical(),
##   OBS = col_logical()
## )
## ℹ Use `spec()` for the full column specifications.
```

```
## Warning: 125 parsing failures.
##  row col           expected                                                           actual                file
## 1410 OBS 1/0/T/F/TRUE/FALSE Identified as P. appendiculata in Boquimpani-Freitas et al. 2002 'data/amphibio.csv'
## 1416 OBS 1/0/T/F/TRUE/FALSE Identified as T. miliaris in Giaretta and Facure 2004            'data/amphibio.csv'
## 1447 OBS 1/0/T/F/TRUE/FALSE Considered endangered by Soto-Azat et al. 2013                   'data/amphibio.csv'
## 1448 OBS 1/0/T/F/TRUE/FALSE Considered extinct by Soto-Azat et al. 2013                      'data/amphibio.csv'
## 1471 OBS 1/0/T/F/TRUE/FALSE nomem dubitum                                                    'data/amphibio.csv'
## .... ... .................. ................................................................ ...................
## See problems(...) for more details.
```

```r
amphibio <- janitor::clean_names(amphibio)
```

## Questions  
**2. Do some exploratory analysis of the `amniota` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

_In the amniota data set NA's are represented by -999._ 


```r
glimpse(amniota)
```

```
## Rows: 21,322
## Columns: 36
## $ class                                 <chr> "Aves", "Aves", "Aves", "Aves",…
## $ order                                 <chr> "Accipitriformes", "Accipitrifo…
## $ family                                <chr> "Accipitridae", "Accipitridae",…
## $ genus                                 <chr> "Accipiter", "Accipiter", "Acci…
## $ species                               <chr> "albogularis", "badius", "bicol…
## $ subspecies                            <dbl> -999, -999, -999, -999, -999, -…
## $ common_name                           <chr> "Pied Goshawk", "Shikra", "Bico…
## $ female_maturity_d                     <dbl> -999.000, 363.468, -999.000, -9…
## $ litter_or_clutch_size_n               <dbl> -999.000, 3.250, 2.700, -999.00…
## $ litters_or_clutches_per_y             <dbl> -999, 1, -999, -999, 1, -999, -…
## $ adult_body_mass_g                     <dbl> 251.500, 140.000, 345.000, 142.…
## $ maximum_longevity_y                   <dbl> -999.00000, -999.00000, -999.00…
## $ gestation_d                           <dbl> -999, -999, -999, -999, -999, -…
## $ weaning_d                             <dbl> -999, -999, -999, -999, -999, -…
## $ birth_or_hatching_weight_g            <dbl> -999, -999, -999, -999, -999, -…
## $ weaning_weight_g                      <dbl> -999, -999, -999, -999, -999, -…
## $ egg_mass_g                            <dbl> -999.00, 21.00, 32.00, -999.00,…
## $ incubation_d                          <dbl> -999.00, 30.00, -999.00, -999.0…
## $ fledging_age_d                        <dbl> -999.00, 32.00, -999.00, -999.0…
## $ longevity_y                           <dbl> -999.00000, -999.00000, -999.00…
## $ male_maturity_d                       <dbl> -999, -999, -999, -999, -999, -…
## $ inter_litter_or_interbirth_interval_y <dbl> -999, -999, -999, -999, -999, -…
## $ female_body_mass_g                    <dbl> 352.500, 168.500, 390.000, -999…
## $ male_body_mass_g                      <dbl> 223.000, 125.000, 212.000, 142.…
## $ no_sex_body_mass_g                    <dbl> -999.0, 123.0, -999.0, -999.0, …
## $ egg_width_mm                          <dbl> -999, -999, -999, -999, -999, -…
## $ egg_length_mm                         <dbl> -999, -999, -999, -999, -999, -…
## $ fledging_mass_g                       <dbl> -999, -999, -999, -999, -999, -…
## $ adult_svl_cm                          <dbl> -999.00, 30.00, 39.50, -999.00,…
## $ male_svl_cm                           <dbl> -999, -999, -999, -999, -999, -…
## $ female_svl_cm                         <dbl> -999, -999, -999, -999, -999, -…
## $ birth_or_hatching_svl_cm              <dbl> -999, -999, -999, -999, -999, -…
## $ female_svl_at_maturity_cm             <dbl> -999, -999, -999, -999, -999, -…
## $ female_body_mass_at_maturity_g        <dbl> -999, -999, -999, -999, -999, -…
## $ no_sex_svl_cm                         <dbl> -999, -999, -999, -999, -999, -…
## $ no_sex_maturity_d                     <dbl> -999, -999, -999, -999, -999, -…
```

**3. Do some exploratory analysis of the `amphibio` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

_In the `amphibio` data, NA's are represented by "NA"._

```r
glimpse(amphibio)
```

```
## Rows: 6,776
## Columns: 38
## $ id                      <chr> "Anf0001", "Anf0002", "Anf0003", "Anf0004", "…
## $ order                   <chr> "Anura", "Anura", "Anura", "Anura", "Anura", …
## $ family                  <chr> "Allophrynidae", "Alytidae", "Alytidae", "Aly…
## $ genus                   <chr> "Allophryne", "Alytes", "Alytes", "Alytes", "…
## $ species                 <chr> "Allophryne ruthveni", "Alytes cisternasii", …
## $ fos                     <dbl> NA, NA, NA, NA, NA, 1, 1, 1, 1, 1, 1, 1, 1, N…
## $ ter                     <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ aqu                     <dbl> 1, 1, 1, 1, NA, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,…
## $ arb                     <dbl> 1, 1, 1, 1, 1, 1, NA, NA, NA, NA, NA, NA, NA,…
## $ leaves                  <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ flowers                 <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ seeds                   <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ fruits                  <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ arthro                  <dbl> 1, 1, 1, NA, 1, 1, 1, 1, 1, NA, 1, 1, NA, NA,…
## $ vert                    <dbl> NA, NA, NA, NA, NA, NA, 1, NA, NA, NA, 1, 1, …
## $ diu                     <dbl> 1, NA, NA, NA, NA, NA, 1, 1, 1, NA, 1, 1, NA,…
## $ noc                     <dbl> 1, 1, 1, NA, 1, 1, 1, 1, 1, NA, 1, 1, 1, NA, …
## $ crepu                   <dbl> 1, NA, NA, NA, NA, 1, NA, NA, NA, NA, NA, NA,…
## $ wet_warm                <dbl> NA, NA, NA, NA, 1, 1, NA, NA, NA, NA, 1, NA, …
## $ wet_cold                <dbl> 1, NA, NA, NA, NA, NA, 1, NA, NA, NA, NA, NA,…
## $ dry_warm                <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ dry_cold                <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ body_mass_g             <dbl> 31.00, 6.10, NA, NA, 2.31, 13.40, 21.80, NA, …
## $ age_at_maturity_min_y   <dbl> NA, 2.0, 2.0, NA, 3.0, 2.0, 3.0, NA, NA, NA, …
## $ age_at_maturity_max_y   <dbl> NA, 2.0, 2.0, NA, 3.0, 3.0, 5.0, NA, NA, NA, …
## $ body_size_mm            <dbl> 31.0, 50.0, 55.0, NA, 40.0, 55.0, 80.0, 60.0,…
## $ size_at_maturity_min_mm <dbl> NA, 27, NA, NA, NA, 35, NA, NA, NA, NA, NA, N…
## $ size_at_maturity_max_mm <dbl> NA, 36.0, NA, NA, NA, 40.5, NA, NA, NA, NA, N…
## $ longevity_max_y         <dbl> NA, 6, NA, NA, NA, 7, 9, NA, NA, NA, NA, NA, …
## $ litter_size_min_n       <dbl> 300, 60, 40, NA, 7, 53, 300, 1500, 1000, NA, …
## $ litter_size_max_n       <dbl> 300, 180, 40, NA, 20, 171, 1500, 1500, 1000, …
## $ reproductive_output_y   <dbl> 1, 4, 1, 4, 1, 4, 6, 1, 1, 1, 1, 1, 1, 1, NA,…
## $ offspring_size_min_mm   <dbl> NA, 2.6, NA, NA, 5.4, 2.6, 1.5, NA, 1.5, NA, …
## $ offspring_size_max_mm   <dbl> NA, 3.5, NA, NA, 7.0, 5.0, 2.0, NA, 1.5, NA, …
## $ dir                     <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
## $ lar                     <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ viv                     <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
## $ obs                     <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
```

**4. How many total NA's are in each data set? Do these values make sense? Are NA's represented by values?**   

`amniota`  
_For such a large data set, it is unlikely that there are no NA's. It appears that NA's are represented by "-999"._

```r
amniota %>% summarize(number_nas = sum(is.na(amniota)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```

`amphibio`  
_In this data set, NA's are represented by NA and not values._

```r
amphibio %>% summarize(number_nas = sum(is.na(amphibio)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1     170691
```

**5. Make any necessary replacements in the data such that all NA's appear as "NA".**   

```r
amniota_tidy <- amniota %>% 
  na_if("-999") %>% 
  na_if("-30258.711")
```


```r
amniota_tidy %>% 
  summarize(number_nas = sum(is.na(amniota_tidy))) #how many NAs
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1     528200
```

**6. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amniota` data.**  

```r
naniar::miss_var_summary(amniota_tidy) #how many NAs with percentages
```

```
## # A tibble: 36 x 3
##    variable                       n_miss pct_miss
##    <chr>                           <int>    <dbl>
##  1 subspecies                      21322    100  
##  2 female_body_mass_at_maturity_g  21318    100. 
##  3 female_svl_at_maturity_cm       21120     99.1
##  4 fledging_mass_g                 21111     99.0
##  5 male_svl_cm                     21040     98.7
##  6 no_sex_maturity_d               20860     97.8
##  7 egg_width_mm                    20727     97.2
##  8 egg_length_mm                   20702     97.1
##  9 weaning_weight_g                20258     95.0
## 10 female_svl_cm                   20242     94.9
## # … with 26 more rows
```

**7. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amphibio` data.**

```r
naniar::miss_var_summary(amphibio)
```

```
## # A tibble: 38 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 obs        6776    100  
##  2 fruits     6774    100. 
##  3 flowers    6772     99.9
##  4 seeds      6772     99.9
##  5 leaves     6752     99.6
##  6 dry_cold   6735     99.4
##  7 vert       6657     98.2
##  8 wet_cold   6625     97.8
##  9 crepu      6608     97.5
## 10 dry_warm   6572     97.0
## # … with 28 more rows
```

**8. For the `amniota` data, calculate the number of NAs in the `egg_mass_g` column sorted by taxonomic class; i.e. how many NA's are present in the `egg_mass_g` column in birds, mammals, and reptiles? Does this results make sense biologically? How do these results affect your interpretation of NA's?**  

_NAs could reflect missing data (no observations were recorded) or it could reflect data that are truly not applicable._  


```r
amniota_tidy %>%
  select(class, egg_mass_g) %>% 
  group_by(class) %>%
  naniar::miss_var_summary(order=T)
```

```
## # A tibble: 3 x 4
## # Groups:   class [3]
##   class    variable   n_miss pct_miss
##   <chr>    <chr>       <int>    <dbl>
## 1 Aves     egg_mass_g   4914     50.1
## 2 Mammalia egg_mass_g   4953    100  
## 3 Reptilia egg_mass_g   6040     92.0
```

**9. The `amphibio` data have variables that classify species as fossorial (burrowing), terrestrial, aquatic, or arboreal.Calculate the number of NA's in each of these variables. Do you think that the authors intend us to think that there are NA's in these columns or could they represent something else? Explain.**

```r
amphibio %>% 
  select(fos, ter, arb, aqu) %>% 
  summarise_all(~(sum(is.na(.))))
```

```
## # A tibble: 1 x 4
##     fos   ter   arb   aqu
##   <int> <int> <int> <int>
## 1  6053  1104  4347  2810
```

**10. Now that we know how NA's are represented in the `amniota` data, how would you load the data such that the values which represent NA's are automatically converted?**

```r
amniota_pre <- 
  readr::read_csv(file = "data/amniota.csv", na = c("-999", "-30258.711"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   subspecies = col_logical(),
##   common_name = col_character(),
##   gestation_d = col_logical(),
##   weaning_d = col_logical(),
##   weaning_weight_g = col_logical(),
##   male_svl_cm = col_logical(),
##   female_svl_cm = col_logical(),
##   birth_or_hatching_svl_cm = col_logical(),
##   female_svl_at_maturity_cm = col_logical(),
##   female_body_mass_at_maturity_g = col_logical(),
##   no_sex_svl_cm = col_logical()
## )
## ℹ Use `spec()` for the full column specifications.
```

```
## Warning: 13577 parsing failures.
##  row                      col           expected actual               file
## 9803 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9804 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9805 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9806 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9807 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## .... ........................ .................. ...... ..................
## See problems(...) for more details.
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
