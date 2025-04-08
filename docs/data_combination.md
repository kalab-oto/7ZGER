
<!-- 9. Data Combination - zonal statistics, raster value extraction  
general string manipulation - for loops, apply functions
-->


## General R functions

### Basic string manipulation
The most common string manipulation functions in R are `paste`, `paste0`, `gsub`, and `substr`. These are useful for data cleaning and preparation, and working with paths, filenames, URLs etc.

#### `paste` and `paste0`
`paste` and `paste0` are used to concatenate strings. They are similar, `paste0` is a shortcut for `paste` without `sep` argument, while `paste` has default `sep = " "`. 

```r
paste("Hello", "World")
paste("Hello", "World", sep = " - ")
x <- paste0("Hello", "World")
```

#### `gsub`
`gsub` is used to replace all occurrences of a pattern in a string. It uses regular expressions.

```r
x <- "Hello World"
gsub("World", "R", x)
```

#### `substr`
`substr` is used to extract a substring from a string

```r
x <- paste0("Hello", "World")
substr(x, 6, 10)
```

```r
my_date <- "20250409"
year <- substr(date, 1, 4)
month <- substr(date, 5, 6)
day <- substr(date, 7, 8)
```
`substr` can be also used to replace a sring
```r
my_date <- "20250409"
substr(my_date, 1, 4) <- "2023"

my_date
```


!!! note
    Notice that good practices to work with dates is use dedicated functions. In base R you can use `as.Date()` or `as.POSIXct()`. 

    ```r
    my_date <- "20250409"
    date_obj <- as.Date(my_date, format = "%Y%m%d")

    year <- format(date_obj, "%Y")

    print(date_obj)
    print(year)
    ```

### Basic automatisation

#### `apply` family of functions

The `apply` family of functions is used to **apply** a function to each element of an object (like a list or a matrix). The most common functions are `apply` for arrays, matrices and data franes, and `lapply` for lists (or simplified `lapply` - `sapply`, which returns vector).

```r
x <- list(1:10, 11:20, 21:30)
sum(x)
lapply(x, sum)
```

when using `apply` function, you need to specify the `MARGIN` argument, which defines if you want to apply the function to rows or columns. `MARGIN = 1` for rows, `MARGIN = 2` for columns.

```r
df <- data.frame(a = 1:5, b = 6:10)
df
apply(df, 1, sum) 
apply(df, 2, sum) 
```


#### `for` loops
Basic `for` loops are used to iterate over a sequence of numbers or elements. The code use executed repeatedly for each element in the sequence. If you want to see what is happening on certain place in the loop, you can use `print()` function, which behaves similarly to runinng variable in the console.  

!!! note
    In many cases, you can use `lapply` or `sapply` instead of `for` loops. They can be more efficient and sometimes easier to read. 

```r
for (i in 1:10) {
    print(i)
}
```

```r
for (i in 1:10) {
    paste("Hello", "I can't see this")
    what_is_happening <- paste("This is iteration number", i)
    print(what_is_happening)
}
```

Lot of times you will need to store the results of the loop in a vector or list. Good practice is to preallocate the vector or list before the loop. 

```r
vector_of_results <- c()
for (i in 1:10) {
    vector_of_results[i] <- paste("This is iteration number", i)
}
```

```r
list_of_results <- list()
for (i in 1:10) {
    list_of_results[[i]] <- paste("This is iteration number", i)
}
```

!!! note
    If you know number of elements in desired list, than more efficient way to preallocate the vector is to use `vector()` function. This avoids the resizing the list in each iteration, which can be slower in large loops.

    ```r
    list_of_results <- vector("list", length = 10)
    for (i in 1:10) {
        list_of_results[[i]] <- paste("This is iteration number", i)
    }
    ```

!!! warning
    Try to avoid appending/combining a resulted vector or list in each iteration of the loop. Its works fine for small loops, but can be slow and inefficient for large loops, due to creating a new vector in each iteration in memory.

    ```r
    #bad practise:
    vector_of_results <- c()
    for (i in 1:10) {
        vector_of_results <- c(vector_of_results, paste("This is iteration number", i))
    }
    ``` 

!!! tip
    Get the good practices, but do not be afraid to use the code that works for you. Be aware of possible performance improvements when your code goes slow.

## Downloading data from a list of urls

Example of downloading files from a list of urls. We will download data from LPIS (https://mze.gov.cz/public/app/eagriapp/lpisdata/) and save them to the local `data` directory. 

URL for the zip files are structured as follows:
https://mze.gov.cz/public/app/eagriapp/lpisdata/20250405-713830-DPB-SHP.zip

Where `20250405` is the date of the data, `713830` is the code of the municipality, and `DPB-XML-A` is the type of data. 

So we can use `paste0` to create the url for each municipality and date. 

```r

municip_codes <- c(682675, 683434, 683442, 683451)
root_url <- "https://mze.gov.cz/public/app/eagriapp/lpisdata/20250405-"

for (municip_code in municip_codes) {
    url <- paste0(root_url, municip_code, "-DPB-SHP.zip")
    destfile <- paste0("data/", municip_code, "-DPB-SHP.zip")
    download.file(url, destfile)
}
```

You can enchance the code by unzipping the files after downloading and deleting the zip files. 

```r
for (municip_code in municip_codes) {
    url <- paste0(root_url, municip_code, "-DPB-SHP.zip")
    destfile <- paste0("data/", municip_code, "-DPB-SHP.zip")
    download.file(url, destfile)
    
    # Unzip the file
    unzip(destfile, exdir = "data/")
    
    # Delete the zip file
    file.remove(destfile)
}
```

## Spatial data combination
Read the data from the local directory. Note that we will use `terra` package also for reading the shapefiles. 

```r
library(terra)

#raster
r <- rast("data/eudem.tif")
names(r) <- "elev"

#vector
shp_paths <- list.files("data", pattern = ".shp$", full.names = TRUE)
lpis <- vect(shp_paths[1])
```

## crop raster data with vector data
Same as in previous example, we can crop the raster data with the vector data, as the function takes the extent object from the raster or vector data. 

```r
r_cropped <- crop(r, lpis)
plot(r_cropped)
plot(lpis, add = TRUE)
```

## zonal statistics
For zonal statistics, we can use `zonal()` function, with `fun` argument to specify the statistics we want to calculate. Default is `mean`, see `?zonal` for more options. 

```r
zonal_r <- zonal(r, lpis)
zonal_r
```

If you need the original polygon object with appended statistics, you can use `as.polygon = TRUE` argument. 

```r
zonal_r <- zonal(r, lpis, as.polygon = TRUE)
zonal_r
zonal_r$elev
plot(zonal_r)
```
task: whats the difference between original elevation in `VYSKA` and recalculated elevation in `elev`? 

```r
zonal_r$elev - zonal_r$VYSKA
```

## raster value extraction
To extract raster values at specific features or raster pixels, we can use `extract()` function. It can be used with any vector object, not only points. In case of polygons its similar to zonal statistics. See `?extract` for more options. 

Get raster vales for lpis centroids

```r
lpis_pts <- centroids(lpis)
extract(r, lpis_pts)
#or
elev_pts <- extract(r, lpis_points, bind = TRUE)
plot(r_cropped)
plot(lpis, add = TRUE)
plot(elev_pts, add = TRUE, col = "red")
```

## bodnus
```r
plot(r_cropped)
for (i in seq(elev_pts)) {
    plot(lpis[i], add = TRUE)
    plot(elev_pts[i], add = TRUE, col = "red")
    Sys.sleep(0.2)
}
```