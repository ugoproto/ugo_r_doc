-   [Documentation](#documentation)
-   [Data Analysis in R, the `data.table` Way](#data-analysis-in-r-the-data.table-way)
    -   [1, `data.table` novice](#data.table-novice)
    -   [2, `data.table` yeoman](#data.table-yeoman)
    -   [3, `data.table` expert](#data.table-expert)
-   [Data Manipulation in R with `dplyr`](#data-manipulation-in-r-with-dplyr)
    -   [1, Introduction to `dplyr`](#introduction-to-dplyr)
    -   [2, `select` and `mutate`](#select-and-mutate)
    -   [3, `filter` and `arrange`](#filter-and-arrange)
    -   [4, `summarise` and the Pipe Operator](#summarise-and-the-pipe-operator)
    -   [5, `group_by` and working with data](#group_by-and-working-with-data)
-   [Adding `tidyr` Functions](#adding-tidyr-functions)
-   [Extension: 'Joining Data in R with `dplyr`'](#extension-joining-data-in-r-with-dplyr)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

Documentation
-------------

`data.table`

-   extension of `data.frame`.
-   Fast aggregation of large data (e.g. 100GB in RAM), fast ordered
    joins, fast add/modify/delete of columns by group using no copies at
    all, list columns, a fast friendly file reader and parallel
    file writer. Offers a natural and flexible syntax, for
    faster development.

`dplyr`

-   A fast, consistent tool for working with data frame like objects,
    both in memory and out of memory.
-   Pipelines.

`tidyr`

-   An evolution of 'reshape2'. It's designed specifically for data
    tidying (not general reshaping or aggregating) and works well with
    dplyr data pipelines.

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

Data Analysis in R, the `data.table` Way
========================================

1, `data.table` novice
----------------------

Find out more with `?data.table`.

**Create and subset a `data.table`**

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

**Getting to know a `data.table`**

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

`DT` is a data.table/data.frame, but `DT[ , B]` is a vector;
`DT[ , .(B)]` is a subsetted data.table.

**Subsetting data tables**

`DT[i, j, by]` means take `DT`, subset rows using `i`, then calculate
`j` grouped by `by`. You can wrap `j` with `.()`.

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

**The `by` basics**

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

**Using `.N` and `by`**

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

**Return multiple numbers in `j`**

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

2, `data.table` yeoman
----------------------

**Chaining, the basics**

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

**Programming time vs readability**

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

**Introducing `.SDcols`**

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

**Mixing it together: `lapply`, `.SD`, `SDcols` and `.N`**

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

**Adding, updating and removing columns**

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

**The functional form**

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

**Ready, `set`, go!**

The `set` function is used to repeatedly update a data.table by
reference. You can think of the `set` function as a loopable.

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

3, `data.table` expert
----------------------

**Selecting rows the `data.table` way**

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

**Removing columns and adapting your column names**

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

**Understanding automatic indexing**

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

**Selecting groups or parts of groups**

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

**Rolling joins - part one**

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

**Rolling joins - part two**

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

------------------------------------------------------------------------

Data Manipulation in R with `dplyr`
===================================

1, Introduction to `dplyr`
--------------------------

**Load the `dplyr` and `hflights` package**

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

**Convert `data.frame` to table**

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

**Changing labels of `hflights`, part 1 of 2**

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

**Changing labels of `hflights`, part 2 of 2**

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

2, `select` and `mutate`
------------------------

**The five verbs and their meaning**

-   `select`; which returns a subset of the columns.
-   `filter`; that is able to return a subset of the rows.
-   `arrange`; that reorders the rows according to single or
    multiple variables.
-   `mutate`; used to add columns from existing data.
-   `summarise`; which reduces each group to a single row by calculating
    aggregate measures.

**Choosing is not losing! The `select` verb**

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

**Helper functions for variable selection**

`select`:

-   `starts_with("X")`; every name that starts with `"X"`,
-   `ends_with("X")`; every name that ends with `"X"`,
-   `contains("X")`; every name that contains `"X"`,
-   `matches("X")`; every name that matches `"X"`, where `"X"` can be a
    regular expression,
-   `num_range("x", 1:5)`; the variables named `x01`, `x02`, `x03`, -
    `x04` and `x05`,
-   `one_of(x)`; every name that appears in `x`, which should be a
    character vector.

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

**Comparison to basic R**

``` r
# add
ex1r <- hflights[c('TaxiIn','TaxiOut','Distance')]

ex1d <- select(hflights, starts_with('Taxi'), Distance)

ex2r <- hflights[c('Year','Month','DayOfWeek','DepTime','ArrTime')]

ex2d <- select(hflights, Year, Month, DayOfWeek, DepTime, ArrTime)

ex3r <- hflights[c('TailNum','TaxiIn','TaxiOut')]

ex3d <- select(hflights, TailNum, starts_with('Taxi'))
```

**Mutating is creating**

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

**Add multiple variables using mutate**

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

3, `filter` and `arrange`
-------------------------

**Logical operators**

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

**Combining tests using boolean operators**

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

**Blend together what you've learned!**

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

**Arranging your data**

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

**Reverse the order of arranging**

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

4, `summarise` and the Pipe Operator
------------------------------------

**The syntax of summarise**

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

**Aggregate functions**

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

**`dplyr` aggregate functions**

-   `first(x)`; the first element of vector `x`.
-   `last(x)`; the last element of vector `x`.
-   `nth(x, n)`; The `n`th element of vector `x`.
-   `n()`; The number of rows in the data.frame or group of observations
    that `summarise()` describes.
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

5, `group_by` and working with data
-----------------------------------

**Unite and conquer using `group_by`**

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

**Combine `group_by` with `mutate`**

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

**Advanced `group_by` exercises**

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

**`dplyr` deals with different types**

``` r
# Use summarise to calculate n_carrier
s2 <- hflights %>%
    summarise(n_carrier = n_distinct(UniqueCarrier))
```

**`dplyr` and mySQL databases**

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

-   `complete`
-   `drop_na`
-   `expand`
-   `extract`
-   `extract_numeric`
-   `complete`
-   `fill`
-   `full_seq`
-   `gather`
-   `nest`
-   `replace_na`
-   `separate`
-   `separate_rows`
-   `separate_rows_`
-   `smiths`
-   `spread`
-   `table1`
-   `unite`
-   `unnest`
-   `who`

------------------------------------------------------------------------

Extension: 'Joining Data in R with `dplyr`'
===========================================

``` text
## 1, Mutating Joins

## 2, Filtering Joins and Set Operations

## 3, Assembling Data

## 4, Advanced Joining

## 5, Case Study
```
