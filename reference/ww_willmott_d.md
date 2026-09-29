# Willmott's d and related values

These functions calculate Willmott's d value, a proposed replacement for
R2 which better differentiates between types and magnitudes of possible
covariations. Additional functions calculate systematic and unsystematic
components of MSE and RMSE; the sum of the systematic and unsystematic
components of MSE equal total MSE (though the same is not true for
RMSE).

## Usage

``` r
ww_willmott_d(data, ...)

# S3 method for class 'data.frame'
ww_willmott_d(data, truth, estimate, na_rm = TRUE, ...)

ww_willmott_d_vec(truth, estimate, na_rm = TRUE, ...)

ww_willmott_d1(data, ...)

# S3 method for class 'data.frame'
ww_willmott_d1(data, truth, estimate, na_rm = TRUE, ...)

ww_willmott_d1_vec(truth, estimate, na_rm = TRUE, ...)

ww_willmott_dr(data, ...)

# S3 method for class 'data.frame'
ww_willmott_dr(data, truth, estimate, na_rm = TRUE, ...)

ww_willmott_dr_vec(truth, estimate, na_rm = TRUE, ...)

ww_systematic_mse(data, ...)

# S3 method for class 'data.frame'
ww_systematic_mse(data, truth, estimate, na_rm = TRUE, ...)

ww_systematic_mse_vec(truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_mse(data, ...)

# S3 method for class 'data.frame'
ww_unsystematic_mse(data, truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_mse_vec(truth, estimate, na_rm = TRUE, ...)

ww_systematic_rmse(data, ...)

# S3 method for class 'data.frame'
ww_systematic_rmse(data, truth, estimate, na_rm = TRUE, ...)

ww_systematic_rmse_vec(truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_rmse(data, ...)

# S3 method for class 'data.frame'
ww_unsystematic_rmse(data, truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_rmse_vec(truth, estimate, na_rm = TRUE, ...)
```

## Arguments

- data:

  A `data.frame` containing the columns specified by the `truth` and
  `estimate` arguments.

- ...:

  Not currently used.

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

- na_rm:

  A `logical` value indicating whether `NA` values should be stripped
  before the computation proceeds.

## Value

A tibble with columns .metric, .estimator, and .estimate and 1 row of
values. For grouped data frames, the number of rows returned will be the
same as the number of groups. For `_vec()` functions, a single value (or
NA).

## Details

Values of d and d1 range from 0 to 1, with 1 indicating perfect
agreement. Values of dr range from -1 to 1, with 1 similarly indicating
perfect agreement. Values of RMSE are in the same units as `truth` and
`estimate`, while values of MSE are in squared units. `truth` and
`estimate` must be the same length. This function is not explicitly
spatial and as such can be applied to data with any number of dimensions
and any coordinate reference system.

## References

Willmott, C. J. 1981. "On the Validation of Models". Physical Geography
2(2), pp 184-194, doi: 10.1080/02723646.1981.10642213.

Willmott, C. J. 1982. "Some Comments on the Evaluation of Model
Performance". Bulletin of the American Meteorological Society 63(11), pp
1309-1313, doi: 10.1175/1520-0477(1982)063\<1309:SCOTEO\>2.0.CO;2.

Willmott C. J., Ackleson S. G., Davis R. E., Feddema J. J., Klink K. M.,
Legates D. R., O’Donnell J., Rowe C. M. 1985. "Statistics for the
evaluation of model performance." Journal of Geophysical Research
90(C5): 8995–9005, doi: 10.1029/jc090ic05p08995

Willmott, C. J., Robeson, S. M., and Matsuura, K. "A refined index of
model performance". International Journal of Climatology 32, pp
2088-2094, doi: 10.1002/joc.2419.

## See also

Other agreement metrics:
[`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md)

Other yardstick metrics:
[`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md)

## Examples

``` r
x <- c(6, 8, 9, 10, 11, 14)
y <- c(2, 3, 5, 5, 6, 8)

ww_willmott_d_vec(x, y)
#> [1] 0.5421558
ww_willmott_d1_vec(x, y)
#> [1] 0.2926829
ww_willmott_dr_vec(x, y)
#> [1] -0.1724138
ww_systematic_mse_vec(x, y)
#> [1] 0.2238443
ww_unsystematic_mse_vec(x, y)
#> [1] 23.60949
ww_systematic_rmse_vec(x, y)
#> [1] 0.4731218
ww_unsystematic_rmse_vec(x, y)
#> [1] 4.85896

example_df <- data.frame(x = x, y = y)
ww_willmott_d(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric    .estimator .estimate
#>   <chr>      <chr>          <dbl>
#> 1 willmott_d standard       0.542
ww_willmott_d1(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric     .estimator .estimate
#>   <chr>       <chr>          <dbl>
#> 1 willmott_d1 standard       0.293
ww_willmott_dr(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric     .estimator .estimate
#>   <chr>       <chr>          <dbl>
#> 1 willmott_dr standard      -0.172
ww_systematic_mse(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric        .estimator .estimate
#>   <chr>          <chr>          <dbl>
#> 1 systematic_mse standard       0.224
ww_unsystematic_mse(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric          .estimator .estimate
#>   <chr>            <chr>          <dbl>
#> 1 unsystematic_mse standard        23.6
ww_systematic_rmse(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric         .estimator .estimate
#>   <chr>           <chr>          <dbl>
#> 1 systematic_rmse standard       0.473
ww_unsystematic_rmse(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric           .estimator .estimate
#>   <chr>             <chr>          <dbl>
#> 1 unsystematic_rmse standard        4.86
```
