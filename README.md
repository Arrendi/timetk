
<!-- README.md is generated from README.Rmd. Please edit that file -->
sweep
=====

[![Travis-CI Build Status](https://travis-ci.org/business-science/sweep.svg?branch=master)](https://travis-ci.org/business-science/sweep.svg?branch=master) [![codecov](https://codecov.io/gh/business-science/sweep/branch/master/graph/badge.svg)](https://codecov.io/gh/business-science/sweep) [![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/sweep)](https://cran.r-project.org/package=sweep) ![](http://cranlogs.r-pkg.org/badges/sweep?color=brightgreen) ![](http://cranlogs.r-pkg.org/badges/grand-total/sweep?color=brightgreen)

> Simplified and extensible time series coercion tools

The `sweep` package combines a collection of coercion tools for time series analysis in the "tidyverse".

Benefits
--------

-   **Simplifies the coercion process between time-based tibbles (`tbl`) and the major time series data types `xts`, `zoo`, `zooreg`, and `ts`**
-   **Maximizes time-based data retention during coercion to regularized time series**

Tools
-----

The package contains the following elements:

1.  **coercion functions**: `sw_tbl`, `sw_ts`, `sw_xts`, `sw_zoo`, and `sw_zooreg` coerce time-based tibbles `tbl` to and from each of the main time-series data types `xts`, `zoo`, `zooreg`, `ts`, maintaining the time-based index.

2.  **index function**: `sw_index` returns the time series index of time series objects, models, and `forecast` objects. The argument `.sweep_idx` can be used to return a special sweep "index" attribute for regularized `ts` objects that returns a non-regularized date / date-time index if present.

Simplified time series coercion
-------------------------------

The coercion functions `sw_tbl`, `sw_xts`, `sw_zoo`, `sw_zooreg`, and `sw_ts` enable maximize data retention and simplify the coercion process. Further working with regularized time series (`ts`) class has been a particular pain until now.

The data process often starts with a time-based tibble:

``` r
# Time based tibble
data_tbl <- tibble(
    date = seq.Date(from = as.Date("2010-01-01"), by = 1, length.out = 5),
    x    = seq(100, 120, by = 5)
)
data_tbl
#> # A tibble: 5 × 2
#>         date     x
#>       <date> <dbl>
#> 1 2010-01-01   100
#> 2 2010-01-02   105
#> 3 2010-01-03   110
#> 4 2010-01-04   115
#> 5 2010-01-05   120
```

Coercion to `xts`, `zoo`, or `ts` is simplified. The data is ordered correctly automatically using the column containing the date or datetime information. Non-numeric columns are automatically dropped with a warning to the user (the `silent = TRUE` hides the warnings).

``` r
# xts
data_xts <- sw_xts(data_tbl, silent = TRUE)
```

``` r
# zoo
data_zoo <- sw_zoo(data_tbl, silent = TRUE)
```

``` r
# ts
data_ts <- sw_ts(data_tbl, start = 2010, freq = 365, silent = TRUE)
```

Maximum data retention
----------------------

-   **Problem**: The `ts()` function drops the time series data for a regularized index using the `start` and `freq` arguments.
-   **Solution**: The new `sw_ts()` stores the original date or datetime index as a second index ("sweep index"). The default regularized index is retrieved with `sw_index()`.

``` r
# Regularized numeric index
sw_index(data_ts) 
#> [1] 2010.000 2010.003 2010.005 2010.008 2010.011
```

The original date or datetime index is retrieved by setting `.sweep_idx = TRUE`.

``` r
# Secondary "sweep index": date or date-time index now retrievable
sw_index(data_ts, .sweep_idx = TRUE)
#> [1] "2010-01-01" "2010-01-02" "2010-01-03" "2010-01-04" "2010-01-05"
```

Using this approach of storing the secondary "sweep index", we can now go from `ts` back to `tbl`.

``` r
data_ts %>%
    sw_tbl(.sweep_idx = TRUE)
#> # A tibble: 5 × 2
#>        index     x
#>       <date> <dbl>
#> 1 2010-01-01   100
#> 2 2010-01-02   105
#> 3 2010-01-03   110
#> 4 2010-01-04   115
#> 5 2010-01-05   120
```

Installation
------------

Here's how to install to get started.

*Development version with latest features*:

``` r
# install.packages("devtools")
devtools::install_github("business-science/sweep")
```

<!-- _CRAN approved version_: -->
<!-- ```{r, eval = FALSE} -->
<!-- install.packages("sweep") -->
<!-- ``` -->
Further Information
-------------------

The `sweep` package includes a vignette to help users get up to speed quickly:

-   SW00 - Time Series Coercion Using `sweep`

<!-- See the [`tidyquant` vignettes](https://cran.r-project.org/package=tidyquant) for further details on the package. -->
