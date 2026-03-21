# Raster data

## General R functions

### Download data
You can download data directliy in R using `download.file()` function. In this example we will download the CHELSA climate - BIO1 and  BIO12 for 1981-2010. Check `?download.file` for arguments and options.  

``` r
download.file("https://os.unil.cloud.switch.ch/chelsa02/chelsa/global/bioclim/bio01/1981-2010/CHELSA_bio01_1981-2010_V.2.1.tif", "data/CHELSA_bio1_1981-2010_V.2.1.tif")
download.file("https://os.unil.cloud.switch.ch/chelsa02/chelsa/global/bioclim/bio12/1981-2010/CHELSA_bio12_1981-2010_V.2.1.tif", "data/CHELSA_bio12_1981-2010_V.2.1.tif")
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

!!! info
    Function `rast()` can read also files directly from url string.
    
    ```r
    urls <- c(
        "https://os.unil.cloud.switch.ch/chelsa02/chelsa/global/bioclim/bio01/1981-2010/CHELSA_bio01_1981-2010_V.2.1.tif",
        "https://os.unil.cloud.switch.ch/chelsa02/chelsa/global/bioclim/bio12/1981-2010/CHELSA_bio12_1981-2010_V.2.1.tif"
        )
    r <- rast(urls)
    r
    ```

If we want to read all raster in stack, all rasters have to have the same extent, resolution and projection. We have to preprocess the data before.

try to read only CHELSA bioclimatic files and dem separately

```r
bio <- rast(list.files("data", pattern = "bio", full.names = TRUE))

dem <- rast("data/eu_dem.tif")
```
### get the same extent

For clipping raster object to specific extent use `crop()` function. It can crop the raster with extent object, or other raster object (retrieve the extent from the other raster).

```r
bio_cz <- crop(bio, dem)
```
Extntes do not overlap, we need to reproject the raster to the same projection.

```r
dem_cz <- project(dem, crs(bio))
bio_cz <- crop(bio, dem_cz)
```


!!! note "Technical note"
    Notice that `project()` function can also reproject the raster to the same projection, resolution and extent as the other raster with argument `to`.



### resample to the same resolution

see `?resample` for more options, particularly `method` argument. In our case we will use some sumary method

```r
dem_cz <- resample(dem_cz,bio_cz, method = "average")
```

### stack the rasters

```r
cz_stack <- c(bio_cz, dem_cz)
```
### masking
mask the raster stack with the DEM

```r
masked_z <- mask(cz_stack, dem_cz)
plot(masked_z)
```
or mask all raster in stack with any NA in any raster

```r
masked_z <- mask(cz_stack, anyNA(cz_stack))   
```
### distance to the nearest 1000m elevation

`distance()` function calculates the distance to the nearest non-NA cell in the raster. So we need to rewrite all values less then 1000 to NA. 

```r
dem_1000 <- dem_cz
dem_1000[dem_1000 < 1000] <- NA
plot(dem_1000)
``` 

```r
dist_1000 <- distance(dem_1000)
plot(dist_1000)
```
mask the distance raster with the DEM and add it to the stack

```r
dist_1000 <- mask(dist_1000, dem_cz)
masked_z <- c(masked_z, dist_1000)
plot(masked_z)
```



## Other exercises

![Whittaker's biomes classification (Ricklefs 2008)](img/biome.png)
