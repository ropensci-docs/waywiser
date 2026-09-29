# Local Getis-Ord G and G\* statistic

Calculate the local Getis-Ord G and G\* statistic for model residuals.
`ww_local_getis_ord_g()` returns the statistic itself, while
`ww_local_getis_ord_pvalue()` returns the associated p value. These
functions are meant to help assess model predictions, for instance by
identifying clusters of higher residuals than expected. For statistical
testing and inference applications, use
[`spdep::localG_perm()`](https://r-spatial.github.io/spdep/reference/localG.html)
instead.

## Usage

``` r
ww_local_getis_ord_g(data, ...)

ww_local_getis_ord_g_vec(truth, estimate, wt, na_rm = FALSE, ...)

ww_local_getis_ord_g_pvalue(data, ...)

ww_local_getis_ord_g_pvalue_vec(truth, estimate, wt, na_rm = FALSE, ...)
```

## Arguments

- data:

  A `data.frame` containing the columns specified by the `truth` and
  `estimate` arguments.

- ...:

  Additional arguments passed to
  [`spdep::localG()`](https://r-spatial.github.io/spdep/reference/localG.html)
  (for `ww_local_getis_ord_g()`) or
  [`spdep::localG_perm()`](https://r-spatial.github.io/spdep/reference/localG.html)
  (for `ww_local_getis_ord_pvalue()`).

- truth:

  The column identifier for the true results (that is `numeric`). This
  should be an unquoted column name although this argument is passed by
  expression and supports
  [quasiquotation](https://rlang.r-lib.org/reference/topic-inject.html)
  (you can unquote column names). For `_vec()` functions, a `numeric`
  vector.

- estimate:

  The column identifier for the predicted results (that is also
  `numeric`). As with `truth` this can be specified different ways but
  the primary method is to use an unquoted variable name. For `_vec()`
  functions, a `numeric` vector.

- wt:

  A `listw` object, for instance as created with
  [`ww_build_weights()`](https://docs.ropensci.org/waywiser/reference/ww_build_weights.md).
  For data.frame input, may also be a function that takes `data` and
  returns a `listw` object.

- na_rm:

  A `logical` value indicating whether `NA` values should be stripped
  before the computation proceeds.

## Value

A tibble with columns .metric, .estimator, and .estimate and
`nrow(data)` rows of values. For `_vec()` functions, a numeric vector of
`length(truth)` (or NA).

## Details

These functions can be used for geographic or projected coordinate
reference systems and expect 2D data.

## References

Ord, J. K. and Getis, A. 1995. Local spatial autocorrelation statistics:
distributional issues and an application. Geographical Analysis, 27,
286–306. doi: 10.1111/j.1538-4632.1995.tb00912.x

## See also

Other autocorrelation metrics:
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md)

Other yardstick metrics:
[`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md),
[`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)

## Examples

``` r
guerry_model <- guerry
guerry_lm <- lm(Crm_prs ~ Litercy, guerry_model)
guerry_model$predictions <- predict(guerry_lm, guerry_model)

ww_local_getis_ord_g(guerry_model, Crm_prs, predictions)
#> # A tibble: 85 × 3
#>    .metric           .estimator .estimate
#>    <chr>             <chr>          <dbl>
#>  1 local_getis_ord_g standard       0.913
#>  2 local_getis_ord_g standard       2.49 
#>  3 local_getis_ord_g standard       2.15 
#>  4 local_getis_ord_g standard      -1.58 
#>  5 local_getis_ord_g standard      -1.19 
#>  6 local_getis_ord_g standard      -1.68 
#>  7 local_getis_ord_g standard       0.627
#>  8 local_getis_ord_g standard      -1.60 
#>  9 local_getis_ord_g standard       0.964
#> 10 local_getis_ord_g standard      -2.71 
#> # ℹ 75 more rows
ww_local_getis_ord_g_pvalue(guerry_model, Crm_prs, predictions)
#> # A tibble: 85 × 3
#>    .metric                  .estimator .estimate
#>    <chr>                    <chr>          <dbl>
#>  1 local_getis_ord_g_pvalue standard     0.346  
#>  2 local_getis_ord_g_pvalue standard     0.0202 
#>  3 local_getis_ord_g_pvalue standard     0.0298 
#>  4 local_getis_ord_g_pvalue standard     0.124  
#>  5 local_getis_ord_g_pvalue standard     0.225  
#>  6 local_getis_ord_g_pvalue standard     0.108  
#>  7 local_getis_ord_g_pvalue standard     0.499  
#>  8 local_getis_ord_g_pvalue standard     0.110  
#>  9 local_getis_ord_g_pvalue standard     0.347  
#> 10 local_getis_ord_g_pvalue standard     0.00679
#> # ℹ 75 more rows

wt <- ww_build_weights(guerry_model)

ww_local_getis_ord_g_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#>  [1]  0.91288164  2.49305353  2.14692501 -1.57511235 -1.18988831 -1.67703329
#>  [7]  0.62706195 -1.60139345  0.96397088 -2.71475331 -3.05125861 -1.64429475
#> [13]  1.15584938 -2.90161984 -0.63376101  0.61005430  2.29888368 -0.28799789
#> [19]  1.53012357  1.22699107  0.59816133 -0.65331166  0.87501663 -1.75661589
#> [25]  0.39100454  0.52017062  1.16424138 -3.20781683 -2.29583912 -0.98116177
#> [31]  0.06685550 -2.13635236  2.30311768  0.63548278  1.99855789 -0.24366435
#> [37]  1.55058791  0.17231008  1.52062197 -0.22999799 -1.29501163  1.08298736
#> [43]  0.13063616 -1.21798455 -1.00549332 -2.29306942  0.89419782  1.19832028
#> [49]  2.03154442  0.87398122  2.88484032 -0.76194353  1.52453019  1.36764146
#> [55] -0.06618369  1.47010332  1.89277905  0.52529402  1.63007385  2.00852739
#> [61]  1.14763247 -0.91644996 -0.49669001 -1.85515687 -1.00553207 -0.86081555
#> [67]  1.63219209  1.04953518  2.51838763  1.71259713 -0.49967695  1.35387348
#> [73] -0.33336379 -0.07498653  0.56058846  1.54116260 -1.91772217 -1.92601432
#> [79] -1.62505600 -2.60384397  0.42262985  1.16021704  1.46229964  0.06056077
#> [85]  1.08651166
ww_local_getis_ord_g_pvalue_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#>  [1] 0.386348562 0.014165697 0.024956460 0.095347274 0.244827223 0.142401593
#>  [7] 0.539545315 0.137543363 0.345146479 0.007091888 0.004973310 0.130555393
#> [13] 0.251115332 0.004244969 0.521372829 0.567497715 0.038020926 0.737776246
#> [19] 0.155949759 0.214555190 0.550193648 0.523965336 0.406875797 0.100868705
#> [25] 0.745205720 0.609060914 0.261126293 0.001296241 0.020368086 0.333336293
#> [31] 0.950638661 0.039000689 0.022876114 0.508002314 0.050354241 0.794188583
#> [37] 0.135114286 0.912629881 0.144575379 0.873652607 0.199446809 0.306409718
#> [43] 0.881932161 0.246666270 0.280980456 0.013640443 0.406739309 0.255184883
#> [49] 0.037537679 0.403963486 0.005383103 0.485156551 0.123603626 0.187753384
#> [55] 0.908397387 0.182580083 0.070613008 0.642082531 0.118208024 0.042980518
#> [61] 0.286749863 0.348808494 0.621948153 0.065456097 0.312638805 0.352676007
#> [67] 0.116543559 0.286809461 0.016126206 0.093056153 0.570700962 0.181499600
#> [73] 0.819950408 0.913966907 0.613409414 0.124614037 0.081675231 0.058054038
#> [79] 0.073040692 0.009804751 0.671014705 0.231443262 0.162204242 0.905659078
#> [85] 0.336748038
```
