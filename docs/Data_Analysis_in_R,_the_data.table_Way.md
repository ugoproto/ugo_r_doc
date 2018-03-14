<!--
-   [Documentation](#documentation)
-   [1, `data.table` novice](#data.table-novice)
    -   [Create and subset a
        `data.table`](#create-and-subset-a-data.table)
    -   [Getting to know a `data.table`](#getting-to-know-a-data.table)
    -   [Subsetting data tables](#subsetting-data-tables)
    -   [The `by` basics](#the-by-basics)
    -   [Using `.N` and `by`](#using-.n-and-by)
    -   [Return multiple numbers in `j`](#return-multiple-numbers-in-j)
-   [2, `data.table` yeoman](#data.table-yeoman)
    -   [Chaining, the basics](#chaining-the-basics)
    -   [Programming time vs
        readability](#programming-time-vs-readability)
    -   [Introducing `.SDcols`](#introducing-.sdcols)
    -   [Mixing it together: `lapply`, `.SD`, `SDcols` and
        `.N`](#mixing-it-together-lapply-.sd-sdcols-and-.n)
    -   [Adding, updating, and removing
        columns](#adding-updating-and-removing-columns)
    -   [The functional form](#the-functional-form)
    -   [Ready, `set`, go!](#ready-set-go)
-   [3, `data.table` expert](#data.table-expert)
    -   [Selecting rows the `data.table`
        way](#selecting-rows-the-data.table-way)
    -   [Removing columns and adapting your column
        names](#removing-columns-and-adapting-your-column-names)
    -   [Understanding automatic
        indexing](#understanding-automatic-indexing)
    -   [Selecting groups or parts of
        groups](#selecting-groups-or-parts-of-groups)
    -   [Rolling joins](#rolling-joins)
-->
------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

Documentation
=============

`data.table`

- Extension of `data.frame`.
- Fast aggregation of large data (e.g. 100GB in RAM), fast ordered     joins, fast add/modify/delete of columns by group using no copies at     all, list columns, a fast friendly file reader and parallel file writer. Offers a natural and flexible syntax, for faster development.

`dplyr`

- A fast, consistent tool for working with data frame like objects, both in memory and out of memory.
- Pipelines.

`tidyr`

- An evolution of 'reshape2'. It's designed specifically for data tidying (not general reshaping or aggregating) and works well with dplyr data pipelines.

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

`data.table` novice
===================

Find out more with `?data.table`.

## Create and subset a `data.table`

``` r
# The data.table package
library(data.table)

# Create my_first_data_table
my_first_data_table <- data.table(x = c('a', 'b', 'c', 'd', 'e'), y = c(1, 2, 3, 4, 5))

# Create a data.table using recycling
DT <- data.table(a = 1:2, b = c('A', 'B', 'C', 'D'))

# Print the third row to the console
DT[3,]
```

    ##    a b
    ## 1: 1 C

``` r
# Print the second and third row to the console, but do not commas
DT[2:3]
```

    ##    a b
    ## 1: 2 B
    ## 2: 1 C

## Getting to know a `data.table`

Like `head`, `tail`.

``` r
# Print the penultimate row of DT using .N
DT[.N - 1]
```

    ##    a b
    ## 1: 1 C

``` r
# Print the column names of DT, and number of rows and number of columns
colnames(DT)
```

    ## [1] "a" "b"

``` r
dim(DT)
```

    ## [1] 4 2

``` r
# Select row 2 twice and row 3, returning a data.table with three rows where row 2 is a duplicate of row 1.
DT[c(2, 2, 3)]
```

    ##    a b
    ## 1: 2 B
    ## 2: 2 B
    ## 3: 1 C

`DT` is a data.table/data.frame, but `DT[ , B]` is a vector; `DT[ , .(B)]` is a subsetted data.table.

## Subsetting data tables

`DT[i, j, by]` means take `DT`, subset rows using `i`, then calculate `j` grouped by `by`. You can wrap `j` with `.()`.

``` r
A <- c(1, 2, 3, 4, 5)
B <- c('a', 'b', 'c', 'd', 'e')
C <- c(6, 7, 8, 9, 10)
DT <- data.table(A, B, C)

# Subset rows 1 and 3, and columns B and C
DT[c(1,3) ,.(B, C)]
```

    ##    B C
    ## 1: a 6
    ## 2: c 8

``` r
# Assign to ans the correct value
ans <- data.table(DT[, .(B, val = A * C)])

# Fill in the blanks such that ans2 equals target
#target <- data.table(B = c('a', 'b', 'c', 'd', 'e', 'a', 'b', 'c', 'd', 'e'),  val = as.integer(c(6:10, 1:5)))
ans2 <- data.table(DT[, .(B, val = as.integer(c(6:10, 1:5)))])
```

## The `by` basics

``` r
# iris and iris3 are already available in the workspace

# Convert iris to a data.table: DT
DT <- as.data.table(iris)

# For each Species, print the mean Sepal.Length
DT[, .(mean(Sepal.Length)), by = .(Species)]
```

    ##       Species    V1
    ## 1:     setosa 5.006
    ## 2: versicolor 5.936
    ## 3:  virginica 6.588

``` r
# Print mean Sepal.Length, grouping by first letter of Species
DT[, .(mean(Sepal.Length)), by = .(substr(Species, 1,1))]
```

    ##    substr    V1
    ## 1:      s 5.006
    ## 2:      v 6.262

## Using `.N` and `by`

`.N`, number, in row or column.

``` r
# data.table version of iris: DT
DT <- as.data.table(iris)

# Group the specimens by Sepal area (to the nearest 10 cm2) and count how many occur in each group.
DT[, .N, by = 10 * round(Sepal.Length * Sepal.Width / 10)]
```

    ##    round   N
    ## 1:    20 117
    ## 2:    10  29
    ## 3:    30   4

``` r
# Now name the output columns `Area` and `Count`
DT[, .(Count = .N), by = .(Area = 10 * round(Sepal.Length * Sepal.Width / 10))]
```

    ##    Area Count
    ## 1:   20   117
    ## 2:   10    29
    ## 3:   30     4

## Return multiple numbers in `j`

``` r
# Create the data.table DT
set.seed(1L)
DT <- data.table(A = rep(letters[2:1], each = 4L), B = rep(1:4, each = 2L), C = sample(8))

# Create the new data.table, DT2
DT2 <- DT[, .(C = cumsum(C)), by=.(A,B)]

# Select from DT2 the last two values from C while you group by A
DT2[, .(C = tail(C,2)), by=A]
```

    ##    A C
    ## 1: b 4
    ## 2: b 9
    ## 3: a 2
    ## 4: a 8

`data.table` yeoman
===================

## Chaining, the basics

``` r
# Build DT
set.seed(1L)
DT <- data.table(A = rep(letters[2:1], each = 4L), B = rep(1:4, each = 2L), C = sample(8)) 
DT
```

    ##    A B C
    ## 1: b 1 3
    ## 2: b 1 8
    ## 3: b 2 4
    ## 4: b 2 5
    ## 5: a 3 1
    ## 6: a 3 7
    ## 7: a 4 2
    ## 8: a 4 6

``` r
# Use chaining
# Cumsum of C while grouping by A and B, and then select last two values of C while grouping by A
DT[, .(C = cumsum(C)), by = .(A,B)][, .(C = tail(C,2)), by = .(A)]
```

    ##    A C
    ## 1: b 4
    ## 2: b 9
    ## 3: a 2
    ## 4: a 8

**Chaining your `iris` dataset**

``` r
DT <- data.table(iris)

# Perform chained operations on DT
DT[, .(Sepal.Length = median(Sepal.Length), Sepal.Width = median(Sepal.Width), Petal.Length = median(Petal.Length), Petal.Width = median(Petal.Width)), by = .(Species)][order(Species, decreasing = TRUE)]
```

    ##       Species Sepal.Length Sepal.Width Petal.Length Petal.Width
    ## 1:  virginica          6.5         3.0         5.55         2.0
    ## 2: versicolor          5.9         2.8         4.35         1.3
    ## 3:     setosa          5.0         3.4         1.50         0.2

``` r
DT
```

    ##      Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ##   1:          5.1         3.5          1.4         0.2    setosa
    ##   2:          4.9         3.0          1.4         0.2    setosa
    ##   3:          4.7         3.2          1.3         0.2    setosa
    ##   4:          4.6         3.1          1.5         0.2    setosa
    ##   5:          5.0         3.6          1.4         0.2    setosa
    ##  ---                                                            
    ## 146:          6.7         3.0          5.2         2.3 virginica
    ## 147:          6.3         2.5          5.0         1.9 virginica
    ## 148:          6.5         3.0          5.2         2.0 virginica
    ## 149:          6.2         3.4          5.4         2.3 virginica
    ## 150:          5.9         3.0          5.1         1.8 virginica

## Programming time vs readability

``` r
x <- c(2, 1, 2, 1, 2, 2, 1)
y <- c(1, 3, 5, 7, 9, 11, 13)
z <- c(2, 4, 6, 8, 10, 12, 14)
DT <- data.table(x, y, z)

# Mean of columns
DT[, lapply(.SD, mean), by = .(x)] 
```

    ##    x        y        z
    ## 1: 2 6.500000 7.500000
    ## 2: 1 7.666667 8.666667

``` r
# Median of columns
DT[, lapply(.SD, median), by = .(x)]
```

    ##    x y z
    ## 1: 2 7 8
    ## 2: 1 7 8

## Introducing `.SDcols`

`.SDcols` specifies the columns of `DT` that are included in `.SD`.

``` r
grp <- c(6, 6, 8, 8, 8)
Q1 <- c(4, 3, 3, 5, 3)
Q2 <- c(1, 4, 1, 4, 4)
Q3 <- c(3, 1, 5, 5, 2)
H1 <- c(1, 2, 3, 2, 4)
H2 <- c(1, 4, 3, 4, 3)
DT <- data.table(grp, Q1, Q2, Q3, H1, H2)

# Calculate the sum of the Q columns
DT[, lapply(.SD, sum), .SDcols = 2:4]
```

    ##    Q1 Q2 Q3
    ## 1: 18 14 16

``` r
# Calculate the sum of columns H1 and H2 
DT[, lapply(.SD, sum), .SDcols = 5:6]
```

    ##    H1 H2
    ## 1: 12 15

``` r
# Select all but the first row of groups 1 and 2, returning only the grp column and the Q columns. 
DT[, .SD[-1], .SDcols = 2:4, by = .(grp)]
```

    ##    grp Q1 Q2 Q3
    ## 1:   6  3  4  1
    ## 2:   8  5  4  5
    ## 3:   8  3  4  2

## Mixing it together: `lapply`, `.SD`, `SDcols` and `.N`

``` r
x <- c(2, 1, 2, 1, 2, 2, 1)
y <- c(1, 3, 5, 7, 9, 11, 13)
z <- c(2, 4, 6, 8, 10, 12, 14)
DT <- data.table(x, y, z)

# Sum of all columns and the number of rows
# For the first part, you need to combine the returned list from lapply, .SD and .SDcols and the integer vector of .N. You have to this because the result of the two together has to be a list again, with all values put together.
DT
```

    ##    x  y  z
    ## 1: 2  1  2
    ## 2: 1  3  4
    ## 3: 2  5  6
    ## 4: 1  7  8
    ## 5: 2  9 10
    ## 6: 2 11 12
    ## 7: 1 13 14

``` r
DT[,c(lapply(.SD, sum), .N), .SDcols = 1:3, by = x]
```

    ##    x x  y  z N
    ## 1: 2 8 26 30 4
    ## 2: 1 3 23 26 3

``` r
# Cumulative sum of column x and y while grouping by x and z > 8
DT[,lapply(.SD, cumsum), .SDcols = 1:2, by = .(by1 = x, by2 = z > 8)]
```

    ##    by1   by2 x  y
    ## 1:   2 FALSE 2  1
    ## 2:   2 FALSE 4  6
    ## 3:   1 FALSE 1  3
    ## 4:   1 FALSE 2 10
    ## 5:   2  TRUE 2  9
    ## 6:   2  TRUE 4 20
    ## 7:   1  TRUE 1 13

``` r
# Chaining
DT[,lapply(.SD, cumsum), .SDcols = 1:2, by = .(by1 = x, by2 = z > 8)][,lapply(.SD, max), .SDcols = 3:4, by = by1]
```

    ##    by1 x  y
    ## 1:   2 4 20
    ## 2:   1 2 13

## Adding, updating, and removing columns

`:=` is defined for use in `j` only.

``` r
# The data.table DT
DT <- data.table(A = letters[c(1, 1, 1, 2, 2)], B = 1:5)
DT
```

    ##    A B
    ## 1: a 1
    ## 2: a 2
    ## 3: a 3
    ## 4: b 4
    ## 5: b 5

``` r
# Add column by reference: Total
DT[, ('Total') := sum(B), by = .(A)]
```

    ##    A B Total
    ## 1: a 1     6
    ## 2: a 2     6
    ## 3: a 3     6
    ## 4: b 4     9
    ## 5: b 5     9

``` r
# Add 1 to column B
DT[c(2,4), ('B') := as.integer(1 + B)]
```

    ##    A B Total
    ## 1: a 1     6
    ## 2: a 3     6
    ## 3: a 3     6
    ## 4: b 5     9
    ## 5: b 5     9

``` r
# Add a new column Total2
DT[2:4, ':='(Total2 = sum(B)), by = .(A)]
```

    ##    A B Total Total2
    ## 1: a 1     6     NA
    ## 2: a 3     6      6
    ## 3: a 3     6      6
    ## 4: b 5     9      5
    ## 5: b 5     9     NA

``` r
# Remove the Total column
DT[, Total := NULL]
```

    ##    A B Total2
    ## 1: a 1     NA
    ## 2: a 3      6
    ## 3: a 3      6
    ## 4: b 5      5
    ## 5: b 5     NA

``` r
# Select the third column using `[[`
DT[[3]]
```

    ## [1] NA  6  6  5 NA

## The functional form

``` r
# A data.table DT
DT <- data.table(A = c(1, 1, 1, 2, 2), B = 1:5)

# Update B, add C and D
DT[, `:=`(B = B + 1,  C = A + B, D = 2)]
```

    ##    A B C D
    ## 1: 1 2 2 2
    ## 2: 1 3 3 2
    ## 3: 1 4 4 2
    ## 4: 2 5 6 2
    ## 5: 2 6 7 2

``` r
# Delete my_cols
my_cols <- c('B', 'C')
DT[, (my_cols) := NULL]
```

    ##    A D
    ## 1: 1 2
    ## 2: 1 2
    ## 3: 1 2
    ## 4: 2 2
    ## 5: 2 2

``` r
# Delete column 2 by number
DT[, 2 := NULL]
```

    ##    A
    ## 1: 1
    ## 2: 1
    ## 3: 1
    ## 4: 2
    ## 5: 2

## Ready, `set`, go!

The `set` function is used to repeatedly update a data.table by reference. You can think of the `set` function as a loopable.

``` r
A <- c(2, 2, 3, 5, 2, 5, 5, 4, 4, 1)
B <- c(2, 1, 4, 2, 4, 3, 4, 5, 2, 4)
C <- c(5, 2, 4, 1, 2, 2, 1, 2, 5, 2)
D <- c(3, 3, 3, 1, 5, 4, 4, 1, 4, 3)
DT <- data.table(A, B, C, D)

# Set the seed
set.seed(1)

# Check the DT
DT
```

    ##     A B C D
    ##  1: 2 2 5 3
    ##  2: 2 1 2 3
    ##  3: 3 4 4 3
    ##  4: 5 2 1 1
    ##  5: 2 4 2 5
    ##  6: 5 3 2 4
    ##  7: 5 4 1 4
    ##  8: 4 5 2 1
    ##  9: 4 2 5 4
    ## 10: 1 4 2 3

``` r
# For loop with set
for (l in 2:4) set(DT, sample(10,3), l, NA)

# Change the column names to lowercase
setnames(DT,c('A','B','C','D'), c('a','b','c','d'))

# Print the resulting DT to the console
DT
```

    ##     a  b  c  d
    ##  1: 2  2  5  3
    ##  2: 2  1 NA  3
    ##  3: 3 NA  4  3
    ##  4: 5 NA  1  1
    ##  5: 2 NA  2  5
    ##  6: 5  3  2 NA
    ##  7: 5  4  1  4
    ##  8: 4  5 NA  1
    ##  9: 4  2  5 NA
    ## 10: 1  4 NA NA

**The `set` family**

``` r
# Define DT
DT <- data.table(a = letters[c(1, 1, 1, 2, 2)], b = 1)
DT
```

    ##    a b
    ## 1: a 1
    ## 2: a 1
    ## 3: a 1
    ## 4: b 1
    ## 5: b 1

``` r
# Add a postfix '_2' to all column names
setnames(DT, c(1:2), paste0(c('a','b'), '_2'))
DT
```

    ##    a_2 b_2
    ## 1:   a   1
    ## 2:   a   1
    ## 3:   a   1
    ## 4:   b   1
    ## 5:   b   1

``` r
# Change column name 'a_2' to 'A2'
setnames(DT, 'a_2', 'A2')
DT
```

    ##    A2 b_2
    ## 1:  a   1
    ## 2:  a   1
    ## 3:  a   1
    ## 4:  b   1
    ## 5:  b   1

``` r
# Reverse the order of the columns
setcolorder(DT, c('b_2','A2'))
```

`data.table` expert
===================

## Selecting rows the `data.table` way

``` r
# Convert iris to a data.table
iris <- data.table('Sepal.Length' = iris$Sepal.Length, 'Sepal.Width' = iris$Sepal.Width, 'Petal.Length' = iris$Petal.Length, 'Petal.Width' = iris$Petal.Width, 'Species' = iris$Species)
iris
```

    ##      Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ##   1:          5.1         3.5          1.4         0.2    setosa
    ##   2:          4.9         3.0          1.4         0.2    setosa
    ##   3:          4.7         3.2          1.3         0.2    setosa
    ##   4:          4.6         3.1          1.5         0.2    setosa
    ##   5:          5.0         3.6          1.4         0.2    setosa
    ##  ---                                                            
    ## 146:          6.7         3.0          5.2         2.3 virginica
    ## 147:          6.3         2.5          5.0         1.9 virginica
    ## 148:          6.5         3.0          5.2         2.0 virginica
    ## 149:          6.2         3.4          5.4         2.3 virginica
    ## 150:          5.9         3.0          5.1         1.8 virginica

``` r
# Species is 'virginica'
head(iris[Species == 'virginica'], 20)
```

    ##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ##  1:          6.3         3.3          6.0         2.5 virginica
    ##  2:          5.8         2.7          5.1         1.9 virginica
    ##  3:          7.1         3.0          5.9         2.1 virginica
    ##  4:          6.3         2.9          5.6         1.8 virginica
    ##  5:          6.5         3.0          5.8         2.2 virginica
    ##  6:          7.6         3.0          6.6         2.1 virginica
    ##  7:          4.9         2.5          4.5         1.7 virginica
    ##  8:          7.3         2.9          6.3         1.8 virginica
    ##  9:          6.7         2.5          5.8         1.8 virginica
    ## 10:          7.2         3.6          6.1         2.5 virginica
    ## 11:          6.5         3.2          5.1         2.0 virginica
    ## 12:          6.4         2.7          5.3         1.9 virginica
    ## 13:          6.8         3.0          5.5         2.1 virginica
    ## 14:          5.7         2.5          5.0         2.0 virginica
    ## 15:          5.8         2.8          5.1         2.4 virginica
    ## 16:          6.4         3.2          5.3         2.3 virginica
    ## 17:          6.5         3.0          5.5         1.8 virginica
    ## 18:          7.7         3.8          6.7         2.2 virginica
    ## 19:          7.7         2.6          6.9         2.3 virginica
    ## 20:          6.0         2.2          5.0         1.5 virginica

``` r
# Species is either 'virginica' or 'versicolor'
head(iris[Species %in% c('virginica', 'versicolor')], 20)
```

    ##     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
    ##  1:          7.0         3.2          4.7         1.4 versicolor
    ##  2:          6.4         3.2          4.5         1.5 versicolor
    ##  3:          6.9         3.1          4.9         1.5 versicolor
    ##  4:          5.5         2.3          4.0         1.3 versicolor
    ##  5:          6.5         2.8          4.6         1.5 versicolor
    ##  6:          5.7         2.8          4.5         1.3 versicolor
    ##  7:          6.3         3.3          4.7         1.6 versicolor
    ##  8:          4.9         2.4          3.3         1.0 versicolor
    ##  9:          6.6         2.9          4.6         1.3 versicolor
    ## 10:          5.2         2.7          3.9         1.4 versicolor
    ## 11:          5.0         2.0          3.5         1.0 versicolor
    ## 12:          5.9         3.0          4.2         1.5 versicolor
    ## 13:          6.0         2.2          4.0         1.0 versicolor
    ## 14:          6.1         2.9          4.7         1.4 versicolor
    ## 15:          5.6         2.9          3.6         1.3 versicolor
    ## 16:          6.7         3.1          4.4         1.4 versicolor
    ## 17:          5.6         3.0          4.5         1.5 versicolor
    ## 18:          5.8         2.7          4.1         1.0 versicolor
    ## 19:          6.2         2.2          4.5         1.5 versicolor
    ## 20:          5.6         2.5          3.9         1.1 versicolor

## Removing columns and adapting your column names

Refer to a regex cheat sheet for metacharacter.

``` r
# iris as a data.table
iris <- as.data.table(iris)
iris
```

    ##      Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
    ##   1:          5.1         3.5          1.4         0.2    setosa
    ##   2:          4.9         3.0          1.4         0.2    setosa
    ##   3:          4.7         3.2          1.3         0.2    setosa
    ##   4:          4.6         3.1          1.5         0.2    setosa
    ##   5:          5.0         3.6          1.4         0.2    setosa
    ##  ---                                                            
    ## 146:          6.7         3.0          5.2         2.3 virginica
    ## 147:          6.3         2.5          5.0         1.9 virginica
    ## 148:          6.5         3.0          5.2         2.0 virginica
    ## 149:          6.2         3.4          5.4         2.3 virginica
    ## 150:          5.9         3.0          5.1         1.8 virginica

``` r
# Remove the 'Sepal.' prefix
#gsub('([ab])', '\\1_\\1_', 'abc and ABC') = pattern, replacement, x
setnames(iris, c('Sepal.Length', 'Sepal.Width'), c('Length','Width')) 
#gsub('^Sepal\\.','', iris)

# Remove the two columns starting with 'Petal'
iris[, c('Petal.Length', 'Petal.Width') := NULL]
```

    ##      Length Width   Species
    ##   1:    5.1   3.5    setosa
    ##   2:    4.9   3.0    setosa
    ##   3:    4.7   3.2    setosa
    ##   4:    4.6   3.1    setosa
    ##   5:    5.0   3.6    setosa
    ##  ---                       
    ## 146:    6.7   3.0 virginica
    ## 147:    6.3   2.5 virginica
    ## 148:    6.5   3.0 virginica
    ## 149:    6.2   3.4 virginica
    ## 150:    5.9   3.0 virginica

## Understanding automatic indexing

``` r
# Cleaned up iris data.table
iris2 <- data.frame(Length = iris$Sepal.Length, Width = iris$Sepal.Width, Species = iris$Species)
iris2 <- as.data.table(iris2)
```

``` r
# Area is greater than 20 square centimeters
iris2[ Width * Length > 20 ], 20
```

``` text
   Length Width    Species is_large
 1:    5.4   3.9     setosa    FALSE
 2:    5.8   4.0     setosa    FALSE
 3:    5.7   4.4     setosa     TRUE
 4:    5.4   3.9     setosa    FALSE
 5:    5.7   3.8     setosa    FALSE
 6:    5.2   4.1     setosa    FALSE
 7:    5.5   4.2     setosa    FALSE
 8:    7.0   3.2 versicolor    FALSE
 9:    6.4   3.2 versicolor    FALSE
10:    6.9   3.1 versicolor    FALSE
11:    6.3   3.3 versicolor    FALSE
12:    6.7   3.1 versicolor    FALSE
13:    6.7   3.0 versicolor    FALSE
14:    6.0   3.4 versicolor    FALSE
15:    6.7   3.1 versicolor    FALSE
16:    6.3   3.3  virginica    FALSE
17:    7.1   3.0  virginica    FALSE
18:    7.6   3.0  virginica    FALSE
19:    7.3   2.9  virginica    FALSE
20:    7.2   3.6  virginica     TRUE
...
```

``` r
# Add new boolean column
iris2[, is_large := Width * Length > 25]
```

``` r
# Now large observations with is_large
iris2[is_large == TRUE]
```

``` text
   Length Width   Species is_large
1:    5.7   4.4    setosa     TRUE
2:    7.2   3.6 virginica     TRUE
3:    7.7   3.8 virginica     TRUE
4:    7.9   3.8 virginica     TRUE
```

``` r
iris2[(is_large)] # Also OK
```

``` text
   Length Width   Species is_large
1:    5.7   4.4    setosa     TRUE
2:    7.2   3.6 virginica     TRUE
3:    7.7   3.8 virginica     TRUE
4:    7.9   3.8 virginica     TRUE
```

## Selecting groups or parts of groups

``` r
# The 'keyed' data.table DT
DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)],
                 B = c(5, 4, 1, 9,8 ,8, 6), 
                 C = 6:12)
setkey(DT, A, B)

# Select the 'b' group
DT['b']
```

    ##    A B  C
    ## 1: b 1  8
    ## 2: b 5  6
    ## 3: b 8 11

``` r
# 'b' and 'c' groups
DT[c('b', 'c')]
```

    ##    A B  C
    ## 1: b 1  8
    ## 2: b 5  6
    ## 3: b 8 11
    ## 4: c 6 12
    ## 5: c 9  9

``` r
# The first row of the 'b' and 'c' groups
DT[c('b', 'c'), mult = 'first']
```

    ##    A B  C
    ## 1: b 1  8
    ## 2: c 6 12

``` r
# First and last row of the 'b' and 'c' groups
DT[c('b', 'c'), .SD[c(1, .N)], by = .EACHI]
```

    ##    A B  C
    ## 1: b 1  8
    ## 2: b 8 11
    ## 3: c 6 12
    ## 4: c 9  9

``` r
# Copy and extend code for instruction 4: add printout
DT[c('b', 'c'), { print(.SD); .SD[c(1, .N)] }, by = .EACHI]
```

    ##    B  C
    ## 1: 1  8
    ## 2: 5  6
    ## 3: 8 11
    ##    B  C
    ## 1: 6 12
    ## 2: 9  9

    ##    A B  C
    ## 1: b 1  8
    ## 2: b 8 11
    ## 3: c 6 12
    ## 4: c 9  9

## Rolling joins

**Rolling joins -- part one**

``` r
# Keyed data.table DT
DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)],                  B = c(5, 4, 1, 9, 8, 8, 6), 
                 C = 6:12, 
                 key = 'A,B')

# Get the key of DT
key(DT)
```

    ## [1] "A" "B"

``` r
# Row where A == 'b' & B == 6
setkey(DT, A, B)
DT[.('b', 6)]
```

    ##    A B  C
    ## 1: b 6 NA

``` r
# Return the prevailing row
DT[.('b',6), roll = TRUE]
```

    ##    A B C
    ## 1: b 6 6

``` r
# Return the nearest row
DT[.('b',6), roll =+ Inf]
```

    ##    A B C
    ## 1: b 6 6

**Rolling joins -- part two**

``` r
# Keyed data.table DT
DT <- data.table(A = letters[c(2, 1, 2, 3, 1, 2, 3)],                  B = c(5, 4, 1, 9, 8, 8, 6), 
                 C = 6:12, 
                 key = 'A,B')

# Look at the sequence (-2):10 for the 'b' group
DT[.('b', (-2):10)]
```

    ##     A  B  C
    ##  1: b -2 NA
    ##  2: b -1 NA
    ##  3: b  0 NA
    ##  4: b  1  8
    ##  5: b  2 NA
    ##  6: b  3 NA
    ##  7: b  4 NA
    ##  8: b  5  6
    ##  9: b  6 NA
    ## 10: b  7 NA
    ## 11: b  8 11
    ## 12: b  9 NA
    ## 13: b 10 NA

``` r
# Add code: carry the prevailing values forwards
DT[.('b', (-2):10), roll = TRUE]
```

    ##     A  B  C
    ##  1: b -2 NA
    ##  2: b -1 NA
    ##  3: b  0 NA
    ##  4: b  1  8
    ##  5: b  2  8
    ##  6: b  3  8
    ##  7: b  4  8
    ##  8: b  5  6
    ##  9: b  6  6
    ## 10: b  7  6
    ## 11: b  8 11
    ## 12: b  9 11
    ## 13: b 10 11

``` r
# Add code: carry the first observation backwards
DT[.('b', (-2):10), roll = TRUE, rollends = TRUE]
```

    ##     A  B  C
    ##  1: b -2  8
    ##  2: b -1  8
    ##  3: b  0  8
    ##  4: b  1  8
    ##  5: b  2  8
    ##  6: b  3  8
    ##  7: b  4  8
    ##  8: b  5  6
    ##  9: b  6  6
    ## 10: b  7  6
    ## 11: b  8 11
    ## 12: b  9 11
    ## 13: b 10 11
