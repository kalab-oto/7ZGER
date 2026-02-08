# R Basics

## R as a calculator

First steps we can do is to use R as a calculator. Type the code directly in the Console pane where `>` is shown, and press `Enter` to run (execute) it. The `>` sign, is promt, that R is ready to accept commands. 

``` r
1 + 1
```

The result will be printed below the command:

```
[1] 2
```

The `[1]` is just console line label, which is useful when printing objects containing more values, which will be explained later.

So you should see this in your console:

``` r
> 1 + 1
[1] 2
```


Use `**` or `^` for exponentiation

``` r
5 * 5 / (3 + 10)
5**2 / (3 + 10)
```
## Relational operators
You can also compare values to test whether they meet a condition. This returns a logical value, `TRUE` or `FALSE`.

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
Values and other objects can be assigned to variables with operator `<-`. Executing the variable name is call *printing* and will print the value assigned in it. Imagine variables as links to the stored values.

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

The result of any operation can be assigned to a variable:

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

- **snake_case** - used in this course
- camelCase
- dot.case - not recommended

Use what you like, but be consistent.

``` r
my_variable <- 5
my_variable
``` 
 
## Text values

We will also work with text values, which are written in quotes, either single `'text'` or double `"text"`.

``` r
my_text <- "Hello World"
my_text
```

```
[1] "Hello World"
```
Text values can be also compared with relational operators and they will be compared alphabetically:

``` r
a <- "A"
b <- "B"
a == b
a < b
a == "a"
```

## Functions
Calling function consist of function name and usually arguments in parentheses. Arguments can be named or unnamed. 
`function_name(argument1 = value1 ,argument2 = value2, argument3 = value3, ...)`.  There are also functions that doesn't have any arguments (`getwd()`).

Try functions `round()` and `abs()`:
``` r
round(3.14159)
```

You can get function help using RStudio GUI or calling `?` before the function name, or `help()` function. Help gives you information about the function, its arguments and their default values, and usage examples.

``` r 
?round
```

In help we found the argument `digits`, try it:
``` r
round(3.14159, digits = 2)
```

or just
``` r
round(3.14159, 2)
```

!!! note
    Actually the function runs
    ``` r
    round(x = 3.14159, digits = 2)
    ```
    where `x` is the first unnamed argument.


Another simple function is `abs()` - absolute value:

``` r
abs(-16)
```

## Writing scripts

As mentioned in the [Introduction to R](intro.md) there are various ways to write and run code in R, **interacively** in the console, or writing **scripts** that can be saved and run later (entire or by parts). Most of the time you will write code to scripts (`.R` or `.r` files). Code can be written in any text editor.

### Runing code

In RStudio you can write and save scripts in the **Source** pane. You can run the entire script or just the selected part of the code. 

- **Run selected part of the code** - `Ctrl + Enter` - or button `Run` - this you willl use mostly in this course. You can run the code line by line, or by selected blocks. This function sends the code to the console and run it, and the cursor will move to the next line or executable part of code.


### Comments

In writing scripts you can "comment" lines. Commented lines starts with `#` and are not executed (even if they are sent to the console). Coments are useful for documenting/explanation code or "deactivating" parts of code.

``` r
# Do some math
x <- 5
y <- 8
# y <- 10
y
x + y
```
