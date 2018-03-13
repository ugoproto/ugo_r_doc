<!--
---

[TOC]
-->
---

**Foreword**

Notes, leads, and ideas on what R can do. *REF: reference(s) to the book.* From Springer, 2014.

---

## A, Presentation

### GUI

- Rcommander; `Rcmdr` pachage.

- [The R Commander: A Basic-Statistics GUI for R](http://socserv.mcmaster.ca/jfox/Misc/Rcmdr/)

*REF: p.3-5*

## B, Datasets

- Datasets.

## Part 1, Basics
## Chapter 1, Basic Concept, Organizing Data

### Editors

- RStudio.
- Tinn-R.
- JGR.
- Emacs/ESS.

### Data entry

- `x <- 2`.
- `2 -> 2`.

### Exponent

- `exp(1)`.

### Logarithm

- `log(3)`.
- `log(x = 3)`.
- `log(x = 3, base(exp(1))`.
- `log(3, exp(1))`.

### Factorial

- `factorial(2)`.

### Find out about an object, a variable (`is.`)

- `is.character()`.
- `is.vector()`.
- `is.character()`.
- `is.character()`
- and many more.

### Convert (`as.`)

- `as.character()`.
- `as.raw()`.
- `as.date()`.
- and many more.

### Array and matrix

```r
matrix(1:12, nrow = 4, ncol = 3, byrow = FALSE)
     [,1] [,2] [,3]
[1,]    1    5    9
[2,]    2    6   10
[3,]    3    7   11
[4,]    4    8   12

matrix(1:12, nrow = 4, ncol = 3, byrow = TRUE)
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
[3,]    7    8    9
[4,]   10   11   12

array(1:12, dim = c(2, 2, 3))
, , 1

     [,1] [,2]
[1,]    1    3
[2,]    2    4

, , 2

     [,1] [,2]
[1,]    5    7
[2,]    6    8

, , 3

     [,1] [,2]
[1,]    9   11
[2,]   10   12
```

### Vector

- `vec <- c(1.1, 2.2, 3.5)`.
- `vec <- 1:3`.

### Sequence

- `seq(1:3)`.
- `1:3`.

### List

```r
c(1:3) # vector
[1] 1 2 3

# vs

list(1:3) # list
[[1]]
[1] 1 2 3
```

### Data Frame

<sub>table, tabular</sub>

- `data.frame(name = c(), name = c(), name = c(), etc)`; each column is a vector with a name.

### Time series

```r
> ts(1:10, frequency=4, start=c(1959,2))
     Qtr1 Qtr2 Qtr3 Qtr4
1959         1    2    3
1960    4    5    6    7
1961    8    9   10     
```

### Class and mode

<sub>type, data, variable, object</sub>

- `mode()`.
- `class()`; mode != class.
- `typeof()`; type of storage.

## Chapter 2, Import-Export and Producing Data

<sub>import, export, i/o</sub>

### Input data from files

```r
read.table(file = path/file.txt, header = TRUE, sep= "\t", dec=".", row.names = 1)
```

- `attach(data)`; dataset is attached to .GlobalEnv.
- `search()`; search objects in .GlobalEnv, including attached dataset,
- `detach(data)`.

More about .GlobalEnv in Chapter 7, Session Management.

**Read .csv and .tsv**

- `read.csv()`.
- `read.csv2()`.
- `read.delim()`.
- `read.delim2()`.
- and many more.

**Read text files**

- `read.ftable("file.txt", row.var.names = c(...), col.vars = list())`.
- `scan("file.txt", skip = 9, nlines = 1, what = "", dec = "")`.
- and many more.

**Read software files**

- `.sav`; SPSS.
	- `read.spss`.
- `.mtp`; Minitab.
	- `read.mtp`.
- `.xpt`; SAS en data.frame.
	- `read.xport`.
- `.mat`; Matlab.
	- `readMat()`.
- and many other formats and commands.

### Ouput data and export

- `lookup.xport()`; for SAS.
- `write.table(data, file="file.txt", sep="\t")`.
- `xlsReadWrite()`.
- and many more.

### Measure computation time

```r
# start the timer
tmps <- Sys.time()

# run
dbsnp <- read.table("file")

# stop the timer
Sys.time() - tmps
```

### Produce by repetition

<sub>repeat</sub>

```r
rep(1:4, reach = 2, len = 10)
[1] 1 2 3 4 1 2 3 4 1 2
```

### Produce random numbers

```r
# generate random numbers between 0 and 1
runif(5)
[1] 0.2424283 0.6140730 0.4824881 0.7263319 0.1381030

runif(5, min = 2, max = 7)
[1] 4.588744 5.522278 4.307162 6.248397 3.854982
```

More on random number in Chapter 10, Random Variables, Laws, and Simulation.

**Produce random number following a distribution**

```r
# generate random numbers following the normal distribution
rnorm(5)
[1]  0.8752170  1.3869022 -0.4419174 -0.6129075 -1.6987139
```

Generate numbers with other distributions.

**Produce random number by sampling a population**

```r
urne <- 0:9

# 20 draws from 'urne'
sample(urne, 20, replace = TRUE)
[1] 5 4 2 2 9 7 4 6 2 2 7 8 3 3 9 6 6 1 1 0
```

### Produce data by manual input (vector-like)

- First,  `z <- scan()`.
- Second, input data in the prompt.

### Produce data with a mini-spreadsheet (tabular-like)

- `x <- as.data.frame(de(""))`; open a spreadsheet.
- `data.entry("")`; alternatively.
	- Input data; column are variables like in a data frame.
- `fix(x)`; invokes edit on x, then assigns the new (edited) version of x to the user's workspace.

### List of objects

- `ls()`; list of objects.
- `rm(list = ls())`; remove all the objects.

### Read from and write to a database

- `RODBC` package.
- `odbcConnect()`.
- `sqlQuery()`.
- `odbcClose()`.

*REF: p.82-84*

### File management

- `file.choose()`; open a window.

### Read from the clipboard

- First, copy from a spreadsheet or a table.
- Second, `read.clipboard()`.

## Chapter 3, Data Manipulation

### Arithmetics

```r
x <- c(1,2,3)
y <- c(4,5,6)

x + y
[1] 5 7 9
```

### Built-in functions

- `length(vec)`; length.
- `sort(vec, decreasing = TRUE)`; sort.
- `rev(vec)`; inverse sorting.
- `order(vec)`; sort a vector according to the names or a list of strings.
- `names(vec) <- 1:9`; attribute names.
- `rank(vec)`; rank the elements.
- `unique(vec)`; remove doubles.
- `duplicated(vec)`; create a TRUE/FALSE vector indicating doubles.
- `x %% y`;  modulus (x mod y).
- `x %/% y`; integer division.

### Dimension functions

<sub>number, row, column, dimension</sub>

- `dim(df)`.
- `nrow(df)`.
- `ncol(df)`.

### Name functions

- `dimnames(df)`.
- `names(df)`, `colnames(df)`.
- `rownames(df)`.

### Merge functions

<sub>combine</sub>

- `cbind()`.
- `rbind()`.

*REF: p.98*

```r
y <- array(1:12, dim = c(4, 3))

y
     [,1] [,2] [,3]
[1,]    1    5    9
[2,]    2    6   10
[3,]    3    7   11
[4,]    4    8   12

y <- cbind(y, c(100, 101, 102, 103))

y
     [,1] [,2] [,3] [,4]
[1,]    1    5    9  100
[2,]    2    6   10  101
[3,]    3    7   11  102
[4,]    4    8   12  103

merge(x, y)
  V1 V2 V3  V4
1  1  5  9 100
2  2  6 10 101
3  3  7 11 102
4  4  8 12 103

# idem with rows
```

*REF: p.96-98*

- `gtools` package.
- `smartbind(x,y)`; for two data frames, similar to merge.

### `apply` functions and family

<sub>excel, wrangle</sub>

Among the most useful function for 'wrangling' data. Excel-like power. When and how to use them.

- `apply()`.
- `lapply()`.
- `sapply()`.
- `mapply()`.
- `by()`.
- `with()`.
- `replicate()`.
- `transform()`.
- `rowSums(df)`.
- `colSums(df)`.
- `rowMeans(df)`.
- `colMeans(df)`.
- `sweep()`.
- `stack()`.
- `unstack()`.
- `aggregate()`

*REF: p.99*

### Sweep functions

```r
u
  V1 V2 V3  V4
1  1  5  9 100
2  2  6 10 101
3  3  7 11 102
4  4  8 12 103

# removes pattern '3, 5, 3, 5, etc.'
sweep(u, MARGIN = 1, STATS = c(3, 5), FUN = "-")
  V1 V2 V3 V4
1 -2  2  6 97
2 -3  1  5 96
3  0  4  8 99
4 -1  3  7 98

# divide by a vector
sweep(u, MARGIN = 2, STATS = c(2, 2, 3, 3), FUN = "/")
   V1  V2       V3       V4
1 0.5 2.5 3.000000 33.33333
2 1.0 3.0 3.333333 33.66667
3 1.5 3.5 3.666667 34.00000
4 2.0 4.0 4.000000 34.33333
```

*REF: p.100-101*

### Stack functions

<sub>stack, unstack</sub>

```r
u
  V1 V2 V3  V4
1  1  5  9 100
2  2  6 10 101
3  3  7 11 102
4  4  8 12 103

v<- stack(u)
v
   values ind
1       1  V1
2       2  V1
3       3  V1
4       4  V1
5       5  V2
6       6  V2
7       7  V2
8       8  V2
9       9  V3
10     10  V3
11     11  V3
12     12  V3
13    100  V4
14    101  V4
15    102  V4
16    103  V4

w <- unstack(v)
w
  V1 V2 V3  V4
1  1  5  9 100
2  2  6 10 101
3  3  7 11 102
4  4  8 12 103
```

### Aggregation functions

<sub>aggregate</sub>

```r
w
  V1 V2 V3  V4
1  1  5  9 100
2  2  6 10 101
3  3  7 11 102
4  4  8 12 103

fac <- c("a", "b", "b", "a")
x <- cbind(w, fac)
x
  V1 V2 V3  V4 fac
1  1  5  9 100   a
2  2  6 10 101   b
3  3  7 11 102   b
4  4  8 12 103   a

aggregate(w, by = list(x$fac), sum)
  Group.1 V1 V2 V3  V4
1       a  5 13 21 203
2       b  5 13 21 203
```

*REF: p.101*

### Boolean & logical functions

- `logical(2)`; generate two FALSE in a vector; change the length.
- `!logical(2)`; generate two TRUE.
- `as.logical(vec)`.
- `is.logical(vec)`.
- `isTRUE()`.
- `&`; AND.
- `&&`; sequential AND.
- `|`; OR.
- `||`; sequential OR.
- Prefer `&&` over `&`, and `||` over `|`. Assessments go from left to right, and keep on going as long as the conditions are TRUE.
- `xor()`; exclusive OR.
- `if`, `else`.
- `any()`; if one or another is TRUE.
- `all()`; if all are TRUE.
- `identical()`; if all are identical.
- `all.equal()`.
	- `==` or `all.equal` (and `!=`) can yield a FALSE because decimal are different on large numbers.
	- `all.equal(x, y, tolerance = 10^-6)` fixes the problem.
- `ifelse(cond, a, b)`; if `cond` is TRUE, `a`, else, `b`.
	- `x <- c(3:-2); sqrt(ifelse(x >= 0, x, NA)`.

*REF: p.126-127*

### Venn functions

```r
A <- 1:3
B <- 3:6

is.element(1, A)
[1] TRUE
is.element(4, A)
[1] FALSE
is.element(4, B)
[1] TRUE

all(A %in% B)
[1] FALSE
all(B %in% A)
[1] FALSE

intersect(A, B)
[1] 3
union(A, B)
[1] 1 2 3 4 5 6

setdiff(A, B)
[1] 1 2
setdiff(B, A)
[1] 4 5 6
intersect(A, B)
[1] 3
```

### Vector functions

- `vec[2]`; extract.
- `vec[2:5]`; extract.
- `vec[c(T, F, T)]`; extraction with filter.
- `vec[vec > 4]`; conditional extraction.
- `vec[vec == 3]`.
- `vec[which.max(z)]`; extract the maximum value.
- `vec[which.min(z)]`.
- `vec > 4`; yield a vector of TRUE or FALSE.
- `vec[-2]`; exclude.
- `vec[-c(1,5)]`; exclude.

### Search functions

- `masque <- c(TRUE, FALSE)`.
- `which(masque)`; return the TRUE indices.
- `which.min(x)`; return the index with minimum value.
- `which.max(x)`.

### Replace functions

- `z[c(1, 5)] <- 1`; replace value 1 and 5 by 1.
- `z[which.max(z)] <- 0`; replace the maximum value.
- `z[z == 0] <- 8`; replace zeros and FALSE.

### Extend a vector

- `vecA`.
- `vecB <- c(vecA, 4, 5)`.
- `vecC <- c(vecA[1:4], 8, 5, vecA[5:9])`.

### Matrix and array

- `mat[r, c]`; extract.
- `mat[1, 2]`.
- `mat[,2]`; all rows, column 2 only.
- `mat[1, ]`; all columns, row 1 only.
- `mat[c(1, 3), c(4:5)]`.
- `mat[, 1, drop = FALSE]`; avoid making a (horizontal) row with a (vertical) column.
- `mat[ind]`; matrix index.
- `array[r, c, m]`; extract.

*REF: p.110-113*

### Lists

```r
char <- c("a", "b", "c")
numb <- c(1, 2, 3)
greek <- c("alpha", "beta", "gamma")

x <- list(char, numb, greek)
x
[[1]]
[1] "a" "b" "c"

[[2]]
[1] 1 2 3

[[3]]
[1] "alpha" "beta"  "gamma"

names(x) <- c("char", "numb", "greek")
x
$char
[1] "a" "b" "c"

$numb
[1] 1 2 3

$greek
[1] "alpha" "beta"  "gamma"

x[2]
$numb
[1] 1 2 3

x[[2]][2]
[1] 2

x$numb[2]
[1] 2
```

*REF: p.113-115*

### String

```r
"bla bla bla"
[1] "bla bla bla"

noquote("bla bla bla")
[1] bla bla bla
 
sQuote("bla bla bla")
[1] "‘bla bla bla’"

dQuote("bla bla bla")
[1] "“bla bla bla”"
```

### Text

<sub>wrangle, text, string, character, natural language processing, nlp</sub>

- `format()`; and arguments:
	- `digits`.
	- `trim`.
	- `digit`.
	- `nsmall`.
	- `justify`.
	- `width`.
	- `na.encode`.
	- `decimal.mark`.
	- `drop0trailing`.
	- and many more.

- `cat("current working dir: ", wd)`; print the objects, concatenate the representations.
- `printf("hello %d\n", 56)`; mix text and data; pythonic print.
- `print(paste0("current working dir: ", wd))`.
- `nchar()`; number of characters.
- `x[nchar(x) > 2]`.
- `x[x %n% c(letters, LETTERS)]`; retrieve letters, patterns or strings in a text object; alike Venn.
- `paste(ch1, ch2, sep = "-")`; concatenate.
- `paste0(ch1, ch2)`; concatenate.
- `substring("abcdef", first = 1:3, last = 2:4)`; create subsets `ab`, `bc`, `cd`.
- `strsplit(c("",""), split=" ")`; break down a string.
- `grep("i", c())`; extract an object index.
- `gsub("i", "L", c())`; substitute.
- `sub()`; substitute the first occurrence.
- `tolower()`.
- `toupper()`.

### Date and time

<sub>convert, extract</sub>

- `Sys.time()`.
- `date()`.
- `Sys.setlocale()`.
- `as.numeric()`.
- `strptime()`; extract time.
- `as.POSIXlt()`.

*REF: p.120-123*

### Custom functions (two examples)

```r
a <- 2
b <- -3

# quadratic
f <- function (x) { x**2 - 2*x - 2 }

f(a)
[1] -2

f(b)
[1] 13

# test
g <- function (x, y) {
  if (x <= y) {
    z <- y - x
    print("x smaller")
  } else {
    z <- x - y
    print("x larger")
  }
} 

g(a, b)
[1] "x larger"

g(a, abs(b))
[1] "x smaller"
```

More about custom functions in [Chapter 6, Initiation to R Programming](#chapter-6,-initiation-to-r-programming).

### Loops structure

- `for`.
- `while`.
- `repeat`.

```r
# while
while(x + y < 7) { x <- x + y }

# for
for (i in 1:4) {
    if (i == 3) break
    for (j in 6:8) {
        if (j == 7) next
        j <- i + j
    }
}

# repeat
i <- 0
repeat {
    i <- i + 1
    if (i == 4) break
}
```

**Loop example**

```r
IMC <- function (poids, taille) {
  imc <- poids / taille^2
  names(imc) <- "IMC"
  return(imc)
}

IMC(100, 1.90)
     IMC 
27.70083 

p <- c(100, 101, 95, 97)
t <- c(1.90, 1.75, 1.68, 1.92)

IMC(p, t)
     IMC     <NA>     <NA>     <NA> 
27.70083 32.97959 33.65930 26.31293 

for (i in 1:4) {
  print(IMC(data[i,1], data[i,2]))
}
     IMC 
27.70083 
     IMC 
32.97959 
    IMC 
33.6593 
     IMC 
26.31293 
```

**Loops increase computation time**

```r
system.time(for (i in 1:1000000) sqrt(i))
utilisateur     système      écoulé 
       0.19        0.01        0.22 

system.time(sqrt(1:1000000))
utilisateur     système      écoulé 
       0.07        0.00        0.07 
```

*REF: p.133-136*

### Binary and decimal

<sub>convert</sub>

- `bin2dec()`.
- `dec2bin()`.

*REF: p.136-144, 147-152*

## Chapter 4, R Documentation

### Help

- `?functionname`.
- `help(functionname)`.
- `help.start()`; user manual in the browser.
- `apropos('mean')`; object with the name 'mean'.

**Library**

- `install.packages('package')`; should be done as an administrator or superuser directly in R, not an editor. Once the package is installed, it can be loaded on the PC by any user in R or an editor (such as RStudio).
- `library(package = 'package')`; load a package.
- `library(help = package)`; help on a specific package.
	- `library(help = base)`; for example.
	- `library(help = utils)`; for example.
	- `library(help = datasets)`; for example.
	- `library(help = graphics)`; for example.
	- `library(help = grDevices)`; for example.
- `library(lib.loc = .Library)`; find which package (system packages, but not user packages).
- `find('function')`; find which package (system).
- `data()`; datasets available.
- `demo()`; available demos by package.
- `demo(graphics)`; give examples.
- `example(mean)`; give examples.

### Resources

- [nmz](http://search.r-project.org/nmz.html); search the content of the R functions, package vignette, and task views.
- [R-Project Mailing Lists](http://www.r-project.org/mail.html); mailing lists.
- [RSeek](http://www.rseek.org); search engine.
- [StackOverflow](http://stackoverflow.com/questions/tagged/r); forum.
- [Abcd'R](http://abcdr.guyader.pro); scripts.
- [CIRAD](http://http://forums.cirad.fr/logiciel-R/index.php); forum.
- [Developpez.net](http://www.developpez.net/forums/f1179/autres-langages/); forum.
- [ETHZ](http://https://stat.ethz.ch/mailman/listinfo/r-help); mailing list.
- [ETHZ Manual](https://stat.ethz.ch/R-manual/).
- [http://a](http://a); manuals and resources.
stat.ethz.ch/mailman/listinfo/r-annonce
- [ETHZ List Info](http://https://stat.ethz.ch/mailman/listinfo).
- [R Programming Wikibooks](http://https://en.wikibooks.org/wiki/R_Programming); wiki.
- [CRAN-R](https://cran.r-project.org/).
- [CRAN-R Doc](http://https://cran.r-project.org/other-docs.html); PDF and documentation.
- [CRAN-R Views](http://https://cran.r-project.org/web/views/); projects.
- [R Journal](http://https://journal.r-project.org/); publication.
- [Journal of Statistical Software](http://www.jstatsoft.org); publication.

## Chapter 5, Techniques for Plotting Graphics

### Graphic windows

Of course, these commands are automated in an editor (RStudio).

- `dev.new()`; graphic window on Windows.
- `windows()`; graphic window on Windows.
- `win.graph()`; graphic window on Windows.
- `X11()`; graphic window on Linux.
- Add parameters in the command:
	- `width = , height = `.
	- `pointsize = `.
	- `xpinch = , ypinch = `; pixels per inch.
	- `xpos = , ypos = `; position of the upper left corner, in pixel.
- `dev.set(num)`; activate window number 'num'; there can be several graphic windows.
- `dev.off(num)`; close a window.
- `graphics.off()`; close all windows.
- `dev.list()`; return window numbers.
- `dev.cur()`; return the current number, active window.
- `dev.print(png, file = , width = , height = )`; print.
- `savePlot(filename = , type = "png")`; save.
- `png(file=" ", width = , height = )`; save directly in .png.
- `jpeg()`.
- `bitmap()`.
- `postscript()`.
- `pdf()`.

```r
# examples

dev.new(width = 200, height = 200, xpos = 100, ypos = 100)
NULL

dev.list()
RStudioGD       png   windows 
        2         3         4 

dev.cur()
windows 
      4

hist(runif(100)) # create a graphic

# in the current working directory
dev.print(png, file = "mygraph.png", width = 480, height = 480)
savePlot(filename = "mygraph.png", type = "png")
pdf(file = "mygraph.pdf")

dev.off()
```

### Multiple windows

- `par(mfrow = c(3, 2))`; create 6 graphic boxes, 3 rows X 2 columns.

*REF: p.166-167*

```r
dev.new()
mat <- matrix(c(2, 3, 0, 1, 0, 0, 0, 4, 0, 0, 0, 5), 4, 3, byrow = TRUE)
layout(mat)


dev.new()
layout(mat, widths = c(1, 5, 14), heights = c(1,2,4,1))
layout.show(5)
```

*REF: p.168*

### Draw on a graphic

- `segments(x0 = 0, y0 = 0, x1 = 1, y1 = 1)`; draw lines on a plot.
- `lines(x = c(1,0), y = c(0,1))`.
- `abline(h = 0, v = 0)`; add a line to a plot.
- `abline(a = 1, b = 1)`.

### Random numbers

```r
x <- runif(12)
x
[1] 0.03344539 0.22711659 0.66696650 0.02840671 0.61067995 0.92957527
[7] 0.26962190 0.60013387 0.04831111 0.12603905 0.41913598 0.13142315

# ranking
i <- order(x)
i
[1]  4  1  9 10 12  2  7 11  8  5  3  6

# reorder
x <- x[i]
x
[1] 0.02840671 0.03344539 0.04831111 0.12603905 0.13142315 0.22711659
[7] 0.26962190 0.41913598 0.60013387 0.61067995 0.66696650 0.92957527
```

More on random number in Chapter 10, Random Variables, Laws, and Simulation.

**Examples**

- `example(polygon)`; see examples.
- `curve(x**3 - 3*x, from = -2, to = 2)`; trace a curve according to a function.
- `hist(rnorm(10000), prob = TRUE, breaks = 100)`.
- `plot(runif(7), type = "h", axes = F); box(lty = "1373")`
- `plot(1:10, runif(10), type = "l", col = "orangered")`

**Help**

- `colors()[grep("orange", colors())]`; list of tones.

### Colors

- `rgb(red = 26, green = 204, blue = 76, maxColorValue = 255)`; return the code.
- `rgb(red = 0.1, green = 0.8, blue = 0.3)`.
- `col2rgb("#1AVV4C")`; inversely.
- R generates 256³ colors, 15M.
- `rainbow(#)`; show 1, 8, 17 or more colors; R can generate 256³ colors (over 15M),
- `pie(rep(1,200), labels = "", col = rainbow(200), border = NA)`; show the colors.
- `plot(1:40, col = rainbow(n), pch = 19, cex = 2),     grid(col = "grey50")`; show.
- `RColorBrewer` package.

```r
library('RColorBrewer')
display.brewer.all()
```

### Images

- `caTools` package to manage images.
- `read.gif()`.
- `image()`.

### Write and add marks on a graphic

- `text(coord x, coord y, 'text')`; write.
- `demo(plotmath)`; see examples.
- `mtext('bas', side = 1)`; write 'bas' on the graphic box; 4 sides of the box (bottom, left, top, right).
- `locator()`; point with the mouse on a graphic to record coordinates; `esc` to show the coordinates.
plot(1,1); locate area to be annotated with `text()`.
- `text(locator(1), label="ici")`; add a label, one or several occurrences, by pointing the location with the mouse.
- `identify(occurrence, label)`; add one or more labels by pointing the location with the mouse. 

### Graphic parameters and graphic windows

<sub>margin, border, box, square, space, height, width, color, text, title, label, mark, font, police, character, language, size, length, size, tick, coordinate, x, y, log, scale, axis, axes, format, alignment, point</sub>

### Advanced graphic package

- `rgl`.
- `lattice`.
- `ggplot2`.

*REF: p.190-201*

## Chapter 6, Initiation to R Programming

```r
function (<parameters>) {
    <body>
}
```

```r
lance <- function (nom) {
  cat("Bonjour", nom, "!")
}

lance('allo')
Bonjour Alex !

bonjour <- function (nom="Pierre", langue="fr") {
  cat(switch(langue, fr="Bonjour", esp="Hola", ang="Hi"), nom, "!")
}

bonjour()
Bonjour Pierre !
bonjour(nom="Ben") # replace the default value.
Bonjour Ben !
bonjour(langue="ang")
Hi Pierre !
bonjour(lang = "ang") # partial call
Hi Pierre !
bonjour(l="ang")
Hi Pierre !
bonjour(l="a")
 Pierre !
```
    
*REF: p.218-219*

### Union

```r
"%union%" <- function (A,B) { union(A,B) }
A <- c(4,5,2,7)
B <- c(2,1,7,3)

A %union% B
[1] 4 5 2 7 1 3
```

### Class

```r
obj <- 1:10

class(obj)
[1] "integer"

class(obj) <- "TheClass"

class(obj)
[1] "TheClass"

inherits(obj, "TheClass")
[1] TRUE
```

### Methods

```r
x <- 1:10

print.default(x)
[1] 1 2 3 4 5 6 7 8 9 10
```

*REF: p.227-231*

### Combine and permute

- `combinat` package.
- `combn(5,3)`; combine 3 numbers from 1:5.
- `choose(200,3)`; choose 3 numbers from 1:200.
- `permn(n,m)`.

Very time consuming!

### More power, speed

- The core of R in programmed in C. Converting a R function into C is easy. C is faster.
- Call it through API C.
- With R graphic interface and C computation speed, it is the best of both world.
- Easier to set up on Linux or OS X (the OS has default compilers. Use the `Rcpp` package. 
- On Windows, there is a need for `Rtools`. 
- R can be compiled (byte compiler) with the `RevoScaleR` package (parallel computing).
-  `bigmemory`, `ff`, packages; split a matrix into sub-matrices, perform computation and combine the results (parallel computing). 
- Use a multi-core architecture with the `parallel` package.
- [HighPerformanceComputing list of packages](https://www.rdocumentation.org/taskviews/HighPerformanceComputing?).

**Try parallel computing with a Monte Carlo with the `parallel` package**

```r
myfunc <- function(M=1000) {
    decision <- 0
    for (i in 1:M) {
      x <- rnorm(100)
      if (shapiro.test(x)$p < 0.05) decision <- decision +1
    }
return(decision)
}

system.time({
    M <- 60000
    decision <- myfunc(M)
    print(decision/M)
})
```

- For a parallel execution.
- Start menu.
- Type devmgmt.msc
- Under Processors in Linux, type `top` in the terminal.
- Then, type `1` in R.
- Enter `detectCores()` from the `parallel` package.

```r
require("parallel")
system.time({
    nbcores <- 6 # less than detectCores() - 1
    M <- 60000
    cl <- makeCluster(nbcores, type = "PSOCK")
    out <- clusterCall(cl, myfunc, round(M/nbcores))
    stopCluster(cl)
    decision <- 0
    for (clus in 1:nbcores) {
        decision <- decision + out[[clus]]
    }
    print(decision/(round(M/nbcores)*nbcores))
})
```

- The process number (PID) of each computation node (core) in the cluster.

```r
require("parallel")
Sys.getpid()
cl <- makeCluster(4,type="PSOCK")
out <- clusterCall(cl, Sys.getpid))
```

### Involve the graphical card for more power

- Run computations with the graphical card, the GPU.
	- `gputools` package.

*REF: p.303-308*

## Chapter 7, Session Management

<sub>work, session, save, object, instruction, graphic, create, package</sub>

### Environment

- `globalenv()`; .GlobalEnv.
- `new.env()`; new environment with its own functions, variable, etc.
- `ls()`; list of objects in the environment.
- `objects()`; idem.
- `rm()`; remove one or more objects.
- `rm(list = ls())`; remove all.
- .RData; workspace file extension.
- file.RData; R file.
- `save.image("file.RData")`; save a R workspace to the current working directory. Several workspace can have their own objects.
- `load("file.RData")`; load a R workspace from the current working directory.
- `load(file.choose())`; open the current working directory.
- `getwd()`; get the current working directory.
- `setwd()`; set the current working directory.
- .Rhistory; history file extension.
- `history()`; consult the R log.
- `savehistory()`; save the log to a file.
- `loadhistory()`; load the log from a file.
- `search()`; list of attached packages.
- `searchpaths()`;  list of paths.
- `library()`; list of packages in memory.
- `require("packages")`, `library("packages")`; load a package.
- `attach(data)`; attach a dataset to .GlobalEnv..
- `detach(data)`.
- `ls(pos = 1)`, `ls()`; object list in database 1.
- `ls(pos = 2)`; object list in database 2.
- `ls(pos = match("package:datasets", search()))`; list datasets.
- `ls(data)`; list object related to the dataset.
- `fix(data)`; open a spreadsheet with the data.
- `sink(file = "sortie.txt")`; save a file on the current working directory.
- `sink()`; stop the recording.

### File manipulation

- Low-level interface to the computer's file system.
- Create a file.
- Execute the command.
- Check out the result in the text files themselves.
- `file.create("sorty.txt", showWarnings = TRUE)`.
- `file.exists("sorty.txt")`.
- `file.remove("sorty.txt")`.
- `file.rename("sorty.txt", "sorti.txt")`.
- `file.append("sorty.txt", "sorti2.txt")`.
- `file.copy("sorty.txt", "sorti2.txt")`.
- `file.symlink("sorty.txt", "sorti2.txt")`.
- `file.path("sorty.txt")`.
- `file.show()`.
- `list.files()`.
- `unlink()`.
- `basename()`.
- `path.expand()`.
- `list.files()`.
- `file.exists()`.
- `memory.size()`.
- `memory.limit()`.

*REF: p.320-330*

### Memory management

- The KSysGuard software analyzes memory usage in real time. It is a task manager and performance monitor for UNIX-like OS.

### Create a package

- How to: build a list of R instruction, load a dataset, create function, and output the results.
- Run a series of scripts in batch, run a script at a distance.
- Set the PATH to an executable Rgui.exe.
- Open a R script without opening R.
- Create a runthis script, apply `chmod` to use and execute it. The bash script launch R commands (it must begin with #!/in/bash).
- and much more.

*REF: p.332-338*

## Part 2, Mathematics and Basic Statistics
## Chapter 8, Basic Mathematics, matrix algebra, integration, optimization

### Math functions

*REF: p.342*

### Matrix calculation

```r
A <- matrix(c(2,3,5,4), nrow = 2, ncol = 2)
B <- matrix(c(1,2,2,7), nrow = 2, ncol = 2)

A
     [,1] [,2]
[1,]    2    5
[2,]    3    4

B
     [,1] [,2]
[1,]    1    2
[2,]    2    7

A + B
     [,1] [,2]
[1,]    3    7
[2,]    5   11

A - B
     [,1] [,2]
[1,]    1    3
[2,]    1   -3

# scalar multiplication
A * B
     [,1] [,2]
[1,]    2   10
[2,]    6   28

# matrix multiplication
A %*% B
     [,1] [,2]
[1,]   12   39
[2,]   11   34

# scalar multiplication
a <- 10
a * A
     [,1] [,2]
[1,]   20   50
[2,]   30   40

# transpose
t(A)
     [,1] [,2]
[1,]    2    3
[2,]    5    4

# inverse
solve(B)
           [,1]       [,2]
[1,]  2.3333333 -0.6666667
[2,] -0.6666667  0.3333333

solve(A) %*% B
           [,1]      [,2]
[1,]  0.8571429  3.857143
[2,] -0.1428571 -1.142857

t(A) * B
     [,1] [,2]
[1,]    2    6
[2,]   10   28

x <- seq(1,4)
y <- seq(4,7)

x
[1] 1 2 3 4
y
[1] 4 5 6 7

outer(x, y, FUN = "*")
     [,1] [,2] [,3] [,4]
[1,]    4    5    6    7
[2,]    8   10   12   14
[3,]   12   15   18   21
[4,]   16   20   24   28

# Kronecker
kronecker(A, B)
     [,1] [,2] [,3] [,4]
[1,]    2    4    5   10
[2,]    4   14   10   35
[3,]    3    6    4    8
[4,]    6   21    8   28

# triangle
lower.tri(A)
      [,1]  [,2]
[1,] FALSE FALSE
[2,]  TRUE FALSE

lower.tri(A, diag = TRUE)
     [,1]  [,2]
[1,] TRUE FALSE
[2,] TRUE  TRUE

lower.tri(A) * A
     [,1] [,2]
[1,]    0    0
[2,]    3    0

# diagonal
diag(A)
[1] 2 4

sum(diag(A))
[1] 6

# kappa
kappa(A, exact = TRUE)
[1] 7.582401

# matrix reduction
scale(A, scale = FALSE)
     [,1] [,2]
[1,] -0.5  0.5
[2,]  0.5 -0.5
attr(,"scaled:center")
[1] 2.5 4.5

scale(A, center = FALSE, scale = apply(A, 2, sd))
         [,1]     [,2]
[1,] 2.828427 7.071068
[2,] 4.242641 5.656854
attr(,"scaled:scale")
[1] 0.7071068 0.7071068

# eigenvalue
eigen(A)
$values
[1]  7 -1

$vectors
           [,1]       [,2]
[1,] -0.7071068 -0.8574929
[2,] -0.7071068  0.5144958

# singular value vector
svd(A)
$d
[1] 7.285383 0.960828

$u
           [,1]       [,2]
[1,] -0.7337222 -0.6794496
[2,] -0.6794496  0.7337222

$v
           [,1]       [,2]
[1,] -0.4812092  0.8766058
[2,] -0.8766058 -0.4812092

# Cholesky
chol2inv(A) 
          [,1]     [,2]
[1,]  0.640625 -0.15625
[2,] -0.156250  0.06250

# QR
qr(A)
$qr
           [,1]      [,2]
[1,] -3.6055513 -6.101702
[2,]  0.8320503 -1.941451

$rank
[1] 2

$qraux
[1] 1.554700 1.941451

$pivot
[1] 1 2

attr(,"class")
[1] "qr"
```

### Integral calculus

<sub>integration</sub>

```r
myf <- function(x) { exp(-x^2 / 2) / sqrt(2  *pi) }

integrate(myf, lower = -Inf, upper = Inf)$value
[1] 1
```

**Differential calculus**

<sub>derivative</sub>

```r
D(expression(sin(cos(x + y^2))), "x")
-(cos(cos(x + y^2)) * sin(x + y^2))

f <- deriv(<sub>x^2, "x", TRUE)
f(3)
[1] 9
attr(,"gradient")
     x
[1,] 6
```

- `numDeriv` package.
	- `grad()`; first-degree derivative.
	- `hessian()`; second-degree derivative.
	- and many more.

### Optimisation

<sub>linear, programming, constraints, min-max</sub>

```r
# compute a 1-variable min, max (y <sub>x)

optimize(function(x) { cos(x^2) }, lower = 0, upper = 2, maximum = FALSE)

source('</sub>/.active-rstudio-document', echo = TRUE)
```

- `nlm()`; compute a 2-variable min, max (z <sub>x + y).
- `nlminb()`; add contraints on x and y, add parameters `upper` and `lower`.
- `optim()`.
- `constrOptim()`.
- and many more.

**Unit root**

```r
unitroot(f=function(x) { cos(x^2) }, lower = 0,upper = 2,tol = 0.00001)$root

polyroot(x(3, -8, 1)) # for p(x) = 3 - 8x + x²
```

- `cummax()`.
- `cummin()`.
- `cumprod()`.
- `cumsum()`.
- and many more.

## Chapter 9, Descriptive Statistics

### Factor, levels, labels

- `factor(c())`
- `as.factor()`.
- `is.factor()`.
- `levels(var) <- c()`.
- `labels(var) <- c()`.
- `levels(var)`; output the levels.
- `labels(var)`; output the labels.
- `nlevels(var)`; output the number of levels.

```r
mydata <- factor(mydata,
	levels = c(1,2,3),
	labels = c("red", "blue", "green")
	) 

mydata <- ordered(mydata,
	levels = c(1,3, 5),
	labels = c("Low", "Medium", "High")
	) 
```

### Names

- `names(var) <- c()`; add names to a vector, data frame, list.
- `colnames(var) <- c()`; idem.
- `rownames()`; left-most column.
- `dimnames()`; add names to an array.

### Order

- `sort(vec, decreasing = TRUE)`.
- `rev(vec)`; inverse sorting.
- `order(vec)`; sort a vector with names or a list of strings.
- `ordered(vec)`.
- `as.ordered()`.
- `is.ordered()`.
- and many more.

Consult keywords 'arithmetics' and 'random numbers', where ordering data is commonly used.

### Convert (`as.`)

- `is.integer()`.
- `as.integer()`.
- `as.double()`.
- `is.double()`.
- `is.numeric()`.
- `as.numeric()`.
- `is.character()`.
- `as.character()`.
- and many more.

### Table, proportion table

<sub>tabular, comparison, 2-dimensional, 2, two, dimensions</sub>

- `table(var1, var2)`; 2-dimensional view; cross table.
- `as.table(var)`; convert.
- `cut()`; divide the range into intervals.
	- `table(cut(x, res$breaks, include.lowest = TRUE))`.
- `addmargins(x, FUN = sum, quiet = TRUE)`; add a column or row with sums, means, etc.
- `read.ftable()`; frequencies.
- `tablefreq <- mytable / sum(mytable)`.
	- `margin.table(tablefreq, 1)`; margin, right and bottom.
	- `tablefreq[ ,ncol()]`; extract the total.
	- `tablefreq[nrow(), ]`; extract the total.
- `prop.table(mytable, 1)`; percentage view; 1, row sum = 100%.
- `prop.table(mytable, 2)`; percentage view; 2, row sum = 100%.
- `which.max(table)`; find the max, min, mean, etc.

### Descriptive statistics

- `mean(x)`.
- `median(x)`.
- `quantile(x, probs = c(0.1, 0.9))`.
	- `probs = 1:10 / 10`.
- `max(x)`.
- `min(x)`.
- `diff(range(x))`.
- `IQR(x)`.
- `var.pop(x)`, `var(x)`.
- `sd.pop(x)`, `sd(x)`.
- `co.var(x)`.
- `mad(x)`; absolute deviation from the median.
	- `mean(abs(x - mean(x)))`.
- `skew(x)`.
- `kurt(x)`.
- `chisq.test()`.
- `round()`.
- `sum()`.
- `nrow()`.
- `ncol()`.
- `cor(var1, var2)`.
	- `method = "kendall", "spearman"`
- `rank()`.
- `rgrs` package .
	- `cramer.v()`.

*REF: p.378-379*

### Graphic descriptive statistics

- `plot()`.
- `dotchart(table(), col = c("", "", "" ,"") ,pch = , main = , lcolor = )`.
- `barplot()`.
- `barplot(var, col = , pareto = TRUE)`.
- `barplot(sort(table(var)), TRUE))`.
- `barplot(xlim = , width = , space = , names.arg = , legend = , density = , ylab = , lwd = )`.
- `pie()`.
- `points(barplot(), cumsum(var), type="l")`.
- `boxplot()`.
- `stem()`.
- `aplpack` package.
	- `stem.left()`
- `hist()`.
- `segments()`.

*REF: p.392-394, 397-398, 400-410*

## Chapter 10, Random Variables, Laws, and Simulation

- `x %% m`; modulo or modulus.

### Randomness

- `runif(1)`; generate a pseudo-random number between 0 and 1.
- `set.seed()`; 'shuffle the dice!'.
- `x <- function() { runif(1) }`; generate random numbers, following the uniform distribution.
- `rnorm(1)`; generate a random numbers, following the normal distribution.
- generate random numbers with a randomness function and `curve()`.

```r
x <- function() { rnorm(1, 7, 1) }
# avg = 7, sd = 1, 1 obs

curve(rnorm(x, 7, 1), xlim = c(-1,10))

plot(density(rnorm(1000, 7, 1)),xlim = c(-1, 10), main="Density Curve")
```

*REF: p.422-423*

```r
mean(runif(1))
[1] 0.6586903

mean(runif(10))
[1] 0.5196868

mean(runif(100))
[1] 0.5345603

mean(runif(1000))
[1] 0.5042301

mean(runif(10000))
[1] 0.5021896

# getting closer to 0.5 as the sample increases
```

- `ConvergenceConcepts` package to view the law of great numbers.

**Dice**

*REF: p.430-431*

**Bootstrap**

*REF: p.436*

**Laws**

*REF: p.437-447*

## Chapter 11, Confidence Intervals and Hypothesis Testing

### Confidence intervals

- Use t-values with at least 30 observation in the sample.
- With smaller samples (or larger too), use bootstraps to simulate populations from the `boot` package (`boot()` and `boot.ci()`).
- For proportion with large samples, use the `epitools` package and `binom.approx()`. With smaller samples, go with `binom.test()`.
- Variance confidence intervals; test for normality with `sigma2.test()`.
- For non-parametric sample, again, simulate populations with `boot()` and `boot.ci()`.
- For median, use `qbinom()`.
- For correlation, use `cor.test()`.

*REF: p.450-456*

### Test

- In a test, $\alpha$ is the signification threshold, $H_0$ is the tested hypothesis.
- Average tests;  compare the theoretical average to a reference value with the `t.test()`.
- Compare two theoretical averages with a `t.test()`.
- Compare pair samples with `t.test(paired=TRUE)`.
- Test variance(s) with ANOVA.
- Compare a theoretical variance with a reference value with `sigma2.test()`.
- Compare two theoretical variances with `var.test()`.
- Compare a theoretical proportion with a reference value with `prop.test()`.
- Compare two theoretical proportions with `prop.test(`).
- Test the theoretical correlation coefficient vs a reference value with `cor.test()` and `cor-.test()`.
- Test two theoretical correlation coefficients vs a reference value with `cor.test.2.sample()`.
- Test of independence or chi² with `chisq.test()`.
- Yates chi², adjustment chi² with `chisq.test()`.
- Fisher test with `fisher.test()`.
- Adequacy test or Shapiro-Walk test with `shapiro.test()`.
- Positional test or sign test or, median sign test with `prop.test()` and `binom.test()`.
- Median sign test for two independent samples with `chisq.test()` and `fisher.test()`.
- Sign test for two matching samples with `prop.test()` and `binom.test()`.
- Wilcoxon rank test or Mann-Whitney test for two independent samples with `wilcox.test()`.
- Wilcoxon test for two matching samples with `wilcox.test()`.

*REF: p.459-488*

## Chapter 12, Simple and Multiple Linear Regression

### Regression

- `lm(y <sub>x`.
- `lm(y <sub>0 + x)`; no intercept.
- `model <- lm(y <sub>x)`; run the regression.
- `summary(model)`; extract the results.
- `plot(y <sub>x)`; plot the results.
- `abline(model)`; add a line on the observations.
- `confint(model)`; confidence intervals; 95% or 2.5% on both sides.
- `coefficients(model)`; extract one or several coefficient.
- `model$coefficients`.
- `model$call`.
- `model$residuals`.
- `anova(model)`.
- `predict(model, data.frame(LWT = prediction), interval = "prediction")`
- and many more.

<sub>prediction, result, extraction, residual</sub>

*REF: p.498-499*

### Normality

<sub>histogram, test, residual, quantile-quantile, quantile, qq<sub> 

```r
par(mfrow=c(1,2))
hist(residuals(model), main = "Histogram")
```

- `qqnorm(resid(model), datax = TRUE)`; quantile-quantile.
- `qqplot()`.
- `qqline()`.
- `plot(model, 1:6, col.smooth = "red")`; 6 graphics.
- `jarque.bera.test(residuals(model))`; from the `tseries` package.
- `dwtest()`; Durbin-Watson test from the `lmtest` package.

*REF: p.502-503*

### Correlation

<sub>test, explanatory variable interaction, colinearity,  best subset</sub>

- `pairs(newdata, lower.panel = panel.smooth, upper.panel = add.cor)`.

*REF: p.506*

### Model improvement

- Variables selection.
- Best subset, leaps and bounds.
- Forward selection.
- Backward selection.
- Stepwise selection.
- Residual analysis.
- and many more.

*REF: p.511-535*

### Polynomial regression

*REF: p.535-540*

## Chapter 13, Elementary Variance Analysis

<sub>anova, repeated measure, between, within, inspection, hypothesis, comparison, factor, table, parameter, repeated</sub>

*REF: p.542-571*

## Appendix, Installing the R Software and Packages

<sub>installation, package</sub>

`install.packages('package')`.

- Be sure to launch RStudio as an administrator or a superuser to install a package in R (not in RStudio); the package is accessible to all users. Then load the package in R or RStudio.
- Then attach the package to the work session with `library('package')` or `require('packages')`.

## Answers to the Exercises

*REF:  p.625-674*
