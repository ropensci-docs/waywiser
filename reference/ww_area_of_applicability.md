# Find the area of applicability

This function calculates the "area of applicability" of a model, as
introduced by Meyer and Pebesma (2021). While the initial paper
introducing this method focused on spatial models, there is nothing
inherently spatial about the method; it can be used with any type of
data (and, because it does not care about the spatial arrangement of
your data, can be used with 2D or 3D spatial data, and with geographic
or projected CRS).

## Usage

``` r
ww_area_of_applicability(x, ...)

# S3 method for class 'data.frame'
ww_area_of_applicability(x, testing = NULL, importance, ..., na_rm = FALSE)

# S3 method for class 'matrix'
ww_area_of_applicability(x, testing = NULL, importance, ..., na_rm = FALSE)

# S3 method for class 'formula'
ww_area_of_applicability(
  x,
  data,
  testing = NULL,
  importance,
  ...,
  na_rm = FALSE
)

# S3 method for class 'recipe'
ww_area_of_applicability(
  x,
  data,
  testing = NULL,
  importance,
  ...,
  na_rm = FALSE
)

# S3 method for class 'rset'
ww_area_of_applicability(x, y = NULL, importance, ..., na_rm = FALSE)
```

## Arguments

- x:

  Either a data frame, matrix, formula (specifying predictor terms on
  the right-hand side), recipe (from
  [`recipes::recipe()`](https://recipes.tidymodels.org/reference/recipe.html),
  or `rset` object, produced by resampling functions from rsample or
  spatialsample.

  If `x` is a recipe, it should be the same one used to pre-process the
  data used in your model. If the recipe used to build the area of
  applicability doesn't match the one used to build the model, the
  returned area of applicability won't be correct.

- ...:

  Not currently used.

- testing:

  A data frame or matrix containing the data used to validate your
  model. This should be the same data as used to calculate all model
  accuracy metrics.

  If this argument is `NULL`, then this function will use the training
  data (from `x` or `data`) to calculate within-sample distances. This
  may result in the area of applicability threshold being set too high,
  with the result that too many points are classed as "inside" the area
  of applicability.

- importance:

  Either:

  - A data.frame with two columns: `term`, containing the names of each
    variable in the training and testing data, and `estimate`,
    containing the (raw or scaled) feature importance for each variable.

  - An object of class `vi` with at least two columns, `Variable` and
    `Importance`.

  All variables in the training data (`x` or `data`, depending on the
  context) must have a matching importance estimate, and all terms with
  importance estimates must be in the training data.

- na_rm:

  A logical of length 1, indicating whether observations (in both
  training and testing) with `NA` values in predictors should be
  removed. Only predictor variables are considered, and this value has
  no impact on predictions (where `NA` values produce `NA` predictions).
  If `na_rm = FALSE` and `NA` values are found, this function returns an
  error.

- data:

  The data frame representing your "training" data, when using the
  formula or recipe methods.

- y:

  Optional: a recipe (from
  [`recipes::recipe()`](https://recipes.tidymodels.org/reference/recipe.html))
  or formula.

  If `y` is a recipe, it should be the same one used to pre-process the
  data used in your model. If the recipe used to build the area of
  applicability doesn't match the one used to build the model, the
  returned area of applicability won't be correct.

## Value

A `ww_area_of_applicability` object, which can be used with
[`predict()`](https://rdrr.io/r/stats/predict.html) to calculate the
distance of new data to the original training data, and determine if new
data is within a model's area of applicability.

## Details

Predictions made on points "inside" the area of applicability should be
as accurate as predictions made on the data provided to `testing`. That
means that generally `testing` should be your final hold-out set so that
predictions on points inside the area of applicability are accurately
described by your reported model metrics. When passing an `rset` object
to `x`, predictions made on points "inside" the area of applicability
instead should be as accurate as predictions made on the assessment sets
during cross-validation.

This method assumes your model was fit using dummy variables in the
place of any non-numeric predictor, and that you have one importance
score per dummy variable. Having non-numeric predictors will cause this
function to fail.

## Differences from CAST

This implementation differs from Meyer and Pebesma (2021) (and therefore
from CAST) when using cross-validated data in order to minimize data
leakage. Namely, in order to calculate the dissimilarity index
\\DI\_{k}\\, CAST:

1.  Rescales all data used for cross validation at once, lumping
    assessment folds in with analysis data.

2.  Calculates a single \\\bar{d}\\ as the mean distance between all
    points in the rescaled data set, including between points in the
    same assessment fold.

3.  For each point \\k\\ that's used in an assessment fold, calculates
    \\d\_{k}\\ as the minimum distance between \\k\\ and any point in
    its corresponding analysis fold.

4.  Calculates \\DI\_{k}\\ by dividing \\d\_{k}\\ by \\\bar{d}\\ (which
    was partially calculated as the distance between \\k\\ and the rest
    of the rescaled data).

Because assessment data is used to calculate constants for rescaling
analysis data and \\\bar{d}\\, the assessment data may appear too
"similar" to the analysis data when calculating \\DI\_{k}\\. As such,
waywiser treats each fold in an `rset` independently:

1.  Each analysis set is rescaled independently.

2.  Separate \\\bar{d}\\ are calculated for each fold, as the mean
    distance between all points in the analysis set for that fold.

3.  Identically to CAST, \\d\_{k}\\ is the minimum distance between a
    point \\k\\ in the assessment fold and any point in the
    corresponding analysis fold.

4.  \\DI\_{k}\\ is then found by dividing \\d\_{k}\\ by \\\bar{d}\\,
    which was calculated independently from \\k\\.

Predictions are made using the full training data set, rescaled once (in
the same way as CAST), and the mean \\\bar{d}\\ across folds, under the
assumption that the "final" model in use will be retrained using the
entire data set.

In practice, this means waywiser produces very slightly higher
\\\bar{d}\\ values than CAST and a slightly higher area of applicability
threshold than CAST when using `rset` objects.

## References

H. Meyer and E. Pebesma. 2021. "Predicting into unknown space?
Estimating the area of applicability of spatial prediction models,"
Methods in Ecology and Evolution 12(9), pp 1620 - 1633, doi:
10.1111/2041-210X.13650.

## See also

Other area of applicability functions:
[`predict.ww_area_of_applicability()`](https://docs.ropensci.org/waywiser/reference/predict.ww_area_of_applicability.md)

## Examples

``` r
if (FALSE) { # rlang::is_installed("vip")
train <- vip::gen_friedman(1000, seed = 101) # ?vip::gen_friedman
test <- train[701:1000, ]
train <- train[1:700, ]
pp <- stats::ppr(y ~ ., data = train, nterms = 11)
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
  train = train
)

aoa <- ww_area_of_applicability(y ~ ., train, test, importance = importance)
predict(aoa, test)

# Equivalent methods for calculating AOA:
ww_area_of_applicability(train[2:11], test[2:11], importance)
ww_area_of_applicability(
  as.matrix(train[2:11]),
  as.matrix(test[2:11]),
  importance
)
}
```
