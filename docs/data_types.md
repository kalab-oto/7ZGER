# Data structures

## Data object types
primary (*atomic*) types in R:

- *double* - decimal and real numbers
- *integer* - whole numbers
- *character* - text
- *logical* - TRUE or FALSE
- (*complex* - complex numbers)

!!! note
    Object can have an attributes - properties like `class`, `length`, `names`, `dim`, `attributes`, that can called or set with with some functions like `class()`, `length()`, `names()`, `dim()`, `attributes()`.

Check the type of the object with `typeof()` function. This returns the low level datatype of the object sotred in memory.

``` r
some_text <- "Hello, world!"
typeof(some_text)
```

```
[1] "character"
```

``` r
x <- 5 == 5
typeof(x)
```

```
[1] "logical"
```
``` r
x <- "5"
typeof(x)
```

```
[1] "character"
```
Conversion between classes can be done with `as.` functions.
``` r
"5" + 5
x <- as.numeric("5")
x + 5
```

## Data structures

- ***vector*** - one dimensional sequence of values of the same class
- *matrix* - two dimensional 
- *array* - multi-dimensional
- ***data.frame*** - two dimensional array of values, values can be different classes
- ***list*** - collection of objects, can be different classes
- *factor* - vector of categorical values

In this course we will mainly work with vectors, data frames and lists in less or more complex structures.

### Atomic vectors
Most basic data structure in R. Sequence of data of same type. Can be created by concatenating values using `c()` function (mostly used), or creating empty vector with `vector()` function. Lot of functions also returns vectors (like `seq()`).
Atomic vectors are usually refered as vectors.

``` r
c(1, 15, 4, 6)
```

```
[1]  1 15  4  6
```
make a sequence of numbers with `seq()` function and arguments `from`, `to`, `by`:
``` r
seq(1, 10, by = 2)
```

```
[1] 1 3 5 7 9
```
or with `:` operator:
``` r
1:10
```

```
[1]  1  2  3  4  5  6  7  8  9 10
```
sequence of characters:

``` r
c("a", "b", "c")
```

```
[1] "a" "b" "c"
```

Vectors can't contain values of different types, various classes well be converted, see `?c`.

``` r
c("123", 456, TRUE)
```

```
[1] "123"  "456"  "TRUE"
``` 

!!! info
    The number at the beginning of the line `[1]` is the index (oreder number) of the first element on the terminal line. It can be useful later when we will work with more complex data and use subsetting.

    try:
    ``` r
    seq(1, 1000, by= 5)
    ```

You can do calculaion with vectors.
``` r
x <- c(1, 2, 3, 4)
x * 5
```

```
[1]  5 10 15 20
```

You can do operations with vectors of the same length.

``` r
x <- c(1, 2, 3, 4)
y <- c(5, 6, 7, 8)

x + y
```

```
[1]  6  8 10 12
```

Adding element to vector:

``` r
x <- c("a", "b", "c")
x

x <- c(x, "d")
x
```

Function `length()` returns the length of the vector (value of attribute length).

``` r
x <- c(1, 2, 3, 4)
length(x)
```

```
[1] 4
```
!!! note
    You can also use attribute `name` and assign names to vector elements.
    ``` r
    x <- c(1, 2, 3, 4)
    x
    
    names(x) <- c("first", "second", "third", "fourth")
    x
    attributes(x)
    ```
    or simply:
    ``` r
    x <- c(first = 1, second = 2, third = 3, fourth = 4)
    x
    attributes(x)
    ```

Function `sum()` returns the sum of the vector.
``` r
x <- c(1, 2, 3, 4)
sum(x)
```

```
[1] 10
```



try:
``` r
x <- c(5, 8, 13, 51)
mean(x)
min(x)
max(x)
typeof(x)

y <- c(1, 2, "c", "4")
mean(y)
```   


Function `str()` returns the structure of object. This is useful for more complex objects, but you can use it for vectors too.
``` r
x <- c(5, 8, 13, 51)
str(x)
```

### Matrix and arrays
We wont't use them much in this course, so I describe them only briefly. They are similar to vectors (technically they are vectors with `dim` attribute), but with two or more dimensions. Matrix is two dimensional, array is multi-dimensional. You can create them with `matrix()` and `array()` functions.

``` r
m <- matrix(1:6, nrow = 2, ncol = 3)
m
```

```
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
```


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

Vectors, matrices and arrays can be converted. 
Try:

``` r
a <- array(1:12, dim = c(2,3,2))
c(a)
m <- matrix(1:6, nrow = 2, ncol = 3)
c(m)
v <- 1:10
dim(v) <- c(2,5)
v
v2 <- c(1:12)
dim(v2) <- c(2,3,2)
```

### Lists
Also vectors - collection of objects, which can be different classes. Can be created with `list()` function. This is fundamental data structure in R, and you can store any objects in it, for example another list.

``` r
my_list <- list(1, "a", TRUE)
my_list
```

nesting lists:
``` r
my_list <- list(1, "a", TRUE)
nested_list <- list(my_list, 2, "b", FALSE)
nested_list
```

list can be named
``` r
named_list <- list(number = 1, text = "a", boolean = TRUE)
named_list
```
### Data frame
Data frames in R is structure similar to common tables. It is two dimensional array of values, where columns can be different classes. It can be created with `data.frame()` function. In next section we will learn how to create a data frame from `.csv` table with `read.csv()` function. Another representation of data frame is `tibble` from `tidyverse`, or `data.table` from `data.table` package, but we will use `data.frame` through the course.

``` r
df <- data.frame(species = "species A", count = 5, year = 2020)
df
```

``` r
df <- data.frame(species = c("species A","species B","species C"), count = c(5,10,7), year = c(2020,2020,2020))
df

class(df)
str(df)

typeof(df)
```

## Subsetting data with `[ ]`, `[[ ]]` and `$`
Subsetting is basic operation which is used to extract part of the data. You can subset data with brackets `[ ]`, `[[ ]]` or `$` operator.

### brackets `[ ]` and `[[ ]]` 
Single brackets are used to subset vectors, lists, matrices and arrays. You can subset data by index:

#### Subsetting vectors
``` r
v <- c(123,789,451,657)
```

``` r
v[3]
```

```
[1] 451
```

You can subset data also by logical vector. In this example we want only values greater than 500:

``` r
v[v > 500]
# sames as
# v[c(FALSE, TRUE, FALSE, TRUE)]
# just we calculate the vector of logical values with
# v > 500
```

```
[1] 789 657
```

You can subset data also by names:

``` r
v <- c(first = 123, second = 789, third = 451, fourth = 657)
v["third"] # returns vector
```
```
third 
  451  
```

``` r
v[["third"]] # also returns vector, but drop names
```
```
[1] 451
```
#### Subsetting lists
If we have this list:
``` r
my_list <- list(1, "a", TRUE, c(123,456,789))
```


Single brackets `[ ]` returns **list** of subsetted data:
``` r
my_list[2] 
```

```
[[1]]
[1] "a"
```
``` r
my_list[4] 
```

```
[[1]]
[1] 123 456 789
```
``` r
class(my_list[4])
```

```
[1] "list"
```
Double brackets `[[ ]]`returns the **vector** of subsetted data :
``` r
my_list[[2]]
```
```
[1] "a"
```
``` r
my_list[[4]]
```
```
[1] 123 456 789
```
``` r
class(my_list[[4]])
```

```
[1] "numeric"

```
Brackets can be used subsequently:
``` r
my_list[[4]][2]
```
```
[1] 456
```
#### Subsetting data.frame
same as lists, but you can also subset row and column, sperated by comma:

``` r
df <- data.frame(species = c("species A","species B","species C"), count = c(5,10,7), year = c(2020,2020,2020))
df
```

```
    species count year
1 species A     5 2020
2 species B    10 2020
3 species C     7 2020
```

Subset column:
``` r
df["species"] #or df[1]
```

```    
    species 
1 species A 
2 species B 
3 species C 
    
```
Subset column returnig vector:

``` r
df[["species"]] #or df[[1]]df$species
```

```
[1] "species A" "species B" "species C"
```
Subset rows:

``` r
df[2,]
```

```
    species count year
2 species B    10 2020
```
Subset row and column:

``` r
df[2, "species"] #or df[2, 1]
```

```
[1] "species B"
```
remember, you can always use variable names instead of indexes:

``` r
row_number <- 2
column_name <- "species"
df[row_number, column_name]
```

```
[1] "species B"
```

### `$` operator
`$` operator is used to subset list or column of data frame by **name**. 

``` r
named_list <- list(number = 1, text = "a", boolean = TRUE, numbers = c(123,456,789))
named_list$numbers # same as named_list[["numbers"]] or named_list[[4]]
```

```
[1] 123 456 789
```

``` r
df <- data.frame(species = c("species A","species B","species C"), count = c(5,10,7), year = c(2020,2020,2020))

df$species # same as df[["species"]] or df[[1]]
```

```
[1] "species A" "species B" "species C"
```

!!! tip
    You can also add columns to **data.frame** or elements to named **list** with `[ ]` or `$`, simply declaring its name as its exists:
    
    data.frame:
    ``` r
    df$source <- "field_obs"
    # or
    df["source"] <- "field_obs"
    df
    ```

    ```
        species count year    source
    1 species A     5 2020 field_obs
    2 species B    10 2020 field_obs
    3 species C     7 2020 field_obs
    ```
    named list:
    ``` r
    named_list <- list(number = 1, text = "a", boolean = TRUE)
    named_list$vect <- c(123,456,789)
    named_list
    ```
    ```
    $number
    [1] 1

    $text
    [1] "a"

    $boolean
    [1] TRUE

    $vect
    [1] 123 456 789
    ```

## Missing values `NA`
`NA` is simply missing value, `NaN` is 'not a number' (and its also `NA`). `NA` can be any type, but `NaN` is double. You can check if value is `NA` with `is.na()` function.

Numeric vector
``` r
x <- c(123, 456, NA, 789, NaN)
```

Check which values are `NA`:
``` r
is.na(x)
```

```
[1] FALSE FALSE  TRUE FALSE  TRUE
```

Check which values are `NaN`:
``` r
is.nan(x)
```

```
[1] FALSE FALSE FALSE FALSE  TRUE
```



`NA` values can cause problems in calculations

``` r
mean(x)
```

```
[1] NA
```
We can usually remove `NA` values with `na.rm` or similar argument, see help for function with `?`:

``` r
mean(x, na.rm = TRUE)
```

```
[1] 456
```

`NA` values can be also removed directly with `na.omit()` function:

``` r
na.omit(x)
```

```
[1] 123 456 789
attr(,"na.action")
[1] 3 5
attr(,"class")
[1] "omit"
```

String vector (note that `NA` is not quoted in result):

``` r
x <- c("a", "b", NA, "c", NaN)
x
```

```
[1] "a"   "b"   NA    "c"   "NaN"
```