# Make 'nb' objects from polygon geometries

This function is an extremely thin wrapper around
[`spdep::poly2nb()`](https://r-spatial.github.io/spdep/reference/poly2nb.html),
renamed to use the waywiser "ww" prefix.

## Usage

``` r
ww_make_polygon_neighbors(data, ...)
```

## Arguments

- data:

  An `sfc_POLYGON` or `sfc_MULTIPOLYGON` object.

- ...:

  Additional arguments passed to
  [`spdep::poly2nb()`](https://r-spatial.github.io/spdep/reference/poly2nb.html).

## Value

An object of class "nb"

## Details

These functions can be used for geographic or projected coordinate
reference systems and expect 2D data.

## Examples

``` r
ww_make_polygon_neighbors(guerry)
#> Neighbour list object:
#> Number of regions: 85 
#> Number of nonzero links: 420 
#> Percentage nonzero weights: 5.813149 
#> Average number of links: 4.941176 
```
