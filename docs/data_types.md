# Data types

## Data object clases
basic classes in R:

- *numeric* - decimal and real numbers
- *integer* - whole numbers
- *character* - text
- *logical* - TRUE or FALSE
- *complex* - complex numbers
- *date/POSIXct* - date

!!! info
    Object can have an attributes - properties like `class`, `length`, `names`, `dim`, `attributes`, that can called or set with with some functions like `class()`, `length()`, `names()`, `dim()`, `attributes()`.

Check the class of the object with `typeof()` function.

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
- *vector* - one dimensional sequence of values of the same class
- *matrix* - two dimensional 
- *array* - multi-dimensional
- *data.frame* - two dimensional array of values, values can be different classes
- *list* - collection of objects, can be different classes
- *factor* - vector of categorical values

In this course we will mainly work with vectors, data frames and lists in less or more complex structures.

### Vectors
Most basic data structure in R. Can be created by concatenating values using `c()` function (mostly used), or creating empty vector with `vector()` function. Lot of functions also returns vectors (like `seq()`).

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

Vectors can't contain values of different classes, various classes well be converted, see `?c`.

``` r
c("123", 456, TRUE)
```

```
[1] "123"  "456"  "TRUE"
``` 

!!! note
    The number at the beginning of the line `[1]` is the index (oreder number) of the first element on the terminal line. It can be useful later when we will work with more complex data and use indexing.

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

!!! note
    Function `str()` returns the structure of object. This is useful for more complex objects, but you can use it for vectors too.
    ``` r
    x <- c(5, 8, 13, 51)
    str(x)
    ```

### Matrix and arrays
We wont't use them much in this course, so I describe them only briefly. They are similar to vectors, but with two or more dimensions. Matrix is two dimensional, array is multi-dimensional. You can create them with `matrix()` and `array()` functions.

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

<!-- todo -->
### Lists
Collection of objects, can be different classes. Can be created with `list()` function. This is fundamental data structure in R, and you can store various objects in it including another list.

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

## Missing values


## Subsetting with indexes


