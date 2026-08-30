# Build "listw" objects of spatial weights

These functions can be used for geographic or projected coordinate
reference systems and expect 2D data.

## Usage

``` r
ww_build_weights(x, wt = NULL, include_self = FALSE, ...)
```

## Arguments

- x:

  Either an sf object or a "nb" neighbors list object. If an sf object,
  will be converted into a neighbors list via
  [`ww_build_neighbors()`](https://docs.ropensci.org/waywiser/reference/ww_build_neighbors.md).

- wt:

  Either a "listw" object (which will be returned unchanged), a function
  for creating a "listw" object from `x`, or `NULL`, in which case
  weights will be constructed via
  [`spdep::nb2listw()`](https://r-spatial.github.io/spdep/reference/nb2listw.html).

- include_self:

  Include each region itself in its own list of neighbors?

- ...:

  Arguments passed to the weight constructing function.

## Value

A `listw` object.

## Examples

``` r
ww_build_weights(guerry)
#> Characteristics of weights list object:
#> Neighbour list object:
#> Number of regions: 85 
#> Number of nonzero links: 420 
#> Percentage nonzero weights: 5.813149 
#> Average number of links: 4.941176 
#> 
#> Weights style: W 
#> Weights constants summary:
#>    n   nn S0      S1       S2
#> W 85 7225 85 37.2761 347.6683
```
