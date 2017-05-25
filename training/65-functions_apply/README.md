# README
Daniel Chen  
  



To prepare the data example I will use the `mtcars` dataframe,
and create a new column, `cars` by passing in the `row.name`


```r
mtcars$cars <- row.names(mtcars)
```


# Functions


```r
my_mean <- function(){
  print(3)
}

my_mean()
```

```
## [1] 3
```


```r
my_mean <- function(x, y){
  print(3)
}

my_mean()
```

```
## [1] 3
```

```r
my_mean(10, 20)
```

```
## [1] 3
```


## Functions will automaticall return the last caculated value

```r
my_mean <- function(x, y) {
    (x + y) / 2
}
```

## Explicitly return a value in a function

```r
my_mean <- function(x, y) {
    return((x + y) / 2)
}
```


```r
my_mean <- function(x, y) {
    val <- (x + y) / 2
    return(val)
}
```

# Functions on data

The summary function worked on our data column-by-column.

You can write your own function that work column-by-column, or row0-by-row by using `apply`


## Visually check your function

When starting to write a function that you will apply on your data,
it's good to write the function on its own with the parameters you will use to visually inspect it


```r
# create a function that calculates the ratio of a cyl_val and wt_val
# append this ratio to a car_name separated by a dash
rename_car <- function(cyl_val, wt_value, car_name) {
    cyl_wt_ratio <- as.numeric(cyl_val) / as.numeric(wt_value)
    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}

rename_car(10, 20, 'pizaaaaaaaaa')
```

```
## [1] "pizaaaaaaaaa - 0.5"
```

## Preparing your function for an `apply`

The way apply works is the first parameter is the dataframe you will work on

The second parameter, `MARGIN` will tell apply to work row-by-row (`1`)
or column-by-column (`2`).

The last parameter is the function you want to apply,
note that you do not need the round brackets when you pass in functions into apply,
just the function name.

To convert your function to be `apply` ready, you replace the parameters
of your function with a new parameter, e.g., `row_data` or `col_data`.
then within the function, you will use the subsetting methods to subset the rows or columns your function needs


```r
rename_car <- function(row_data) {
    cyl_val <- row_data['cyl']
    wt_value <- row_data['wt']
    car_name <- row_data['cars']

    cyl_wt_ratio <- as.numeric(cyl_val) / as.numeric(wt_value)

    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}
```

with your new function, you can then apply it to your data


```r
apply(mtcars, 1, rename_car)
```

```
##                                Mazda RX4 
##           "Mazda RX4 - 2.29007633587786" 
##                            Mazda RX4 Wag 
##       "Mazda RX4 Wag - 2.08695652173913" 
##                               Datsun 710 
##          "Datsun 710 - 1.72413793103448" 
##                           Hornet 4 Drive 
##      "Hornet 4 Drive - 1.86625194401244" 
##                        Hornet Sportabout 
##   "Hornet Sportabout - 2.32558139534884" 
##                                  Valiant 
##             "Valiant - 1.73410404624277" 
##                               Duster 360 
##          "Duster 360 - 2.24089635854342" 
##                                Merc 240D 
##           "Merc 240D - 1.25391849529781" 
##                                 Merc 230 
##            "Merc 230 - 1.26984126984127" 
##                                 Merc 280 
##            "Merc 280 - 1.74418604651163" 
##                                Merc 280C 
##           "Merc 280C - 1.74418604651163" 
##                               Merc 450SE 
##          "Merc 450SE - 1.96560196560197" 
##                               Merc 450SL 
##          "Merc 450SL - 2.14477211796247" 
##                              Merc 450SLC 
##         "Merc 450SLC - 2.11640211640212" 
##                       Cadillac Fleetwood 
##  "Cadillac Fleetwood - 1.52380952380952" 
##                      Lincoln Continental 
## "Lincoln Continental - 1.47492625368732" 
##                        Chrysler Imperial 
##   "Chrysler Imperial - 1.49672591206735" 
##                                 Fiat 128 
##            "Fiat 128 - 1.81818181818182" 
##                              Honda Civic 
##         "Honda Civic - 2.47678018575851" 
##                           Toyota Corolla 
##      "Toyota Corolla - 2.17983651226158" 
##                            Toyota Corona 
##       "Toyota Corona - 1.62271805273834" 
##                         Dodge Challenger 
##    "Dodge Challenger - 2.27272727272727" 
##                              AMC Javelin 
##         "AMC Javelin - 2.32896652110626" 
##                               Camaro Z28 
##          "Camaro Z28 - 2.08333333333333" 
##                         Pontiac Firebird 
##    "Pontiac Firebird - 2.08062418725618" 
##                                Fiat X1-9 
##            "Fiat X1-9 - 2.0671834625323" 
##                            Porsche 914-2 
##       "Porsche 914-2 - 1.86915887850467" 
##                             Lotus Europa 
##        "Lotus Europa - 2.64375413086583" 
##                           Ford Pantera L 
##      "Ford Pantera L - 2.52365930599369" 
##                             Ferrari Dino 
##        "Ferrari Dino - 2.16606498194946" 
##                            Maserati Bora 
##       "Maserati Bora - 2.24089635854342" 
##                               Volvo 142E 
##          "Volvo 142E - 1.43884892086331"
```

You can see this returns a vector of values, but we want to assign this to a new column


```r
mtcars$car_ratio <- apply(mtcars, 1, rename_car)

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
##                                cars                            car_ratio
## Mazda RX4                 Mazda RX4         Mazda RX4 - 2.29007633587786
## Mazda RX4 Wag         Mazda RX4 Wag     Mazda RX4 Wag - 2.08695652173913
## Datsun 710               Datsun 710        Datsun 710 - 1.72413793103448
## Hornet 4 Drive       Hornet 4 Drive    Hornet 4 Drive - 1.86625194401244
## Hornet Sportabout Hornet Sportabout Hornet Sportabout - 2.32558139534884
## Valiant                     Valiant           Valiant - 1.73410404624277
```


# Common mistakes

What went wrong when I was live coding?

Here's the code I originally typed


```r
rename_car <- function(cyl_val, wt_value, car_name) {
    cyl_wt_ratio <- as.numeric(cyl_val) / as.numeric(wt_value)
    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}
```

The first time I `apply`ified the function I wrote this:


```r
rename_car <- function(row_data, cyl_val, wt_value, car_name) {
    cyl_wt_ratio <- as.numeric(cyl_val) / as.numeric(wt_value)
    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}
```

Because the way apply works is that the row (or column) data all gets passed into the
first argument of the function, but the output was 'strange'/wrong.

I attempted to fix this by subsetting the 'columns' that the row is passed in.


```r
rename_car <- function(row_data) {
    cyl_val <- row_data[, 'cyl']
    wt_value <- row_data[, 'wt']
    car_name <- row_data[, 'cars']

    cyl_wt_ratio <- cyl_val / wt_value

    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}

apply(mtcars, 1, rename_car)
```

```
## Error in row_data[, "cyl"]: incorrect number of dimensions
```

That didn't work because the row of data got passed in.
This row or `vector` can't be subset by using the `[, ]` notation (not the comma)
because there is no way you can subset both a row and column on a 1-dimentional thing (which a row/vector is)

So I had to remove the comma from the subsetting


```r
rename_car <- function(row_data) {
    cyl_val <- row_data['cyl']
    wt_value <- row_data['wt']
    car_name <- row_data['cars']

    cyl_wt_ratio <- cyl_val / wt_value

    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}

apply(mtcars, 1, rename_car)
```

```
## Error in cyl_val/wt_value: non-numeric argument to binary operator
```

Next problem was this `non-numeric argument to binary operator` error.

It tells me that the error occurs when trying to calculate `cyl_val/wt_value`.

the `binary operator` the error message is refering to is the division `/`,
and it is telling me that values passed into the division is not a `numeric` value.

We fix this by forcing the values to be `numeric` by using the `as.numeric` function


```r
rename_car <- function(row_data) {
    cyl_val <- row_data['cyl']
    wt_value <- row_data['wt']
    car_name <- row_data['cars']

    cyl_wt_ratio <- as.numeric(cyl_val) / as.numeric(wt_value)

    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}
```

you can also do the conversion at the very beginning


```r
rename_car <- function(row_data) {
    cyl_val <- as.numeric(row_data['cyl'])
    wt_value <- as.numeric(row_data['wt'])
    car_name <- row_data['cars']

    cyl_wt_ratio <- cyl_val / wt_value

    new_name <- paste(car_name, '-', cyl_wt_ratio)
    return(new_name)
}
```

In R (and statistical datasets), we know each column will be the same data type (character, numeric, etc)

But in statistical datasets, rows can be of mixed types.

What R does in the apply is that when the row data is passed,
it converts everything into the data type that is common to all the row values, a character vector (aka string)

which is why we need to manually convert the `cyl` and `wt` values into numerics.
