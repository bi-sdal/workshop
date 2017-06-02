-   [Load Data](#load-data)
-   [Subset columns (note use of .(column list) syntax)](#subset-columns-note-use-of-.column-list-syntax)
-   [Subset rows (note no need for following comma)](#subset-rows-note-no-need-for-following-comma)
-   [Order variables in ascending or descending order (note use of "-" to connote descending order)](#order-variables-in-ascending-or-descending-order-note-use-of---to-connote-descending-order)
-   [Add a column](#add-a-column)
-   [Update a columns value based on it's current value](#update-a-columns-value-based-on-its-current-value)
-   [Update a columns value based on another column's value](#update-a-columns-value-based-on-another-columns-value)
-   [Delete a column](#delete-a-column)
-   [Chain Commands Together!](#chain-commands-together)
-   [Use functions (e.g. aggregates) on a column](#use-functions-e.g.-aggregates-on-a-column)
-   [Use functions (e.g. aggregates) on a column with grouping](#use-functions-e.g.-aggregates-on-a-column-with-grouping)
-   [Get counts by grouping](#get-counts-by-grouping)
-   [Reshaping](#reshaping)
-   [Applying functions](#applying-functions)

``` r
library(data.table)

getwd()
```

    ## [1] "/home/dchen/git/hub/sdal/workshop/training/100-datatable"

``` r
if (interactive()) {
  data_file <- "data/GB_full.csv.zip"
  data_dir <- "data/"
} else {
  data_file <- "../../data/GB_full.csv.zip"
  data_dir <- '../../data/'
}
data_file
```

    ## [1] "../../data/GB_full.csv.zip"

``` r
data_dir
```

    ## [1] "../../data/"

### Load Data

``` r
!file.exists(data_file)
```

    ## [1] FALSE

``` r
if (!file.exists(data_file)) {
    download.file("http://download.geonames.org/export/zip/GB_full.csv.zip", data_dir)
}
```

``` r
DT <- fread(unzip(zipfile = data_file, exdir = data_dir))
```

    ## 
    Read 37.8% of 1720673 rows
    Read 64.5% of 1720673 rows
    Read 90.7% of 1720673 rows
    Read 1720673 rows and 12 (of 12) columns from 0.214 GB file in 00:00:05

``` r
head(DT)
```

    ##    V1       V2                     V3       V4  V5            V6        V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire          
    ## 2: GB AB10 1AB George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 3: GB AB10 1AF George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 4: GB AB10 1AG George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 5: GB AB10 1AH George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 6: GB AB10 1AL George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ##               V8        V9      V10       V11 V12
    ## 1:                               NA        NA   4
    ## 2: Aberdeen City S12000033 57.14961 -2.096916   6
    ## 3: Aberdeen City S12000033 57.14871 -2.097806   6
    ## 4: Aberdeen City S12000033 57.14907 -2.096997   6
    ## 5: Aberdeen City S12000033 57.14808 -2.094664   6
    ## 6: Aberdeen City S12000033 57.14954 -2.095412   6

``` r
tail(DT)
```

    ##    V1      V2                  V3       V4  V5               V6        V7
    ## 1: GB ZE3 9JT Shetland South Ward Scotland SCT Shetland Islands S12000027
    ## 2: GB ZE3 9JU Shetland South Ward Scotland SCT Shetland Islands S12000027
    ## 3: GB ZE3 9JW Shetland South Ward Scotland SCT Shetland Islands S12000027
    ## 4: GB ZE3 9JX Shetland South Ward Scotland SCT Shetland Islands S12000027
    ## 5: GB ZE3 9JY Shetland South Ward Scotland SCT Shetland Islands S12000027
    ## 6: GB ZE3 9JZ Shetland South Ward Scotland SCT Shetland Islands S12000027
    ##                  V8        V9      V10       V11 V12
    ## 1: Shetland Islands S12000027 59.87262 -1.306772   6
    ## 2: Shetland Islands S12000027 59.88954 -1.307206   6
    ## 3: Shetland Islands S12000027 59.87365 -1.305697   6
    ## 4: Shetland Islands S12000027 59.87529 -1.307502   6
    ## 5: Shetland Islands S12000027 59.89157 -1.313847   6
    ## 6: Shetland Islands S12000027 59.89239 -1.310899   6

``` r
dim(DT)
```

    ## [1] 1720673      12

### Subset columns (note use of .(column list) syntax)

``` r
#subsetting columns
sub_columns <- DT[, .(V2, V3, V4)]
head(sub_columns)
```

    ##          V2                     V3       V4
    ## 1:     AB10               Aberdeen Scotland
    ## 2: AB10 1AB George St/Harbour Ward Scotland
    ## 3: AB10 1AF George St/Harbour Ward Scotland
    ## 4: AB10 1AG George St/Harbour Ward Scotland
    ## 5: AB10 1AH George St/Harbour Ward Scotland
    ## 6: AB10 1AL George St/Harbour Ward Scotland

### Subset rows (note no need for following comma)

``` r
#subsetting rows
sub_rows <- DT[V4 == "England" & V3 == "Beswick"]
head(sub_rows)
```

    ##    V1       V2      V3      V4  V5                       V6        V7
    ## 1: GB     YO25 Beswick England ENG    E Riding of Yorkshire          
    ## 2: GB YO25 9AR Beswick England ENG East Riding of Yorkshire E06000011
    ## 3: GB YO25 9AS Beswick England ENG East Riding of Yorkshire E06000011
    ## 4: GB YO25 9AT Beswick England ENG East Riding of Yorkshire E06000011
    ## 5: GB YO25 9AU Beswick England ENG East Riding of Yorkshire E06000011
    ## 6: GB YO25 9AX Beswick England ENG East Riding of Yorkshire E06000011
    ##                          V8        V9      V10        V11 V12
    ## 1:                                          NA         NA   4
    ## 2: East Riding of Yorkshire E06000011 53.92683 -0.4586771   6
    ## 3: East Riding of Yorkshire E06000011 53.91991 -0.4601350   6
    ## 4: East Riding of Yorkshire E06000011 53.92142 -0.4594243   6
    ## 5: East Riding of Yorkshire E06000011 53.91834 -0.4599795   6
    ## 6: East Riding of Yorkshire E06000011 53.91854 -0.4399180   6

### Order variables in ascending or descending order (note use of "-" to connote descending order)

``` r
#ordering columns
dt_order <- DT[order(V4, -V8)]
head(dt_order)
```

    ##    V1   V2            V3 V4 V5                       V6 V7 V8 V9 V10 V11
    ## 1: GB  GY1 St Peter Port       Guernsey Channel Islands           NA  NA
    ## 2: GB GY10          Sark                Channel Islands           NA  NA
    ## 3: GB  GY3          Vale       Guernsey Channel Islands           NA  NA
    ## 4: GB  GY4     St Martin       Guernsey Channel Islands           NA  NA
    ## 5: GB  GY5        Castel       Guernsey Channel Islands           NA  NA
    ## 6: GB  GY9      Alderney                Channel Islands           NA  NA
    ##    V12
    ## 1:   4
    ## 2:   4
    ## 3:   1
    ## 4:   4
    ## 5:   4
    ## 6:   4

### Add a column

``` r
#add a new column - here we are adding columns V10 and V11 to create new column V_New
DT[, V_New := V10 + V11]
head(DT)
```

    ##    V1       V2                     V3       V4  V5            V6        V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire          
    ## 2: GB AB10 1AB George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 3: GB AB10 1AF George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 4: GB AB10 1AG George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 5: GB AB10 1AH George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 6: GB AB10 1AL George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ##               V8        V9      V10       V11 V12    V_New
    ## 1:                               NA        NA   4       NA
    ## 2: Aberdeen City S12000033 57.14961 -2.096916   6 55.05269
    ## 3: Aberdeen City S12000033 57.14871 -2.097806   6 55.05090
    ## 4: Aberdeen City S12000033 57.14907 -2.096997   6 55.05207
    ## 5: Aberdeen City S12000033 57.14808 -2.094664   6 55.05342
    ## 6: Aberdeen City S12000033 57.14954 -2.095412   6 55.05413

### Update a columns value based on it's current value

``` r
head(DT[V8 == "Aberdeen City"])
```

    ##    V1       V2                     V3       V4  V5            V6        V7
    ## 1: GB AB10 1AB George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 2: GB AB10 1AF George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 3: GB AB10 1AG George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 4: GB AB10 1AH George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 5: GB AB10 1AL George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 6: GB AB10 1AN George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ##               V8        V9      V10       V11 V12    V_New
    ## 1: Aberdeen City S12000033 57.14961 -2.096916   6 55.05269
    ## 2: Aberdeen City S12000033 57.14871 -2.097806   6 55.05090
    ## 3: Aberdeen City S12000033 57.14907 -2.096997   6 55.05207
    ## 4: Aberdeen City S12000033 57.14808 -2.094664   6 55.05342
    ## 5: Aberdeen City S12000033 57.14954 -2.095412   6 55.05413
    ## 6: Aberdeen City S12000033 57.14972 -2.094735   6 55.05498

``` r
#update row values - here we change the value to "Abr City" where it equals "Aberdeen City"
DT[V8 == "Aberdeen City", V8 := "Abr City"]
head(DT[V8 == "Abr City"])
```

    ##    V1       V2                     V3       V4  V5            V6        V7
    ## 1: GB AB10 1AB George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 2: GB AB10 1AF George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 3: GB AB10 1AG George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 4: GB AB10 1AH George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 5: GB AB10 1AL George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 6: GB AB10 1AN George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ##          V8        V9      V10       V11 V12    V_New
    ## 1: Abr City S12000033 57.14961 -2.096916   6 55.05269
    ## 2: Abr City S12000033 57.14871 -2.097806   6 55.05090
    ## 3: Abr City S12000033 57.14907 -2.096997   6 55.05207
    ## 4: Abr City S12000033 57.14808 -2.094664   6 55.05342
    ## 5: Abr City S12000033 57.14954 -2.095412   6 55.05413
    ## 6: Abr City S12000033 57.14972 -2.094735   6 55.05498

### Update a columns value based on another column's value

``` r
#update row values - here we change the value of column V8 to "Aberdeen City" where column V6 equals "Aberdeen City"
DT[V6 == "Aberdeen City", V8 := "Aberdeen City"]
```

### Delete a column

``` r
head(DT)
```

    ##    V1       V2                     V3       V4  V5            V6        V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire          
    ## 2: GB AB10 1AB George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 3: GB AB10 1AF George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 4: GB AB10 1AG George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 5: GB AB10 1AH George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 6: GB AB10 1AL George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ##               V8        V9      V10       V11 V12    V_New
    ## 1:                               NA        NA   4       NA
    ## 2: Aberdeen City S12000033 57.14961 -2.096916   6 55.05269
    ## 3: Aberdeen City S12000033 57.14871 -2.097806   6 55.05090
    ## 4: Aberdeen City S12000033 57.14907 -2.096997   6 55.05207
    ## 5: Aberdeen City S12000033 57.14808 -2.094664   6 55.05342
    ## 6: Aberdeen City S12000033 57.14954 -2.095412   6 55.05413

``` r
#delete a column - here we delete 2 columns
DT [, c("V6","V7") := NULL ]
head(DT)
```

    ##    V1       V2                     V3       V4  V5            V8        V9
    ## 1: GB     AB10               Aberdeen Scotland SCT                        
    ## 2: GB AB10 1AB George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 3: GB AB10 1AF George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 4: GB AB10 1AG George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 5: GB AB10 1AH George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ## 6: GB AB10 1AL George St/Harbour Ward Scotland SCT Aberdeen City S12000033
    ##         V10       V11 V12    V_New
    ## 1:       NA        NA   4       NA
    ## 2: 57.14961 -2.096916   6 55.05269
    ## 3: 57.14871 -2.097806   6 55.05090
    ## 4: 57.14907 -2.096997   6 55.05207
    ## 5: 57.14808 -2.094664   6 55.05342
    ## 6: 57.14954 -2.095412   6 55.05413

### Chain Commands Together!

``` r
DT[V8 == "Aberdeen City", V8 := "Abr City"][, V_New := V10 + V11][,c("V6","V7") := NULL]
```

    ## Warning in `[.data.table`(DT[V8 == "Aberdeen City", `:=`(V8, "Abr City")]
    ## [, : Adding new column 'V6' then assigning NULL (deleting it).

    ## Warning in `[.data.table`(DT[V8 == "Aberdeen City", `:=`(V8, "Abr City")]
    ## [, : Adding new column 'V7' then assigning NULL (deleting it).

### Use functions (e.g. aggregates) on a column

``` r
#compute the average - note you must remove NAs for an aggreagte function to work
DT[, .(average = mean(V10, na.rm = TRUE))]
```

    ##     average
    ## 1: 52.70528

### Use functions (e.g. aggregates) on a column with grouping

``` r
#compute the average
DT[, .(average = mean(V10, na.rm = TRUE)), by = V4]
```

    ##                  V4  average
    ## 1:         Scotland 56.19868
    ## 2:          England 52.36342
    ## 3: Northern Ireland      NaN
    ## 4:            Wales 52.11765
    ## 5:                       NaN

### Get counts by grouping

``` r
#compute the count
DT[, .N, by = V4]
```

    ##                  V4       N
    ## 1:         Scotland  162751
    ## 2:          England 1464285
    ## 3: Northern Ireland     417
    ## 4:            Wales   93123
    ## 5:                       97

### Reshaping

<http://seananderson.ca/2013/10/19/reshape.html>

<https://www.r-bloggers.com/how-to-reshape-data-in-r-tidyr-vs-reshape2/>

#### Melt

``` r
aqdt <- as.data.table(airquality)
  
aqdt_melt <- data.table::melt(data = aqdt, id.vars = c('Month', 'Day'), measure.vars = c('Ozone', 'Solar.R', 'Wind'),
                 variable.name = 'measurement', value.name = 'reading')
```

    ## Warning in melt.data.table(data = aqdt, id.vars = c("Month", "Day"),
    ## measure.vars = c("Ozone", : 'measure.vars' [Ozone, Solar.R, Wind] are not
    ## all of the same type. By order of hierarchy, the molten data value column
    ## will be of type 'double'. All measure variables not of type 'double' will
    ## be coerced to. Check DETAILS in ?melt.data.table for more on coercion.

#### Cast

``` r
data.table::dcast(aqdt_melt, Month + Day ~ measurement)
```

    ## Using 'reading' as value column. Use 'value.var' to override

    ##      Month Day Ozone Solar.R Wind
    ##   1:     5   1    41     190  7.4
    ##   2:     5   2    36     118  8.0
    ##   3:     5   3    12     149 12.6
    ##   4:     5   4    18     313 11.5
    ##   5:     5   5    NA      NA 14.3
    ##  ---                             
    ## 149:     9  26    30     193  6.9
    ## 150:     9  27    NA     145 13.2
    ## 151:     9  28    14     191 14.3
    ## 152:     9  29    18     131  8.0
    ## 153:     9  30    20     223 11.5

### Applying functions
