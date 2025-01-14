# ebvcube 0.1.3
## Major changes
- ebv_create: implemented paleo and irregular dates netCDFs (terranova)
- ebv_properties: remove backwards compatibility to old standard (before ACDD)
- replaced h5ls by h5dump to make ebv_properties and ebv_datacubepaths faster
- ebv_properties: rename 'timesteps_natural' to 'dates' (changes across functions)
## Bug fixes
- ebv_create: contributor_name, content coverage type and domain corrected (comma + white space)
- ebv_i_file_opened: solve error on MAC OS (CRAN)
- ebv_i_eval_epsg: suppress warning by terra when assigning a crs (test error). [#23](https://github.com/LuiseQuoss/ebvcube/issues/23) by Roger Bivand

# ebvcube 0.1.2
## Major changes
- implement CRAN feedback (ebv_i_file_opened (lsof to fuser), examples and test (writing to user library))

## Bug fixes
- ebv_create: keywords for spatial_scope
- ebv_attribute: ebv_cube attributes connected to metric (changes over all scenarios)

# ebvcube 0.1.1
## Major changes
- implement CRAN feedback (examples(dontrun))

## Bug fixes

# ebvcube 0.1.0
## Major changes
- using ggplot2 for plots
- using terra for geospatial operations (removing dependencies: gdalUtils, rgdal, raster and sp)

## Bug fixes


# ebvcube 0.0.1
First version of the package
