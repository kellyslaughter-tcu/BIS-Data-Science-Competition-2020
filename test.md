External Data
================

External Data
=============

The theme of this notebook is acquiring 3rd party data. 3rd party data organizations are interested in include ecomomic data (often collected through government efforts), social media, web site scraping, and demographic (often provided for a fee through marketing organizations).

Packages
--------

Some R packages The usual mechanism for obtaining 3rd party data is

**httr** <https://cran.r-project.org/web/packages/httr/>

**jsonlite** <https://cran.r-project.org/web/packages/jsonlite>

External Data Sources
---------------------

Financial
=========

BEA
===

Twitter
=======

### Financial

<hr>
``` r
library(quantmod)      # retrieve financial data (used for function via getSymbols) (masks as.Date from base R) 
```

    ## Loading required package: xts

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ## Loading required package: TTR

    ## Registered S3 method overwritten by 'quantmod':
    ##   method            from
    ##   as.zoo.data.frame zoo

    ## Version 0.4-0 included new data defaults. See ?getSymbols.

``` r
library(broom)         # allows manipulation of data for prep for ggplot2 (used for function tidy)
library(tidyr)         # allows manipulation of data for prep for ggplot2 (used for function spread)
library(dplyr)         # allows manipulation of data for prep for ggplot2 (used for function filter)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:xts':
    ## 
    ##     first, last

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
stock_data <- getSymbols(c("^DJI","^GSPC"),src='yahoo', from = "2019-09-01", to = "2020-04-01") # DJI = Dow Jones Industrial Index, GSPC = Standard and Poor's 500 Index
```

    ## 'getSymbols' currently uses auto.assign=TRUE by default, but will
    ## use auto.assign=FALSE in 0.5-0. You will still be able to use
    ## 'loadSymbols' to automatically load data. getOption("getSymbols.env")
    ## and getOption("getSymbols.auto.assign") will still be checked for
    ## alternate defaults.
    ## 
    ## This message is shown once per session and may be disabled by setting 
    ## options("getSymbols.warning4.0"=FALSE). See ?getSymbols for details.

``` r
head(stock_data)
```

    ## [1] "^DJI"  "^GSPC"

``` r
head(DJI)
```

    ##            DJI.Open DJI.High  DJI.Low DJI.Close DJI.Volume DJI.Adjusted
    ## 2019-09-03 26198.26 26198.26 25978.22  26118.02  223210000     26118.02
    ## 2019-09-04 26301.99 26362.35 26244.44  26355.47  202710000     26355.47
    ## 2019-09-05 26603.15 26836.30 26603.15  26728.15  256670000     26728.15
    ## 2019-09-06 26790.25 26860.87 26708.39  26797.46  209700000     26797.46
    ## 2019-09-09 26866.23 26900.83 26762.18  26835.51  273120000     26835.51
    ## 2019-09-10 26805.83 26909.43 26717.05  26909.43  322340000     26909.43

``` r
dow_jones2 <- tidy(DJI) # tidy from broom package converts list to "long" data frame for DJI 
head(dow_jones2)
```

    ## # A tibble: 6 x 3
    ##   index      series    value
    ##   <date>     <chr>     <dbl>
    ## 1 2019-09-03 DJI.Open 26198.
    ## 2 2019-09-04 DJI.Open 26302.
    ## 3 2019-09-05 DJI.Open 26603.
    ## 4 2019-09-06 DJI.Open 26790.
    ## 5 2019-09-09 DJI.Open 26866.
    ## 6 2019-09-10 DJI.Open 26806.

``` r
dow_jones <- spread(dow_jones2, series, value) # spread from tidyr; converts long to wide using series values for new columns; that is, turn series row values to new columns
head(dow_jones)
```

    ## # A tibble: 6 x 7
    ##   index      DJI.Adjusted DJI.Close DJI.High DJI.Low DJI.Open DJI.Volume
    ##   <date>            <dbl>     <dbl>    <dbl>   <dbl>    <dbl>      <dbl>
    ## 1 2019-09-03       26118.    26118.   26198.  25978.   26198.  223210000
    ## 2 2019-09-04       26355.    26355.   26362.  26244.   26302.  202710000
    ## 3 2019-09-05       26728.    26728.   26836.  26603.   26603.  256670000
    ## 4 2019-09-06       26797.    26797.   26861.  26708.   26790.  209700000
    ## 5 2019-09-09       26836.    26836.   26901.  26762.   26866.  273120000
    ## 6 2019-09-10       26909.    26909.   26909.  26717.   26806.  322340000

``` r
dim(dow_jones) # spread function broke rows into new columns, so fewer rows and more columns
```

    ## [1] 146   7
