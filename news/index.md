# Changelog

## waywiser 0.6.3

CRAN release: 2025-04-15

- Avoid downloading data over the internet on CRAN.

## waywiser 0.6.2

CRAN release: 2025-03-03

- Set
  [`sf_proj_search_paths()`](https://r-spatial.github.io/sf/reference/proj_tools.html)
  in vignettes to not write to directories on CRAN.

## waywiser 0.6.1

CRAN release: 2025-02-17

- Avoid using `\()` in order to keep R version needed below 4.1

- Update tests for new versions of dependencies

## waywiser 0.6.0

CRAN release: 2024-06-27

- Metric functions now return `NA` in all cases where they previously
  returned `NaN`. This improves cross-platform consistency; in
  particular, MacOS often returned `NA` when every other platform would
  return `NaN`. ([\#63](https://github.com/ropensci/waywiser/issues/63))

- [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
  now handles classification and class probability metrics better when
  called with raster arguments (either to `data` or to `truth` and
  `estimate`):

  - When called with classification metrics,
    [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
    will now convert `truth` and `estimate` to factors before passing
    them to the metric set. Thanks to
    [@nowosad](https://github.com/nowosad) for the report in
    [\#60](https://github.com/ropensci/waywiser/issues/60)
    ([\#61](https://github.com/ropensci/waywiser/issues/61)).
  - When called with class probability metrics,
    [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
    will convert `truth` to a factor and will pass `estimate` as an
    unnamed argument.
    ([\#62](https://github.com/ropensci/waywiser/issues/62))
  - When called with a mix of class and probability metrics,
    [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
    will error. ([\#62](https://github.com/ropensci/waywiser/issues/62))

## waywiser 0.5.1

CRAN release: 2023-10-31

- [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
  now warns if you provide `crs` as an argument to
  [`sf::st_make_grid()`](https://r-spatial.github.io/sf/reference/st_make_grid.html)
  via `...`. Grids created by this function will always take their CRS
  from `data`.

- [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
  now throws an error if you pass arguments via `...` while also
  providing a list of grids (because those arguments would be ignored).

- [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
  is now faster when `data` is an sf object, particularly when grids are
  created by passing arguments to
  [`sf::st_make_grid()`](https://r-spatial.github.io/sf/reference/st_make_grid.html)
  (rather than passing grids via `grids`).

## waywiser 0.5.0

CRAN release: 2023-10-16

- [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
  did not correctly handle grid cellsizes with units. Units (set using
  the `units` package) are now respected.

- [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
  using sf data always returned “1” for truth and estimate counts. This
  was because counts were calculated post-aggregation by mistake.
  Calculation order has been fixed, and these counts should now be
  correct.

## waywiser 0.4.2

CRAN release: 2023-07-20

- This is a patch release to keep up with additional breaking changes in
  the vip package. There are no user-facing differences in this version.

## waywiser 0.4.1

CRAN release: 2023-07-03

- This is a patch release to keep up with breaking changes in the vip
  package. There are no user-facing differences in this version.

## waywiser 0.4.0

CRAN release: 2023-05-17

### Breaking Changes

- [`ww_build_neighbors()`](https://docs.ropensci.org/waywiser/reference/ww_build_neighbors.md)
  (and by extent, every spatial dependence metric) will no longer
  calculate neighbors for non-point or non-polygon geometries.

- Functions dealing with local Moran’s I and related p-values (both data
  frame and vector variants) now return unnamed vectors.

### New Features

- Added a new method to support passing a `SpatRaster` to `data` in
  [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md),
  with `truth` and `estimate` being indices used to subset `data`. This
  is a bit faster than passing `SpatRaster` objects to `truth` and
  `estimate`, as extraction is only done once per grid rather than
  twice, but does not easily support passing R functions to
  `aggregation_function`.

- Metric functions now have better error messages including the name of
  the function the *user* called that errored, not the internal function
  that errored. Huge thanks to
  [@EmilHvitfeldt](https://github.com/EmilHvitfeldt) for their PR
  ([\#40](https://github.com/ropensci/waywiser/issues/40)).

- Data frame metric functions now guarantee that `.estimate` will be an
  unnamed vector.

### Bug Fixes

- The `sf` method for
  [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md)
  is now *much* faster (and more memory efficient).

- Fixed the warning when
  [`ww_area_of_applicability()`](https://docs.ropensci.org/waywiser/reference/ww_area_of_applicability.md)
  calculates an AOA threshold of 0. It now includes “Did you
  accidentally pass the same data as testing and training?” as a bullet.

## waywiser 0.3.0

CRAN release: 2023-03-19

### Breaking Changes

- Removed combination functions – `ww_global_geary`, `ww_global_moran`,
  `ww_local_geary`, `ww_local_moran`, `ww_local_getis_ord`. Use
  `metric_set()` to combine functions instead.

- Renamed `ww_local_getis_ord_pvalue_vec()` and variants to
  [`ww_local_getis_ord_g_pvalue_vec()`](https://docs.ropensci.org/waywiser/reference/local_getis_ord_g.md);
  this change allows internal functions to work properly, and makes it
  easier for the output to indicate if the p-value is associated with a
  g or g\* value.

- Yardstick metrics will no longer include geometry columns in their
  returns.

- The `na_action` argument to
  [`ww_area_of_applicability()`](https://docs.ropensci.org/waywiser/reference/ww_area_of_applicability.md)
  has been replaced by `na_rm`, with a default value of `FALSE`.

- `na_rm` is now `TRUE` by default for non-spatial-autocorrelation
  functions. NA values will cause spatial-autocorrelation functions to
  fail with an error.

### New Features

- Added functions (primarily
  [`ww_multi_scale()`](https://docs.ropensci.org/waywiser/reference/ww_multi_scale.md))
  and a vignette for multi-scale assessment of model predictions.

- Added functions to calculate metrics from Ji and Gallo (2006) and
  Willmott (1981, 1982, 2012):
  [`ww_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
  [`ww_systematic_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
  [`ww_unsystematic_agreement_coefficient()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
  `ww_unsystematic_mpd(),`,
  [`ww_systematic_mpd()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
  [`ww_unsystematic_rmpd()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
  [`ww_systematic_rmpd()`](https://docs.ropensci.org/waywiser/reference/ww_agreement_coefficient.md),
  [`ww_willmott_d()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md),
  [`ww_willmott_dr()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md),
  [`ww_willmott_d1()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md),
  [`ww_systematic_mse()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md),
  [`ww_unsystematic_mse()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md),
  [`ww_systematic_rmse()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md),
  [`ww_unsystematic_rmse()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md).
  Note that
  [`ww_willmott_dr()`](https://docs.ropensci.org/waywiser/reference/ww_willmott_d.md)
  uses the version from Willmott (2012); other implementations
  (sometimes called “d1r”) seem to use an unbounded variant that I
  haven’t found a reference to support.

- Changed a call in
  [`ww_area_of_applicability()`](https://docs.ropensci.org/waywiser/reference/ww_area_of_applicability.md)
  to use FNN for nearest neighbors, rather than fields. This sped up
  prediction by a *lot*.

### Bug Fixes

- Rewrote README and moved the old content to a new vignette on
  assessing the spatial dependency in model residuals.

- Added a dependency on FNN.

- Minimum version for dplyr has been bumped to 1.1.0

## waywiser 0.2.0

CRAN release: 2022-10-23

- Added functions for calculating the area of applicability of a model.

## waywiser 0.1.0

CRAN release: 2022-08-10

- Added functions for automatically constructing `nb` and `listw`
  objects

- Added functions for global and local Geary’s C values.

- Added functions for local Getis-Ord G and G\* values.

- Added functions for global and local Moran’s I values.

- Added a `NEWS.md` file to track changes to the package.
