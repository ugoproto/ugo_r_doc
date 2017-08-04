-   [`apply` to Matrices and Arrays](#apply-to-matrices-and-arrays)
    -   [`sweep`](#sweep)
    -   [`aggregate`](#aggregate)
    -   [`by`](#by)
    -   [`split`](#split)
    -   [`strsplit`](#strsplit)
    -   [`Vectorize`](#vectorize)
-   [`lapply`: list-apply](#lapply-list-apply)
    -   [`sapply`: simplify-list-apply](#sapply-simplify-list-apply)
    -   [`vapply`](#vapply)
    -   [`rapply`: recursive-list-apply](#rapply-recursive-list-apply)
-   [`mapply`: multivariate-apply](#mapply-multivariate-apply)
    -   [`Vectorize`](#vectorize-1)
-   [And more](#and-more)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

-   Manipulate all data classes.
-   Transform one data class into another class
-   Avoid explicit use of loop constructs and speed up the code.
-   Aggregate or subset the data.

`apply` to Matrices and Arrays
------------------------------

Flatten a matrix into a vector.

``` r
# Dataset
X <- matrix(rnorm(30), nrow = 4, ncol = 4)
X
```

    ##            [,1]       [,2]       [,3]       [,4]
    ## [1,] -0.1902972 -0.3250492  0.1597852 -0.4878550
    ## [2,] -0.4195968 -1.8485172 -0.1185904  0.2307813
    ## [3,] -0.6387653 -0.7848382 -0.4198662 -0.6314304
    ## [4,]  0.7374748 -2.0634558  0.2374686 -1.6572091

``` r
# Sum the values of each column
apply(X, 2, sum)
```

    ## [1] -0.5111845 -5.0218604 -0.1412028 -2.5457132

-   `X` is the array or matrix (2D, 3D, etc.).
-   `MARGIN=1` for row, `2` for column.
-   `FUN=sum`, `mean`, `median`, `min`, `max`, `var`, `sd`, `range`,
    `length`, `skew`, `kurtosis`, `abs`, `round`, `tolower`,
    `toupper`, etc.
-   user-defined function such as
    `FUN = function(x) { c(m = mean(x), s = sd(x)) }`.

<!-- -->

``` r
# More examples
apply(X, 2, max)
```

    ## [1]  0.7374748 -0.3250492  0.2374686  0.2307813

``` r
apply(X, 2, range)
```

    ##            [,1]       [,2]       [,3]       [,4]
    ## [1,] -0.6387653 -2.0634558 -0.4198662 -1.6572091
    ## [2,]  0.7374748 -0.3250492  0.2374686  0.2307813

``` r
apply(X, 2, abs)
```

    ##           [,1]      [,2]      [,3]      [,4]
    ## [1,] 0.1902972 0.3250492 0.1597852 0.4878550
    ## [2,] 0.4195968 1.8485172 0.1185904 0.2307813
    ## [3,] 0.6387653 0.7848382 0.4198662 0.6314304
    ## [4,] 0.7374748 2.0634558 0.2374686 1.6572091

``` r
apply(X, 2, round, 2)
```

    ##       [,1]  [,2]  [,3]  [,4]
    ## [1,] -0.19 -0.33  0.16 -0.49
    ## [2,] -0.42 -1.85 -0.12  0.23
    ## [3,] -0.64 -0.78 -0.42 -0.63
    ## [4,]  0.74 -2.06  0.24 -1.66

**Lambda & custom function**

``` r
# Dataset
X
```

    ##            [,1]       [,2]       [,3]       [,4]
    ## [1,] -0.1902972 -0.3250492  0.1597852 -0.4878550
    ## [2,] -0.4195968 -1.8485172 -0.1185904  0.2307813
    ## [3,] -0.6387653 -0.7848382 -0.4198662 -0.6314304
    ## [4,]  0.7374748 -2.0634558  0.2374686 -1.6572091

``` r
# Built-in function
apply(X, 2, max)
```

    ## [1]  0.7374748 -0.3250492  0.2374686  0.2307813

``` r
# Lambda function
apply(X, 2, function(x) x + 10)
```

    ##           [,1]     [,2]      [,3]      [,4]
    ## [1,]  9.809703 9.674951 10.159785  9.512145
    ## [2,]  9.580403 8.151483  9.881410 10.230781
    ## [3,]  9.361235 9.215162  9.580134  9.368570
    ## [4,] 10.737475 7.936544 10.237469  8.342791

``` r
# Custom function
select_first <- function(x) {
    return(x[1])
}
apply(X, 2, select_first)
```

    ## [1] -0.1902972 -0.3250492  0.1597852 -0.4878550

``` r
# Custom function with two arguments
select_el <- function(x, i) { 
  x[i] 
}
apply(X, 2, select_el, i = 2)
```

    ## [1] -0.4195968 -1.8485172 -0.1185904  0.2307813

**Strings**

``` r
# Dataset
Y <- matrix(c('a', 'b', 'c', 'd'), nrow = 2, ncol = 2)
Y
```

    ##      [,1] [,2]
    ## [1,] "a"  "c" 
    ## [2,] "b"  "d"

``` r
# Change the format
apply(Y, 2, toupper)
```

    ##      [,1] [,2]
    ## [1,] "A"  "C" 
    ## [2,] "B"  "D"

### `sweep`

**Several steps**

``` r
# Dataset
dataPoints <- matrix(4:15, nrow = 4, ncol = 3)
dataPoints
```

    ##      [,1] [,2] [,3]
    ## [1,]    4    8   12
    ## [2,]    5    9   13
    ## [3,]    6   10   14
    ## [4,]    7   11   15

``` r
# Find means (center) per column with `apply()`
dataPoints_means <- apply(dataPoints, 2, mean)
dataPoints_means
```

    ## [1]  5.5  9.5 13.5

``` r
# Find standard deviation (dispersion) with `apply()`
dataPoints_sdev <- apply(dataPoints, 2, sd)
dataPoints_sdev
```

    ## [1] 1.290994 1.290994 1.290994

``` r
# Center the points; shift all the points with respect to their center
dataPoints_Trans1 <- sweep(dataPoints, 2, dataPoints_means,"-")
dataPoints_Trans1
```

    ##      [,1] [,2] [,3]
    ## [1,] -1.5 -1.5 -1.5
    ## [2,] -0.5 -0.5 -0.5
    ## [3,]  0.5  0.5  0.5
    ## [4,]  1.5  1.5  1.5

``` r
# Normalize
dataPoints_Trans2 <- sweep(dataPoints_Trans1, 2, dataPoints_sdev, "/")
dataPoints_Trans2
```

    ##            [,1]       [,2]       [,3]
    ## [1,] -1.1618950 -1.1618950 -1.1618950
    ## [2,] -0.3872983 -0.3872983 -0.3872983
    ## [3,]  0.3872983  0.3872983  0.3872983
    ## [4,]  1.1618950  1.1618950  1.1618950

``` r
sweep(dataPoints, 2, dataPoints_means, sum)
```

    ## [1] 228

-   `X`.
-   `MARGIN=1` for row, `2` for column.
-   `STATS` for the summary statistics to be swept out: `sum`, `mean`,
    `median`, `min`, `max`, `var`, `sd`, `range`, `length`, `skew`,
    `kurtosis`, `se`, etc.
-   `FUN="-"`, `"+"`, `"/"`, `"*"`, `"**"`, etc.
-   user-defined function such as
    `FUN = function(x) { c(m = mean(x), s = sd(x)) }`.

**One step**

``` r
# Normalize the data with a nested call
dataPoints_Trans <- sweep(sweep(dataPoints, 2, dataPoints_means,"-"), 2, dataPoints_sdev,"/")

dataPoints_Trans
```

    ##            [,1]       [,2]       [,3]
    ## [1,] -1.1618950 -1.1618950 -1.1618950
    ## [2,] -0.3872983 -0.3872983 -0.3872983
    ## [3,]  0.3872983  0.3872983  0.3872983
    ## [4,]  1.1618950  1.1618950  1.1618950

**Even simpler**

``` r
# Automatic data scaling
scale(dataPoints)
```

    ##            [,1]       [,2]       [,3]
    ## [1,] -1.1618950 -1.1618950 -1.1618950
    ## [2,] -0.3872983 -0.3872983 -0.3872983
    ## [3,]  0.3872983  0.3872983  0.3872983
    ## [4,]  1.1618950  1.1618950  1.1618950
    ## attr(,"scaled:center")
    ## [1]  5.5  9.5 13.5
    ## attr(,"scaled:scale")
    ## [1] 1.290994 1.290994 1.290994

### `aggregate`

``` r
# Dataset
head(Mydf, 15)
```

    ##     X DepPC DProgr Qty Delivered
    ## 1   1    90      1   7     FALSE
    ## 2   2    91      2   8      TRUE
    ## 3   3    92      3   9     FALSE
    ## 4   4    93      4  10      TRUE
    ## 5   5    94      5  11      TRUE
    ## 6   6    75      6  12     FALSE
    ## 7   7    90      7  13      TRUE
    ## 8   8    91      8  14      TRUE
    ## 9   9    92      9  15     FALSE
    ## 10 10    93     10  16     FALSE
    ## 11 11    94     11  17      TRUE
    ## 12 12    75     12  18     FALSE
    ## 13 13    90     13  19     FALSE
    ## 14 14    91     14  20      TRUE
    ## 15 15    92     15  21      TRUE

``` r
# Show data types for each column
sapply(Mydf, class)
```

    ##         X     DepPC    DProgr       Qty Delivered 
    ## "integer" "integer" "integer" "integer" "logical"

``` r
# Return number of rows and columns
dim(Mydf)
```

    ## [1] 120   5

``` r
nrow(Mydf)
```

    ## [1] 120

``` r
ncol(Mydf)
```

    ## [1] 5

``` r
# How many departments? 
unique(Mydf$DepPC)
```

    ## [1] 90 91 92 93 94 75

``` r
# Dataset
head(Mydf, 5)
```

    ##   X DepPC DProgr Qty Delivered
    ## 1 1    90      1   7     FALSE
    ## 2 2    91      2   8      TRUE
    ## 3 3    92      3   9     FALSE
    ## 4 4    93      4  10      TRUE
    ## 5 5    94      5  11      TRUE

``` r
# Aggregate by a variable (categories) and sum up another variable
aggregate(Mydf$Qty, by = Mydf["DepPC"], FUN = sum)
```

    ##   DepPC   x
    ## 1    75 878
    ## 2    90 689
    ## 3    91 684
    ## 4    92 701
    ## 5    93 707
    ## 6    94 802

``` r
# Aggregate by a variable (categories) and extract descriptive stats from another variable
aggregate(Mydf$Qty, by = Mydf["DepPC"], FUN = summary)
```

    ##   DepPC x.Min. x.1st Qu. x.Median x.Mean x.3rd Qu. x.Max.
    ## 1    75   4.00     15.25    23.50  43.90     74.50 124.00
    ## 2    90   2.00     11.75    19.50  34.45     27.25 119.00
    ## 3    91   3.00      9.00    19.00  34.20     26.25 120.00
    ## 4    92   1.00     10.00    20.00  35.05     27.25 121.00
    ## 5    93   2.00     11.00    19.00  35.35     27.25 122.00
    ## 6    94   3.00     12.00    20.00  40.10     46.50 123.00

### `by`

An alternative to `aggregate` with pros and cons.

``` r
# Dataset
head(Mydf, 5)
```

    ##   X DepPC DProgr Qty Delivered
    ## 1 1    90      1   7     FALSE
    ## 2 2    91      2   8      TRUE
    ## 3 3    92      3   9     FALSE
    ## 4 4    93      4  10      TRUE
    ## 5 5    94      5  11      TRUE

``` r
by(Mydf$Qty, Mydf["DepPC"], FUN = sum)
```

    ## DepPC: 75
    ## [1] 878
    ## -------------------------------------------------------- 
    ## DepPC: 90
    ## [1] 689
    ## -------------------------------------------------------- 
    ## DepPC: 91
    ## [1] 684
    ## -------------------------------------------------------- 
    ## DepPC: 92
    ## [1] 701
    ## -------------------------------------------------------- 
    ## DepPC: 93
    ## [1] 707
    ## -------------------------------------------------------- 
    ## DepPC: 94
    ## [1] 802

``` r
by(Mydf$Qty, Mydf["DepPC"], FUN = summary)
```

    ## DepPC: 75
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    4.00   15.25   23.50   43.90   74.50  124.00 
    ## -------------------------------------------------------- 
    ## DepPC: 90
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    2.00   11.75   19.50   34.45   27.25  119.00 
    ## -------------------------------------------------------- 
    ## DepPC: 91
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    3.00    9.00   19.00   34.20   26.25  120.00 
    ## -------------------------------------------------------- 
    ## DepPC: 92
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1.00   10.00   20.00   35.05   27.25  121.00 
    ## -------------------------------------------------------- 
    ## DepPC: 93
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    2.00   11.00   19.00   35.35   27.25  122.00 
    ## -------------------------------------------------------- 
    ## DepPC: 94
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     3.0    12.0    20.0    40.1    46.5   123.0

### `split`

``` r
# Dataset
head(Mydf, 5)
```

    ##   X DepPC DProgr Qty Delivered
    ## 1 1    90      1   7     FALSE
    ## 2 2    91      2   8      TRUE
    ## 3 3    92      3   9     FALSE
    ## 4 4    93      4  10      TRUE
    ## 5 5    94      5  11      TRUE

``` r
nrow(Mydf)
```

    ## [1] 120

``` r
# Split with a variable (categories)
split(Mydf$Qty, Mydf["DepPC"])
```

    ## $`75`
    ##  [1]  12  18  24  30  13  19 100 106 112 118 124   7  13  19  25  16  23
    ## [18]  29   4  66
    ## 
    ## $`90`
    ##  [1]   7  13  19  25  31  14  20 101 107 113 119   2   8  14  20  26  17
    ## [18]  24   4   5
    ## 
    ## $`91`
    ##  [1]   8  14  20  26   9  15  21 102 108 114 120   3   9  15  21  27  18
    ## [18]  25   3   6
    ## 
    ## $`92`
    ##  [1]   9  15  21  27  10  16  22 103 109 115 121   4  10  16  22  28  19
    ## [18]  26   1   7
    ## 
    ## $`93`
    ##  [1]  10  16  22  28  11  17  23 104 110 116 122   5  11  17  23  14  21
    ## [18]  27   2   8
    ## 
    ## $`94`
    ##  [1]  11  17  23  29  12  18  99 105 111 117 123   6  12  18  24  15  22
    ## [18]  28   3   9

``` r
# Split by row
unlist(Mydf$Qty)
```

    ##   [1]   7   8   9  10  11  12  13  14  15  16  17  18  19  20  21  22  23
    ##  [18]  24  25  26  27  28  29  30  31   9  10  11  12  13  14  15  16  17
    ##  [35]  18  19  20  21  22  23  99 100 101 102 103 104 105 106 107 108 109
    ##  [52] 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124   2   3
    ##  [69]   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19  20
    ##  [86]  21  22  23  24  25  26  27  28  14  15  16  17  18  19  21  22  23
    ## [103]  24  25  26  27  28  29   4   3   1   2   3   4   5   6   7   8   9
    ## [120]  66

``` r
split(Mydf$Qty, c(60, 120))
```

    ## $`60`
    ##  [1]   7   9  11  13  15  17  19  21  23  25  27  29  31  10  12  14  16
    ## [18]  18  20  22  99 101 103 105 107 109 111 113 115 117 119 121 123   2
    ## [35]   4   6   8  10  12  14  16  18  20  22  24  26  28  15  17  19  22
    ## [52]  24  26  28   4   1   3   5   7   9
    ## 
    ## $`120`
    ##  [1]   8  10  12  14  16  18  20  22  24  26  28  30   9  11  13  15  17
    ## [18]  19  21  23 100 102 104 106 108 110 112 114 116 118 120 122 124   3
    ## [35]   5   7   9  11  13  15  17  19  21  23  25  27  14  16  18  21  23
    ## [52]  25  27  29   3   2   4   6   8  66

### `strsplit`

``` r
# The vector pioneers
pioneers <- c('GAUSS:1777', 'BAYES:1702', 'PASCAL:1623', 'PEARSON:1857')

# Split names from birth year: split_math
split_math <- strsplit(pioneers, ':')
```

### `Vectorize`

Vectorize a scalar function.

``` r
# Scalar function
rep(c(1, 2), 2)
```

    ## [1] 1 2 1 2

``` r
# Vector function
vrep <- Vectorize(rep.int)
vrep(c(1, 2), 2)
```

    ##      [,1] [,2]
    ## [1,]    1    2
    ## [2,]    1    2

``` r
# Scalar function
f <- function(x = 1:3, y) c(x, y)
f(1:3, 1:3)
```

    ## [1] 1 2 3 1 2 3

``` r
# Vector function
vf <- Vectorize(f, SIMPLIFY = FALSE)
vf(1:3, 1:3)
```

    ## [[1]]
    ## [1] 1 1
    ## 
    ## [[2]]
    ## [1] 2 2
    ## 
    ## [[3]]
    ## [1] 3 3

`lapply`: list-apply
--------------------

For lists, vectors, and data frames.

``` r
# Dataset
A <- matrix(1:9, nrow = 3, ncol = 3)
B <- matrix(10:18, nrow = 3, ncol = 3)
C <- matrix(19:28, nrow = 3, ncol = 3)

# Create a list of matrices; an array (a 3D matrix)
MyList <- list(A,B,C)
MyList
```

    ## [[1]]
    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3]
    ## [1,]   10   13   16
    ## [2,]   11   14   17
    ## [3,]   12   15   18
    ## 
    ## [[3]]
    ##      [,1] [,2] [,3]
    ## [1,]   19   22   25
    ## [2,]   20   23   26
    ## [3,]   21   24   27

``` r
# Extract values
MyList[[1]]
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9

``` r
MyList[[1]][1,]
```

    ## [1] 1 4 7

``` r
MyList[[1]][,1]
```

    ## [1] 1 2 3

``` r
MyList[[1]][1,1]
```

    ## [1] 1

``` r
# Extract the 2nd column from `MyList` with the selection operator `[` with `lapply()`
lapply(MyList,"[", , 2)
```

    ## [[1]]
    ## [1] 4 5 6
    ## 
    ## [[2]]
    ## [1] 13 14 15
    ## 
    ## [[3]]
    ## [1] 22 23 24

``` r
# Extract the 1st row from `MyList`
lapply(MyList,"[", 1, )
```

    ## [[1]]
    ## [1] 1 4 7
    ## 
    ## [[2]]
    ## [1] 10 13 16
    ## 
    ## [[3]]
    ## [1] 19 22 25

**Lambda & custom function**

``` r
# Dataset
MyList
```

    ## [[1]]
    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3]
    ## [1,]   10   13   16
    ## [2,]   11   14   17
    ## [3,]   12   15   18
    ## 
    ## [[3]]
    ##      [,1] [,2] [,3]
    ## [1,]   19   22   25
    ## [2,]   20   23   26
    ## [3,]   21   24   27

``` r
# Built-in function
lapply(MyList, max)
```

    ## [[1]]
    ## [1] 9
    ## 
    ## [[2]]
    ## [1] 18
    ## 
    ## [[3]]
    ## [1] 27

``` r
# Lambda function
lapply(MyList, function(x) max(x) + 10)
```

    ## [[1]]
    ## [1] 19
    ## 
    ## [[2]]
    ## [1] 28
    ## 
    ## [[3]]
    ## [1] 37

``` r
# Custom function
select_first <- function(x) {
    return(x[1])
}
lapply(MyList, select_first)
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] 10
    ## 
    ## [[3]]
    ## [1] 19

``` r
# Custom function with two arguments
select_el <- function(x, i) { 
  x[i] 
}
lapply(MyList, select_el, i = 2)
```

    ## [[1]]
    ## [1] 2
    ## 
    ## [[2]]
    ## [1] 11
    ## 
    ## [[3]]
    ## [1] 20

**Strings**

``` r
# Dataset
Y
```

    ##      [,1] [,2]
    ## [1,] "a"  "c" 
    ## [2,] "b"  "d"

``` r
# Change the format
lapply(Y, toupper)
```

    ## [[1]]
    ## [1] "A"
    ## 
    ## [[2]]
    ## [1] "B"
    ## 
    ## [[3]]
    ## [1] "C"
    ## 
    ## [[4]]
    ## [1] "D"

### `sapply`: simplify-list-apply

A wrapper that simplifies `lapply`.

``` r
# Dataset
MyList
```

    ## [[1]]
    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3]
    ## [1,]   10   13   16
    ## [2,]   11   14   17
    ## [3,]   12   15   18
    ## 
    ## [[3]]
    ##      [,1] [,2] [,3]
    ## [1,]   19   22   25
    ## [2,]   20   23   26
    ## [3,]   21   24   27

``` r
# Return a list with `lapply()`
lapply(MyList,"[", 2, 1)
```

    ## [[1]]
    ## [1] 2
    ## 
    ## [[2]]
    ## [1] 11
    ## 
    ## [[3]]
    ## [1] 20

``` r
# Return a vector with `sapply()`
sapply(MyList,"[", 2, 1) # same result, but simpler
```

    ## [1]  2 11 20

``` r
# Return a list with `sapply()`
sapply(MyList,"[", 2, 1, simplify = T) # by default
```

    ## [1]  2 11 20

``` r
sapply(MyList,"[", 2, 1, simplify = F)
```

    ## [[1]]
    ## [1] 2
    ## 
    ## [[2]]
    ## [1] 11
    ## 
    ## [[3]]
    ## [1] 20

``` r
# Return a vector with `unlist()`
unlist(lapply(MyList,"[", 2, 1)) # similar
```

    ## [1]  2 11 20

**Lambda & custom function**

``` r
# Dataset
MyList
```

    ## [[1]]
    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3]
    ## [1,]   10   13   16
    ## [2,]   11   14   17
    ## [3,]   12   15   18
    ## 
    ## [[3]]
    ##      [,1] [,2] [,3]
    ## [1,]   19   22   25
    ## [2,]   20   23   26
    ## [3,]   21   24   27

``` r
# Built-in function
sapply(MyList, max)
```

    ## [1]  9 18 27

``` r
# Lambda function
lapply(MyList, function(x) max(x) + 10)
```

    ## [[1]]
    ## [1] 19
    ## 
    ## [[2]]
    ## [1] 28
    ## 
    ## [[3]]
    ## [1] 37

``` r
# Custom function
select_first <- function(x) {
    return(x[1])
}
sapply(MyList, select_first)
```

    ## [1]  1 10 19

``` r
# Custom function with two arguments
select_el <- function(x, i) { 
  x[i] 
}
sapply(MyList, select_el, i = 2)
```

    ## [1]  2 11 20

**Strings**

``` r
# Dataset
Y
```

    ##      [,1] [,2]
    ## [1,] "a"  "c" 
    ## [2,] "b"  "d"

``` r
# Change the format
sapply(Y, toupper)
```

    ##   a   b   c   d 
    ## "A" "B" "C" "D"

### `vapply`

A variant.

### `rapply`: recursive-list-apply

``` r
# Dataset
MyList
```

    ## [[1]]
    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3]
    ## [1,]   10   13   16
    ## [2,]   11   14   17
    ## [3,]   12   15   18
    ## 
    ## [[3]]
    ##      [,1] [,2] [,3]
    ## [1,]   19   22   25
    ## [2,]   20   23   26
    ## [3,]   21   24   27

``` r
# Build-in function
rapply(MyList, sqrt, classes = "ANY", how = "replace")
```

    ## [[1]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 1.000000 2.000000 2.645751
    ## [2,] 1.414214 2.236068 2.828427
    ## [3,] 1.732051 2.449490 3.000000
    ## 
    ## [[2]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 3.162278 3.605551 4.000000
    ## [2,] 3.316625 3.741657 4.123106
    ## [3,] 3.464102 3.872983 4.242641
    ## 
    ## [[3]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 4.358899 4.690416 5.000000
    ## [2,] 4.472136 4.795832 5.099020
    ## [3,] 4.582576 4.898979 5.196152

``` r
# Lambda or custom the function
rapply(MyList, function(x) x^2, classes = "ANY", how = "replace")
```

    ## [[1]]
    ##      [,1] [,2] [,3]
    ## [1,]    1   16   49
    ## [2,]    4   25   64
    ## [3,]    9   36   81
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3]
    ## [1,]  100  169  256
    ## [2,]  121  196  289
    ## [3,]  144  225  324
    ## 
    ## [[3]]
    ##      [,1] [,2] [,3]
    ## [1,]  361  484  625
    ## [2,]  400  529  676
    ## [3,]  441  576  729

``` r
select_first <- function(x) {
    return(x[1])
}
rapply(MyList, select_first, classes = "ANY", how = "replace")
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] 10
    ## 
    ## [[3]]
    ## [1] 19

``` r
# Change classes =
class(MyList) # the object
```

    ## [1] "list"

``` r
class(MyList[[1]][1]) # each entry
```

    ## [1] "integer"

``` r
rapply(MyList, sqrt, classes = "list", how = "replace") # not work
```

    ## [[1]]
    ##      [,1] [,2] [,3]
    ## [1,]    1    4    7
    ## [2,]    2    5    8
    ## [3,]    3    6    9
    ## 
    ## [[2]]
    ##      [,1] [,2] [,3]
    ## [1,]   10   13   16
    ## [2,]   11   14   17
    ## [3,]   12   15   18
    ## 
    ## [[3]]
    ##      [,1] [,2] [,3]
    ## [1,]   19   22   25
    ## [2,]   20   23   26
    ## [3,]   21   24   27

``` r
rapply(MyList, sqrt, classes = "ANY", how = "replace")
```

    ## [[1]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 1.000000 2.000000 2.645751
    ## [2,] 1.414214 2.236068 2.828427
    ## [3,] 1.732051 2.449490 3.000000
    ## 
    ## [[2]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 3.162278 3.605551 4.000000
    ## [2,] 3.316625 3.741657 4.123106
    ## [3,] 3.464102 3.872983 4.242641
    ## 
    ## [[3]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 4.358899 4.690416 5.000000
    ## [2,] 4.472136 4.795832 5.099020
    ## [3,] 4.582576 4.898979 5.196152

``` r
# Change how = 
rapply(MyList, sqrt, classes = "ANY", how = "list")
```

    ## [[1]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 1.000000 2.000000 2.645751
    ## [2,] 1.414214 2.236068 2.828427
    ## [3,] 1.732051 2.449490 3.000000
    ## 
    ## [[2]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 3.162278 3.605551 4.000000
    ## [2,] 3.316625 3.741657 4.123106
    ## [3,] 3.464102 3.872983 4.242641
    ## 
    ## [[3]]
    ##          [,1]     [,2]     [,3]
    ## [1,] 4.358899 4.690416 5.000000
    ## [2,] 4.472136 4.795832 5.099020
    ## [3,] 4.582576 4.898979 5.196152

``` r
rapply(MyList, sqrt, classes = "ANY", how = "unlist")
```

    ##  [1] 1.000000 1.414214 1.732051 2.000000 2.236068 2.449490 2.645751
    ##  [8] 2.828427 3.000000 3.162278 3.316625 3.464102 3.605551 3.741657
    ## [15] 3.872983 4.000000 4.123106 4.242641 4.358899 4.472136 4.582576
    ## [22] 4.690416 4.795832 4.898979 5.000000 5.099020 5.196152

-   Built-in or custom `function(x) x`.
-   `classes = "ANY"` for any object classes or a given object class.
    Useful when the data is mixed and we want to focus on one
    class only.
-   `how = "replace"` or `"list"`, `"unlist"`

`mapply`: multivariate-apply
----------------------------

``` r
# Create a 4x4 matrix
Q1 <- matrix(c(rep(1, 4), rep(2, 4), rep(3, 4), rep(4, 4)),4,4)
Q1
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    1    2    3    4
    ## [3,]    1    2    3    4
    ## [4,]    1    2    3    4

``` r
# Or use `mapply()`
Q2 <- mapply(rep, 1:4, 4)
Q2
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    1    2    3    4
    ## [3,]    1    2    3    4
    ## [4,]    1    2    3    4

Vectorize arguments to a function, `rep`, that is not usually accepting
vectors as arguments. Applies a function to multiple lists, `1:4`, or
multiple vector, `c()`, arguments.

### `Vectorize`

Vectorize a scaler function.

``` r
# Scalar function
rep(c(1, 2), 2)
```

    ## [1] 1 2 1 2

``` r
# Vector function
vrep <- Vectorize(rep.int)
vrep(c(1, 2), 2)
```

    ##      [,1] [,2]
    ## [1,]    1    2
    ## [2,]    1    2

``` r
# Scalar function
f <- function(x = 1:3, y) c(x, y)
f(1:3, 1:3)
```

    ## [1] 1 2 3 1 2 3

``` r
# Vector function
vf <- Vectorize(f, SIMPLIFY = FALSE)
vf(1:3, 1:3)
```

    ## [[1]]
    ## [1] 1 1
    ## 
    ## [[2]]
    ## [1] 2 2
    ## 
    ## [[3]]
    ## [1] 3 3

And more
--------

-   `tapply`.
-   `eapply`.
