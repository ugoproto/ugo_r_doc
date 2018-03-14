<!--
-   [1, An introduction to repeated measures](#an-introduction-to-repeated-measures)
-   [2, Repeated measures ANOVA](#repeated-measures-anova)
-->
------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

An introduction to repeated measures
====================================

The independent t-test is analogous to between-groups ANOVA and the paired-sample t-test is analogous to repeated measures ANOVA.

In a between-groups design, each subject is exposed to two or more treatments or conditions over time. In a within-subjects design, each subject is allocated to exactly one treatment or condition.

Between:

-   Experiment 1: You want to test the effect of alcohol on test scores of students. There are three conditions: the student consumed no alcohol, two glasses of beer, or five glasses of beer. Alcohol tolerance and time spent studying should also be considered somehow.
-   Experiment 2: You want to investigate the effects of certain fertilizers on plant growth. Assume you have two different
    fertilizers, A and B. Consider three conditions: you give the plant no fertilizer, fertilizer A, or fertilizer B. You measure the height of the plant after a specific period of time to see whether the fertilizers had an effect.

Use a within-subjects design for for Experiment 1 and between-groups design for Experiment 2.

Is it always either manipulation between-groups or manipulation within-groups, or are there experiments where you could use either approach? In some cases, either approach is possible.

**Explore the working memory data**

``` r
# Print the data set in the console
str(wm)
```

    ## 'data.frame':    80 obs. of  3 variables:
    ##  $ subject  : num  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ condition: Factor w/ 4 levels "12 days","17 days",..: 4 4 4 4 4 4 4 4 4 4 ...
    ##  $ iq       : num  12.4 11.8 14.6 7.7 15.7 11.6 7 8.4 10.7 10.6 ...

``` r
library(psych)
library(ggplot2)

# Define the variable subject as a categorical variable
wm$subject <- factor(wm$subject)

# Summary statistics by all groups (8 sessions, 12 sessions, 17 sessions, 19 sessions)
describeBy(wm, wm$condition)
```

    ## 
    ##  Descriptive statistics by group 
    ## group: 12 days
    ##            vars  n mean   sd median trimmed  mad min  max range skew
    ## subject*      1 20 10.5 5.92  10.50   10.50 7.41 1.0 20.0  19.0 0.00
    ## condition*    2 20  1.0 0.00   1.00    1.00 0.00 1.0  1.0   0.0  NaN
    ## iq            3 20 11.7 2.58  11.65   11.69 2.89 6.9 16.1   9.2 0.05
    ##            kurtosis   se
    ## subject*      -1.38 1.32
    ## condition*      NaN 0.00
    ## iq            -1.06 0.58
    ## -------------------------------------------------------- 
    ## group: 17 days
    ##            vars  n mean   sd median trimmed  mad min  max range skew
    ## subject*      1 20 10.5 5.92   10.5    10.5 7.41 1.0 20.0  19.0 0.00
    ## condition*    2 20  2.0 0.00    2.0     2.0 0.00 2.0  2.0   0.0  NaN
    ## iq            3 20 13.9 2.26   13.6    13.9 2.00 9.8 18.1   8.3 0.11
    ##            kurtosis   se
    ## subject*      -1.38 1.32
    ## condition*      NaN 0.00
    ## iq            -0.85 0.50
    ## -------------------------------------------------------- 
    ## group: 19 days
    ##            vars  n  mean   sd median trimmed  mad  min  max range  skew
    ## subject*      1 20 10.50 5.92   10.5   10.50 7.41  1.0 20.0  19.0  0.00
    ## condition*    2 20  3.00 0.00    3.0    3.00 0.00  3.0  3.0   0.0   NaN
    ## iq            3 20 14.75 2.50   15.3   14.71 2.15 10.4 19.2   8.8 -0.09
    ##            kurtosis   se
    ## subject*      -1.38 1.32
    ## condition*      NaN 0.00
    ## iq            -0.99 0.56
    ## -------------------------------------------------------- 
    ## group: 8 days
    ##            vars  n  mean   sd median trimmed  mad min  max range  skew
    ## subject*      1 20 10.50 5.92   10.5   10.50 7.41 1.0 20.0  19.0  0.00
    ## condition*    2 20  4.00 0.00    4.0    4.00 0.00 4.0  4.0   0.0   NaN
    ## iq            3 20 10.91 2.63   11.3   10.97 2.67 5.4 15.7  10.3 -0.21
    ##            kurtosis   se
    ## subject*      -1.38 1.32
    ## condition*      NaN 0.00
    ## iq            -0.70 0.59

``` r
# Boxplot IQ versus condition
boxplot(wm$iq ~ wm$condition, main = 'Boxplot', xlab = 'Training sessions', ylab = 'IQ')
```

<center>![](img/A_Hands-on_Introduction_to_Statistics_with_R,_Course_Four,_Repeated_Measures_Anova/unnamed-chunk-3-1.png)</center>

``` r
# Illustration data, each line represents the development of each subject by number of trainings
ggplot(data = wm, aes(x = wm$condition, y = wm$iq, group = wm$subject, colour = wm$subject)) + geom_line() + geom_point()
```

<center>![](img/A_Hands-on_Introduction_to_Statistics_with_R,_Course_Four,_Repeated_Measures_Anova/unnamed-chunk-3-2.png)</center>

**Reduced cost**

The cost advantage of using manipulation within groups verses manipulation between groups for the working memory experiment is you need 60 subjects fewer.

**Statistically more powerful**

Repeated measures analysis accounts for individual differences across the experiment. This reduces the error term, which  increases statistical power.

**Counterbalancing**

Suppose you have three levels of an independent variable A (i.e. A1, A2, A3) and a blocked design. You want to use full counterbalancing to take into account order effects.

What are all the possible orders that you need to use? In other words, what are the order conditions?

(A1, A2, A3), (A1, A3, A2) , (A2, A1, A3), (A2, A3, A1) , (A3, A2, A1),
(A3, A1, A2)

**Number of order conditions?**

Assume the number of levels of the independent variable goes up and you want to completely counterbalance. What will happen to the number of order conditions you'll need?

The number becomes really large. An independent variable with n levels will have `n!=n*(n???1)*(n???2)*...` order conditions.

**Latin Squares design**

As you hopefully realized in the previous exercise, completely counterbalancing is not always a practical solution for taking into account order effects. This is because the number of different orders required gets really large as the number of possible conditions increases.

The most common workaround to this problem is the Latin Squares design, in which you do not completely counterbalance, but instead put each condition at every position (at least) once.

Which of the following examples has been constructed according to the Latin Squares design?

(A1, A2, A3), (A2, A3, A1), (A3, A1, A2)

**More on Latin Squares**

The number of order conditions is always equal to the number of levels of your independent variable.

**Why is missing data a problem?**

In a between-groups design, it is okay to have a slightly different number of subjects in each group, so if one subject drops out in one of the conditions then that group has just one less subject. Now you want to look at how subjects change over time and the different scores between two or more conditions for each subject.

**Understanding sphericity**

The variances of the differences between all possible pairs of groups (i.e. levels of the independent variable) are equal.

**Mauchly's test**

``` r
# Define iq as a data frame where each column represents a condition
iq <- cbind(wm$iq[wm$condition == '8 days'], wm$iq[wm$condition == '12 days'], wm$iq[wm$condition == '17 days'], wm$iq[wm$condition == '19 days'])

# Make an mlm object
mlm <- lm(iq ~ 1)

# Mauchly's test
mauchly.test(mlm, x = ~ 1)
```

    ## 
    ##  Mauchly's test of sphericity
    ## 
    ## data:  SSD matrix from lm(formula = iq ~ 1)
    ## W = 0.81725, p-value = 0.9407

Based on the results, the sphericity assumption holds.

**Pros of repeated measures**

Less cost and statistically more powerful

**Cons of repeated measures**

Order effects, counterbalancing, missing data, and an extra assumption

Repeated measures ANOVA
=======================

**The systematic between groups variance**

To understand everything a bit better, we will calculate the F-ratio for a repeated measures design by ourself in the next exercises.

First, we will need the systematic between-groups variance. This is the same as in the between-groups design--the variance due to grouping by condition.

``` r
# Define number of subjects for each condition
n <- 20

# Calculate group means
y_j <- tapply(wm$iq, wm$condition, mean)

# Calculate the grand mean
y_t <- mean(y_j)

# Calculate the sum of squares
ss_cond <- sum((y_j - y_t)^2)*n

# Define the degrees of freedom for conditions
df <- (4 - 1)

# Calculate the mean squares (variance)
ms_cond <- ss_cond/df
```

**The subject variance**

We will also need the error term of the repeated measures design. This can be calculated in a few steps.

-   First calculate the systematic variance due to subjects.
-   Below, we will calculate the unsystematic variance, like we did with the between-groups design.
-   If we subtract these two results, we will get the error term of the repeated measures design.

The systematic (stable) subject variance will be taken out of the error term, so the error term is reduced in comparison with the between-groups design.

``` r
# Define number of conditions for each subject
n <- 4

# Calculate subject means
y_j <- tapply(wm$iq, wm$subject, mean)

# Calculate the grand mean
y_t <- mean(y_j)

# Calculate the sum of squares
ss_subjects <- sum((y_j - y_t)^2)*n

# Define the degrees of freedom for subjects
df <- (20 - 1)

# Calculate the mean squares (variance)
ms_subjects <- ss_subjects/df
```

**The unsystematic within groups variance**

To calculate the error term of the repeated measures design, we need the unsystematic within-groups variance: the unsystematic variance or the error term of the between-groups design.

``` r
# Create four subsets of the four groups, containing the IQ results
    # Make the subset for the group condition = "8 days"
y_i1 <- subset(wm$iq, wm$condition == "8 days")
    # Make the subset for the group condition = "12 days"
y_i2 <- subset(wm$iq, wm$condition == "12 days")
    # Make the subset for the group condition = "17 days"
y_i3 <- subset(wm$iq, wm$condition == "17 days")
    # Make the subset for the group condition = "19 days"
y_i4 <- subset(wm$iq, wm$condition == "19 days")

# Subtract the individual values by their group means
s_1 <- y_i1 - mean(y_i1)
s_2 <- y_i2 - mean(y_i2)
s_3 <- y_i3 - mean(y_i3)
s_4 <- y_i4 - mean(y_i4)

# Put everything back into one vector
s_t <- c(s_1, s_2, s_3, s_4)

# Calculate the within sum of squares by using the vector s_t
ss_sa <- sum(s_t^2)

# Define the degrees of freedom
df <- 4*(20-1)

# Calculate the mean squares (variances)
ms_sa <- ss_sa/df
```

**The unsystematic variance for the repeated measures design**

Now we can easily calculate the unsystematic variance for the repeated measures design, also called the error term.

``` r
# ss_sa = ss_subjects + ss_rm
ss_rm <- ss_sa - ss_subjects

# Define the degrees of freedom
df <- (20 - 1)*(4 - 1)

# Calculate the mean squares (variances)
ms_rm <- ss_rm/df
```

Now we've calculated the error term of the repeated measures design, which is clearly smaller than the error term of the between-groups design because you reduced this one. To complete you can now calculate the F-ratio and the corresponding p-value.

**F-ratio and p-value**

To do the ANOVA analysis we actually need the F-ratio and the corresponding p-value.

``` r
# Calculate the F-ratio
f_rat <- ms_cond/ms_rm

# Define the degrees of freedom of the F-distribution
# df1 for freedom of conditions (ss_cond)
# df2 for (freedom of) conditions and subjects (sa_sa)
df1 <- (4 - 1)
df2 <- (4 - 1)*(20 - 1)

# Calculate the p-value
p <- 1 - pf(f_rat, df1, df2)
```

**Error term in a repeated measures design?**

The inconsistent individual differences across conditions, the effect of subjects that differ across conditions. So it is an interaction between subjects and condition.

**Anova in R**

ANOVA in R is usually done with the `aov` function.

``` r
# anova model
model <- aov(wm$iq ~ wm$condition + Error(wm$subject / wm$condition))

# summary model
summary(model)
```

    ## 
    ## Error: wm$subject
    ##           Df Sum Sq Mean Sq F value Pr(>F)
    ## Residuals 19  175.6   9.242               
    ## 
    ## Error: wm$subject:wm$condition
    ##              Df Sum Sq Mean Sq F value   Pr(>F)    
    ## wm$condition  3  196.1   65.36   12.51 2.16e-06 ***
    ## Residuals    57  297.8    5.22                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

The F-ratio is significant. Therefore, the number of training days does affect the IQ scores.

**Effect size**

Calculate eta-squared, which helps you estimate effect size.

``` r
# Define the total sum of squares

# ss_cond (syst. between groups or of the effect), 
# ss_sa (unsyst. within groups) =
# ss_rm (unsyst. for repeated measures design) + # ss_subjects (ok)
ss_total <- ss_cond + ss_rm

# Calculate the effect size
eta_sq <- ss_cond / ss_total
```

**Post-hoc test one**

We will now look at a few different procedures for the post-hoc test. Let's start with the Holm procedure.

``` r
# Post-hoc test: default procedure
with(wm, pairwise.t.test(iq, condition, paired = T))
```

    ## 
    ##  Pairwise comparisons using paired t tests 
    ## 
    ## data:  iq and condition 
    ## 
    ##         12 days 17 days 19 days
    ## 17 days 0.01955 -       -      
    ## 19 days 0.00270 0.40038 -      
    ## 8 days  0.40038 0.00244 0.00054
    ## 
    ## P value adjustment method: holm

We get a table with some values as the output for the post-hoc test and a line saying that we have used the Holm procedure. What are the values in the table of the output? p-values.

Recall that the hypotheses are tested at a 5% significance level.

We can conclude that all pairwise comparisons are significant, except for the comparison between 19 and 17 and the comparison between 12 and 8.

**Post-hoc test: Bonferroni**

Now we'll take a look at the most conservative procedure, Bonferroni. This procedure will apply the most extreme adjustments to the p-values.

``` r
# Post-hoc test: Bonferroni procedure
with(wm, pairwise.t.test(iq, condition, paired = TRUE, p.adjust.method = 'bonferroni'))
```

    ## 
    ##  Pairwise comparisons using paired t tests 
    ## 
    ## data:  iq and condition 
    ## 
    ##         12 days 17 days 19 days
    ## 17 days 0.03910 -       -      
    ## 19 days 0.00405 1.00000 -      
    ## 8 days  1.00000 0.00293 0.00054
    ## 
    ## P value adjustment method: bonferroni

Notice the change in p-values in comparison with the previous procedure.

**Paired t-test**

Assume that you do not know how to perform an analysis of variance (ANOVA), we may do a number of paired t-tests instead.

Have a look at just one paired t-test and take, for example, the comparison between 12 days and 17 days.

``` r
# Define two subsets containing the IQ scores for the condition group '12 days' and '17 days'
cond_12days <- subset(wm, condition == '12 days')$iq
cond_17days <- subset(wm, condition == '17 days')$iq
cond_12days
```

    ##  [1] 12.5 11.6  8.9  8.3 10.9 13.4 12.3  8.7  9.7  9.7  6.9 10.5 11.6 13.8
    ## [15] 15.6 11.7 16.1 11.7 15.0 15.1

``` r
# t-test
t.test(cond_12days, cond_17days, paired = TRUE)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  cond_12days and cond_17days
    ## t = -3.0549, df = 19, p-value = 0.006517
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -3.7157116 -0.6942884
    ## sample estimates:
    ## mean of the differences 
    ##                  -2.205

It is clear that we can apply different procedures for post-hoc tests. These procedures differ with respect to how they handle inflation of the possibility of a type I error and will therefore give us different p-values. However, these p-values will always be in a certain range.

What is the (smallest) range of p-values for the comparison between 12 days and 17 days? Look at the results from applying the Bonferroni procedure as well as the paired t-test.

p-values:

-   Bonferroni 12 days-17 days: 0.03910
-   Paired t-test (cond\_12days, cond\_17days): 0.006517

Therefore: \[0.0065, 0.0391\].
