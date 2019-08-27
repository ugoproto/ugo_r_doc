<!--
-   [Documentation](#documentation)
-   [1, Introduction to `dplyr`](#introduction-to-dplyr)
    -   [Load the `dplyr` and `hflights`
        package](#load-the-dplyr-and-hflights-package)
    -   [Convert `data.frame` to table](#convert-data.frame-to-table)
    -   [Changing labels of `hflights`](#changing-labels-of-hflights)
-   [2, `select` and `mutate`](#select-and-mutate)
    -   [The five verbs and their
        meaning](#the-five-verbs-and-their-meaning)
    -   [The `select` verb](#the-select-verb)
    -   [Helper functions for variable
        selection](#helper-functions-for-variable-selection)
    -   [Comparison to basic R](#comparison-to-basic-r)
    -   [`mutate` is creating](#mutate-is-creating)
    -   [Add multiple variables using
        `mutate`](#add-multiple-variables-using-mutate)
-   [3, `filter` and `arrange`](#filter-and-arrange)
    -   [Logical operators](#logical-operators)
    -   [Combining tests using boolean
        operators](#combining-tests-using-boolean-operators)
    -   [Blend together](#blend-together)
    -   [Arranging your data](#arranging-your-data)
    -   [Reverse the order of
        arranging](#reverse-the-order-of-arranging)
-   [4, `summarise` and the Pipe
    Operator](#summarise-and-the-pipe-operator)
    -   [The syntax of `summarise`](#the-syntax-of-summarise)
    -   [Aggregate functions](#aggregate-functions)
    -   [`dplyr` aggregate functions](#dplyr-aggregate-functions)
-   [5, `group_by` and working with
    data](#group_by-and-working-with-data)
    -   [Unite and conquer using
        `group_by`](#unite-and-conquer-using-group_by)
    -   [Combine `group_by` with
        `mutate`](#combine-group_by-with-mutate)
    -   [Advanced `group_by`](#advanced-group_by)
    -   [`dplyr` deals with different
        types](#dplyr-deals-with-different-types)
    -   [`dplyr` and mySQL databases](#dplyr-and-mysql-databases)
-   [Adding `tidyr` Functions](#adding-tidyr-functions)
-   [Joining Data in R with `dplyr`](#joining-data-in-r-with-dplyr)
-->
------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

Documentation
=============

`data.table`

-   extension of `data.frame`.
-   Fast aggregation of large data (e.g. 100GB in RAM), fast ordered joins, fast add/modify/delete of columns by group using no copies at all, list columns, a fast friendly file reader and parallel file writer. Offers a natural and flexible syntax, for faster development.

`dplyr`

-   A fast, consistent tool for working with data frame like objects, both in memory and out of memory.
-   Pipelines.

`tidyr`

-   An evolution of 'reshape2'. It's designed specifically for data tidying (not general reshaping or aggregating) and works well with dplyr data pipelines.

<table>
<thead>
<tr class="header">
<th align="left">package</th>
<th align="left">'narrower'</th>
<th align="left">'wider'</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><code>tidyr</code></td>
<td align="left">gather</td>
<td align="left">spread</td>
</tr>
<tr class="even">
<td align="left"><code>reshape2</code></td>
<td align="left">melt</td>
<td align="left">cast</td>
</tr>
<tr class="odd">
<td align="left">spreadsheets</td>
<td align="left">unpivot</td>
<td align="left">pivot</td>
</tr>
<tr class="even">
<td align="left">databases</td>
<td align="left">fold</td>
<td align="left">unfold</td>
</tr>
</tbody>
</table>

Introduction to `dplyr`
========================

## Load the `dplyr` and `hflights` package

``` r
# Load the dplyr package
library(dplyr)
library(dtplyr)

# Load the hflights package
# A data only package containing commercial domestic flights that departed Houston (IAH and HOU) in 2011
library(hflights)

# Call both head() and summary() on hflights
head(hflights)
```

    ##      Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ## 5424 2011     1          1         6    1400    1500            AA
    ## 5425 2011     1          2         7    1401    1501            AA
    ## 5426 2011     1          3         1    1352    1502            AA
    ## 5427 2011     1          4         2    1403    1513            AA
    ## 5428 2011     1          5         3    1405    1507            AA
    ## 5429 2011     1          6         4    1359    1503            AA
    ##      FlightNum TailNum ActualElapsedTime AirTime ArrDelay DepDelay Origin
    ## 5424       428  N576AA                60      40      -10        0    IAH
    ## 5425       428  N557AA                60      45       -9        1    IAH
    ## 5426       428  N541AA                70      48       -8       -8    IAH
    ## 5427       428  N403AA                70      39        3        3    IAH
    ## 5428       428  N492AA                62      44       -3        5    IAH
    ## 5429       428  N262AA                64      45       -7       -1    IAH
    ##      Dest Distance TaxiIn TaxiOut Cancelled CancellationCode Diverted
    ## 5424  DFW      224      7      13         0                         0
    ## 5425  DFW      224      6       9         0                         0
    ## 5426  DFW      224      5      17         0                         0
    ## 5427  DFW      224      9      22         0                         0
    ## 5428  DFW      224      9       9         0                         0
    ## 5429  DFW      224      6      13         0                         0

``` r
summary(hflights)
```

    ##       Year          Month          DayofMonth      DayOfWeek    
    ##  Min.   :2011   Min.   : 1.000   Min.   : 1.00   Min.   :1.000  
    ##  1st Qu.:2011   1st Qu.: 4.000   1st Qu.: 8.00   1st Qu.:2.000  
    ##  Median :2011   Median : 7.000   Median :16.00   Median :4.000  
    ##  Mean   :2011   Mean   : 6.514   Mean   :15.74   Mean   :3.948  
    ##  3rd Qu.:2011   3rd Qu.: 9.000   3rd Qu.:23.00   3rd Qu.:6.000  
    ##  Max.   :2011   Max.   :12.000   Max.   :31.00   Max.   :7.000  
    ##                                                                 
    ##     DepTime        ArrTime     UniqueCarrier        FlightNum   
    ##  Min.   :   1   Min.   :   1   Length:227496      Min.   :   1  
    ##  1st Qu.:1021   1st Qu.:1215   Class :character   1st Qu.: 855  
    ##  Median :1416   Median :1617   Mode  :character   Median :1696  
    ##  Mean   :1396   Mean   :1578                      Mean   :1962  
    ##  3rd Qu.:1801   3rd Qu.:1953                      3rd Qu.:2755  
    ##  Max.   :2400   Max.   :2400                      Max.   :7290  
    ##  NA's   :2905   NA's   :3066                                    
    ##    TailNum          ActualElapsedTime    AirTime         ArrDelay      
    ##  Length:227496      Min.   : 34.0     Min.   : 11.0   Min.   :-70.000  
    ##  Class :character   1st Qu.: 77.0     1st Qu.: 58.0   1st Qu.: -8.000  
    ##  Mode  :character   Median :128.0     Median :107.0   Median :  0.000  
    ##                     Mean   :129.3     Mean   :108.1   Mean   :  7.094  
    ##                     3rd Qu.:165.0     3rd Qu.:141.0   3rd Qu.: 11.000  
    ##                     Max.   :575.0     Max.   :549.0   Max.   :978.000  
    ##                     NA's   :3622      NA's   :3622    NA's   :3622     
    ##     DepDelay          Origin              Dest              Distance     
    ##  Min.   :-33.000   Length:227496      Length:227496      Min.   :  79.0  
    ##  1st Qu.: -3.000   Class :character   Class :character   1st Qu.: 376.0  
    ##  Median :  0.000   Mode  :character   Mode  :character   Median : 809.0  
    ##  Mean   :  9.445                                         Mean   : 787.8  
    ##  3rd Qu.:  9.000                                         3rd Qu.:1042.0  
    ##  Max.   :981.000                                         Max.   :3904.0  
    ##  NA's   :2905                                                            
    ##      TaxiIn           TaxiOut         Cancelled       CancellationCode  
    ##  Min.   :  1.000   Min.   :  1.00   Min.   :0.00000   Length:227496     
    ##  1st Qu.:  4.000   1st Qu.: 10.00   1st Qu.:0.00000   Class :character  
    ##  Median :  5.000   Median : 14.00   Median :0.00000   Mode  :character  
    ##  Mean   :  6.099   Mean   : 15.09   Mean   :0.01307                     
    ##  3rd Qu.:  7.000   3rd Qu.: 18.00   3rd Qu.:0.00000                     
    ##  Max.   :165.000   Max.   :163.00   Max.   :1.00000                     
    ##  NA's   :3066      NA's   :2947                                         
    ##     Diverted       
    ##  Min.   :0.000000  
    ##  1st Qu.:0.000000  
    ##  Median :0.000000  
    ##  Mean   :0.002853  
    ##  3rd Qu.:0.000000  
    ##  Max.   :1.000000  
    ## 

## Convert `data.frame` to table

``` r
# Convert the hflights data.frame into a hflights tbl
hflights <- tbl_df(hflights)

# Display the hflights tbl
hflights
```

    ## # A tibble: 227,496 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ## *  <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1          1         6    1400    1500            AA
    ## 2   2011     1          2         7    1401    1501            AA
    ## 3   2011     1          3         1    1352    1502            AA
    ## 4   2011     1          4         2    1403    1513            AA
    ## 5   2011     1          5         3    1405    1507            AA
    ## 6   2011     1          6         4    1359    1503            AA
    ## 7   2011     1          7         5    1359    1509            AA
    ## 8   2011     1          8         6    1355    1454            AA
    ## 9   2011     1          9         7    1443    1554            AA
    ## 10  2011     1         10         1    1443    1553            AA
    ## # ... with 227,486 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# Create the object carriers, containing only the UniqueCarrier variable of hflights
carriers <- hflights$UniqueCarrier
```

## Changing labels of `hflights`

**part 1 of 2**

``` r
# add
lut <- c('AA' = 'American', 'AS' = 'Alaska', 'B6' = 'JetBlue', 'CO' = 'Continental', 
         'DL' = 'Delta', 'OO' = 'SkyWest', 'UA' = 'United', 'US' = 'US_Airways', 
         'WN' = 'Southwest', 'EV' = 'Atlantic_Southeast', 'F9' = 'Frontier', 
         'FL' = 'AirTran', 'MQ' = 'American_Eagle', 'XE' = 'ExpressJet', 'YV' = 'Mesa')

# Use lut to translate the UniqueCarrier column of hflights
hflights$UniqueCarrier <- lut[hflights$UniqueCarrier]

# Inspect the resulting raw values of your variables
glimpse(hflights)
```

    ## Observations: 227,496
    ## Variables: 21
    ## $ Year              <int> 2011, 2011, 2011, 2011, 2011, 2011, 2011, 20...
    ## $ Month             <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
    ## $ DayofMonth        <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 1...
    ## $ DayOfWeek         <int> 6, 7, 1, 2, 3, 4, 5, 6, 7, 1, 2, 3, 4, 5, 6,...
    ## $ DepTime           <int> 1400, 1401, 1352, 1403, 1405, 1359, 1359, 13...
    ## $ ArrTime           <int> 1500, 1501, 1502, 1513, 1507, 1503, 1509, 14...
    ## $ UniqueCarrier     <chr> "American", "American", "American", "America...
    ## $ FlightNum         <int> 428, 428, 428, 428, 428, 428, 428, 428, 428,...
    ## $ TailNum           <chr> "N576AA", "N557AA", "N541AA", "N403AA", "N49...
    ## $ ActualElapsedTime <int> 60, 60, 70, 70, 62, 64, 70, 59, 71, 70, 70, ...
    ## $ AirTime           <int> 40, 45, 48, 39, 44, 45, 43, 40, 41, 45, 42, ...
    ## $ ArrDelay          <int> -10, -9, -8, 3, -3, -7, -1, -16, 44, 43, 29,...
    ## $ DepDelay          <int> 0, 1, -8, 3, 5, -1, -1, -5, 43, 43, 29, 19, ...
    ## $ Origin            <chr> "IAH", "IAH", "IAH", "IAH", "IAH", "IAH", "I...
    ## $ Dest              <chr> "DFW", "DFW", "DFW", "DFW", "DFW", "DFW", "D...
    ## $ Distance          <int> 224, 224, 224, 224, 224, 224, 224, 224, 224,...
    ## $ TaxiIn            <int> 7, 6, 5, 9, 9, 6, 12, 7, 8, 6, 8, 4, 6, 5, 6...
    ## $ TaxiOut           <int> 13, 9, 17, 22, 9, 13, 15, 12, 22, 19, 20, 11...
    ## $ Cancelled         <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ CancellationCode  <chr> "", "", "", "", "", "", "", "", "", "", "", ...
    ## $ Diverted          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...

**part 2 of 2**

``` r
# Build the lookup table: lut
lut <- c("A" = "carrier", "B" = "weather" ,"C" = "FFA" ,"D" = "security", "E" = "not cancelled")

# Add the Code column
hflights$Code <- lut[hflights$CancellationCode]

# Glimpse at hflights
glimpse(hflights)
```

Result.

``` text
Observations: 227,496
Variables: 22
$ Year              <int> 2011, 2011, 2011, 2011, 2011, 2011,...
$ Month             <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
$ DayofMonth        <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...
$ DayOfWeek         <int> 6, 7, 1, 2, 3, 4, 5, 6, 7, 1, 2, 3,...
$ DepTime           <int> 1400, 1401, 1352, 1403, 1405, 1359,...
$ ArrTime           <int> 1500, 1501, 1502, 1513, 1507, 1503,...
$ UniqueCarrier     <chr> "American", "American", "American",...
$ FlightNum         <int> 428, 428, 428, 428, 428, 428, 428, ...
$ TailNum           <chr> "N576AA", "N557AA", "N541AA", "N403...
$ ActualElapsedTime <int> 60, 60, 70, 70, 62, 64, 70, 59, 71,...
$ AirTime           <int> 40, 45, 48, 39, 44, 45, 43, 40, 41,...
$ ArrDelay          <int> -10, -9, -8, 3, -3, -7, -1, -16, 44...
$ DepDelay          <int> 0, 1, -8, 3, 5, -1, -1, -5, 43, 43,...
$ Origin            <chr> "IAH", "IAH", "IAH", "IAH", "IAH", ...
$ Dest              <chr> "DFW", "DFW", "DFW", "DFW", "DFW", ...
$ Distance          <int> 224, 224, 224, 224, 224, 224, 224, ...
$ TaxiIn            <int> 7, 6, 5, 9, 9, 6, 12, 7, 8, 6, 8, 4...
$ TaxiOut           <int> 13, 9, 17, 22, 9, 13, 15, 12, 22, 1...
$ Cancelled         <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
$ CancellationCode  <chr> "", "", "", "", "", "", "", "", "",...
$ Diverted          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
$ Code              <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA,...
```

`select` and `mutate`
=====================

## The five verbs and their meaning

-   `select`; which returns a subset of the columns.
-   `filter`; that is able to return a subset of the rows.
-   `arrange`; that reorders the rows according to single or multiple variables.
-   `mutate`; used to add columns from existing data.
-   `summarise`; which reduces each group to a single row by calculating aggregate measures.

## The `select` verb

``` r
# Print out a tbl with the four columns of hflights related to delay
select(hflights, ActualElapsedTime, AirTime, ArrDelay, DepDelay)
```

    ## # A tibble: 227,496 × 4
    ##    ActualElapsedTime AirTime ArrDelay DepDelay
    ## *              <int>   <int>    <int>    <int>
    ## 1                 60      40      -10        0
    ## 2                 60      45       -9        1
    ## 3                 70      48       -8       -8
    ## 4                 70      39        3        3
    ## 5                 62      44       -3        5
    ## 6                 64      45       -7       -1
    ## 7                 70      43       -1       -1
    ## 8                 59      40      -16       -5
    ## 9                 71      41       44       43
    ## 10                70      45       43       43
    ## # ... with 227,486 more rows

``` r
# Print out hflights, nothing has changed!
hflights
```

    ## # A tibble: 227,496 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ## *  <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1          1         6    1400    1500      American
    ## 2   2011     1          2         7    1401    1501      American
    ## 3   2011     1          3         1    1352    1502      American
    ## 4   2011     1          4         2    1403    1513      American
    ## 5   2011     1          5         3    1405    1507      American
    ## 6   2011     1          6         4    1359    1503      American
    ## 7   2011     1          7         5    1359    1509      American
    ## 8   2011     1          8         6    1355    1454      American
    ## 9   2011     1          9         7    1443    1554      American
    ## 10  2011     1         10         1    1443    1553      American
    ## # ... with 227,486 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# Print out the columns Origin up to Cancelled of hflights
select(hflights, 14:19)
```

    ## # A tibble: 227,496 × 6
    ##    Origin  Dest Distance TaxiIn TaxiOut Cancelled
    ## *   <chr> <chr>    <int>  <int>   <int>     <int>
    ## 1     IAH   DFW      224      7      13         0
    ## 2     IAH   DFW      224      6       9         0
    ## 3     IAH   DFW      224      5      17         0
    ## 4     IAH   DFW      224      9      22         0
    ## 5     IAH   DFW      224      9       9         0
    ## 6     IAH   DFW      224      6      13         0
    ## 7     IAH   DFW      224     12      15         0
    ## 8     IAH   DFW      224      7      12         0
    ## 9     IAH   DFW      224      8      22         0
    ## 10    IAH   DFW      224      6      19         0
    ## # ... with 227,486 more rows

``` r
# Answer to last question: be concise!
select(hflights, 1:4, 12:21)
```

    ## # A tibble: 227,496 × 14
    ##     Year Month DayofMonth DayOfWeek ArrDelay DepDelay Origin  Dest
    ## *  <int> <int>      <int>     <int>    <int>    <int>  <chr> <chr>
    ## 1   2011     1          1         6      -10        0    IAH   DFW
    ## 2   2011     1          2         7       -9        1    IAH   DFW
    ## 3   2011     1          3         1       -8       -8    IAH   DFW
    ## 4   2011     1          4         2        3        3    IAH   DFW
    ## 5   2011     1          5         3       -3        5    IAH   DFW
    ## 6   2011     1          6         4       -7       -1    IAH   DFW
    ## 7   2011     1          7         5       -1       -1    IAH   DFW
    ## 8   2011     1          8         6      -16       -5    IAH   DFW
    ## 9   2011     1          9         7       44       43    IAH   DFW
    ## 10  2011     1         10         1       43       43    IAH   DFW
    ## # ... with 227,486 more rows, and 6 more variables: Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

## Helper functions for variable selection

`select`:

-   `starts_with("X")`; every name that starts with `"X"`,
-   `ends_with("X")`; every name that ends with `"X"`,
-   `contains("X")`; every name that contains `"X"`,
-   `matches("X")`; every name that matches `"X"`, where `"X"` can be a regular expression,
-   `num_range("x", 1:5)`; the variables named `x01`, `x02`, `x03`, `x04` and `x05`,
-   `one_of(x)`; every name that appears in `x`, which should be a character vector.

<!-- -->

``` r
# Print out a tbl containing just ArrDelay and DepDelay
select(hflights, ArrDelay, DepDelay)
```

    ## # A tibble: 227,496 × 2
    ##    ArrDelay DepDelay
    ## *     <int>    <int>
    ## 1       -10        0
    ## 2        -9        1
    ## 3        -8       -8
    ## 4         3        3
    ## 5        -3        5
    ## 6        -7       -1
    ## 7        -1       -1
    ## 8       -16       -5
    ## 9        44       43
    ## 10       43       43
    ## # ... with 227,486 more rows

``` r
# Print out a tbl as described in the second instruction, using both helper functions and variable names
select(hflights, UniqueCarrier, ends_with('Num'), starts_with('Cancel'))
```

    ## # A tibble: 227,496 × 5
    ##    UniqueCarrier FlightNum TailNum Cancelled CancellationCode
    ## *          <chr>     <int>   <chr>     <int>            <chr>
    ## 1       American       428  N576AA         0                 
    ## 2       American       428  N557AA         0                 
    ## 3       American       428  N541AA         0                 
    ## 4       American       428  N403AA         0                 
    ## 5       American       428  N492AA         0                 
    ## 6       American       428  N262AA         0                 
    ## 7       American       428  N493AA         0                 
    ## 8       American       428  N477AA         0                 
    ## 9       American       428  N476AA         0                 
    ## 10      American       428  N504AA         0                 
    ## # ... with 227,486 more rows

``` r
# Print out a tbl as described in the third instruction, using only helper functions.
select(hflights, ends_with('Time'), ends_with('Delay'))
```

    ## # A tibble: 227,496 × 6
    ##    DepTime ArrTime ActualElapsedTime AirTime ArrDelay DepDelay
    ## *    <int>   <int>             <int>   <int>    <int>    <int>
    ## 1     1400    1500                60      40      -10        0
    ## 2     1401    1501                60      45       -9        1
    ## 3     1352    1502                70      48       -8       -8
    ## 4     1403    1513                70      39        3        3
    ## 5     1405    1507                62      44       -3        5
    ## 6     1359    1503                64      45       -7       -1
    ## 7     1359    1509                70      43       -1       -1
    ## 8     1355    1454                59      40      -16       -5
    ## 9     1443    1554                71      41       44       43
    ## 10    1443    1553                70      45       43       43
    ## # ... with 227,486 more rows

## Comparison to basic R

``` r
# add
ex1r <- hflights[c('TaxiIn','TaxiOut','Distance')]

ex1d <- select(hflights, starts_with('Taxi'), Distance)

ex2r <- hflights[c('Year','Month','DayOfWeek','DepTime','ArrTime')]

ex2d <- select(hflights, Year, Month, DayOfWeek, DepTime, ArrTime)

ex3r <- hflights[c('TailNum','TaxiIn','TaxiOut')]

ex3d <- select(hflights, TailNum, starts_with('Taxi'))
```

## `mutate` is creating

``` r
# Add the new variable ActualGroundTime to a copy of hflights and save the result as g1
g1 <- mutate(hflights, ActualGroundTime = ActualElapsedTime - AirTime)
glimpse(hflights)
```

    ## Observations: 227,496
    ## Variables: 21
    ## $ Year              <int> 2011, 2011, 2011, 2011, 2011, 2011, 2011, 20...
    ## $ Month             <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
    ## $ DayofMonth        <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 1...
    ## $ DayOfWeek         <int> 6, 7, 1, 2, 3, 4, 5, 6, 7, 1, 2, 3, 4, 5, 6,...
    ## $ DepTime           <int> 1400, 1401, 1352, 1403, 1405, 1359, 1359, 13...
    ## $ ArrTime           <int> 1500, 1501, 1502, 1513, 1507, 1503, 1509, 14...
    ## $ UniqueCarrier     <chr> "American", "American", "American", "America...
    ## $ FlightNum         <int> 428, 428, 428, 428, 428, 428, 428, 428, 428,...
    ## $ TailNum           <chr> "N576AA", "N557AA", "N541AA", "N403AA", "N49...
    ## $ ActualElapsedTime <int> 60, 60, 70, 70, 62, 64, 70, 59, 71, 70, 70, ...
    ## $ AirTime           <int> 40, 45, 48, 39, 44, 45, 43, 40, 41, 45, 42, ...
    ## $ ArrDelay          <int> -10, -9, -8, 3, -3, -7, -1, -16, 44, 43, 29,...
    ## $ DepDelay          <int> 0, 1, -8, 3, 5, -1, -1, -5, 43, 43, 29, 19, ...
    ## $ Origin            <chr> "IAH", "IAH", "IAH", "IAH", "IAH", "IAH", "I...
    ## $ Dest              <chr> "DFW", "DFW", "DFW", "DFW", "DFW", "DFW", "D...
    ## $ Distance          <int> 224, 224, 224, 224, 224, 224, 224, 224, 224,...
    ## $ TaxiIn            <int> 7, 6, 5, 9, 9, 6, 12, 7, 8, 6, 8, 4, 6, 5, 6...
    ## $ TaxiOut           <int> 13, 9, 17, 22, 9, 13, 15, 12, 22, 19, 20, 11...
    ## $ Cancelled         <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ CancellationCode  <chr> "", "", "", "", "", "", "", "", "", "", "", ...
    ## $ Diverted          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...

``` r
glimpse(g1)
```

    ## Observations: 227,496
    ## Variables: 22
    ## $ Year              <int> 2011, 2011, 2011, 2011, 2011, 2011, 2011, 20...
    ## $ Month             <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
    ## $ DayofMonth        <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 1...
    ## $ DayOfWeek         <int> 6, 7, 1, 2, 3, 4, 5, 6, 7, 1, 2, 3, 4, 5, 6,...
    ## $ DepTime           <int> 1400, 1401, 1352, 1403, 1405, 1359, 1359, 13...
    ## $ ArrTime           <int> 1500, 1501, 1502, 1513, 1507, 1503, 1509, 14...
    ## $ UniqueCarrier     <chr> "American", "American", "American", "America...
    ## $ FlightNum         <int> 428, 428, 428, 428, 428, 428, 428, 428, 428,...
    ## $ TailNum           <chr> "N576AA", "N557AA", "N541AA", "N403AA", "N49...
    ## $ ActualElapsedTime <int> 60, 60, 70, 70, 62, 64, 70, 59, 71, 70, 70, ...
    ## $ AirTime           <int> 40, 45, 48, 39, 44, 45, 43, 40, 41, 45, 42, ...
    ## $ ArrDelay          <int> -10, -9, -8, 3, -3, -7, -1, -16, 44, 43, 29,...
    ## $ DepDelay          <int> 0, 1, -8, 3, 5, -1, -1, -5, 43, 43, 29, 19, ...
    ## $ Origin            <chr> "IAH", "IAH", "IAH", "IAH", "IAH", "IAH", "I...
    ## $ Dest              <chr> "DFW", "DFW", "DFW", "DFW", "DFW", "DFW", "D...
    ## $ Distance          <int> 224, 224, 224, 224, 224, 224, 224, 224, 224,...
    ## $ TaxiIn            <int> 7, 6, 5, 9, 9, 6, 12, 7, 8, 6, 8, 4, 6, 5, 6...
    ## $ TaxiOut           <int> 13, 9, 17, 22, 9, 13, 15, 12, 22, 19, 20, 11...
    ## $ Cancelled         <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ CancellationCode  <chr> "", "", "", "", "", "", "", "", "", "", "", ...
    ## $ Diverted          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ ActualGroundTime  <int> 20, 15, 22, 31, 18, 19, 27, 19, 30, 25, 28, ...

``` r
# Add the new variable GroundTime to a g1; save the result as g2
g2 <- mutate(g1, GroundTime = TaxiIn + TaxiOut)

head(g1$ActualGroundTime == g2$GroundTime, 20)
```

    ##  [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
    ## [15] TRUE TRUE TRUE TRUE TRUE TRUE

``` r
# Add the new variable AverageSpeed to g2; save the result as g3
g3 <- mutate(g2, AverageSpeed = Distance / AirTime * 60)
g3
```

    ## # A tibble: 227,496 × 24
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1          1         6    1400    1500      American
    ## 2   2011     1          2         7    1401    1501      American
    ## 3   2011     1          3         1    1352    1502      American
    ## 4   2011     1          4         2    1403    1513      American
    ## 5   2011     1          5         3    1405    1507      American
    ## 6   2011     1          6         4    1359    1503      American
    ## 7   2011     1          7         5    1359    1509      American
    ## 8   2011     1          8         6    1355    1454      American
    ## 9   2011     1          9         7    1443    1554      American
    ## 10  2011     1         10         1    1443    1553      American
    ## # ... with 227,486 more rows, and 17 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>, ActualGroundTime <int>, GroundTime <int>,
    ## #   AverageSpeed <dbl>

## Add multiple variables using `mutate`

``` r
# Add a second variable loss_percent to the dataset: m1
m1 <- mutate(hflights, loss = ArrDelay - DepDelay, loss_percent = (ArrDelay - DepDelay)/DepDelay*100)

# Copy and adapt the previous command to reduce redendancy: m2
m2 <- mutate(hflights, loss = ArrDelay - DepDelay, loss_percent = loss/DepDelay * 100)

# Add the three variables as described in the third instruction: m3
m3 <- mutate(hflights, TotalTaxi = TaxiIn + TaxiOut, ActualGroundTime = ActualElapsedTime - AirTime, Diff = TotalTaxi - ActualGroundTime)
glimpse(m3)
```

    ## Observations: 227,496
    ## Variables: 24
    ## $ Year              <int> 2011, 2011, 2011, 2011, 2011, 2011, 2011, 20...
    ## $ Month             <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,...
    ## $ DayofMonth        <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 1...
    ## $ DayOfWeek         <int> 6, 7, 1, 2, 3, 4, 5, 6, 7, 1, 2, 3, 4, 5, 6,...
    ## $ DepTime           <int> 1400, 1401, 1352, 1403, 1405, 1359, 1359, 13...
    ## $ ArrTime           <int> 1500, 1501, 1502, 1513, 1507, 1503, 1509, 14...
    ## $ UniqueCarrier     <chr> "American", "American", "American", "America...
    ## $ FlightNum         <int> 428, 428, 428, 428, 428, 428, 428, 428, 428,...
    ## $ TailNum           <chr> "N576AA", "N557AA", "N541AA", "N403AA", "N49...
    ## $ ActualElapsedTime <int> 60, 60, 70, 70, 62, 64, 70, 59, 71, 70, 70, ...
    ## $ AirTime           <int> 40, 45, 48, 39, 44, 45, 43, 40, 41, 45, 42, ...
    ## $ ArrDelay          <int> -10, -9, -8, 3, -3, -7, -1, -16, 44, 43, 29,...
    ## $ DepDelay          <int> 0, 1, -8, 3, 5, -1, -1, -5, 43, 43, 29, 19, ...
    ## $ Origin            <chr> "IAH", "IAH", "IAH", "IAH", "IAH", "IAH", "I...
    ## $ Dest              <chr> "DFW", "DFW", "DFW", "DFW", "DFW", "DFW", "D...
    ## $ Distance          <int> 224, 224, 224, 224, 224, 224, 224, 224, 224,...
    ## $ TaxiIn            <int> 7, 6, 5, 9, 9, 6, 12, 7, 8, 6, 8, 4, 6, 5, 6...
    ## $ TaxiOut           <int> 13, 9, 17, 22, 9, 13, 15, 12, 22, 19, 20, 11...
    ## $ Cancelled         <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ CancellationCode  <chr> "", "", "", "", "", "", "", "", "", "", "", ...
    ## $ Diverted          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...
    ## $ TotalTaxi         <int> 20, 15, 22, 31, 18, 19, 27, 19, 30, 25, 28, ...
    ## $ ActualGroundTime  <int> 20, 15, 22, 31, 18, 19, 27, 19, 30, 25, 28, ...
    ## $ Diff              <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,...

`filter` and `arrange`
======================

## Logical operators

`filter`:

-   `x < y`; `TRUE` if `x` is less than `y`.
-   `x <= y`; `TRUE` if `x` is less than or equal to `y`.
-   `x == y`; `TRUE` if `x` equals `y`.
-   `x != y`; `TRUE` if `x` does not equal `y`.
-   `x >= y`; `TRUE` if `x` is greater than or equal to `y`.
-   `x > y`; `TRUE` if `x` is greater than `y`.
-   `x %in% c(a, b, c)`; `TRUE` if `x` is in the vector `c(a, b, c)`.

<!-- -->

``` r
# All flights that traveled 3000 miles or more
filter(hflights, Distance >= 3000)
```

    ## # A tibble: 527 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1         31         1     924    1413   Continental
    ## 2   2011     1         30         7     925    1410   Continental
    ## 3   2011     1         29         6    1045    1445   Continental
    ## 4   2011     1         28         5    1516    1916   Continental
    ## 5   2011     1         27         4     950    1344   Continental
    ## 6   2011     1         26         3     944    1350   Continental
    ## 7   2011     1         25         2     924    1337   Continental
    ## 8   2011     1         24         1    1144    1605   Continental
    ## 9   2011     1         23         7     926    1335   Continental
    ## 10  2011     1         22         6     942    1340   Continental
    ## # ... with 517 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# All flights flown by one of JetBlue, Southwest, or Delta
filter(hflights, UniqueCarrier %in% c('JetBlue','Southwest','Delta'))
```

    ## # A tibble: 48,679 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1          1         6     654    1124       JetBlue
    ## 2   2011     1          1         6    1639    2110       JetBlue
    ## 3   2011     1          2         7     703    1113       JetBlue
    ## 4   2011     1          2         7    1604    2040       JetBlue
    ## 5   2011     1          3         1     659    1100       JetBlue
    ## 6   2011     1          3         1    1801    2200       JetBlue
    ## 7   2011     1          4         2     654    1103       JetBlue
    ## 8   2011     1          4         2    1608    2034       JetBlue
    ## 9   2011     1          5         3     700    1103       JetBlue
    ## 10  2011     1          5         3    1544    1954       JetBlue
    ## # ... with 48,669 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# All flights where taxiing took longer than flying
filter(hflights, (TaxiIn + TaxiOut) > AirTime)
```

    ## # A tibble: 1,389 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1         24         1     731     904      American
    ## 2   2011     1         30         7    1959    2132      American
    ## 3   2011     1         24         1    1621    1749      American
    ## 4   2011     1         10         1     941    1113      American
    ## 5   2011     1         31         1    1301    1356   Continental
    ## 6   2011     1         31         1    2113    2215   Continental
    ## 7   2011     1         31         1    1434    1539   Continental
    ## 8   2011     1         31         1     900    1006   Continental
    ## 9   2011     1         30         7    1304    1408   Continental
    ## 10  2011     1         30         7    2004    2128   Continental
    ## # ... with 1,379 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

## Combining tests using boolean operators

``` r
# All flights that departed before 5am or arrived after 10pm
filter(hflights, DepTime < 500 | ArrTime > 2200)
```

    ## # A tibble: 27,799 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1          4         2    2100    2207      American
    ## 2   2011     1         14         5    2119    2229      American
    ## 3   2011     1         10         1    1934    2235      American
    ## 4   2011     1         26         3    1905    2211      American
    ## 5   2011     1         30         7    1856    2209      American
    ## 6   2011     1          9         7    1938    2228        Alaska
    ## 7   2011     1         31         1    1919    2231   Continental
    ## 8   2011     1         31         1    2116    2344   Continental
    ## 9   2011     1         31         1    1850    2211   Continental
    ## 10  2011     1         31         1    2102    2216   Continental
    ## # ... with 27,789 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# All flights that departed late but arrived ahead of schedule
filter(hflights, DepDelay > 0 & ArrDelay < 0)
```

    ## # A tibble: 27,712 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1          2         7    1401    1501      American
    ## 2   2011     1          5         3    1405    1507      American
    ## 3   2011     1         18         2    1408    1508      American
    ## 4   2011     1         18         2     721     827      American
    ## 5   2011     1         12         3    2015    2113      American
    ## 6   2011     1         13         4    2020    2116      American
    ## 7   2011     1         26         3    2009    2103      American
    ## 8   2011     1          1         6    1631    1736      American
    ## 9   2011     1         10         1    1639    1740      American
    ## 10  2011     1         12         3    1631    1739      American
    ## # ... with 27,702 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# All cancelled weekend flights
filter(hflights, DayOfWeek %in% c(6,7) & Cancelled == 1)
```

    ## # A tibble: 585 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime      UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>              <chr>
    ## 1   2011     1          9         7      NA      NA           American
    ## 2   2011     1         29         6      NA      NA        Continental
    ## 3   2011     1          9         7      NA      NA        Continental
    ## 4   2011     1          9         7      NA      NA              Delta
    ## 5   2011     1          9         7      NA      NA            SkyWest
    ## 6   2011     1          2         7      NA      NA          Southwest
    ## 7   2011     1         29         6      NA      NA              Delta
    ## 8   2011     1          9         7      NA      NA Atlantic_Southeast
    ## 9   2011     1          1         6      NA      NA            AirTran
    ## 10  2011     1          9         7      NA      NA            AirTran
    ## # ... with 575 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# All flights that were cancelled after being delayed
filter(hflights, DepDelay > 0 & Cancelled == 1)
```

    ## # A tibble: 40 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     1         26         3    1926      NA   Continental
    ## 2   2011     1         11         2    1100      NA    US_Airways
    ## 3   2011     1         19         3    1811      NA    ExpressJet
    ## 4   2011     1          7         5    2028      NA    ExpressJet
    ## 5   2011     2          4         5    1638      NA      American
    ## 6   2011     2          8         2    1057      NA   Continental
    ## 7   2011     2          2         3     802      NA    ExpressJet
    ## 8   2011     2          9         3     904      NA    ExpressJet
    ## 9   2011     2          1         2    1508      NA       SkyWest
    ## 10  2011     3         31         4    1016      NA   Continental
    ## # ... with 30 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

## Blend together

``` r
# Select the flights that had JFK as their destination: c1
c1 <- filter(hflights, Dest == 'JFK')

# Combine the Year, Month and DayofMonth variables to create a Date column: c2
c2 <- mutate(c1, Date = paste(Year, Month, DayofMonth, sep = '-'))

# Print out a selection of columns of c2
select(c2, Date, DepTime, ArrTime, TailNum)
```

    ## # A tibble: 695 × 4
    ##        Date DepTime ArrTime TailNum
    ##       <chr>   <int>   <int>   <chr>
    ## 1  2011-1-1     654    1124  N324JB
    ## 2  2011-1-1    1639    2110  N324JB
    ## 3  2011-1-2     703    1113  N324JB
    ## 4  2011-1-2    1604    2040  N324JB
    ## 5  2011-1-3     659    1100  N229JB
    ## 6  2011-1-3    1801    2200  N206JB
    ## 7  2011-1-4     654    1103  N267JB
    ## 8  2011-1-4    1608    2034  N267JB
    ## 9  2011-1-5     700    1103  N708JB
    ## 10 2011-1-5    1544    1954  N644JB
    ## # ... with 685 more rows

## Arranging your data

``` r
# Definition of dtc
dtc <- filter(hflights, Cancelled == 1, !is.na(DepDelay))

# Arrange dtc by departure delays
arrange(dtc, DepDelay)
```

    ## # A tibble: 68 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime  UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>          <chr>
    ## 1   2011     7         23         6     605      NA       Frontier
    ## 2   2011     1         17         1     916      NA     ExpressJet
    ## 3   2011    12          1         4     541      NA     US_Airways
    ## 4   2011    10         12         3    2022      NA American_Eagle
    ## 5   2011     7         29         5    1424      NA    Continental
    ## 6   2011     9         29         4    1639      NA        SkyWest
    ## 7   2011     2          9         3     555      NA American_Eagle
    ## 8   2011     5          9         1     715      NA        SkyWest
    ## 9   2011     1         20         4    1413      NA         United
    ## 10  2011     1         17         1     831      NA      Southwest
    ## # ... with 58 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# Arrange dtc so that cancellation reasons are grouped
arrange(dtc, CancellationCode)
```

    ## # A tibble: 68 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime  UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>          <chr>
    ## 1   2011     1         20         4    1413      NA         United
    ## 2   2011     1          7         5    2028      NA     ExpressJet
    ## 3   2011     2          4         5    1638      NA       American
    ## 4   2011     2          8         2    1057      NA    Continental
    ## 5   2011     2          1         2    1508      NA        SkyWest
    ## 6   2011     2         21         1    2257      NA        SkyWest
    ## 7   2011     2          9         3     555      NA American_Eagle
    ## 8   2011     3         18         5     727      NA         United
    ## 9   2011     4          4         1    1632      NA          Delta
    ## 10  2011     4          8         5    1608      NA      Southwest
    ## # ... with 58 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# Arrange dtc according to carrier and departure delays
arrange(dtc, UniqueCarrier, DepDelay)
```

    ## # A tibble: 68 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime      UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>              <chr>
    ## 1   2011     6         11         6    1649      NA            AirTran
    ## 2   2011     8         18         4    1808      NA           American
    ## 3   2011     2          4         5    1638      NA           American
    ## 4   2011    10         12         3    2022      NA     American_Eagle
    ## 5   2011     2          9         3     555      NA     American_Eagle
    ## 6   2011     7         17         7    1917      NA     American_Eagle
    ## 7   2011     4         30         6     612      NA Atlantic_Southeast
    ## 8   2011     4         10         7    1147      NA Atlantic_Southeast
    ## 9   2011     5         23         1     657      NA Atlantic_Southeast
    ## 10  2011     9         29         4     723      NA Atlantic_Southeast
    ## # ... with 58 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

## Reverse the order of arranging

``` r
# Arrange according to carrier and decreasing departure delays
arrange(hflights, UniqueCarrier, desc(DepDelay))
```

    ## # A tibble: 227,496 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     2         19         6    1902    2143       AirTran
    ## 2   2011     3         14         1    2024    2309       AirTran
    ## 3   2011     2         16         3    2349     227       AirTran
    ## 4   2011    11         13         7    2312     213       AirTran
    ## 5   2011     5         26         4    2353     305       AirTran
    ## 6   2011     5         26         4    1922    2229       AirTran
    ## 7   2011     4         28         4    1045    1328       AirTran
    ## 8   2011     6          5         7    2207      52       AirTran
    ## 9   2011     5          7         6    1009    1256       AirTran
    ## 10  2011     7         25         1    2107      14       AirTran
    ## # ... with 227,486 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# Arrange flights by total delay (normal order).
arrange(hflights, (ArrDelay+DepDelay))
```

    ## # A tibble: 227,496 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>         <chr>
    ## 1   2011     7          3         7    1914    2039    ExpressJet
    ## 2   2011     8         31         3     934    1039       SkyWest
    ## 3   2011     8         21         7     935    1039       SkyWest
    ## 4   2011     8         28         7    2059    2206       SkyWest
    ## 5   2011     8         29         1     935    1041       SkyWest
    ## 6   2011    12         25         7     741     926       SkyWest
    ## 7   2011     1         30         7     620     812       SkyWest
    ## 8   2011     8          3         3    1741    1810    ExpressJet
    ## 9   2011     8          4         4     930    1041       SkyWest
    ## 10  2011     8         18         4     939    1043       SkyWest
    ## # ... with 227,486 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

``` r
# Keep flights leaving to DFW before 8am and arrange according to decreasing AirTime 
arrange(filter(hflights, Dest == 'DFW' & DepTime<800), desc(AirTime))
```

    ## # A tibble: 799 × 21
    ##     Year Month DayofMonth DayOfWeek DepTime ArrTime  UniqueCarrier
    ##    <int> <int>      <int>     <int>   <int>   <int>          <chr>
    ## 1   2011    11         22         2     635     825       American
    ## 2   2011     8         25         4     602     758 American_Eagle
    ## 3   2011    10         12         3     559     738 American_Eagle
    ## 4   2011     5          2         1     716     854       American
    ## 5   2011     4          4         1     741     949       American
    ## 6   2011     4          4         1     627     742 American_Eagle
    ## 7   2011     6         21         2     726     848     ExpressJet
    ## 8   2011     9          1         4     715     844       American
    ## 9   2011     3         14         1     729     917    Continental
    ## 10  2011    12          5         1     724     847    Continental
    ## # ... with 789 more rows, and 14 more variables: FlightNum <int>,
    ## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
    ## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
    ## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
    ## #   Diverted <int>

`summarise` and the Pipe Operator
=================================

## The syntax of `summarise`

``` r
# Print out a summary with variables min_dist and max_dist
summarise(hflights, min_dist = min(Distance), max_dist = max(Distance))
```

    ## # A tibble: 1 × 2
    ##   min_dist max_dist
    ##      <int>    <int>
    ## 1       79     3904

``` r
# Print out a summary with variable max_div
summarise(filter(hflights, Diverted == 1), max_div = max(Distance))
```

    ## # A tibble: 1 × 1
    ##   max_div
    ##     <int>
    ## 1    3904

## Aggregate functions

`summarise`:

-   `min(x)`; minimum value of vector x.
-   `max(x)`; maximum value of vector x.
-   `mean(x)`; mean value of vector x.
-   `median(x)`; median value of vector x.
-   `quantile(x, p)`; pth quantile of vector x.
-   `sd(x)`; standard deviation of vector x.
-   `var(x)`; variance of vector x.
-   `IQR(x)`; Inter Quartile Range (IQR) of vector x.
-   `diff(range(x))`; total range of vector x.

<!-- -->

``` r
# Remove rows that have NA ArrDelay: temp1
temp1 <- filter(hflights, !is.na(ArrDelay))

# Generate summary about ArrDelay column of temp1
summarise(temp1, earliest = min(ArrDelay), average = mean(ArrDelay), latest = max(ArrDelay), sd = sd(ArrDelay))
```

    ## # A tibble: 1 × 4
    ##   earliest  average latest       sd
    ##      <int>    <dbl>  <int>    <dbl>
    ## 1      -70 7.094334    978 30.70852

``` r
# Keep rows that have no NA TaxiIn and no NA TaxiOut: temp2
temp2 <- filter(hflights, !is.na(TaxiIn), !is.na(TaxiOut))

# Print the maximum taxiing difference of temp2 with summarise()
summarise(temp2, max_taxi_diff = max(abs(TaxiIn - TaxiOut)))
```

    ## # A tibble: 1 × 1
    ##   max_taxi_diff
    ##           <int>
    ## 1           160

## `dplyr` aggregate functions

-   `first(x)`; the first element of vector `x`.
-   `last(x)`; the last element of vector `x`.
-   `nth(x, n)`; The `n`th element of vector `x`.
-   `n()`; The number of rows in the data.frame or group of observations that `summarise()` describes.
-   `n_distinct(x)`; The number of unique values in vector `x`.

<!-- -->

``` r
# Generate summarizing statistics for hflights
summarise(hflights, n_obs = n(), n_carrier = n_distinct(UniqueCarrier), n_dest = n_distinct(Dest), dest100 = nth(Dest, 100))
```

    ## # A tibble: 1 × 4
    ##    n_obs n_carrier n_dest dest100
    ##    <int>     <int>  <int>   <chr>
    ## 1 227496        15    116     DFW

``` r
# Filter hflights to keep all American Airline flights: aa
aa <- filter(hflights, UniqueCarrier == 'American')

# Generate summarizing statistics for aa 
summarise(aa, n_flights = n(), n_canc = sum(Cancelled), p_canc = n_canc/n_flights*100, avg_delay = mean(ArrDelay, na.rm = TRUE))
```

    ## # A tibble: 1 × 4
    ##   n_flights n_canc   p_canc avg_delay
    ##       <int>  <int>    <dbl>     <dbl>
    ## 1      3244     60 1.849568 0.8917558

**Overview of syntax**

``` r
# Write the 'piped' version of the English sentences
hflights %>%
    mutate(diff = TaxiOut - TaxiIn) %>%
    filter(!is.na(diff)) %>%
    summarise( avg = mean(diff))
```

    ## # A tibble: 1 × 1
    ##        avg
    ##      <dbl>
    ## 1 8.992064

**Drive or fly? Part 1 of 2**

``` r
# Part 1, concerning the selection and creation of columns
d <- hflights %>%
  select(Dest, UniqueCarrier, Distance, ActualElapsedTime) %>%  
  mutate(RealTime = ActualElapsedTime + 100, mph = Distance / RealTime * 60)

# Part 2, concerning flights that had an actual average speed of < 70 mph.
d %>%
  filter(!is.na(mph), mph < 70) %>%
  summarise( n_less = n(), 
             n_dest = n_distinct(Dest), 
             min_dist = min(Distance), 
             max_dist = max(Distance))
```

    ## # A tibble: 1 × 4
    ##   n_less n_dest min_dist max_dist
    ##    <int>  <int>    <int>    <int>
    ## 1   6726     13       79      305

**Drive or fly? Part 2 of 2**

``` r
# Solve the exercise using a combination of dplyr verbs and %>%
hflights %>%
    #summarise(all_flights = n()) %>%
    filter(((Distance / (ActualElapsedTime + 100) * 60) < 105) | Cancelled == 1 | Diverted == 1) %>%
    summarise(n_non = n(), p_non = n_non / 22751 *100, n_dest = n_distinct(Dest), min_dist = min(Distance), max_dist = max(Distance))
```

    ## # A tibble: 1 × 5
    ##   n_non    p_non n_dest min_dist max_dist
    ##   <int>    <dbl>  <int>    <int>    <int>
    ## 1 42400 186.3654    113       79     3904

**Advanced piping exercise**

``` r
# Count the number of overnight flights
hflights %>%
    filter(ArrTime < DepTime & !is.na(DepTime) & !is.na(ArrTime)) %>%
    summarise(n = n())
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1  2718

`group_by` and working with data
================================

## Unite and conquer using `group_by`

``` r
# Make an ordered per-carrier summary of hflights
hflights %>%
   group_by(UniqueCarrier) %>%
   summarise(n_flights = n(), 
             n_canc = sum(Cancelled == 1), 
             p_canc = mean(Cancelled == 1) * 100, 
             avg_delay = mean(ArrDelay, na.rm = TRUE)) %>%
   arrange(avg_delay, p_canc)
```

    ## # A tibble: 15 × 5
    ##         UniqueCarrier n_flights n_canc    p_canc  avg_delay
    ##                 <chr>     <int>  <int>     <dbl>      <dbl>
    ## 1          US_Airways      4082     46 1.1268986 -0.6307692
    ## 2            American      3244     60 1.8495684  0.8917558
    ## 3             AirTran      2139     21 0.9817672  1.8536239
    ## 4              Alaska       365      0 0.0000000  3.1923077
    ## 5                Mesa        79      1 1.2658228  4.0128205
    ## 6               Delta      2641     42 1.5903067  6.0841374
    ## 7         Continental     70032    475 0.6782614  6.0986983
    ## 8      American_Eagle      4648    135 2.9044750  7.1529751
    ## 9  Atlantic_Southeast      2204     76 3.4482759  7.2569543
    ## 10          Southwest     45343    703 1.5504047  7.5871430
    ## 11           Frontier       838      6 0.7159905  7.6682692
    ## 12         ExpressJet     73053   1132 1.5495599  8.1865242
    ## 13            SkyWest     16061    224 1.3946828  8.6934922
    ## 14            JetBlue       695     18 2.5899281  9.8588410
    ## 15             United      2072     34 1.6409266 10.4628628

``` r
# Make an ordered per-day summary of hflights
hflights %>% 
   group_by(DayOfWeek) %>%
   summarise(avg_taxi = mean(TaxiIn + TaxiOut, na.rm=TRUE)) %>%
   arrange(desc(avg_taxi))
```

    ## # A tibble: 7 × 2
    ##   DayOfWeek avg_taxi
    ##       <int>    <dbl>
    ## 1         1 21.77027
    ## 2         2 21.43505
    ## 3         4 21.26076
    ## 4         3 21.19055
    ## 5         5 21.15805
    ## 6         7 20.93726
    ## 7         6 20.43061

## Combine `group_by` with `mutate`

``` r
# Solution to first instruction
hflights %>%
    filter(!is.na(ArrDelay)) %>%
    group_by(UniqueCarrier) %>%
    summarise(p_delay = sum(ArrDelay > 0) / n()) %>%
    mutate(rank = rank(p_delay)) %>%
    arrange(rank)
```

    ## # A tibble: 15 × 3
    ##         UniqueCarrier   p_delay  rank
    ##                 <chr>     <dbl> <dbl>
    ## 1            American 0.3030208     1
    ## 2             AirTran 0.3112269     2
    ## 3          US_Airways 0.3267990     3
    ## 4  Atlantic_Southeast 0.3677511     4
    ## 5      American_Eagle 0.3696714     5
    ## 6               Delta 0.3871092     6
    ## 7             JetBlue 0.3952452     7
    ## 8              Alaska 0.4368132     8
    ## 9           Southwest 0.4644557     9
    ## 10               Mesa 0.4743590    10
    ## 11        Continental 0.4907385    11
    ## 12         ExpressJet 0.4943420    12
    ## 13             United 0.4963109    13
    ## 14            SkyWest 0.5350105    14
    ## 15           Frontier 0.5564904    15

``` r
# Solution to second instruction
hflights %>%
    filter(!is.na(ArrDelay), ArrDelay > 0) %>%
    group_by(UniqueCarrier) %>%
    summarise(avg = mean(ArrDelay)) %>%
    mutate(rank = rank(avg)) %>%
    arrange(rank)
```

    ## # A tibble: 15 × 3
    ##         UniqueCarrier      avg  rank
    ##                 <chr>    <dbl> <dbl>
    ## 1                Mesa 18.67568     1
    ## 2            Frontier 18.68683     2
    ## 3          US_Airways 20.70235     3
    ## 4         Continental 22.13374     4
    ## 5              Alaska 22.91195     5
    ## 6             SkyWest 24.14663     6
    ## 7          ExpressJet 24.19337     7
    ## 8           Southwest 25.27750     8
    ## 9             AirTran 27.85693     9
    ## 10           American 28.49740    10
    ## 11              Delta 32.12463    11
    ## 12             United 32.48067    12
    ## 13     American_Eagle 38.75135    13
    ## 14 Atlantic_Southeast 40.24231    14
    ## 15            JetBlue 45.47744    15

## Advanced `group_by`

``` r
# Which plane (by tail number) flew out of Houston the most times? How many times? adv1
adv1 <- hflights %>%
          group_by(TailNum) %>%
          summarise(n = n()) %>%
          filter(n == max(n))

# How many airplanes only flew to one destination from Houston? adv2
adv2 <- hflights %>%
          group_by(TailNum) %>%
          summarise(ndest = n_distinct(Dest)) %>%
          filter(ndest == 1) %>%
          summarise(nplanes = n())

# Find the most visited destination for each carrier: adv3
adv3 <- hflights %>% 
          group_by(UniqueCarrier, Dest) %>%
          summarise(n = n()) %>%
          mutate(rank = rank(desc(n))) %>%
          filter(rank == 1)

# Find the carrier that travels to each destination the most: adv4
adv4 <- hflights %>% 
          group_by(Dest, UniqueCarrier) %>%
          summarise(n = n()) %>%
          mutate(rank = rank(desc(n))) %>%
          filter(rank == 1)
```

## `dplyr` deals with different types

``` r
# Use summarise to calculate n_carrier
s2 <- hflights %>%
    summarise(n_carrier = n_distinct(UniqueCarrier))
```

## `dplyr` and mySQL databases

Code only.

``` r
# set up a src that connects to the mysql database (src_mysql is provided by dplyr)
my_db <- src_mysql(dbname = 'dplyr', 
                  host = 'dplyr.csrrinzqubik.us-east-1.rds.amazonaws.com', 
                  port = 3306,
                  user = 'dplyr',
                  password = 'dplyr')

# and reference a table within that src: nycflights is now available as an R object that references to the remote nycflights table
nycflights <- tbl(my_db, 'dplyr')

# glimpse at nycflights
glimpse(nycflights)

# Calculate the grouped summaries detailed in the instructions
nycflights %>%
   group_by(carrier) %>%
   summarise(n_flights = n(), avg_delay = mean(arr_delay)) %>%
   arrange(avg_delay)
```

Adding `tidyr` Functions
========================

-   `complete`.
-   `drop_na`.
-   `expand`.
-   `extract`.
-   `extract_numeric`.
-   `complete`.
-   `fill`.
-   `full_seq`.
-   `gather`.
-   `nest`.
-   `replace_na`.
-   `separate`.
-   `separate_rows`.
-   `separate_rows_`.
-   `smiths`.
-   `spread`.
-   `table1`.
-   `unite`.
-   `unnest`.
-   `who`.

------------------------------------------------------------------------

Joining Data in R with `dplyr`
==============================

*In development*

``` text
## 1, Mutating Joins

## 2, Filtering Joins and Set Operations

## 3, Assembling Data

## 4, Advanced Joining

## 5, Case Study
```
