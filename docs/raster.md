 Raster data

As with vector data, raster data can be handled with various packages. The most common package is the `terra` package, successor of the deprecated `raster` package. `terra` package is also capable process vector data, but it is not as complex as `sf` package in this way. 

!!! note 
    `terra` objects are spcific objects (S4), data are stoed in matrix/array not in data.frame. It have hidden structure and its not stored in lists like `sf` objects. To access the metadata you have to use specific functions.

<!-- 7. Raster Data - packages, reading and writing files, terrain analysis, reclassification  -->

``` r
# install.packages("terra")
library(terra)
```

### Reading raster data - `SpatRaster`
``` r
dem <- rast("/home/ok/git/orthoptera-atlas/processed_data/eudem.tif")
dem

crs(dem)
ext(dem)
``` 
plot the raster data
``` r
plot(dem)
```
plot the histogram of the raster data
``` r
hist(dem)
```
get more bins

``` r
hist(dem, breaks = 100)
```
Class `SpatRaster` can handle multiple layers (bands) of raster data. These can be accessed by `[[` operator as in lists. Layers can be stacked together simply with `c()` function, but layers have to have the same extent, resolution and crs. 

``` r
dem2 <- c(dem, dem)
dem2
dem2[[1]]
```
Layers can be named, and accessed by name. 

``` r
names(dem2) <- c("dem1", "dem2")
dem2[["dem1"]]
```
Filtering layers can be done by `[[` operator with index including negative idenxing.

drop the first layer:
``` r
dem3 <- dem2[[-1]]
```
get the first layer:
``` r
dem3 <- dem2[[1]]
```

## Summary statistics
How to get mean, min, max, sum, etc. of the raster data?

``` r
mean(dem)
max(dem)
```

This seems not works as expected. That is because theese functions calculates tha statistics across all layers, eg. maximum value of all layers at each certain pixel position.

To get quick statistics for each layer, you can use `summary` function.
``` r
summary(dem)
```

or work with the raw values of the raster data, but take care of the `NA` values.

``` r
mean(values(dem), na.rm = TRUE)
```
## Terrain analysis
function `terrain` can be used to calculate various terrain attributes from the DEM. See `?terrain` for more details.

``` r
slope <- terrain(dem, "slope")
aspect <- terrain(dem, "aspect")
```

stack the layers together
``` r
dem <- c(dem, slope, aspect)
```
plot entire object
``` r
plot(dem)
```

or all histograms of the slope layer
``` r
hist(dem)
```

---
=== "try"
 
    plot only the slope layer from `dem` object
  
=== "solution"
 
    ``` r
    plot(dem[[2]])
    # or
    plot(dem[["slope"]])
    ```
---


## Reclassification

### logical reclassification
simply use logical operators to create new raster object with values `TRUE` and `FALSE` (1 and 0)

``` r
reclassified_dem <- dem[[1]]  > 1000
plot(reclassified_dem)
```
multiple conditions can be combined with logical operators
``` r

reclassified_dem <- dem[[1]] > 500 & dem[[1]] < 550
```
### replacing values
use of subsetting and logical operators can be used to replace values in the raster data. 

``` r
reclassified_dem <- dem[[1]]
reclassified_dem[reclassified_dem > 500] <- NA
plot(reclassified_dem)
```

!!! tip
    functions `replace` or `ifelse` works similar way, but can be used also for more complex operations

    ``` r
    reclassified_dem <- replace(dem[[1]], dem[[1]] > 1000, 1)
    plot(reclassified_dem)
    ```


### value reclassification
Standard way to reclassify is use of `classify` function, which reclassify the raster data with reclassification matrix. The matrix has to have 3 columns, first two are the range of values and the third is the value to replace the values in the range.  

``` r
reclass_matrix <- matrix(c(0, 500, 1,    
                           500, 1000, 2,  
                           1000, Inf, 3),  
                         ncol = 3, byrow = TRUE)

reclass_matrix
```
``` r
reclassified_dem <- classify(dem[[1]], reclass_matrix)
```
plot the reclassified raster
``` r
plot(reclassified_dem)
```

## Map algebra
Map algebra is a way to perform operations on raster data. It is similar to vector data, but the operations are performed on the pixel values. 

``` r
dem2 <- dem[[1]] * 2
plot(dem2)
```

``` r
sum(dem)
```

!!! tip
    map algebra is commonmly used for multispectral data, eg. to calculate vegetation indices NDVI, NDWI, etc. 


### exercise
Find the suitable areas for vineyards, these areas are on southern slopes (aspect between 135 and 225 degrees) with slope between 5 and 15 degrees, and elevation between 200 and 500 meters.  

``` r

dem <- rast("/home/ok/git/orthoptera-atlas/processed_data/eudem.tif")
slope <- terrain(dem, "slope")
aspect <- terrain(dem, "aspect")
```


``` r

# Sklon mezi 5-30% (3–16 stupňů)
suitable_aspect <- (aspect >= 135) & (aspect <= 225)
suitable_slope <- (slope > 4) & (slope < 16)
suitable_elevation <- (dem >= 200) & (dem <= 500)

suitable_area <- suitable_aspect & suitable_slope &  suitable_elevation
```
If you want to keep all layers in the object, you can stack them together. 

``` r
suitable_area <- c(suitable_aspect, suitable_slope,suitable_elevation)
plot(suitable_area)
```
``` r
min(suitable_area)
plot(min(suitable_area))
```
get the area of the suitable areas

```r
sum(suitable_area)
plot(sum(suitable_area))
```

## writing raster data

``` r
writeRaster(suitable_area, "outputs/suitable_area.tif", overwrite = TRUE)
```