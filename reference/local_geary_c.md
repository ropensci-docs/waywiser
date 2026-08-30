# Local Geary's C statistic

Calculate the local Geary's C statistic for model residuals.
`ww_local_geary_c()` returns the statistic itself, while
`ww_local_geary_pvalue()` returns the associated p value. These
functions are meant to help assess model predictions, for instance by
identifying clusters of higher residuals than expected. For statistical
testing and inference applications, use
[`spdep::localC_perm()`](https://r-spatial.github.io/spdep/reference/localC.html)
instead.

## Usage

``` r
ww_local_geary_c(data, ...)

ww_local_geary_c_vec(truth, estimate, wt, na_rm = FALSE, ...)

ww_local_geary_pvalue(data, ...)

ww_local_geary_pvalue_vec(truth, estimate, wt = NULL, na_rm = FALSE, ...)
```

## Arguments

- data:

  A `data.frame` containing the columns specified by the `truth` and
  `estimate` arguments.

- ...:

  Additional arguments passed to
  [`spdep::localC()`](https://r-spatial.github.io/spdep/reference/localC.html)
  (for `ww_local_geary_c()`) or
  [`spdep::localC_perm()`](https://r-spatial.github.io/spdep/reference/localC.html)
  (for `ww_local_geary_pvalue()`).

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

Anselin, L. 1995. Local indicators of spatial association, Geographical
Analysis, 27, pp 93–115. doi: 10.1111/j.1538-4632.1995.tb00338.x.

Anselin, L. 2019. A Local Indicator of Multivariate Spatial Association:
Extending Geary's C. Geographical Analysis, 51, pp 133-150. doi:
10.1111/gean.12164

## See also

Other autocorrelation metrics:
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md)

Other yardstick metrics:
[`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md),
[`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)

## Examples

``` r
guerry_model <- guerry
guerry_lm <- lm(Crm_prs ~ Litercy, guerry_model)
guerry_model$predictions <- predict(guerry_lm, guerry_model)

ww_local_geary_c(guerry_model, Crm_prs, predictions)
#> # A tibble: 85 × 3
#>    .metric       .estimator .estimate
#>    <chr>         <chr>          <dbl>
#>  1 local_geary_c standard       0.981
#>  2 local_geary_c standard       0.836
#>  3 local_geary_c standard       0.707
#>  4 local_geary_c standard       0.108
#>  5 local_geary_c standard       0.264
#>  6 local_geary_c standard       1.36 
#>  7 local_geary_c standard       3.64 
#>  8 local_geary_c standard       1.57 
#>  9 local_geary_c standard       0.867
#> 10 local_geary_c standard       0.737
#> # ℹ 75 more rows
ww_local_geary_pvalue(guerry_model, Crm_prs, predictions)
#> # A tibble: 85 × 3
#>    .metric            .estimator .estimate
#>    <chr>              <chr>          <dbl>
#>  1 local_geary_pvalue standard       0.202
#>  2 local_geary_pvalue standard       0.217
#>  3 local_geary_pvalue standard       0.154
#>  4 local_geary_pvalue standard       0.106
#>  5 local_geary_pvalue standard       0.325
#>  6 local_geary_pvalue standard       0.144
#>  7 local_geary_pvalue standard       0.420
#>  8 local_geary_pvalue standard       0.209
#>  9 local_geary_pvalue standard       0.806
#> 10 local_geary_pvalue standard       0.484
#> # ℹ 75 more rows

wt <- ww_build_weights(guerry_model)

ww_local_geary_c_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#>  [1] 0.981119438 0.836402177 0.707464373 0.108332465 0.264075824 1.361485477
#>  [7] 3.641239412 1.571824022 0.867252524 0.737094462 0.573376555 0.001605731
#> [13] 1.891988440 1.152840284 1.029320931 0.297642850 1.219953394 1.934113868
#> [19] 1.632566652 0.441916658 5.202733790 0.921953310 3.084515822 0.237218594
#> [25] 1.346684045 1.051652204 0.419414691 0.217280214 0.794409207 0.243971372
#> [31] 0.376678958 0.139152907 0.711305633 3.096840680 1.974463944 0.922230710
#> [37] 1.032031759 0.339464386 0.933794842 1.910440700 0.937597672 0.625628647
#> [43] 0.376707677 2.692250283 1.288784962 0.798443065 1.671895951 1.310183326
#> [49] 2.347513577 0.845204889 0.302940809 2.291804447 0.881999216 0.412051312
#> [55] 2.006031605 0.561239582 0.375776092 1.853716391 1.191472387 1.146802970
#> [61] 1.857618679 0.149044974 0.614228825 0.755373475 1.287962784 1.447534518
#> [67] 1.236607966 0.962394651 0.338400653 1.914478855 0.641340157 2.146993342
#> [73] 0.703881855 1.417638272 0.692636715 1.765618175 0.246058853 0.700262130
#> [79] 0.002876896 0.057575267 0.420878038 2.025012395 2.525093274 1.053335832
#> [85] 1.030009749
ww_local_geary_pvalue_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#>  [1] 0.22353697 0.24781623 0.17448864 0.12902127 0.34233713 0.17806830
#>  [7] 0.42126839 0.20441515 0.77358915 0.41647803 0.02270210 0.14677863
#> [13] 0.31385768 0.89895233 0.50534270 0.19402980 0.76786488 0.54781191
#> [19] 0.07408913 0.18260594 0.50597346 0.78319076 0.66630216 0.12082692
#> [25] 0.87398906 0.98472054 0.23481835 0.09237508 0.62943425 0.10832625
#> [31] 0.21743737 0.14810649 0.47389135 0.61769049 0.06301975 0.83554885
#> [37] 0.37774586 0.28507552 0.84038045 0.89127347 0.65136602 0.55253414
#> [43] 0.16822728 0.23696852 0.79412396 0.04538616 0.25545316 0.19101792
#> [49] 0.15737708 0.17846773 0.05909223 0.74821923 0.43423506 0.22681302
#> [55] 0.93468453 0.18978463 0.21094258 0.64203913 0.23531367 0.95349735
#> [61] 0.30674078 0.27287224 0.33273024 0.18110350 0.50030358 0.39043232
#> [67] 0.70683850 0.76783525 0.02304469 0.07689631 0.52518926 0.17104498
#> [73] 0.36914356 0.51449997 0.54119903 0.08535662 0.10801452 0.30550119
#> [79] 0.15350952 0.06045808 0.27410805 0.53571792 0.06872913 0.95254756
#> [85] 0.99351460
```
