# Local Moran's I statistic

Calculate the local Moran's I statistic for model residuals.
`ww_local_moran_i()` returns the statistic itself, while
`ww_local_moran_pvalue()` returns the associated p value. These
functions are meant to help assess model predictions, for instance by
identifying clusters of higher residuals than expected. For statistical
testing and inference applications, use
[`spdep::localmoran_perm()`](https://r-spatial.github.io/spdep/reference/localmoran.html)
instead.

## Usage

``` r
ww_local_moran_i(data, ...)

ww_local_moran_i_vec(truth, estimate, wt, na_rm = FALSE, ...)

ww_local_moran_pvalue(data, ...)

ww_local_moran_pvalue_vec(truth, estimate, wt = NULL, na_rm = FALSE, ...)
```

## Arguments

- data:

  A `data.frame` containing the columns specified by the `truth` and
  `estimate` arguments.

- ...:

  Additional arguments passed to
  [`spdep::localmoran()`](https://r-spatial.github.io/spdep/reference/localmoran.html).

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

Sokal, R. R, Oden, N. L. and Thomson, B. A. 1998. Local Spatial
Autocorrelation in a Biological Model. Geographical Analysis, 30, pp
331–354. doi: 10.1111/j.1538-4632.1998.tb00406.x

## See also

Other autocorrelation metrics:
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md)

Other yardstick metrics:
[`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
[`ww_global_geary_c()`](https://docs.ropensci.org/waywiser/reference/global_geary_c.md),
[`ww_global_moran_i()`](https://docs.ropensci.org/waywiser/reference/global_moran_i.md),
[`ww_local_geary_c()`](https://docs.ropensci.org/waywiser/reference/local_geary_c.md),
[`ww_local_getis_ord_g()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md),
[`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)

## Examples

``` r
guerry_model <- guerry
guerry_lm <- lm(Crm_prs ~ Litercy, guerry_model)
guerry_model$predictions <- predict(guerry_lm, guerry_model)

ww_local_moran_i(guerry_model, Crm_prs, predictions)
#> # A tibble: 85 × 3
#>    .metric       .estimator .estimate
#>    <chr>         <chr>          <dbl>
#>  1 local_moran_i standard      0.530 
#>  2 local_moran_i standard      0.858 
#>  3 local_moran_i standard      0.759 
#>  4 local_moran_i standard      0.732 
#>  5 local_moran_i standard      0.207 
#>  6 local_moran_i standard      0.860 
#>  7 local_moran_i standard      0.692 
#>  8 local_moran_i standard      1.69  
#>  9 local_moran_i standard     -0.0109
#> 10 local_moran_i standard      0.710 
#> # ℹ 75 more rows
ww_local_moran_pvalue(guerry_model, Crm_prs, predictions)
#> # A tibble: 85 × 3
#>    .metric            .estimator .estimate
#>    <chr>              <chr>          <dbl>
#>  1 local_moran_pvalue standard     0.361  
#>  2 local_moran_pvalue standard     0.0127 
#>  3 local_moran_pvalue standard     0.0318 
#>  4 local_moran_pvalue standard     0.115  
#>  5 local_moran_pvalue standard     0.234  
#>  6 local_moran_pvalue standard     0.0935 
#>  7 local_moran_pvalue standard     0.531  
#>  8 local_moran_pvalue standard     0.109  
#>  9 local_moran_pvalue standard     0.335  
#> 10 local_moran_pvalue standard     0.00663
#> # ℹ 75 more rows

wt <- ww_build_weights(guerry_model)

ww_local_moran_i_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#>  [1]  0.529586027  0.857962397  0.759397482  0.731821184  0.207216255
#>  [6]  0.859824645  0.692480894  1.685682868 -0.010937577  0.709971045
#> [11]  1.756476080  0.839390997 -0.208812822  0.311287253 -0.195850256
#> [16] -0.046485425  0.219659575  0.072248473  0.911260991  0.796818074
#> [21]  0.472218810 -0.047995949 -0.701165391  0.682001844 -0.114131742
#> [26]  0.043283334  1.067791069  1.186850176  0.174554949  0.071132504
#> [31]  0.014932487  1.014614517  0.258635858  0.385988835 -0.113213840
#> [36]  0.016531123  0.601974328 -0.029051514  0.101963855 -0.098393898
#> [41]  0.305211136 -0.057462330 -0.015702560  0.882089292 -0.163892577
#> [46]  1.649695545  0.377330987  0.868476489 -0.465975751  0.303084203
#> [51]  1.404344537 -0.370062874  0.440556284  0.289554503  0.035787495
#> [56]  0.393521099  1.006384006  0.222959827  0.730981130  0.628215009
#> [61] -0.183012992  0.227295946  0.284153229  2.316505472  0.494418600
#> [66]  0.982994320 -0.124397352  0.160297076  1.039537767  1.231583113
#> [71]  0.271055716 -0.168894660 -0.038283576  0.017831736 -0.052920056
#> [76]  1.205308932  0.808428811  0.551329387  0.878044848  0.901458850
#> [81]  0.022009901 -0.327876773 -0.318368758 -0.003280457 -0.124796245
ww_local_moran_pvalue_vec(
  guerry_model$Crm_prs,
  guerry_model$predictions,
  wt = wt
)
#>  [1] 0.361304795 0.012664975 0.031799252 0.115230513 0.234090293 0.093535973
#>  [7] 0.530618631 0.109289803 0.335060524 0.006632515 0.002278842 0.100115333
#> [13] 0.247742772 0.003712388 0.526236804 0.541825841 0.021511546 0.773348351
#> [19] 0.125986145 0.219825946 0.549732292 0.513555378 0.381564858 0.078983302
#> [25] 0.695793884 0.602944660 0.244326204 0.001337467 0.021685082 0.326512972
#> [31] 0.946696741 0.032650704 0.021272223 0.525113591 0.045656211 0.807490784
#> [37] 0.121000471 0.863193762 0.128354731 0.818093330 0.195316218 0.278814034
#> [43] 0.896063138 0.223229844 0.314659364 0.021844009 0.371216056 0.230792356
#> [49] 0.042199799 0.382128483 0.003916122 0.446093710 0.127376322 0.171424332
#> [55] 0.947231579 0.141533773 0.058387258 0.599378815 0.103085890 0.044587278
#> [61] 0.251120319 0.359430944 0.619407669 0.063573829 0.314640714 0.389339642
#> [67] 0.102639026 0.293931872 0.011789349 0.086786681 0.617302569 0.175776744
#> [73] 0.738859695 0.940225426 0.575078121 0.123277217 0.055146249 0.054102586
#> [79] 0.104150628 0.009218471 0.672565343 0.245960451 0.143659118 0.951709014
#> [85] 0.277252686
```
