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
## # A tibble: 53,940 x 10
##    carat       cut color clarity depth table price     x     y     z
##    <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1  0.23     Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
##  2  0.21   Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
##  3  0.23      Good     E     VS1  56.9    65   327  4.05  4.07  2.31
##  4  0.29   Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
##  5  0.31      Good     J     SI2  63.3    58   335  4.34  4.35  2.75
##  6  0.24 Very Good     J    VVS2  62.8    57   336  3.94  3.96  2.48
##  7  0.24 Very Good     I    VVS1  62.3    57   336  3.95  3.98  2.47
##  8  0.26 Very Good     H     SI1  61.9    55   337  4.07  4.11  2.53
##  9  0.22      Fair     E     VS2  65.1    61   337  3.87  3.78  2.49
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
## # A tibble: 53,940 x 1
##    carat
##    <dbl>
##  1  0.23
##  2  0.21
##  3  0.23
##  4  0.29
##  5  0.31
##  6  0.24
##  7  0.24
##  8  0.26
##  9  0.22
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
## # A tibble: 53,940 x 2
##    carat price
##    <dbl> <int>
##  1  0.23   326
##  2  0.21   326
##  3  0.23   327
##  4  0.29   334
##  5  0.31   335
##  6  0.24   336
##  7  0.24   336
##  8  0.26   337
##  9  0.22   337
## 10  0.23   338
## # ... with 53,930 more rows
```

## Subset rows

```r
# various ways of getting rows from data

diamonds[1, ]
```

```
## # A tibble: 1 x 10
##   carat   cut color clarity depth table price     x     y     z
##   <dbl> <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23 Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
```

```r
diamonds[10, ]
```

```
## # A tibble: 1 x 10
##   carat       cut color clarity depth table price     x     y     z
##   <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23 Very Good     H     VS1  59.4    61   338     4  4.05  2.39
```

```r
diamonds[c(1, 2), ]
```

```
## # A tibble: 2 x 10
##   carat     cut color clarity depth table price     x     y     z
##   <dbl>   <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
## 1  0.23   Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
## 2  0.21 Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
```

```r
diamonds[c(11:20), ]
```

```
## # A tibble: 10 x 10
##    carat       cut color clarity depth table price     x     y     z
##    <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1  0.30      Good     J     SI1  64.0    55   339  4.25  4.28  2.73
##  2  0.23     Ideal     J     VS1  62.8    56   340  3.93  3.90  2.46
##  3  0.22   Premium     F     SI1  60.4    61   342  3.88  3.84  2.33
##  4  0.31     Ideal     J     SI2  62.2    54   344  4.35  4.37  2.71
##  5  0.20   Premium     E     SI2  60.2    62   345  3.79  3.75  2.27
##  6  0.32   Premium     E      I1  60.9    58   345  4.38  4.42  2.68
##  7  0.30     Ideal     I     SI2  62.0    54   348  4.31  4.34  2.68
##  8  0.30      Good     J     SI1  63.4    54   351  4.23  4.29  2.70
##  9  0.30      Good     J     SI1  63.8    56   351  4.23  4.26  2.71
## 10  0.30 Very Good     J     SI1  62.7    59   351  4.21  4.27  2.66
```

```r
diamonds[11:20, ]
```

```
## # A tibble: 10 x 10
##    carat       cut color clarity depth table price     x     y     z
##    <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1  0.30      Good     J     SI1  64.0    55   339  4.25  4.28  2.73
##  2  0.23     Ideal     J     VS1  62.8    56   340  3.93  3.90  2.46
##  3  0.22   Premium     F     SI1  60.4    61   342  3.88  3.84  2.33
##  4  0.31     Ideal     J     SI2  62.2    54   344  4.35  4.37  2.71
##  5  0.20   Premium     E     SI2  60.2    62   345  3.79  3.75  2.27
##  6  0.32   Premium     E      I1  60.9    58   345  4.38  4.42  2.68
##  7  0.30     Ideal     I     SI2  62.0    54   348  4.31  4.34  2.68
##  8  0.30      Good     J     SI1  63.4    54   351  4.23  4.29  2.70
##  9  0.30      Good     J     SI1  63.8    56   351  4.23  4.26  2.71
## 10  0.30 Very Good     J     SI1  62.7    59   351  4.21  4.27  2.66
```

## Subset rows and columns


```r
# getting rows and columns from your data
diamonds[11:20, c('cut', 'color')]
```

```
## # A tibble: 10 x 2
##          cut color
##        <ord> <ord>
##  1      Good     J
##  2     Ideal     J
##  3   Premium     F
##  4     Ideal     J
##  5   Premium     E
##  6   Premium     E
##  7     Ideal     I
##  8      Good     J
##  9      Good     J
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
## # A tibble: 6 x 10
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
## # A tibble: 6 x 10
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

# Conditonal Subsetting

We can perform conditional subsetting by using the way we subset dataframes: `[ , ]`


```r
summary(mtcars)
```

```
##       mpg             cyl             disp             hp       
##  Min.   :10.40   Min.   :4.000   Min.   : 71.1   Min.   : 52.0  
##  1st Qu.:15.43   1st Qu.:4.000   1st Qu.:120.8   1st Qu.: 96.5  
##  Median :19.20   Median :6.000   Median :196.3   Median :123.0  
##  Mean   :20.09   Mean   :6.188   Mean   :230.7   Mean   :146.7  
##  3rd Qu.:22.80   3rd Qu.:8.000   3rd Qu.:326.0   3rd Qu.:180.0  
##  Max.   :33.90   Max.   :8.000   Max.   :472.0   Max.   :335.0  
##       drat             wt             qsec             vs        
##  Min.   :2.760   Min.   :1.513   Min.   :14.50   Min.   :0.0000  
##  1st Qu.:3.080   1st Qu.:2.581   1st Qu.:16.89   1st Qu.:0.0000  
##  Median :3.695   Median :3.325   Median :17.71   Median :0.0000  
##  Mean   :3.597   Mean   :3.217   Mean   :17.85   Mean   :0.4375  
##  3rd Qu.:3.920   3rd Qu.:3.610   3rd Qu.:18.90   3rd Qu.:1.0000  
##  Max.   :4.930   Max.   :5.424   Max.   :22.90   Max.   :1.0000  
##        am              gear            carb           cars          
##  Min.   :0.0000   Min.   :3.000   Min.   :1.000   Length:32         
##  1st Qu.:0.0000   1st Qu.:3.000   1st Qu.:2.000   Class :character  
##  Median :0.0000   Median :4.000   Median :2.000   Mode  :character  
##  Mean   :0.4062   Mean   :3.688   Mean   :2.812                     
##  3rd Qu.:1.0000   3rd Qu.:4.000   3rd Qu.:4.000                     
##  Max.   :1.0000   Max.   :5.000   Max.   :8.000                     
##      cyl_wt     
##  Min.   :1.254  
##  1st Qu.:1.732  
##  Median :2.074  
##  Mean   :1.963  
##  3rd Qu.:2.241  
##  Max.   :2.644
```

Let's subset our data and look for all the cars where the `mpg` is less than `15.43`


```r
mtcars[mtcars$mpg < 15.43, ]
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
##                                    cars   cyl_wt
## Duster 360                   Duster 360 2.240896
## Merc 450SLC                 Merc 450SLC 2.116402
## Cadillac Fleetwood   Cadillac Fleetwood 1.523810
## Lincoln Continental Lincoln Continental 1.474926
## Chrysler Imperial     Chrysler Imperial 1.496726
## AMC Javelin                 AMC Javelin 2.328967
## Camaro Z28                   Camaro Z28 2.083333
## Maserati Bora             Maserati Bora 2.240896
```

The reason why this works is becuase the row subsetter returns a vector of `TRUE`/`FALSE` values


```r
mtcars$mpg < 15.43
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE
## [12] FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE
## [23]  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE
```

When you give a dataframe row or column slicer a vector of booleans (i.e., TRUE/FALSE),
it will return the rows and/or columns that have a TRUE.

## Subsetting a single column with `[ ]`

Subsetting and slicing dataframs can be weird.
when you subset a `data.frame` that ends up with 1 column it will come back as a `vector` and not a `data.frame`

Note, `tibble` objects don't do this.

You can prevent this automatic conversion to a vector by passing in the `drop = FALSE` into the `[ ]` subsetter


```r
mtcars[mtcars$mpg < 15.43, 'cyl', drop = FALSE]
```

```
##                     cyl
## Duster 360            8
## Merc 450SLC           8
## Cadillac Fleetwood    8
## Lincoln Continental   8
## Chrysler Imperial     8
## AMC Javelin           8
## Camaro Z28            8
## Maserati Bora         8
```

Otherwise you will have to manually convert the vector back to a `data.frame` by using `as.data.frame`


```r
as.data.frame(mtcars[mtcars$mpg < 15.43, 'cyl'])
```

```
##   mtcars[mtcars$mpg < 15.43, "cyl"]
## 1                                 8
## 2                                 8
## 3                                 8
## 4                                 8
## 5                                 8
## 6                                 8
## 7                                 8
## 8                                 8
```

But then you'll have to end up changing the column name


```r
bool_df <- as.data.frame(mtcars[mtcars$mpg < 15.43, 'cyl'])
names(bool_df) <- 'cyl'
bool_df
```

```
##   cyl
## 1   8
## 2   8
## 3   8
## 4   8
## 5   8
## 6   8
## 7   8
## 8   8
```

## Multiple boolean conditions

You can use the `|` or `&` to have an OR or AND operator in your boolean condition, respectively.


```r
mtcars[mtcars$mpg < 15.43 | mtcars$cyl == 4, ]
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
##                                    cars   cyl_wt
## Datsun 710                   Datsun 710 1.724138
## Duster 360                   Duster 360 2.240896
## Merc 240D                     Merc 240D 1.253918
## Merc 230                       Merc 230 1.269841
## Merc 450SLC                 Merc 450SLC 2.116402
## Cadillac Fleetwood   Cadillac Fleetwood 1.523810
## Lincoln Continental Lincoln Continental 1.474926
## Chrysler Imperial     Chrysler Imperial 1.496726
## Fiat 128                       Fiat 128 1.818182
## Honda Civic                 Honda Civic 2.476780
## Toyota Corolla           Toyota Corolla 2.179837
## Toyota Corona             Toyota Corona 1.622718
## AMC Javelin                 AMC Javelin 2.328967
## Camaro Z28                   Camaro Z28 2.083333
## Fiat X1-9                     Fiat X1-9 2.067183
## Porsche 914-2             Porsche 914-2 1.869159
## Lotus Europa               Lotus Europa 2.643754
## Maserati Bora             Maserati Bora 2.240896
## Volvo 142E                   Volvo 142E 1.438849
```


```r
mtcars[mtcars$mpg < 15.43 & mtcars$cyl == 4, ]
```

```
##  [1] mpg    cyl    disp   hp     drat   wt     qsec   vs     am     gear  
## [11] carb   cars   cyl_wt
## <0 rows> (or 0-length row.names)
```


# NA Missing Values

Missing values in R are coded as an `NA` value


```r
NA
```

```
## [1] NA
```


You can't use the `==` operator to check for missing values


```r
NA == NA
```

```
## [1] NA
```


```r
NA == TRUE
```

```
## [1] NA
```


```r
NA == FALSE
```

```
## [1] NA
```


```r
NA == 3
```

```
## [1] NA
```


You have to use a special function to check for missing values


```r
is.na(NA)
```

```
## [1] TRUE
```


