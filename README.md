
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ebvnetcdf

<!-- badges: start -->

<!-- badges: end -->

This package can be used to easily access the data of the EBV NetCDFs
which can be downloaded from the [Geobon
Portal](https://portal.geobon.org/). It also provides some basic
visualization. Advanced users can build their own NetCDFs with the EBV
standard.

## Installation

The ebvnetcdf packages is not yet released on CRAN. You can install it
with:

``` r
#visibility needs to be set to public first! Does not work right now. 
devtools::install_gitlab("lq39quba/ebvnetcdf", host='git.idiv.de') 
```

## Working with the package - a quick intro

### Take a very first look at the file

With these two functions you get a basic impression what data you can
get from the EBV NetCDF.

``` r
library(ebvnetcdf)

file <- paste0(path.package("ebvnetcdf"),"/extdata/cSAR_idiv_v1.nc")

# Take a look at some basic properties of that file.
# prop.file <- ebv_properties(file)

# Get all possible paths to the datacubes - dataframe including the paths and also descriptions of e.g. metric and or scenario - take a look!
# datacubes <- ebv_datacubepaths(file)

# Get the properties of one specific datacube - fyi: the result also holds the general file properties from above.
# This time you get the warning that the value_range does not exists. So don't take the displayed value_range seriously.
#prop.dc <- ebv_properties(file, datacubes[1,1], verbose=T)
```

### Plot the data to get a better impression

``` r
# Plot a map of the datacube that we just looked at - it has 12 timesteps, mabe look at two different ones?
# dc <- datacubes[1,1]
# ebv_plot_map(file, dc, timestep = 1)
# ebv_plot_map(file, dc, timestep = 6)

# what was the data about again? Check the properties!
# prop.dc@title
# And the datacube?
# prop.dc@entity_information@label

# It's nice to see the global distribution, but how it the change of that datacube (non forest birds) over time?
# Let's take a look at the average. The function returns the values, catch them!
# averages <- ebv_plot_indicator(file, dc)

# It would be cool to have that for other indicators as well? Well you have to wait for an update of the package.
# Or maybe implement it yourself using the functions coming up next?
```

### Read the data from the files to start working

### Take a peek on the creation of an EBV NetCDF
