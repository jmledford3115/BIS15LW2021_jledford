---
title: "Lab 3 Homework"
author: "Joel Ledford"
date: "2021-01-19"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep  
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
dim(msleep)
```

```
## [1] 83 11
```
</div>  

2. Store these data into a new data frame `sleep`.  

```r
sleep <- data.frame(msleep)
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
glimpse(sleep)
```

```
## Rows: 83
## Columns: 11
## $ name         <chr> "Cheetah", "Owl monkey", "Mountain beaver", "Greater sho…
## $ genus        <chr> "Acinonyx", "Aotus", "Aplodontia", "Blarina", "Bos", "Br…
## $ vore         <chr> "carni", "omni", "herbi", "omni", "herbi", "herbi", "car…
## $ order        <chr> "Carnivora", "Primates", "Rodentia", "Soricomorpha", "Ar…
## $ conservation <chr> "lc", NA, "nt", "lc", "domesticated", NA, "vu", NA, "dom…
## $ sleep_total  <dbl> 12.1, 17.0, 14.4, 14.9, 4.0, 14.4, 8.7, 7.0, 10.1, 3.0, …
## $ sleep_rem    <dbl> NA, 1.8, 2.4, 2.3, 0.7, 2.2, 1.4, NA, 2.9, NA, 0.6, 0.8,…
## $ sleep_cycle  <dbl> NA, NA, NA, 0.1333333, 0.6666667, 0.7666667, 0.3833333, …
## $ awake        <dbl> 11.9, 7.0, 9.6, 9.1, 20.0, 9.6, 15.3, 17.0, 13.9, 21.0, …
## $ brainwt      <dbl> NA, 0.01550, NA, 0.00029, 0.42300, NA, NA, NA, 0.07000, …
## $ bodywt       <dbl> 50.000, 0.480, 1.350, 0.019, 600.000, 3.850, 20.490, 0.0…
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
anyNA(sleep)
```

```
## [1] TRUE
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small <- subset(sleep, bodywt<=1)
large <- subset(sleep, bodywt>=200)
```

8. What is the mean weight for both the small and large mammals?

```r
mean(small$bodywt, na.rm = T)
```

```
## [1] 0.2596667
```


```r
mean(large$bodywt, na.rm = T)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

_Small animals have a sleep_total average of 12.65_

```r
mean(large$sleep_total, na.rm = T)
```

```
## [1] 3.3
```


```r
mean(small$sleep_total, na.rm = T)
```

```
## [1] 12.65833
```

10. Which animal is the sleepiest among the entire dataframe?

```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```


```r
subset(sleep, sleep_total==19.9)
```

```
##                name  genus    vore      order conservation sleep_total
## 43 Little brown bat Myotis insecti Chiroptera         <NA>        19.9
##    sleep_rem sleep_cycle awake brainwt bodywt
## 43         2         0.2   4.1 0.00025   0.01
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
