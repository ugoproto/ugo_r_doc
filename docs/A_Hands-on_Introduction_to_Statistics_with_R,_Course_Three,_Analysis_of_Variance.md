-   [1, An introduction to ANOVA](#an-introduction-to-anova)
-   [2, Post-hoc analysis](#post-hoc-analysis)
-   [3, Between groups factorialANOVA](#between-groups-factorial-anova)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

1, An introduction to ANOVA
---------------------------

**Working memory experiment**

We'll use data from the working memory experiment, which investigates
the relationship between the number of training days and a change in IQ.
There are four independent groups, each of which trained for a different
period of time: 8, 12, 17, or 19 days. The independent variable is the
number of training days and the dependent variable is the IQ gain.

``` r
library(XLConnectJars)
library(XLConnect)

# Read in the data set and assign to the object
wm <- readWorksheetFromFile('A Hands-on Introduction to Statistics with R.xls', sheet = 'w_m', header = TRUE, startCol = 1, startRow = 1)

# Check it out
head(wm)
```

``` r
library(psych)

# Summary statistics by all groups (8 sessions, 12 sessions, 17 sessions, 19 sessions)
describeBy(wm, wm$condition)

# Boxplot IQ versus cond
boxplot(wm$iq ~ wm$condition, main="Boxplot", xlab="Group (cond)", ylab="IQ")
```

Notice that the IQ increases as the amount of training sessions
increases.

**t-test vs ANOVA**

ANOVA is used when more than two group means are compared, whereas a
t-test can only compare two group means.

**Generate density plot of the F-distribution**

The test statistic associated with ANOVA is the F-test (or F-ratio).
Recall that when carrying out a t-test, you computed an observed
t-value, then compared that with a critical value derived from the
relevant t-distribution. That t-distribution came from a family of
t-distributions, each of which was defined entirely by its degrees of
freedom.

ANOVA uses the same principle, but instead an observed F-value is
computed and compared to the relevant F-distribution. That
F-distribution comes from a family of F-distributions, each of which is
defined by two numbers (i.e. degrees of freedom).

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

The F-distribution cannot take negative values, because it is a ratio of
variances and variances are always non-negative numbers. The
distribution represents the ratio between the variance between groups
and the variance within groups.

**Between group sum of squares**

To calculate the F-value, you need to calculate the ratio between the
variance between groups and the variance within groups. Furthermore, to
calculate the variance (i.e. mean of squares), you first have to
calculate the sum of squares.

Now, remember that the working memory experiment investigates the
relationship between the change in IQ and the number of training
sessions. Calculate the between group sum of squares for the data from
this experiment.

``` r
# Define number of subjects in each group
n <- 20

# Calculate group means
Y_j <- as.numeric(tapply(wm$iq, wm$condition, mean))
Y_j

# Calculate the grand mean
Y_T <- mean(wm$iq)
Y_T

# Calculate the sum of squares
SS_A <- sum((Y_j - Y_T)^2) * n
SS_A
```

**Within groups sum of squares**

To calculate the F-value, you also need the variance within groups.
Similar to the last exercise, we'll start by computing the within groups
sum of squares.

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

**Calculating the F-ratio**&lt;

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

# Look at the summary table of the result
summary(anova.wm)
```

F-value is significant.

**Levene's test**

The assumptions of ANOVA are relatively simple. Similar to an
independent t-test, we have a continuous dependent variable, which we
assume to be normally distributed. Furthermore, we assume homogeneity of
variance, which can be tested with Levene's test.

``` r
library(car)

# Levene's test
leveneTest(wm$iq ~ wm$condition)

# Levene's test with the change for the default, namely center = mean
leveneTest(wm$iq ~ wm$condition, center = mean)
```

The assumption of homogeneity of variance hold: the within group
variance equivalent for all groups.

2, Post-hoc analysis
--------------------

Post-hoc tests help finding out which groups differ significantly from
one other and which do not. More formally, post-hoc tests allow for
multiple pairwise comparisons without inflating the type I error.

What does it mean to inflate the type I error?

Suppose the post-hoc test involves performing three pairwise
comparisons, each with the probability of a type I error set at 5%. The
probability of making at least one type I error is then equal to:

1 − (*n**o* : *t**y**p**e* : *I* : *e**r**r**o**r* : × : *n**o* : *t**y**p**e* : *I* : *e**r**r**o**r* : × : *n**o* : *t**y**p**e* : *I* : *e**r**r**o**r*)

If, for simplicity, you assume independence of these three events, the
maximum familywise error rate becomes:

1 − (0.95 × 0.95 × 0.95)=14.26%

In other words, the probability of having at least one false alarm (i.e.
type I error) is 14.26%.

What is the maximum familywise error rate for the working memory
experiment, assuming that you do all possible pairwise comparisons with
a type I error of 5%? 26.49%.

Null Hypothesis Significance Testing (NHST) is a statistical method used
to test whether or not you are able to reject or retain the null
hypothesis. This type of test can confront you with a type I error. This
happens when the test rejects the null hypothesis, while it is actually
true in reality. Furthermore, the test can also deliver a type II error.
This is the failure to reject a null hypothesis when it is false. All
hypothesis tests have a probability of making type I and II errors.

Sensitivity and specificity are two concepts that statisticians use to
measure the performance of a statistical test. The sensitivity of a test
is its true positive rate:

$$\\mathrm{sensitivity} = \\frac{\\mathrm{number\\ of\\ true\\ positives}}{\\mathrm{number\\ of\\ true\\ positives} + \\mathrm{number\\ of\\ false\\ negatives}}$$

The specificity of a test is its true negative rate:

$$\\mathrm{specificity} = \\frac{\\mathrm{number\\ of\\ true\\ negatives}}{\\mathrm{number\\ of\\ true\\ negatives} + \\mathrm{number\\ of\\ false\\ positives}}$$

Calculate both the sensitivity and specificity of the test based on
numbers displayed in the NHST table?

![](img/A_Hands-on_Introduction_to_Statistics_with_R,_Course_Three,_Analysis_of_Variance/NHST_1.png)

The sensitivity is 0.89 and the specificity is 0.85.

**Calculate and interpret the results of Tukey**

In a situation were you do multiple pairwise comparisons, the
probability of type I errors in the process inflates substantially.
Therefore, it is better to build in adjustments to take this into
account. This is what Tukey tests and other post-hoc procedures do. They
adjust the p-value to prevent inflation of the type I error rate.Use
Tukey's procedure.

``` r
# Read in the data set and assign to the object
wm <- readWorksheetFromFile('A Hands-on Introduction to Statistics with R.xls', sheet = 'wm', header = TRUE, startCol = 1, startRow = 1)

# This will print the data set in the console
head(wm)
```

``` r
# Revision: Analysis of variance
anova_wm <- aov(wm$gain ~ wm$cond)

# Summary Analysis of Variance
summary(anova_wm)

# Post-hoc (Tukey)
TukeyHSD(anova_wm)

# Plot confidence intervals
#plot(c(TukeyHSD(anova_wm)$lwr,TukeyHSD(anova_wm)$upr))
plot(TukeyHSD(anova_wm))
```

**Bonferroni adjusted p-values**

Just like Tukey's procedure, the Bonferroni correction is a method that
is used to counteract the problem of inflated type I errors while
engaging in multiple pairwise comparisons between subgroups. Bonferroni
is generally known as the most conservative method to control the
familywise error rate.

Bonferroni is based on the idea that if you test *N* dependent or
independent hypotheses, one way of maintaining the familywise error rate
is to test each individual hypothesis at a statistical significance
level that is deflated by a factor of $\\frac{1}{n}$. So, for a
significance level for the whole family of tests of *α*, the Bonferroni
correction would be to test each of the individual tests at a
significance level of $\\frac{\\alpha}{n}$.

The Bonferroni correction is controversial. It is a strict measure to
limit false positives and generates conservative p-value. Alternative:
increase the sample size, compute the false discovery rate (the expected
percent of false predictions in the set of predictions. For example if
the algorithm returns 100 results with a false discovery rate of .3 then
we should expect 70 of them to be correct.), and the Holm-Bonferroni
method.

``` r
# Use `p.adjust` 
bonferroni_ex <- p.adjust(.005, method='bonferroni', n = 8)

bonferroni_ex

# Pairwise T-test
pairwise.t.test(wm$gain,wm$cond, p.adjust = 'bonferroni')
```

3, Between groups factorial ANOVA
---------------------------------

**Data exploration with a barplot**

We'll use in this chapter is a randomized controlled experiment
investigating the effects of talking on a cell phone while driving. The
dependent variable in the experiment is the number of driving errors
that subjects made in a driving simulator. There are two independent
variables:

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

Each of the subjects was randomly assigned to one of six conditions
formed by combining different values of the independent variables.

-   `subject`: unique identifier for each subject conversation: level of
    conversation difficulty.
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

The driving errors made during different driving conditions are
influenced by the level of conversation demand. In other words, the
driving conditions have a different effect on the number of errors made,
depending on the level of conversation demand.

**The homogeneity of variance assumption**

Before we do factorial ANOVA, we need to test the homogeneity of the
variance assumption. When studying one-way ANOVA, we tested this
assumption with the `leveneTest` function.

We now have two independent variables instead of just one.

``` r
# Test the homogeneity of variance assumption
leveneTest(ab$errors ~ ab$conversation * ab$driving)
```

The homogeneity of variance assumption holds.

By performing a `leveneTest`, we can check whether or not the
homogeneity of variance assumption holds for a given dataset. The
assumption must hold for the results of an ANOVA analysis to be valid.

Recall from the first chapter that ANOVA makes use of F-statistics, or
F-ratios, in which two types of degrees of freedom are involved.

``` r
dim(ab)
str(ab$conversation)
str(ab$driving)
```

There are 120 subjets and 2 (Easy, Difficult) \* 3 (High Demand, Low
Demand, None) = 6 groups.

**The factorial ANOVA**

``` r
# Factorial ANOVA
ab_model <- aov(ab$errors ~ ab$conversation * ab$driving)

# Get the summary table
summary(ab_model)
```

Based on the summary table of the factorial ANOVA, the main effect for
driving difficulty, the main effect for conversation difficulty and the
interaction effect are all significant.

**The interaction effect**

Now it's time to explore the interaction effect. You will do this with
the help of a simple effects analysis.

Why a simple effects analysis? Well, remember what we had to do when you
had a significant main effect in a one-way ANOVA? There, you just had to
perform some post-hoc tests to see from which level of the categorical
variable the main effect was coming. With an interaction effect, it is
quite similar. Conduct a simple effects analysis of the variable
`conversation` on the outcome variable `errors` at each level of
`driving`.

``` r
# Create the two subsets
ab_1 <- subset(ab, ab$driving == 'Easy')  
ab_2 <- subset(ab, ab$driving == 'Difficult')

# Perform the one-way ANOVA for both subsets
aov_ab_1 <- aov(ab_1$errors ~ ab_1$conversation)
aov_ab_2 <- aov(ab_2$errors ~ ab_2$conversation)

# Get the summary tables for both aov_ab_1 and aov_ab_2
summary(aov_ab_1)
summary(aov_ab_2)
```

There a significant simple effect for the easy driving condition based
on the summary table of `aov_ab_1`.

There a significant simple effect for the difficult driving condition
based on the summary table of `aov_ab_2`.

**The effect sizes**

The definition of an interaction effect states that the effect of one
variable changes across levels of the other variable. For example, we
might expect the effect of conversation to be greater when driving
conditions are difficult than when they are relatively easy.

Unfortunately, it is not quite that simple. In order to really
understand the different effect sizes, you should make use of the
`etaSquared` function.

``` r
library(lsr)

# Calculate the etaSquared for the easy driving case
#easy is ab_1
#difficult is ab_2
etaSquared(aov_ab_1, anova = TRUE)

# Calculate the etaSquared for the difficult driving case
etaSquared(aov_ab_2, anova = TRUE)
```

Based on the output of the `etaSquared` function for the easy driving
condition, the percentage of variance explained by the conversation
variable is 14.7%; the percentage of variance explained by the
conversation variable is 57.8%.

**Pairwise comparisons**

Finally, let us look at pairwise comparisons for the simple effects. You
can do this with the Tukey post-hoc test.

``` r
# Tukey for easy driving
TukeyHSD(aov_ab_1)

# Tukey for difficult driving
TukeyHSD(aov_ab_2)
```

For 'Easy Driving', two mean differences in terms of number of errors
are significant (below 0.05).

For 'Difficult Driving', three mean differences in terms of number of
errors are significant (below 0.05).
