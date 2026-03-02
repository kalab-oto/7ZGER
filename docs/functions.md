Summary of used functions in the course sorted by the order of appearance in the course. 

## General functions

- `round()` - round a number to a specified number of decimal places
- `abs()` - absolute value of a number

- `typeof()` - type of the object
- `as.*()` - conversion between data types
- `c()` - combine values into vector
- `length()` - length of vector
- `min()`,`max()`, `mean()`, `sum()` - calculate descriptive statistics of numeric vector

- `read.csv()` - read a CSV file and create a data frame
- `write.csv()` - write a data frame to a CSV file
- `is.na()` - return logical vector of `TRUE` and `FALSE` values, where `TRUE` means that the value is `NA`
- `table()` - count the unique values of a vector
- `unique()` - return unique values of a vector

- `install.packages()` - install packages from CRAN
- `library()` - load package in session

- `names()` - return the names of the columns in a data frame
- `plot()`, `hist()`, `barplot()` - basic plots

## Spatial functions - `sf` package

- `st_read()` - read a spatial file and create an `sf` object
- `st_bbox()` - calculate the bounding box of the geometries
- `st_crs()` - return the coordinate reference system of the data
- `st_transform()` - transform the data to another coordinate reference system
- `st_area()` - calculate the area of the geometries
- `st_perimeter()` - calculate the perimeter of the geometries
- `st_write()` - write an `sf` object to a spatial file