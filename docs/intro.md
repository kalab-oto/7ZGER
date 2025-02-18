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
  [https://posit.co/download/rstudio-desktop/](https://posit.co/download/rstudio-desktop/)
- other IDEs: Visual Studio Code, Jupyter Notebook, Vim, Emacs, and recently [Positron](https://positron.posit.co/) (IDE based on VS Code)
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

### Project workflow

Following settings are not necessary at the beginning, but is good to know them and avoid building a bad habits.

- **.RData workspace** - R can save the entire workspace (all objects, variables, functions, ...) to the `.RData` file, which can be 
  restored. Don't use the workspace. Better practice is write your scripts in a way that they recreate the worksapce.
    - option to set: *Tools -> Global options* -> *General/basic* tab: *Workspace* - Restore .RData - **uncheck**; .RData on exit **Never**
- **text encoding** - use UTF-8 when saving files.
    - option to set: *Tools -> Global options* -> *Code/Saving* tab -> Default text encoding: **UTF-8**
- **Working directory** - the directory where the R console is launched. This is important when you want to work with files. 
  The working directory can be confortably changed with the function `setwd()`, but its not so good practice, and its 
  better to make habit to work with relative paths in *project directory* based workflow. This ensures that the code will
  work without changes if you move the project direcory to another place, renaming part of path, use other IDE, use Git etc. 
  In RStudio you can use for this purpose the *Project* feature, which creates a project file `.Rproj` in the project 
  directory. Loading `.Rproj` automatically set the working directory to the project directory.
    - no option to set, jsut keep in mind that you are working in project directory, use relative paths while 
      working with files and save `.Rproj` (if you use RStudio)
      file in the project directory
    - creating project - *File -> New Project...*
    - opening existing project - *File -> Open Project...* or *File -> Recent Projects*
    - RStudio opens last project by default, but you can change this in *Tools -> Global options* -> *General/basic* 
      tab: *Default working directory* - disable *Restore most recently opened project at startup*
  