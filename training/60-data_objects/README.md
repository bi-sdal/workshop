# README.md
Daniel Chen  
  



# Working with dataframes


```r
# load the ggplot2 library to get the diamonds dataset
library(ggplot2)

# look at the diamonds dataset
diamonds
```

```
## # A tibble: 53,940 × 10
##    carat       cut color clarity depth table price     x     y     z
##    <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1   0.23     Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
## 2   0.21   Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
## 3   0.23      Good     E     VS1  56.9    65   327  4.05  4.07  2.31
## 4   0.29   Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
## 5   0.31      Good     J     SI2  63.3    58   335  4.34  4.35  2.75
## 6   0.24 Very Good     J    VVS2  62.8    57   336  3.94  3.96  2.48
## 7   0.24 Very Good     I    VVS1  62.3    57   336  3.95  3.98  2.47
## 8   0.26 Very Good     H     SI1  61.9    55   337  4.07  4.11  2.53
## 9   0.22      Fair     E     VS2  65.1    61   337  3.87  3.78  2.49
## 10  0.23 Very Good     H     VS1  59.4    61   338  4.00  4.05  2.39
## # ... with 53,930 more rows
```


You can see here that it prints out a portion of the dataset,
this is because this is an `tibble` object, which is an extension of the core
`data.frame` object in R.

If you work with any package that Hadley Whickham has written,
be prepared to see tibbles.

They (for the most part) will work exactly the same as `data.frame` objects


## Subset columns

```r
# get 1 column from the dataframe
diamonds$carat
```

I'm not showing the output for the code becuse there are too many values to print to the screen
The results of using the `$` notation for subsetting will be a `vector` of values


```r
# get 1 column using bracket notation
# bracket noation [row, columns]
diamonds[, 'carat']
```

```
## # A tibble: 53,940 × 1
##    carat
##    <dbl>
## 1   0.23
## 2   0.21
## 3   0.23
## 4   0.29
## 5   0.31
## 6   0.24
## 7   0.24
## 8   0.26
## 9   0.22
## 10  0.23
## # ... with 53,930 more rows
```

You can visually see the difference between the `[ ]` and `$` notation of subsetting.
`[ ]` will return a dataframe (or in this case, a tibble) back


```r
# get multiple columns from the dataset
diamonds[, c('carat', 'price')]
```

```
## # A tibble: 53,940 × 2
##    carat price
##    <dbl> <int>
## 1   0.23   326
## 2   0.21   326
## 3   0.23   327
## 4   0.29   334
## 5   0.31   335
## 6   0.24   336
## 7   0.24   336
## 8   0.26   337
## 9   0.22   337
## 10  0.23   338
## # ... with 53,930 more rows
```

## Subset rows

```r
# various ways of getting rows from data

diamonds[1, ]
```

```
## # A tibble: 1 × 10
##   carat   cut color clarity depth table price     x     y     z
##   <dbl> <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23 Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
```

```r
diamonds[10, ]
```

```
## # A tibble: 1 × 10
##   carat       cut color clarity depth table price     x     y     z
##   <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23 Very Good     H     VS1  59.4    61   338     4  4.05  2.39
```

```r
diamonds[c(1, 2), ]
```

```
## # A tibble: 2 × 10
##   carat     cut color clarity depth table price     x     y     z
##   <dbl>   <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23   Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
## 2  0.21 Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
```

```r
diamonds[c(11:20), ]
```

```
## # A tibble: 10 × 10
##    carat       cut color clarity depth table price     x     y     z
##    <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1   0.30      Good     J     SI1  64.0    55   339  4.25  4.28  2.73
## 2   0.23     Ideal     J     VS1  62.8    56   340  3.93  3.90  2.46
## 3   0.22   Premium     F     SI1  60.4    61   342  3.88  3.84  2.33
## 4   0.31     Ideal     J     SI2  62.2    54   344  4.35  4.37  2.71
## 5   0.20   Premium     E     SI2  60.2    62   345  3.79  3.75  2.27
## 6   0.32   Premium     E      I1  60.9    58   345  4.38  4.42  2.68
## 7   0.30     Ideal     I     SI2  62.0    54   348  4.31  4.34  2.68
## 8   0.30      Good     J     SI1  63.4    54   351  4.23  4.29  2.70
## 9   0.30      Good     J     SI1  63.8    56   351  4.23  4.26  2.71
## 10  0.30 Very Good     J     SI1  62.7    59   351  4.21  4.27  2.66
```

```r
diamonds[11:20, ]
```

```
## # A tibble: 10 × 10
##    carat       cut color clarity depth table price     x     y     z
##    <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1   0.30      Good     J     SI1  64.0    55   339  4.25  4.28  2.73
## 2   0.23     Ideal     J     VS1  62.8    56   340  3.93  3.90  2.46
## 3   0.22   Premium     F     SI1  60.4    61   342  3.88  3.84  2.33
## 4   0.31     Ideal     J     SI2  62.2    54   344  4.35  4.37  2.71
## 5   0.20   Premium     E     SI2  60.2    62   345  3.79  3.75  2.27
## 6   0.32   Premium     E      I1  60.9    58   345  4.38  4.42  2.68
## 7   0.30     Ideal     I     SI2  62.0    54   348  4.31  4.34  2.68
## 8   0.30      Good     J     SI1  63.4    54   351  4.23  4.29  2.70
## 9   0.30      Good     J     SI1  63.8    56   351  4.23  4.26  2.71
## 10  0.30 Very Good     J     SI1  62.7    59   351  4.21  4.27  2.66
```

## Subset rows and columns


```r
# getting rows and columns from your data
diamonds[11:20, c('cut', 'color')]
```

```
## # A tibble: 10 × 2
##          cut color
##        <ord> <ord>
## 1       Good     J
## 2      Ideal     J
## 3    Premium     F
## 4      Ideal     J
## 5    Premium     E
## 6    Premium     E
## 7      Ideal     I
## 8       Good     J
## 9       Good     J
## 10 Very Good     J
```

## Getting a glimpse of your data

Tibbles are nice in that they don't print all the rows out to the screen by default.
If you end up working with other dataframe-like objects, they might print out more rows of data you will care to look at


```r
# get the first 6 rows of the dataset
head(diamonds)
```

```
## # A tibble: 6 × 10
##   carat       cut color clarity depth table price     x     y     z
##   <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23     Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
## 2  0.21   Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
## 3  0.23      Good     E     VS1  56.9    65   327  4.05  4.07  2.31
## 4  0.29   Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
## 5  0.31      Good     J     SI2  63.3    58   335  4.34  4.35  2.75
## 6  0.24 Very Good     J    VVS2  62.8    57   336  3.94  3.96  2.48
```

```r
# get the last 6 rows of the dataset
tail(diamonds)
```

```
## # A tibble: 6 × 10
##   carat       cut color clarity depth table price     x     y     z
##   <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.72   Premium     D     SI1  62.7    59  2757  5.69  5.73  3.58
## 2  0.72     Ideal     D     SI1  60.8    57  2757  5.75  5.76  3.50
## 3  0.72      Good     D     SI1  63.1    55  2757  5.69  5.75  3.61
## 4  0.70 Very Good     D     SI1  62.8    60  2757  5.66  5.68  3.56
## 5  0.86   Premium     H     SI2  61.0    58  2757  6.15  6.12  3.74
## 6  0.75     Ideal     D     SI2  62.2    55  2757  5.83  5.87  3.64
```


```r
# get numner of rows and columns
dim(diamonds)
```

```
## [1] 53940    10
```

```r
nrow(diamonds)
```

```
## [1] 53940
```

```r
ncol(diamonds)
```

```
## [1] 10
```

```r
# get the number of columns from dim
dim(diamonds)[2]
```

```
## [1] 10
```

# cars dataset

```r
# look at the built in r dataset, cars
head(cars)
```

```
##   speed dist
## 1     4    2
## 2     4   10
## 3     7    4
## 4     7   22
## 5     8   16
## 6     9   10
```


```r
# use the class function to see what you have
class(diamonds)
```

```
## [1] "tbl_df"     "tbl"        "data.frame"
```

```r
class(cars)
```

```
## [1] "data.frame"
```

# mtcars datset


```r
# the mtcars datset
head(mtcars)
```

```
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```


```r
# get a specific value out of the data
mtcars[1, 'wt']
```

```
## [1] 2.62
```

## getting just the columns names and row names

```r
names(mtcars)
```

```
##  [1] "mpg"  "cyl"  "disp" "hp"   "drat" "wt"   "qsec" "vs"   "am"   "gear"
## [11] "carb"
```

```r
row.names(mtcars)
```

```
##  [1] "Mazda RX4"           "Mazda RX4 Wag"       "Datsun 710"         
##  [4] "Hornet 4 Drive"      "Hornet Sportabout"   "Valiant"            
##  [7] "Duster 360"          "Merc 240D"           "Merc 230"           
## [10] "Merc 280"            "Merc 280C"           "Merc 450SE"         
## [13] "Merc 450SL"          "Merc 450SLC"         "Cadillac Fleetwood" 
## [16] "Lincoln Continental" "Chrysler Imperial"   "Fiat 128"           
## [19] "Honda Civic"         "Toyota Corolla"      "Toyota Corona"      
## [22] "Dodge Challenger"    "AMC Javelin"         "Camaro Z28"         
## [25] "Pontiac Firebird"    "Fiat X1-9"           "Porsche 914-2"      
## [28] "Lotus Europa"        "Ford Pantera L"      "Ferrari Dino"       
## [31] "Maserati Bora"       "Volvo 142E"
```


```r
# specifying row and columns by name
mtcars["Mazda RX4", 'wt']
```

```
## [1] 2.62
```

## Creating new columns

```r
# create new column called cars
mtcars$cars <- row.names(mtcars)

mtcars$cyl_wt <- mtcars$cyl / mtcars$wt
```

## Getting summary statistics on your data
summary(mtcars)

## Using `attach`

The reason why you would *not* want to use `attach` in your code is becuase it mucks up the variables
you have access to in your R code.

It can be very easy to forget which what the column names are in your dataset.


```r
# dont do this
attach(mtcars)
```

```
## The following object is masked from package:ggplot2:
## 
##     mpg
```

```
## The following object is masked from package:datasets:
## 
##     cars
```

```r
mpg
```

```
##  [1] 21.0 21.0 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 17.8 16.4 17.3 15.2
## [15] 10.4 10.4 14.7 32.4 30.4 33.9 21.5 15.5 15.2 13.3 19.2 27.3 26.0 30.4
## [29] 15.8 19.7 15.0 21.4
```

```r
detach(mtcars)

# do this idead, if you want
with(mtcars, mpg)
```

```
##  [1] 21.0 21.0 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 17.8 16.4 17.3 15.2
## [15] 10.4 10.4 14.7 32.4 30.4 33.9 21.5 15.5 15.2 13.3 19.2 27.3 26.0 30.4
## [29] 15.8 19.7 15.0 21.4
```
