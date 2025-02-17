# R Basics

## R as a calculator

``` r
1 + 1
```

```
[1] 2
```

use `**` or `^` for exponentiation

``` r
5 * 5 / (3 + 10)
5**2/ (3 + 10)
```

### Comparsion

You can also test if values meets some condition

- `==` - equals
- `!=` - not equals
- `>` - greater than
- `<` - less than
- `>=` - greater or equal
- `<=` - less or equal

``` r
5 == 5
```

```
[1] TRUE
```

try:
``` r
5 > 10
6 != 6
10 <= 10
8 > 2 * 3
```

## Variables
Values and objects can be stored in variables with assigment operator `<-` (or `=`), but `<-` is more common, and will be used in this course. Calling the variable name will return the value stored in it.

``` r
x <- 42

x
```

```
[1] 42
```

``` r
x <- 7
y <- 6

y * x
```

Assigning a value to existing variable will overwrite the previous value.

``` r
x <- 7
y <- 6
x <- 2

y * x
```

try:
``` r 
x <- 5
y <- 10
z <- x + y
z
```

Variables names:

- can't contain spaces and special characters
- can't start with number
- are case sensitive
- can't be some reserved words (`if`, `for`, ...)

The convention is to use descriptive but not excessive long variable names. There are tree main types of naming in R:

- snake_case - used in this course
- camelCase
- dot.case

Use what you like, but be consistent.

``` r
my_variable <- 5
1st_variable <- 5

my_variable
1st_variable
``` 
 
## Functions
Calling function consist of function name and arguments in parentheses. Arguments can be named or unnamed. 
`function_name(argument1 = value1 ,argument2 = value2, argument3 = value3, ...)`. Some functions doesn't have
 any arguments (`getwd()`).

``` r
sin(5)
sqrt(25)
```
You can get function help using RStudio GUI or calling `?` before the function name, or `help()` function.

``` r 
?sqrt
```

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
# Writing script
<!-- todo -->