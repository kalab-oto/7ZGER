# Vector geometry operations and analysis

data for this lesson

``` r
library(sf)
clc <- st_read("data/CLC18_CZ.shp")
vzchu <- st_read("data/vzchu.shp")
mantis <- read.csv("data/observations-537954.csv")
```


## Creating `sf` objects from coordinate data (`data.frame`)
``` r
mantis_sf <- st_as_sf(mantis, coords = c("longitude", "latitude"), crs = 4326)
mantis_sf <- st_transform(mantis_sf, 5514)
plot(mantis_sf$geometry)
```

## Spatial join

How many mantis observations are in NP and PLA?

``` r
st_join(mantis_sf, vzchu)
#or
st_intersection(mantis_sf, vzchu)
```

In which land cover class are the mantis observations most often?

``` r
st_join(mantis_sf, clc)
```

``` r
st_crs(clc) <- st_crs(mantis_sf) #or st_transform(clc, st_crs(vzchu))
mantis_clc <- st_join(mantis_sf, clc)
```

``` r
mantis_clc$CODE_18
table(mantis_clc$CODE_18) 
sort(table(mantis_clc$CODE_18),decreasing = T)
```

## Spatial operations

We will use the `Beskydy` region as an example.

``` r
beskydy <- vzchu[vzchu$NAZEV == "Beskydy", "geometry"]
plot(beskydy)
```

### subsetting

subsetting points - How many mantis observations are in PLA Beskydy?

``` r
mantis_beskydy <- mantis_sf[beskydy,]
mantis_beskydy
plot(beskydy$geometry)
plot(mantis_beskydy$geometry, add = TRUE)
```
or using `st_intersection` function

``` r
mantis_beskydy <- st_intersection(mantis_sf, beskydy)
plot(beskydy$geometry)
plot(mantis_beskydy$geometry, add = TRUE)
```

subsetting polygons

``` r
clc_beskydy <- clc[beskydy,]
plot(clc_beskydy$geometry)
plot(beskydy$geometry, border = "red", add = T, lwd = 2)
```

### clip

What is the most common land cover class in Beskydy?

``` r
clc_beskydy <- st_intersection(beskydy, clc)
plot(clc_beskydy$geometry)
plot(beskydy$geometry, border = "red", add = T, lwd = 2)
table(clc_beskydy$CODE_18)
sort(table(clc_beskydy$CODE_18), decreasing = T)
```

What is the area of each land cover class in Beskydy?

1. sum the `data.frame` area column with `aggregate` function

``` r
aggregate(x = clc_beskydy$Area_Ha, by = list(clc_beskydy$CODE_18), FUN = sum)
```

2. aggregate the `sf` object area column with `aggregate` function

``` r

clc_beskydy_ag <- aggregate(clc_beskydy, list(clc_beskydy$CODE_18), sum)

```


### buffer

What is the area of each land cover class in 1km buffer around the mantis observations?

``` r
buffer_mantis <- st_buffer(mantis_beskydy, 1000)
plot(buffer_mantis$geometry)
```

``` r
clc_buffer_mantis <- st_intersection(clc,buffer_mantis)
```
overlapping polygons
``` r
overlapping_polygons <- st_overlaps(clc_buffer_mantis)
overlapping_polygons <- clc_buffer_mantis[unlist(overlapping_polygons),]
plot(overlapping_polygons["CODE_18"])
```
use `st_union` to merge overlapping buffer polygons (similar to `dissolve` in desktop GIS)

``` r
clc_buffer_mantis <- st_union(clc_buffer_mantis)
clc_buffer_mantis <- st_intersection(clc,buffer_mantis)
```

``` r
aggregate(x = st_area(clc_buffer_mantis), by = list(clc_buffer_mantis$CODE_18), FUN = sum)
```

### centroid

``` r
centroid_vzchu <- st_centroid(vzchu)
plot(vzchu$geometry, col = "grey")
plot(centroid_vzchu$geometry, col = "red", add = TRUE)
```
### convex hull (mcp home range)

``` r
hull_mantis <- st_convex_hull(st_union(mantis_beskydy))
plot(hull_mantis, col = "grey")
plot(mantis_beskydy$geometry, col = "red", add = TRUE)
```

### distance
distance matrix
``` r
mantis_distances <- st_distance(mantis_sf, vzchu)
head(mantis_distances)
```
#### Compute the most distant mantis observation to any protected area
``` r
mantis_distances <- st_distance(mantis_sf, vzchu)
max(mantis_distances)
hist(mantis_sf$max_distance)
```

#### distance: Compute the nearest distance from each mantis observation to the nearest protected area

apply family of functions (apply, lapply)

!!! info
    `apply` function applies a function to the rows or columns of a matrix or array. 
    `lapply` function applies a function to each element of a list or vector.

    ```r 
    m <- matrix(1:9, nrow = 3)
    m
    apply(m, 1, sum)
    apply(m, 2, sum)
    ```
    ``` r
    l <- list(10:50,6:10,45:100)
    l
    lapply(l, mean)
    ```


``` r
mantis_distances <- st_distance(mantis_sf, vzchu)
mantis_sf$min_distance <- apply(mantis_distances, 1, min)
hist(mantis_sf$min_distance, breaks = 20, main = "Distance to Nearest Protected Area", xlab = "Distance (m)")
```


### Exercise: Append Land Cover Areas to Mantis Data

``` r
mantis_beskydy <- mantis_sf[beskydy,]
clc_beskydy <- clc[beskydy,]

buffer_mantis <- st_buffer(mantis_beskydy, 1000)

clc_buffer_mantis <- st_intersection(clc,buffer_mantis)


clc_buffer_mantis$area <- st_area(clc_buffer_mantis)

# Aggregate areas by mantis observation and land cover class
mantis_clc_areas <- aggregate(clc_buffer_mantis$area,
                              by = list(mantis_id = clc_buffer_mantis$id,
                                        CODE_18 = clc_buffer_mantis$CODE_18),
                              FUN = sum)

# Reshape to wide format and merge with mantis dataset
mantis_clc_wide <- reshape(mantis_clc_areas, idvar = "mantis_id", timevar = "CODE_18", direction = "wide")

mantis_sf <- merge(mantis_sf, mantis_clc_wide, by.x = "id", by.y = "mantis_id", all.x = TRUE)

```

## Summary
### New functions
#### Base R
- `merge()`
- `apply()`
- `reshape()`

#### `sf`
- `st_buffer()`
- `st_centroid()`
- `st_convex_hull()`
- `st_distance()`