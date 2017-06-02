-   [Load Data](#load-data)
-   [How to subset columns (note use of .(column list) syntax)](#how-to-subset-columns-note-use-of-.column-list-syntax)
-   [How to subset rows (note no need for following comma)](#how-to-subset-rows-note-no-need-for-following-comma)
-   [How to order variables in ascending or descending order (note use of "-" to connote descending order)](#how-to-order-variables-in-ascending-or-descending-order-note-use-of---to-connote-descending-order)
-   [How to add a column](#how-to-add-a-column)
-   [How to update a columns value based on it's current value](#how-to-update-a-columns-value-based-on-its-current-value)
-   [How to update a columns value based on another column's value](#how-to-update-a-columns-value-based-on-another-columns-value)
-   [How to delete a column](#how-to-delete-a-column)
-   [You can Chain Commands Together!](#you-can-chain-commands-together)
-   [Use functions (e.g. aggregates) on a column](#use-functions-e.g.-aggregates-on-a-column)
-   [Use functions (e.g. aggregates) on a column with grouping](#use-functions-e.g.-aggregates-on-a-column-with-grouping)
-   [Get counts by grouping](#get-counts-by-grouping)

``` r
library(data.table)

getwd()
```

    ## [1] "/home/dchen/git/vt/workshop/training/67-datatable"

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
    download.file("http://download.geonames.org/export/zip/GB_full.csv.zip", data_path)
}
DT <- fread(unzip(zipfile = data_file, exdir = data_dir))
```

    ## 
    Read 49.4% of 1720673 rows
    Read 73.2% of 1720673 rows
    Read 1720673 rows and 12 (of 12) columns from 0.214 GB file in 00:00:04

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

### How to subset columns (note use of .(column list) syntax)

``` r
#subsetting columns
sub_columns <- DT[, .(V2,V3,V4)]
head(sub_columns)
```

    ##          V2                     V3       V4
    ## 1:     AB10               Aberdeen Scotland
    ## 2: AB10 1AB George St/Harbour Ward Scotland
    ## 3: AB10 1AF George St/Harbour Ward Scotland
    ## 4: AB10 1AG George St/Harbour Ward Scotland
    ## 5: AB10 1AH George St/Harbour Ward Scotland
    ## 6: AB10 1AL George St/Harbour Ward Scotland

### How to subset rows (note no need for following comma)

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

### How to order variables in ascending or descending order (note use of "-" to connote descending order)

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

### How to add a column

``` r
#add a new column - here we are adding columns V10 and V11 to create new column V_New
DT[, V_New := V10 + V11]
```

### How to update a columns value based on it's current value

``` r
#update row values - here we change the value to "Abr City" where it equals "Aberdeen City"
DT[V8 == "Aberdeen City", V8 := "Abr City"]
```

### How to update a columns value based on another column's value

``` r
#update row values - here we change the value of column V8 to "Aberdeen City" where column V6 equals "Aberdeen City"
DT[V6 == "Aberdeen City", V8 := "Aberdeen City"]
```

### How to delete a column

``` r
#delete a column - here we delete 2 columns
DT [,c("V6","V7") := NULL ]
```

### You can Chain Commands Together!

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
