# Vector data

In R we can work with vector data using many packages, but the most popular and widely used is `sf` package, and we will use it in following lessons. Other packages used for spatial vector data processing are `stars` and `terra`, and discontinued `sp` and `raster` packages. Each from this packages has its own data classes, but they can be easily converted between each other, which can be useful, since some other packages may be dependent on data classes from specific package.

## Exercise

In this exercise we will work with National Parks and Protected Landscape Areas and aditionall informations (similar to [previous lesson](files_workflow.md#first-project)). We will read the data, explore it and create some plots.

### Data

- shapefile with National Parks and Protected Landscape Areas - download the data form [here](https://data.nature.cz/ds/3) and extract it to `data/vzchu` directory.

- table with additional informations - similarly to [previous lesson](files_workflow.md#first-project), get the `csv` table from [https://drusop.nature.cz/](https://drusop.nature.cz/), but this time with *Velkoplošná zvláště chráněná území* - National Parks and Protected Landscape Areas. Save the table to `data/` directory. 

### Reading data and general exploration

Load `sf` package (if not installed, install it with `install.packages("sf")` in console). So this line should be in the beginning of your script.

``` r
library(sf)
```

Read the data

``` r
vzchu <- st_read("data/vzchu/vzchu.shp")
```

!!! note "`read_sf`"
    there is also function `read_sf` - which is only alias for `st_read` with different default arguments. For more info see `?st_read` or `?read_sf` (similar applies for `write_sf` and `st_write`). The main noticable difference is that `read_sf` reads data as `sf-tibble` and `st_read` as `sf-data.frame`. 


!!! note "`st_layers`"
    If you work with files that can store multiple layers (like `geopackage`), you have to specify which layer to read by adding `layer` argument. To see available layers in file use `st_layers` function.

Check the structure of the data

``` r
vzchu
```

``` r
str(vzchu)
```

``` r
summary(vzchu)
```

``` r
class(vzchu)
```
``` r
head(vzchu)
```
``` r
names(vzchu)
```

You can see that the data is stored as `sf` object, which is `data.frame` with additional attributes. The `geometry` column contains the geometries of the features. 

So we can hadnle the data as `data.frame` and use many functionality we know from working with `data.frame`.

for xample:

get vector of unique values in column `KAT`

``` r
unique(vzchu$KAT)
```

subsetting the columns to get only `KAT` and `NAZEV` columns

``` r
vzchu[,c("KAT", "NAZEV")]
```

subsetting the rows to get only National Parks

``` r
vzchu[vzchu$KAT == "NP",]
```

!!! note
    You can drop the geometries and get plain `data.frame` without geometries and vice versa.

    drop geometries

    ``` r
    vzchu_df <- as.data.frame(vzchu)
    # or
    vzchu_df <- st_drop_geometry(vzchu)
    ```

    drop data.frame

    ``` r
    vzchu_sfc <- vzchu$geometry
    ```

### Plotting
In this point we learn how to show simple plots of the data (spatial and non-spatial). We will use built-in `plot` function, which is able to plot `sf` objects, and try other  types of plots for data `barplot` and `hist` (histogram).

Basic built-in plot functions are good for quick visualization or for simple plots, but later in the course we dedicate a whole lesson to more advanced plotting with `ggplot2` package and other packages for visualizations spatial data.


plot two numeric columns

``` r
plot(vzchu$SHAPE_AREA, vzchu$SHAPE_LEN)
```

!!! note "advanced"
    regression line (linear model fit `lm`) can be easily added to the plot using `abline` function

    ``` r
    area_len_lm <- lm(vzchu$SHAPE_LEN ~ vzchu$SHAPE_AREA)
    
    plot(vzchu$SHAPE_AREA, vzchu$SHAPE_LEN)
    abline(area_len_lm)
    ```

histogram of `SHAPE_AREA` column

``` r
hist(vzchu$SHAPE_AREA)
```

simple barplot of categories in `KAT` column. There is need to count the values first with `table` function.

``` r
barplot(table(vzchu$KAT))
```

#### Plotting `sf` objects

You can use just `plot` with `sf` object, but this will plot all columns in the data (see `?plot.sf`).

``` r
plot(vzchu)
```

If you want to plot only specific column, you can use `plot` with subset using `[]` and name of single column. If you want to plot only geometries, you can use `plot` with `geometry` column.

``` r
plot(vzchu["KAT"])
```


``` r
plot(vzchu["geometry"])
#or
plot(vzchu$geometry)
#or
plot(st_geometry(vzchu))
```


Some examples of use of subsetting for plotting data (same as with `data.frame`)

``` r
vzchu[vzchu$NAZEV == "Šumava","KAT"]
plot(vzchu[vzchu$NAZEV == "Šumava","KAT"])
```
``` r
plot(vzchu[vzchu$NAZEV == "Beskydy","geometry"])
```

``` r
plot(vzchu[vzchu$NAZEV == "Labské pískovce","geometry"])
```


!!! info "Do you remeber the difference of `[]` and `$` operators?"

    `[]` - keeping classes, with `list` this returns `list`, with `data.frame` returns the `data.frame`, and now with `sf` object - returns `sf` object
   
    ``` r
    vzchu["SHAPE_AREA"]
    class(vzchu["SHAPE_AREA"])
    ```
    thus plot use `sf` object
    
    ``` r
    plot(vzchu["SHAPE_AREA"])
    ```

    `$` - subset which always returns the data as `numeric` vector

    ``` r
    vzchu$SHAPE_AREA
    class(vzchu["SHAPE_AREA"])
    ```
    plot use `numeric` vector

    ``` r
    plot(vzchu$SHAPE_AREA)
    ```
    
    but! you can use `$` with `sf` object to get spatial data (`sfc` class) with the `geometry` column, since it is special `sfc` class column (see `str(vzchu)` above)

    ``` r
    vzchu$geometry
    class(vzchu$geometry)
    ```
    thus plotting:
    
    ``` r
    plot(vzchu["geometry"])
    plot(vzchu$geometry) # same as `plot(st_geometry(vzchu))`
    ```
    looks the same, but the first one is `sf` object and the second one is `sfc` object


### Spatial proeprties

Now we will explore some spatial properties of the data.

#### Geometry type

``` r
st_geometry_type(vzchu)
```

geometry type can be converted with `st_cast` function. For example whe have `MULTIPOLYGON` (multi-part features) and we want to convert it to `POLYGON` (single-part features).

``` r
st_cast(vzchu, "POLYGON")
```

!!! note
    `st_cast` can be useful if you have mixed geometry types, typically multi-part features with single-part features., then you can convert all of them to multi-part features.

#### bounding box

Bounding box is the rectangle that encloses all the geometries in the data.

``` r
st_bbox(vzchu)
class(st_bbox(vzchu))
```

#### CRS - coordinate reference system

See the definition of the coordinate reference system (CRS) 

``` r
st_crs(vzchu)
class(st_crs(vzchu))
```

You can transform `sf` data to other CRS with `st_transform` function. For example we can change the CRS to WGS84 (EPSG:4326)

``` r
st_transform(vzchu, 4326)
```
notice the transformed geometry column

### Basic spatial calculations
Here we will calculate the area and the perimeter of the features.

`st_area` - calculate the area

``` r
st_area(vzchu)
```

`st_periemeter` - calculate the perimeter

``` r
st_perimeter(vzchu)
```

add the area and perimeter to the data

``` r
vzchu$area <- st_area(vzchu)
vzchu$perimeter <- st_perimeter(vzchu)
vzchu$shape <- st_perimeter(vzchu)/st_area(vzchu)

```

### Joining data

Now we will join the additional informations from the table to the `sf` object. We will use `merge` function, which is used for merging data frames. In this example we will join the information about the date of the first declaration of the National Park or Protected Landscape Area, and minimum elevation.

``` r
df <- read.csv("data/export.csv")
```

Since this is similar data as last lesson, we can see similar problems. We resolve this using the part of the code from last lesson.

``` r
df <- read.csv("data/export.csv", sep = ";", dec = ",")

df <- df[!is.na(df$Rozloha..ha), ]
df$Datum.prvního.vyhlášení <- as.Date(df$Datum.prvního.vyhlášení, format = "%d.%m.%Y")
```

Before join, we have to check the column names 
``` r
str(df)
str(vzchu)
```

filter out desired columns
``` r
df <- df[,c("Kód", "Datum.prvního.vyhlášení", "Nadmořská.výška.min.")]


``` r
vzchu <- merge(vzchu, df, by.x = "KOD", by.y = "Kód")
```

``` r
vzchu
```

plot the results

``` r
plot(vzchu["Datum.prvního.vyhlášení"])
```

``` r
plot(vzchu["Nadmořská.výška.min."])
```


<!-- ## `data.frame` with coordinates to point `sf` -->


<!-- ## exercise
plot only National Parks
 -->



