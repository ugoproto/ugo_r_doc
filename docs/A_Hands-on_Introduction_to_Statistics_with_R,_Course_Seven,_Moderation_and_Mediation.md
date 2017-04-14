-   [1, An introduction to moderation](#an-introduction-to-moderation)
-   [2, An introduction to centering
    predictors](#an-introduction-to-centering-predictors)
-   [3, An introduction to mediation](#an-introduction-to-mediation)

------------------------------------------------------------------------

**Foreword**

-   Output options: the 'tango' syntax and the 'readable' theme.
-   Snippets only.

------------------------------------------------------------------------

1, An introduction to moderation
--------------------------------

**Data exploration**

``` r
library(psych)

# Summary statistics
describeBy(mod, mod$condition)

# Create a boxplot of the data
boxplot(mod$iq ~ mod$condition, main = 'Boxplot', ylab = 'IQ', xlab = 'Group condition')
```

**Calculate correlations**

``` r
# Create subsets of the three groups

# Make the subset for the group condition = 'control'
mod_control <- subset(mod, condition == 'control')

# Make the subset for the group condition = 'threat1'
mod_threat1 <- subset(mod, condition == 'threat1')

# Make the subset for the group condition = 'threat2'
mod_threat2 <- subset(mod, condition == 'threat2')

# Calculate the correlations
cor(mod_control$iq, mod_control$wm, method = 'pearson')

cor(mod_threat1$iq, mod_threat1$wm, method = 'pearson')

cor(mod_threat2$iq, mod_threat2$wm, method = 'pearson')
```

**Model with and without moderation**

A moderator variable (Z) will enhance a regression model if the
relationship between X and Y varies as a function of Z.

Experimental research

-   The manipulation of an X causes change in a Y.
-   A moderator variable (Z) implies that the effect of the X on the Y
    is NOT consistent across the distribution of Z.

Correlational research

-   Assume a correlation between X and Y.
-   A moderator variable (Z) implies that the correlation between X and
    Y is NOT consistent across the distribution of Z.

If both X and Z are continuous:

*Y* = *β*<sub>0</sub> + *β*<sub>1</sub>*X* + *β*<sub>2</sub>*Z* + *β*<sub>3</sub>(*X* \* *Z*)+*ϵ*

If X is categorical and Z is continuous:

*Y* = *β*<sub>0</sub> + *β*<sub>1</sub>(*D*<sub>1</sub>)+*β*<sub>2</sub>(*D*<sub>2</sub>)+*β*<sub>3</sub>*Z* + *β*<sub>4</sub>(*D*<sub>1</sub> \* *Z*)+*β*<sub>5</sub>(*D*<sub>2</sub> \* *Z*)+*ϵ*

Consult the PDF for performing tests.

``` r
# Model without moderation (tests for 'first-order effects')
model_1 <- lm(mod$iq ~ mod$wm + mod$d1 + mod$d2)

 # Make a summary of model_1
summary(model_1)

# Create new predictor variables
wm_d1 <- mod$wm * mod$d1
wm_d2 <- mod$wm * mod$d2

# Model with moderation
model_2 <- lm(mod$iq ~ mod$wm + mod$d1 + mod$d2 + wm_d1 + wm_d2)

# Make a summary of model_2
summary(model_2)
```

**Model comparison**

``` r
# Compare model_1 and model_2
anova(model_1, model_2)
```

**Scatterplot**

``` r
# Choose colors to represent the points by group
color <- c('red', 'green', 'blue')

# Illustration of the first-order effects of working memory on IQ
ggplot(mod, aes(x = wm, y = iq)) + geom_smooth(method = 'lm', color = 'black') + 
  geom_point(aes(color = condition))

# Illustration of the moderation effect of working memory on IQ
ggplot(mod, aes(x = wm, y = iq)) + geom_smooth(aes(group = condition), method = 'lm', se = T, color = 'black', fullrange = T) + geom_point(aes(color = condition))
```

**Centering data**

``` r
# Define wm_center
wm_center <- mod$wm - mean(mod$wm)

# Compare with the variable wm.centered
all.equal(wm_center, mod$wm.centered)
```

2, An introduction to centering predictors
------------------------------------------

**Centering versus no centering**

To center means to put in deviation form: *X*<sub>*C*</sub> = *X* − *M*.
Convert raw scores to deviation scores.

Two reason:

-   Conceptual: regression constant will be more meaningful.
-   Statistical: avoid multicolinearity.

Conceptual.

-   The intercept, *β*<sub>0</sub>, is the predicted score on Y (child's
    verbal ability) when all predictors (X = mother's vocabulary, Z =
    child's age) are zero.
-   If X = zero or Z = zero is meaningless, or impossible, then
    *β*<sub>0</sub> will be difficult to interpret.
-   In contrast, if X = zero and Z = zero, are the average then
    *β*<sub>0</sub> is easy to interpret.
-   The regression coefficient *β*<sub>1</sub> is the slope for X
    assuming an average score on Z.
-   No moderation effect implies that *β*<sub>1</sub> is consistent
    across the entire distribution of Z.
-   In contrast, a moderation effect implies that *β*<sub>1</sub> is NOT
    consistent across the entire distribution of Z.
-   Where in the distribution of Z is *β*<sub>1</sub> most
    representative of the relationship between X & Y?

Statistical

-   The predictors, X and Z, can become highly correlated with the
    product, (X\*Z).
-   Multicolinearity: when two predictor variables in a GLM are so
    highly correlated that they are essentially redundant and it becomes
    difficult to estimate *β* values associated with each predictor.

<!-- -->

``` r
# Model without moderation and with centered data
model_1_centered <- lm(mod$iq ~ mod$wm.centered + mod$d1 + mod$d2)

# Make a summary of model_1_centered
summary(model_1_centered)
```

**Centering versus no centering with moderation**

``` r
# Create new predictor variables
wm_d1_centered <- mod$wm.centered * mod$d1
wm_d2_centered <- mod$wm.centered * mod$d2

# Define model_2_centered
model_2_centered <- lm(mod$iq ~ mod$wm.centered + mod$d1 + mod$d2 + wm_d1_centered + wm_d2_centered)

# Make a summary of model_2_centered
summary(model_2_centered)
```

**Model comparison**

``` r
# Compare model_1_centered and model_2_centered
anova(model_1_centered, model_2_centered)

# Compare model_1 and model_2
anova(model_1, model_2)
```

**Some correlations**

``` r
# Calculate the correlations between working memory capacity and the product terms
 cor_wmd1 <- cor(mod$wm, wm_d1)
 cor_wmd2 <- cor(mod$wm, wm_d2)
 cor_wmd1_centered <- cor(mod$wm.centered, wm_d1_centered)
 cor_wmd2_centered <- cor(mod$wm.centered, wm_d2_centered)
 
# Calculate the correlations between the dummy variables and the product terms
 cor_d1d1<- cor(mod$d1, wm_d1)
 cor_d2d2 <- cor(mod$d2, wm_d2)
 cor_d1d1_centered <- cor(mod$d1, wm_d1_centered)
 cor_d2d2_centered <- cor(mod$d2, wm_d2_centered)
 
# correlations
 rbind(c(cor_wmd1, cor_wmd2), c(cor_wmd1_centered, cor_wmd2_centered))
 rbind(c(cor_d1d1, cor_d2d2), c(cor_d1d1_centered, cor_d2d2_centered))
```

3, An introduction to mediation
-------------------------------

**Model with and without mediation**

-   X: Experimental manipulation (Stereotype threat).
-   Y: Behavioral outcome (IQ score).
-   M: Mediator (Mechanism = Working memory capacity).

A mediation analysis is typically conducted to better understand an
observed effect of a correlation between X and Y. Why, and how, does
stereotype threat influence IQ test performance?

If X and Y are correlated then we can use regression to predict Y from X

*Y* = *β*<sub>0</sub> + *β*<sub>1</sub>*X* + *ϵ*

If X and Y are correlated BECAUSE of the mediator M, then (X à M à Y):

*Y* = *β*<sub>0</sub> + *β*<sub>1</sub>*M* + *ϵ*

*M* = *β*<sub>0</sub> + *β*<sub>1</sub>*X* + *ϵ*

If X and Y are correlated BECAUSE of the mediator M, and:

*Y* = *β*<sub>0</sub> + *β*<sub>1</sub>*M* + *β*<sub>2</sub>*X* + *ϵ*

A mediator variable (M) accounts for some (partial) or all of the
relationship between X and Y.

CAUTION:

-   Correlation does not imply causation!
-   In other words, there is a BIG difference between statistical
    mediation and true causal mediation.

**Data exploration**

``` r
# Summary statistics
describeBy(med, med$condition)

# Create a boxplot of the data
boxplot(med$iq ~ med$condition, main = 'Boxplot', xlab = 'Group condition', ylab = 'IQ')
```

**Run 3 regression models on the data**

``` r
# Run the three regression models
# outcome ~ predictor
model_yx <- lm(med$iq ~ med$condition)

# mediator ~ predictor
model_mx <- lm(med$wm ~ med$condition)

# outcome ~ predictor + mediator
model_yxm <- lm(med$iq ~ med$condition + med$wm)

# Make a summary of the three models
summary(model_yx)
summary(model_mx)
summary(model_yxm)
```

**Sobel test**

``` r
# Compare the previous results to the output of the sobel function

# sobel(pred,med,out)
model_all <- sobel(med$condition, med$wm, med$iq)
model_all
```

The Sobel test is a method of testing the significance of a mediation
effect. Consult the PDF.
