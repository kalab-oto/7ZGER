# Reading and writing files, project workflow
In this section we learn how to work with simplistic project workflow, and how to read and write processed files.

## Project workflow
The directory where the R console is launched is called **Working directory**. This is important when you want to work with files. 
The working directory can be confortably changed with the function `setwd()`, but its not so good practice, and its 
better to make habit to work with relative paths in *project directory* based workflow. This ensures that the code will
work without changes if you move the project direcory to another place, renaming part of path, use other IDE, use Git etc. 

While wokring with the data in R, it is good practice to:
1. **one directory** - keep all files (data, scripts, outputs) in one directory (*project* or *workspace* directory) and run R from this 
   directory (*working directory*)
2. **automate as much as possible** - write scripts that  reproduce the workflow, without the need to store some workspace files (`.RData`). This includes
   reading data, processing, writing outputs etc. 

!!! note
    This is not necessary for working with R, but it is good practice to avoid building bad habits. In some cases, you
    will need to use absolute paths, i.e. when starting R project and you have dedicated directory for some
    general data which are often used. But try to avoid it as much as possible, since this disables the possibility of sharing the project.

### RStudio setting

- **.RData workspace** - R can save the entire workspace (all objects, variables, functions, ...) to the `.RData` file, which can be 
  restored. Don't use the workspace. Better practice is write your scripts in a way that they recreate the worksapce.
    - option to set: *Tools -> Global options* -> *General/basic* tab: *Workspace* - Restore .RData - **uncheck**; .RData on exit **Never**
- **text encoding** - use UTF-8 when saving files.
    - option to set: *Tools -> Global options* -> *Code/Saving* tab -> Default text encoding: **UTF-8**
- **Working directory** -  use the *Project* feature, which creates a project file `.Rproj` in the project 
  directory. Loading `.Rproj` automatically set the working directory to the project directory.
    - no option to set, jsut keep in mind that you are working in project directory, use relative paths while 
      working with files and save `.Rproj` (if you use RStudio)
      file in the project directory
    - creating project - *File -> New Project...*
    - opening existing project - *File -> Open Project...* or *File -> Recent Projects*
    - RStudio opens last project by default, but you can change this in *Tools -> Global options* -> *General/basic* 
      tab: *Default working directory* - disable *Restore most recently opened project at startup*


### Setting *project* directory

When starting, just create a directory for your project, move data files to the *data* directory, and create scripts in the *scripts* directory. Example of simple project directory structure:

```
project_name/
├── data/
├── scripts/
├── outputs/
```

- *data/* - directory for all input data (`.csv`, `.shp`, `.gpkg`,`.tif`, ...)
- *scripts/* - directory for R scripts
- *outputs/* - directory for output files - plots, tables, ...

## First project
In this example project we write one script that reads the data, makes some simple processing and writes the output 
to new file. We will work with export from DRUSOP which is digital register of central list of nature conservation 
in the Czech Republic.

1. Create a new project and directory (*first_project* or name it as you want) - this can be done in RStudio
   - *File -> New Project...*. Choose *New Directory*, *New Project*, and name the project. This will create a new 
     directory with the `.Rproj` file in it.
2. Create subdirectories in *data*, *scripts* and *outputs* - just creating directories, this can be done in file 
   mnanager as usual or directly in RStudio 
3. Create a new script file *first_script.R* in the *scripts* directory - just creating file, this can be done in file 
   mnanager as usual or directly in RStudio. If you create it in RStudio, it will be automatically opened in the editor.
   Otherwise, open it in RStudio by *File -> Open File...*
4. Download the data file
   - go to the [https://drusop.nature.cz/](https://drusop.nature.cz/) and select *Maloplošná zvláště chráněná území* in
     the **Objekty ústředního seznamu** section.
   - click the *Export* button, - check all with *Označ/Zruš VŠE na stránce*, select *Excel (CSV)* format in *Formát*,
     *UTF-8* encoding in *Kódovaání*, and click export *Exportovat*. This will download the file *export.csv*. Move this
     file to the *data* directory.

**Aim of the project**:


### Dataset
The dataset contain information about smaller specially protected areas in 4 categories: national nature reserves (NPR), nature reserves (PR), national natural monuments (NPP), and natural monuments (PP)

### Reading data
Our file is `.csv` file, which is simple data structure for storing tables in simple text file, see [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) on Wikipedia. You can open `.csv` in common spreadsheet editor such as MS Excel or Libre Oficce Calc. In R we can read it with the function `read.csv()` from `read.` family of functions.

``` r
df <- read.csv("data/export.csv")
```
Now we have the data stored in the `df` object, which is `data.frame` object. Now you need to check if the data are read  correctly (which is not always the case). You can show the data with calling `df` as any other object:

``` r
df
```
But as you see this is not ideal. There are two functions to explore the data - `head()` and `str()` (from previous section).

`head()` returns a `data.frame` with the first 6 rows of the original data frame:

``` r
head(df)
```

Now we can se that the data is messy. Better way to explore the data is to use `str()` function, which returns the 
structure of the data frame, including rows and columns count, columns data types and names, and example of first values in the columns

``` r
str(df)
```

!!! tip
    The `str()` and `head()` function are the most used functions for exploring the data. You can also use `summary()` function. These function are usualy executed directly in the console, but you can also use them in the script.

Now we can see that the data are not read correctly. We have 2681 rows (*observations*) and only 1 column (*variable*).
Evidently this is caused by reading data with wrong separator, because the `read.csv()` function uses comma (`,`) as 
default separator, but the data are separated by semicolon (`;`).

This and many ohter parameters can be set in the `read.csv()` function as arguments, see `?read.csv` for help.

To set the separator, we can use the `sep` argument:

``` r
df <- read.csv("data/export.csv", sep = ";")
str(df)
```

Strucutre of the data now looks better but still not ideal - check the column `Rozloha..ha` which have data type `chr` (*character*) but should be `num` (*numeric*). This is caused by the decimal separator, which is comma (`,`) instead of dot (`.`). This can be set in the `read.csv()` function with the `dec` argument:

``` r
df <- read.csv("data/export.csv", sep = ";", dec = ",")
str(df)
```

### Exploring data
Now when we correctly read the data, we can explore them. We did some xploration with the `str()` function, but now we can do some more advanced exploration. 

**Is there any protected area twice in the data?**
We can check this by many ways. We know that every monument has its unique code `Kód`, so we can check if there are any duplicates in the `Kód` column. We can do this by many ways, but we will show two ways

- using the `duplicated()` and `sum()` - `duplicated()` returns logical vector of duplicated rows (vector of `TRUE` or `FLASE` values), and `sum()` counts the number of `TRUE` values:

``` r
dupl_kod <- duplicated(df$Kód)
sum(dupl_kod)
# or simply
sum(duplicated(df$Kód))
```
- using the `unique()` and `length()` - `unique()` returns unique values in the vector, and `length()` counts the number of unique values, so if length of unique values is same as length of the original data, there are no duplicates:

``` r
uniq_kod <- unique(df$Kód)
length(uniq_kod)
# or simply
length(unique(df$Kód))
length(df$Kód) # length of original data
```

**What is the mean area of the protected areas?**

``` r
mean(df$Rozloha..ha)
```


**Which protected area is the largest?**

**Which category has the largest protected area?**

**How many protected areas are in each category?**




### Resulting script
``` r
df <- read.csv("data/export.csv", sep = ";", dec = ",")
```
