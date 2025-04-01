# Raster data


## General R functions

### Download data
You can download data directliy in R using `download.file()` function. In this example we will download the CHELSA climate - BIO1 1981-2010, and predicted BIO1 2071-2100 for GFDL-ESM4 model and SSP126 scenario. Check `?download.file` for arguments and options.  

``` r
download.file("https://os.zhdk.cloud.switch.ch/chelsav2/GLOBAL/climatologies/1981-2010/bio/CHELSA_bio1_1981-2010_V.2.1.tif", "data/CHELSA_bio1_1981-2010_V.2.1.tif")

download.file("https://os.zhdk.cloud.switch.ch/chelsav2/GLOBAL/climatologies/2071-2100/GFDL-ESM4/ssp126/bio/CHELSA_bio1_2071-2100_gfdl-esm4_ssp126_V.2.1.tif", "data/CHELSA_bio1_2071-2100_gfdl-esm4_ssp126_V.2.1.tif")

```

### Listing files
You can list files in any directory using `list.files()` function. With argument `pattern` you can filter the files by matching string - *regular expression* *(regex)*, for example part of the name or file extension.

``` r
list.files()
```
list all files in the current directory recursively

``` r
list.files(recursive = TRUE)
```

or list all files in subdirectory `data`:

``` r
list.files("data")
```

get the relative path of the files :

``` r
list.files("data", full.names = TRUE)
```

use `pattern` to filter the files with regex
``` r
list.files(recursive = TRUE, pattern = "bio")
```

## Raster manipulation
Task: 
1. Prepare raster stack of bioclimatic CHELSA data and elevation data. The resolution of the raster stack will be 30seconds (~1km) - resolution of the CHELSA data.
2. Mask the raster stack with the DEM data.
3. Create raster of distances to the nearest 1000m elevation.
4. Save the raster stack to separate files.


```r
library(terra)
```

read all raster files in the directory `data`:

```r
raster_files <- list.files("data", pattern = ".tif$", full.names = TRUE)
```
!!! note
    Notice that we used `pattern = ".tif$"` to filter only the files with `.tif` extension. The `$` means that the string should end with `.tif`.

```r
r <- rast(raster_files)
```

If we want to read all raster in stack, all rasters have to have the same extent, resolution and projection. We have to preprocess the data before.

try to read only bio1 files and dem separately

```r
r_bio <- rast(list.files("data", pattern = "bio", full.names = TRUE))

r_dem <- rast("data/eu_dem.tif")
```
### get the same extent

For clipping raster object to specific extent use `crop()` function. It can crop the raster with extent object, or other raster object (retrieve the extent from the other raster).

```r
r_bio_cz <- crop(r_bio, r_dem)
```
Extntes do not overlap, we need to reproject the raster to the same projection.

```r
r_dem_cz <- project(r_dem, crs(r_bio))
r_bio_cz <- crop(r_bio, r_dem_cz)
```

### resample to the same resolution

see `?resample` for more options, particularly `method` argument. In our case we will use some sumary method

```r
r_dem_cz <- resample(r_dem_cz,r_bio_cz, method = "average")
```

### stack the rasters

```r
r_stack <- c(r_bio_cz, r_dem_cz)
```
### masking
mask the raster stack with the DEM

```r
masked_z <- mask(r_stack, r_dem_cz)
plot(masked_z)
```
or mask all raster in stack with any NA in any raster

```r
masked_z <- mask(r_stack, anyNA(r_stack))   
```
### distance to the nearest 1000m elevation

`distance()` function calculates the distance to the nearest non-NA cell in the raster. So we need to rewrite all values less then 1000 to NA. 

```r
dem_1000 <- r_dem_cz
dem_1000[dem_1000 < 1000] <- NA
plot(dem_1000)
``` 

```r
dist_1000 <- distance(dem_1000)
plot(dist_1000)
```
mask the distance raster with the DEM and add it to the stack

```r
dist_1000 <- mask(dist_1000, r_dem_cz)
masked_z <- c(masked_z, dist_1000)
plot(masked_z)
```
<!-- Todo: writeRaster(b, file.path("D:/...filepath/", names(b)),
             format="GTiff", bylayer=T, overwrite=T) -->


## Other exercises
- compare between 1981-2010 and 2071-2100, where the mean temperature is greater than 9  in the Czech Republic

```r
r9 <- c(r_bio_cz[[1]] > 9, r_bio_cz[[2]] > 9)
```
!!! Example "Advanced"
    write a function that will plot the difference of these rasters with given temperature threshold.

- check how the mean temperature changes between 1981-2010 and 2071-2100 in the Czech Republic

```r
d <- r_bio_cz[[2]] - r_bio_cz[[1]]
d
plot(d)
```

check how mean temperature changes between 1981-2010 and 2071-2100 globally

```r
d <- r_bio[[2]] - r_bio[[1]]
plot(d)
```

