# Packages

## Class

In R each object belong to some **Class**. The class of an object defines how the object behaves and how it looks like. So, what you can do with an object depends on objetcs class. Some operations are defined for some classes, but not for others. For example double and integers are both numeric classes, and thus you can do mathematical operations with them, but you cannot do mathematical operations with character class.

We will not go into details of classes, but it is important to know that there are different classes for spatial data. For example, when working with vector spatial data, the `sf` package defines `sf` class, and `terra` package defines `SpatVaster` class, each of them have different approach to handle the same data.

## Packages
Until this point, we relied in the in-built R functions (*Base R*), but in real world you will use functionality from R extensions called **packages**. 

There are thousands of packages available on official default R repository called [CRAN](https://cran.r-project.org/). 

!!! note
    Packages can be also installed from other repositories like [Bioconductor](https://www.bioconductor.org/), or git repositories (i.e. on [GitHub](https://www.github.com), [GitLab](https://gitlab.com), [Codeberg](https://codeberg.org), ...), or you can install packages directly from files.

In the course we will use mainly these two packages:

- `sf` - for vector spatial data manipulation
- `terra` - mainly for raster spatial data manipulation, but can handle also for vector data

!!! note
    We will also mention `sp`, `raster` packages. They are predecessors of `sf` and `terra` packages respectively, now **deprecated**, but you can still encounter them in some projects 

You can find more packages used in spatial analysis in [Spatial Task View](https://cran.r-project.org/web/views/Spatial.html) on CRAN.

## Installing packages

Installation of packages is easy, and can be don from R console with function `install.packages()` or with dedicated button in  GUI which runs this function fro you. 

Installation of `sf` and `terra` packages:

```r
install.packages("sf")
install.packages("terra")
```

or

```r
install.packages(c("sf", "terra"))
```

## Using packages

Once installed, package can be loaded in any session using `library()` function. 

Loading `sf` package:

```r
library(sf)
```

When writing scripts, it is good practice to load all packages at the beginning of the script.

!!! note "Technical note"
    Sometimes you can also see use of `require()` function, which is similar to `library()`, but it returns `TRUE` or `FALSE` if the package is loaded or not. And also use of `::` operator, which is used to call functions from package without loading it, like `sf::st_read()` for using `st_read()` function from `sf` package without loading it. Also the `::` operator is used to declare from which package the function is, when there are functions with the same name in the environment (R base or from different packages).