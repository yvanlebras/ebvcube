
<!-- README.md is generated from README.Rmd. Please edit that file -->

# ebvnetcdf package

<!-- badges: start -->

<!-- badges: end -->

This package can be used to easily access the data of the EBV NetCDFs
which can be downloaded from the [Geobon
Portal](https://portal.geobon.org/). It also provides some basic
visualization. Advanced users can build their own NetCDFs with the EBV
standard.

## EBVs - Essential Biodiversity Variables

The EBV NetCDF standard is designed to hold Essential Biodiversity
Variables. This concept is further described
[here](https://geobon.org/ebvs/what-are-ebvs/). An important core
element of the EBV NetCDFs is their nested structure. All datacubes in
the NetCDF are assigned to one metric. But this metric can have several
entities. On top of this hierarchy there can be several scenarios. The
following block displays an abstract exhausted hierarchy.

``` bash
├── scenario1
│   └── metric1
│       ├── entity1
│       ├── entity2
│       ├── ...
│       └── entity999
└── scenario2
    └── metric2
        ├── entity1
        ├── entity2
        ├── ...
        └── entity999
```

The following is a practical example of the NetCDF structure. Basis is
the [global habitat availability for mammals
dataset](https://portal.geobon.org/ebv-detail?id=5).

``` bash
├── SSP1-RCP2.6
│   └── absolute values per 5 years and species (km2)
│       ├── Hipposideros calcaratus
│       ├── Hipposideros fulvus
│       ├── ...
│       └── Habromys lepturus
│
├── SSP2-RCP4.5
│   └── absolute values per 5 years and species (km2)
│       ├── ...
│       └── Habromys lepturus
├── ...
│
└── SSP5-RCP8.5
    └── absolute values per 5 years and species (km2)
        ├── ...
        └── Habromys lepturus    
```

Just keep in mind: All EBV NetCDF always have a metric. But they may or
may not have a scenario and/or entity.

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

file <- system.file("extdata/cSAR_idiv_v1.nc", package="ebvnetcdf")
prop.file <- ebv_properties(file)

prop.file
#> An object of class "EBV NetCDF properties"
#> Slot "general":
#> $title
#> [1] "Changes in local bird diversity (cSAR)"
#> 
#> $description
#> [1] "Changes in bird diversity at the grid cell level caused by land-use, estimated by the cSAR model (Martins & Pereira, 2017). It reports changes in species number (percentage and absolute), relative to 1900, for all bird species, forest bird species, and non-forest bird species in each cell. Uses the LUH 2.0 projections for land-use, and the PREDICTS coefficients for bird affinities to land-uses."
#> 
#> $ebv_class
#> [1] "Community composition"
#> 
#> $ebv_name
#> [1] "Species diversity"
#> 
#> $ebv_subgroups
#> [1] "scenario" "metric"   "entity"  
#> 
#> $creator
#> [1] "Ines Martins"
#> 
#> $institution
#> [1] ""
#> 
#> $contactname
#> [1] ""
#> 
#> $contactemail
#> [1] ""
#> 
#> $value_range
#> [1] NA
#> 
#> 
#> Slot "spatial":
#> $srs
#> CRS arguments: +proj=longlat +datum=WGS84 +no_defs 
#> 
#> $epsg
#> [1] 4326
#> 
#> $resolution
#> [1] 1 1
#> 
#> $extent
#> [1] -180  180  -90   90
#> 
#> $dimensions
#> [1] 180 360  12
#> 
#> 
#> Slot "temporal":
#> $units
#> [1] "days since 1860-01-01 00:00:00.0"
#> 
#> $t_delta
#> [1] "10 Years"
#> 
#> $timesteps
#>  [1] 18262 21914 25567 29219 32872 36524 40177 43829 47482 51134 54787 56613
#> 
#> $timesteps.natural
#>  [1] "1910-01-01" "1920-01-01" "1930-01-01" "1940-01-01" "1950-01-01"
#>  [6] "1960-01-01" "1970-01-01" "1980-01-01" "1990-01-01" "2000-01-01"
#> [11] "2010-01-01" "2015-01-01"
#> 
#> 
#> Slot "metric":
#> $status
#> [1] "Only available with datacube argument."
#> 
#> 
#> Slot "scenario":
#> $status
#> [1] "Only available with datacube argument."
#> 
#> 
#> Slot "entity":
#> $status
#> [1] "Only available with datacube argument."
```

Now let’s get the paths to all possible datacubes. The resulting
dataframe includes the paths and also descriptions of e.g. metric and or
scenario - take a look\!

``` r
datacubes <- ebv_datacubepaths(file)
datacubes
#>         paths scenario_longnames metric_longnames         entity_longnames
#> 1 past/mean/0  past: 1900 - 2015             mean non forest birds species
#> 2 past/mean/A  past: 1900 - 2015             mean         all brid species
#> 3 past/mean/F  past: 1900 - 2015             mean      forest bird species
```

We will get the properties of one specific datacube - fyi: the result
also holds the general file properties from above. This time you get the
warning that the value\_range does not exists. So don’t take the
displayed value\_range seriously.

``` r
prop.dc <- ebv_properties(file, datacubes[1,1], verbose=T)
#> Warning in ebv_i_read_att(hdf, "value_range"): The attribute value_range does not exist. Or maybe wrong location in NetCDF?
prop.dc
#> An object of class "EBV NetCDF properties"
#> Slot "general":
#> $title
#> [1] "Changes in local bird diversity (cSAR)"
#> 
#> $description
#> [1] "Changes in bird diversity at the grid cell level caused by land-use, estimated by the cSAR model (Martins & Pereira, 2017). It reports changes in species number (percentage and absolute), relative to 1900, for all bird species, forest bird species, and non-forest bird species in each cell. Uses the LUH 2.0 projections for land-use, and the PREDICTS coefficients for bird affinities to land-uses."
#> 
#> $ebv_class
#> [1] "Community composition"
#> 
#> $ebv_name
#> [1] "Species diversity"
#> 
#> $ebv_subgroups
#> [1] "scenario" "metric"   "entity"  
#> 
#> $creator
#> [1] "Ines Martins"
#> 
#> $institution
#> [1] ""
#> 
#> $contactname
#> [1] ""
#> 
#> $contactemail
#> [1] ""
#> 
#> $value_range
#> [1] NA
#> 
#> 
#> Slot "spatial":
#> $srs
#> CRS arguments: +proj=longlat +datum=WGS84 +no_defs 
#> 
#> $epsg
#> [1] 4326
#> 
#> $resolution
#> [1] 1 1
#> 
#> $extent
#> [1] -180  180  -90   90
#> 
#> $dimensions
#> [1] 180 360  12
#> 
#> 
#> Slot "temporal":
#> $units
#> [1] "days since 1860-01-01 00:00:00.0"
#> 
#> $t_delta
#> [1] "10 Years"
#> 
#> $timesteps
#>  [1] 18262 21914 25567 29219 32872 36524 40177 43829 47482 51134 54787 56613
#> 
#> $timesteps.natural
#>  [1] "1910-01-01" "1920-01-01" "1930-01-01" "1940-01-01" "1950-01-01"
#>  [6] "1960-01-01" "1970-01-01" "1980-01-01" "1990-01-01" "2000-01-01"
#> [11] "2010-01-01" "2015-01-01"
#> 
#> 
#> Slot "metric":
#> $label
#> [1] "mean"
#> 
#> $description
#> [1] "mean values per decade"
#> 
#> 
#> Slot "scenario":
#> $label
#> [1] "past: 1900 - 2015"
#> 
#> $description
#> [1] "calculations where done per decade betrween 1900 and 2015"
#> 
#> 
#> Slot "entity":
#> $long_name
#> [1] "Changes in local bird diversity (cSAR)"
#> 
#> $label
#> [1] "non forest birds species"
#> 
#> $unit
#> [1] "mean change of species diversity per area (pixel size) to baseline 1900 "
#> 
#> $type
#> [1] "H5T_IEEE_F32LE"
#> 
#> $fillvalue
#> [1] -3.4e+38
```

### Plot the data to get a better impression

Plot a map of the datacube that we just looked at - it has 12 timesteps,
mabe look at two different ones?

``` r
options('temp_directory'=system.file("extdata/", package="ebvnetcdf"))
dc <- datacubes[1,1]
ebv_plot_map(file, dc, timestep = 1)
```

<img src="man/figures/README-unnamed-chunk-4-1.png" width="100%" />

``` r
ebv_plot_map(file, dc, timestep = 6)
```

<img src="man/figures/README-unnamed-chunk-4-2.png" width="100%" />

``` r

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
#load whole data set for two timesteps
data <- ebv_data_read(file, dc, c(1,2), delayed = F)
dim(data)
#> [1] 180 360   2

#load subset (shapefile)
#shp <- 'path/to/subset/file/.shp'
#ebv_data_read_shp(file, dc, shp, NULL, c(1,2,3))
```

### Take a peek on the creation of an EBV NetCDF

This process is still work in progress. Right now you’ll have to insert
all the metadata in the Geobon Portal and then use the resulting json
file to create an empty NetCDF file with the correct structure and the
metadata. Additionally to that json file the function needs the amount
of entities the NetCDF will encompass.

``` r
#json <- 'path/to/json/file.json'
#out <- 'path/to/new/netcdf/file.nc'
#ebv_ncdf_create(json, out, 5)
```

Afterwards you can add your data to the NetCDF from GeoTiff files. You
need to indicate which scenario and/or metric and/or entity the data
belongs to. You can add your data timestep per timestep or all at once.

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
also alter attributes at the metric or scenario level. See the vignette
for more info.
