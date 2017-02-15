-   [1, An introduction to Correlation](#an-introduction-to-correlation)
-   [2, An introduction to Linear Regression Models](#an-introduction-to-linear-regression-models)
-   [3, Linear Regression Models continued](#linear-regression-models-continued)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets only.

------------------------------------------------------------------------

1, An introduction to Correlation
---------------------------------

**Manual computation of correlation coefficients - Part 1**

``` r
library(XLConnectJars)
library(XLConnect)

# Read in the data set and assign to the object
PE <- readWorksheetFromFile('A Hands-on Introduction to Statistics with R.xls', sheet = 'PE', header = TRUE, startCol = 1, startRow = 1)

# Check it out
str(PE)
```

``` r
# Take a quick peek at both vectors
A <- PE$activeyears
B <- PE$endurance

# Save the differences of each vector element with the mean in a new variable
diff_A <- A - mean(A)
diff_B <- B - mean(B)

# Do the summation of the elements of the vectors and divide by N-1 in order to acquire the covariance between the two vectors
cov <- sum(diff_A*diff_B) / (length(A) - 1)
cov
```

**Manual computation of correlation coefficients - Part 2**

``` r
# Square the differences that were found in the previous step
sq_diff_A <- diff_A^2
sq_diff_B <- diff_B^2

# Take the sum of the elements, divide them by N-1 and consequently take the square root to acquire the sample standard deviations
sd_A <- sqrt(sum(sq_diff_A)/(length(A) - 1))
sd_B <- sqrt(sum(sq_diff_B)/(length(B) - 1))
sd_A
sd_B
```

**Manual computation of correlation coefficients - part 3**

``` r
# Combine all the pieces of the puzzle
correlation <- cov/(sd_A*sd_B)
correlation

# Check the validity of your result with the cor() command
cor(A,B)
```

**Creating scatterplots**

``` r
library(psych)

# Summary statistics
describe(PE)

# Scatter plots
par(mfrow = c(1, 3))

plot(PE$age ~ PE$activeyears)
plot(PE$endurance ~ PE$activeyears)
plot(PE$endurance ~ PE$age)

par(mfrow = c(1, 1))
```

**Correlation matrix**

``` r
# Correlation Analysis 
round(cor(PE[2:4], use = 'pairwise.complete.obs', method = 'pearson'), 2)  
# Do some correlation tests. If the null hypothesis of no correlation can be rejected on a significance level of 5%, then the relationship between variables is  significantly different from zero at the 95% confidence level

cor.test(PE$age, PE$activeyears)
cor.test(PE$age, PE$endurance)
cor.test(PE$endurance, PE$activeyears)
```

CAUTION:

-   The magnitude of a correlation depends upon many factors,
    including sampling.
-   The magnitude of a correlation is also influenced by measurement of
    X & Y.
-   The correlation coefficient is a sample statistic, just like
    the mean.

**Non-representative data samples**

``` r
# Summary statistics entire dataset
describe(impact)

# Calculate correlation coefficient 
entirecorr <- round(cor(impact$vismem2, impact$vermem2),2)

# Summary statistics subsets
describeBy(impact, group = impact$condition)

# Create 2 subsets: control and concussed
control <- subset(impact, impact$condition == 'control')
concussed <- subset(impact, impact$condition == 'concussed')

# Calculate correlation coefficients for each subset
controlcorr <- round(cor(control$vismem2, control$vermem2),2)
concussedcorr <- round(cor(concussed$vismem2, concussed$vermem2),2)

# Display all values at the same time
correlations <- cbind(entirecorr, controlcorr, concussedcorr)
correlations
```

2, An introduction to Linear Regression Models
----------------------------------------------

**Impact experiment**

``` r
# Create a correlation matrix for the dataset (9-14 are the '2' variables only)
correlations <- cor(PE[9:14,])

# Create the scatterplot matrix for the dataset
library(corrplot)

corrplot(correlations)
```

**Manual computation of a simple linear regression**

``` r
# Calculate the required means, standard deviations and correlation coefficient
mean_ay <- mean(PE$activeyears)
mean_e <- mean(PE$endurance)
sd_ay <- sd(PE$activeyears)
sd_e <- sd(PE$endurance)
r <- cor(PE$activeyears, PE$endurance)

# Calculate the slope
B_1 <- r * (sd_e)/(sd_ay)

# Calculate the intercept
B_0 <- mean_e - B_1 * mean_ay

# Plot of ic2 against sym2
plot(PE$activeyear, PE$endurance, main = 'Scatterplot', ylab = 'Endurance', xlab = 'Active Years')

# Add the regression line
abline(B_0, B_1, col = 'red')
```

**Executing a simple linear regression using R**

``` r
# Construct the regression model
model_1 <- lm(PE$endurance ~ PE$activeyear)

# Look at the results of the regression by using the summary function
summary(model_1)

# Extract the predicted values
predicted <- fitted(model_1)

# Create a scatter plot of Impulse Control against Symptom Score
plot(PE$endurance ~ PE$activeyear, main = 'Scatterplot', ylab = 'Endurance', xlab = 'Active Years')

# Add a regression line
abline(model_1, col = 'red')
abline(lm(predicted ~ PE$activeyears), col = 'green', lty = 2)
```

**Executing a multiple regression in R**

``` r
# Multiple Regression
model_2 <- lm(PE$endurance ~ PE$activeyear + PE$age)

# Examine the results of the regression
summary(model_2)

# Extract the predicted values
predicted <- fitted(model_2)

# Plotting predicted scores against observed scores
plot(predicted ~ PE$activeyears, main = 'Scatterplot', ylab = 'Endurance', xlab = 'Active Years')
abline(lm(predicted ~ PE$activeyears), col = 'green')
```

3, Linear Regression Models continued
-------------------------------------

**Calculating the sum of squared residuals**

``` r
# Create a linear regression with `ic2` and `vismem2` as regressors
model_1 <- lm(PE$endurance ~ PE$activeyear + PE$age)

# Extract the predicted values
predicted_1 <- fitted(model_1)

# Calculate the squared deviation of the predicted values from the observed values 
deviation_1 <- (predicted_1 - PE$endurance)^2

# Sum the squared deviations
SSR_1 <- sum(deviation_1)
SSR_1
```

**Standardized linear regression**

``` r
# Create a standardized simple linear regression
model_1_z <- lm(scale(PE$endurance) ~ scale(PE$activeyear))

# Look at the output of this regression model
summary(model_1_z)

# Extract the R-Squared value for this regression
r_square_1 <- summary(model_1_z)$r.squared
r_square_1

# Calculate the correlation coefficient
corr_coef_1 <- sqrt(r_square_1)
corr_coef_1

# Create a standardized multiple linear regression
model_2_z <- lm(scale(PE$endurance) ~ scale(PE$activeyear) + scale(PE$age))

# Look at the output of this regression model
summary(model_2_z)

# Extract the R-Squared value for this regression
r_square_2 <- summary(model_2_z)$r.squared
r_square_2

# Calculate the correlation coefficient
corr_coef_2 <- sqrt(r_square_2)
corr_coef_2
```

Assumptions of linear regression:

-   Normal distribution for Y.
-   Linear relationship between X and Y.
-   Homoscedasticity.
-   Reliability of X and Y.
-   Validity of X and Y.
-   Random and representative sampling.

Check it out with Anscombe's quartet plots.

**Plotting residuals**

``` r
# Extract the residuals from the model
residual <- resid(model_2)

# Draw a histogram of the residuals
hist(residual)

# Extract the predicted symptom scores from the model
predicted <- fitted(model_2)

# Plot the residuals against the predicted symptom scores
plot(residual ~ predicted, main = 'Scatterplot', ylab = 'Model 2 Residuals', xlab = 'Model 2 Predicted Scores')
abline(lm(residual ~ predicted), col = 'red')
```
