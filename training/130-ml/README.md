-   [Introducetion](#introducetion)
    -   [](#section)
-   [Summary Statistics](#summary-statistics)
    -   [Correlation](#correlation)
    -   [Covariance](#covariance)
-   [Linear Models](#linear-models)
-   [Generalized Linear Models](#generalized-linear-models)
    -   [Logistic](#logistic)
    -   [Survival Analysis](#survival-analysis)

> The fact that data science exists as a field is a colossal failure of statistics. To me, that is what statistics is all about. It is gaining insight from data using modelling and visualization. Data munging and manipulation is hard and statistics has just said that’s not our domain.”

-   Hadley Wickham, PhD

<https://priceonomics.com/hadley-wickham-the-man-who-revolutionized-r/>

``` r
diamonds <- ggplot2::diamonds
economics <- ggplot2::economics
```

Introducetion
=============

Understanding your dataset is more important than fitting a model and calling it a day.

<https://rpubs.com/neilfws/91339>

``` r
data(anscombe)
summary(anscombe)
```

    ##        x1             x2             x3             x4    
    ##  Min.   : 4.0   Min.   : 4.0   Min.   : 4.0   Min.   : 8  
    ##  1st Qu.: 6.5   1st Qu.: 6.5   1st Qu.: 6.5   1st Qu.: 8  
    ##  Median : 9.0   Median : 9.0   Median : 9.0   Median : 8  
    ##  Mean   : 9.0   Mean   : 9.0   Mean   : 9.0   Mean   : 9  
    ##  3rd Qu.:11.5   3rd Qu.:11.5   3rd Qu.:11.5   3rd Qu.: 8  
    ##  Max.   :14.0   Max.   :14.0   Max.   :14.0   Max.   :19  
    ##        y1               y2              y3              y4        
    ##  Min.   : 4.260   Min.   :3.100   Min.   : 5.39   Min.   : 5.250  
    ##  1st Qu.: 6.315   1st Qu.:6.695   1st Qu.: 6.25   1st Qu.: 6.170  
    ##  Median : 7.580   Median :8.140   Median : 7.11   Median : 7.040  
    ##  Mean   : 7.501   Mean   :7.501   Mean   : 7.50   Mean   : 7.501  
    ##  3rd Qu.: 8.570   3rd Qu.:8.950   3rd Qu.: 7.98   3rd Qu.: 8.190  
    ##  Max.   :10.840   Max.   :9.260   Max.   :12.74   Max.   :12.500

``` r
sapply(1:4, function(x) cor(anscombe[, x], anscombe[, x+4]))
```

    ## [1] 0.8164205 0.8162365 0.8162867 0.8165214

``` r
sapply(5:8, function(x) var(anscombe[, x]))
```

    ## [1] 4.127269 4.127629 4.122620 4.123249

``` r
lm(y1 ~ x1, data = anscombe)
```

    ## 
    ## Call:
    ## lm(formula = y1 ~ x1, data = anscombe)
    ## 
    ## Coefficients:
    ## (Intercept)           x1  
    ##      3.0001       0.5001

``` r
library(ggplot2)
library(gridExtra)

p1 <- ggplot(anscombe) +
  geom_point(aes(x1, y1), color = "darkorange", size = 3) +
  theme_bw() +
  scale_x_continuous(breaks = seq(0, 20, 2)) +
  scale_y_continuous(breaks = seq(0, 12, 2)) +
  geom_abline(intercept = 3, slope = 0.5, color = "cornflowerblue") +
  expand_limits(x = 0, y = 0) +
  labs(title = "dataset 1")

p2 <- ggplot(anscombe) +
  geom_point(aes(x2, y2), color = "darkorange", size = 3) +
  theme_bw() +
  scale_x_continuous(breaks = seq(0, 20, 2)) +
  scale_y_continuous(breaks = seq(0, 12, 2)) +
  geom_abline(intercept = 3, slope = 0.5, color = "cornflowerblue") +
  expand_limits(x = 0, y = 0) +
  labs(title = "dataset 2")

p3 <- ggplot(anscombe) +
  geom_point(aes(x3, y3), color = "darkorange", size = 3) +
  theme_bw() +
  scale_x_continuous(breaks = seq(0, 20, 2)) +
  scale_y_continuous(breaks = seq(0, 12, 2)) +
  geom_abline(intercept = 3, slope = 0.5, color = "cornflowerblue") +
  expand_limits(x = 0, y = 0) +
  labs(title = "dataset 3")

p4 <- ggplot(anscombe) +
  geom_point(aes(x4, y4), color = "darkorange", size = 3) +
  theme_bw() +
  scale_x_continuous(breaks = seq(0, 20, 2)) +
  scale_y_continuous(breaks = seq(0, 12, 2)) +
  geom_abline(intercept = 3, slope = 0.5, color = "cornflowerblue") +
  expand_limits(x = 0, y = 0) +
  labs(title = "dataset 4")

sdalr::multiplot(p1, p2, p3, p4, cols = 2)
```

![](README_files/figure-markdown_github/unnamed-chunk-6-1.png)

``` r
data("anscombosaurus", package = 'sdalr')

summary(anscombosaurus)
```

    ##        x               y         
    ##  Min.   :22.31   Min.   : 2.949  
    ##  1st Qu.:44.10   1st Qu.:25.288  
    ##  Median :53.33   Median :46.026  
    ##  Mean   :54.26   Mean   :47.832  
    ##  3rd Qu.:64.74   3rd Qu.:68.526  
    ##  Max.   :98.21   Max.   :99.487

Summary Statistics
==================

``` r
summary(diamonds$price)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     326     950    2401    3933    5324   18823

``` r
summary(diamonds)
```

    ##      carat               cut        color        clarity     
    ##  Min.   :0.2000   Fair     : 1610   D: 6775   SI1    :13065  
    ##  1st Qu.:0.4000   Good     : 4906   E: 9797   VS2    :12258  
    ##  Median :0.7000   Very Good:12082   F: 9542   SI2    : 9194  
    ##  Mean   :0.7979   Premium  :13791   G:11292   VS1    : 8171  
    ##  3rd Qu.:1.0400   Ideal    :21551   H: 8304   VVS2   : 5066  
    ##  Max.   :5.0100                     I: 5422   VVS1   : 3655  
    ##                                     J: 2808   (Other): 2531  
    ##      depth           table           price             x         
    ##  Min.   :43.00   Min.   :43.00   Min.   :  326   Min.   : 0.000  
    ##  1st Qu.:61.00   1st Qu.:56.00   1st Qu.:  950   1st Qu.: 4.710  
    ##  Median :61.80   Median :57.00   Median : 2401   Median : 5.700  
    ##  Mean   :61.75   Mean   :57.46   Mean   : 3933   Mean   : 5.731  
    ##  3rd Qu.:62.50   3rd Qu.:59.00   3rd Qu.: 5324   3rd Qu.: 6.540  
    ##  Max.   :79.00   Max.   :95.00   Max.   :18823   Max.   :10.740  
    ##                                                                  
    ##        y                z         
    ##  Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.: 4.720   1st Qu.: 2.910  
    ##  Median : 5.710   Median : 3.530  
    ##  Mean   : 5.735   Mean   : 3.539  
    ##  3rd Qu.: 6.540   3rd Qu.: 4.040  
    ##  Max.   :58.900   Max.   :31.800  
    ## 

``` r
mean(diamonds$price)
```

    ## [1] 3932.8

``` r
sd(diamonds$price)
```

    ## [1] 3989.44

``` r
quantile(diamonds$price, probs = 0.25)
```

    ## 25% 
    ## 950

Correlation
-----------

$$
r\_{xy} = \\dfrac{\\sum\_{i = 1}^n (x\_i - \\bar{x}) (y\_i - \\bar{y})}{(n - 1) s\_x s\_y}
$$

``` r
cor(economics$pce, economics$psavert)
```

    ## [1] -0.837069

``` r
GGally::ggpairs(data = dplyr::select(economics, -date, -pop))
```

![](README_files/figure-markdown_github/unnamed-chunk-12-1.png)

``` r
GGally::wrap(
  GGally::ggpairs(data = dplyr::select(economics, -date, -pop)),
  labelSize = 8
)
```

    ## function (data, mapping, ...) 
    ## {
    ##     allParams$data <- data
    ##     allParams$mapping <- mapping
    ##     argsList <- list(...)
    ##     allParams[names(argsList)] <- argsList
    ##     do.call(original_fn, allParams)
    ## }
    ## <environment: 0x4abc970>
    ## attr(,"class")
    ## [1] "ggmatrix_fn_with_params"
    ## attr(,"name")
    ##  [1] "data"                "plots"               "title"              
    ##  [4] "xlab"                "ylab"                "showStrips"         
    ##  [7] "xAxisLabels"         "yAxisLabels"         "showXAxisPlotLabels"
    ## [10] "showYAxisPlotLabels" "labeller"            "xProportions"       
    ## [13] "yProportions"        "legend"              "gg"                 
    ## [16] "nrow"                "ncol"                "byrow"              
    ## attr(,"params")
    ## attr(,"params")$labelSize
    ## [1] 8
    ## 
    ## attr(,"fn")

![](README_files/figure-markdown_github/unnamed-chunk-13-1.png)

Covariance
----------

$$
cov(X, Y) = \\dfrac{1}{N - 1} \\sum\_{i = 1}^N (x\_i - \\bar{x})(y\_i - \\bar{y})
$$

``` r
cov(economics$pce, economics$psavert)
```

    ## [1] -9361.028

``` r
identical(
  cov(economics$pce, economics$psavert),
  cor(economics$pce, economics$psavert) *
    sd(economics$pce) *
    sd(economics$psavert)
)
```

    ## [1] TRUE

Linear Models
=============

``` r
data(diamonds, package = 'ggplot2')
```

``` r
head(diamonds)
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

``` r
glm(formula = price ~ carat, data = diamonds)
```

    ## 
    ## Call:  glm(formula = price ~ carat, data = diamonds)
    ## 
    ## Coefficients:
    ## (Intercept)        carat  
    ##       -2256         7756  
    ## 
    ## Degrees of Freedom: 53939 Total (i.e. Null);  53938 Residual
    ## Null Deviance:       8.585e+11 
    ## Residual Deviance: 1.293e+11     AIC: 945500

``` r
glm(formula = price ~ carat + depth + table + x + y + z, data = diamonds)
```

    ## 
    ## Call:  glm(formula = price ~ carat + depth + table + x + y + z, data = diamonds)
    ## 
    ## Coefficients:
    ## (Intercept)        carat        depth        table            x  
    ##    20849.32     10686.31      -203.15      -102.45     -1315.67  
    ##           y            z  
    ##       66.32        41.63  
    ## 
    ## Degrees of Freedom: 53939 Total (i.e. Null);  53933 Residual
    ## Null Deviance:       8.585e+11 
    ## Residual Deviance: 1.209e+11     AIC: 941800

``` r
m <- glm(formula = price ~ carat + depth + table + x + y + z, data = diamonds)
summary(m)
```

    ## 
    ## Call:
    ## glm(formula = price ~ carat + depth + table + x + y + z, data = diamonds)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -23878.2    -615.0     -50.7     347.9   12759.2  
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 20849.316    447.562  46.584  < 2e-16 ***
    ## carat       10686.309     63.201 169.085  < 2e-16 ***
    ## depth        -203.154      5.504 -36.910  < 2e-16 ***
    ## table        -102.446      3.084 -33.216  < 2e-16 ***
    ## x           -1315.668     43.070 -30.547  < 2e-16 ***
    ## y              66.322     25.523   2.599  0.00937 ** 
    ## z              41.628     44.305   0.940  0.34744    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 2240873)
    ## 
    ##     Null deviance: 8.5847e+11  on 53939  degrees of freedom
    ## Residual deviance: 1.2086e+11  on 53933  degrees of freedom
    ## AIC: 941815
    ## 
    ## Number of Fisher Scoring iterations: 2

``` r
library(broom)

broom::tidy(m)
```

    ##          term   estimate  std.error   statistic       p.value
    ## 1 (Intercept) 20849.3164 447.561823  46.5842155  0.000000e+00
    ## 2       carat 10686.3091  63.200807 169.0850101  0.000000e+00
    ## 3       depth  -203.1541   5.503984 -36.9103634 1.508901e-294
    ## 4       table  -102.4457   3.084213 -33.2161394 1.663814e-239
    ## 5           x -1315.6678  43.070264 -30.5470111 3.378404e-203
    ## 6           y    66.3216  25.523021   2.5985013  9.365715e-03
    ## 7           z    41.6277  44.304632   0.9395789  3.474378e-01

``` r
head(broom::augment(m))
```

    ##   price carat depth table    x    y    z    .fitted  .se.fit    .resid
    ## 1   326  0.23  61.5    55 3.95 3.98 2.43 346.909718 17.43614 -20.90972
    ## 2   326  0.21  59.8    61 3.89 3.84 2.31 -71.468765 22.50732 397.46876
    ## 3   327  0.23  56.9    65 4.05 4.07 2.31 126.368674 33.73874 200.63133
    ## 4   334  0.29  62.4    58 4.20 4.23 2.63 193.901639 13.92640 140.09836
    ## 5   335  0.31  63.3    58 4.34 4.35 2.75  53.549591 13.96294 281.45041
    ## 6   336  0.24  62.8    57 3.94 3.96 2.48  -1.307132 16.65387 337.30713
    ##           .hat   .sigma      .cooksd  .std.resid
    ## 1 1.356700e-04 1496.968 3.782533e-09 -0.01396912
    ## 2 2.260635e-04 1496.967 2.277810e-06  0.26554830
    ## 3 5.079728e-04 1496.968 1.304861e-06  0.13406040
    ## 4 8.654865e-05 1496.968 1.083144e-07  0.09359298
    ## 5 8.700346e-05 1496.968 4.394411e-07  0.18802353
    ## 6 1.237693e-04 1496.968 8.979587e-07  0.22534287

``` r
broom::glance(m)
```

    ##   null.deviance df.null    logLik    AIC      BIC    deviance df.residual
    ## 1  858473135517   53939 -470899.5 941815 941886.2 1.20857e+11       53933

``` r
plot(m)
```

![](README_files/figure-markdown_github/unnamed-chunk-22-1.png)![](README_files/figure-markdown_github/unnamed-chunk-22-2.png)![](README_files/figure-markdown_github/unnamed-chunk-22-3.png)![](README_files/figure-markdown_github/unnamed-chunk-22-4.png)

Generalized Linear Models
=========================

Logistic
--------

    [, 1]    mpg     Miles/(US) gallon
    [, 2]    cyl     Number of cylinders
    [, 3]    disp    Displacement (cu.in.)
    [, 4]    hp  Gross horsepower
    [, 5]    drat    Rear axle ratio
    [, 6]    wt  Weight (1000 lbs)
    [, 7]    qsec    1/4 mile time
    [, 8]    vs  V/S
    [, 9]    am  Transmission (0 = automatic, 1 = manual)
    [,10]    gear    Number of forward gears
    [,11]    carb    Number of carburetors

``` r
mpg_bin <- function(mpg) {
  if (mpg > 22) {
    return('good')
  } else {
    return('poor')
  }
}

mpg_bin_int <- function(mpg) {
  if (mpg > 22) {
    return(1)
  } else {
    return(0)
  }
}

mtcars$mpg_bin <- sapply(X = mtcars$mpg, FUN = mpg_bin)
mtcars$mpg_bin_int <- sapply(X = mtcars$mpg, FUN = mpg_bin_int)
head(mtcars)
```

    ##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
    ##                   mpg_bin mpg_bin_int
    ## Mazda RX4            poor           0
    ## Mazda RX4 Wag        poor           0
    ## Datsun 710           good           1
    ## Hornet 4 Drive       poor           0
    ## Hornet Sportabout    poor           0
    ## Valiant              poor           0

``` r
m <- glm(formula = as.factor(mpg_bin) ~ cyl + hp + wt + as.factor(am),
         data = mtcars,
         family = binomial(link = 'logit'))
```

    ## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

``` r
summary(m)
```

    ## 
    ## Call:
    ## glm(formula = as.factor(mpg_bin) ~ cyl + hp + wt + as.factor(am), 
    ##     family = binomial(link = "logit"), data = mtcars)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -1.35924  -0.00017   0.00000   0.00000   1.35930  
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)      -73.3121 10328.2144  -0.007    0.994
    ## cyl                8.9415  2581.9822   0.003    0.997
    ## hp                 0.3275     0.7501   0.437    0.662
    ## wt                 2.1764     3.1433   0.692    0.489
    ## as.factor(am)1    -3.4853     9.8480  -0.354    0.723
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 38.0243  on 31  degrees of freedom
    ## Residual deviance:  5.2874  on 27  degrees of freedom
    ## AIC: 15.287
    ## 
    ## Number of Fisher Scoring iterations: 21

``` r
broom::tidy(m)
```

    ##             term    estimate    std.error    statistic   p.value
    ## 1    (Intercept) -73.3120582 1.032821e+04 -0.007098232 0.9943365
    ## 2            cyl   8.9414791 2.581982e+03  0.003463029 0.9972369
    ## 3             hp   0.3274562 7.501192e-01  0.436538924 0.6624458
    ## 4             wt   2.1764112 3.143263e+00  0.692405086 0.4886830
    ## 5 as.factor(am)1  -3.4853254 9.848018e+00 -0.353911369 0.7234053

``` r
m <- glm(formula = as.factor(mpg_bin_int) ~ cyl + hp + wt + as.factor(am),
         data = mtcars,
         family = binomial(link = 'logit'))
```

    ## Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

``` r
summary(m)
```

    ## 
    ## Call:
    ## glm(formula = as.factor(mpg_bin_int) ~ cyl + hp + wt + as.factor(am), 
    ##     family = binomial(link = "logit"), data = mtcars)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -1.35930   0.00000   0.00000   0.00017   1.35924  
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)       73.3121 10328.2144   0.007    0.994
    ## cyl               -8.9415  2581.9822  -0.003    0.997
    ## hp                -0.3275     0.7501  -0.437    0.662
    ## wt                -2.1764     3.1433  -0.692    0.489
    ## as.factor(am)1     3.4853     9.8480   0.354    0.723
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 38.0243  on 31  degrees of freedom
    ## Residual deviance:  5.2874  on 27  degrees of freedom
    ## AIC: 15.287
    ## 
    ## Number of Fisher Scoring iterations: 21

``` r
broom::tidy(m)
```

    ##             term   estimate    std.error    statistic   p.value
    ## 1    (Intercept) 73.3120582 1.032821e+04  0.007098232 0.9943365
    ## 2            cyl -8.9414791 2.581982e+03 -0.003463029 0.9972369
    ## 3             hp -0.3274562 7.501192e-01 -0.436538924 0.6624458
    ## 4             wt -2.1764112 3.143263e+00 -0.692405086 0.4886830
    ## 5 as.factor(am)1  3.4853254 9.848018e+00  0.353911369 0.7234053

``` r
results  <- broom::tidy(m)
results$or <- exp(results$estimate)
results
```

    ##             term   estimate    std.error    statistic   p.value
    ## 1    (Intercept) 73.3120582 1.032821e+04  0.007098232 0.9943365
    ## 2            cyl -8.9414791 2.581982e+03 -0.003463029 0.9972369
    ## 3             hp -0.3274562 7.501192e-01 -0.436538924 0.6624458
    ## 4             wt -2.1764112 3.143263e+00 -0.692405086 0.4886830
    ## 5 as.factor(am)1  3.4853254 9.848018e+00  0.353911369 0.7234053
    ##             or
    ## 1 6.902753e+31
    ## 2 1.308474e-04
    ## 3 7.207548e-01
    ## 4 1.134479e-01
    ## 5 3.263304e+01

Survival Analysis
-----------------

``` r
library(survival)
head(bladder)
```

    ##   id rx number size stop event enum
    ## 1  1  1      1    3    1     0    1
    ## 2  1  1      1    3    1     0    2
    ## 3  1  1      1    3    1     0    3
    ## 4  1  1      1    3    1     0    4
    ## 5  2  1      2    1    4     0    1
    ## 6  2  1      2    1    4     0    2

``` r
s <- Surv(bladder$stop, bladder$event)
```

``` r
cox <- coxph(Surv(stop, event) ~ rx + number + size + enum, data = bladder)
summary(cox)
```

    ## Call:
    ## coxph(formula = Surv(stop, event) ~ rx + number + size + enum, 
    ##     data = bladder)
    ## 
    ##   n= 340, number of events= 112 
    ## 
    ##            coef exp(coef) se(coef)      z Pr(>|z|)    
    ## rx     -0.59739   0.55024  0.20088 -2.974  0.00294 ** 
    ## number  0.21754   1.24301  0.04653  4.675 2.93e-06 ***
    ## size   -0.05677   0.94481  0.07091 -0.801  0.42333    
    ## enum   -0.60385   0.54670  0.09401 -6.423 1.34e-10 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ##        exp(coef) exp(-coef) lower .95 upper .95
    ## rx        0.5502     1.8174    0.3712    0.8157
    ## number    1.2430     0.8045    1.1347    1.3617
    ## size      0.9448     1.0584    0.8222    1.0857
    ## enum      0.5467     1.8291    0.4547    0.6573
    ## 
    ## Concordance= 0.753  (se = 0.029 )
    ## Rsquare= 0.179   (max possible= 0.971 )
    ## Likelihood ratio test= 67.21  on 4 df,   p=8.804e-14
    ## Wald test            = 64.73  on 4 df,   p=2.932e-13
    ## Score (logrank) test = 69.42  on 4 df,   p=2.998e-14

``` r
plot(survfit(cox), xlab = 'days', ylab = 'Survival Rate', conf.int = TRUE)
```

![](README_files/figure-markdown_github/unnamed-chunk-30-1.png)

``` r
cox <- coxph(Surv(stop, event) ~ strata(rx) + number + size + enum, data = bladder)
summary(cox)
```

    ## Call:
    ## coxph(formula = Surv(stop, event) ~ strata(rx) + number + size + 
    ##     enum, data = bladder)
    ## 
    ##   n= 340, number of events= 112 
    ## 
    ##            coef exp(coef) se(coef)      z Pr(>|z|)    
    ## number  0.21371   1.23826  0.04648  4.598 4.27e-06 ***
    ## size   -0.05485   0.94662  0.07097 -0.773     0.44    
    ## enum   -0.60695   0.54501  0.09408 -6.451 1.11e-10 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ##        exp(coef) exp(-coef) lower .95 upper .95
    ## number    1.2383     0.8076    1.1304    1.3564
    ## size      0.9466     1.0564    0.8237    1.0879
    ## enum      0.5450     1.8348    0.4532    0.6554
    ## 
    ## Concordance= 0.74  (se = 0.04 )
    ## Rsquare= 0.166   (max possible= 0.954 )
    ## Likelihood ratio test= 61.84  on 3 df,   p=2.379e-13
    ## Wald test            = 60.04  on 3 df,   p=5.751e-13
    ## Score (logrank) test = 65.05  on 3 df,   p=4.896e-14

``` r
plot(survfit(cox), xlab = 'days', ylab = 'Survival Rate', conf.int = TRUE, col = 1:2)
legend("bottomleft", legend = c(1, 2), lty = 1, col = 1:2, text.col = 1:2, title = 'rx')
```

![](README_files/figure-markdown_github/unnamed-chunk-31-1.png)
