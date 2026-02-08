# Reading and writing files, project workflow
In this section we learn how to work with simplistic project workflow, and how to read and write processed files.

## Project workflow
The R is always running in specific directory called **working directory**. *Working directory* is important when you want to work with paths or files. You can use absuloute paths, but better practice is to use relative paths to the *working directory*. You can show the path to *working direcotry* with function `getwd()`, or change the path with the function `setwd()`, but its not so good practice, and its better to make habit to work with relative paths in *project directory* based workflow. This ensures that the code will
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

!!! tip
    You can create this structure in RStudio by creating a new project and than creating the directories incliding files (scripts, etc.) in the *Files* pane.

### RStudio setting

- **.RData workspace** - R can save the entire workspace (all objects, variables, functions, ...) to the `.RData` file, which can be 
  restored. I recommend to don't use the workspace, and write your scripts in a way that they recreate the worksapce.
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

## First project
In this example project we write one script that reads the data, makes some simple processing and writes the output 
to new file. We will work with export from DRUSOP which is digital register of central list of nature conservation 
in the Czech Republic.

1. Create a new project and directory (*first_project* or name it as you want) - this can be done in RStudio
2. Create subdirectories in *data*, *scripts* and *outputs* - just creating directories
3. Create a new script file *first_script.R* in the *scripts* directory - just creating file
4. Download the data file
    - go to the [https://drusop.nature.cz/](https://drusop.nature.cz/) and select *Maloplošná zvláště chráněná území* in
     the **Objekty ústředního seznamu** section.
    - click the *Export* button, - check all with *Označ/Zruš VŠE na stránce*, select *Excel (CSV)* format in *Formát*,
     *UTF-8* encoding in *Kódování*, and click export *Exportovat*. This will download the file *export.csv*. Move this
     file to the *data* directory.

**Aim of the project**:

Perform some basic exploration of the data, and create a `.csv` table only with protected areas with area larger than 500 ha, declared from the 2020.

### Dataset
The dataset contain information about smaller specially protected areas in 4 categories: national nature reserves (NPR), nature reserves (PR), national natural monuments (NPP), and natural monuments (PP)

### Reading data and general exploration
Our file is `.csv` file, which is simple data structure for storing tables in simple text file, see [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) on Wikipedia. You can open `.csv` in common spreadsheet editor such as MS Excel or Libre Oficce Calc. In R we can read it with the function `read.csv()`. As an argument, we pass the path to the file. The path can be absolute or relative, and it can be also url pointing to the file on some server. 

``` r
df <- read.csv("data/export.csv")
```
Now we have the data stored in the `df` object, which is `data.frame` object. Now you need to check if the data are read correctly (which is not always the case). You can show the data with calling `df` as any other object:

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

Now we can see that the data are not read correctly. We have `2681` rows (*observations*) and only 1 column (*variable*).
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

### Exploring and cleaning data
Now when we correctly read the data, we can explore them. We did some exploration with the `str()` function, 
but in following exercises we will do some further exploration.


This is not some exploration routine, we just show in this section common cases you will encounter while processing data, so you learn how to solve these basic problems.


<!-- #### Is there any protected area twice in the data?

At first its good to know some nature of the data, for example if the data strictly represent single protected area in each row, e.g. if there are no duplicates.

We can check this by many ways. We know that every monument has its unique code `Kód`, so we can check if there are any duplicates in the `Kód` column. We can do this by many ways, but we will show two simple ways:

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

So now we know that each protected area is represented by single row. -->

#### Dealing with `NA` - What is the mean area of the protected areas?

=== "code"

    ``` r
    mean(df$Rozloha..ha)
    ```
    
    returns `NA`, which indicates that there are some `NA` values in the data. 
    
  
    !!! tip
        This can be further explored by the `is.na()` function, which returns logical vector of `TRUE` and `FALSE` values. This can be simply checked be `unique()` function (show unique values of vector), or summarized with `table()` function (counts the unique values of vector).

        ``` r
        table(is.na(df$Rozloha..ha))
        unique(is.na(df$Rozloha..ha))
        ```

=== "solution"
    We can use the `na.rm` argument to remove the `NA` values from the calculation:

    ``` r
    mean(df$Rozloha..ha, na.rm = TRUE)
    ```

#### Explore the `NA` value of the `Rozloha..ha` column
We know that we can perform some calculation ommiting the na with the `na.rm` argument, but its good to know what is the `NA` value in the data.

We know that we can return logical vector of `TRUE` and `FALSE` values with the `is.na()` function, and further use this vector to subset the data frame, returning only the row with `NA` value in the `Rozloha..ha` column: 

``` r
df[is.na(df$Rozloha..ha),]
```
Which returns line `2681`, and as we know the `df` have `2681` rows (*observations*), so its the last row of the data.

Its clear that the last row is not a valid observation, and in this case its kind of metadata from data provider, so it should be cleaned up from the data.

##### Removing the row

In this case there are many ways to remove the row. Here are some simple examples:

- simpy with the `[-2681]` notation, where `-` mens to exclude the row with index `2681`:

``` r
df[-2681, ]
```
- with the subset of using `is.na()` function, but using `!` operator to negate (*reverse*) the logical vector, so it performs the opposite operation - get all rows where `Rozloha..ha` is not `NA`:

``` r
df[!is.na(Rozloha..ha), ]
```

- with the subset of string of some column

``` r
df[df$Kód != "COUNT:", ]
```

Choose the one you like the most, and assign it to the `df` object:

``` r
df <- df[!is.na(df$Rozloha..ha), ]
```

#### Dealing with Dates
There are few columns with dates in the data, for example `Datum.prvního.vyhlášení`. This column is the `character` data type, but if we want to work with dates, we have to convert it for further analyssis like counting the number of protected areas with certain year of declaration, etc.

We can convert the column to the `Date` class with the `as.Date()` function. Or just retrieve the desired information (year) with the `substr()` function.

We need to know how the dates looks like, so we list first, or first few dates:

```r
df$Datum.prvního.vyhlášení[1:10]
class(df$Datum.prvního.vyhlášení[1:10])
```

So this date is formatted as `dd.mm.yyyy` - `day.month.year`, so we can use the `as.Date()` function with the `format` argument with `"%d.%m.%Y"` to convert the column to the `Date` class (in `YYYY-MM-DD` format):

``` r
as.Date(df$Datum.prvního.vyhlášení[1:10], format = "%d.%m.%Y")
class(as.Date(df$Datum.prvního.vyhlášení[1:10], format = "%d.%m.%Y"))
```

If we have task to find the newest protected area, we can use the `max()` function on the converted column:

``` r
max(df$Datum.prvního.vyhlášení) # nonsense, its character - text
max(as.Date(df$Datum.prvního.vyhlášení, format = "%d.%m.%Y"))
```

Converting column to the `Date` class is just replacing the values with the new values in `Date` class:

``` r
df$Datum.prvního.vyhlášení <- as.Date(df$Datum.prvního.vyhlášení, format = "%d.%m.%Y")
max(df$Datum.prvního.vyhlášení)
```
Now we can filter the data with the date, for example to get the protected areas declared after 2020:

``` r
d <- as.Date("2020-01-01")
df[df$Datum.prvního.vyhlášení > d,]
```

#### Other exploration
##### Get the protected areas with area larger than 500 ha and category NPR

``` r
df[df$Rozloha..ha > 500,]
```

##### Get the protected areas with area larger than 500 ha and category NPR
``` r
df[df$Kategorie == "NPR",]
```
##### What is the total area of the protected areas?
##### Which protected area is the largest?
##### Which category has the largest protected area?
##### How many protected areas are in each category?

### Writing the output
As we cane read the `.csv` file, we can also creating/write it. We can use the `write.csv()` function from the `write.` family of functions. The `write.csv()` function writes the `data.frame` to the `.csv` file. 


``` r
write.csv(df, "outputs/processed_data.csv", row.names = FALSE)
# we use the row.names = FALSE to not write the row names to the file
```

If the task was: Create export of the protected areas with area larger than 500 ha and category NPR, the entire script can look like this:

``` r
df <- read.csv("data/export.csv", sep = ";", dec = ",")

df <- df[!is.na(df$Rozloha..ha), ]
df$Datum.prvního.vyhlášení <- as.Date(df$Datum.prvního.vyhlášení, format = "%d.%m.%Y")

d <- as.Date("2020-01-01")
df <- df[df$Datum.prvního.vyhlášení > d,]

df <- df[df$Rozloha..ha > 500,]
df <- df[df$Kategorie == "NPR",]

write.csv(df, "outputs/processed_data.csv", row.names = FALSE)
```

or using the "and" operator `&`:

``` r
df <- read.csv("data/export.csv", sep = ";", dec = ",")

df$Datum.prvního.vyhlášení <- as.Date(df$Datum.prvního.vyhlášení, format = "%d.%m.%Y")
d <- as.Date("2020-01-01")
df <- df[!is.na(df$Rozloha..ha) & df$Rozloha..ha > 500 & df$Kategorie == "NPR" & df$Datum.prvního.vyhlášení > d, ]

write.csv(df, "outputs/processed_data.csv", row.names = FALSE)
```
## Practice exploration
Practice some exploration on the Zoraptera Occurrence Dataset (https://zenodo.org/records/14652555). The dataset contains information about the occurrence of the order Zoraptera. The link directly to dataset is "https://raw.githubusercontent.com/kalab-oto/zoraptera-occurrence-dataset/refs/tags/1.1.0/zoraptera_occs.csv"


---
=== "task 1"
 
    Read the data, and assign them to the `zoraptera` object
  
=== "solution 1"
 
    ``` r
    zoraptera <- read.csv("https://raw.githubusercontent.com/kalab-oto/zoraptera-occurrence-dataset/refs/tags/1.1.0/zoraptera_occs.csv")
    ```

---
=== "task 2"
 
    How many observations and variables are in the data?


=== "solution 2"
 
    There are `656` rows (*observations*) and `41` columns (*variables*)

---
=== "task 3"
 
    Is there `species` column in the data?

=== "solutfion 3"
 
    There is no `species` column

---


=== "task 4"
 
    How many records are not identified to the species level?
    
=== "solutfion 4"
 
    `581`

---

=== "task 5"
 
    What is the most common species in the data?

=== "solutfion 5"
  
      `Usazoros hubbardi`

---

=== "task 6"
    
    What is the most common `country` in the data?

=== "solution 6"

    `United States of America`

---

=== "task 7"
    
    How many records are from the `Fiji`?

=== "solution 8"

    `9`
