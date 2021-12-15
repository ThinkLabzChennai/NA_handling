---
title: "Handling missing values using R"
author:    
    -  "RSN_Thinklabz_Chennai"

output:
  html_document:
    keep_md: true
    toc: true
    toc_float: true
    
---




# Preamble
Understanding the problem and cleaning the dataset covers around 70% of our data analysis. In this notes we shall look at what are missing values and how can we handle them in a dataset before analyzing the data.

# Missing observations
   A data is collected from different sources by different persons. It is not always possible to get complete data where all the values of features for all observations are available. Therefore it is common to have some observations missing in the dataset.
   
# Dataset and pakages used
Let us use **`tidyverse`** for data wrangling and **`modeest`** for finding mode


```r
library(tidyverse)
library(modeest)
```


We shall use employee dataset which represents wage and other information of 11 persons



<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> Sl.No </th>
   <th style="text-align:left;"> Variable_name </th>
   <th style="text-align:left;"> Type </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Age </td>
   <td style="text-align:left;"> int </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> Education </td>
   <td style="text-align:left;"> chr </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> Experience.in.years </td>
   <td style="text-align:left;"> int </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> Wage </td>
   <td style="text-align:left;"> int </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> Balance </td>
   <td style="text-align:left;"> int </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> Jobclass </td>
   <td style="text-align:left;"> chr </td>
  </tr>
</tbody>
</table>

# Handling missing observations

  Once we have a dataset with missing values we can either remove the observation or we can remove feature if it has more number of missing values when compared to the total number of values. If we have very less number of missing observations, we can replace them with some value based on available values of dataset.
  
  In R missing values are stored as NA. There is a small difficulty that R only identifies missing values if the cell is blank in the dataset or it is mentioned as NA ( means 'Not Available') or NAn(Not a number). 
  
Let us look at our dataset in hand

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> Age </th>
   <th style="text-align:left;"> Education </th>
   <th style="text-align:right;"> Experience.in.years </th>
   <th style="text-align:left;"> Wage </th>
   <th style="text-align:right;"> Balance </th>
   <th style="text-align:left;"> Jobclass </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> 35000 </td>
   <td style="text-align:right;"> 245000 </td>
   <td style="text-align:left;"> Information </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:left;"> PG </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:left;"> 38000 </td>
   <td style="text-align:right;"> 322000 </td>
   <td style="text-align:left;"> Industry </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:left;"> UG </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 450000 </td>
   <td style="text-align:left;"> Not available </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 55 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:left;"> 30000 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> N/A </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:left;"> PG </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 595000 </td>
   <td style="text-align:left;"> N/A </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:left;"> PHD </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 12300 </td>
   <td style="text-align:left;"> na </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:left;"> UG </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:left;"> 40000 </td>
   <td style="text-align:right;"> 99000 </td>
   <td style="text-align:left;"> Information </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 49 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:left;"> 35500 </td>
   <td style="text-align:right;"> 150000 </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 65 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:left;"> 55800 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:left;"> UG </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> 38000 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> Industry </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:left;"> Not available </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>
Here the cells which are blank and cell having the following values 'NA', 'N/A', 'na', 'Not available' are all missing values. 

## Finding existence of missing value
**`is.na()`** is our function for identifying missing observation in a dataset, it returns a data frame or vector based on input with values TRUE for indicating missing observation or FALSE otherwise.

```r
is.na(da)
```

```
        Age Education Experience.in.years  Wage Balance Jobclass
 [1,] FALSE     FALSE               FALSE FALSE   FALSE    FALSE
 [2,] FALSE     FALSE               FALSE FALSE   FALSE    FALSE
 [3,] FALSE     FALSE               FALSE FALSE   FALSE    FALSE
 [4,] FALSE     FALSE               FALSE FALSE    TRUE    FALSE
 [5,] FALSE     FALSE                TRUE FALSE   FALSE    FALSE
 [6,] FALSE     FALSE               FALSE FALSE   FALSE    FALSE
 [7,] FALSE     FALSE               FALSE FALSE   FALSE    FALSE
 [8,] FALSE     FALSE               FALSE FALSE   FALSE    FALSE
 [9,] FALSE     FALSE               FALSE FALSE    TRUE    FALSE
[10,] FALSE     FALSE                TRUE FALSE    TRUE    FALSE
[11,] FALSE     FALSE               FALSE FALSE    TRUE    FALSE
```
Here we can see for some cells it has TRUE, whereas in original dataset we have more missing observations which are represented in various forms. To overcome this, we can use `dplyr` to mutate the missing values which are represented in different form to **NA** using **`replace()`** function. 

## Conversion for missing observation 
   
### Empty string in Character/Factor type
   Empty cell in a categorical/ Factor or character type variable is considered as a separate value which is taken as empty string. We need to change this to NA so that R can identify it as missing observation. We have two variables Education and Jobclass which has different style of representation of missing value. Let us change those
   

```r
da=da %>% mutate(Education=replace(Education,Education=="",NA),
                 Jobclass=replace(Jobclass,Jobclass%in%c("Not available","N/A","na",""),NA),
                 )
```



### Missing value represented as string in a numeric variable

When a missing observation is represented using a string in a numeric variable, the whole variable will be considered as character in R. For this variable we replace missing observation first and then convert the variable to numeric


```r
da=da %>% mutate(Wage=replace(Wage,Wage%in%c("","Not available"),NA),
                 Wage=as.numeric(Wage))
```

Let us now look at our modified dataset and also corresponding missing value indication
<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> Age </th>
   <th style="text-align:left;"> Education </th>
   <th style="text-align:right;"> Experience.in.years </th>
   <th style="text-align:right;"> Wage </th>
   <th style="text-align:right;"> Balance </th>
   <th style="text-align:left;"> Jobclass </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 35000 </td>
   <td style="text-align:right;"> 245000 </td>
   <td style="text-align:left;"> Information </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:left;"> PG </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 38000 </td>
   <td style="text-align:right;"> 322000 </td>
   <td style="text-align:left;"> Industry </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:left;"> UG </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 450000 </td>
   <td style="text-align:left;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 55 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 30000 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:left;"> PG </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 595000 </td>
   <td style="text-align:left;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:left;"> PHD </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 12300 </td>
   <td style="text-align:left;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:left;"> UG </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 40000 </td>
   <td style="text-align:right;"> 99000 </td>
   <td style="text-align:left;"> Information </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 49 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 35500 </td>
   <td style="text-align:right;"> 150000 </td>
   <td style="text-align:left;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 65 </td>
   <td style="text-align:left;"> HS </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 55800 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:left;"> UG </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 38000 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> Industry </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:left;"> NA </td>
  </tr>
</tbody>
</table>

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Age </th>
   <th style="text-align:left;"> Education </th>
   <th style="text-align:left;"> Experience.in.years </th>
   <th style="text-align:left;"> Wage </th>
   <th style="text-align:left;"> Balance </th>
   <th style="text-align:left;"> Jobclass </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> TRUE </td>
  </tr>
</tbody>
</table>

## Counting missing observations
Once we have prepared data in a form where missing values are clearly indicated as NA we can count

   1. Total number of missing values

```r
sum(is.na(da))
```

```
[1] 18
```
   
   1. Number of missing values in each column

```r
colSums(is.na(da))
```

```
                Age           Education Experience.in.years                Wage 
                  0                   1                   2                   4 
            Balance            Jobclass 
                  4                   7 
```

   
   1.  Number of missing values in each row

```r
rowSums(is.na(da))
```

```
 [1] 0 0 2 2 3 2 0 1 2 2 4
```

We can compare this number of missing values with total number of rows and columns and if the proportion missing values is high with total number of values we can exclude the specific row or column from the dataset.


```r
dim(da)
```

```
[1] 11  6
```
Since the last row has 4 missing values out of 6, we can remove the single row and store it as another data


```r
da1=da[,-6]
```

## Finding numerical summary if missing values are present
While attempting to find Numerical summaries like mean,median,variance etc. for variables with NA we will get error, to overcome this we use argument **`na.rm=T`**

```r
mean(da$Experience.in.years,na.rm=T)
```

```
[1] 20.77778
```

```r
median(da$Balance,na.rm=T)
```

```
[1] 245000
```

```r
var(da$Wage,na.rm=T)
```

```
[1] 65736667
```



## Removing rows which have missing values
We can remove all the rows in a dataset which has missing values using **`na.omit()`**.we shall remove all rows with missing values and store it as separate data.


```r
da2=na.omit(da)
dim(da2)
```

```
[1] 3 6
```

We can see that this dataset is much smaller when compared to original data

## Replacing NA with values
If we remove rows or columns having missing values we lose many information from the dataset, to avoid this, we can replace missing observation with some value, let us see some simple techniques for replacement

### Replacing NA with values central tendecy for numeric data
For numerical data we can replace missing observation with **mean** value if all values are closer or **median** if some values are extreme. For this we can use either **`base::replace()``** or **`tidyr::replace_na()``**.



```r
da=da %>% mutate(Experience.in.years=
                   replace(Experience.in.years,
                           is.na(Experience.in.years),
                           mean(Experience.in.years,na.rm=T)
                           )
                 )
```


### Replacing NA with most frequent values for Categorical data

For categorical data we can replace missing observation with **mode** value

```r
da=da %>% mutate(Jobclass=replace_na(Jobclass,mfv1(Jobclass,na_rm=T))) 
```

# Final note
We have discussed what are missing values, converting various representations to Standard NA. Identifying missing observations. Removing missing rows or columns and finally how to replace them with suitable values for analysis. More methods can be used for replacing and those can be learned once we master basic ones.

<div class="tocify-extend-page" data-unique="tocify-extend-page" style="height: 0;"></div>
