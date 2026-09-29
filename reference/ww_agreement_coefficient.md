# Agreement coefficients and related methods

These functions calculate the agreement coefficient and mean product
difference (MPD), as well as their systematic and unsystematic
components, from Ji and Gallo (2006). Agreement coefficients provides a
useful measurement of agreement between two data sets which is bounded,
symmetrical, and can be decomposed into systematic and unsystematic
components; however, it assumes a linear relationship between the two
data sets and treats both "truth" and "estimate" as being of equal
quality, and as such may not be a useful metric in all scenarios.

## Usage

``` r
ww_agreement_coefficient(data, ...)

# S3 method for class 'data.frame'
ww_agreement_coefficient(data, truth, estimate, na_rm = TRUE, ...)

ww_agreement_coefficient_vec(truth, estimate, na_rm = TRUE, ...)

ww_systematic_agreement_coefficient(data, ...)

# S3 method for class 'data.frame'
ww_systematic_agreement_coefficient(data, truth, estimate, na_rm = TRUE, ...)

ww_systematic_agreement_coefficient_vec(truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_agreement_coefficient(data, ...)

# S3 method for class 'data.frame'
ww_unsystematic_agreement_coefficient(data, truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_agreement_coefficient_vec(truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_mpd(data, ...)

# S3 method for class 'data.frame'
ww_unsystematic_mpd(data, truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_mpd_vec(truth, estimate, na_rm = TRUE, ...)

ww_systematic_mpd(data, ...)

# S3 method for class 'data.frame'
ww_systematic_mpd(data, truth, estimate, na_rm = TRUE, ...)

ww_systematic_mpd_vec(truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_rmpd(data, ...)

# S3 method for class 'data.frame'
ww_unsystematic_rmpd(data, truth, estimate, na_rm = TRUE, ...)

ww_unsystematic_rmpd_vec(truth, estimate, na_rm = TRUE, ...)

ww_systematic_rmpd(data, ...)

# S3 method for class 'data.frame'
ww_systematic_rmpd(data, truth, estimate, na_rm = TRUE, ...)

ww_systematic_rmpd_vec(truth, estimate, na_rm = TRUE, ...)
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

Agreement coefficient values range from 0 to 1, with 1 indicating
perfect agreement. `truth` and `estimate` must be the same length. This
function is not explicitly spatial and as such can be applied to data
with any number of dimensions and any coordinate reference system.

## References

Ji, L. and Gallo, K. 2006. "An Agreement Coefficient for Image
Comparison." Photogrammetric Engineering & Remote Sensing 72(7), pp
823–833, doi: 10.14358/PERS.72.7.823.

## See also

Other agreement metrics:
[`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)

Other yardstick metrics:
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_local_moran_i()`](https://docs.ropensci.org/waywiser/reference/local_moran_i.md),
[`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)

## Examples

``` r
# Calculated values match Ji and Gallo 2006:
x <- c(6, 8, 9, 10, 11, 14)
y <- c(2, 3, 5, 5, 6, 8)

ww_agreement_coefficient_vec(x, y)
#> [1] 0.4745867
ww_systematic_agreement_coefficient_vec(x, y)
#> [1] 0.4784812
ww_unsystematic_agreement_coefficient_vec(x, y)
#> [1] 0.9961054
ww_systematic_mpd_vec(x, y)
#> [1] 23.65667
ww_unsystematic_mpd_vec(x, y)
#> [1] 0.1766615
ww_systematic_rmpd_vec(x, y)
#> [1] 4.863812
ww_unsystematic_rmpd_vec(x, y)
#> [1] 0.4203112

example_df <- data.frame(x = x, y = y)
ww_agreement_coefficient(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric               .estimator .estimate
#>   <chr>                 <chr>          <dbl>
#> 1 agreement_coefficient standard       0.475
ww_systematic_agreement_coefficient(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric                          .estimator .estimate
#>   <chr>                            <chr>          <dbl>
#> 1 systematic_agreement_coefficient standard       0.478
ww_unsystematic_agreement_coefficient(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric                            .estimator .estimate
#>   <chr>                              <chr>          <dbl>
#> 1 unsystematic_agreement_coefficient standard       0.996
ww_systematic_mpd(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric        .estimator .estimate
#>   <chr>          <chr>          <dbl>
#> 1 systematic_mpd standard        23.7
ww_unsystematic_mpd(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric          .estimator .estimate
#>   <chr>            <chr>          <dbl>
#> 1 unsystematic_mpd standard       0.177
ww_systematic_rmpd(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric         .estimator .estimate
#>   <chr>           <chr>          <dbl>
#> 1 systematic_rmpd standard        4.86
ww_unsystematic_rmpd(example_df, x, y)
#> # A tibble: 1 × 3
#>   .metric           .estimator .estimate
#>   <chr>             <chr>          <dbl>
#> 1 unsystematic_rmpd standard       0.420
```
