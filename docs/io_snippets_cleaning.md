<!--
-   [Datasets](#datasets)
-   [Importing Data Into R](#importing-data-into-r)
    -   [1, Importing Data from Flat
        Files](#importing-data-from-flat-files)
    -   [2, Importing Data from Excel](#importing-data-from-excel)
    -   [3, Importing Data from Other Statistical
        Software](#importing-data-from-other-statistical-software)
    -   [4, Importing Data from Relational
        Data](#importing-data-from-relational-data)
    -   [4b, Importing Data from Relational Data --
        More](#b-importing-data-from-relational-data----more)
    -   [5, Importing Data from the Web](#importing-data-from-the-web)
    -   [6, Keyboard Inputting](#keyboard-inputting)
    -   [7, Exporting Data](#exporting-data)
    -   [8, Inspecting Data - Missing
        Data](#inspecting-data---missing-data)
    -   [9, Labels & Levels](#labels-levels)
-   [How to work with Quandl in R](#how-to-work-with-quandl-in-r)
    -   [1, Importing Quandl Datasets](#importing-quandl-datasets)
    -   [2, Manipulating Quandl Datasets](#manipulating-quandl-datasets)
-   [Cleaning Data in R](#cleaning-data-in-r)
    -   [1, Introduction and Exploring Raw
        Data](#introduction-and-exploring-raw-data)
    -   [2, Tidying Data](#tidying-data)
    -   [3, Preparing Data for Analysis](#preparing-data-for-analysis)
    -   [4, Putting it All Together](#putting-it-all-together)
-->
------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

Datasets
--------

-   [R Dataset Packages](http://stat.ethz.ch/R-manual/R-patched/library/datasets/html/00Index.html); by default in R.
-   Other dataset can be imported with `data(Cars93, package = 'MASS')` for example.
-   [csv/doc Datasets](https://vincentarelbundock.github.io/Rdatasets/datasets.html).
-   [Free Datasets](https://r-dir.com/reference/datasets.html) from the World Bank, Gapminder, Kaggle, Quandl, Reddit, and many more websites.
-   [Datasets](https://www.r-bloggers.com/datasets-to-practice-your-data-mining/) to Practice Your Data Mining.
-   [Houghton Mifflin Data](http://college.cengage.com/mathematics/brase/understandable_statistics/7e/students/datasets/slr/frames/frame.html) for linear regressions.
-   [Regression Datasets](http://data.princeton.edu/wws509/datasets/) for Generalized Linear Models (linear, logistic, poisson, multinomial, survival).
-   [Public Datasets on GitHub](http://www.kdnuggets.com/2015/04/awesome-public-datasets-github.html).
-   [Awesome Public Datasets](https://github.com/caesar0301/awesome-public-datasets).

------------------------------------------------------------------------

Importing Data Into R
=====================

The packages:

-   `utils`.
-   `readr`.
-   `data.table`.
-   `readxl`.
-   `gdata`.
-   `XLConnect`.
-   `haven`.
-   `foreign`.
-   `DBI`.
-   `httr`.
-   `jsonlite`.

Importing Data from Flat Files
==============================

## R functions, by default.

-   `read.csv`; `sep = ','`, `dec = '.'`.
-   `read.delim`; .txt, `dec = '.'`.
-   `read.csv2`; `sep = ';'`, `dec = ','`.
-   `read.delim2`; .txt, `dec = ','`.
-   Needs arguments.

**`read.csv` for .csv files**

``` r
# List the files in your working directory
dir()

# Import swimming_pools.csv: pools
# stringAsFactors = FALSE does not import strings as categorical variables
pools <- read.csv('swimming_pools.csv', stringsAsFactors = FALSE)
```

`stringsAsFactors`

``` r
# Import swimming_pools.csv correctly: pools
pools <- read.csv('swimming_pools.csv', stringsAsFactor = FALSE, header = TRUE, sep = ',')

# Import swimming_pools.csv with factors: pools_factor
pools_factor <- read.csv('swimming_pools.csv', header = TRUE, sep = ',')
```

**`read.delim` for .txt files**

``` r
# Import hotdogs.txt: hotdogs
hotdogs <- read.delim('hotdogs.txt', header = FALSE)

# Name the columns of hotdogs appropriately
names(hotdogs) <- c('type', 'calories', 'sodium')
```

Arguments.

``` r
# Load in the hotdogs data set: hotdogs
hotdogs <- read.delim('hotdogs.txt', header = FALSE, sep = '\t', col.names = c('type', 'calories', 'sodium'))

# Select the hot dog with the least calories: lily
lily <- hotdogs[which.min(hotdogs$calories), ]
# Select the observation with the most sodium: tom

tom <- hotdogs[which.max(hotdogs$sodium), ]
```

``` r
# Previous call to import hotdogs.txt
hotdogs <- read.delim('hotdogs.txt', header = FALSE, col.names = c('type', 'calories', 'sodium'))

# Print a vector representing the classes of the columns
sapply(hotdogs, class)

# Edit the colClasses argument to import the data correctly: hotdogs2
hotdogs2 <- read.delim('hotdogs.txt', header = FALSE, col.names = c('type', 'calories', 'sodium'), colClasses = c('factor', 'NULL', 'numeric'))
```

## The `utils` package

-   `read.table`; `sep = '\t'`, `= ','`, `= ';'`.
-   Read any tabular as a d.f.
-   Needs arguments; lots of argument for precision.
-   Slow.

<!-- -->

``` r
library(utils)
```

**`read.table` .txt files**

``` r
# Create a path to the hotdogs.txt file
path <- file.path('hotdogs', 'hotdogs.txt')

# Import the hotdogs.txt file: hotdogs
hotdogs <- read.table(path, header = FALSE, sep = '\t', col.names = c('type', 'calories', 'sodium'))
```

------------------------------------------------------------------------

## (from Importing Data from the Web)

``` r
# https URL to the swimming_pools csv file.
url_csv <- 'https://s3.amazonaws.com/assets.datacamp.com/course/importing_data_into_r/swimming_pools.csv'

# Import the file using read.csv(): pools1
pools1 <- read.csv(url_csv)
```

------------------------------------------------------------------------

## The `readr` package

-   `read_delim`; `delim = '\t'`, `= ','`.
-   `read_csv`; read `100.000, 200.000`
-   `read_tsv`; idem.
-   `read_csv2`; read `100,000; 200,000` or European files..
-   `read_tsv2`; idem.
-   `read_lines`.
-   `read_file`.
-   `write_csv`.
-   `write_rds`.
-   `type_convert`.
-   `parse_factor`.
-   `parse_date`.
-   `parse_number`.
-   `spec_csv`.
-   `spec_delim`.
-   Fast, few arguments.
-   Detect data type.

<!-- -->

``` r
library(readr)
```

**`read_delim` .txt files**

``` r
# Import potatoes.txt using read_delim(): potatoes
potatoes <- read_delim('potatoes.txt', delim = '\t')
```

**`read_csv` .csv files**

``` r
# Column names
properties <- c('area', 'temp', 'size', 'storage', 'method', 'texture', 'flavor', 'moistness')

# Import potatoes.csv with read_csv(): potatoes
potatoes <- read_csv('potatoes.csv', col_names = properties)

# Create a copy of potatoes: potatoes2
potatoes2 <- potatoes

# Convert the method column of potatoes2 to a factor
potatoes2$method = factor(potatoes2$method)

# or

potatoes2$method = as.factor(potatoes2$method)
```

**`col_types`, `skip` and `n_max` in .tsv files**

``` r
# Column names
properties <- c('area', 'temp', 'size', 'storage', 'method', 'texture', 'flavor', 'moistness')

# Import 5 observations from potatoes.txt: potatoes_fragment
# read_tsv or tab-separated values
potatoes_fragment <- read_tsv('potatoes.txt', col_names = properties, skip = 7, n_max = 5)

# Import all data, but force all columns to be character: potatoes_char
potatoes_char <- read_tsv('potatoes.txt', col_types = 'cccccccc')
```

**Setting column types**

``` r
cols(
  weight = col_integer(),
  feed = col_character()
)
```

**Removing NA**

``` r
na = c('NA', 'null')
```

**`col_types` with collectors .tsv files**

``` r
# Import without col_types
hotdogs <- read_tsv('hotdogs.txt', col_names = c('type', 'calories', 'sodium'))

# The collectors you will need to import the data
fac <- col_factor(levels = c('Beef', 'Meat', 'Poultry'))
int <- col_integer()

# Edit the col_types argument to import the data correctly: hotdogs_factor
# Change col_types to the correct vector of collectors; coerce the vector into a list
hotdogs_factor <- read_tsv('hotdogs.txt', col_names = c('type', 'calories', 'sodium'), col_types = list(fac, int, int))
```

**Skiping columns**

``` r
salaries <- read_tsv('Salaries.txt', col_names = FALSE, col_types = cols(
  X2 = col_skip(),
  X3 = col_skip(), 
  X4 = col_skip()
))
```

**Reading an ordinary text file**

``` r
# vector of character strings. 
# Import as a character vector, one item per line: tweets
tweets <- read_lines('tweets.txt')
tweets

# returns a length 1 vector of the entire file, with line breaks represented as \n
# Import as a length 1 vector: tweets_all
tweets_all <- read_file('tweets.txt')
tweets_all
```

**Writing .csv and .tsv files**

``` r
# Save cwts as chickwts.csv
write_csv(cwts, "chickwts.csv")

# Append cwts2 to chickwts.csv
write_csv(cwts2, "chickwts.csv", append = TRUE)
```

**Writing .rds files**

``` r
# Save trees as trees.rds
write_rds(trees, 'trees.rds')

# Import trees.rds: trees2
trees2 <- read_rds('trees.rds')

# Check whether trees and trees2 are the same
identical(trees, trees2)
```

**Coercing columns to different data types**

``` r
# Convert all columns to double
trees2 <- type_convert(trees, col_types = cols(Girth = 'd', Height = 'd', Volume = 'd'))
```

**Coercing character columns into factors**

``` r
# Parse the title column
salaries$title <- parse_factor(salaries$title, levels = c('Prof', 'AsstProf', 'AssocProf'))

# Parse the gender column
salaries$gender <- parse_factor(salaries$gender, levels = c('Male', 'Female'))
```

**Creating Date objects**

``` r
# Change type of date column
weather$date <- parse_date(weather$date, format = '%m/%d/%Y')
```

**Parsing number formats**

``` r
# Parse amount column as a number
debt$amount <- parse_number(debt$amount)
```

**Viewing metadata before importing**

-   `spec_csv` for .csv and .tsv files.
-   `spec_delim` for .txt files (among others).

<!-- -->

``` r
# Specifications of chickwts
spec_csv('chickwts.csv')
```

------------------------------------------------------------------------

## (from Importing Data from the Web)

**Import Flat files from the web**

``` r
# Import the csv file: pools
url_csv <- 'http://s3.amazonaws.com/assets.datacamp.com/course/importing_data_into_r/swimming_pools.csv'
pools <- read_csv(url_csv)

pools

# Import the txt file: potatoes
url_delim <- 'http://s3.amazonaws.com/assets.datacamp.com/course/importing_data_into_r/potatoes.txt'
potatoes <- read_tsv(url_delim)

potatoes
```

**Secure importing**

``` r
# Import the file using read_csv(): pools2
pools2 <- read_csv(url_csv)
```

------------------------------------------------------------------------

## The `data.table` package

-   `fread` == `read.table`.
-   .txt files only.
-   Fast.

<!-- -->

``` r
library(data.table)
```

**`fread` for .txt files**

``` r
# Import potatoes.txt with fread(): potatoes
potatoes <- fread('potatoes.txt')

# Print out arranged version of potatoes
potatoes[order(moistness),] 

# Import 20 rows of potatoes.txt with fread(): potatoes_part
potatoes_part <- fread('potatoes.txt', nrows = 20)
```

**`fread`: more advanced use**

``` r
# Import columns 6, 7 and 8 of potatoes.txt: potatoes
potatoes <- fread('potatoes.txt', select = c(6:8))

# Keep only tasty potatoes (flavor > 3): tasty_potatoes
tasty_potatoes <- subset(potatoes, potatoes$flavor > 3)
```

Importing Data from Excel
=========================

## The `readxl` package

-   `excel_sheets`; list.
-   `read_excel`; import.
-   .xlsx files only.

<!-- -->

``` r
library(readxl)
```

**List the sheets of an Excel file**

``` r
# Find the names of both spreadsheets: sheets
# Before, find out what is in the directory with 'dir()'
sheets <- excel_sheets('latitude.xlsx')

sheets
```

**Importing an Excel sheet**

``` r
# Read the first sheet of latitude.xlsx: latitude_1
latitude_1 <- read_excel('latitude.xlsx', sheet = 1)

# Read the second sheet of latitude.xlsx: latitude_2
latitude_2 <- read_excel('latitude.xlsx', sheet = 2)

# Put latitude_1 and latitude_2 in a list: lat_list
lat_list <- list(latitude_1, latitude_2)
```

**Reading a workbook**

``` r
# Read all Excel sheets with lapply(): lat_list
lat_list <- lapply(excel_sheets('latitude.xlsx'), read_excel, path = 'latitude.xlsx')
```

**The `col_names` argument**

``` r
# Import the the first Excel sheet of latitude_nonames.xlsx (R gives names): latitude_3
latitude_3 <- read_excel('latitude_nonames.xlsx', sheet = 1, col_names = FALSE)

# Import the the second Excel sheet of latitude_nonames.xlsx (specify col_names): latitude_4 
latitude_4 <- read_excel('latitude_nonames.xlsx', sheet = 1, col_names = c('country', 'latitude'))
```

**The `skip` argument**

``` r
# Import the second sheet of latitude.xlsx, skipping the first 21 rows: latitude_sel
latitude_sel <- read_excel('latitude.xlsx', skip = 21, col_names = FALSE)
```

------------------------------------------------------------------------

## (from Importing Data from the Web)

**Import Excel files from the web**

``` r
# Download file behind URL, name it local_latitude.xls
download.file(url_xls, 'local_latitude.xls')

# Import the local .xls file with readxl: excel_readxl
excel_readxl <- read_excel('local_latitude.xls')
```

**Downloading any file, secure or not**

``` r
# https URL to the wine RData file.
url_rdata <- 'https://s3.amazonaws.com/assets.datacamp.com/course/importing_data_into_r/wine.RData'

# Download the wine file to your working directory
download.file(url_rdata, 'wine_local.RData')
```

------------------------------------------------------------------------

## The `XLConnect` package

-   `loadWorkbook`.
-   `getSheets`.
-   `readWorksheet`.
-   `readWorksheetFromFile`
-   `readNamedRegion`
-   `readNamedRegionFromFile`

-   .xls & .xlsx files.
-   Like reading a database.

<!-- -->

``` r
library(XLConnectJars)
library(XLConnect)
```

**Import a workbook**

``` r
# Build connection to latitude.xlsx: my_book
my_book <- loadWorkbook('latitude.xlsx')
```

**List and read Excel sheets**

``` r
# Build connection to latitude.xlsx
my_book <- loadWorkbook('latitude.xlsx')

# List the sheets in latitude.xlsx
getSheets(my_book)

# Import the second sheet in latitude.xlsx
readWorksheet(my_book, sheet = 2)

# Import the second column of the first sheet in latitude.xlsx
readWorksheet(my_book, sheet = 2, startCol = 2)
```

**Add and populate worksheets**

``` r
# Build connection to latitude.xlsx
my_book <- loadWorkbook('latitude.xlsx')

# Create data frame: summ
dims1 <- dim(readWorksheet(my_book, 1))
dims2 <- dim(readWorksheet(my_book, 2))
summ <- data.frame(sheets = getSheets(my_book), 
                   nrows = c(dims1[1], dims2[1]), 
                   ncols = c(dims1[2], dims2[2]))

# Add a worksheet to my_book, named 'data_summary'
createSheet(my_book, name = 'data_summary')

# Populate 'data_summary' with summ data frame
writeWorksheet(my_book, summ, sheet = 'data_summary')
# Save workbook as latitude_with_summ.xlsx

saveWorkbook(my_book, 'latitude_with_summ.xlsx')
```

**One unique function**

``` r
# Read in the data set and assign to the object
impact <- readWorksheetFromFile('A Hands-on Introduction to Statistics with R.xls', sheet = 'impact', header = TRUE, startCol = 1, startRow = 1)

# more arguments
# endCol = 1
# endRow = 1
# autofitRow = 
# autofitCol = 
# region =
# rownames =
# colTypes =
# forceConversion =
# dateTimeFormat =
# check.names =
# useCachedValues =
# keep =
# drop =
# simplify =
# readStrategy =
```

Importing Data from Other Statistical Software
==============================================

## The `haven` package

-   `read_sas`; sas7bdat & sas7bcat files.
-   `read_stata`; version; dta files.
-   `read_dta`; idem.
-   `read_spss`; sav & por files (and see below).
-   `read_por`.
-   `read_sav`.
-   Simple, few arguments.
-   Create a d.f.

<!-- -->

``` r
library(haven)
```

**Import SAS data with haven**

``` r
# Import sales.sas7bdat: sales
sales <- read_sas('sales.sas7bdat')
```

**Import STATA data with haven**

``` r
# Import the data from the URL: sugar
sugar <- read_dta('http://assets.datacamp.com/course/importing_data_into_r/trade.dta')
```

**Import SPSS data with haven**

``` r
# Specify the file path using file.path(): path
path <- file.path('datasets', 'person.sav')

# Import person.sav, which is in the datasets folder: traits
traits <- read_sav(path)
```

**Factorize, round two**

``` r
# Import SPSS data from the URL: work
work <- read_sav('http://assets.datacamp.com/course/importing_data_into_r/employee.sav')
```

## The`foreign` package

-   Cannot import SAS, see the `sas7bdat` package.
-   `read.dta`; dta files.
-   `read.spss`; sav & por files.
-   Comprehensive.

<!-- -->

``` r
library(foreign)
```

**Import STATA data with foreign (1)**

``` r
# Import florida.dta and name the resulting data frame florida
florida <- read.dta('florida.dta')
```

**Import STATA data with foreign (2)**

``` r
# Specify the file path using file.path(): path
path <- file.path('worldbank', 'edequality.dta')

# Create and print structure of edu_equal_1
edu_equal_1 <- read.dta(path)

# Create and print structure of edu_equal_2
edu_equal_2 <- read.dta(path, convert.factors = FALSE)

# Create and print structure of edu_equal_3
edu_equal_3 <- read.dta(path, convert.underscore = TRUE) 
```

**Import SPSS data with foreign (1)**

``` r
# Import international.sav as a data frame: demo
demo <- read.spss('international.sav', to.data.frame = TRUE)
```

**Import SPSS data with foreign (2)**

``` r
# Import international.sav as demo_1
demo_1 <- read.spss('international.sav', to.data.frame = TRUE)

# Import international.sav as demo_2
demo_2 <- read.spss('international.sav', to.data.frame = TRUE, use.value.labels = FALSE)
```

Importing Data from Relational Data
===================================

## The `DBI` package

-   `dbConnect`.
-   `dbReadTable`.
-   `dbGetQuery`.
-   `dbFetch`.
-   `dbDisconnect`.

<!-- -->

``` r
library(DBI)
```

**Step 1: Establish a connection**

``` r
# Connect to the MySQL database: con
con <- dbConnect(RMySQL::MySQL(), dbname = 'tweater', host = 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com', port = 3306, user = 'student', password = 'datacamp') 

con
```

**Step 2: List the database tables**

``` r
# Connect to the MySQL database: con
con <- dbConnect(RMySQL::MySQL(), 
                 dbname = 'tweater', 
                 host = 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com', 
                 port = 3306,
                 user = 'student',
                 password = 'datacamp')

# Build a vector of table names: tables
tables <- dbListTables(con)

# Display structure of tables
str(tables)
```

**Step 3: Import data from a table**

``` r
# Connect to the MySQL database: con
con <- dbConnect(RMySQL::MySQL(), 
                 dbname = 'tweater', 
                 host = 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com', 
                 port = 3306,
                 user = 'student',
                 password = 'datacamp')

# Import the users table from tweater: users
users <- dbReadTable(con, 'users')

users

# Import and print the tweats table from tweater: tweats
tweats <- dbReadTable(con, 'tweats')

tweats

# Import and print the comments table from tweater: comments
comments <- dbReadTable(con, 'comments')

comments
```

**Your very first SQL query**

``` r
con <- dbConnect(RMySQL::MySQL(), 
                 dbname = 'tweater', 
                 host = 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com', 
                 port = 3306,
                 user = 'student',
                 password = 'datacamp')

# Import post column of tweats where date is higher than '2015-09-21': latest
latest <- dbGetQuery(con, 'SELECT post FROM tweats WHERE date > \'2015-09-21\'')

latest

# Import tweat_id column of comments where user_id is 1: elisabeth
elisabeth <- dbGetQuery(con, 'SELECT tweat_id FROM comments WHERE user_id = 1')

elisabeth
```

**More advanced SQL queries**

``` r
# Connect to the database
con <- dbConnect(RMySQL::MySQL(),
                 dbname = 'tweater', 
                 host = 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com', 
                 port = 3306,
                 user = 'student',
                 password = 'datacamp')

# Create data frame specific
specific <- dbGetQuery(con, 'SELECT message FROM comments WHERE tweat_id = 77 AND user_id > 4')

specific

# Create data frame short
short <- dbGetQuery(con, 'SELECT id, name FROM users WHERE CHAR_LENGTH(name) < 5')

short
```

**Send - Fetch - Clear**

``` r
# Connect to the database
con <- dbConnect(RMySQL::MySQL(), 
                 dbname = 'tweater', 
                 host = 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com', 
                 port = 3306,
                 user = 'student',
                 password = 'datacamp')

# Send query to the database with dbSendQuery(): res
res <- dbSendQuery(con, 'SELECT * FROM comments WHERE user_id > 4')

# Display information contained in res
dbGetInfo(res)

# Use dbFetch() twice
while (!dbHasCompleted(res)) {
    chunk <- dbFetch(res, n = 2)
    chunk2 <- dbFetch(res)
    print(chunk)
}

# Clear res
dbClearResult(res)
```

**Be polite and ...**

``` r
# Database specifics
dbname <- 'tweater'
host <- 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com'
port <- 3306
user <- 'student'
password <- 'datacamp'

# Connect to the database
con <- dbConnect(RMySQL::MySQL(), dbname = 'tweater', host = 'courses.csrrinzqubik.us-east-1.rds.amazonaws.com', port = 3306 , user = 'student', password = 'datacamp')

# Create the data frame  long_tweats
long_tweats <- dbGetQuery(con, 'SELECT post, date FROM tweats WHERE CHAR_LENGTH(post) > 40')

# Print long_tweats
print(long_tweats)

# Disconnect from the database
dbDisconnect(con)
```

**Other general packages**

The `RODBC` package provides access to databases (including Microsoft
Access and Microsoft SQL Server) through an ODBC interface.

The `RJDBC` package provides access to databases through a JDBC
interface.

**Specialized packages**

-   `ROracle` provides an interface for Oracle.
-   `RMySQL` provides access to MySQL.
-   `RpostgreSQL` to PostgreSQL.
-   `RSQLite` to SQLite.
-   And there are manu more packages for NoSQL databases such
    as MongoDB.

Importing Data from Relational Data -- More
===========================================

## `DBI`

First, change the working directory with `setwd`. Install the `DBI`
library.

**Connect and read preliminary results**

``` r
library(DBI)
library(sqliter)

# Assign the sqlite database and full path to a variable
dbfile = 'chinook.db'
   
# Instantiate the dbDriver to a convenient object
sqlite = dbDriver('SQLite')
   
# Assign the connection string to a connection object
sqlitedb <- dbConnect(RSQLite::SQLite(), 
                 dbname = dbfile, 
                 host = '', 
                 port = 3306,
                 user = '',
                 password = '')
   
# Request a list of tables using the connection object
dbListTables(sqlitedb)
```

**Extract some data**

``` r
# Assign the results of a SQL query to an object
results = dbSendQuery(sqlitedb, "SELECT * FROM albums")
   
# Return results from a custom object to a data.frame
data = fetch(results)
   
# Print data frame to console
head(data)
```

``` r
# Clear the results and close the connection
dbClearResult(results)

# Disconnect from the database
dbDisconnect(sqlitedb)
```

## `RSQLite`

First, change the working directory with `setwd`. Install the `RSQLite`
library.

**Connect and read preliminary results**

``` r
library(RSQLite)
library(sqliter)

# Assign the sqlite database and full path to a variable
dbfile = 'chinook.db'
   
# Instantiate the dbDriver to a convenient object
sqlite = dbDriver('SQLite')
   
# Assign the connection string to a connection object
mysqldb = dbConnect(sqlite, dbfile)
   
# Request a list of tables using the connection object
dbListTables(sqlitedb)
```

**Extract some data**

``` r
# Assign the results of a SQL query to an object
results = dbSendQuery(sqlitedb, "SELECT * FROM albums")

# Check the object
results
dbGetInfo(results)

# Return results from a custom object to a data.frame
data = fetch(results)
   
# Print data frame to console
head(data)
```

``` r
# Clear the results and close the connection
dbClearResult(results)

# Disconnect from the database
dbDisconnect(sqlitedb)
```

## MySQL with `DBI` or `RMySQL`

``` r
library(DBI)

# Assign the sqlite database and full path to a variable
dbfile = 'chinook.db'
   
# Instantiate the dbDriver to a convenient object
mysql = dbDriver('MySQL')
   
# Assign the connection string to a connection object
mysqldb <- dbConnect(RMySQL::MySQL(), 
                 dbname = dbfile, 
                 host = '', 
                 port = 3306,
                 user = '',
                 password = '')

# Request a list of tables using the connection object
dbListTables(mysqldb)

# Request a list of tables using the connection object
dbListTables(mysqldb)

# Disconnect from the database
dbDisconnect(mysqldb)
```

``` r
library(RMySQL)

# Assign the sqlite database and full path to a variable
dbfile = 'chinook.db'
   
# Instantiate the dbDriver to a convenient object
mysql = dbDriver('MySQL')
   
# Assign the connection string to a connection object
mysqldb = dbConnect(mysql, dbfile)
   
# Request a list of tables using the connection object
dbListTables(mysqldb)

# Request a list of tables using the connection object
dbListTables(mysqldb)

# Disconnect from the database
dbDisconnect(mysqldb)
```

## PosgreSQL with `DBI` or `RPostgreSQL`

``` r
library(DBI)

# Assign the sqlite database and full path to a variable
dbfile = 'chinook.db'
   
# Instantiate the dbDriver to a convenient object
postgresql = dbDriver('PostgreSQL')
   
# Assign the connection string to a connection object
postgresqldb <- dbConnect(RPostgreSQL::PostgreSQL(), 
                 dbname = dbfile, 
                 host = '', 
                 port = 3306,
                 user = '',
                 password = '')

# Request a list of tables using the connection object
dbListTables(postgresqldb)

# Request a list of tables using the connection object
dbListTables(postgresqldb)

# Disconnect from the database
dbDisconnect(postgresqldb)
```

``` r
library(RPostgreSQL)

# Assign the sqlite database and full path to a variable
dbfile = 'chinook.db'
   
# Instantiate the dbDriver to a convenient object
postgresql = dbDriver('PostgreSQL')
   
# Assign the connection string to a connection object
postgresqldb = dbConnect(postgresql, dbfile)
   
# Request a list of tables using the connection object
dbListTables(postgresqldb)

# Request a list of tables using the connection object
dbListTables(postgresqldb)

# Disconnect from the database
dbDisconnect(postgresqldb)
```

Importing Data from the Web
===========================

The other package above can download files from the web. The next
packages are web-oriented.

## The `httr` package

-   `GET` pages and files from the web.
-   Concise.
-   Parse JSON files.
-   Communicate with APIs.

<!-- -->

``` r
library(httr)
```

**HTTP? `httr`!**

``` r
# Get the url, save response to resp
url <- 'http://docs.datacamp.com/teach/'
resp <- GET(url)

resp

# Get the raw content of resp
raw_content <- content(resp, as = 'raw')

# Print the head of content
head(raw_content)
```

``` r
# Get the url
url <- 'https://www.omdbapi.com/?t=Annie+Hall&y=&plot=short&r=json'

resp <- GET(url)

# Print resp
resp

# Print content of resp as text
content(resp, as = 'text')

# Print content of resp
content(resp)
```

## The `jsonlite` package

-   Robust.
-   Improve the imported data.
-   `fromJSON`.
-   from an R object to `toJSON`
-   `prettify`.
-   `minify`.

<!-- -->

``` r
library(jsonlite)
```

**From `JSON` to R**

``` r
# Convert wine_json to a list: wine
wine_json <- '{'name':'Chateau Migraine', 'year':1997, 'alcohol_pct':12.4, 'color':'red', 'awarded':false}'
wine <- fromJSON(wine_json)

str(wine)

# Import Quandl data: quandl_data
quandl_url <- 'http://www.quandl.com/api/v1/datasets/IWS/INTERNET_INDIA.json?auth_token=i83asDsiWUUyfoypkgMz'
quandl_data <- fromJSON(quandl_url)

str(quandl_data)
```

``` r
# Experiment 1
json1 <- '[1, 2, 3, 4, 5, 6]'
fromJSON(json1)

# Experiment 2
json2 <- '{'a': [1, 2, 3], 'b': [4, 5, 6]}'
fromJSON(json2)

# Experiment 3
json3 <- '[[1, 2], [3, 4]]'
fromJSON(json3)

# Experiment 4
json4 <- '[{'a': 1, 'b': 2}, {'a': 3, 'b': 4}, {'a': 5, 'b': 6}]'
fromJSON(json4)
```

**Ask OMDb**

``` r
# Definition of the URLs
url_sw4 <- 'http://www.omdbapi.com/?i=tt0076759&r=json'
url_sw3 <- 'http://www.omdbapi.com/?i=tt0121766&r=json'

# Import two URLs with fromJSON(): sw4 and sw3
sw4 <- fromJSON(url_sw4)
sw3 <- fromJSON(url_sw3)

# Print out the Title element of both lists
sw4$Title
sw3$Title

# Is the release year of sw4 later than sw3
sw4$Year > sw3$Year
```

**From R to `JSON`**

``` r
# URL pointing to the .csv file
url_csv <- 'http://s3.amazonaws.com/assets.datacamp.com/course/importing_data_into_r/water.csv'

# Import the .csv file located at url_csv
water <- read.csv(url_csv, stringsAsFactors = FALSE)

# Generate a summary of water
summary(water)

# Convert the data file according to the requirements
water_json <- toJSON(water)

water_json
```

**`Minify` and `prettify`**

``` r
# Convert mtcars to a pretty JSON: pretty_json
pretty_json <- toJSON(mtcars, pretty = TRUE)

# Print pretty_json
pretty_json

# Minify pretty_json: mini_json
mini_json <- minify(pretty_json)

# Print mini_json
mini_json
```

Keyboard Inputting
==================

**Coding**

``` r
# create a data frame from scratch
age <- c(25, 30, 56)
gender <- c("male", "female", "male")
weight <- c(160, 110, 220)
mydata <- data.frame(age,gender,weight)
```

**Spreadsheet-like**

``` r
# enter data using editor
mydata <- data.frame(age = numeric(0), gender = character(0), weight = numeric(0))

mydata <- edit(mydata)
# note that without the assignment in the line above, the edits are not saved! 
```

Exporting Data
==============

## To a Tab-Delimited Text File

``` r
write.table(mydata, 'c:/mydata.txt', sep = "\t")
```

## To an Excel Spreadsheet

``` r
library(xlsx)

write.xlsx(mydata, "c:/mydata.xlsx")
```

**Worksheet**

``` r
library(XLConnect)
# xls or xlsx

# write a worksheet in steps
wb <- loadWorkbook('XLConnectExample1.xls', create = TRUE)
createSheet(wb, name = 'chickSheet')
writeWorksheet(wb, ChickWeight, sheet = 'chickSheet', startRow = 3, startCol = 4)
saveWorkbook(wb)

# write a worksheet all in one step
ChickWeight <- 1

writeWorksheetToFile('XLConnectExample2.xlsx', data = ChickWeight, sheet = 'chickSheet', startRow = 3, startCol = 4)
```

**Field**

``` r
# write a field in steps
wb = loadWorkbook('XLConnectExample3.xlsx', create = TRUE)
createSheet(wb, name = 'womenData')
createName(wb, name = 'womenName', formula = 'womenData!$C$5', overwrite = TRUE)
writeNamedRegion(wb, women, name = "womenName")
saveWorkbook(wb)

# write a field all in one step
writeNamedRegionToFile("XLConnectExample4.xlsx", women, name = "womenName", formula = "womenData!$C$5")
```

**I/O**

``` r
# Build connection to latitude.xlsx
my_book <- loadWorkbook('latitude.xlsx')

# Create data frame: summ
dims1 <- dim(readWorksheet(my_book, 1))
dims2 <- dim(readWorksheet(my_book, 2))
summ <- data.frame(sheets = getSheets(my_book), 
                   nrows = c(dims1[1], dims2[1]), 
                   ncols = c(dims1[2], dims2[2]))

# Add a worksheet to my_book, named 'data_summary'
createSheet(my_book, name = 'data_summary')

# Populate 'data_summary' with summ data frame
writeWorksheet(my_book, summ, sheet = 'data_summary')
# Save workbook as latitude_with_summ.xlsx

saveWorkbook(my_book, 'latitude_with_summ.xlsx')
```

## To SPSS

``` r
library(foreign)

write.foreign(mydata, "c:/mydata.txt", "c:/mydata.sps", package = "SPSS")
```

## To SAS

``` r
library(foreign)

write.foreign(mydata, "c:/mydata.txt", "c:/mydata.sas", package = "SAS") 
```

## To Stata

``` r
library(foreign)

write.dta(mydata, "c:/mydata.dta") 
```

Inspecting Data - Missing Data
==============================

## Inspecting

-   `ls(object)`.
-   `names(object)`.
-   `str(object)`.
-   `levels(object$v1)`.
-   `dim(object)`.
-   `class(object)`.
-   `print(object)`.
-   `head(object, 10)`.
-   `tail(object, 20)`.

**Testing for Missing Values**

``` r
y <- c(1, 2, 3, NA) # returns TRUE of x is missing
is.na(y) # returns a vector (F F F T) 
```

**Recoding Values to Missing**

``` r
# recode 99 to missing for variable v1
# select rows where v1 is 99 and recode column v1
mydata$v1[mydata$v1 == 99] <- NA 
```

**Excluding Missing Values from Analyses**

``` r
x <- c(1, 2, NA, 3)

mean(x) # returns NA
mean(x, na.rm = TRUE) # returns 2 
```

``` r
# list rows of data that have missing values
mydata[!complete.cases(mydata),]
```

``` r
# create new dataset without missing data
newdata <- na.omit(mydata) 
```

## The `dplyr` package

``` r
library(dplyr)

tbl_df(iris) # almost like head/tail
glimpse(iris) # almost like str
View(iris) # open a spreadsheet
```

## For thorough cleaning

-   The [Amelia II](http://gking.harvard.edu/amelia/) software.
-   The `mitools` package.

Labels & Levels
===============

**Basic**

``` r
# variable v1 is coded 1, 2 or 3
# we want to attach value labels 1=red, 2=blue, 3=green
mydata$v1 <- factor(mydata$v1, 
                    levels = c(1,2,3),
                    labels = c("red", "blue", "green"))
```

``` r
# variable y is coded 1, 3 or 5
# we want to attach value labels 1=Low, 3=Medium, 5=High
mydata$v1 <- ordered(mydata$y,
                     levels = c(1,3, 5),
                     labels = c("Low", "Medium", "High")) 
```

**Order**

``` r
# Create a vector of temperature observations
temperature_vector <- c('High', 'Low', 'High', 'Low', 'Medium')

# Specify that they are ordinal variables with the given levels
factor_temperature_vector <- factor(temperature_vector, order = TRUE, levels = c('Low', 'Medium', 'High'))
```

**Add comments to an object**

``` r
names(iris)[5] <- "This is the label for variable 5"

names(iris)[5] # the comment
iris[5] # the data
```

``` r
# labeling the variables
library(Hmisc)

label(iris$Species) <- "Variable label for variable myvar"

describe(iris$Species) # commented
#vs
describe(iris$Sepal.Length) # not commented
```

------------------------------------------------------------------------

How to work with Quandl in R
============================

## Importing Quandl Datasets

[Quandl](https://www.quandl.com/) delivers financial, economic and
alternative data to the world's top hedge funds, asset managers and
investment banks in several formats:

-   Excel.
-   R.
-   Python.
-   API.
-   DB.

The packages used:

-   `Quandl`.
-   `quantmod` for plotting.

**Quandl - A first date**

``` r
# Load in the Quandl package
library(Quandl)

# Assign your first dataset to the variable:
mydata <- Quandl('NSE/OIL')
```

**Identifying a dataset with its ID**

``` r
# Assign the Prague Stock Exchange to:
PragueStockExchange <- Quandl('PRAGUESE/PX')
```

**Plotting a stock chart**

``` r
# The quantmod package
library(quantmod)

# Load the Facebook data with the help of Quandl
Facebook <- Quandl('GOOG/NASDAQ_FB', type = 'xts')

# Plot the chart with the help of candleChart()
candleChart(Facebook)
```

**Searching a Quandl dataset in R**

``` r
# Look up the first 3 results for 'Bitcoin' within the Quandl database:
results <- Quandl.search(query = 'Bitcoin', silent = FALSE)

# Print out the results
str(results)

# Assign the data set with code BCHAIN/TOTBC
BitCoin <- Quandl('BCHAIN/TOTBC')
```

## Manipulating Quandl Datasets


**Manipulating data**

``` r
# Assign to the variable Exchange
Exchange <- Quandl('BNP/USDEUR', start_date = '2013-01-01', end_date = '2013-12-01')
```

**Transforming your Quandl dataset**

``` r
# API transformation
# The result:
GDP_Change <- Quandl('FRED/CANRGDPR', transformation = 'rdiff')
head(GDP_Change)
GDP_Chang <- Quandl('FRED/CANRGDPR')
head(GDP_Chang)
```

**The magic of frequency collapsing**

``` r
# The result:
eiaQuarterly <- Quandl('DOE/RWTC', collapse = 'quarterly')
```

**Truncation and sort**

``` r
# Assign to TruSo the first 5 observations of the crude oil prices
TruSo <- Quandl('DOE/RWTC', sort = 'asc', rows = 5)

# Print the result
TruSo
```

**A complex example**

``` r
# Here you should place the return:
Final <- Quandl('DOE/RWTC', collapse = 'daily', transformation = 'rdiff', start_date = '2005-01-01', end_date = '2010-03-01', sort = 'asc')
```

------------------------------------------------------------------------

Cleaning Data in R
==================

The packages used:

-   `dplyr` & `tidyr` for data wrangling.
-   `stringr` for regex.
-   `lubridate` for time and date.

## Introduction and Exploring Raw Data

**Here's what messy data look like**

``` r
# View the first 6 rows of data
head(weather)

# View the last 6 rows of data
tail(weather)

# View a condensed summary of the data
str(weather)
```

**Getting a feel for your data**

``` r
# Check the class of bmi
class(bmi)

# Check the dimensions of bmi
dim(bmi)

# View the column names of bmi
names(bmi)
```

**Viewing the structure of your data**

``` r
# Check the structure of bmi
str(bmi)

# Load dplyr
library(dplyr)

# Check the structure of bmi, the dplyr way
glimpse(bmi)

# View a summary of bmi
summary(bmi)
```

**Looking at your data**

``` r
# Print bmi to the console
print(bmi)

# View the first 6 rows
head(bmi, 6)

# View the first 15 rows
head(bmi, 15)

# View the last 6 rows
tail(bmi, 6)

# View the last 10 rows
tail(bmi, 10)
```

**Visualizing your data**

``` r
# Histogram of BMIs from 2008
hist(bmi$Y2008)

# Scatter plot comparing BMIs from 1980 to those from 2008
plot(bmi$Y1980, bmi$Y2008)
```

## Tidying Data

**Gathering columns into key-value pairs**

``` r
# Load tidyr
library(tidyr)

# Apply gather() to bmi and save the result as bmi_long
bmi_long <- gather(bmi, year, bmi_val, -Country)

# View the first 20 rows of the result
head(bmi_long, 20)
```

**Spreading key-value pairs into columns**

``` r
# Apply spread() to bmi_long
bmi_wide <- spread(bmi_long, year, bmi_val)

# View the head of bmi_wide
head(bmi_wide)
```

**Separating columns**

``` r
# Apply separate() to bmi_cc
bmi_cc_clean <- separate(bmi_cc, col = Country_ISO, into = c('Country', 'ISO'), sep = '/')

# Print the head of the result
head(bmi_cc_clean)
```

**Uniting columns**

``` r
# Apply unite() to bmi_cc_clean
bmi_cc <- unite(bmi_cc_clean, Country_ISO, Country, ISO, sep = '-')

# View the head of the result
head(bmi_cc)
```

**Column headers are values, not variable names**

``` r
# View the head of census
head(census)

# Gather the month columns
census2 <- gather(census, month, amount, JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC)

# Arrange rows by YEAR using dplyr's arrange
census2 <- arrange(census2, YEAR)

# View first 20 rows of census2
head(census2, 20)
```

**Variables are stored in both rows and columns**

``` r
# View first 50 rows of census_long
head(census_long, 50)

# Spread the type column
census_long2 <- spread(census_long, type, amount)

# View first 20 rows of census_long2
head(census_long2, 20)
```

**Multiple values are stored in one column**

``` r
# View the head of census_long3
head(census_long3)

# Separate the yr_month column into two
census_long4 <- separate(census_long3, yr_month, c('year', 'month'), '_')

# View the first 6 rows of the result
head(census_long4, 6)
```

## Preparing Data for Analysis

**Types of variables in R**

``` r
# Make this evaluate to character
class('true')

# Make this evaluate to numeric
class(8484.00)

# Make this evaluate to integer
class(99L)

# Make this evaluate to factor
class(factor('factor'))

# Make this evaluate to logical
class(FALSE)
```

**Common type conversions**

``` r
# Preview students with str()
str(students)

# Coerce Grades to character
students$Grades <- as.character(students$Grades)

# Coerce Medu to factor
students$Medu <- as.factor(students$Medu)

# Coerce Fedu to factor
students$Fedu <- as.factor(students$Fedu)

 # Look at students once more with str()
str(students)
```

**Working with dates**

``` r
# Preview students2 with str()
str(students2)

# Load the lubridate package
library(lubridate)

# Parse as date
ymd('2015-Sep-17')

# Parse as date and time (with no seconds!)
ymd_hm('2012-July-15, 12.56')

# Coerce dob to a date (with no time)
students2$dob <- ymd(students2$dob)

# Coerce nurse_visit to a date and time
students2$nurse_visit <- ymd_hms(students2$nurse_visit)

# Look at students2 once more with str()
str(students2)
```

**Trimming and padding strings**

``` r
# Load the stringr package
library(stringr)

# Trim all leading and trailing whitespace
str_trim(c('   Filip ', 'Nick  ', ' Jonathan'))

# Pad these strings with leading zeros
str_pad(c('23485W', '8823453Q', '994Z'), width = 9, side = 'left', pad = '0')
```

**Upper and lower case**

``` r
# Print state abbreviations
states

# Make states all uppercase and save result to states_upper
states_upper <- toupper(states)
states_upper

# Make states_upper all lowercase again
tolower(states_upper)
```

**Finding and replacing strings**

``` r
# stringr has been loaded for you
# Look at the head of students2
head(students2)

# Detect all dates of birth (dob) in 1997
str_detect(students2$dob, '1997')

# In the sex column, replace 'F' with 'Female'...
students2$sex <- str_replace(students2$sex, 'F', 'Female')

# ...And 'M' with 'Male'
students2$sex <- str_replace(students2$sex, 'M', 'Male')

# View the head of students2
head(students2)
```

**Finding missing values**

``` r
# Call is.na() on the full social_df to spot all NAs
is.na(social_df)

# Use the any() function to ask whether there are any NAs in the data
any(is.na(social_df))
sum(is.na(social_df))

# View a summary() of the dataset
summary(social_df)

# Call table() on the status column
table(social_df$status)
```

**Dealing with missing values**

``` r
# Use str_replace() to replace all missing strings in status with NA
social_df$status <- str_replace(social_df$status, '^$', NA)

# Print social_df to the console
social_df

# Use complete.cases() to see which rows have no missing values
complete.cases(social_df)

# Use na.omit() to remove all rows with any missing values
na.omit(social_df)
```

**Dealing with outliers and obvious errors**

``` r
# Look at a summary() of students3
summary(students3)

# View a histogram of the age variable
hist(students3$age, breaks = 20)

# View a histogram of the absences variable
hist(students3$absences, breaks = 20)

# View a histogram of absences, but force zeros to be bucketed to the right of zero
hist(students3$absences, breaks = 20, right = FALSE)
```

**Another look at strange values**

``` r
# View a boxplot of age
boxplot(students3$age)

# View a boxplot of absences
boxplot(students3$absences)
```

## Putting it All Together

**Get a feel for the data**

``` r
# Verify that weather is a data.frame
class(weather)

# Check the dimensions
dim(weather)

# View the column names
names(weather)
```

**Summarize the data**

``` r
# View the structure of the data
str(weather)

# Load dplyr package
library(dplyr)

# Look at the structure using dplyr's glimpse()
glimpse(weather)

# View a summary of the data
summary(weather)
```

**Take a closer look**

``` r
# View first 6 rows
head(weather, 6)

# View first 15 rows
head(weather, 15)

# View the last 6 rows
tail(weather, 6)

# View the last 10 rows
tail(weather, 10)
```

**Column names are values**

``` r
# Load the tidyr package
library(tidyr)

# Gather the columns
weather2 <- gather(weather, day, value, X1:X31, na.rm = TRUE)

# View the head
head(weather2)
```

**Values are variable names**

``` r
# First remove column of row names
weather2 <- weather2[, -1]

# Spread the data
weather3 <- spread(weather2, measure, value)

# View the head
head(weather3)
```

**Clean up dates**

``` r
# Load the stringr and lubridate packages
library(stringr)
library(lubridate)

# Remove X's from day column
# weather3$day <- str_pad(str_replace(weather3$day, 'X', ''), width = 2, side = 'left', pad = '0')
weather3$day <- str_replace(weather3$day, 'X', '')

# Unite the year, month, and day columns
weather4 <- unite(weather3, date, year, month, day, sep = '-')

# Convert date column to proper date format using stringr's ymd()
weather4$date <- ymd(weather4$date)

# Rearrange columns using dplyr's select()
weather5 <- select(weather4, date, Events, CloudCover:WindDirDegrees)

# View the head
head(weather5)
```

**A closer look at column types**

``` r
# View the structure of weather5
str(weather5)

# Examine the first 20 rows of weather5. Are most of the characters numeric?
head(weather5, 20)

# See what happens if we try to convert PrecipitationIn to numeric
as.numeric(weather5$PrecipitationIn)
```

**Column type conversions**

``` r
# The dplyr package is already loaded
# Replace T with 0 (T = trace)
weather5$PrecipitationIn <- str_replace(weather5$PrecipitationIn, 'T', '0')

# Convert characters to numerics
weather6 <- mutate_each(weather5, funs(as.numeric), CloudCover:WindDirDegrees)

# Look at result
str(weather6)
```

**Find missing values**

``` r
# Count missing values
sum(is.na(weather6))

# Find missing values
summary(weather6)

# Find indices of NAs in Max.Gust.SpeedMPH
ind <- which(is.na(weather6$Max.Gust.SpeedMPH))
ind

# Look at the full rows for records missing Max.Gust.SpeedMPH
weather6[ind, ]
```

**An obvious error**

``` r
# Review distibutions for all variables
summary(weather6)

# Find row with Max.Humidity of 1000
ind <- which(weather6$Max.Humidity == 1000)

# Look at the data for that day
weather6[ind, ]

# Change 1000 to 100
weather6$Max.Humidity[ind] <- 100
```

**Another obvious error**

``` r
# Look at summary of Mean.VisibilityMiles
summary(weather6$Mean.VisibilityMiles)

# Get index of row with -1 value
ind <- which(weather6$Mean.VisibilityMiles == -1)

# Look at full row
weather6[ind, ]

# Set Mean.VisibilityMiles to the appropriate value
weather6$Mean.VisibilityMiles[ind] <- 10
```

**Check other extreme values**

``` r
# Review summary of full data once more
summary(weather6)

# Look at histogram for MeanDew.PointF
hist(weather6$MeanDew.PointF)

# Look at histogram for Min.TemperatureF
hist(weather6$Min.TemperatureF)

# Compare to histogram for Mean.TemperatureF
hist(weather6$Mean.TemperatureF)
```

**Finishing touches**

``` r
# Clean up column names
names(weather6) <- new_colnames

# Replace empty cells in Events column
weather6$events[weather6$events == ''] <- 'None'

# Print the first 6 rows of weather6
head(weather6, 6)
```
