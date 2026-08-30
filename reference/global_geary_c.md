# Global Geary's C statistic

Calculate the global Geary's C statistic for model residuals.
`ww_global_geary_c()` returns the statistic itself, while
`ww_global_geary_pvalue()` returns the associated p value. These
functions are meant to help assess model predictions, for instance by
identifying if there are clusters of higher residuals than expected. For
statistical testing and inference applications, use
[`spdep::geary.test()`](https://r-spatial.github.io/spdep/reference/geary.test.html)
instead.

## Usage

``` r
ww_global_geary_c(data, ...)

ww_global_geary_c_vec(truth, estimate, wt, na_rm = FALSE, ...)

ww_global_geary_pvalue(data, ...)

ww_global_geary_pvalue_vec(truth, estimate, wt = NULL, na_rm = FALSE, ...)
```

## Arguments

- data:

  A `data.frame` containing the columns specified by the `truth` and
  `estimate` arguments.

- ...:

  Additional arguments passed to
  [`spdep::geary()`](https://r-spatial.github.io/spdep/reference/geary.html)
  (for `ww_global_geary_c()`) or
  [`spdep::geary.test()`](https://r-spatial.github.io/spdep/reference/geary.test.html)
  (for `ww_global_geary_pvalue()`).

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

A tibble with columns .metric, .estimator, and .estimate and 1 row of
values. For grouped data frames, the number of rows returned will be the
same as the number of groups. For `_vec()` functions, a single value (or
NA).

## Details

These functions can be used for geographic or projected coordinate
reference systems and expect 2D data.

## References

Geary, R. C. (1954). "The Contiguity Ratio and Statistical Mapping". The
Incorporated Statistician. 5 (3): 115–145. doi:10.2307/2986645.

Cliff, A. D., Ord, J. K. 1981 Spatial processes, Pion, p. 17.

## See also

Other autocorrelation metrics:
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md)

Other yardstick metrics:
[`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md),
[`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)

## Examples

``` r
guerry_model <- guerry
guerry_lm <- lm(Crm_prs ~ Litercy, guerry_model)
guerry_model$predictions <- predict(guerry_lm, guerry_model)

ww_global_geary_c(guerry_model, Crm_prs, predictions)
#> # A tibble: 1 × 3
#>   .metric        .estimator .estimate
#>   <chr>          <chr>          <dbl>
#> 1 global_geary_c standard       0.565
ww_global_geary_pvalue(guerry_model, Crm_prs, predictions)
#> # A tibble: 1 × 3
#>   .metric             .estimator .estimate
#>   <chr>               <chr>          <dbl>
#> 1 global_geary_pvalue standard    7.55e-10

wt <- ww_build_weights(guerry_model)

ww_global_geary_c_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#> [1] 0.5654044
ww_global_geary_pvalue_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#> [1] 7.548865e-10
```
