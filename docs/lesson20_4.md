
## Lists
Lists are objects that are used as collection of objects, which can be different types. They are also vectors but *non-atomic*. Can be created with `list()` function. 

``` r
my_list <- list(1, "a", TRUE)
my_list
```

list may contain also other lists:
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

### Subsetting lists
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


### Named lists
If the list is named, we can subset it with the name of the element:
``` r
my_list <- list(number = 1, text = "a", boolean = TRUE)
my_list
```

``` r
my_list$number
[1] 1   
```
``` r
my_list["number"]

[1] 1
```


## `apply` family of functions

The `apply` family of functions is used to **apply** a function to each element of an object (like a list or a matrix). The most common functions are `apply` for arrays, matrices and data frames, and `lapply` for lists (or simplified `lapply` - `sapply`, which returns vector).

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


## `do.call`

!!! todo





## which(), match(), %in% , `do.call`