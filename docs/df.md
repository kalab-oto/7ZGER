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

!!! note "Technical note: list?"

    In the previous lesson, we skipped the data type *list* because we will explain it later in the course. When you try call function `typeof()` on data frame object, the result will be `"list"`. So don't be confused by this, because data frame is a special type of list, which will be explained in the detail later in the course.

## Subsetting data
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
## Summary

### Main outcomes
- understand structure of data frame
- create data frame with `data.frame()` function
- using the `[]` and `$` operators with data frames
<!-- - indices can be negative - subsetting with negative index returns all values except the indexed one -->

### Function overview
<!-- - `read.csv()` - read comma separated values (CSV) files -->
- `data.frame()` - create a data frame
<!-- - `str()` - show structure of object
- `head()` - show first rows of data frame
- `tail()` - show last rows of data frame
- `summary()` - show summary statistics of each column -->



!!! note "Technical note: array and matrix"
    Simplier multi-dimensional structures are arrays and matrices, which are atomic vectors with two (matrix/array) or more (array) dimensions. You can create them with `matrix()` and `array()` functions. We wont't use them much in this course. 

    matrix with 2 rows and 3 columns:
    ``` r
    m <- matrix(1:6, nrow = 2, ncol = 3)
    m
    ```

    ```
        [,1] [,2] [,3]
    [1,]    1    3    5
    [2,]    2    4    6
    ```

    array with 2 rows, 3 columns and 2 "layers":

    ``` r
    a <- array(1:12, dim = c(2,3,2))
    a
    ```
    ``` 
    , , 1

        [,1] [,2] [,3]
    [1,]    1    3    5
    [2,]    2    4    6

    , , 2

        [,1] [,2] [,3]
    [1,]    7    9   11
    [2,]    8   10   12
    ```