# An Introduction to R

Collected notes, leads, and ideas on what R can do; more at:

- [www.statmethods.net (Quick-R, searchable R guide)](http://www.statmethods.net/)
- [R-intro](https://cran.r-project.org/doc/manuals/R-intro.html)
- [cran.r-project.org/manuals (series of official manuals)](https://cran.r-project.org/manuals.html)

**Table of Content**

[TOC]

## 1, Introduction and preliminaries

- Run R (CLI).
	- Quit with `q()`.
- Run R in an IDE (GUI); like RStudio.
- Create a working directory `work`.
- Ask help.
	- `help(function)`; open a web documentation in a browser or in the IDE.
	- `?function`; idem.
	- `??function`; idem, but showing concordances.
	- `help("[[")`; idem, searching with a string.
	- `help.start()`; show the entire manual.
- `sink()`; divert output from the console to a connection; restore the output to the console.
- `objects()`, `ls()`; see the objects stored in a session.
	- Objects are in a file with `.RData` extension.
- `rm(x, y, ink, ...)`; remove stored objects.
- All commands entered or run are recorded in a file with `.Rhistory` extension.

## 2, Simple manipulations; numbers and vectors

**Create a vector**

```r
x <- c(1, 2) # assignment (universal).
x
[1] 1 2

c(2, 1) -> y # alternative assignment.
y
[1] 2 1

x = c(1, 2) # alternative assignment (some limitations).
x <<- c(1, 2) # permanent assignment.

# examples
ab <- 9
ab
[1] 9

assign("ab", 10)
ab 
[1] 10
```

**Create a sequence (vector)**

```r
x <- 2 * 1:15

x
[1]  2  4  6  8 10 12 14 16 18 20 22 24 26 28 30

n <- 10
x <- 1:n-1

x
[1] 1 2 3 4 5 6 7 8 9

x <- 30:1
x
[1] 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3
[29]  2  1

seq(from = 1, to = 30)
[1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28
[29] 29 30

seq(1, 30)
[1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28
[29] 29 30

seq(30, 1)
[1] 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3
[29]  2  1

seq(1, 30, by = 2)
[1]  1  3  5  7  9 11 13 15 17 19 21 23 25 27 29
```

**Repetition**

```r
rep(2, times = 5)
[1] 2 2 2 2 2
```

```r
x <- c("x", "y")[rep(c(1, 2, 2, 1), times = 4)]

x
[1] "x" "y" "y" "x" "x" "y" "y" "x" "x" "y" "y" "x" "x" "y" "y" "x"
```

**Length (vector)**

- `length(vector)`

**Boolean**

- `TRUE` or `T`; `T == 1`.
- `FALSE` or `F`; `F == 0`.

**Some operators**

- `<`
- `>=`
- `<`
- `>=`
- `==`
- `!=`
- `<>`
- `&`
- `&&`
- `|`
-  `||`
- and many more.

**Missing data and more**

- NA; not available.
- NaN; not a number.
- Inf - Inf == NaN == 0/0; infinite number.

```r
x <- c(1:3, NA)

x
[1] 1 2 3 NA
```

- `is.na(var)`.
- `var == NA`.
- `is.na(x)`.
- `!is.na(x)`.

```r
x <- c(-1:3, NA)
y <- x[!is.na(x) & x > 0]

x
[1]  -1  0  1  2  3 NA

y
[1] 1 2 3
```

**Extract, subset (vector)**

- `x[i]`; index.

**Backslash use for some characters**

- `\\`; backslash.
- `\n`; new line.
- `\t`; tab.
- `\b`; backspace.

**Concatenate, paste (vector)**

```r
labs <- paste(c("X", "Y"), 1:10, sep = "")

labs
[1] "X1"  "Y2"  "X3"  "Y4"  "X5"  "Y6"  "X7"  "Y8"  "X9"  "Y10"
```

**A note of data handling and manipulations**

You can also `split()`, `merge()`, `rbind()`, `cbind() ` vectors. 

It is also possible with other objects such as factors, lists, arrays, matrices, and data frames.

There are built-in functions to extract, exclude, subset, replace, transform or convert (`.as`), concatenate, paste, group, and bind.

<\sub>slice, extract, exclude, subset, replace, convert,  concatenate, paste, group, bind,   

**Exclude, remove (vector)**

```r
z <- 1:20

z
[1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

zz <- z[-(1:5)]

zz
[1]  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
```

**Replace (vector)**

```r
x <- c(1, 1, 1, NA)

x
[1]  1  1  1 NA

x[is.na(x)] <- 0

x
[1] 1 1 1 0
# replaces any missing values in x by zeros
```

**Absolute value**

- `y <- abs(y)`.

**Convert (`.as`)**

- `as.vector()`.
- `as.integer()`.
- `as.numeric()`
- `as.factor()`.
- `as.character()`.
-  and many more.

## 3, Objects, their modes and attributes

**Object type**

```r
obj <- 1

obj1 <- numeric(obj)
mode(obj1)
[1] "numeric"
obj2 <- character(obj)
mode(obj2)
[1] "character"

x <- 1

# class() != mode(), but almost
mode(x)
[1] "numeric"

x <- factor(x)
x
[1] 1
Levels: 1

x <- numeric(x)
x
[1] 0

# load key:value
obj[3] <- 17

obj
[1]  1 NA 17
```

**Classes**

<sub>class<\sub>

- "numeric".
- "logical".
- "character".
- "list".
- "matrix".
- "array".
- "factor".
- "data.frame".

```r
class(obj)
[1] "numeric"

# print the object as ordinary
unclass(obj)
[1]  1 NA 17
```

## 4, Ordered and unordered factors

```r
state <- c("tas","qld","sa","sa","sa","vic","nt","act","qld","nsw","wa","nsw","nsw","vic","vic","vic","nsw","qld","qld","vic","nt","wa","wa","qld","sa", "tas","nsw", "nsw", "wa","act")

statef <- factor(state)

# class != mode()
class(statef)
[1] "factor"
mode(statef)
[1] "numeric"

levels(statef)
[1] "act" "nsw" "nt"  "qld" "sa"  "tas" "vic" "wa" 

incomes <- c(60, 49, 40, 61, 64, 60, 59, 54, 62, 69, 70, 42, 56, 61, 61, 61, 58, 51, 48, 65, 49, 49, 41, 48, 52, 46, 59, 46, 58, 43)

incmeans <- tapply(incomes, statef, mean)

incmeans
  act   nsw    nt   qld    sa   tas   vic    wa 
48.50 55.00 54.00 51.60 54.25 53.00 61.60 54.50 

stderr <- function(x) { sqrt(var(x) / length(x)) }

stderr(incomes)
[1] 1.524462

# alternatively
incster <- tapply(incomes, statef, stderr)

incster
      act       nsw        nt       qld        sa       tas       vic        wa 
5.5000000 3.9665266 5.0000000 2.6570661 5.3909647 7.0000000 0.8717798 6.2249498  
```

```r
# a vector of characters
stateff <- c("a", "b", "c")
[1] "a" "b" "c"

as.factor(stateff)
[1] a b c
Levels: a b c
```

```r
# Create a factor with the wrong order of levels
sizes <- factor(c("small", "large", "large", "small", "medium"))

sizes
[1] small  large  large  small  medium
Levels: large medium small

# levels can be specified explicitly
sizes <- factor(sizes, levels = c("small", "medium", "large"))

sizes
[1] small  large  large  small  medium
Levels: small medium large

# do the same with an ordered factor
sizes <- ordered(c("small", "large", "large", "small", "medium"))

sizes <- ordered(sizes, levels = c("small", "medium", "large"))

sizes
[1] small  large  large  small  medium
Levels: small < medium < large

# use relevel() to make a particular level first in the list. (This will not work for ordered factors.)

# Create a factor with the wrong order
sizes
[1] small  large  large  small  medium
Levels: large medium small

# make medium first
sizes <- relevel(sizes, "medium")

sizes
[1] small  large  large  small  medium
Levels: medium large small

# make small first
sizes <- relevel(sizes, "small")

sizes
[1] small  large  large  small  medium
Levels: small medium large

# specify the proper order when the factor is created
sizes <- factor(c("small", "large", "large", "small", "medium"),
                  levels = c("small", "medium", "large"))

sizes
[1] small  large  large  small  medium
Levels: small medium large

# Create a factor with the wrong order of levels
sizes <- factor(c("small", "large", "large", "small", "medium"))

sizes
[1] small  large  large  small  medium
Levels: large medium small

# reverse the order of levels in a factor
sizes <- factor(sizes, levels=rev(levels(sizes)))

sizes
[1] small  large  large  small  medium
Levels: small medium large
```

**Convert (`.as`)**

- `as.factor()`.

## 5, Arrays and matrices

**Dimension**

```r
z <- 1:1500
dim(z) <- c(3, 5, 100)

# gives 100 arrays of 3 lines by 5 columns
```
**Create a matrix, an array**

```r
x <- array(1:20, dim=c(4, 5))

x
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20

x <- array(1:20, dim=c(2, 5, 2))

x
, , 1

     [,1] [,2] [,3] [,4] [,5]
[1,]    1    3    5    7    9
[2,]    2    4    6    8   10

, , 2

     [,1] [,2] [,3] [,4] [,5]
[1,]   11   13   15   17   19
[2,]   12   14   16   18   20


i <- array(c(1:3, 3:1), dim = c(3, 2))

i
     [,1] [,2]
[1,]    1    3
[2,]    2    2
[3,]    3    1

j <- array(c(1:8), dim = c(2, 2, 2))

j
, , 1

     [,1] [,2]
[1,]    1    3
[2,]    2    4

, , 2

     [,1] [,2]
[1,]    5    7
[2,]    6    8


k <- array(1:27, c(3, 3, 3))

> k
, , 1

     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9

, , 2

     [,1] [,2] [,3]
[1,]   10   13   16
[2,]   11   14   17
[3,]   12   15   18

, , 3

     [,1] [,2] [,3]
[1,]   19   22   25
[2,]   20   23   26
[3,]   21   24   27


a <- matrix(1, 2, 2)

a
     [,1] [,2]
[1,]    1    1
[2,]    1    1

b <- matrix(2, 2, 2)

b
     [,1] [,2]
[1,]    2    2
[2,]    2    2
```

**Extract, subset (matrix)**

- `a[2, 1]`; rows, columns.

**Extract, subset (array)**

- `a[2, 1, 1]`; rows, columns, matrix.

**Cross product vs multiplication**

```r
a <- 1:5
b <- seq(10, 6, -1)

a
[1] 1 2 3 4 5
b
[1] 10  9  8  7  6

a * b
[1] 10 18 24 28 30

crossprod(a, b)
     [,1]
[1,]  110

ab <- a %o% b
ab
     [,1] [,2] [,3] [,4] [,5]
[1,]   10    9    8    7    6
[2,]   20   18   16   14   12
[3,]   30   27   24   21   18
[4,]   40   36   32   28   24
[5,]   50   45   40   35   30
```

**Matrix operation**

A et B are 2x2 matrices:

- `A * B`; scalar multiplication.
- `A %*% B`; matrix multiplication
- `x %*% A %*% y`; matrix multiplication.
- `crossproduct(A, B)`; cross multiplication.
- `ab <- outer(A,B,"*")`; `a * b`.
- `ab <- outer(A,B,"+")`; `a + b`.
- `ab <- outer(A,B,"-")`; `a - b`.
- and many more.

**Diagonal and triangle**

```r
ab
     [,1] [,2] [,3] [,4] [,5]
[1,]   11   10    9    8    7
[2,]   12   11   10    9    8
[3,]   13   12   11   10    9
[4,]   14   13   12   11   10
[5,]   15   14   13   12   11

diag(ab)
[1] 11 11 11 11 11

lower.tri(ab)
      [,1]  [,2]  [,3]  [,4]  [,5]
[1,] FALSE FALSE FALSE FALSE FALSE
[2,]  TRUE FALSE FALSE FALSE FALSE
[3,]  TRUE  TRUE FALSE FALSE FALSE
[4,]  TRUE  TRUE  TRUE FALSE FALSE
[5,]  TRUE  TRUE  TRUE  TRUE FALSE

lower.tri(ab) * ab
     [,1] [,2] [,3] [,4] [,5]
[1,]    0    0    0    0    0
[2,]   12    0    0    0    0
[3,]   13   12    0    0    0
[4,]   14   13   12    0    0
[5,]   15   14   13   12    0

upper.tri(ab)
      [,1]  [,2]  [,3]  [,4]  [,5]
[1,] FALSE  TRUE  TRUE  TRUE  TRUE
[2,] FALSE FALSE  TRUE  TRUE  TRUE
[3,] FALSE FALSE FALSE  TRUE  TRUE
[4,] FALSE FALSE FALSE FALSE  TRUE
[5,] FALSE FALSE FALSE FALSE FALSE

upper.tri(ab) * ab
     [,1] [,2] [,3] [,4] [,5]
[1,]    0   10    9    8    7
[2,]    0    0   10    9    8
[3,]    0    0    0   10    9
[4,]    0    0    0    0   10
[5,]    0    0    0    0    0
```

**Solving a matrix system**

```r
b <- A %*% x

# or
solve(A, b)
```

- `solve(A)`; inverse the matrix.

**Symmetrical matrix and eigen value**

```r
b <- matrix(2, 2, 2)

b
     [,1] [,2]
[1,]    2    2
[2,]    2    2

e <- eigen(b, only.values = TRUE)
e
$values
[1] 4 0

$vectors
NULL
```

**Singular value decomposition (matrix)**

```r
svd(b)
$d
[1] 4 0

$u
           [,1]       [,2]
[1,] -0.7071068 -0.7071068
[2,] -0.7071068  0.7071068

$v
           [,1]       [,2]
[1,] -0.7071068 -0.7071068
[2,] -0.7071068  0.7071068
```

**Determinant (matrix)**

```r
det(ab)
[1] 0
```

**Least square fitting (matrix)**

- `lsfit()` or least squares estimate of `b` in the model: `y = X b + e`.


**QR decomposition (matrix)**
```r
qr(ab)
$qr
            [,1]        [,2]          [,3]          [,4]          [,5]
[1,] -29.2403830 -27.0174299 -2.479448e+01 -2.257152e+01 -2.034857e+01
[2,]   0.4103913   0.2418254  4.836508e-01  7.254763e-01  9.673017e-01
[3,]   0.4445906  -0.1703815  3.717512e-15  1.134003e-14  1.749210e-14
[4,]   0.4787899  -0.5015812  3.957070e-01  1.553722e-15  1.295516e-15
[5,]   0.5129892  -0.8327809  5.076995e-01  6.936403e-01 -1.685698e-16

$rank
[1] 2

$qraux
[1] 1.376192e+00 1.160818e+00 1.765282e+00 1.720322e+00 1.685698e-16

$pivot
[1] 1 2 3 4 5

attr(,"class")
[1] "qr"
```

Also:

- `qr.coef()`.
- `qr.fitted()`.
- `qr.resid()`.

**Convert (`.as`)**

- `as.array()`.
- `as.matrix()`.

## 6, Lists and data frames

**Crate a list**

```r
Lst <- list(name = "Fred", wife = "Mary", no.children = 3, child.ages = c(4,7,9))

Lst
$name
[1] "Fred"

$wife
[1] "Mary"

$no.children
[1] 3

$child.ages
[1] 4 7 9
```

**Extract, subset (list)**

```r
Lst$name
[1] "Fred"

Lst[["name"]] == Lst[[1]]
[1] TRUE


Lst[5] <- list("") # or list()  
Lst["new"] <- list()  # key, value

Lst[1]
$name
[1] "Fred"

Lst$child.ages[1]
[1] 4
Lst[[4]][1]
[1] 4
```

**Concatenate, paste**

```r
x <- c(1,2)
y <- c(3,4)

c(x, y)
[1] 1 2 3 4

paste(x, y)
[1] "1 3" "2 4"

data.frame(x, y)
  x y
1 1 3
2 2 4

a <- list(1, 2)
b <- list("a", "b")

list(a, b)
[[1]]
[[1]][[1]]
[1] 1

[[1]][[2]]
[1] 2


[[2]]
[[2]][[1]]
[1] "a"

[[2]][[2]]
[1] "b"

h <- matrix(2, 2, 2)
g <- matrix(1, 2, 2)

c(h, g)
[1] 2 2 2 2 1 1 1 1

paste(h, g)
[1] "2 1" "2 1" "2 1" "2 1"

list(h, g)
[[1]]
     [,1] [,2]
[1,]    2    2
[2,]    2    2

[[2]]
     [,1] [,2]
[1,]    1    1
[2,]    1    1
```

**Convert (`as.`)**

- `as.matrix()`

**Data frame**

- A data frame can hold other data frames.
	- A list can hold other lists.
	- A vector can hold other vectors.
- Each variable can be numeric, character, logical, factor, numeric matrix, list, data.frame.

```r
state <- c("tas","qld","sa","sa","sa","vic","nt","act","qld","nsw","wa","nsw","nsw","vic","vic","vic","nsw","qld","qld","vic","nt","wa","wa","qld","sa", "tas","nsw", "nsw", "wa","act")

statef <- factor(state)

incomes <- c(60, 49, 40, 61, 64, 60, 59, 54, 62, 69, 70, 42, 56, 61, 61, 61, 58, 51, 48, 65, 49, 49, 41, 48, 52, 46, 59, 46, 58, 43)

incomef <- factor(incomes)

accountants <- data.frame(home=statef, loot=incomes, shot=incomef)

head(accountants, 10)
   home loot shot
1   tas   60   60
2   qld   49   49
3    sa   40   40
4    sa   61   61
5    sa   64   64
6   vic   60   60
7    nt   59   59
8   act   54   54
9   qld   62   62
10  nsw   69   69

# class() != mode()
class(accountants)
[1] "data.frame"

mode(accountants)
[1] "list"
```

**Concatenate, paste (data frame)**

```r
# accountants == acc

c(accountants, acc)
$home
 [1] tas qld sa  sa  sa  vic nt  act qld nsw wa  nsw nsw
[14] vic vic vic nsw qld qld vic nt  wa  wa  qld sa  tas
[27] nsw nsw wa  act
Levels: act nsw nt qld sa tas vic wa

$loot
 [1] 60 49 40 61 64 60 59 54 62 69 70 42 56 61 61 61 58 51
[19] 48 65 49 49 41 48 52 46 59 46 58 43

$shot
 [1] 60 49 40 61 64 60 59 54 62 69 70 42 56 61 61 61 58 51
[19] 48 65 49 49 41 48 52 46 59 46 58 43
20 Levels: 40 41 42 43 46 48 49 51 52 54 56 58 59 ... 70

$home
 [1] tas qld sa  sa  sa  vic nt  act qld nsw wa  nsw nsw
[14] vic vic vic nsw qld qld vic nt  wa  wa  qld sa  tas
[27] nsw nsw wa  act
Levels: act nsw nt qld sa tas vic wa

$loot
 [1] 60 49 40 61 64 60 59 54 62 69 70 42 56 61 61 61 58 51
[19] 48 65 49 49 41 48 52 46 59 46 58 43

$shot
 [1] 60 49 40 61 64 60 59 54 62 69 70 42 56 61 61 61 58 51
[19] 48 65 49 49 41 48 52 46 59 46 58 43
20 Levels: 40 41 42 43 46 48 49 51 52 54 56 58 59 ... 70


paste(accountants, acc)
[1] "c(6, 4, 5, 5, 5, 7, 3, 1, 4, 2, 8, 2, 2, 7, 7, 7, 2, 4, 4, 7, 3, 8, 8, 4, 5, 6, 2, 2, 8, 1) c(6, 4, 5, 5, 5, 7, 3, 1, 4, 2, 8, 2, 2, 7, 7, 7, 2, 4, 4, 7, 3, 8, 8, 4, 5, 6, 2, 2, 8, 1)"                                                            
[2] "c(60, 49, 40, 61, 64, 60, 59, 54, 62, 69, 70, 42, 56, 61, 61, 61, 58, 51, 48, 65, 49, 49, 41, 48, 52, 46, 59, 46, 58, 43) c(60, 49, 40, 61, 64, 60, 59, 54, 62, 69, 70, 42, 56, 61, 61, 61, 58, 51, 48, 65, 49, 49, 41, 48, 52, 46, 59, 46, 58, 43)"
[3] "c(14, 7, 1, 15, 17, 14, 13, 10, 16, 19, 20, 3, 11, 15, 15, 15, 12, 8, 6, 18, 7, 7, 2, 6, 9, 5, 13, 5, 12, 4) c(14, 7, 1, 15, 17, 14, 13, 10, 16, 19, 20, 3, 11, 15, 15, 15, 12, 8, 6, 18, 7, 7, 2, 6, 9, 5, 13, 5, 12, 4)"                          

list(accountants, acc)
[[1]]
   home loot shot
1   tas   60   60
2   qld   49   49
3    sa   40   40
4    sa   61   61
5    sa   64   64
6   vic   60   60
7    nt   59   59
8   act   54   54
9   qld   62   62
10  nsw   69   69
...

[[2]]
   home loot shot
1   tas   60   60
2   qld   49   49
3    sa   40   40
4    sa   61   61
5    sa   64   64
6   vic   60   60
7    nt   59   59
8   act   54   54
9   qld   62   62
10  nsw   69   69
...

accountants + acc
   home loot shot
1    NA  120   NA
2    NA   98   NA
3    NA   80   NA
4    NA  122   NA
5    NA  128   NA
6    NA  120   NA
7    NA  118   NA
8    NA  108   NA
9    NA  124   NA
10   NA  138   NA
...
```

**Convert (`as.`)**

- `as.data.frame()`

**Load data into R**

- `read.table()`; produce a data frame with inputs.

**A note on environment objects**

- `search()`; objects in .GlobalEnv; including packages.
	- `ls()`; list these objects.
- `attach()`; attach an object to .GlobalEnv.
- `detach()`.

## 7, Reading/writing data from/to files (input/output)

A lot more can be found on I/O: depending on the file type, the data format, the desired R object, many commands are available. Depending on a file format, look for the dedicated package on CRAN.

<sub>read, reading, write, writing, input, output, i/o<\sub>

**Examples**

```r
# read from a file
HousePrice <- read.table("house.data", header = TRUE, sep = ";")

# read a 3-variable list
input <- scan("input.dat", list("", 0, 0))  

# read a3-variable list
input <- scan("input.dat", list(id="", x = 0, y = 0))

# make 3 separate vectors
label <- input[[1]]
x <- input[[2]]
y <- input[[3]]

# label them
label <- inp$id
x <- inp$x
y <- inp$y

# read a 5-variable list and transform it into a matrix
X <- matrix(scan("light.dat", 0), ncol = 5, byrow = TRUE)
```

**Inputing from file types...**

- .txt.
- .csv.
- .tsv.
- .hdf5.
- .bmp.
- .jpeg.
- .png.
- .tiff.
- .zip.
- .xls, spreadsheet.
- databases.
- statistical programs.
- binary files.
- and many more (some are up and coming such as julia files).

**Cleaning parameter**

- `strip.white = TRUE`; remove unnecessary spaces.

**Change the data frame or matrix format**

- `stack()`; for example, take a 6-column, 4-variable data frame and stack the 4 variables into one long column.
- `unstack()`; vice-versa.

```r
# 6 columns or variables
Status Age  V1    V2    V3    V4
 P 23646 45190 50333 55166 56271
CC 26174 35535 38227 37911 41184
CC 27723 25691 25712 26144 26398
CC 27193 30949 29693 29754 30772
CC 24370 50542 51966 54341 54273
CC 28359 58591 58803 59435 61292
CC 25136 45801 45389 47197 47126
...

# 4 columns, more rows
Status Age values ind
X1  P 23646 45190 V1
X2 CC 26174 35535 V1
X3 CC 27723 25691 V1
X4 CC 27193 30949 V1
X5 CC 24370 50542 V1
X6 CC 28359 58591 V1
X7 CC 25136 45801 V1
X11 P 23646 50333 V2
...
```

- `reshape()`; stack and unstack variables, according to parameters.

```r
reshape(zz, idvar = "id", timevar = "var", varying = list(c("V1", "V2", "V3", "V4")), direction = "long")
```

- `ftable()`; flatten multidimensional matrices (arrays);
- `read.ftable()`.
- `data()`; list of datasets.
- `data(package = "rpart")`; load package.
- `edit(input)`; open a mini-spreadsheet.
- `xnew <- edit(input)`; open and save it as a new dataset.
- and many more.

**Encoding**

- utf-8 for Linux and OS X.
- UCS-2LE and UTF-16 for Windows.

```r
# examples
readLines("filename.txt", encoding = "UTF-8")
readLines("filename.txt", encoding = "UCS2LE")
read.delim("clipboard", fileEncoding="UTF-16")
```

**Output**

- `write.table()`; many parameters.
- `write.matrix()`.
- `write.foreign()`.

**Check a file**

- `readLines("aab.txt")`.
- `readLines("aab.txt", 1)`.
- `unlink("aab.txt")`; delete the file on the working directory.

**Directory management**

- `getwd()`; get the current working directory.
- `setwd()`; set the current working directory.
- `file.show(filepath)`.
- `system.file()`

**Spreadsheet editor and edition in R**

- `fix(c)`; edit object c; a vetor, list, data frame, etc. in a mini-spreadsheet.
- `data.entry(c, mode=NULL, Names=NULL), dataentry(c), de(c, Modes=list(), Names=NULL)`; idem.
- `vi(file= )`; invoke a text editor.
- `emacs(file= )`; idem.
- `pico(file= )`; idem.
- `xemacs(file= )`; idem.
- `xedit(file=)`; idem.

## 8, Probability distributions

**Distribution**

- `beta`.
- `binom`.
- `cauchy`.
- `chisq`.
- `exp`.
- `f`.
- `gamma`.
- `geom`.
- `hyper`.
- `lnorm`.
- `logis`.
- `nbinom`.
- `norm`.
- `pois`.
- `signrank`.
- `t`.
- `unif`.
- `weibull`.
- `wilcox`.
- and many more.

**Prefixes**

- `d`; density.
- `p`; CDF or cumulative density function.
- `q`; quantile.
- `r`; random deviates or simulation or number generation.

**Distribution operation**

Commands, prefix + distribution, examples:

- `dbeta(x= )`.
- `pbinom(q= , lower.tail= , log.p= )`.
- `qcauchy(p= , lower.tail= , log.p= )`.
- `rchisq(n or r= )`.
- `ptukey()`; studentized (t) distribution
- `qtukey()`.
- `dmultinom()`.
- `ltinomial`.
- `rmultinom()`.
- etc.

**Descriptive statistics**

```r
# overview
summary(cars)
str(cars)
boxplot(cars$speed)
boxplot(cars$dist)

# Tukey five-number summaries
fivenum(cars$speed)

# histograms and bar charts
stem(cars$speed)
hist(cars$speed)
barplot(cars$speed)
dotchart(cars$speed)

# scatter plot
plot(cars$speed, cars$dist)
lines(cars$speed, cars$dist)

# add a (1-d) representation
plot(cars$speed, cars$dist)
rug(cars$speed, ticksize = 0.03)
plot(cars$speed, cars$dist)
rug(cars$dist, ticksize = 0.03)

# normality of the residuals, linearity
qqnorm(cars$speed)
qqline(cars$speed)

qqnorm(cars$dist)
qqline(cars$dist)

qqplot(cars$speed, cars$dist)

# normality test
shapiro.test(cars$speed)

# Kolmogorov-Smirnov test on a sample
ks.test(cars$speed[10], "pnorm", mean = mean(cars$speed), sd = sqrt(var(cars$speed)))
ks.test(cars$speed[40], "pnorm", mean = mean(cars$speed), sd = sqrt(var(cars$speed)))
```

**Test**

- `t.test(A,B)`; true difference of means is not 0, difference means.
- `var.test(A,B)`; true ratio of variances is not 1, difference variances.
- `t.test(A,B, var.equal=TRUE)`; true difference of means is not 0.
- `wilcox.test(A,B)`; rank sum with continuity correction, continuous distribution.
- and many more.

**Normality**

- `plot(ecdf(A), do.points=FALSE, verticals=TRUE, xlim=range(A, B))`
- `plot(ecdf(B), do.points=FALSE, verticals=TRUE, add=TRUE)`
- `ks.test(A,B); maximal vertical distance between the two ecdf`

## 9, Grouping, loops and conditional execution

- `&`; AND.
- `&&`; AND; evaluates left to right, examining only the first element of each vector.
- `|`; OR.
- `||`; OR; evaluates left to right, examining only the first element of each vector.
- `xor()`; elementwise exclusive OR.
- `isTRUE(x)`; is true if and only if x is a length-one logical vector whose only element is TRUE and which has no attributes.
- `ifelse(condition, a, b)`; if `condition` is proven true, return `a`, or else, return `b`.

**Loop**

<sub>looping<\sub>

- `for(var in seq) expr`.
- `while(condition is true) expr`.
- `repeat expr`.
- `break`; break out of a for, while or repeat loop; control is transferred to the first statement outside the inner-most loop.
- `next`; halt the processing of the current iteration and advance the looping index.

## 10, Writing you own functions

**Simple custom function**

```r
# define
myfunction <- function(arg1, arg2, ...){
statements
return(object)
}

# call
myfunction(arg1, arg2, ...)
```

It is possible to remplace function arguments with variables and objects.

**Complex custom function**

Involve conditions and loops.

```r
twosam <- function(y1, y2) {
    n1 <- length(y1); n2 <- length(y2)
    yb1 <- mean(y1);
    yb2 <- mean(y2)
    s1 <- var(y1);
    s2 <- var(y2)
    s <- ((n1 - 1) * s1 + (n2-1) * s2) / (n1 + n2 - 2)
    tst <- (yb1 - yb2) / sqrt(s * (1 / n1 + 1 / n2))
    tst
}
```

**Define and call on the same line**

- `tstat <- twosam(data$male, data$female); tstat`

Several commands and functions can be called on the same line with:

- `command1; command2; function1; function2` 

**Two ways of defining and calling a function**

```r
multy <- function(x, y) {
	x * y
	}

# with a binary operator
"%!%" <- function(x, y) {
	x * y
	}

x <- 2
y <- 2

multy(x, y)
[1] 4

x %!% y
[1] 4

```

Matrix multiplication operator, `%*%`, and the outer product matrix operator, `%o%`, are other examples of binary operators.

**Function in a function**

```r
# case 1
area <- function(f, a, b, eps = 1.0e-06, lim = 10) {
    fun1 <- function(f, a, b, fa, fb, a0, eps, lim, fun) {
        d <- (a + b)/2
        h <- (b - a)/4
        fd <- f(d)
        a1 <- h * (fa + fd)
        a2 <- h * (fd + fb)
        if(abs(a0 - a1 - a2) < eps || lim == 0)
            return(a1 + a2)
        else {
            return(fun(f, a, d, fa, fd, a1, eps, lim - 1, fun) 
                fun(f, d, b, fd, fb, a2, eps, lim - 1, fun))
        }
    }
    fa <- f(a)
    fb <- f(b)
    a0 <- ((fa + fb) * (b - a))/2
    fun1(f, a, b, fa, fb, a0, eps, lim, fun1)
}

# case 2
f <- function(x) {
    y <- 2*x
    print(x)
    print(y)
    print(z)
}

# case 3
cube <- function(n) {
    sq <- function() n*n
    n*sq()
}

# case 4
open.account <- function(total) {
    list(
        deposit = function(amount) {
            if(amount <= 0)
                stop("Deposits must be positive!\n")
            total <<- total + amount
            cat(amount, "deposited. Your balance is", total, "\n\n")
        },
        withdraw = function(amount) {
            if(amount > total)
                stop("You don’t have that much money!\n")
            total <<- total - amount
            cat(amount, "withdrawn. Your balance is", total, "\n\n")
        },
        balance = function() {
            cat("Your balance is", total, "\n\n")
        }
    )
}

ross <- open.account(100)
robert <- open.account(200)

ross$withdraw(30)
ross$balance()
robert$balance()
ross$deposit(50)
ross$balance()
ross$withdraw(500)
```
**Name**

Add,  modify, and remove (with `names(x) <- NA or 0`) names.

- `names()`.
- `rownames()`.
- `colnames()`.
- `dimnames()`.

**Customizing startup**

Customize the R environment through a directory initialization file; commands that you want to execute every time R is started under your system.

R will always source the Rprofile.site file first. 

On Windows, the file is in C:\Program Files\R\R-n.n.n\etc. You can also place a .Rprofile file in any directory that you are going to run R from or in the user home directory. 

Individual users control over their workspace and allows for different startup procedures in different working directories.

If no .Rprofile file is found in the startup directory, then R looks for a .Rprofile file in the user’s home directory and uses that (if it exists) environment variable R_PROFILE_USER is set. The file it points to is used instead of the .Rprofile files. 

Function named .First() in either of the two profile files or in the .RData image has a special status: initialize the environment

Sequence in which files are executed is:

1. Rprofile.site
2. the user profile
3. .RData
4. .First()
5. .Last(), if defined, is (normally) executed at the very end of the session.

**List function and method**

- `methods(class = "data.frame")`; list methods associated with the class.
- `methods(plot)`; list methods specific to the object.

```r
methods(class = "data.frame")
 [1] $             $<-           [             [[            [[<-         
 [6] [<-           aggregate     anyDuplicated as.data.frame as.list      
[11] as.matrix     by            cbind         coerce        dim          
[16] dimnames      dimnames<-    droplevels    duplicated    edit         
[21] format        formula       head          initialize    is.na        
[26] Math          merge         na.exclude    na.omit       Ops          
[31] plot          print         prompt        rbind         row.names    
[36] row.names<-   rowsum        show          slotsFromS3   split        
[41] split<-       stack         str           subset        summary      
[46] Summary       t             tail          transform     unique       
[51] unstack       within       
see '?methods' for accessing help and source code

methods(plot)
 [1] plot.acf*           plot.data.frame*    plot.decomposed.ts*
 [4] plot.default        plot.dendrogram*    plot.density*      
 [7] plot.ecdf           plot.factor*        plot.formula*      
[10] plot.function       plot.hclust*        plot.histogram*    
[13] plot.HoltWinters*   plot.isoreg*        plot.lm*           
[16] plot.medpolish*     plot.mlm*           plot.ppr*          
[19] plot.prcomp*        plot.princomp*      plot.profile.nls*  
[22] plot.R6*            plot.raster*        plot.spec*         
[25] plot.stepfun        plot.stl*           plot.table*        
[28] plot.ts             plot.tskernel*      plot.TukeyHSD*     
see '?methods' for accessing help and source code
```

Difference:

- `plot()`; a generic method.
- `plot.` ; a specific method such as `plot.ts()` for example.

## 11, Statistical models in R

**Regression**

```r
y <- 1:10
x <- 1:10

a <- lm(y <sub>x)
a
Call:
lm(formula = y <sub>x)

Coefficients:
(Intercept)            x  
  1.123e-15    1.000e+00  


# no intercept, through the origin
b <- lm(y <sub>0 + x)
b
Call:
lm(formula = y <sub>0 + x)

Coefficients:
x  
1  


# no intercept
c <- lm(y <sub>-1 + x)
c
Call:
lm(formula = y <sub>-1 + x)

Coefficients:
x  
1  


d <- lm(y <sub>x - 1)
d
Call:
lm(formula = y <sub>x - 1)

Coefficients:
x  
1  


# log
e <- lm(log(y) <sub>x)
e
Call:
lm(formula = log(y) <sub>x)

Coefficients:
(Intercept)            x  
     0.2432       0.2304  


# quadratic
f <- lm(y <sub>1 + x + I(x^2))
f
Call:
lm(formula = y <sub>1 + x + I(x^2))

Coefficients:
(Intercept)            x       I(x^2)  
  1.123e-15    1.000e+00    5.699e-18  


# polynomial
g <- lm(y <sub>X + poly(x, 2))



# weighted regression
fm1 <- lm(y <sub>x, data = dummy, weight = 1 / w^2)


# and more
new.model <- update(old.model, new.formula)

fm05 <- lm(y <sub>x1 + x2 + x3 + x4 + x5, data = production)

fm6 <- update(fm06, . <sub>. + x6)
smf6 <- update(fm6, sqrt(,) <sub>.)
```

**Explore the results**

- `summary(regression results)`.
- `vcov()`; variance-covariance matrix.
- `aov(formula, data = data.frame)`.
- `anova(fitted.model.1, fitted.model.2, ...)`
- and many more.

<sub>anova, coeficient, coef, deviance, residuals, effects, formula, model, kappa, labels, plot, predict, proj, projection<sub>

**Stepwise Regression**

Select a suitable model by adding or dropping variables and preserving hierarchies. The best model with the smallest AIC (Akaike’s Information Criterion) is discovered with the search.

**Generalized least squares**

<sub>gls, binomial, logit, probit, log, cloglog, gaussian, identity, log, inverse, gamma, identity, inverse, log, inverse.gaussian, 1/mu^2, identity, inverse, log, poisson, identity, log, sqrt, quasi-likelihood, logit, probit<\sub>

```r
fitted.model <- glm(formula, famili=family.generator, data = data.frame)
```

**Nonlinear least squares**

<sub>nls<\sub>

- `nlm(function)`

**Maximum likehood**

<sub>ml<\sub>

When errors are not normal.

- `ml(function)`

**Mixed model**

- `nlme` package.

**Local Approximating Regression**

Nonparametric local regression function.

- `lrf <- lowess(x, y)`.

**Robust regression**

`MASS` package.

**Additive model**

- `acepack` package.
- `mda` package.
- `gam` package.
- `mgcv` package.

**Tree-based model**

<sub>decision, classification tree, random forest<\sub>

- `rpart` package.
- `tree` package.

## 12, Graphical procedures

[www.statmethods.net/advgraphs](http://www.statmethods.net/advgraphs/)

**Graphic, packages**

- `lattice` package.
- `ggplot2` package.
- `grid` package.
- `ggobi`, `rgl` packages; for interactive graphics, 3D, and surfaces.
- and many more.

**Basic plot**

- `plot()`.
- `boxplot()`..
- `lines(x,y)`; add a line to a basic plot.
- `pairs()`; multivariate, pairwise scatterplot matrix. 
- `coplot(a <sub>b | c)`; scatter plot of a <sub>b given c, a factor vector (levels)
- `coplot(a <sub>v | c + d)`.
- `pie()`.
- `hist(x)`; where `nclass = n` and `breaks = b`.
- `barplot()`; can be horizontal or vertical.
- `dotchart(x, ...)`; a case of bar chart.
- and many more with lots of options.

**qq plot**

- `qqnorm(x)`.
- `qqline(x)`.
- `qqplot(x, y)`; comparison.

**Picture**

- `image(x, y, z, ...)`; grid of rectangles with colors corresponding to the values in z.
- `contour(x, y, z, ...)`; z add contour lines (even to an existing plot).
- `persp(x, y, z, ...)`; perspective plots of a surface over the x–y plane.

**Graphic arguments and parameters**

[www.statmethods.net/advgraphs/parameters](http://www.statmethods.net/advgraphs/parameters.html)

```r
x1 <- rnorm(1000, 0.4, 0.8)
x2 <- rnorm(1000, 0.0, 1.0)
x3 <- rnorm(1000, -1.0, 1.0)
hist(x1, width = 0.33, offset = 0.00, col = "blue", xlim = c(-4,4),
     main = "Histogram of x1, x2 & x3",
     xlab = "x1 - blue, x2 - red, x3 - green")
hist(x2, width = 0.33, offset = 0.33, col = "red", add = TRUE)
hist(x3, width = 0.33, offset = 0.66, col = "green", add = TRUE)
```

- `add = TRUE`; superimpose a plot on another plot.
- `axes = FALSE`; no axes.
- `axis(side,...)`; 1 to 4, bottom, left, top, right.
- `log = "x", "y", "xy"`; difference scale.
- `type = "p"` (points); `"l"` (lines), `"b"` (p+l), `"o"` (l+p), `"h"` (vertical lines from points to the zero axis), `"s"` (step-function), `"n"` (not plotting).
- `xlab = "bla"`.
- `ylab = "bla"`.
- `main = "bla"`.
- `sub = "bla"`.
- `title(main, sub)`.
- `points(x, y)`; add points on top of `plot()`.
- `text(x, y, labels,...)`; add text to each point.
- `plot(x, t, type = "n"); text(x, y, names)`; replace the dot with text.
- `abline(a, b)`; add a line.
- `abline(0, 1, lty = 3)`.
- `abline(coef(fm))`.
- `abline(coef(fm1), col = "red")`.
- `abline(h = y)`; or `h = value`; add a horizontal line.
- `abline(v = x)`; or `v = value`; add a vertical line.
- `abline(lm.obj)`.
- `legend(x, y, legend, ...)`; add a legend at a specified position.
- `fill = v`; add a vector of the same length as a legend.
- `lty = 2`; line style.
- `lwd = 1.5`; line width.
- `pch = 0`; dot style.
- `par()`; list of permanent graphic parameters.
- `par(c("col", "lty"))`; limit the list of parameters.
- `par(col = 4, lty = 2)`; set the parameters for all plots.
- `plot(x, y, pch = "+")`; will set a temporary parameter inside a plot.
- `pch = "+"`, `pch = 4`.
- `col = "red"`; dot color.
- `col =  "red"`.
- `col.axis = "red"`.
- `col.lab = "red"`.
- `col.main = "red"`.
- `col.sub = "red"`.
- `font = "red"`.
- `font.axis = "red"`.
- `font.lab = "red"`.
- `font.main = "red"`.
- `font.sub = "red"`.
- `adj = -0.1`; adjust the text to the plotting position (-1...-0.5...0...0.5...1), from left to right, 0 being the center.
- `cex = 1.5`; character 50% larger.
- `cex.axis = 1.5`.
- `cex.lab = 1.5`.
- `cex.main = 1.5`.
- `cex.sub = 1.5`.
- `lab = c(5, 7, 12)`; the first two numbers are the desired number of tick intervals on the x and y axes respectively. The third number is the desired length of axis labels.
- `las = 1`; orientation of axis labels (text and numbers). 0 means always parallel to axis, 1 means always horizontal, and 2 means always perpendicular to the axis.
- `mgp = c(3, 1, 0)`; position of the axis components.
- `tck = 0.01`; length of tick marks.
- `xaxs = "r"`; axis style.
- `xaxs = "i"`; inside.
- `mai = c(1, 0.5, 0.5, 0)`; margins in inches around the plot.
- `mar = c(4, 2, 2, 1)`; in text lines.
- and many more.

R allows you to create an n by m array of plots on a single page:

- `mfcol = c(3.2)`; 3 rows, 2 columns, 6 plotting areas.
- `mfrow = c(3,2)`; idem but filled by rows.
- `omi = c(1, 0.5, 0.5, 0)`; margins between plots.
- `oma = c(1, 0.5, 0.5, 0)`; margins outside plots.
- `mfg = c(2, 2, 3, 2)`; position of the current figure in a multiple figure environment. The first two numbers are the row and column of the current figure; the last two are the total number of rows and columns in the multiple figure array; in other words, 2nd row and 2nd column in a 3x2 panel.
- `fig = c(4, 9, 1, 4) / 10`; position of the current figure on the page. Values are the positions of the left, right, bottom and top edges, respectively, as a percentage of the page measured from the bottom left corner.
- and many more.

**Geometric shapes**

- `polygon(x, y, ...)`; x, y are vectors containing the coordinates of the vertices of the polygon.

**Graphic help**

- `help(plotmath)`.
- `example(plotmath)`.
- `demo(plotmath)`.
- `help(Hershey)`.
- `demo(Hershey)`.
- `help(Japanese)`.
- `demo(Japanese)`.

**Graphic text and mouse**

Leave graphic marks and texts.

- `locator(n, type)`; select with mouse a max of n locations to mark on the graph (left click, right click to stop).
- `text(locator(1), "Outlier", adj=0)`; select with mouse a location to add a string on the graph.
- `identify(x, y)`; add a label to a dot with mouse.
- `identify(x, y, labels)`.
- `identify(x, y, "yes")`.

**Graphic device**

- `split.screen()`; FALSE or number of regions within the current device which can, to some extent, be treated as separate graphics devices. It is useful for generating multiple plots on a single device.
- `layout()`; divides the device up into as many rows and columns as there are in matrix mat.

**Graphic device driver**

Open a special graphics window.

- `X11()`; under UNIX.
- `windows()`; under Windows.
	- `win.printer()`.
	- `win.metafile()`.
- `quartz()`; under OS X.
- `dev.new()`; returns the return value of the device opened, usually invisible NULL.
- `grid()` adds an rectangular grid to an existing plot.
- `postscript()`; starts the graphics device driver for producing PostScript graphics.
	- `postscript("file.ps", horizontal=FALSE, height=5, pointsize=10)`.
	- `postscript("plot1.eps", horizontal=FALSE, onefile=FALSE, height=8, width=6, pointsize=10)`.
- `pdf()`; starts the graphics device driver for producing PDF graphics.
- `png()`; idem.
- `jpeg()`; idem.
- `tiff()`; idem.
- `bitmap()`; idem.
- `dev.off()`; shuts down the specified (by default the current) device.
- `dev.set()`; dev.set makes the specified device the active device.
- `dev.list()`; returns the numbers of all open devices.
- `dev.next()`, `dev.prev()`; return the number and name of the next / previous device in the list of devices.
- graphics.off(); terminate all devices.

## 13, Packages

- `install.package()`; on the computer.
- `library()`; load it.
- `search()`; check what is loaded.
- `loadedNamespaces()`; idem.
- `help.start()`; start the HTML help system, package section.

Find out about packages:

[CRAN.R-project.org](https://CRAN.R-project.org/)

[www.bioconductor.org](https://www.bioconductor.org/)

[www.omegahat.org](http://www.omegahat.org/)

## 14, OS facilities

Manage files with Linux or Windows or RStudio.