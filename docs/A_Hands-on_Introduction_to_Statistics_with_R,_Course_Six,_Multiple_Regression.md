-   [1, A gentle introduction to the principles of multiple regression](#a-gentle-introduction-to-the-principles-of-multiple-regression)
-   [2, Intuition behind estimation of multiple regression  coefficients](#intuition-behind-estimation-of-multiple-regression-coefficients)
-   [3, Dummy coding](#dummy-coding)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets only.

------------------------------------------------------------------------

1, A gentle introduction to the principles of multiple regression
-----------------------------------------------------------------

**Multiple regression: visualization of the relationships**

``` r
# Perform the two single regressions and save them in a variable
#model_years <- lm(salary ~ years, data = fs)
#model_pubs <-  lm(salary ~ pubs, data = fs)
model_years <- lm(fs$salary ~ fs$years)
model_pubs <- lm(fs$salary ~ fs$pubs)

# Plot both enhanced scatter plots in one plot matrix of 1 by 2
par(mfrow = c(1, 2))

#plot(fs$years, fs$salary, main = 'plot_years', xlab = 'years', ylab = 'salary')
plot(fs$salary ~ fs$years, main = 'plot_years', xlab = 'years', ylab = 'salary')
abline(model_years)

#plot(fs$pubs, fs$salary, main = 'plot_pubs', xlab = 'pubs', ylab = 'salary')
plot(fs$salary ~ fs$pubs, main = 'plot_pubs', xlab = 'pubs', ylab = 'salary')
abline(model_pubs)
```

**Multiple regression: model selection**

``` r
# Do a single regression of salary onto years of experience and check the output
model_1 <- lm(fs$salary ~ fs$years)
summary(model_1)

# Do a multiple regression of salary onto years of experience and numbers of publications and check the output
model_2 <- lm(fs$salary ~ fs$years + fs$pubs)
summary(model_2)

# Save the R squared of both models in preliminary variables
preliminary_model_1 <- summary(model_1)$r.squared
preliminary_model_2 <- summary(model_2)$r.squared

# Round them off while you save them in new variables
r_squared <- c()
r_squared[1] <- round(preliminary_model_1, 3)
r_squared[2] <- round(preliminary_model_2, 3)

# Print out the vector to see both R squared coefficients
r_squared
```

**Multiple regression: beware of redundancy**

``` r
# Do multiple regression and check the regression output
model_3 <- lm(fs$salary ~ fs$years + fs$pubs + fs$age)
summary(model_3)

# Round off the R squared coefficients and save the result in the vector (in one step!)
r_squared[3] <- round(summary(model_3)$r.squared,3)

# Print out the vector in order to display all R squared coefficients simultaneously
r_squared
```

2, Intuition behind estimation of multiple regression coefficients
------------------------------------------------------------------

**Definition of matrices**

``` r
# Construction of 3 by 8 matrix r that contains the numbers 1 up to 24
r <- matrix(seq(1,24), 3)

# Construction of 3 by 8 matrix s that contains the numbers 21 up to 44
s <- matrix(seq(21,44), 3)

# Take the transpose t of matrix r
t <- t(r)
```

**Addition, subtraction and multiplication of matrices**

``` r
# Compute the sum of matrices r and s
operation_1 <- r + s

# Compute the difference between matrices r and s
operation_2 <- r - s

# Multiply matrices t and s
operation_3 <- t %*% s
```

**Row vector of sums**

``` r
# The raw dataframe `X` is already loaded in.
X

# Construction of 1 by 10 matrix I of which the elements are all 1
I <- matrix(rep(1,10), 1, 10)

# Compute the row vector of sums
t_mat <- I %*% X
```

**Row vector of means and matrix of means**

``` r
# The data matrix `X` and the row vector of sums (`T`) are saved and can be used.
# Number of observations
n = 10

# Compute the row vector of means
# you summed up the row, you divide by the nrow to compute the average
M <- t_mat / 10

# Construction of 10 by 1 matrix J of which the elements are all 1
J <- matrix(rep(1,10), 10, 1)

# Compute the matrix of means 
MM <- J %*% M
```

**Matrix of deviation scores**

``` r
# The previously generated matrices X, M and MM do not need to be constructed again but are saved and can be used.

# Matrix of deviation scores D 
D <- X - MM
```

**Sum of squares and sum of cross products matrix**

``` r
# The previously generated matrices X, M, MM and D do not need to be constructed again but are saved and can be used.

# Sum of squares and sum of cross products matrix
S <- t(D) %*% D
```

**Calculating the correlation matrix**

``` r
# The previously generated matrices X, M, MM, D and S do not need to be constructed again but are saved and can be used.
n = 10

# Construct the variance-covariance matrix 
C <-  S * 1/n

# Generate the standard deviations matrix 
SD <- diag(x = diag(C)^(1/2), nrow = 3, ncol = 3)

# Compute the correlation matrix
R <- solve(SD) %*% C %*% solve(SD)
```

3, Dummy coding
---------------

**Starting off**

``` r
# Summary statistics
describeBy(fs, fs$dept)
```

A system to code categorical predictors in a regression analysis

Suppose we have a categorical vector of observations. The vector counts 4 distinct groups. Here is how to assign the dummy variables:

<table>
<thead>
<tr class="header">
<th>ProfID</th>
<th>Group</th>
<th>Pubs</th>
<th>D1</th>
<th>D2</th>
<th>D3</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>NU</td>
<td>Cognitive</td>
<td>83</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr class="even">
<td>ZH</td>
<td>Clinical</td>
<td>74</td>
<td>1</td>
<td>0</td>
<td>0</td>
</tr>
<tr class="odd">
<td>MK</td>
<td>Developmental</td>
<td>80</td>
<td>0</td>
<td>1</td>
<td>0</td>
</tr>
<tr class="even">
<td>RH</td>
<td>Social</td>
<td>68</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
</tbody>
</table>

Summary statistics:

<table>
<thead>
<tr class="header">
<th>Group</th>
<th>M</th>
<th>SD</th>
<th>N</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Cognitive</td>
<td>93.31</td>
<td>29.48</td>
<td>13</td>
</tr>
<tr class="even">
<td>Clinical</td>
<td>60.67</td>
<td>11.12</td>
<td>8</td>
</tr>
<tr class="odd">
<td>Developmental</td>
<td>103.5</td>
<td>23.64</td>
<td>6</td>
</tr>
<tr class="even">
<td>Social</td>
<td>70.13</td>
<td>21.82</td>
<td>9</td>
</tr>
</tbody>
</table>

Model:

*Y* = *β*<sub>0</sub> + *β*<sub>1</sub>(*D*<sub>1</sub>)+*β*<sub>2</sub>(*D*<sub>2</sub>)+*β*<sub>3</sub>(*D*<sub>3</sub>)+*ϵ*

Coefficient:

<table>
<thead>
<tr class="header">
<th></th>
<th>B</th>
<th>SE</th>
<th>B</th>
<th>t</th>
<th>p</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td></td>
<td>93.31</td>
<td>6.5</td>
<td>0</td>
<td>14.37</td>
<td>&lt;.001</td>
</tr>
<tr class="even">
<td>D1 (Clinical)</td>
<td>-32.64</td>
<td>10.16</td>
<td>-0.51</td>
<td>-3.21</td>
<td>0.003</td>
</tr>
<tr class="odd">
<td>D2 (Devel)</td>
<td>10.19</td>
<td>11.56</td>
<td>0.14</td>
<td>0.88</td>
<td>0.384</td>
</tr>
<tr class="even">
<td>D3 (Social)</td>
<td>-23.18</td>
<td>10.52</td>
<td>-0.35</td>
<td>-2.2</td>
<td>0.035</td>
</tr>
</tbody>
</table>

**Creating dummy variables (1)**

``` r
# Create the dummy variables
dept_code <- dummy.code(fs$dept)
dept_code

# Merge the dataset in an extended dataframe
extended_fs <- cbind(fs, dept_code)

# Look at the extended dataframe
extended_fs

# Provide summary statistics
summary(extended_fs)
```

**Creating dummy variables (2)**

``` r
# Regress salary against years and publications
model <- lm(fs$salary ~ fs$years + fs$pubs)

# Apply the summary function to get summarized results for model
summary(model)

# Compute the confidence intervals for model
confint(model) 

# Create dummies for the categorical variable fs$dept by using the C() function
dept_code <- C(fs$dept, treatment)

# Regress salary against years, publications and department 
model_dummy <- lm(fs$salary ~ fs$years + fs$pubs + dept_code)

# Apply the summary function to get summarized results for model_dummy
summary(model_dummy)

# Compute the confidence intervals for model_dummy
confint(model_dummy)
```

**Model selection: ANOVA**

``` r
# Compare model 4 with model3
anova(model, model_dummy)
```

**Discrepancy between actual and predicted means**

``` r
# Actual means of fs$salary
tapply(fs$salary, fs$dept, mean)
```

**Unweighted effects coding**

Consult the PDF for 'Unweighted Effects Coding'.

``` r
# Number of levels
fs$dept

# Factorize the categorical variable fs$dept and name the factorized variable dept.f
dept.f <- factor(fs$dept)

# Assign the 3 levels generated in step 2 to dept.f
contrasts(dept.f) <- contr.sum(3)

# Regress salary against dept.f
model_unweighted <- lm(fs$salary ~ dept.f)

# Apply the summary() function
summary(model_unweighted)
```

**Weighted effects coding**

Consult 'Weighted Effects Coding'.

``` r
# Factorize the categorical variable fs$dept and name the factorized variable dept.g
dept.g <- factor(fs$dept)

# Assign the weights matrix to dept.g 
contrasts(dept.g) <- weights

# Regress salary against dept.f and apply the summary() function
model_weighted <- lm(fs$salary ~ dept.g)

# Apply the summary() function
summary(model_weighted)
```
