------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

**`apply` functions**

``` r
# Apply Functions Over Array Margins
apply; sweep; scale

# Apply a Function over a List or Vector
lapply; sapply; vapply; replicate; rapply

# Apply a Function to Multiple List or Vector Arguments
mapply; Vectorize

# Apply a Function Over a Ragged Array
tapply

# Apply a Function to a Data Frame Split by Factors
by; aggregate; split

# Apply a Function Over Values in an Environment
eapply
```

**`apply`, `sweep`, `scale`**

``` r
# "apply" returns a vector or array or list of values obtained by applying a function to margins of an array or matrix.
(m = matrix(1:6, nrow = 2))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

``` r
apply(m, 1, sum)
```

    ## [1]  9 12

``` r
apply(m, 1:2, sqrt)
```

    ##          [,1]     [,2]     [,3]
    ## [1,] 1.000000 1.732051 2.236068
    ## [2,] 1.414214 2.000000 2.449490

``` r
# "sweep" returns an array obtained from an input array by sweeping out a summary statistic.
(X = array(1:24, dim = 4:2))
```

    ## , , 1
    ## 
    ##      [,1] [,2] [,3]
    ## [1,]    1    5    9
    ## [2,]    2    6   10
    ## [3,]    3    7   11
    ## [4,]    4    8   12
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2] [,3]
    ## [1,]   13   17   21
    ## [2,]   14   18   22
    ## [3,]   15   19   23
    ## [4,]   16   20   24

``` r
sweep(X, 1, apply(X, 1, mean))
```

    ## , , 1
    ## 
    ##      [,1] [,2] [,3]
    ## [1,]  -10   -6   -2
    ## [2,]  -10   -6   -2
    ## [3,]  -10   -6   -2
    ## [4,]  -10   -6   -2
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2] [,3]
    ## [1,]    2    6   10
    ## [2,]    2    6   10
    ## [3,]    2    6   10
    ## [4,]    2    6   10

``` r
# "scale" is generic function whose default method centers and/or scales the columns of a numeric matrix.
(mat = matrix(round(rnorm(10)*10), nrow = 5))
```

    ##      [,1] [,2]
    ## [1,]  -25   -9
    ## [2,]   -1    6
    ## [3,]   -2   21
    ## [4,]  -18  -10
    ## [5,]   13    2

``` r
scale(mat)
```

    ##            [,1]       [,2]
    ## [1,] -1.2231382 -0.8682707
    ## [2,]  0.3722595  0.3157348
    ## [3,]  0.3057846  1.4997404
    ## [4,] -0.7578139 -0.9472044
    ## [5,]  1.3029081  0.0000000
    ## attr(,"scaled:center")
    ## [1] -6.6  2.0
    ## attr(,"scaled:scale")
    ## [1] 15.04327 12.66886

``` r
apply(mat, 2, mean)
```

    ## [1] -6.6  2.0

``` r
apply(mat, 2, sd)
```

    ## [1] 15.04327 12.66886

``` r
cov(scale(mat))
```

    ##           [,1]      [,2]
    ## [1,] 1.0000000 0.5889881
    ## [2,] 0.5889881 1.0000000

**`lapply`, `sapply`, `vapply`, `replicate`, `rapply`**

``` r
# "lapply" returns a list of the same length as X, each element of which is the result of applying FUN to the corresponding element of X.
lapply(1:5, sqrt)
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] 1.414214
    ## 
    ## [[3]]
    ## [1] 1.732051
    ## 
    ## [[4]]
    ## [1] 2
    ## 
    ## [[5]]
    ## [1] 2.236068

``` r
# "sapply" is a user-friendly version of lapply by default returning a vector or matrix if appropriate.
sapply(1:5, sqrt)
```

    ## [1] 1.000000 1.414214 1.732051 2.000000 2.236068

``` r
# "vapply" is similar to sapply, but has a pre-specified type of return value, so it can be safer (and sometimes faster) to use.
vapply(1:5, sqrt, 1i)
```

    ## [1] 1.000000+0i 1.414214+0i 1.732051+0i 2.000000+0i 2.236068+0i

``` r
# "replicate" is a wrapper for the common use of sapply for repeated evaluation of an expression (which will usually involve random number generation).
replicate(3, rnorm(5))
```

    ##             [,1]       [,2]       [,3]
    ## [1,]  1.27143977  1.2557048 -1.8773681
    ## [2,]  1.39467927 -0.4677682  1.0071086
    ## [3,] -1.40587412 -0.4815548  1.6167008
    ## [4,] -0.69712942  0.6934136 -0.4936395
    ## [5,]  0.06646416 -0.4202453  0.3566716

``` r
# "rapply" is a recursive version of lapply.
(x = list(A = list(a = pi, b = list(b1 = 1)), B = "a character string"))
```

    ## $A
    ## $A$a
    ## [1] 3.141593
    ## 
    ## $A$b
    ## $A$b$b1
    ## [1] 1
    ## 
    ## 
    ## 
    ## $B
    ## [1] "a character string"

``` r
rapply(x, sqrt, classes = "numeric", how = "unlist")
```

    ##      A.a   A.b.b1 
    ## 1.772454 1.000000

``` r
rapply(x, sqrt, classes = "numeric", how = "replace")
```

    ## $A
    ## $A$a
    ## [1] 1.772454
    ## 
    ## $A$b
    ## $A$b$b1
    ## [1] 1
    ## 
    ## 
    ## 
    ## $B
    ## [1] "a character string"

``` r
rapply(x, sqrt, classes = "numeric", how = "list")
```

    ## $A
    ## $A$a
    ## [1] 1.772454
    ## 
    ## $A$b
    ## $A$b$b1
    ## [1] 1
    ## 
    ## 
    ## 
    ## $B
    ## NULL

``` r
rapply(x, sqrt, classes = "numeric", deflt = NA, how = "unlist")
```

    ##      A.a   A.b.b1        B 
    ## 1.772454 1.000000       NA

``` r
rapply(x, sqrt, classes = "numeric", deflt = NA, how = "replace")
```

    ## $A
    ## $A$a
    ## [1] 1.772454
    ## 
    ## $A$b
    ## $A$b$b1
    ## [1] 1
    ## 
    ## 
    ## 
    ## $B
    ## [1] "a character string"

``` r
rapply(x, sqrt, classes = "numeric", deflt = NA, how = "list")
```

    ## $A
    ## $A$a
    ## [1] 1.772454
    ## 
    ## $A$b
    ## $A$b$b1
    ## [1] 1
    ## 
    ## 
    ## 
    ## $B
    ## [1] NA

``` r
rapply(x, round, classes = "numeric", how = "replace", digits = 2)
```

    ## $A
    ## $A$a
    ## [1] 3.14
    ## 
    ## $A$b
    ## $A$b$b1
    ## [1] 1
    ## 
    ## 
    ## 
    ## $B
    ## [1] "a character string"

**`mapply`, `Vectorize`**

``` r
# "mapply" is a multivariate version of sapply. mapply applies FUN to the first elements of each argument, the second elements, the third elements, and so on. Arguments are recycled if necessary.
mapply(rep, LETTERS[1:3], 1:3)
```

    ## $A
    ## [1] "A"
    ## 
    ## $B
    ## [1] "B" "B"
    ## 
    ## $C
    ## [1] "C" "C" "C"

``` r
# "Vectorize" returns a new function that acts as if mapply was called.
vrep = Vectorize(rep)
vrep(LETTERS[1:3], 1:3)
```

    ## [1] "A" "B" "B" "C" "C" "C"

``` r
vrep = Vectorize(rep.int)
vrep(LETTERS[1:3], 1:3)
```

    ## $A
    ## [1] "A"
    ## 
    ## $B
    ## [1] "B" "B"
    ## 
    ## $C
    ## [1] "C" "C" "C"

**`tapply`**

``` r
# "tapply" applies a function to each cell of a ragged array, that is to each (non-empty) group of values given by a unique combination of the levels of certain factors.
(m = matrix(1:6, 2, 3))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    3    5
    ## [2,]    2    4    6

``` r
(fac = matrix(c(1,3,1,2,2,2), 2, 3))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    1    2
    ## [2,]    3    2    2

``` r
tapply(m, fac, max)
```

    ## 1 2 3 
    ## 3 6 2

**`by`, `aggregate`, `split`**

``` r
# "by" is an object-oriented wrapper for tapply applied to data frames.
by(mtcars$mpg, mtcars$cyl, max)
```

    ## mtcars$cyl: 4
    ## [1] 33.9
    ## -------------------------------------------------------- 
    ## mtcars$cyl: 6
    ## [1] 21.4
    ## -------------------------------------------------------- 
    ## mtcars$cyl: 8
    ## [1] 19.2

``` r
# "aggregate" splits the data into subsets, computes summary statistics for each, and returns the result in a convenient form.
aggregate(mpg ~ cyl, data = mtcars, max)
```

    ##   cyl  mpg
    ## 1   4 33.9
    ## 2   6 21.4
    ## 3   8 19.2

``` r
# See help for usage of NA, formula, dot notation, xtabs, nfrequency.

# "split" divides the data in the vector x into the groups defined by f. The replacement forms replace values corresponding to such a division.
split(mtcars[1], mtcars[2])
```

    ## $`4`
    ##                 mpg
    ## Datsun 710     22.8
    ## Merc 240D      24.4
    ## Merc 230       22.8
    ## Fiat 128       32.4
    ## Honda Civic    30.4
    ## Toyota Corolla 33.9
    ## Toyota Corona  21.5
    ## Fiat X1-9      27.3
    ## Porsche 914-2  26.0
    ## Lotus Europa   30.4
    ## Volvo 142E     21.4
    ## 
    ## $`6`
    ##                 mpg
    ## Mazda RX4      21.0
    ## Mazda RX4 Wag  21.0
    ## Hornet 4 Drive 21.4
    ## Valiant        18.1
    ## Merc 280       19.2
    ## Merc 280C      17.8
    ## Ferrari Dino   19.7
    ## 
    ## $`8`
    ##                      mpg
    ## Hornet Sportabout   18.7
    ## Duster 360          14.3
    ## Merc 450SE          16.4
    ## Merc 450SL          17.3
    ## Merc 450SLC         15.2
    ## Cadillac Fleetwood  10.4
    ## Lincoln Continental 10.4
    ## Chrysler Imperial   14.7
    ## Dodge Challenger    15.5
    ## AMC Javelin         15.2
    ## Camaro Z28          13.3
    ## Pontiac Firebird    19.2
    ## Ford Pantera L      15.8
    ## Maserati Bora       15.0

**`eapply`**

``` r
# "eapply" applies FUN to the named values from an environment and returns the results as a list. The user can request that all named objects are used (normally names that begin with a dot are not). The output is not sorted and no parent environments are searched.
env = new.env()
env$x = 1:3
env$y = 4:6
env$x
```

    ## [1] 1 2 3

``` r
env$y
```

    ## [1] 4 5 6

``` r
eapply(env, sum)
```

    ## $x
    ## [1] 6
    ## 
    ## $y
    ## [1] 15
