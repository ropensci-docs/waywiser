# Make 'nb' objects from sf objects

These functions can be used for geographic or projected coordinate
reference systems and expect 2D data.

## Usage

``` r
ww_build_neighbors(data, nb = NULL, ..., call = rlang::caller_env())
```

## Arguments

- data:

  An sf object (of class "sf" or "sfc").

- nb:

  An object of class "nb" (in which case it will be returned unchanged),
  or a function to create an object of class "nb" from `data` and `...`,
  or `NULL`. See details.

- ...:

  Arguments passed to the neighbor-creating function.

- call:

  The execution environment of a currently running function, e.g.
  `call = caller_env()`. The corresponding function call is retrieved
  and mentioned in error messages as the source of the error.

  You only need to supply `call` when throwing a condition from a helper
  function which wouldn't be relevant to mention in the message.

  Can also be `NULL` or a [defused function
  call](https://rlang.r-lib.org/reference/topic-defuse.html) to
  respectively not display any call or hard-code a code to display.

  For more information about error calls, see [Including function calls
  in error
  messages](https://rlang.r-lib.org/reference/topic-error-call.html).

## Value

An object of class "nb".

## Details

When `nb = NULL`, the method used to create neighbors from `data` is
dependent on what geometry type `data` is:

- If `nb = NULL` and `data` is a point geometry (classes "sfc_POINT" or
  "sfc_MULTIPOINT") the "nb" object will be created using
  [`ww_make_point_neighbors()`](https://docs.ropensci.org/waywiser/reference/ww_make_point_neighbors.md).

- If `nb = NULL` and `data` is a polygon geometry (classes "sfc_POLYGON"
  or "sfc_MULTIPOLYGON") the "nb" object will be created using
  [`ww_make_polygon_neighbors()`](https://docs.ropensci.org/waywiser/reference/ww_make_polygon_neighbors.md).

- If `nb = NULL` and `data` is any other geometry type, the "nb" object
  will be created using the centroids of the data as points, with a
  warning.

## Examples

``` r
ww_build_neighbors(guerry)
#> Neighbour list object:
#> Number of regions: 85 
#> Number of nonzero links: 420 
#> Percentage nonzero weights: 5.813149 
#> Average number of links: 4.941176 
```
