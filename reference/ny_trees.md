# Number of trees and aboveground biomass for Forest Inventory and Analysis plots in New York State

The original data is derived from the Forest Inventory and Analysis
program, implemented by the US Department of Agriculture's Forest
Service.

## Usage

``` r
ny_trees
```

## Format

An sf object using EPSG 5070: NAD83 / Conus Albers (in meters), with
5,303 rows and 5 columns:

- yr:

  The year measurements were taken.

- plot:

  A unique identifier signifying the plot measurements were taken at.

- n_trees:

  The number of trees present on a plot.

- agb:

  The total aboveground biomass at the plot location, in pounds.

- geometry:

  The centroid of the plot location.
