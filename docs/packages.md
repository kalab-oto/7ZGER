!!! warning
    This page is in state from the spring semester 25, and will undergo changes for the actual semester.
# Packages

Until this point, we relied in the in-built R functions (*Base R*), but in real world you will use function and classes from R extensions called **packages**. 

There are thousands of packages available on official repository called [CRAN](https://cran.r-project.org/), but also on other repositories like [Bioconductor](https://www.bioconductor.org/), or git repositories (i.e. on [GitHub](https://www.github.com), [GitLab](https://gitlab.com), [Codeberg](https://codeberg.org), ...), or you can install packages directly from files.

In the course we will use mainly these two packages:

- `sf` - for vector spatial data manipulation
- `terra` - mainly for raster spatial data manipulation, but can handle also for vector data

In some lesson we will also use these packages:

- `ggplot2` - for plotting
- `geodata` - for downloading spatial data
- `Rczechia` of `CzechData`- for Czech Republic spatial data

!!! note
    We will also mention `sp`,`raster` packages. They are predecessors of `sf` and `terra` packages respectively, now **deprecated**, but you can still encounter them in some projects 

You can find more packages used in spatial analysis in [Spatial Task View](https://cran.r-project.org/web/views/Spatial.html) on CRAN.

## Installing packages

Installation of packages is easy, and can be don from R console with function `install.packages()` or with dedicated button in  GUI which runs this function fro you. 

Installation of `sf` package:

```r
install.packages("sf")
```

## Using packages

Once installed, package can be loaded in any session using `library()` function. 

Loading `sf` package:

```r
library(sf)
```

When writing scripts, it is good practice to load all packages at the beginning of the script.

!!! info
    Sometimes you can also see use of `require()` function, which is similar to `library()`, but it returns `TRUE` or `FALSE` if the package is loaded or not. And also use of `::` operator, which is used to call functions from package without loading it, like `sf::st_read()` for using `st_read()` function from `sf` package without loading it.