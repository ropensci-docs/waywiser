# Print number of predictors and area-of-applicability threshold

Print number of predictors and area-of-applicability threshold

## Usage

``` r
# S3 method for class 'ww_area_of_applicability'
print(x, digits = getOption("digits"), ...)
```

## Arguments

- x:

  A `ww_area_of_applicability` object.

- digits:

  The number of digits to print, used when rounding the AOA threshold.

- ...:

  These dots are for future extensions and must be empty.

## Examples

``` r
if (FALSE) { # rlang::is_installed("vip")
library(vip)
trn <- gen_friedman(500, seed = 101) # ?vip::gen_friedman
pp <- ppr(y ~ ., data = trn, nterms = 11)
metric_name <- ifelse(
  packageVersion("vip") > package_version("0.3.2"),
  "rsq",
  "rsquared"
)

importance <- vip::vi_permute(
  pp,
  target = "y",
  metric = metric_name,
  pred_wrapper = predict,
  train = trn
)


ww_area_of_applicability(trn[2:11], importance = importance)
}
```
