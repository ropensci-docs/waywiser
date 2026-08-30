# Simulated data based on WorldClim Bioclimatic variables

This data is adapted from the CAST vignette
`vignette("cast02-AOA-tutorial", package = "CAST")`. The original data
is derived from the Worldclim global climate variables.

## Usage

``` r
worldclim_simulation
```

## Format

An sf object with 10,000 rows and 6 columns:

- bio2:

  Mean Diurnal Range (Mean of monthly (max temp - min temp))

- bio10:

  Mean Temperature of Warmest Quarter

- bio13:

  Precipitation of Wettest Month

- bio19:

  Precipitation of Coldest Quarter

- geometry:

  The location of the sampled point.

- response:

  A virtual species distribution, generated using the
  `generateSpFromPCA()` function from the `virtualspecies` package.

## Source

<https://www.worldclim.org>
