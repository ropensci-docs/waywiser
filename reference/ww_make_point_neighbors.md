# Make 'nb' objects from point geometries

This function uses
[`spdep::knearneigh()`](https://r-spatial.github.io/spdep/reference/knearneigh.html)
and
[`spdep::knn2nb()`](https://r-spatial.github.io/spdep/reference/knn2nb.html)
to create a "nb" neighbors list.

## Usage

``` r
ww_make_point_neighbors(data, k = 1, sym = FALSE, ...)
```

## Arguments

- data:

  An `sfc_POINT` or `sfc_MULTIPOINT` object.

- k:

  How many nearest neighbors to use in
  [`spdep::knearneigh()`](https://r-spatial.github.io/spdep/reference/knearneigh.html).

- sym:

  Force the output neighbors list (from
  [`spdep::knn2nb()`](https://r-spatial.github.io/spdep/reference/knn2nb.html))
  to symmetry.

- ...:

  Other arguments passed to
  [`spdep::knearneigh()`](https://r-spatial.github.io/spdep/reference/knearneigh.html).

## Value

An object of class "nb"

## Details

These functions can be used for geographic or projected coordinate
reference systems and expect 2D data.

## Examples

``` r
ww_make_point_neighbors(ny_trees)
#> Warning: neighbour object has 1712 sub-graphs
#> Neighbour list object:
#> Number of regions: 5303 
#> Number of nonzero links: 5303 
#> Percentage nonzero weights: 0.01885725 
#> Average number of links: 1 
#> 1712 disjoint connected subgraphs
#> Non-symmetric neighbours list
```
