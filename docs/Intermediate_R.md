-   [1, Conditionals and Control Flow](#conditionals-and-control-flow)
    -   [Equality (or not)](#equality-or-not)
    -   [`&` and `|`](#and)
    -   [The `if` statement (and more)](#the-if-statement-and-more)
-   [2, Loops](#loops)
    -   [Write a `while` loop](#write-a-while-loop)
    -   [Write a `for` loop](#write-a-for-loop)
    -   [Mix up loops with control flow](#mix-up-loops-with-control-flow)
-   [3, Functions](#functions)
    -   [Function documentation](#function-documentation)
    -   [Use a function](#use-a-function)
    -   [Functions inside functions](#functions-inside-functions)
    -   [Write your own function](#write-your-own-function)
    -   [R you functional?](#r-you-functional)
    -   [Load an R package](#load-an-r-package)
-   [4, The `apply` Family](#the-apply-family)
    -   [Use `lapply` (with a built-in R function)](#use-lapply-with-a-built-in-r-function)
    -   [Use `sapply`](#use-sapply)
    -   [Use `vapply`](#use-vapply)
    -   [Apply your knowledge. Or better yet: `sapply` it?](#apply-your-knowledge.-or-better-yet-sapply-it)
-   [5, Utilities](#utilities)
    -   [Mathematical utilities](#mathematical-utilities)
    -   [Find the error](#find-the-error)
    -   [Data utilities](#data-utilities)
    -   [Beat Gauss using R](#beat-gauss-using-r)
    -   [`grepl` & `grep` (and the likes)](#grepl-grep-and-the-likes)
    -   [Time is of the essence](#time-is-of-the-essence)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

1, Conditionals and Control Flow
--------------------------------

### Equality (or not)

``` r
# Comparison of logicals
TRUE == FALSE
```

    ## [1] FALSE

``` r
# Comparison of numerics
(-6 * 14) != (17 - 101)
```

    ## [1] FALSE

``` r
# Comparison of character strings
'useR' == 'user'
```

    ## [1] FALSE

``` r
# Compare a logical with a numeric
TRUE == 1
```

    ## [1] TRUE

**Greater and less than**

``` r
# Comparison of numerics
(-6*5 + 2) >= (-10 + 1)
```

    ## [1] FALSE

``` r
# Comparison of character strings
'raining' <= 'raining dogs'
```

    ## [1] TRUE

``` r
# Comparison of logicals
TRUE > FALSE
```

    ## [1] TRUE

**Compare vectors**

``` r
# The linkedin and facebook vectors
linkedin <- c(16, 9, 13, 5, 2, 17, 14)
facebook <- c(17, 7, 5, 16, 8, 13, 14)

# Popular days
linkedin > 15
```

    ## [1]  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE

``` r
# Quiet days
linkedin <= 5
```

    ## [1] FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE

``` r
# LinkedIn more popular than Facebook
linkedin > facebook
```

    ## [1] FALSE  TRUE  TRUE FALSE FALSE  TRUE FALSE

**Compare matrices**

``` r
views <- matrix(c(linkedin, facebook), nrow = 2, byrow = TRUE)

# When does views equal 13?
views == 13
```

    ##       [,1]  [,2]  [,3]  [,4]  [,5]  [,6]  [,7]
    ## [1,] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE
    ## [2,] FALSE FALSE FALSE FALSE FALSE  TRUE FALSE

``` r
# When is views less than or equal to 14?
views <= 14
```

    ##       [,1] [,2] [,3]  [,4] [,5]  [,6] [,7]
    ## [1,] FALSE TRUE TRUE  TRUE TRUE FALSE TRUE
    ## [2,] FALSE TRUE TRUE FALSE TRUE  TRUE TRUE

``` r
# How often does facebook equal or exceed linkedin times two?
sum(facebook >= linkedin * 2)
```

    ## [1] 2

### `&` and `|`

``` r
# The linkedin and last variable
linkedin <- c(16, 9, 13, 5, 2, 17, 14)
last <- tail(linkedin, 1)

# Is last under 5 or above 10?
last < 5 | last > 10
```

    ## [1] TRUE

``` r
# Is last between 15 (exclusive) and 20 (inclusive)?
last > 15 & last <= 20
```

    ## [1] FALSE

``` r
# Is last between 0 and 5 or between 10 and 15?
(last > 0 & last < 5) | (last > 10 & last < 15)
```

    ## [1] TRUE

**`&` and `|` (2)**

``` r
# linkedin exceeds 10 but facebook below 10
linkedin > 10 & facebook < 10
```

    ## [1] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE

``` r
# When were one or both visited at least 12 times?
linkedin >= 12 | facebook >= 12
```

    ## [1]  TRUE FALSE  TRUE  TRUE FALSE  TRUE  TRUE

``` r
# When is views between 11 (exclusive) and 14 (inclusive)?
views > 11 & views <= 14
```

    ##       [,1]  [,2]  [,3]  [,4]  [,5]  [,6] [,7]
    ## [1,] FALSE FALSE  TRUE FALSE FALSE FALSE TRUE
    ## [2,] FALSE FALSE FALSE FALSE FALSE  TRUE TRUE

**Blend it all together**

``` r
# Select the second column, named day2, from li_df: second
second <- li_df$day2

# Build a logical vector, TRUE if value in second is extreme: extremes
extremes <- (second > 25 | second < 5)

# Count the number of TRUEs in extremes
sum(extremes)
```

    ## [1] 16

### The `if` statement (and more)

``` r
# Variables related to your last day of recordings
medium <- 'LinkedIn'
num_views <- 14

# Examine the if statement for medium
if (medium == 'LinkedIn') {
  print('Showing LinkedIn information')
}
```

    ## [1] "Showing LinkedIn information"

``` r
# Write the if statement for num_views
if (num_views > 15) {
    print('You\'re popular!')
}
```

**Add an `else`**

``` r
# Variables related to your last day of recordings
medium <- 'LinkedIn'
num_views <- 14

# Control structure for medium
if (medium == 'LinkedIn') {
  print('Showing LinkedIn information')
} else {
    print('Unknown medium')
}
```

    ## [1] "Showing LinkedIn information"

``` r
# Control structure for num_views
if (num_views > 15) {
  print('You\'re popular!')
} else {
    print('Try to be more visible!')
}
```

    ## [1] "Try to be more visible!"

**Customize further: `else if`**

``` r
# Variables related to your last day of recordings
medium <- 'LinkedIn'
num_views <- 14

# Control structure for medium
if (medium == 'LinkedIn') {
  print('Showing LinkedIn information')
} else if (medium == 'Facebook') {
  print('Showing Facebook information')
} else {
  print('Unknown medium')
}
```

    ## [1] "Showing LinkedIn information"

``` r
# Control structure for num_views
if (num_views > 15) {
  print('You\'re popular!')
} else if (num_views > 10 | num_views <= 15) {
  print('Your number of views is average')
} else {
  print('Try to be more visible!')
}
```

    ## [1] "Your number of views is average"

**Take control!**

``` r
# Variables related to your last day of recordings
li <- 15
fb <- 9

# Code the control-flow construct
if (li >= 15 & fb >= 15) {
    sms <- 2*(li + fb)
} else if (li < 10 & fb < 10) {
    sms <- (li + fb)/2
} else {
    sms <- li + fb
}

# Print the resulting sms to the console
sms
```

    ## [1] 24

2, Loops
--------

### Write a `while` loop

``` r
# Initialize the speed variable
speed <- 64

# Code the while loop
while (speed > 30) {
  print('Slow down!')
  speed <- speed - 7
}
```

    ## [1] "Slow down!"
    ## [1] "Slow down!"
    ## [1] "Slow down!"
    ## [1] "Slow down!"
    ## [1] "Slow down!"

``` r
# Print out the speed variable
speed
```

    ## [1] 29

**Throw in more conditionals**

``` r
# Initialize the speed variable
speed <- 64

# Extend/adapt the while loop
while (speed > 30) {
  print(paste('Your speed is ', speed))
  if (speed > 48) {
    print('Slow down big time!')
    speed <- speed - 11
  } else {
        print('Slow down!')
        speed <- speed - 6
  }
}
```

    ## [1] "Your speed is  64"
    ## [1] "Slow down big time!"
    ## [1] "Your speed is  53"
    ## [1] "Slow down big time!"
    ## [1] "Your speed is  42"
    ## [1] "Slow down!"
    ## [1] "Your speed is  36"
    ## [1] "Slow down!"

**Stop the `while` loop: `break`**

``` r
# Initialize the speed variable
speed <- 88
while (speed > 30) {
  print(paste('Your speed is',speed))
    # Break the while loop when speed exceeds 80
  if (speed > 80) {
    break
  } else if (speed > 48) {
    print('Slow down big time!')
    speed <- speed - 11
  } else {
    print('Slow down!')
    speed <- speed - 6
  }
}
```

    ## [1] "Your speed is 88"

**Build a `while` loop from scratch**

`strsplit`; split up in a vector that contains separate letters.

``` r
# Initialize i
i <- 1

# Code the while loop
while (i <= 10) {
  print(i * 3)
  if ( (i * 3) %% 8 == 0) {
    break
  }
  i <- i + 1
}
```

    ## [1] 3
    ## [1] 6
    ## [1] 9
    ## [1] 12
    ## [1] 15
    ## [1] 18
    ## [1] 21
    ## [1] 24

### Write a `for` loop

**Loop over a vector**

``` r
# The linkedin vector
linkedin <- c(16, 9, 13, 5, 2, 17, 14)

# Loop version 1
for (lin in linkedin) {
    print(lin)
}
```

    ## [1] 16
    ## [1] 9
    ## [1] 13
    ## [1] 5
    ## [1] 2
    ## [1] 17
    ## [1] 14

``` r
# Loop version 2
for (i in 1:length(linkedin)) {
    print(linkedin[i])
}
```

    ## [1] 16
    ## [1] 9
    ## [1] 13
    ## [1] 5
    ## [1] 2
    ## [1] 17
    ## [1] 14

**Loop over a list**

`[[]]`; list of list.

``` r
# The nyc list is already specified
nyc <- list(pop = 8405837, 
            boroughs = c('Manhattan', 'Bronx', 'Brooklyn', 'Queens', 'Staten Island'), 
            capital = FALSE)

# Loop version 1
for (item in nyc) {
    print(item)
}
```

    ## [1] 8405837
    ## [1] "Manhattan"     "Bronx"         "Brooklyn"      "Queens"       
    ## [5] "Staten Island"
    ## [1] FALSE

``` r
# Loop version 2
for (i in 1:length(nyc)) {
    print(nyc[[i]])
}
```

    ## [1] 8405837
    ## [1] "Manhattan"     "Bronx"         "Brooklyn"      "Queens"       
    ## [5] "Staten Island"
    ## [1] FALSE

**Loop over a matrix**

``` r
# The tic-tac-toe matrix has already been defined for you
ttt <- matrix(c('O', NA, 'X', NA, 'O', NA, 'X', 'O', 'X'), nrow = 3, ncol = 3)

# define the double for loop
for (i in 1:nrow(ttt)) {
    for (j in 1:ncol(ttt)) {
    print(paste('On row', i,'and column', j,'the board contains ', ttt[i,j]))
    }
}
```

    ## [1] "On row 1 and column 1 the board contains  O"
    ## [1] "On row 1 and column 2 the board contains  NA"
    ## [1] "On row 1 and column 3 the board contains  X"
    ## [1] "On row 2 and column 1 the board contains  NA"
    ## [1] "On row 2 and column 2 the board contains  O"
    ## [1] "On row 2 and column 3 the board contains  O"
    ## [1] "On row 3 and column 1 the board contains  X"
    ## [1] "On row 3 and column 2 the board contains  NA"
    ## [1] "On row 3 and column 3 the board contains  X"

### Mix up loops with control flow

``` r
# The linkedin vector
linkedin <- c(16, 9, 13, 5, 2, 17, 14)

# Code the for loop with conditionals
for (i in 1:length(linkedin)) {
    if (linkedin[i] > 10) {
        print('You\'re popular!')
    } else {
        print('Be more visible!')
    }
    print(linkedin[i])
}
```

    ## [1] "You're popular!"
    ## [1] 16
    ## [1] "Be more visible!"
    ## [1] 9
    ## [1] "You're popular!"
    ## [1] 13
    ## [1] "Be more visible!"
    ## [1] 5
    ## [1] "Be more visible!"
    ## [1] 2
    ## [1] "You're popular!"
    ## [1] 17
    ## [1] "You're popular!"
    ## [1] 14

**Next, you break it**

``` r
# The linkedin vector
linkedin <- c(16, 9, 13, 5, 2, 17, 14)

# Extend the for loop
for (li in linkedin) {
  if (li > 10) {
    print('You\'re popular!')
  } else {
    print('Be more visible!')
  }
    # Add code to conditionally break iteration
  if (li > 16) {
    print('This is ridiculous, I\'m outta here!')
    break
  }
  # Add code to conditionally skip iteration
  if (li < 5) {
    print('This is too embarrassing!')
    next
    }
  print(li)
}
```

    ## [1] "You're popular!"
    ## [1] 16
    ## [1] "Be more visible!"
    ## [1] 9
    ## [1] "You're popular!"
    ## [1] 13
    ## [1] "Be more visible!"
    ## [1] 5
    ## [1] "Be more visible!"
    ## [1] "This is too embarrassing!"
    ## [1] "You're popular!"
    ## [1] "This is ridiculous, I'm outta here!"

**Build a for loop from scratch**

``` r
# Pre-defined variables
rquote <- 'R\'s internals are irrefutably intriguing'

chars <- strsplit(rquote, split = '')[[1]]
rcount <- 0

# Your solution here
for (i in 1:length(chars)) {
    if (chars[i] == 'u') {
    break
    }
    if (chars[i] == 'r' | chars[i] == 'R') {
        rcount <- rcount + 1
    }
}

# Print the resulting rcount variable to the console
print(rcount)
```

    ## [1] 5

3, Functions
------------

### Function documentation

``` r
# Consult the documentation on the mean() function
?mean

# Inspect the arguments of the mean() function
args(mean)
```

### Use a function

``` r
# The linkedin and facebook vectors
linkedin <- c(16, 9, 13, 5, 2, 17, 14)
facebook <- c(17, 7, 5, 16, 8, 13, 14)

# Calculate average number of views
avg_li <- mean(linkedin)
avg_fb <- mean(facebook)

# Inspect avg_li and avg_fb
print(avg_li)
```

    ## [1] 10.85714

``` r
print(avg_fb)
```

    ## [1] 11.42857

``` r
avg_li
```

    ## [1] 10.85714

``` r
# Calculate the mean of linkedin minus facebook
print(mean(linkedin - facebook))
```

    ## [1] -0.5714286

**Use a function (2)**

``` r
# The linkedin and facebook vectors
linkedin <- c(16, 9, 13, 5, 2, 17, 14)
facebook <- c(17, 7, 5, 16, 8, 13, 14)

# Calculate the mean of the sum
avg_sum <- mean(linkedin + facebook)

# Calculate the trimmed mean of the sum
avg_sum_trimmed <- mean((linkedin + facebook), trim = 0.2)

# Inspect both new variables
avg_sum
```

    ## [1] 22.28571

``` r
avg_sum_trimmed
```

    ## [1] 22.6

**Use a function (3)**

``` r
# The linkedin and facebook vectors
linkedin <- c(16, 9, 13, 5, NA, 17, 14)
facebook <- c(17, NA, 5, 16, 8, 13, 14)

# Basic average of linkedin
print(mean(linkedin))
```

    ## [1] NA

``` r
# Advanced average of facebook
print(mean(facebook, na.rm = TRUE))
```

    ## [1] 12.16667

### Functions inside functions

``` r
# The linkedin and facebook vectors
linkedin <- c(16, 9, 13, 5, NA, 17, 14)
facebook <- c(17, NA, 5, 16, 8, 13, 14)

# Calculate the mean absolute deviation
mean((abs(linkedin - facebook)), na.rm = TRUE)
```

    ## [1] 4.8

### Write your own function

``` r
# Create a function pow_two()
pow_two <- function(arg1) {
    arg1^2
}

# Use the function 
pow_two(12)
```

    ## [1] 144

``` r
# Create a function sum_abs()
sum_abs <- function(arg2,arg3) {
    abs(arg2) + abs(arg3)
}

# Use the function
sum_abs(-2,3)
```

    ## [1] 5

**Write your own function (2)**

``` r
# Define the function hello()
hello <- function() {
    print('Hi there!')
    return(TRUE)
}

# Call the function hello()
hello()
```

    ## [1] "Hi there!"

    ## [1] TRUE

``` r
# Define the function my_filter()
my_filter <- function(arg1) {
    if (arg1 > 0) {
        return(arg1)
    } else {
        return(NULL)
    }
}

# Call the function my_filter() twice
my_filter(5)
```

    ## [1] 5

``` r
my_filter(-5)
```

    ## NULL

**Write your own function (3)**

Variables inside a function are not in the Global Environment.

``` r
# Extend the pow_two() function
pow_two <- function(x, print_info = TRUE) {
  y <- x ^ 2
  if (print_info) {
    print(paste(x, 'to the power two equals', y))
  }
  return(y)
}

#pow_two(2)
pow_two(2, FALSE)
```

    ## [1] 4

### R you functional?

``` r
# The linkedin and facebook vectors
linkedin <- c(16, 9, 13, 5, NA, 17, 14)
facebook <- c(17, 7, 5, 16, 8, 13, 14)

# Define the interpret function
interpret <- function(arg) {
    if (arg > 15) {
        print('You\'re popular!')
        return(arg)
    } else {
        print('Try to be more visible!')
        return(0)
    }
}

interpret(linkedin[1])
```

    ## [1] "You're popular!"

    ## [1] 16

``` r
interpret(facebook[2])
```

    ## [1] "Try to be more visible!"

    ## [1] 0

**R you functional? (2)**

``` r
# The linkedin and facebook vectors
linkedin <- c(16, 9, 13, 5, 2, 17, 14)
facebook <- c(17, 7, 5, 16, 8, 13, 14)

# The interpret() can be used inside interpret_all()
interpret <- function(num_views){
  if (num_views > 15) {
    print('You\'re popular!')
    return(num_views)
  } else {
    print('Try to be more visible!')
    return(0)
  }
}

# Define the interpret_all() function
interpret_all <- function(data, logi = TRUE){
  yy <- 0
  for (i in data) {
    yy <- yy + interpret(i)
  }
  if (logi) {
    return(yy)
  } else {
    return(NULL)
  }
}

# Call the interpret_all() function on both linkedin and facebook
interpret_all(linkedin)
```

    ## [1] "You're popular!"
    ## [1] "Try to be more visible!"
    ## [1] "Try to be more visible!"
    ## [1] "Try to be more visible!"
    ## [1] "Try to be more visible!"
    ## [1] "You're popular!"
    ## [1] "Try to be more visible!"

    ## [1] 33

``` r
interpret_all(facebook)
```

    ## [1] "You're popular!"
    ## [1] "Try to be more visible!"
    ## [1] "Try to be more visible!"
    ## [1] "You're popular!"
    ## [1] "Try to be more visible!"
    ## [1] "Try to be more visible!"
    ## [1] "Try to be more visible!"

    ## [1] 33

### Load an R package

``` r
# The mtcars vectors have already been prepared for you
wt <- mtcars$wt
hp <- mtcars$hp

# Request the currently attached packages
search()
```

    ##  [1] ".GlobalEnv"            "package:XLConnect"    
    ##  [3] "package:XLConnectJars" "package:stats"        
    ##  [5] "package:graphics"      "package:grDevices"    
    ##  [7] "package:utils"         "package:datasets"     
    ##  [9] "package:methods"       "Autoloads"            
    ## [11] "package:base"

``` r
# Try the qplot() function with wt and hp
plot(wt,hp)

# Load the ggplot2 package
library('ggplot2')
```

![](Intermediate_R_files/figure-markdown_strict+backtick_code_blocks+autolink_bare_uris/unnamed-chunk-33-1.png)

``` r
# or
require('ggplot2')

# Retry the qplot() function
qplot(wt,hp)
```

![](Intermediate_R_files/figure-markdown_strict+backtick_code_blocks+autolink_bare_uris/unnamed-chunk-33-2.png)

``` r
# Check out the currently attached packages again
search()
```

    ##  [1] ".GlobalEnv"            "package:ggplot2"      
    ##  [3] "package:XLConnect"     "package:XLConnectJars"
    ##  [5] "package:stats"         "package:graphics"     
    ##  [7] "package:grDevices"     "package:utils"        
    ##  [9] "package:datasets"      "package:methods"      
    ## [11] "Autoloads"             "package:base"

4, The `apply` Family
---------------------

### Use `lapply` (with a built-in R function)

``` r
# The vector pioneers
pioneers <- c('GAUSS:1777', 'BAYES:1702', 'PASCAL:1623', 'PEARSON:1857')

# Split names from birth year: split_math
split_math <- strsplit(pioneers, ':')

# Convert to lowercase strings: split_low
split_low <- lapply(split_math,tolower)

# Take a look at the structure of split_low
str(split_low)
```

    ## List of 4
    ##  $ : chr [1:2] "gauss" "1777"
    ##  $ : chr [1:2] "bayes" "1702"
    ##  $ : chr [1:2] "pascal" "1623"
    ##  $ : chr [1:2] "pearson" "1857"

**Use `lapply` with your own function**

``` r
# Code from previous exercise
pioneers <- c('GAUSS:1777', 'BAYES:1702', 'PASCAL:1623', 'PEARSON:1857')

split <- strsplit(pioneers, split = ':')
split_low <- lapply(split, tolower)

# Write function select_first()
select_first <- function(x) {
    return(x[1])
}

# Apply select_first() over split_low: names
names <- lapply(split_low, select_first)
print(names)
```

    ## [[1]]
    ## [1] "gauss"
    ## 
    ## [[2]]
    ## [1] "bayes"
    ## 
    ## [[3]]
    ## [1] "pascal"
    ## 
    ## [[4]]
    ## [1] "pearson"

``` r
# Write function select_second()
select_second <- function(x) {
    return(x[2])
}

# Apply select_second() over split_low: years
years <- lapply(split_low, select_second)
print(years)
```

    ## [[1]]
    ## [1] "1777"
    ## 
    ## [[2]]
    ## [1] "1702"
    ## 
    ## [[3]]
    ## [1] "1623"
    ## 
    ## [[4]]
    ## [1] "1857"

**`lapply` and anonymous functions**

Anonymous function == lambda function.

``` r
# Definition of split_low
pioneers <- c('GAUSS:1777', 'BAYES:1702', 'PASCAL:1623', 'PEARSON:1857')
split <- strsplit(pioneers, split = ':')
split_low <- lapply(split, tolower)

#select_first <- function(x) {
#  x[1]
#}

names <- lapply(split_low, function(x) { x[1] })

#select_second <- function(x) {
#  x[2]
#}

years <- lapply(split_low, function(x) { x[2] })
```

**Use `lapply` with additional arguments**

``` r
# Definition of split_low
pioneers <- c('GAUSS:1777', 'BAYES:1702', 'PASCAL:1623', 'PEARSON:1857')

split <- strsplit(pioneers, split = ':')
split_low <- lapply(split, tolower)

# Replace the select_*() functions by a single function: select_el
select_el <- function(x, i) { 
  x[i] 
}

#select_second <- function(x) { 
#  x[2] 
#}

# Call the select_el() function twice on split_low: names and years
names <- lapply(split_low, select_el, i=1)
years <- lapply(split_low, select_el, 2)
```

### Use `sapply`

``` r
temp1 <- c(3, 7, 9, 6, -1)
temp2 <- c(6, 9, 12, 13, 5)
temp3 <- c(4, 8, 3, -1, -3)
temp4 <- c(1, 4, 7, 2, -2)
temp5 <- c(5, 7, 9, 4, 2)
temp6 <- c(-3, 5, 8, 9, 4)
temp7 <- c(3, 6, 9, 4, 1)

temp <- list(temp1, temp2, temp3, temp4, temp5, temp6, temp7)

# Use lapply() to find each day's minimum temperature
lapply(temp, min)
```

    ## [[1]]
    ## [1] -1
    ## 
    ## [[2]]
    ## [1] 5
    ## 
    ## [[3]]
    ## [1] -3
    ## 
    ## [[4]]
    ## [1] -2
    ## 
    ## [[5]]
    ## [1] 2
    ## 
    ## [[6]]
    ## [1] -3
    ## 
    ## [[7]]
    ## [1] 1

``` r
# Use sapply() to find each day's minimum temperature
sapply(temp, min)
```

    ## [1] -1  5 -3 -2  2 -3  1

``` r
# Use lapply() to find each day's maximum temperature
lapply(temp, max)
```

    ## [[1]]
    ## [1] 9
    ## 
    ## [[2]]
    ## [1] 13
    ## 
    ## [[3]]
    ## [1] 8
    ## 
    ## [[4]]
    ## [1] 7
    ## 
    ## [[5]]
    ## [1] 9
    ## 
    ## [[6]]
    ## [1] 9
    ## 
    ## [[7]]
    ## [1] 9

``` r
# Use sapply() to find each day's maximum temperature
sapply(temp, max)
```

    ## [1]  9 13  8  7  9  9  9

**`sapply` with your own function**

``` r
# temp is already defined in the workspace

# Define a function calculates the average of the min and max of a vector: extremes_avg
extremes_avg <- function(x) {
    return((min(x) + max(x))/2)
}

# Apply extremes_avg() over temp using sapply()
sapply(temp, extremes_avg)
```

    ## [1] 4.0 9.0 2.5 2.5 5.5 3.0 5.0

``` r
# Apply extremes_avg() over temp using lapply()
lapply(temp, extremes_avg)
```

    ## [[1]]
    ## [1] 4
    ## 
    ## [[2]]
    ## [1] 9
    ## 
    ## [[3]]
    ## [1] 2.5
    ## 
    ## [[4]]
    ## [1] 2.5
    ## 
    ## [[5]]
    ## [1] 5.5
    ## 
    ## [[6]]
    ## [1] 3
    ## 
    ## [[7]]
    ## [1] 5

**`sapply` with function returning vector**

``` r
# temp is already available in the workspace

# Create a function that returns min and max of a vector: extremes
extremes <- function(x) {
    c(min(x), max(x))
}

# Apply extremes() over temp with sapply()
sapply(temp, extremes)
```

    ##      [,1] [,2] [,3] [,4] [,5] [,6] [,7]
    ## [1,]   -1    5   -3   -2    2   -3    1
    ## [2,]    9   13    8    7    9    9    9

``` r
# Apply extremes() over temp with lapply()
lapply(temp, extremes)
```

    ## [[1]]
    ## [1] -1  9
    ## 
    ## [[2]]
    ## [1]  5 13
    ## 
    ## [[3]]
    ## [1] -3  8
    ## 
    ## [[4]]
    ## [1] -2  7
    ## 
    ## [[5]]
    ## [1] 2 9
    ## 
    ## [[6]]
    ## [1] -3  9
    ## 
    ## [[7]]
    ## [1] 1 9

**`sapply` can't simplify, now what?**

``` r
# temp is already prepared for you in the workspace

# Create a function that returns all values below zero: below_zero
below_zero <- function(x) {
    x[x<0]
}

#below_zero(temp) alone won't work!!!

# Apply below_zero over temp using sapply(): freezing_s
freezing_s <- sapply(temp, below_zero)

# Apply below_zero over temp using lapply(): freezing_l
freezing_l <- lapply(temp, below_zero)

# Compare freezing_s to freezing_l using identical()
identical(freezing_s, freezing_l)
```

    ## [1] TRUE

**`sapply` with functions that return NULL**

``` r
# temp is already available in the workspace

# Write a function that 'cat()s' out the average temperatures: print_info
print_info <- function(x) {
    cat('The average temperature is', mean(x), '\n')
}

# Apply print_info() over temp using lapply()
lapply(temp, print_info)
```

    ## The average temperature is 4.8 
    ## The average temperature is 9 
    ## The average temperature is 2.2 
    ## The average temperature is 2.4 
    ## The average temperature is 5.4 
    ## The average temperature is 4.6 
    ## The average temperature is 4.6

    ## [[1]]
    ## NULL
    ## 
    ## [[2]]
    ## NULL
    ## 
    ## [[3]]
    ## NULL
    ## 
    ## [[4]]
    ## NULL
    ## 
    ## [[5]]
    ## NULL
    ## 
    ## [[6]]
    ## NULL
    ## 
    ## [[7]]
    ## NULL

``` r
# Apply print_info() over temp using sapply()
sapply(temp, print_info)
```

    ## The average temperature is 4.8 
    ## The average temperature is 9 
    ## The average temperature is 2.2 
    ## The average temperature is 2.4 
    ## The average temperature is 5.4 
    ## The average temperature is 4.6 
    ## The average temperature is 4.6

    ## [[1]]
    ## NULL
    ## 
    ## [[2]]
    ## NULL
    ## 
    ## [[3]]
    ## NULL
    ## 
    ## [[4]]
    ## NULL
    ## 
    ## [[5]]
    ## NULL
    ## 
    ## [[6]]
    ## NULL
    ## 
    ## [[7]]
    ## NULL

### Use `vapply`

``` r
# temp is already available in the workspace

# Code the basics() function
basics <- function(x) {
    c(minimum = min(x), average = mean(x), maximum = max(x))
}

# Apply basics() over temp using vapply()
vapply(temp, basics, numeric(3))
```

    ##         [,1] [,2] [,3] [,4] [,5] [,6] [,7]
    ## minimum -1.0    5 -3.0 -2.0  2.0 -3.0  1.0
    ## average  4.8    9  2.2  2.4  5.4  4.6  4.6
    ## maximum  9.0   13  8.0  7.0  9.0  9.0  9.0

**Use `vapply` (2)**

``` r
# temp is already available in the workspace

# Definition of the basics() function
basics <- function(x) {
  c(min = min(x), mean = mean(x), median = median(x), max = max(x))
}

# Fix the error:
vapply(temp, basics, numeric(4))
```

    ##        [,1] [,2] [,3] [,4] [,5] [,6] [,7]
    ## min    -1.0    5 -3.0 -2.0  2.0 -3.0  1.0
    ## mean    4.8    9  2.2  2.4  5.4  4.6  4.6
    ## median  6.0    9  3.0  2.0  5.0  5.0  4.0
    ## max     9.0   13  8.0  7.0  9.0  9.0  9.0

**From `sapply` to `vapply`**

``` r
# temp is already defined in the workspace

# Convert to vapply() expression
vapply(temp, max, numeric(1))
```

    ## [1]  9 13  8  7  9  9  9

``` r
# Convert to vapply() expression
vapply(temp, function(x, y) { mean(x) > y }, y = 5, logical(1))
```

    ## [1] FALSE  TRUE FALSE FALSE  TRUE FALSE FALSE

``` r
# Definition of get_info (don't change)
get_info <- function(x, y) { 
  if (mean(x) > y) {
    return('Not too cold!')
  } else {
    return('Pretty cold!')
  }
}

# Convert to vapply() expression
vapply(temp, get_info, y = 5, character(1))
```

    ## [1] "Pretty cold!"  "Not too cold!" "Pretty cold!"  "Pretty cold!" 
    ## [5] "Not too cold!" "Pretty cold!"  "Pretty cold!"

### Apply your knowledge. Or better yet: `sapply` it?

``` r
# work_todos and fun_todos have already been defined
work_todos <- c('Schedule call with team', 
                'Fix error in Recommendation System', 
                'Respond to Marc from IT')

fun_todos <- c('Sleep', 'Make arrangements for summer trip')

# Create a list: todos
todos <- list(work_todos, fun_todos)
todos
```

    ## [[1]]
    ## [1] "Schedule call with team"           
    ## [2] "Fix error in Recommendation System"
    ## [3] "Respond to Marc from IT"           
    ## 
    ## [[2]]
    ## [1] "Sleep"                             "Make arrangements for summer trip"

``` r
# Sort the vectors inside todos alphabetically
lapply(todos, sort)
```

    ## [[1]]
    ## [1] "Fix error in Recommendation System"
    ## [2] "Respond to Marc from IT"           
    ## [3] "Schedule call with team"           
    ## 
    ## [[2]]
    ## [1] "Make arrangements for summer trip" "Sleep"

5, Utilities
------------

### Mathematical utilities

-   `abs`; calculate the absolute value.
-   `sum`; calculate the sum of all the values in a data structure.
-   `mean`; calculate the arithmetic mean.
-   `round`; round the values to 0 decimal places by default. Try out
    `?round` in the console for variations of `round` and ways to change
    the number of digits to round to.

<!-- -->

``` r
# The errors vector
errors <- c(1.9, -2.6, 4.0, -9.5, -3.4, 7.3)

# Sum of absolute rounded values of errors
sum(abs(round(errors)))
```

    ## [1] 29

### Find the error

``` r
# Vectors
vec1 <- c(1.5, 2.5, 8.4, 3.7, 6.3)
vec2 <- rev(vec1)

# Fix the error
mean(abs(append(vec1, vec2)))
```

    ## [1] 4.48

### Data utilities

-   `seq`; generate sequences, by specifying the from, to and
    by arguments.
-   `rep`; replicate elements of vectors and lists.
-   `sort`; sort a vector in ascending order. Works on numerics, but
    also on character strings and logicals.
-   `rev`; reverse the elements in a data structures for which reversal
    is defined.
-   `str`; display the structure of any R object. append; Merge vectors
    or lists.
-   `is.*`; check for the class of an R object.
-   `as.*`; convert an R object from one class to another.
-   `unlist`; flatten (possibly embedded) lists to produce a vector.

<!-- -->

``` r
# The linkedin and facebook vectors
linkedin <- list(16, 9, 13, 5, 2, 17, 14)
facebook <- list(17, 7, 5, 16, 8, 13, 14)

# Convert linkedin and facebook to a vector: li_vec and fb_vec
li_vec <- unlist(as.vector(linkedin))
fb_vec <- unlist(as.vector(facebook))

# Append fb_vec to li_vec: social_vec
social_vec <- append(li_vec, fb_vec)

# Sort social_vec
sort(social_vec, decreasing = TRUE)
```

    ##  [1] 17 17 16 16 14 14 13 13  9  8  7  5  5  2

**Find the error (2)**

``` r
# Fix me
round(sum(unlist(list(1.1, 3, 5))))
```

    ## [1] 9

``` r
# Fix me
rep(seq(1, 7, by = 2), times = 7)
```

    ##  [1] 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7

``` r
print(rep(seq(1, 7, by = 2), times = 7))
```

    ##  [1] 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7 1 3 5 7

### Beat Gauss using R

``` r
# Create first sequence: seq1
seq1 <- seq(1,500, by = 3)
print(seq1)
```

    ##   [1]   1   4   7  10  13  16  19  22  25  28  31  34  37  40  43  46  49
    ##  [18]  52  55  58  61  64  67  70  73  76  79  82  85  88  91  94  97 100
    ##  [35] 103 106 109 112 115 118 121 124 127 130 133 136 139 142 145 148 151
    ##  [52] 154 157 160 163 166 169 172 175 178 181 184 187 190 193 196 199 202
    ##  [69] 205 208 211 214 217 220 223 226 229 232 235 238 241 244 247 250 253
    ##  [86] 256 259 262 265 268 271 274 277 280 283 286 289 292 295 298 301 304
    ## [103] 307 310 313 316 319 322 325 328 331 334 337 340 343 346 349 352 355
    ## [120] 358 361 364 367 370 373 376 379 382 385 388 391 394 397 400 403 406
    ## [137] 409 412 415 418 421 424 427 430 433 436 439 442 445 448 451 454 457
    ## [154] 460 463 466 469 472 475 478 481 484 487 490 493 496 499

``` r
# Create second sequence: seq2
seq2 <- seq(1200, 900, by = -7)
print(seq2)
```

    ##  [1] 1200 1193 1186 1179 1172 1165 1158 1151 1144 1137 1130 1123 1116 1109
    ## [15] 1102 1095 1088 1081 1074 1067 1060 1053 1046 1039 1032 1025 1018 1011
    ## [29] 1004  997  990  983  976  969  962  955  948  941  934  927  920  913
    ## [43]  906

``` r
# Calculate total sum of the sequences
print(sum(append(seq1, seq2)))
```

    ## [1] 87029

### `grepl` & `grep` (and the likes)

-   `grepl`; return TRUE when a pattern is found in the corresponding
    character string.
-   `grep`; return a vector of indices of the character strings that
    contains the pattern.

<!-- -->

``` r
# The emails vector has
emails <- c('john.doe@ivyleague.edu', 'education@world.gov', 'dalai.lama@peace.org', 
            'invalid.edu', 'quant@bigdatacollege.edu', 'cookie.monster@sesame.tv')

# Use grepl() to match for 'edu'
print(grepl(pattern = 'edu', x = emails))
```

    ## [1]  TRUE  TRUE FALSE  TRUE  TRUE FALSE

``` r
# Use grep() to match for 'edu', save result to hits
hits <- grep(pattern = 'edu', x = emails)
hits
```

    ## [1] 1 2 4 5

``` r
# Subset emails using hits
print(emails[hits])
```

    ## [1] "john.doe@ivyleague.edu"   "education@world.gov"     
    ## [3] "invalid.edu"              "quant@bigdatacollege.edu"

**`grepl` & `grep` (2)**

Consult a regex character chart for more.

``` r
# The emails vector
emails <- c('john.doe@ivyleague.edu', 'education@world.gov', 'dalai.lama@peace.org', 
            'invalid.edu', 'quant@bigdatacollege.edu', 'cookie.monster@sesame.tv')

# Use grep() to match for .edu addresses more robustly
print(grep(pattern = '@.*\\.edu$',x = emails))
```

    ## [1] 1 5

``` r
# Use grepl() to match for .edu addresses more robustly, save result to hits
hits <- grepl(pattern = '@.*\\.edu$',x = emails)
hits
```

    ## [1]  TRUE FALSE FALSE FALSE  TRUE FALSE

``` r
# Subset emails using hits
print(emails[hits])
```

    ## [1] "john.doe@ivyleague.edu"   "quant@bigdatacollege.edu"

**`sub` & `gsub`**

``` r
# The emails vector
emails <- c('john.doe@ivyleague.edu', 'education@world.gov', 'dalai.lama@peace.org', 
            'invalid.edu', 'quant@bigdatacollege.edu', 'cookie.monster@sesame.tv')

# Use sub() to convert the email domains to datacamp.edu (attempt 1)
print(sub(pattern = '@.*\\.edu$', replacement = 'datacamp.edu', x = emails))
```

    ## [1] "john.doedatacamp.edu"     "education@world.gov"     
    ## [3] "dalai.lama@peace.org"     "invalid.edu"             
    ## [5] "quantdatacamp.edu"        "cookie.monster@sesame.tv"

``` r
# Use sub() to convert the email domains to datacamp.edu (attempt 2)
print(sub(pattern = '@.*\\.edu$', replacement = '@datacamp.edu', x = emails))
```

    ## [1] "john.doe@datacamp.edu"    "education@world.gov"     
    ## [3] "dalai.lama@peace.org"     "invalid.edu"             
    ## [5] "quant@datacamp.edu"       "cookie.monster@sesame.tv"

### Time is of the essence

**Right here, right now**

``` r
# Get the current date: today
today <- Sys.Date()
today
```

    ## [1] "2017-04-14"

``` r
# See what today looks like under the hood
print(unclass(today))
```

    ## [1] 17270

``` r
# Get the current time: now
now <- Sys.time()
now
```

    ## [1] "2017-04-14 08:29:36 EDT"

``` r
# See what now looks like under the hood
print(unclass(now))
```

    ## [1] 1492172976

**Create and format dates**

<table>
<thead>
<tr class="header">
<th align="left">Symbol</th>
<th align="left">Meaning</th>
<th align="left">Example</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">%d</td>
<td align="left">day as a number (0-31)</td>
<td align="left">31-janv</td>
</tr>
<tr class="even">
<td align="left">%a</td>
<td align="left">abbreviated weekday</td>
<td align="left">Mon</td>
</tr>
<tr class="odd">
<td align="left">%A</td>
<td align="left">unabbreviated weekday</td>
<td align="left">Monday</td>
</tr>
<tr class="even">
<td align="left">%m</td>
<td align="left">month (00-12)</td>
<td align="left">00-12</td>
</tr>
<tr class="odd">
<td align="left">%b</td>
<td align="left">abbreviated month</td>
<td align="left">Jan</td>
</tr>
<tr class="even">
<td align="left">%B</td>
<td align="left">unabbreviated month</td>
<td align="left">January</td>
</tr>
<tr class="odd">
<td align="left">%y</td>
<td align="left">2-digit year</td>
<td align="left">07</td>
</tr>
<tr class="even">
<td align="left">%Y</td>
<td align="left">4-digit year</td>
<td align="left">2007</td>
</tr>
<tr class="odd">
<td align="left">%H</td>
<td align="left">hours as a decimal number</td>
<td align="left">23</td>
</tr>
<tr class="even">
<td align="left">%M</td>
<td align="left">minutes as a decimal number</td>
<td align="left">10</td>
</tr>
<tr class="odd">
<td align="left">%S</td>
<td align="left">seconds as a decimal number</td>
<td align="left">53</td>
</tr>
<tr class="even">
<td align="left">%T</td>
<td align="left">shorthand notation for the typical format %H:%M:%S</td>
<td align="left">23:10:53</td>
</tr>
</tbody>
</table>

Find out more with `?strptime`.

R offer default functions for dealing with time and dates. There are
better packages: `date` and `lubridate`.

`lubridate` enhances time-series packages such as `zoo` and `xts`, and
works well with `dplyr` for data wrangling.

``` r
library(date)

# Definition of character strings representing dates
str1 <- "May 23, 96"
str2 <- "2012-3-15"
str3 <- "30/January/2006"

# Convert the strings to dates: date1, date2, date3
date1 <- as.date(str1, order = "mdy")
date1
```

    ## [1] 23May96

``` r
date1 <- as.POSIXct(date1, format = "%d %m %y")
date1
```

    ## [1] "1996-05-22 20:00:00 EDT"

``` r
date2 <- as.date(str2, order = "ymd")
date2
```

    ## [1] 15Mar2012

``` r
date2 <- as.POSIXct(date2, format = "%d %m %y")
date2
```

    ## [1] "2012-03-14 20:00:00 EDT"

``` r
date3 <- as.date(str3, order = "dmy")
date3
```

    ## [1] 30Jan2006

``` r
date3 <- as.POSIXct(date3, format = "%d %m %y")
date3
```

    ## [1] "2006-01-29 19:00:00 EST"

``` r
# Convert dates to formatted strings
format(date1, "%A")
```

    ## [1] "mercredi"

``` r
format(date2, "%d")
```

    ## [1] "14"

``` r
format(date3, "%b %Y")
```

    ## [1] "janv. 2006"

``` r
# convert dates to character data
strDate2 <- as.character(date2)
strDate2
```

    ## [1] "2012-03-14 20:00:00"

**Create and format times**

``` r
# Definition of character strings representing times
str1 <- "2012-3-12 14:23:08"

# Convert the strings to POSIXct objects: time1, time2
time1 <- as.POSIXct(str2, format = "%Y-%m-%d %H:%M:%S")

# Convert times to formatted strings

# Definition of character strings representing dates
format(time1, "%M")
```

    ## [1] NA

``` r
format(time1, "%I:%M %p")
```

    ## [1] NA

**Calculations with dates**

``` r
# day1, day2, day3, day4 and day5
day1 <- as.Date("2016-11-21")
day2 <- as.Date("2016-11-16")
day3 <- as.Date("2016-11-27")
day4 <- as.Date("2016-11-14")
day5 <- as.Date("2016-12-02")

# Difference between last and first pizza day
print(day5 - day1)
```

    ## Time difference of 11 days

``` r
# Create vector pizza
pizza <- c(day1, day2, day3, day4, day5)

# Create differences between consecutive pizza days: day_diff
day_diff <- diff(pizza, lag = 1, differences = 1)
day_diff
```

    ## Time differences in days
    ## [1]  -5  11 -13  18

``` r
# Average period between two consecutive pizza days
print(mean(day_diff))
```

    ## Time difference of 2.75 days

**Calculus with times**

``` r
# login and logout
login <- as.POSIXct(c("2016-11-18 10:18:04 UTC", "2016-11-23 09:14:18 UTC", "2016-11-23 12:21:51 UTC", "2016-11-23 12:37:24 UTC", "2016-11-25 21:37:55 UTC"))

logout <- as.POSIXct(c("2016-11-18 10:56:29 UTC", "2016-11-23 09:14:52 UTC", "2016-11-23 12:35:48 UTC", "2016-11-23 13:17:22 UTC", "2016-11-25 22:08:47 UTC"))

# Calculate the difference between login and logout: time_online
time_online <- logout - login

# Inspect the variable time_online
#class(time_online)
time_online
```

    ## Time differences in secs
    ## [1] 2305   34  837 2398 1852

``` r
# Calculate the total time online
print(sum(time_online))
```

    ## Time difference of 7426 secs

``` r
# Calculate the average time online
print(mean(time_online))
```

    ## Time difference of 1485.2 secs
