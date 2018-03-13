<!--
-   [1, Importing data with `readr`](#importing-data-with-readr)
-   [2, Parsing Data with `readr`](#parsing-data-with-readr)
-->
------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

Importing data with `readr`
------------------------------

**Reading a .csv file**

Your first task will be to master the use of the `read_csv()` function. There are many arguments available, but the only required argument is `file`, a path to a CSV file on your computer (or the web).

One big advantage that `read_csv()` has over `read.csv()` is that it doesn't convert strings into factors by default.

`read_csv()` recognizes 8 different data types (integer, logical, etc.) and leaves anything else as characters. That means you don't have to set `stringsAsFactors = FALSE` every time you import a CSV file with character strings!

``` r
#install.packages('readr')
library(readr)

getwd()
```

    ## [1] "D:/.../Rprojects/Data Wrangling"

``` r
setwd("D:/.../Rprojects/Data Wrangling")
```

Import .csv (only ',').

``` r
# Import chickwts.csv: cwts
cwts <- read_csv('chickwts.csv')

# View the head of cwts
head(cwts)
```

    ## # A tibble: 6 × 2
    ##   weight      feed
    ##    <int>     <chr>
    ## 1    179 horsebean
    ## 2    160 horsebean
    ## 3    136 horsebean
    ## 4    227 horsebean
    ## 5    217 horsebean
    ## 6    168 horsebean

**Reading a (.txt) .tsv file**

Skipping columns with `col_skip()`.

Code only:

-   Setting the column type.

<!-- -->

``` r
cols(
  weight = col_integer(),
  feed = col_character()
)
```

-   Setting the column names.

<!-- -->

``` r
col_names = c('name', 'state', 'phone')
```

-   Removing NA.

<!-- -->

``` r
na = c('NA', 'null')
```

In practice.

``` r
# Import data
salaries <- read_tsv('Salaries.txt', col_names = FALSE, col_types = cols(
  X2 = col_skip(),
  X3 = col_skip(), 
  X4 = col_skip()
))

# View first six rows of salaries
head(salaries)
```

    ## # A tibble: 6 × 3
    ##          X1    X5     X6
    ##       <chr> <chr>  <int>
    ## 1      Prof  Male 139750
    ## 2      Prof  Male 173200
    ## 3  AsstProf  Male  79750
    ## 4      Prof  Male 115000
    ## 5      Prof  Male 141500
    ## 6 AssocProf  Male  97000

**Reading a European .csv**

In most of Europe, commas (rather than periods) are used as  decimal points.

``` r
# Import data with read_csv2(): trees
trees <- read_csv2('trees.csv')

# View dimensions and head of trees
dim(trees)
```

    ## [1] 9 3

``` r
head(trees)
```

    ## # A tibble: 6 × 3
    ##   Girth Height Volume
    ##   <dbl>  <int>  <dbl>
    ## 1    83     70    103
    ## 2    86     65    103
    ## 3    88     63    102
    ## 4   105     72    164
    ## 5   107     81    188
    ## 6   108     83    197

**Read a fixed-width file**

Files containing columns of data that are separated by whitespace and all line up on one side.

Code only:

``` r
# Import names.txt: names
names <- read_table('names.txt', col_names = c('name', 'state', 'phone'), na = c('NA', 'null'))
```

**Reading a text file**

Import ordinary text files.

``` r
# vector of character strings. 
# Import as a character vector, one item per line: tweets
tweets <- read_lines('tweets.txt')
tweets
```

    ## [1] "carrots  can be eat by most people"                                                                                          
    ## [2] "On predisents day we honor the big US man himself: Aberham Liclon.   Tall, skinny, dry, and cruncy - he was america's carrot"
    ## [3] "knock knoc who is there? yup: carosot   ( joke )"                                                                            
    ## [4] "it is 2016 time for a carot emoji   please!"                                                                                 
    ## [5] "when life give you lemnos ,  have a carrot"                                                                                  
    ## [6] "If you squent your eyes real hard a football  look like  a dry brown carrot   Honestly"

``` r
# returns a length 1 vector of the entire file, with line breaks represented as \n
# Import as a length 1 vector: tweets_all
tweets_all <- read_file('tweets.txt')
tweets_all
```

    ## [1] "carrots  can be eat by most people\r\nOn predisents day we honor the big US man himself: Aberham Liclon.   Tall, skinny, dry, and cruncy - he was america's carrot\r\nknock knoc who is there? yup: carosot   ( joke )\r\nit is 2016 time for a carot emoji   please!\r\nwhen life give you lemnos ,  have a carrot\r\nIf you squent your eyes real hard a football  look like  a dry brown carrot   Honestly"

**Writing .csv and .tsv files**

Code only:

``` r
# Save cwts as chickwts.csv
write_csv(cwts, "chickwts.csv")

# Append cwts2 to chickwts.csv
write_csv(cwts2, "chickwts.csv", append = TRUE)
```

**Writing .rds files**

If the R object you're working with has metadata associated with it, saving to a CSV will cause that information to be lost.

Exports an entire R object (metadata and all).

Code only:

``` r
# Save trees as trees.rds
write_rds(trees, 'trees.rds')

# Import trees.rds: trees2
trees2 <- read_rds('trees.rds')

# Check whether trees and trees2 are the same
identical(trees, trees2)
```

Parsing Data with `readr`
----------------------------

**Coercing columns to different data types**

`readr` functions are quite good at guessing the correct data type for each column in a dataset. Of course, they aren't perfect, so sometimes you will need to change the type of a column after importing.

Code only:

``` r
# Convert all columns to double
trees2 <- type_convert(trees, col_types = cols(Girth = 'd', Height = 'd', Volume = 'd'))
```

**Coercing character columns into factors**

`readr` import functions is that they don't automatically convert strings into factors like `read.csv` does.

Code only:

``` r
# Parse the title column
salaries$title <- parse_factor(salaries$title, levels = c('Prof', 'AsstProf', 'AssocProf'))

# Parse the gender column
salaries$gender <- parse_factor(salaries$gender, levels = c('Male', 'Female'))
```

**Creating Date objects**

The `readr` import functions can automatically recognize dates in standard ISO 8601 format (YYYY-MM-DD) and parse columns accordingly. If you want to import a dataset with dates in other formats, you can use `parse_date`.

Code only:

``` r
# Change type of date column
weather$date <- parse_date(weather$date, format = '%m/%d/%Y')
```

**Parsing number formats**

The `readr` importing functions can sometimes run into trouble parsing a column as numbers when it contains non-numeric symbols in addition to numerals.

Code only:

``` r
# Parse amount column as a number
debt$amount <- parse_number(debt$amount)
```

**Viewing metadata before importing**

In some cases, it may be easier to get an idea of how `readr` plans to parse a dataset before you actually import it. When you see the planned column specification, you might decide to change the type of one or more columns, for example.

-   `spec_csv` for .csv and .tsv files.
-   `spec_delim` for .txt files (among others).

<!-- -->

``` r
# Specifications of chickwts
spec_csv('chickwts.csv')
```

    ## cols(
    ##   weight = col_integer(),
    ##   feed = col_character()
    ## )
