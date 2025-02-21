# Introduction to R

## What is R?
- R is a programming language for statistical computing but...
- ... you can use it without any knowledge of statistics - to do data manipulation, data visualization, and more (file 
  handling, automatization, web scraping, ...)
- it is open-source, free and multiplatform
- [https://cran.r-project.org/](https://cran.r-project.org/)
- R can be used in two ways: interactively in command line or the code can be saved in scripts - files 
  with code (`.R`, `.r`)
- the scripts can be written in **any text editor**, but there are specialized software to make writing and running code
  easier: IDEs (Integrated Development Environment)
- the most popular IDE for R is **RStudio** - 
  [https://posit.co/download/rstudio-desktop/](https://posit.co/download/rstudio-desktop/) but see 
  [Positron](https://positron.posit.co/) (IDE based on VS Code)
- other IDEs used for R: VS Code, Vim, Emacs, *Jupyter Notebook* ...
- so **R** ≠ **RStudio**

## Why programming for spatial data handling?
- why write programs/scritps when I can already can do the same in Excel, QGIS, ArcGIS, ... ?
- practically, you can do the same, but with programming you writing the "reciepe" how to do it, which can leads to:
  
    - **reproducibility** - run the code again, share, collaborate when coding
    - **flexibility** - do the same task with different data, change/improve parts of the workflow anytime

!!! example 

    Imagine this GIS task: *Get mean evelation for 1 km area around occurrence of specific species A*. So the task will be 
    something like:

      1. read point vector data (shapefile, geopackage, ...)
      2. retrieve only points with specific attribute (species is `species A`)
      3. do 1 km buffer around the points
      4. read the raster data (DEM)
      5. extract raster values for the buffer areas, and calculate the mean
      6. write it to a file

    ??? example " But than..."
        ... what if you need to make same changes in workflow? Like:

        - do this for all other species in the dataset ...
        - ... and for multiple buffer distances ...
        - and for other 100 raster tiffs in the direcotry or on servers (monthly temperatures, percipitation, pressure, ...)

    You can also do this in desktop applications, but it will be time consuming and inefficient. With writing the workflow 
    in R, you can easily change the parameters, set multiple values run it on multiple datasets, with minimal effort 
    compared to manual work.

## R syntax
- no strinct syntax rules, and naming conventions
- many ways to do the same thing
- many packages for different tasks 
- many packages for same tasks
- code styles /"grammar" - **base R**, tidyverse, data.table, 

## Installation of R 
- Download R from [https://cran.r-project.org/](https://cran.r-project.org/)
    -  choose your system (Windows, macOS or Linux) and follow the instruction
    -  for windows: choose *base* distribution, than click *Download R-4.x.x for Windows* and follow the installation 
       wizard

After installation, you can run R with inbuild IDE *Rgui* on directly in terminal. But in the course we will use 
RStudio, and I recommend this to all beginners.

## Rstudio

RStudio is the most popular IDE and *de facto* standard for R programming. It is free and open-source, and you can 
download it from [https://rstudio.com/products/rstudio/download/](https://rstudio.com/products/rstudio/download/). 
Just download the installer for your system and follow the installation wizard. Than you can run RStudio and start
coding. 

!!! note

    You have to have R installed before you install RStudio.

We won't go too much into details for now, and just show the basic interface and useful settings for start. Other settings 
will be covered during the course.

### Interface

The GUI consists of four main panes, which can be customized and rearranged. But for now we will focus on two of them:

- **Console** - here you can write and run code interactively
- **Source** - here you can write and save your scripts (files with code), and also send the parts of the code to the 
  console

!!! note

    The R is always running in specific directory, which is called *working directory*. You can show the directory with 
    `getwd()`, or change the working directory with the function `setwd()`. *Working directory* is important when you 
    want to work with paths or files. You can use absuloute paths, but better practice is to use relative paths to the 
    *working directory*.
 