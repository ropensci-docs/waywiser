# Global Moran's I statistic

Calculate the global Moran's I statistic for model residuals.
`ww_global_moran_i()` returns the statistic itself, while
`ww_global_moran_pvalue()` returns the associated p value. These
functions are meant to help assess model predictions, for instance by
identifying if there are clusters of higher residuals than expected. For
statistical testing and inference applications, use
[`spdep::moran.test()`](https://r-spatial.github.io/spdep/reference/moran.test.html)
instead.

## Usage

``` r
ww_global_moran_i(data, ...)

ww_global_moran_i_vec(truth, estimate, wt = NULL, na_rm = FALSE, ...)

ww_global_moran_pvalue(data, ...)

ww_global_moran_pvalue_vec(truth, estimate, wt = NULL, na_rm = FALSE, ...)
```

## Arguments

- data:

  A `data.frame` containing the columns specified by the `truth` and
  `estimate` arguments.

- ...:

  Additional arguments passed to
  [`spdep::moran()`](https://r-spatial.github.io/spdep/reference/moran.html)
  (for `ww_global_moran_i()`) or
  [`spdep::moran.test()`](https://r-spatial.github.io/spdep/reference/moran.test.html)
  (for `ww_global_moran_pvalue()`).

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

Moran, P.A.P. (1950). "Notes on Continuous Stochastic Phenomena."
Biometrika, 37(1/2), pp 17. doi: 10.2307/2332142

Cliff, A. D., Ord, J. K. 1981 Spatial processes, Pion, p. 17.

## See also

Other autocorrelation metrics:
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md)

Other yardstick metrics:
[`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md),
[`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)

## Examples

``` r
guerry_model <- guerry
guerry_lm <- lm(Crm_prs ~ Litercy, guerry_model)
guerry_model$predictions <- predict(guerry_lm, guerry_model)

ww_global_moran_i(guerry_model, Crm_prs, predictions)
#> # A tibble: 1 × 3
#>   .metric        .estimator .estimate
#>   <chr>          <chr>          <dbl>
#> 1 global_moran_i standard       0.412
ww_global_moran_pvalue(guerry_model, Crm_prs, predictions)
#> # A tibble: 1 × 3
#>   .metric             .estimator .estimate
#>   <chr>               <chr>          <dbl>
#> 1 global_moran_pvalue standard    7.23e-10

wt <- ww_build_weights(guerry_model)

ww_global_moran_i_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#> [1] 0.4115652
ww_global_moran_pvalue_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#> [1] 7.234758e-10
```
