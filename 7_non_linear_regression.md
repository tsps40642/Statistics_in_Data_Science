NON-LINEAR REGRESSION
================

# Polynomial Regression

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
ff <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/FastFood.xlsx')

f_fit <- lm(Revenue ~ Income + IncomeSqd, data = ff)
summary(f_fit)
```

    ## 
    ## Call:
    ## lm(formula = Revenue ~ Income + IncomeSqd, data = ff)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -105.452  -44.967    8.613   41.906  104.164 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -1454.521    279.993  -5.195 3.29e-05 ***
    ## Income        209.815     24.084   8.712 1.39e-08 ***
    ## IncomeSqd      -4.170      0.504  -8.275 3.36e-08 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 60.31 on 22 degrees of freedom
    ## Multiple R-squared:  0.8029, Adjusted R-squared:  0.785 
    ## F-statistic: 44.82 on 2 and 22 DF,  p-value: 1.74e-08

``` r
# min(ff$Income) = 15.6, not containing 0
# confint(f_fit, interval = "confidence", level = 0.95)
```

## Interpretation

### β0, βi

Revenue = β0 + β1*Income + β2*(Income^2) + ε

β0:  
It’s the mean of revenue when income is 0,  
yet min(ff\$Income) is 15.6, and 0 is not in the range of the dataset,  
and no revenue could be -1454.521 when income is 0.  
So β0 has no interpretation in the problem context here.

β1, β2:  
Values of β1_hat and β2_hat are not interpretable since they are
mathematical connected;  
we cannot change either of the value with another being fixed.

### p-value for model as a whole (global T-test)

Global test, see p-value of 1.74e-08

H0: β1 = β2 = 0; no relationship  
H1: at least one is not 0; relationship

There is a chance of XX% that we would get a sample result as extreme as
the one we obtain if there’s no relationship between Y and any of
predictors.  
DON’T USE P-VALUE AS MODEL SELECTION SINCE IT GIVES THE COMPARISON OF K
PREDICTORS TO NONE

### p-value for individual predictor (individual T-test)

Determine curvilinear relationship, see p-value of 3.36e-08

H0: β2 = 0; no curvilinear relationship  
H1: β2 != 0; curvilinear relationship

(we don’t drop X while keep X^2, that’s why we won’t do test for β1 = 0
or not;  
we only do that for higher-power term)  
There is a probability of 3.36e-08 that we would get a sample result as
extreme as the one we obtained if there’s no curvilinear relationship
between Y and X(use the context).

CANNOT SAY HOLDING OTHER PREDICTORS BE FIXED since the strong
collinearity.

## Visualization

β1 and β2 same sign:  
β1 and β2 different sign: shift right

``` r
par(mfrow = c(1, 2))

# 
x <- c(1, 2, 3, 4, 5)
plot(3 + x, 3 - x*x + 2*x, type = "l", xlim = c(0, 10), ylim = c(0, 40))
abline(x, 2*x, lty = 2)

x <- c(1, 2, 3, 4, 5)
plot(3 + x, 3 + x*x + 2*x, type = "l", xlim = c(0, 10), ylim = c(0, 40))
abline(x, 2*x, lty = 2)
```

![](7_non_linear_regression_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->
