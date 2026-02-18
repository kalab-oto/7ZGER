# Data frames
Data frame in R is structure similar to common tables. It is two dimensional array of values, where **columns are vectors** of same length and specific data type. It can be created with `data.frame()` function, or can be created by reading a table data, for example with `read.csv()` function. 



## Creating data frame
For example, we can create a data frame with three columns: `taxon`, `sp_count` and `year`, and one row with values `"birds"`, `10` and `2020`. Just use a desired names for columns as arguments of `data.frame()` function, and assign the values to these arguments:

``` r
df <- data.frame(taxon = "birds", sp_count = 10, year = 2020)

df
```
this returns a data frame with one row and three columns

```
  taxon sp_count year
1 birds       10 2020
```

!!! info
    note that there is also row number `1` on the left. This is not part of the data, it is property (*attribute*) of the data frame object, and it is called *row names*. We will not use row names in this course.

As been mentioned, columns are vectors, so they can contain multiple values, but the vectors have to be of the same length. This is how we can create a data frame with multiple rows:

``` r
df <- data.frame(taxon = c("birds", "reptiles", "mammals", "birds"), sp_count = c(10, 2, 4, 15), year = c(2020, 2020, 2020, 2021))
df
```

it can be also written in multiple lines for better readability

``` r
df <- data.frame(
    taxon = c("birds", "reptiles", "mammals", "birds"),
    sp_count = c(10, 2, 4, 15),
    year = c(2020, 2020, 2020, 2021)
)
```

```
     taxon sp_count year
1    birds       10 2020
2 reptiles        2 2020
3  mammals        4 2020
4    birds       15 2021
```

!!! note
    
    Also in this case the *recycling* rule applies:
    ``` r
    df_rec <- data.frame(
        taxon = c("birds", "reptiles", "mammals", "birds"),
        sp_count = 10,
        year = 2020
    )

    df_rec
    ```
    ```
         taxon sp_count year
    1    birds       10 2020
    2 reptiles       10 2020
    3  mammals       10 2020
    4    birds       10 2020
    ```

!!! info

    Most of the time you will work with data frames created by reading tables from files. We will learn how to work with files in the [Working with files, workflow](files_workflow.md) lesson.



## Subsetting data - using indices
The are various ways to subset data frame with row and column names and position (index). The basic way to subset data frame is with `$` and `[]` operator as we do with vectors, but with data frames we can subset rows and columns.

### `$` operator
Let's start with `$` operator. This simply returns the column (vector) by its name:

``` r
df$taxon
```

```
[1] "birds"    "reptiles" "mammals"  "birds"   
```
or

``` r
df$year
```

```
[1] 2020 2020 2020 2021
```

Now you can quicly calculate some statistics on the column:

``` r
max(df$year)
```

``` r
mean(df$sp_count)
```

We will use this a lot in filtering data during the course.

### `[]` operator
The brackets `[]` operator is used to subset rows and columns by name or index. The basic syntax is `data_frame[row, column]`, where `row` and `column` can be name or index (numeric or logical), same as we subset vectods. If you want to subset only rows or columns, you can leave the other part empty (e.g. `data_frame[row, ]`). 


subsetting single row results in data frame with one row and all columns:

``` r
df[2,]
```

```
     taxon sp_count year
2 reptiles        2 2020
```

subsetting with multiple values:

``` r
df[c(1,3),]
```
```
     taxon sp_count year
1    birds       10 2020
3  mammals        4 2020
```

``` r
df[1:2,]
```

```
     taxon sp_count year
1    birds       10 2020
2 reptiles        2 2020
```

subsetting single column results in **vector** with values of the column (similar to `$` operator):

``` r
df[,2]
```

```
[1] 10  2  4 15
```
or

``` r
df[, "sp_count"]
```
```
[1] 10  2  4 15
```

but subsetting columns with multiple values will return data frame:

``` r
df[,c(1,3)]
```
or

``` r
df[, c("taxon", "year")]
```

```
     taxon year
1    birds 2020
2 reptiles 2020
3  mammals 2020
4    birds 2021
```

!!! note
    You can also subset with single index, which will return a data frame with one column with the indexed or named column.

    ``` r
    df[1]
    ```
    or
    ``` r
    df["taxon"] 
    ```
    ```
        taxon
    1    birds
    2 reptiles
    3  mammals
    4    birds 
    ```

getting specific value with row and column index:

``` r
df[2, 1]    
```

```
[1] "reptiles"
```
"get the first two values of `taxon` column":
``` r
df[1:2, "taxon"]
```

```
[1] "birds"    "reptiles"
```
<!-- !!! note "Technical note: subsetting with `[[]]`"

    Theere is third way to subset column, which is use of double brackets, like `df[["taxon"]]` or `df[[1]]`. This is similar to `$` operator, but you can use variable as column name if needed. -->

<!-- !!! note "Technical note"
list/matrix like?
    -->

!!! note "Technical note: list? matrix?"

    In the previous lesson, we skipped the data type *list* because we will explain it later in the course. When you try call function `typeof()` on data frame object, the result will be `"list"`. So don't be confused by this, because data frame is a special type of list, which will be explained in the detail later in the course.

    ``` r
    l <- list(c("birds", "reptiles", "mammals", "birds"), c(10, 2, 4, 15), c(2020, 2020, 2020, 2021))
    l
    ```
    ``` r
    l <- list(taxon = c("birds", "reptiles", "mammals", "birds"), sp_count = c(10, 2, 4, 15), year = c(2020, 2020, 2020, 2021))
    l
    ```

    Also simplier multi-dimensional structures exists in R. These are *arrays* and *matrices*, which are atomic vectors with two (matrix/array) or more (array) dimensions.

    matrix with 2 rows and 3 columns:

    ``` r
    m <- matrix(1:12, nrow = 4, ncol = 3)
    m
    ```

    ```
        [,1] [,2] [,3]
    [1,]    1    5    9
    [2,]    2    6   10
    [3,]    3    7   11
    [4,]    4    8   12
    ```
    
    While *data.frame* is a special *list*, its rectangular structure makes it also similiar to *matrix*. That's why we can subset data various ways, with `$` and single index `[i]` like list, but also with two indices `[row, column]` like matrix.
   


#### Using logical values for subsetting
Most of the time you will subset a data with some condition, and you can do this with logical values in row index, same as we did it with vectors. In this case this returns only rows with position that corresponds to the position of `TRUE` value in logical vector.

For example, if you want to subset only rows with `sp_count` greater than 5, you can do it like this:

``` r
df[df$sp_count > 5,]
```
```
     taxon sp_count year
1    birds       10 2020
4    birds       15 2021
```
if we break it down, its like:
``` r
condition <- df$sp_count > 5
df[condition,]
```
where:
``` r
condition
```
```
[1]  TRUE FALSE FALSE  TRUE
```

so we subset only 1. and 4. row, because only in these rows the condition is `TRUE`

Another example, get only `birds` `sp_count`:

``` r
df[df$taxon == "birds", "sp_count"]
```
```
[1] 10 15
```
this is same as:
``` r
birds_df <- df[df$taxon == "birds",]
birds_df$sp_count
```
```
[1] 10 15
```
so you can calculate some statstics for `birds` only, like what is the minimum of recorded bird species in the data:

``` r
min(df[df$taxon == "birds", "sp_count"])
```
or
``` r
birds_df <- df[df$taxon == "birds",]
min(birds_df$sp_count)
```
```
[1] 10
```

### Mixing the subsetting methods
You can also mix the subsetting methods

``` r
df$sp_count[df$taxon == "birds"]
```
```
[1] 10 15
```
``` r
df[df$year == 2020,]$taxon
```
```
[1] "birds"    "reptiles" "mammals" 
```
## Inspecting data frame
The most common function to inspect objects in R is `str()`, which shows the structure of the object. It gives information about the type of object (called *class*, not so important for now), number of rows and columns, and data type of each column.

``` r 
str(df)
```
```
'data.frame':   4 obs. of  3 variables:
 $ taxon   : chr  "birds" "reptiles" "mammals" "birds"
 $ sp_count: num  10 2 4 15
 $ year    : num  2020 2020 2020 2021
```

!!! question
    Try function `str()` on other objects 


Function `summary()` gives summary based on object nature. For data frame it gives summary statistics of each column.
``` r 
summary(df)
```
```
    taxon              sp_count          year     
 Length:4           Min.   : 2.00   Min.   :2020  
 Class :character   1st Qu.: 3.50   1st Qu.:2020  
 Mode  :character   Median : 7.00   Median :2020  
                    Mean   : 7.75   Mean   :2020  
                    3rd Qu.:11.25   3rd Qu.:2020  
                    Max.   :15.00   Max.   :2021  
```

For quickly accessing dimensions of data frame, you can use functions`dim()`, `nrow()`, and `ncol()`/`length()`, which returns vectors with number of rows and columns, respectively:

dimensions - rows and columns
``` r
dim(df)
```
```
[1] 4 3
```
number of rows directly
``` r
nrow(df)
```
```
[1] 4
```
number of columns directly
``` r
ncol(df)
```
```
[1] 3
```
number of columns with `length()` (same as `ncol()`)
``` r
length(df)
```
```
[1] 3
```

Also for larger data frames, you can use `head()` and `tail()` functions to show first and last rows of data frame, the default value is `6`, but number of rows can be specified with `n` (second) argument:

first 6 rows of data frame, which is in our case all rows

```r
head(df)
```

first 2 rows of data frame
```r
head(df, n = 2)
```

last 2 rows of data frame
```r
tail(df, n = 2)
```


## Editing data frame

Use of `$`  allows you to add new column if the column name does not exist in the data frame:

```r
df
```
```
     taxon sp_count year
1    birds       10 2020
2 reptiles        2 2020
3  mammals        4 2020
4    birds       15 2021
```


```r
df$month <- 8
df
```
```
     taxon sp_count year month
1    birds       10 2020     8
2 reptiles        2 2020     8
3  mammals        4 2020     8
4    birds       15 2021     8

```

If the column name already exists, it replace the values. *Recycling* rule applies here

``` r
df$month <- c(7,8)
df
```
```
     taxon sp_count year month
1    birds       10 2020     7
2 reptiles        2 2020     8
3  mammals        4 2020     7
4    birds       15 2021     8
```

Specific value can be also edited with row and column index:

``` r
df[2, 4]
```
```
[1] 8
```

``` r
df[2, 4] <- 9
df
```
```     taxon sp_count year month
1    birds       10 2020     7
2 reptiles        2 2020     9
3  mammals        4 2020     7
4    birds       15 2021     8
```
or with logical values for row index. For example, you realize that birds were recorded in 2019

``` r
df[df$taxon == "birds", "year"] <- 2019
# or
# df$year[df$taxon == "birds"] <- 2019
# or
# df[df$taxon == "birds",]$year <- 2019
df
```
```
     taxon sp_count year month
1    birds       10 2019     7
2 reptiles        2 2020     9
3  mammals        4 2020     7
4    birds       15 2019     8
```
multiple values can be edited as well

``` r
df[df$taxon == "birds", ]$year <- c(2022, 2025)
df
```
```
     taxon sp_count year month
1    birds       10 2022     7
2 reptiles        2 2020     9
3  mammals        4 2020     7
4    birds       15 2025     8
```


## Summary

### Main outcomes
- understand structure of data frame
- create data frame with `data.frame()` function
- using the `[]` and `$` operators with data frames
<!-- - indices can be negative - subsetting with negative index returns all values except the indexed one -->

### Function overview
- `data.frame()` - create a data frame
- `str()` - show structure of object
- `summary()` - show summary statistics of each column
- `dim()` - show dimensions of data frame
- `nrow()` - show number of rows
- `ncol()`/`length()` - show number of columns
- `head()` - show first rows of data frame
- `tail()` - show last rows of data frame


