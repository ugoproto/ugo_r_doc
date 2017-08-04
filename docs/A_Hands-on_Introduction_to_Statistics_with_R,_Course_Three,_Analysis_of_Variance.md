-   [1, An introduction to ANOVA](#an-introduction-to-anova)
-   [2, Post-hoc analysis](#post-hoc-analysis)
-   [3, Between groups factorial ANOVA](#between-groups-factorial-anova)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

1, An introduction to ANOVA
---------------------------

**Working memory experiment**

We'll use data from the working memory experiment, which investigates the relationship between the number of training days and a change in IQ. There are four independent groups, each of which trained for a different period of time: 8, 12, 17, or 19 days. The independent variable is the number of training days and the dependent variable is the IQ gain.

``` r
# Print the data set in the console
head(wm)
```

    ##   subject condition   iq
    ## 1       1    8 days 12.4
    ## 2       2    8 days 11.8
    ## 3       3    8 days 14.6
    ## 4       4    8 days  7.7
    ## 5       5    8 days 15.7
    ## 6       6    8 days 11.6

``` r
library(psych)

# Summary statistics by all groups (8 sessions, 12 sessions, 17 sessions, 19 sessions)
describeBy(wm, wm$condition)
```

    ## 
    ##  Descriptive statistics by group 
    ## group: 12 days
    ##            vars  n mean   sd median trimmed  mad  min  max range skew
    ## subject       1 20 30.5 5.92  30.50   30.50 7.41 21.0 40.0  19.0 0.00
    ## condition*    2 20  NaN   NA     NA     NaN   NA  Inf -Inf  -Inf   NA
    ## iq            3 20 11.7 2.58  11.65   11.69 2.89  6.9 16.1   9.2 0.05
    ##            kurtosis   se
    ## subject       -1.38 1.32
    ## condition*       NA   NA
    ## iq            -1.06 0.58
    ## -------------------------------------------------------- 
    ## group: 17 days
    ##            vars  n mean   sd median trimmed  mad  min  max range skew
    ## subject       1 20 50.5 5.92   50.5    50.5 7.41 41.0 60.0  19.0 0.00
    ## condition*    2 20  NaN   NA     NA     NaN   NA  Inf -Inf  -Inf   NA
    ## iq            3 20 13.9 2.26   13.6    13.9 2.00  9.8 18.1   8.3 0.11
    ##            kurtosis   se
    ## subject       -1.38 1.32
    ## condition*       NA   NA
    ## iq            -0.85 0.50
    ## -------------------------------------------------------- 
    ## group: 19 days
    ##            vars  n  mean   sd median trimmed  mad  min  max range  skew
    ## subject       1 20 70.50 5.92   70.5   70.50 7.41 61.0 80.0  19.0  0.00
    ## condition*    2 20   NaN   NA     NA     NaN   NA  Inf -Inf  -Inf    NA
    ## iq            3 20 14.75 2.50   15.3   14.71 2.15 10.4 19.2   8.8 -0.09
    ##            kurtosis   se
    ## subject       -1.38 1.32
    ## condition*       NA   NA
    ## iq            -0.99 0.56
    ## -------------------------------------------------------- 
    ## group: 8 days
    ##            vars  n  mean   sd median trimmed  mad min  max range  skew
    ## subject       1 20 10.50 5.92   10.5   10.50 7.41 1.0 20.0  19.0  0.00
    ## condition*    2 20   NaN   NA     NA     NaN   NA Inf -Inf  -Inf    NA
    ## iq            3 20 10.91 2.63   11.3   10.97 2.67 5.4 15.7  10.3 -0.21
    ##            kurtosis   se
    ## subject       -1.38 1.32
    ## condition*       NA   NA
    ## iq            -0.70 0.59

``` r
# Boxplot IQ versus cond
boxplot(wm$iq ~ wm$condition, main="Boxplot", xlab="Group (cond)", ylab="IQ")
```

<center>![](A_Hands-on_Introduction_to_Statistics_with_R,_Course_Three,_Analysis_of_Variance/figure-markdown_strict+backtick_code_blocks+autolink_bare_uris/unnamed-chunk-3-1.png)</center>

Notice that the IQ increases as the amount of training sessions increases.

**t-test vs ANOVA**

ANOVA is used when more than two group means are compared, whereas a t-test can only compare two group means.

**Generate density plot of the F-distribution**

The test statistic associated with ANOVA is the F-test (or F-ratio). Recall that when carrying out a t-test, you computed an observed t-value, then compared that with a critical value derived from the relevant t-distribution. That t-distribution came from a family of t-distributions, each of which was defined entirely by its degrees of freedom.

ANOVA uses the same principle, but instead an observed F-value is computed and compared to the relevant F-distribution. That F-distribution comes from a family of F-distributions, each of which is defined by two numbers (i.e. degrees of freedom).

F-distribution has a different shape than the t-distribution.

``` r
# Create the vector x
x <- seq(from = 0, to = 2, length = 100)

# Simulate the F-distributions
y_1 <- df(x, 1, 1)
y_2 <- df(x, 3, 1)
y_3 <- df(x, 6, 1)
y_4 <- df(x, 3, 3)
y_5 <- df(x, 6, 3)
y_6 <- df(x, 3, 6)
y_7 <- df(x, 6, 6)

# Plot the F-distributions
plot(x, y_1, col = 1, 'l')
lines(x,y_2, col = 2, 'l')
lines(x,y_3, col = 3, 'l')
lines(x,y_4, col = 4, 'l')
lines(x,y_5, col = 5, 'l')
lines(x,y_6, col = 6, 'l')
lines(x,y_7, col = 7, 'l')

# Add the legend in the top right corner and with the title 'F distributions'
legend('topright', title = 'F distributions', c('df = (1,1)', 'df = (3,1)', 'df = (6,1)', 'df = (3,3)', 'df = (6,3)', 'df = (3,6)', 'df = (6,6)'), col = c(1, 2, 3, 4, 5, 6, 7), lty = 1)
```

<center>![](A_Hands-on_Introduction_to_Statistics_with_R,_Course_Three,_Analysis_of_Variance/figure-markdown_strict+backtick_code_blocks+autolink_bare_uris/unnamed-chunk-4-1.png)</center>

The F-distribution cannot take negative values, because it is a ratio of variances and variances are always non-negative numbers. The distribution represents the ratio between the variance between groups and the variance within groups.

**Between group sum of squares**

To calculate the F-value, you need to calculate the ratio between the variance between groups and the variance within groups. Furthermore, to calculate the variance (i.e. mean of squares), you first have to calculate the sum of squares.

Now, remember that the working memory experiment investigates the relationship between the change in IQ and the number of training sessions. Calculate the between group sum of squares for the data from this experiment.

``` r
# Define number of subjects in each group
n <- 20

# Calculate group means
Y_j <- as.numeric(tapply(wm$iq, wm$condition, mean))
Y_j
```

    ## [1] 11.700 13.905 14.750 10.910

``` r
# Calculate the grand mean
Y_T <- mean(wm$iq)
Y_T
```

    ## [1] 12.81625

``` r
# Calculate the sum of squares
SS_A <- sum((Y_j - Y_T)^2) * n
SS_A
```

    ## [1] 196.0914

**Within groups sum of squares**

To calculate the F-value, you also need the variance within groups. Similar to the last exercise, we'll start by computing the within groups sum of squares.

``` r
# Create four subsets of the four groups, containing the IQ results

# Make the subset for the group cond = '8 days'
Y_i1 <- subset(wm$iq, wm$condition == '8 days')

# Make the subset for the group cond = '12 days'
Y_i2 <- subset(wm$iq, wm$condition == '12 days')

# Make the subset for the group cond = '17 days'
Y_i3 <- subset(wm$iq, wm$condition == '17 days')

# Make the subset for the group cond = '19 days'
Y_i4 <- subset(wm$iq, wm$condition == '19 days')

# subtract the individual values by their group means
# You have already calculated the group means in the previous exercise so use this result, the vector that contains these group means was called Y_j
S_1 <- Y_i1 - Y_j[1]
S_2 <- Y_i2 - Y_j[2]

# Do it without the vector Y_j, so calculate the group means again.
S_3 <- Y_i3 - mean(Y_i3)
S_4 <- Y_i4 - mean(Y_i4)

#Put everything back in one vector
S_T <- c(S_1,S_2,S_3,S_4)

#Calculate the sum of squares by using the vector S_T
SS_SA <- sum(S_T^2)
```

**Calculating the F-ratio**

Calculate the F-ratio.

``` r
# Number of groups
a <- 4

# Number of subject in each group
n <- 20

# Define the degrees of freedom
df_A <- a - 1
df_SA <- a*(n - 1)

# Calculate the mean squares (variances) by using the sum of squares SS_A and SS_SA
MS_A <- SS_A/df_A
MS_SA <- SS_SA/df_SA

# Calculate the F-ratio
F <- MS_A/MS_SA
```

**A faster way: ANOVA in R**

Normally, we do not have to do all calculations.

``` r
# Apply the aov function
anova.wm <- aov(wm$iq ~ wm$condition)
anova.wm
```

    ## Call:
    ##    aov(formula = wm$iq ~ wm$condition)
    ## 
    ## Terms:
    ##                 wm$condition Residuals
    ## Sum of Squares      196.0914  473.4175
    ## Deg. of Freedom            3        76
    ## 
    ## Residual standard error: 2.495832
    ## Estimated effects may be unbalanced

``` r
# Look at the summary table of the result
summary(anova.wm)
```

    ##              Df Sum Sq Mean Sq F value   Pr(>F)    
    ## wm$condition  3  196.1   65.36   10.49 7.47e-06 ***
    ## Residuals    76  473.4    6.23                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

F-value is significant.

**Levene's test**

The assumptions of ANOVA are relatively simple. Similar to an independent t-test, we have a continuous dependent variable, which we assume to be normally distributed. Furthermore, we assume homogeneity of variance, which can be tested with Levene's test.

``` r
library(car)

# Levene's test
leveneTest(wm$iq ~ wm$condition)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value Pr(>F)
    ## group  3  0.1405 0.9355
    ##       76

``` r
# Levene's test with the change for the default, namely center = mean
leveneTest(wm$iq ~ wm$condition, center = mean)
```

    ## Levene's Test for Homogeneity of Variance (center = mean)
    ##       Df F value Pr(>F)
    ## group  3  0.1598  0.923
    ##       76

The assumption of homogeneity of variance hold: the within group variance equivalent for all groups.

2, Post-hoc analysis
--------------------

Post-hoc tests help finding out which groups differ significantly from one other and which do not. More formally, post-hoc tests allow for multiple pairwise comparisons without inflating the type I error.

What does it mean to inflate the type I error?

Suppose the post-hoc test involves performing three pairwise comparisons, each with the probability of a type I error set at 5%. The probability of making at least one type I error: if you assume independence of these three events, the maximum familywise error rate is then equal to: 1 - (0.95 x 0.95 x 0.95) = 14.26 %.

In other words, the probability of having at least one false alarm (i.e. type I error) is 14.26%.

What is the maximum familywise error rate for the working memory experiment, assuming that you do all possible pairwise comparisons with a type I error of 5%? 26.49%.

Null Hypothesis Significance Testing (NHST) is a statistical method used to test whether or not you are able to reject or retain the null hypothesis. This type of test can confront you with a type I error. This happens when the test rejects the null hypothesis, while it is actually true in reality. Furthermore, the test can also deliver a type II error. This is the failure to reject a null hypothesis when it is false. All hypothesis tests have a probability of making type I and II errors.

Sensitivity and specificity are two concepts that statisticians use to measure the performance of a statistical test. The sensitivity of a test is its true positive rate:

$$sensitivity = \frac{number~of~true~positives}{number~ of~true~positives + number~of~false~negatives}$$

The specificity of a test is its true negative rate:

$$specificity = \frac{number~of~true~negatives}{number~ of~true~negatives + number~of~false~positives}$$

Calculate both the sensitivity and specificity of the test based on numbers displayed in the NHST table?

<center>![](A_Hands-on_Introduction_to_Statistics_with_R,_Course_Three,_Analysis_of_Variance/NHST_1.png)</center>

The sensitivity is 0.89 and the specificity is 0.85.

**Calculate and interpret the results of Tukey**

In a situation were you do multiple pairwise comparisons, the probability of type I errors in the process inflates substantially. Therefore, it is better to build in adjustments to take this into account. This is what Tukey tests and other post-hoc procedures do. They adjust the p-value to prevent inflation of the type I error rate. Use Tukey's procedure.

``` r
# Read in the data set and assign to the object
wm <- readWorksheetFromFile('A Hands-on Introduction to Statistics with R.xls', sheet = 'wm', header = TRUE, startCol = 1, startRow = 1)

# This will print the data set in the console
head(wm)
```

    ##   cond pre post gain train
    ## 1  t08   8    9    1     1
    ## 2  t08   8   10    2     1
    ## 3  t08   8    8    0     1
    ## 4  t08   8    7   -1     1
    ## 5  t08   9   11    2     1
    ## 6  t08   9   10    1     1

``` r
# Revision: Analysis of variance
anova_wm <- aov(wm$gain ~ wm$cond)

# Summary Analysis of Variance
summary(anova_wm)
```

    ##              Df Sum Sq Mean Sq F value Pr(>F)    
    ## wm$cond       4  274.0   68.51   34.57 <2e-16 ***
    ## Residuals   115  227.9    1.98                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# Post-hoc (Tukey)
TukeyHSD(anova_wm)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = wm$gain ~ wm$cond)
    ## 
    ## $`wm$cond`
    ##               diff         lwr       upr     p adj
    ## t08-control -0.625 -1.69355785 0.4435578 0.4871216
    ## t12-control  0.625 -0.44355785 1.6935578 0.4871216
    ## t17-control  2.425  1.35644215 3.4935578 0.0000001
    ## t19-control  3.625  2.55644215 4.6935578 0.0000000
    ## t12-t08      1.250  0.01613568 2.4838643 0.0454650
    ## t17-t08      3.050  1.81613568 4.2838643 0.0000000
    ## t19-t08      4.250  3.01613568 5.4838643 0.0000000
    ## t17-t12      1.800  0.56613568 3.0338643 0.0008953
    ## t19-t12      3.000  1.76613568 4.2338643 0.0000000
    ## t19-t17      1.200 -0.03386432 2.4338643 0.0607853

``` r
# Plot confidence intervals
#plot(c(TukeyHSD(anova_wm)$lwr,TukeyHSD(anova_wm)$upr))
plot(TukeyHSD(anova_wm))
```

<center>![](A_Hands-on_Introduction_to_Statistics_with_R,_Course_Three,_Analysis_of_Variance/figure-markdown_strict+backtick_code_blocks+autolink_bare_uris/unnamed-chunk-11-1.png)</center>

**Bonferroni adjusted p-values**

Just like Tukey's procedure, the Bonferroni correction is a method that is used to counteract the problem of inflated type I errors while engaging in multiple pairwise comparisons between subgroups. Bonferroni is generally known as the most conservative method to control the familywise error rate.

Bonferroni is based on the idea that if you test *N* dependent or independent hypotheses, one way of maintaining the familywise error rate is to test each individual hypothesis at a statistical significance level that is deflated by a factor of $\frac{1}{n}$. So, for a significance level for the whole family of tests of *α*, the Bonferroni correction would be to test each of the individual tests at a significance level of $\frac{\alpha}{n}$.

The Bonferroni correction is controversial. It is a strict measure to limit false positives and generates conservative p-value. Alternative: increase the sample size, compute the false discovery rate (the expected percent of false predictions in the set of predictions. For example if the algorithm returns 100 results with a false discovery rate of .3 then we should expect 70 of them to be correct.), and the Holm-Bonferroni method.

``` r
# Use `p.adjust` 
bonferroni_ex <- p.adjust(.005, method='bonferroni', n = 8)

bonferroni_ex
```

    ## [1] 0.04

``` r
# Pairwise T-test
pairwise.t.test(wm$gain,wm$cond, p.adjust = 'bonferroni')
```

    ## 
    ##  Pairwise comparisons using t tests with pooled SD 
    ## 
    ## data:  wm$gain and wm$cond 
    ## 
    ##     control t08     t12     t17    
    ## t08 1.00000 -       -       -      
    ## t12 1.00000 0.05862 -       -      
    ## t17 5.9e-08 3.8e-09 0.00096 -      
    ## t19 6.4e-15 2.9e-15 6.7e-09 0.08084
    ## 
    ## P value adjustment method: bonferroni

3, Between groups factorial ANOVA
---------------------------------

**Data exploration with a barplot**

We'll use in this chapter is a randomized controlled experiment investigating the effects of talking on a cell phone while driving. The dependent variable in the experiment is the number of driving errors that subjects made in a driving simulator. There are two independent variables:

-   Conversation difficulty: None, Low, High
-   Driving difficulty: Easy, Difficult

<!-- -->

``` r
# Read in the data set and assign to the object
ab <- readWorksheetFromFile('A Hands-on Introduction to Statistics with R.xls', sheet = 'ab', header = TRUE, startCol = 1, startRow = 1)

ab$subject <- as.integer(ab$subject)
ab$conversation <- as.factor(ab$conversation)
ab$driving <- as.factor(ab$driving)
ab$error <- as.integer(ab$error)

# This will print the data set in the console
head(ab)
```

    ##   subject conversation driving errors error
    ## 1       1         None    Easy     20    20
    ## 2       2         None    Easy     19    19
    ## 3       3         None    Easy     31    31
    ## 4       4         None    Easy     27    27
    ## 5       5         None    Easy     31    31
    ## 6       6         None    Easy     17    17

Each of the subjects was randomly assigned to one of six conditions formed by combining different values of the independent variables.

-   `subject`: unique identifier for each subject conversation: level of conversation difficulty.
-   `driving` : level of driving difficulty in the simulator.
-   `errors`: number of driving errors made.

<!-- -->

``` r
# Use the tapply function to create your groups
ab_groups <- tapply(ab$errors, list(ab$driving, ab$conversation), sum)

# Make the required barplot
barplot(ab_groups, beside = TRUE, col = c('orange','blue'), main = 'Driving Errors', xlab = 'Conversation Demands', ylab = 'Errors')

# Add the legend
legend('topright', c('Difficult','Easy'), title = 'Driving', fill = c('orange','blue'))
```

<center>![](A_Hands-on_Introduction_to_Statistics_with_R,_Course_Three,_Analysis_of_Variance/figure-markdown_strict+backtick_code_blocks+autolink_bare_uris/unnamed-chunk-14-1.png)</center>

The driving errors made during different driving conditions are influenced by the level of conversation demand. In other words, the driving conditions have a different effect on the number of errors made, depending on the level of conversation demand.

**The homogeneity of variance assumption**

Before we do factorial ANOVA, we need to test the homogeneity of the variance assumption. When studying one-way ANOVA, we tested this assumption with the `leveneTest` function.

We now have two independent variables instead of just one.

``` r
# Test the homogeneity of variance assumption
leveneTest(ab$errors ~ ab$conversation * ab$driving)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##        Df F value Pr(>F)
    ## group   5  0.5206 0.7602
    ##       114

The homogeneity of variance assumption holds.

By performing a `leveneTest`, we can check whether or not the homogeneity of variance assumption holds for a given dataset. The assumption must hold for the results of an ANOVA analysis to be valid.

Recall from the first chapter that ANOVA makes use of F-statistics, or F-ratios, in which two types of degrees of freedom are involved.

``` r
dim(ab)
```

    ## [1] 120   5

``` r
str(ab$conversation)
```

    ##  Factor w/ 3 levels "High demand",..: 3 3 3 3 3 3 3 3 3 3 ...

``` r
str(ab$driving)
```

    ##  Factor w/ 2 levels "Difficult","Easy": 2 2 2 2 2 2 2 2 2 2 ...

There are 120 subjets and 2 (Easy, Difficult) \* 3 (High Demand, Low Demand, None) = 6 groups.

**The factorial ANOVA**

``` r
# Factorial ANOVA
ab_model <- aov(ab$errors ~ ab$conversation * ab$driving)

# Get the summary table
summary(ab_model)
```

    ##                             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## ab$conversation              2   4416    2208   36.14 6.98e-13 ***
    ## ab$driving                   1   5782    5782   94.64  < 2e-16 ***
    ## ab$conversation:ab$driving   2   1639     820   13.41 5.86e-06 ***
    ## Residuals                  114   6965      61                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Based on the summary table of the factorial ANOVA, the main effect for driving difficulty, the main effect for conversation difficulty and the interaction effect are all significant.

**The interaction effect**

Now it's time to explore the interaction effect. You will do this with the help of a simple effects analysis.

Why a simple effects analysis? Well, remember what we had to do when you had a significant main effect in a one-way ANOVA? There, you just had to perform some post-hoc tests to see from which level of the categorical variable the main effect was coming. With an interaction effect, it is quite similar. Conduct a simple effects analysis of the variable `conversation` on the outcome variable `errors` at each level of `driving`.

``` r
# Create the two subsets
ab_1 <- subset(ab, ab$driving == 'Easy')  
ab_2 <- subset(ab, ab$driving == 'Difficult')

# Perform the one-way ANOVA for both subsets
aov_ab_1 <- aov(ab_1$errors ~ ab_1$conversation)
aov_ab_2 <- aov(ab_2$errors ~ ab_2$conversation)

# Get the summary tables for both aov_ab_1 and aov_ab_2
summary(aov_ab_1)
```

    ##                   Df Sum Sq Mean Sq F value Pr(>F)  
    ## ab_1$conversation  2  504.7   252.3   4.928 0.0106 *
    ## Residuals         57 2918.5    51.2                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
summary(aov_ab_2)
```

    ##                   Df Sum Sq Mean Sq F value   Pr(>F)    
    ## ab_2$conversation  2   5551    2776   39.09 2.05e-11 ***
    ## Residuals         57   4047      71                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

There a significant simple effect for the easy driving condition based on the summary table of `aov_ab_1`.

There a significant simple effect for the difficult driving condition based on the summary table of `aov_ab_2`.

**The effect sizes**

The definition of an interaction effect states that the effect of one variable changes across levels of the other variable. For example, we might expect the effect of conversation to be greater when driving conditions are difficult than when they are relatively easy.

Unfortunately, it is not quite that simple. In order to really understand the different effect sizes, you should make use of the `etaSquared` function.

``` r
library(lsr)

# Calculate the etaSquared for the easy driving case
#easy is ab_1
#difficult is ab_2
etaSquared(aov_ab_1, anova = TRUE)
```

    ##                     eta.sq eta.sq.part      SS df        MS        F
    ## ab_1$conversation 0.147433    0.147433  504.70  2 252.35000 4.928458
    ## Residuals         0.852567          NA 2918.55 57  51.20263       NA
    ##                            p
    ## ab_1$conversation 0.01061116
    ## Residuals                 NA

``` r
# Calculate the etaSquared for the difficult driving case
etaSquared(aov_ab_2, anova = TRUE)
```

    ##                      eta.sq eta.sq.part       SS df         MS        F
    ## ab_2$conversation 0.5783571   0.5783571 5551.033  2 2775.51667 39.09275
    ## Residuals         0.4216429          NA 4046.900 57   70.99825       NA
    ##                              p
    ## ab_2$conversation 2.046097e-11
    ## Residuals                   NA

Based on the output of the `etaSquared` function for the easy driving condition, the percentage of variance explained by the conversation variable is 14.7%; the percentage of variance explained by the conversation variable is 57.8%.

**Pairwise comparisons**

Finally, let us look at pairwise comparisons for the simple effects. You can do this with the Tukey post-hoc test.

``` r
# Tukey for easy driving
TukeyHSD(aov_ab_1)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = ab_1$errors ~ ab_1$conversation)
    ## 
    ## $`ab_1$conversation`
    ##                         diff        lwr        upr     p adj
    ## Low demand-High demand -6.05 -11.495243 -0.6047574 0.0260458
    ## None-High demand       -6.25 -11.695243 -0.8047574 0.0207614
    ## None-Low demand        -0.20  -5.645243  5.2452426 0.9957026

``` r
# Tukey for difficult driving
TukeyHSD(aov_ab_2)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = ab_2$errors ~ ab_2$conversation)
    ## 
    ## $`ab_2$conversation`
    ##                          diff       lwr        upr     p adj
    ## Low demand-High demand  -9.75 -16.16202  -3.337979 0.0015849
    ## None-High demand       -23.45 -29.86202 -17.037979 0.0000000
    ## None-Low demand        -13.70 -20.11202  -7.287979 0.0000103

For 'Easy Driving', two mean differences in terms of number of errors are significant (below 0.05).

For 'Difficult Driving', three mean differences in terms of number of errors are significant (below 0.05).