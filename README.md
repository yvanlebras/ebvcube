
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ebvnetcdf package

<!-- badges: start -->

<!-- badges: end -->

This package can be used to easily access the data of the EBV NetCDFs
which can be downloaded from the [Geobon
Portal](https://portal.geobon.org/). It also provides some basic
visualization. Advanced users can build their own NetCDFs with the EBV
standard.

## Installation

You can install the ebvnetcdf packages with:

``` r
#visibility needs to be set to public first! Does not work right now. 
devtools::install_gitlab("lq39quba/ebvnetcdf", host='git.idiv.de') 
```

## Working with the package - a quick intro

### Take a very first look at the file

With the following two functions you get a basic impression what data
you can get from the EBV NetCDF. First we take a look at some basic
properties of that file.

``` r
library(ebvnetcdf)

file <- paste0(path.package("ebvnetcdf"),"/extdata/cSAR_idiv_v1.nc")
#file <- system.file("extdata/cSAR_idiv_v1.nc", package="raster")

#prop.file <- ebv_properties(file)
```

Now let’s get the paths to all possible datacubes. The resulting
dataframe includes the paths and also descriptions of e.g. metric and or
scenario - take a look\!

``` r
#datacubes <- ebv_datacubepaths(file)
```

We will get the properties of one specific datacube - fyi: the result
also holds the general file properties from above. This time you get the
warning that the value\_range does not exists. So don’t take the
displayed value\_range seriously.

``` r
#prop.dc <- ebv_properties(file, datacubes[1,1], verbose=T)
```

### Plot the data to get a better impression

Plot a map of the datacube that we just looked at - it has 12 timesteps,
mabe look at two different ones?

``` r
# dc <- datacubes[1,1]
# ebv_plot_map(file, dc, timestep = 1)
# ebv_plot_map(file, dc, timestep = 6)

# What was the data about again? Check the properties!
# prop.dc@title
# And the datacube?
# prop.dc@entity_information@label
```

It’s nice to see the global distribution, but how is the change of that
datacube (non forest birds) over time? Let’s take a look at the average.
The function returns the values, catch them\!

``` r
# averages <- ebv_plot_indicator(file, dc)
```

It would be cool to have that for other indicators as well? Well you
have to wait for an update of the package. Or maybe implement it
yourself using the functions coming up next?

### Read the data from the files to start working

Before you actually load the data it may be nice to get an impression of
the value range and other basic measurements.

``` r
#info for whole dataset
#ebv_data_analyse(file, dc)

#info for a subset (bounding box)
#bb <- c(5,15,47,55)
#ebv_data_analyse(file, dc, bb)
```

To access the data use the following

``` r
#load whole data set for all timesteps
#ebv_data_read(file, dc, c(1:12), delayed = F)

#load subset (shapefile)
#shp <- 'path/to/subset/file/.shp'
#ebv_data_read_shp(file, dc, shp, NULL, c(1,2,3))
```

### Take a peek on the creation of an EBV NetCDF

This process is still work in progress. Right now you’ll have to insert
all the metadata in the Geobon Portal and then use the resulting json
file to start. Additionally to that json file the function needs the
amount of entities you want to add.

``` r
#json <- 'path/to/json/file.json'
#out <- 'path/to/new/netcdf/file.nc'
#ebv_ncdf_create(json, out, 5)
```

Afterwards you can add your data to the NetCDF from GeoTiff files. You
need to indicate which scenario and/or metric and/or entity the data
belongs to.

``` r
#tif <- 'path/to/data.tif' 
#ebv_ncdf_add_data(out, tif, metric=1, scenario=NULL, entity=1, timestep=c(1:6), band=c(1:6))
```

Now there are still a few information missing about the data you just
added. The following function makes it possible to add the information.

``` r
#dc.new <- ebv_datacubepaths(out)
#ebv_ncdf_entity_attributes(out, dc.new[1,1], longname='habitat', label='bog', units='Percentage', fillvalue=-999)
```

Ups\! You did a mistake and want to change the attribute?\! No
problem:

``` r
#ebv_ncdf_write_attribute(out, attribute_name='longname', value='raised bog', levelpath='metric00/entity00')
```

In this case the levelpath corresponds to the datacube path. But you can
also alter attributes at the metric or scenario level. See the Vignette
for more info.
