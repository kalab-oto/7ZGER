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
5**2 / (3 + 10)
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


``` r
variable1 <- 5
variable1
``` 

``` r
1variable <- 5
1variable
``` 
The convention is to use descriptive but not excessive long variable names. There are tree main types of naming in R:

- snake_case - used in this course
- camelCase
- dot.case

Use what you like, but be consistent.

``` r
my_variable <- 5
my_variable
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

# Writing script
<!-- todo -->
