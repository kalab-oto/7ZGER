# Data structures

## Data types 
As we saw in previous section, in R, as in every other software for working with data, we operate with various types of data (e.g., numbers, text, logical values). These types of data are in R called *data types* and they are important to understand, so we can work with the data properly and know what functions we can use. 

Basic data types in R, that we will use in this course are:

- *double* - real numbers - `1.23`
- *integer* - whole numbers - `1L`
- *character* - text - `"Hello, world!"` 
- *logical* - TRUE or FALSE - `TRUE`
- *list* - a collection of any objects, including other lists and objects we will learn later in course - `list(1, "a", TRUE)`


Check the data type of the object with `typeof()` function. This returns the low level datatype of the object sotred in memory. 


``` r
num <- 5.1
typeof(num)
```

```
[1] "double"
```


``` r
num <- 5
typeof(num)
```

```
[1] "double"
```
This seems to be unexpected, but even if we write whole number, R stores it as double by default. If you want to store it as integer, you have to add `L` at the end of the number, or use conversion functions that explained later.


``` r
num_int <- 5L
typeof(num_int)
```

```
[1] "integer"
```


``` r
some_text <- "Hello, world!"
typeof(some_text)
```

```
[1] "character"
```

``` r
x <- "5"
typeof(x)
```

```
[1] "character"
```

``` r
x <- TRUE
typeof(x)
```

```
[1] "logical"
```
!!! note "Technical note"
    `TRUE` and `FALSE` are reserved words in R, but you can also use `T` and `F` as shortcuts for them. Hovewer `TRUE` and `FALSE` are more explicit, readable, and less error prone, because `T` and `F` are just predefined variables and can be redefined by user.
    
    ``` r
    TRUE <- 456 
    ```
    
    ``` r
    TRUE == T
    T <- 456
    TRUE == T
    T <- TRUE
    TRUE == T
    ```

``` r
x <- 5 == 5
typeof(x)
```

```
[1] "logical"
```
Basic awareness of data types is also important to understand more complex data structures later. Also it is needed for basic data manipulation, for instance when correcting data that have been read incorrectly from tables. For example when numbers are read as characters and vice versa. 

### Data type conversion
Conversion between types can be done with functions `as.*` family.

text (character) to numeric:

``` r
"5" + 5
x <- as.double("5")
x + 5
```
or

``` r
as.numeric("5")
```

numeric to text (character):

``` r
as.character(123)
```

### Vectors (Atomic vectors)
Atomic vectors are fundamental data structure in R. It is sequence of elements of same data type (homogenous). Even a single value, like `5`, `"a"`, or `TRUE`, is technically an atomic vector of length 1. Atomic vectors of multiple values can be created with `c()` function (combine), and are usually called simply *vectors*.

``` r
c(1, 15, 4, 6)
```

```
[1]  1 15  4  6
``` 
``` r
typeof(c(1, 15, 4, 6))
```

```
[1] "double"
```

Numeric sequence can be created various ways, basic approach is use `:` operator

``` r
1:10
```

```
[1]  1  2  3  4  5  6  7  8  9 10
```

!!! note "Technical note"
    `:` operator creates sequence of integers if both numbers are integers

!!! tip
    You can make a specific sequence of numbers with `seq()` function and arguments `from`, `to`, `by`:
    ``` r
    seq(1, 10, by = 2)
    ```

    ```
    [1] 1 3 5 7 9
    ```


vector of characters:

``` r
c("a", "b", "c")
```

```
[1] "a" "b" "c"
```

vector of logical values:

``` r
c(TRUE, FALSE, TRUE)
```

```
[1]  TRUE FALSE  TRUE
```

As (atomic) vectors can't contain values of different types, various types will be converted, see `?c`.

``` r
c("123", 456, TRUE)
```

```
[1] "123"  "456"  "TRUE"
``` 

!!! note "Technical note"
    When we combine different types of data, R will convert them to the most general type. 
    
    The `==` help `?"=="` says:
    ```
    If the two arguments are atomic vectors of different types, one is
    coerced to the type of the other, the (decreasing) order of
    precedence being character, complex, numeric, integer, logical and
    raw.
    ```
    The `c()` help `?c` says:
    ```
    The output type is determined from the highest type of the
    components in the hierarchy NULL < raw < logical < integer <
    double < complex < character < list < expression.
    ```

    And the arithemetic operations help `?Arithmetic` or `?"+"` says:
    ```
    Logical vectors will be coerced to integer or numeric vectors,
    ‘FALSE’ having value zero and ‘TRUE’ having value one.
    ```

    For our purposes the **logical < integer < double** priority and that TRUE and FALSE are considered as 1 and 0 respectively is the most important.

    Thats why `c("123", 456, TRUE)` returns character vector, because character is the most general type

    Try another examples:
    ```
    "123" == 123
    TRUE + 5
    TRUE == "TRUE"
    TRUE == "T"
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

But also its multiple. The simplest example is adding a single number to vector:

``` r
x <- c(1, 2, 3, 4)
x + 10
```

```
[1] 11 12 13 14
```
This is called *recycling* and it works with vectors of different lengths, but the shorter vector has to be multiple of the longer one. 

When you execute:
``` r
x <- c(1, 2, 3, 4)
x + 10
```
R is actually using `c(10, 10, 10, 10)` as the second vector, because it recycles the value `10` to match the length of `x`.


Another example:
``` r
x <- c(1, 2, 3, 4)
y <- c(10, 20)
x+y
```
Here, R is using `c(10, 20, 10, 20)` as `y`, and so on.


This works also with comparison operators:

``` r
x <- ("a", "b", "c","c")
x == "c"
```

```
[1] FALSE FALSE  TRUE  TRUE
```
In this case `"c"` is recycled to `c("c", "c", "c", "c")` and compared to each element of `x`.

Adding element to existing vector:

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


You can use basic descriptive statistics functions as `sum()`, `mean()`, `min()`, `max()` etc.
``` r
x <- c(1, 2, 3, 4, 5, 8, 13, 51)
sum(x)
```

```
[1] 97
```

try:
``` r
mean(x)
min(x)
max(x)
```   

!!! example
    === "Examples"
        When you get used to with the basics we learned to this point, you can already do some basic data manipulation and analysis. 

        1. How many values in vector `x` are greater than 5?
        2. How many times the "species A" is in vector `species`?
        
        This can be done with combination of what we learned, however we will learn more efficient ways to do this later in course.
        
        ``` r
        x <- c(1, 2, 3, 4,5, 8, 13, 51)
        species <- c("species A", "species B", "species C", "species A", "species B", "species A")
        ``` 
        
    === "1"
        How many values in vector `x` are greater than 5?

        ``` r
        x <- c(1, 2, 3, 4,5, 8, 13, 51)
        tf <- x > 5
        sum(tf)
        ```
        or simply:
        ``` r
        sum(x > 5)
        ```
    === "2"
        How many times the "species A" is in vector `species`?

        ``` r
        species <- c("species A", "species B", "species C", "species A", "species B", "species A")
        tf <- species == "species A"
        sum(tf)
        ```
        or simply:
        ``` r
        sum(species == "species A")
        ```

## Basics of indexing vectors

Indexing is a basic operation used to extract parts of data. We will use indexing extensively throughout this course.

Vectors are not only collections of elements but also ordered structures.  
This ordering allows us to extract elements by their position using numeric indices or logical vectors inside square brackets `[ ]`. There are also other options in indexing data, but we will keep it simple for now.

Indexing with numeric values means specifying which elements we want to extract by their position in the vector.


Get a single element of vector:
``` r
x <- c(10, 20, 30, 40, 50)
x[2]
```
```r
species <- c("species A", "species B", "species C", "species A", "species B", "species A")
species[3]
```

To extract multiple elements, we can use vector of indices:
``` r
x <- c(10, 20, 30, 40, 50)
x[c(2, 4)]
```

We can also use vector assigned to variable as index:
``` r
x <- c(10, 20, 30, 40, 50)
i <- 2
x[i]
i <- c(2, 4)
x[i]
```

!!! tip
    Remember that you can always assign the result of any operation to a new variable

    ``` r
    x <- c(10, 20, 30, 40, 50)
    i <- c(2, 4)
    y <- x[i]
    y
    ```

Indexing with logical values works slightly differently. In this case, the vector of logical values (`TRUE` or `FALSE`) is used to select elements from the original vector. The `TRUE` values indicate which elements to extract.

``` r
x <- c(10, 20, 30, 40, 50)
x[x > 35]
```
```
[1] 40 50
```


Let's break down what happens in `x[x > 35]`:
``` r
x <- c(10, 20, 30, 40, 50)
x > 35
```
``` r
x <- c(10, 20, 30, 40, 50)
tf <- x > 35
x[tf]
```
The result will be the same if we directly use the logical condition inside the brackets.
``` r
x <- c(10, 20, 30, 40, 50)
x[c(FALSE, FALSE, FALSE, TRUE, TRUE)]
```
but this is not usually how we do it, because we can calculate the logical vector with condition `x > 35` and use it directly as index. 

!!! note "Technical note"
    The logical index should have the same length as the vector we want to subset, but R will handle this for us. If we use shorter logical vectors, R will recycle the values.

    ``` r
    x <- c(10, 20, 30, 40, 50)
    i <- c(TRUE, FALSE, TRUE)
    x[i]
    ```

Let’s get back to the line labels (`[1]`) that appear when values are printed in the console.  
The number actually shows the index (position in a vector) of the first element printed on that line.

Try executing the sequence `1:100` to see how line labels in the console work:

```r
x <- 25:125
x
```

The nineteenth element of the sequence is `43`, which is indicated by the line label `[19]`, where `43` is printed on first position of that line.

```r

x[19]

```
```
[1] 43
```

## Summary

### Main outcomes
- understand basic data types in R
- what is vector, how to create vectors with multiple values including `c()` function and `:` operator
- how to do basic operations with vectors
- what is index and how to subset data with `[ ]`

### Function overview
- `typeof()` - type of the object
- `as.*()` - conversion between data types
- `c()` - combine values into vector with multiple values (*elements*)
- `length()` - length of vector
- `min()`,`max()`, `mean()`, `sum()` - calculate descriptive statistics of numeric vector
