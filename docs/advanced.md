# Optional and advanced topic

``` r
df <- read.csv("data/export.csv", sep = ";", dec = ",")
df <- df[!is.na(df$Rozloha..ha), ]
```
## Dealing with Dates
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
