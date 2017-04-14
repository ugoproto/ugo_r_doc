---

[TOC]

---

**Foreword**

Course.

---

## 1, Introduction to eXtensible Time

**What is an `xts` object**

`xts`, a constructor or a subclass that inherits behavior from parents. `xts` (as a subclass) extends the popular `zoo` class (as a parent). Most `zoo` methods work for `xts`. 

`xts` is a matrix objects; subsets always preserve the matrix form. 

`xts` are indexed by a formal time object. You can time-stamp the data.

The main `xts` constructor two most important arguments are `x` for the data and `order.by` for the index. `x` must be a vector or matrix. `order.by` is a vector of the same length or number of rows of `x`; it must be a proper time or date object and it must be in increasing order.

`xts` also allows you to bind arbitrary key-value attributes to your data. This lets you keep metadata about your object inside your object.


```R
# Load xts
library(xts)
```

**A first `xts` object**


```R
core <- matrix(c(rep(1,3), rep(2,3)), nrow = 3, ncol = 2)
core

idx <- as.Date(c("2016-06-01","2016-06-02", "2016-06-03"))
idx

ex_matrix <- xts(core, order.by = idx)
ex_matrix
```


<table>
<tbody>
	<tr><td>1</td><td>2</td></tr>
	<tr><td>1</td><td>2</td></tr>
	<tr><td>1</td><td>2</td></tr>
</tbody>
</table>




    [1] "2016-06-01" "2016-06-02" "2016-06-03"



               [,1] [,2]
    2016-06-01    1    2
    2016-06-02    1    2
    2016-06-03    1    2


**More than a matrix**

`xts` objects are normal R matrices, but with special powers.


```R
# View the structure of ex_matrix
str(ex_matrix)
```

    An 'xts' object on 2016-06-01/2016-06-03 containing:
      Data: num [1:3, 1:2] 1 1 1 2 2 2
      Indexed by objects of class: [Date] TZ: UTC
      xts Attributes:  
     NULL
    


```R
# Extract the 3rd observation of the 2nd column of ex_matrix
ex_matrix[3, 2]

# Extract the 3rd observation of the 2nd column of core 
core[3, 2]
```


               [,1]
    2016-06-03    2



2


**Your first xts object**

Vector must be of the same length or number of rows as x and, this is very important: it must be a proper time or date object and it must be in increasing order.


```R
# Create the object data using 5 random numbers
data <- rnorm(5)
data

# Create dates as a Date class object starting from 2016-01-01
dates <- seq(as.Date("2016-01-01"), length = 5, by = "days")
dates

# Use xts() to create smith
smith <- xts(x = data, order.by = dates)
smith

# Create bday (1899-05-08) using a POSIXct date class object
bday <- as.POSIXct("1899-05-08")
bday

# Create hayek and add a new attribute called born
hayek <- xts(x = data, order.by = dates, born = bday)
hayek
```


<ol class=list-inline>
	<li>0.384581498733952</li>
	<li>-0.423869463715667</li>
	<li>-0.49549963387221</li>
	<li>1.49946058244083</li>
	<li>-0.92906780054414</li>
</ol>




    [1] "2016-01-01" "2016-01-02" "2016-01-03" "2016-01-04" "2016-01-05"



                     [,1]
    2016-01-01  0.3845815
    2016-01-02 -0.4238695
    2016-01-03 -0.4954996
    2016-01-04  1.4994606
    2016-01-05 -0.9290678



    [1] "1899-05-08 EST"



                     [,1]
    2016-01-01  0.3845815
    2016-01-02 -0.4238695
    2016-01-03 -0.4954996
    2016-01-04  1.4994606
    2016-01-05 -0.9290678


**Deconstructing xts**

Separate a time series into its core data and index attributes for additional analysis and manipulation.

Functions are methods from the `zoo` class.


```R
# Extract the core data of hayek
hayek_core <- coredata(hayek)

# View the class of hayek_core
class(hayek_core)

# Extract the index of hayek
hayek_index <- index(hayek)

# View the class of hayek_index
class(hayek_index)
```


'matrix'



'Date'


**Time-based indices**

`xts` objects get their power from the index attribute that holds the time dimension. One major difference between `xts` and most other time series objects in R is the ability to use any one of various classes that are used to represent time. Whether `POSIXct`, `Date`, or some other class, xts will convert this into an internal form to make subsetting as natural to the user as possible.


```R
# Create dates
dates <- as.Date("2016-01-01") + 0:4

# Create ts_a
ts_a <- xts(x = 1:5, order.by = dates)

# Create ts_b
ts_b <- xts(x = 1:5, order.by = as.POSIXct(dates))

# Extract the rows of ts_a using the index of ts_b
ts_a[index(ts_a)]

# Extract the rows of ts_b using the index of ts_a
ts_b[index(ts_a)]
```


               [,1]
    2016-01-01    1
    2016-01-02    2
    2016-01-03    3
    2016-01-04    4
    2016-01-05    5



         [,1]


## 2, First Order of Business - Basic Manipulations

**Converting xts objects**

`xts` provides methods to convert all of the major objects you are likely to come across - suitable native R types like matrix, data.frame, and ts are supported, as well as contributed ones such as `timeSeries`, `fts` and of course `zoo`. `as.xts` is the workhorse function to do the conversions to `xts`, and similar functions will provide the reverse behavior.


```R
# using a R dataset
head(austres)
```


<ol class=list-inline>
	<li>13067.3</li>
	<li>13130.5</li>
	<li>13198.4</li>
	<li>13254.2</li>
	<li>13303.7</li>
	<li>13353.9</li>
</ol>




```R
# Convert austres to an xts object called au
au <- as.xts(austres)
head(au, 5)

# Convert your xts object (au) into a matrix am
am <- as.matrix(au)
head(am, 5)

# Convert the original austres into a matrix am2
am2 <- as.matrix(austres)
head(am2, 5)
```


               [,1]
    1971 Q2 13067.3
    1971 Q3 13130.5
    1971 Q4 13198.4
    1972 Q1 13254.2
    1972 Q2 13303.7



<table>
<tbody>
	<tr><th scope=row>1971 Q2</th><td>13067.3</td></tr>
	<tr><th scope=row>1971 Q3</th><td>13130.5</td></tr>
	<tr><th scope=row>1971 Q4</th><td>13198.4</td></tr>
	<tr><th scope=row>1972 Q1</th><td>13254.2</td></tr>
	<tr><th scope=row>1972 Q2</th><td>13303.7</td></tr>
</tbody>
</table>




<table>
<tbody>
	<tr><td>13067.3</td></tr>
	<tr><td>13130.5</td></tr>
	<tr><td>13198.4</td></tr>
	<tr><td>13254.2</td></tr>
	<tr><td>13303.7</td></tr>
</tbody>
</table>



**Importing data**

Read raw data from files on disk or the web and convert data to `xts`.

Read the same data into a `zoo` object using read.zoo and convert the `zoo` into an `xts` object.


```R
# Create dat by reading tmp_file
dat <- as.matrix(read.csv('tmp_file.csv', sep = ',', header = TRUE))
dat
```


<table>
<thead><tr><th></th><th scope=col>a</th><th scope=col>b</th></tr></thead>
<tbody>
	<tr><th scope=row>1/02/2015</th><td>1</td><td>3</td></tr>
	<tr><th scope=row>2/03/2015</th><td>2</td><td>4</td></tr>
</tbody>
</table>




```R
# Convert dat into xts
xts(dat, order.by = as.Date(rownames(dat), "%m%d%Y"))
```


         a b
    <NA> 1 3
    <NA> 2 4



```R
# Read tmp_file using read.zoo and as.xts
dat_xts <- as.xts(read.zoo('tmp_file.csv', index.column = 0, sep = ",", format = "%m/%d/%Y"))
dat_xts
```


               a b
    2015-01-02 1 3
    2015-02-03 2 4


**Exporting xts objects**


```R
head(sunspots, 5)

# Convert sunspots to xts
sunspots_xts <- as.xts(sunspots)
```


<ol class=list-inline>
	<li>58</li>
	<li>62.6</li>
	<li>70</li>
	<li>55.7</li>
	<li>85</li>
</ol>




```R
# Get the temporary file name
tmp <- tempfile()

# Write the xts object using zoo to tmp 
write.zoo(sunspots, sep = ",", file = tmp)

tmp
```


'C:\Users\Dell\AppData\Local\Temp\RtmpEdYcAQ\file1a706c8c732b'



```R
# Read the tmp file
# FUN=as.yearmon will convert strings such as Jan 1749 into a proper time class
sun <- read.zoo(tmp, sep = ",", FUN = as.yearmon)

# Convert sun into xts
sun_xts <- as.xts(sun)
head(sun_xts, 5)
```


    Error in eval(expr, envir, enclos): impossible de trouver la fonction "read.zoo"
    Traceback:
    


**The ISO8601 Standard**

The ISO8601 standard is the internationally recognized and accepted way to represent dates and times. From the biggest to the smallest, left to right, YYYY-MM-DDTHH:MM:SS.

The standard allows for a common format to not only describe dates, but also a way to represent ranges and repeating intervals:

1. 2004 or 2001/2015
1. 201402/03
1. 2014-02-22 08:30:00
1. T08:00/T09:00

**Querying for dates**

Date ranges can be extracted from xts objects by simply specifying the period(s) you want using special character strings in your subset.

Code:

```r
A["201601"]         ## Jan 2016
A["20160125"]       ## Jan 25, 2016
A["201203/201212"]  ## Feb to Dec 2012

# Select all of 2016 from x
x_2016 <- x["2016"]

# Select January 2016 to March 22
jan_march <- x["201601/20160322"]
```

**Extracting recurring intraday intervals**

Most common data "in the wild" is daily. On ocassion you may find yourself working with intraday data, that is date plus times.

You can slice days easily by using special notation in the `i =` argument to the single bracket extraction (i.e. `[i,j]`).

Code:

```r
# Intraday times for all days
NYSE["T09:30/T16:00"]

# Extract all data between 8AM and 10AM
morn_2010 <- irreg["T8:00/T10:00"]

# Extract the observations for January 13th
morn_2010["2010-01-13"]
```

**Alternating extraction techniques**

Often times you may need to subset an existing time series with a set of Dates, or time-based objects. These might be from `as.Date()`, or `as.POSIXct()`, or a variety of other classes.

Code:

```r
# Subset x using the vector dates
x[dates]

# Subset x using dates as POSIXct
x[as.POSIXct(dates)]
```

**Extracting recurring intraday intervals**

The most common time series data is daily, intraday data, which contains both dates and times. 

```r
# Extract all data between 8AM and 10AM
morn_2010 <- irreg["T8:00/T10:00"]

# Extract the observations for January 13th
morn_2010["2010-01-13"]
```

**Row selection with time objects**

Subset an existing time series with a set of Dates, or time-based objects.

```r
# Subset x using the vector dates
x[dates]

# Subset x using dates as POSIXct
x[as.POSIXct(dates)]
```

**Update and replace elements**

Replace known intervals or observations with an NA, say due to a malfunctioning sensor on a particular day or a set of outliers given a holiday.

Code:

```r
# Replace all values in x on dates with NA
x[dates] <- NA

# Replace dates from 2016-06-09 and on with 0
x["20160609/"] <- 0
```

**Find the `first` or `last` period of time**

Sometimes you need to locate data by relative time. Instead of using an absolute offset, you describe a position relative in time. A simple example would be something like the *last 3 weeks* of a series, or the *first day of current month*.

Code:

```r
# Create lastweek with using the last 1 week of temps
lastweek <- last(temps, "1 week")

# Print the last 2 observations in lastweek
last(lastweek, 2)

# Extract all but the last two days of lastweek
last(lastweek, "-2 day")
```

**Combining `first` and `last`**

Code:

```r
# Last 3 days of first week
last(first(Temps, '1 week'),'3 days') 

# Extract the first three days of the second week of temps
first(last(first(temps, "2 week"), "1 week"), "3 day")
```

**Matrix arithmetic - add, subtract, multiply and divide in time!**

When you perform any binary operation using two xts objects, these objects are first aligned using the intersection of the indexes to preserve the point-in-time aspect of your data, assuring that you don't introduce accidental look ahead (or look behind!) bias into your calculations.

Your options include:

1. coredata() or as.numeric() (drop one to a matrix or vector).
1. Manually shift index values - i.e. use lag().
1. Reindex your data (before or after the calculation).

`xts` respects time and will only return the intersection of times when doing various mathematical operations. First option:

```r
# Add a and b
a + b

# Add a with the numeric value of b
a + as.numeric(b)
```

**Math with non-overlapping indexes**

This third way involves modifying the two series you want by aligning assuring you have some union of dates - the dates you require in your final output. This makes it possibly to preserve dimensionality of the data:

```r
merge(b, index(a))

# Add a to b, and fill all missing rows of b with 0
a + merge(b, index(a), fill = 0)

# Add a to b and fill NAs with the last observation
a + merge(b, index(a), fill = na.locf)
```

## 3, Merging and modifying time series

**Combining `xts` by column with `merge`**

`xts` makes it easy to join data by column and row using a few different functions.  

`xts` objects must be of identical type (e.g. integer + integer), or be POSIXct dates vector, or be atomic vectors of the same type (e.g. numeric), or be a single NA. It does not work on data.frames with various column types.

One of the most important functions to accomplish this is `merge`. It works like a `cbind` or and SQL `join`:

1. inner join (and)
1. outer join (or)
1. left join
1. right join

```r
# Basic argument use
merge(a, b, join = "right", fill = 9999)

# Perform an inner join of a and b
merge(a, b, join = "inner")

# Perform of a left-join of a and b, fill mising values with 0
merge(a, b, join = "left", fill = 0)

# fill = na.locf, fill = NA
```

**Combining `xts` by row with `rbind`**

Easy to add new rows to your data.

```r
# Row bind temps_june30 to temps, assign this to temps2
temps2 <- rbind(temps, temps_june30)

# Row bind temps_july17 and temps_july18 to temps2, call this temps3
temps3 <- rbind(temps2, temps_july17, temps_july18)
```

**Fill missing values using last or previous observation**

The `na.locf` function takes the last-observation-carried-forward approach.

The `xts` package leverages the power of `zoo` for help with this. `zoo` provides a variety of missing data handling functions which are usable by `xts`, and very handy.

```r
# Last obs. carried forward
na.locf(x)                

# Next obs. carried backward
na.locf(x, fromLast = TRUE)

# Fill missing values using the last observation
na.locf(temps, na.rm = TRUE,
                fromLast = TRUE)

# Fill missing values using the next observation
na.locf(temps, na.rm = TRUE)

# maxgap = Inf
# rule = 2,
```

**NA interpolation**

From `zoo`.

On occassion, a simple carry forward approach to missingness isn't appropriate. It may be that a series is missing an observation due to a higher frequency sampling than the generating process. You might also encounter an observation that is in error, yet expected to be somewhere between the values of its neighboring observations.

Both of these cases are examples of where interpolation is useful.

```r
# Interpolate NAs using linear approximation
na.approx(AirPass)
```

**Combine a leading and lagging time series**

Another common modification for time series is the ability to lag a series.

```r
# Your final object
lag(x, k = 1, na.pad = TRUE)

# k = -1  leading
# k = 1   lagging
# zoo uses the other way around

# k = c(0, 1, 5)  multiple leads or lags

# Create a leading object called lead_x
lead_x <- lag(x, k = -1)

# Create a lagging object called lag_x
lag_x <- lag(x, k = 1)

# Merge your three series together and assign to z
z <- merge(lead_x, x, lag_x)
```

**Calculate a difference of a series with `diff`**

Another common operation on time series, typically on those that are non-stationary, is to take a difference of the series.

`differences` is the order of the difference (how many times `diff` is called). A simple way to view a single (or *first order*) difference is to see it as `x(t) - x(t-k)` where `k` is the number of lags to go back. Higher order differences are simply the reapplication of a difference to each prior result (like a second derivative or a difference of the difference).

```r
diff(x, lag = 1, difference = 1, log = FALSE, na.pad = TRUE)

# calculate the first difference of AirPass using lag and subtraction
AirPass - lag(AirPass, k = 1)

# calculate the first order 12-month difference if AirPass
diff(AirPass, lag = 12, differences = 1)
```

What is the key difference in lag between xts and zoo? The `k` argument in `zoo` uses positive values for shifting past observations forward.

## 4, Apply and aggregate by time

**Find intervals by time in `xts`**

The main function in xts to facilitate this is endpoints. It takes a time series (or a vector of times) and returns the locations of the last observations in each interval.

For example, the code below locates the last observation of each year for the `AirPass` dataset:

```r
endpoints(AirPass, on = "years")

# The argument on supports a variety of periods, including "years", "quarters", "months", as well as intraday intervals such as "hours", and "minutes"

# on = "weeks", k = 2, the result would be the final day of every other week in your data

# Locate the final day of every week
endpoints(temps, on = "weeks")

# Locate the final day of every two weeks
endpoints(temps, on = "weeks", k = 2)
```

**Apply a function by time period(s)**

`xts` provides `period.apply`, which takes a time series, an `INDEX` of endpoints, and a function to `apply`:

```r
period.apply(x, INDEX, FUN, ...)

# Calculate the weekly endpoints
ep <- endpoints(temps, on = "weeks")

# Now calculate the weekly mean and display the results
period.apply(temps[, "Temp.Mean"], INDEX = ep, FUN = mean)
```

**Using `lapply` and `split` to apply functions on intervals**

Often it is useful to physically split your data into disjoint chunks by time and perform some calculation on these periods.

```r
# Split temps by week
temps_weekly <- split(temps, f = "weeks")

# Create a list of weekly means
lapply(X = temps_weekly, FUN = mean)
```

**Selection by `endpoints` vs. `split-lapply-rbind`**

```r
# use the proper combination of split, lapply and rbind.
#T1 <- do.call(rbind, ___(___(Temps, "weeks"), function(w) last(w, n="___")))
T1 <- do.call(rbind, lapply(split(Temps, "weeks"), function(w) last(w, n= "1 day")))

# now subset Temps using the results of 'endpoints'
last_day_of_weeks <- endpoints(Temps, on = "weeks")
T2 <- Temps[last_day_of_weeks]
```

**Convert univariate series to OHLC data**

In financial series it is common to find Open-High-Low-Close data calculated over some repeating and regular interval.

Also known as range bars, aggregating a series based on some regular window can make analysis easier amongst series that have varying frequencies. For example, a weekly economic series and a daily stock series can be compared more easily if the daily is converted to weekly.

OHLC means Opening, High, Low, Closing values.

```r
to.period(x,
          period = "months", 
          k = 1, 
          indexAt, 
          name=NULL,
          OHLC = TRUE,
          ...)
          
# Convert USDEUR to weekly
# OHLC = FALSE by default
USDEUR_weekly <- to.period(USDEUR, period = "weeks")
head(USDEUR)
head(USDEUR_weekly)

# Convert USDEUR_weekly to monthly
USDEUR_monthly <- to.period(USDEUR_weekly, period = "months")

# Convert USDEUR_monthly to yearly univariate
USDEUR_yearly <- to.period(USDEUR_monthly, period = "years", OHLC = FALSE)
```

**Convert a series to a lower frequency**

Besides converting univariate time series to OHLC series, to.period() also lets you convert OHLC to lower regularized frequency - something like subsampling your data.

For example, when using the shortcut function `to.quarterly()` xts will convert your index to the `yearqtr` class to make periods more obvious.

```r
# Convert EqMktNeutral to quarterly OHLC
mkt_quarterly <- to.period(EqMktNeutral, period = "quarters")

# Convert EqMktNeutral to quarterly using shortcut function
# Change the base name of each OHLC column to EDHEC.Equity and change the index to "firstof"
mkt_quarterly2 <- to.quarterly(EqMktNeutral, name = "EDHEC.Equity", indexAt = "first")
```

**Calculate basic rolling value of series by month**

Rolling windows can be discrete: use `lapply` with (`cumsum, cumprod, cummax, cummin`) and `split`:

```r
# split-lapply-rbind pattern

x_split <- split(x, f = "months")
x_list <- lapply(x_split, cummax)
x_list_rbind <- do.call(rbind, x_list)

# Split edhec into years
edhec_years <- split(edhec , f = "years")

# Use lapply to calculate the cumsum for each year
edhec_ytd <- lapply(edhec_years, FUN = cumsum)

# Use do.call to rbind the results
edhec_xts <- do.call(rbind, edhec_ytd)
```

**Calculate the rolling standard deviation of a time series**

Rolling windows can be continuous: use `rollapply`:

```r
rollapply(x, 10, FUN = max, na.rm = TRUE)

# Use rollapply to calculate the rolling 3 period sd of EqMktNeutral
eq_monthly <- rollapply(EqMktNeutral, 3, FUN = sd)
```

## 5, Extra features of xts

**Index, attributes, and time zones**

All time is stored in seconds since 1970-01-01. Time via `.index()` () and `index()` (raw seconds since 1970-01-01).

`index()` returns the time of an `xts` object, as a vector of class `indexClass`.

`Sys.setenv()`.

**Class attributes - `tclass`, `tzone` and `tformat`**

`xts` objects are somewhat tricky when it comes to times. Internally, we have now seen that the index attribute is really a vector of numeric values corresponding to the seconds since the UNIX epoch (1970-01-01).

How these values are displayed on printing and how they are returned to the user when using the `index()` function is dependent on a few key internal attributes.

The information that controls this behavior can be viewed and even changed through a set of accessor functions detailed below:

- The index class using indexClass (e.g. from Date to chron)
- The time zone using indexTZ (e.g. from America/Chicago to Europe/London)
- The time format to be displayed via indexFormat (e.g. YYYY/MM/DD)

```r
# Get the index class of temps
indexClass(temps)

# Get the timezone of temps
indexTZ(temps)

# Change the format of the time display
indexFormat(temps) <- "M/DD/YYYY"

# Extract the new index
time(temps)
```

**Time zones - (and why you should care!)**

R provides time zone support in native classes `POSIXct` and `POSIXlt`. xts extends this power to the entire object, allowing you to have multiple timezones across various objects. One very important thing to remember is that some internal operation system functions require a timezone to do date math, and if not set one is chosen for you! Be careful to always set a timezone in your environment to prevent very hard to debug errors when working with dates and times. 

`xts` provides the function `tzone`. This function allows to you to extract, or set timezones. `tzone(x) <- "Time_Zone"` In this exercise you will work with an object called times to practice constructing your own xts objects with custom time zones.

```r
# Construct times_xts with TZ set to America/Chicago
times_xts <- xts(1:10, order.by = times, tzone = "America/Chicago")

# Change the time zone to Asia/Hong_Kong
tzone(times_xts) <- "Asia/Hong_Kong"
  
# Extract the current timezone 
indexTZ(times_xts)
```

**Periods, periodicity, and timestamps**

Periods (yearly or intradays time index?): `periodicity()`

Broken downtime with `.index*`, `.index()`, `.indexmday()`, `indexyday()`, `indexyear()`.

Timestamps.

`align.time()` round time stamps to another period.

`make.index.unique()` removes observations from duplicate time stamps.

**Determining periodicity**

The idea of periodicity is pretty simple; with what regularity does your data repeat? For stock market data, you might have hourly prices or maybe daily open-high-low-close bars. For macro economic series, it might be monthly or weekly survey numbers.

```r
# Calculate the periodicity of temps
periodicity(temps)

# Calculate the periodicity of edhec
periodicity(edhec)

# Convert edhec to yearly
edhec_yearly <- to.yearly(edhec, 12)

# Calculate the periodicity of edhec_yearly
periodicity(edhec_yearly)
```

**Find the number of periods in your data**

Often times it is handy to know not just the range of your time series index, but also have an idea of how many discrete irregular periods this covers. xts provides a set of functions to do just that. If you have a time series, it is now easy to see how many days, weeks or years your data contains.

Count: `nseconds()`, `nminutes()`, `nhours()`, etc.

```r
`# Count the months
nmonths(edhec)

# Count the quarters
nquarters(edhec)

# Count the years
nyears(edhec)  
```

**Secret index tools**

Normally you want to access the times you stored. `index()` does this magically for you by using your `indexClass`. To get to the raw vector another function is provided, `.index()`. Note the critical dot before the function name.

More useful than extracting raw seconds is the ability to extract time components similar to the `POSIXlt` class, which mirrors closely the underlying POSIX internal compiled structure `tm`. This is provided by a handful of functions such as `.indexday()`, `.indexmon()`, `.indexyear()` and more.

```r
# Explore underlying units of temps
.index(temps) # in seconds
.indexwday(temps) # (0 for Sun, 1, 2,..., 6 for Sat) day of the week

# Create an index using which (Sunday has a value of 0, and Saturday has a value of 6)
index <- which(.indexwday(temps) == 0 | .indexwday(temps) == 6)

# Select the index
temps[index]
```

**Modifying timestamps**

Depending on your field you might encounter higher frequency data - think intraday trading intervals, or sensor data from medical equipment.

If you find that you have observations with identical timestamps, it might be useful to perturb or remove these times to allow for uniqueness.

On other ocassions you might find your timestamps a bit too precise. In these instances it might be better to round up to some fixed interval, for example an observation may occur at any point in an hour, but you want to record the latest as of beginning of the next hour.

```r
make.index.unique(x, eps = 1e-4)  # Perturb
make.index.unique(x, drop = TRUE) # Drop duplicates
align.time(x, n = 60) # Round to the minute

# Make z have unique timestamps
make.index.unique(z, eps = 1e-4)

# Remove duplicate times in z
make.index.unique(z, drop = TRUE)

# Round observations to the next time
align.time(z, n = 3600) # next hour
```
