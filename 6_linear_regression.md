LINEAR REGRESSION
================

# Linear model

## Assumptions:

1.  random sampling  
2.  stability over time  
3.  εi ~ N(mean = 0, sd = σ) constant over all possible values of the
    predictors

### More detailed assupmtions of the lm

1.  The regression model is linear, correctly specified, and has an
    additive error term  
2.  The error term has a zero population mean  
3.  All explanatory variables are uncorrelated with the error term
4.  Observations of the error term are uncorrelated with each other (no
    serial correlation)  
5.  The error term has a constant variance i.e. no heteroskedasticity  
6.  No explanatory variable is a perfect linear function of any other
    explanatory variables, i.e. no perfect multicollinearity

## State the linear model

#### Explicitly state the “process model” (that is of interest to the companym, business, …)

Volume = β0 + β1 \* Income + ε  
1. Remember ε, which captures the uncertainty within the model  
2. Use the actual response variable name rather than solely Y

#### Explicitly identify the “fitted model”

Volume_hat = β0_hat + β1_hat \* Income, use estimated coefficient:
Volume_hat = 36.537 + 0.00218 \* Income  
1. Remember DON’T ADD ε  
2. Use the fitted coefficients rather than only typing β1_hat

#### We are interested in estimating Y given a set of predictors

Since Y = β0 + β1*X1 + ε, we take mean of this equation,  
- mean(β1*X1)=β1*X1 since β1 fixed,  
- and we want to identify μ_Y given a set of X_i, {X_i}, so X1 fixed as
well  
μ_Y\|{X_i} = β0 + β1*X1 + mean(ε) = β0 + β1\*X1 since mean(ε)=0,  
so Y = μ_Y\|{X_i} + ε = its mean on a given set of predictors +
uncertainty

#### Distribution of ε

For the process we assume that Y are normally scattered around the
regression line with a common sd, that is, centered at the regression
line with σ as sd for every value of predictors  
\>\> this is what the assumption of ε look like

## Interpretation - practical significance

#### Coefficient of determination, R-squared

1.  The square of correlation coefficient = SS_regression/SS_total  
2.  The proportion of the variability in Y that is accounted for by the
    linear relationship between X and Y  
3.  Range: \[0, 1\]

#### Correlation Ciefficient, R

1.  The (square root + sign) of the coefficient of determination  
2.  Used to check each pair of the variables to see whether collinearity
    exists
    - abs(R_ij) \<= 2: can interpret without reservation  
    - abs(R_ij) \< R \<= 7: interpret cautiously  
    - abs(R_ij) \> 7: do not interpret βi_hat and βj_hat (i!=j), whether
      drop one or not depends  
3.  Range: \[-1, 1\]

#### Standard error of the residuals, S_sub_ε_hat

S_sub_ε_hat = sqrt(sum of square error/df_error) = sqrt(\[Σ(i=1 to
n)(Y_i-Y_i_hat)<sup>2</sup>\]/(n-2))  
1. The standard deviation of εi_hat around the regression line  
= measure of unexplained variation of the response variable Y  
= measure of the residual variability (sd) around our model  
= sqrt(variance of the residuals)  
= sqrt(MSE)  
2. It’s in the unit of Y  
3. df_error = n-2, since we calculate β0_hat and β1_hat to get Y_hat

## Interpretation - statistical significance

#### p-value for model as a whole (global T-test)

1.  There is a chance of XX% that we would get a sample result as
    extreme as the one we obtain if there’s no relationship between Y
    and any predictors  
2.  DON’T USE P-VALUE AS MODEL SELECTION SINCE IT GIVES THE COMPARISON
    OF K PREDICTORS TO NONE

#### p-value for individual predictor (individual T-test)

1.  There is a chance of XX% that we would get a sample result as
    extreme as the one we obtain if there’s no relationship between Xi
    and Y, holding other predictors fixed

## β0, βi

Volume = β0 + β1 (Income) + ε

#### β0:

Purchase volume when the Income = 0, since this value of Income is well
outside the range of values (minimum Income = \$12,000 with s = \$6039),
β0 has no interpretation in the problem context.

Note that β0_hat is the value of μY_hat when all the {Xi} = 0  
we can interpret it only if the 2 conditions meet:  
1. it makes logical sense for all the {Xi} = 0  
2. {Xi} = 0 within the range of data

#### β1:

The increase in mean monthly purchase volume (in \$s) for every \$1
increase in family income for the process of Moose customers using their
charge card.  
(… “holding other predictors fixed” needed for multiple regression)

#### 85% CI for β1

85% of the time this procedure will give us an interval that contains
the true slope describing the linear relationship between LotSize(X) and
HouseValue(Y) as specified by the given model.

## ASA Statement: Principles

1.  p-values can indicate how incompatible the data are with a specific
    statistical model. \[H0\]  
2.  p-values do not measure the probability that the studied hypothesis
    is true, or the probability that the data were produced by random
    chance alone.  
3.  Scientific conclusions and business or policy decisions should not
    be based only on whether a p-value passes a specific threshold.  
4.  proper inference requires full reporting and transparency.  
5.  a p-value, or statistical significance, does not measure the size of
    an effect or the importance of a result.  
    by itself, a p-value does not provide a good measure of evidence
    regarding a model or hypothesis.

# Codes

## Visualization

Do the scatter plot w/ fitted linear regression line.

``` r
# plot(X_data, Y_data, type = "p"): scatter plot
# plot(X_data, Y_data, type = "l"): line plot

# I've adjusted some of the parameter in the functions but not big deal
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
ff <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/FastFood.xlsx')

plot(ff$Income, ff$Revenue, pch = 16, main = "Revenue ~ Income", xlab = "Income", ylab = "Revenue")
# regression line
abline(lm(Revenue ~ Income, data = ff), lty=2, lwd = 2, col="red") 
```

![](6_linear_regression_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

## Fit the model, model summary

``` r
f_fit <- lm(Revenue ~ Income, data = ff)
summary(f_fit)
```

    ## 
    ## Call:
    ## lm(formula = Revenue ~ Income, data = ff)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -338.86  -61.15    9.53   90.00  168.70 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  804.181    123.624   6.505 1.23e-06 ***
    ## Income        11.627      5.012   2.320   0.0296 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 119.6 on 23 degrees of freedom
    ## Multiple R-squared:  0.1896, Adjusted R-squared:  0.1544 
    ## F-statistic: 5.382 on 1 and 23 DF,  p-value: 0.02957

``` r
# Coefficients, β-hats
coefficients(f_fit)
```

    ## (Intercept)      Income 
    ##   804.18115    11.62723

``` r
# Coefficient of determination R-squared
summary(f_fit)$r.squared
```

    ## [1] 0.1896306

## Global T-test: for the whole model

H0: β1 = βj = … = βk = 0  
H1: at least one not 0

Assumptions:  
1. random sampling  
2. stability over time  
3. εi ~ N(mean = 0, sd = σ) constant (over all possible values of the
predictors)

``` r
# for simple lm, the F-statistic, p-value for global t-test are the same in anova(fitted_model) and summary(fitted_model)
f_fit <- lm(Revenue ~ Income, data = ff)
summary(f_fit)
```

    ## 
    ## Call:
    ## lm(formula = Revenue ~ Income, data = ff)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -338.86  -61.15    9.53   90.00  168.70 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  804.181    123.624   6.505 1.23e-06 ***
    ## Income        11.627      5.012   2.320   0.0296 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 119.6 on 23 degrees of freedom
    ## Multiple R-squared:  0.1896, Adjusted R-squared:  0.1544 
    ## F-statistic: 5.382 on 1 and 23 DF,  p-value: 0.02957

``` r
anova(f_fit)
```

    ## Analysis of Variance Table
    ## 
    ## Response: Revenue
    ##           Df Sum Sq Mean Sq F value  Pr(>F)  
    ## Income     1  77008   77008  5.3821 0.02957 *
    ## Residuals 23 329088   14308                  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# yet for multiple lm not the same, so we have to use summary(fitted_model) to check the result
f_fit_2 <- lm(Revenue ~ Income + IncomeSqd, data = ff)
summary(f_fit_2)
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
anova(f_fit_2)
```

    ## Analysis of Variance Table
    ## 
    ## Response: Revenue
    ##           Df Sum Sq Mean Sq F value    Pr(>F)    
    ## Income     1  77008   77008   21.17  0.000139 ***
    ## IncomeSqd  1 249062  249062   68.47 3.355e-08 ***
    ## Residuals 22  80026    3638                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

## Individual two-tailed t tests: for each of the variable

H0: βi = 0  
H1: βi != 0

Assumptions:  
1. random sampling  
2. stability over time  
3. εi ~ N(mean = 0, sd = σ) constant (over all possible values of the
predictors)

``` r
# along with other summary information
summary(f_fit)
```

    ## 
    ## Call:
    ## lm(formula = Revenue ~ Income, data = ff)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -338.86  -61.15    9.53   90.00  168.70 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  804.181    123.624   6.505 1.23e-06 ***
    ## Income        11.627      5.012   2.320   0.0296 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 119.6 on 23 degrees of freedom
    ## Multiple R-squared:  0.1896, Adjusted R-squared:  0.1544 
    ## F-statistic: 5.382 on 1 and 23 DF,  p-value: 0.02957

## Point estimation: estimate yi\|{X_sub_i}

Use the saved coefficients for getting a point estimate for Period 5.

``` r
# period 5: the 5-th row
f_fit$coefficients[1] + f_fit$coefficients[2] * 22.3    
```

    ## (Intercept) 
    ##    1063.468

``` r
# the above is equivalent to 804.181 + 11.627 * 22.3

# resudual for Period 5
ff$Revenue[5] - (f_fit$coefficients[1] + f_fit$coefficients[2] * 22.3) 
```

    ## (Intercept) 
    ##    9.531728

## Interval estimation: predictors

``` r
# Confidence intervals for βi
confint(f_fit, level = .90)
```

    ##                    5 %       95 %
    ## (Intercept) 592.305460 1016.05684
    ## Income        3.037531   20.21692

``` r
confint(f_fit, level = .80)
```

    ##                   10 %      90 %
    ## (Intercept) 641.064169 967.29813
    ## Income        5.014268  18.24018

``` r
# t statistic for 80% confidence interval
qt(.90, 24) # df = 24 for example
```

    ## [1] 1.317836

## Interval estimation for predicting µ_sub_Y\|{X_sub_i}

FYI:  
interval = “confidence” for statistical inference  
interval = “prediction” for predictive tasks

``` r
# want to estimate interval for the group having mean Income of 20
newdata <- data.frame(Income = 20) 
# Note that the predicting a mean and an individual have the same predictor (Y = β0 + β1X1 + ... + βkXk)yet different target (E(Y) = β0 + β1X1 + ... + βkXk for mean prediction, Y = β0 + β1X1 + ... + βkXk + εi for individual prediction), thus they have different interval(i.e., different prediction tolerance)

# 80% confidence interval for the response mean
predict(f_fit, newdata, interval = "confidence", level = .80)
```

    ##        fit      lwr      upr
    ## 1 1036.726 994.6801 1078.771

``` r
# confint(f_fit, level = 0.8) # this is for CI of each βi, don't get confused with predict()

# Note that since we specify predicting CI for the MEAN (of a set of new data points), use parameter interval = "confidence"
# if focusing on individual data point prediction, use interval = "prediction" since it's prediction interval
```

Note that we are estimating the mean of Y, so we have to write our
interpretation in terms of the mean of Y, just like we do the
interpretation to the 90% CI for the mean of Y.  
What we’re doing is we can have a better estimation of the mean of Y
since we have more information from {Xi}.

## Residuals

``` r
# residuals() can get the observed residuals, ε-hats
resids <- residuals(f_fit)
resids
```

    ##             1             2             3             4             5 
    ##   50.57905762   -3.82031389  102.02282715 -103.03036621    9.53172782 
    ##             6             7             8             9            10 
    ##   71.34827218   22.27727748  -28.19015695  108.72104701 -173.56586356 
    ##            11            12            13            14            15 
    ##   89.99916225 -175.81151856  168.69738211   52.99916225  -25.65465960 
    ##            16            17            18            19            20 
    ## -338.85591658  -34.30848144    0.04062849  -63.16062849 -102.67246094 
    ##            21            22            23            24            25 
    ##   56.44293178  132.07015695   99.20921456  146.27727748  -61.14575893

``` r
# or you can simply see the residual from the following code
f_fit$residuals
```

    ##             1             2             3             4             5 
    ##   50.57905762   -3.82031389  102.02282715 -103.03036621    9.53172782 
    ##             6             7             8             9            10 
    ##   71.34827218   22.27727748  -28.19015695  108.72104701 -173.56586356 
    ##            11            12            13            14            15 
    ##   89.99916225 -175.81151856  168.69738211   52.99916225  -25.65465960 
    ##            16            17            18            19            20 
    ## -338.85591658  -34.30848144    0.04062849  -63.16062849 -102.67246094 
    ##            21            22            23            24            25 
    ##   56.44293178  132.07015695   99.20921456  146.27727748  -61.14575893

``` r
# Adding the residuals to the data frame
ff$resids <- resids

# Use the data frame to identify specific residuals
# subset(dataset, subset = "col_name" == "your_condition", select = c('resids'))
subset(ff, subset = Income > 20, select = c('resids'))
```

    ## # A tibble: 19 × 1
    ##     resids
    ##      <dbl>
    ##  1   50.6 
    ##  2  102.  
    ##  3    9.53
    ##  4   71.3 
    ##  5   22.3 
    ##  6  -28.2 
    ##  7  109.  
    ##  8   90.0 
    ##  9 -176.  
    ## 10  169.  
    ## 11   53.0 
    ## 12  -25.7 
    ## 13 -339.  
    ## 14  -63.2 
    ## 15 -103.  
    ## 16   56.4 
    ## 17  132.  
    ## 18   99.2 
    ## 19  146.

``` r
# standard error of the residuals = (s sub ε-hat) = sqrt(MSE)
# sqrt((εi - ε)^2/(df = n-2))
summary(f_fit)$sigma
```

    ## [1] 119.6168

``` r
# or you can simply see the residual from the model in the following line:
# Residual standard error: 0.6122 on 28 degrees of freedom
```

## Check assupmtion about ε

Assumptions about ε:  
1. Normality  
2. Mean 0, constant standard deviation across predictor values

#### Check assupmtion of mean 0, constant standard deviation across all X

There are two way to draw residual plot:

``` r
# standardized residual plot vs. individual Xi
f_fit_stres <- rstandard(f_fit)
plot(ff$Income, f_fit_stres, pch = 16, main = "Standardized Residual vs. Individual Income (individual Xi)", xlab = "individual Income", ylab = "Standardized Residuals")
abline(0,0, lty=2, col="red")
```

![](6_linear_regression_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
# standardized residual plot vs. fitted Yi
plot(f_fit$fitted.values, f_fit_stres, pch = 16, main = "Standardized Residual vs. Fitted Revenue (fitted Yi)", xlab = "fitted Revenue", ylab = "Standardized Residuals")
abline(0, 0, lty=2, col="red")
```

![](6_linear_regression_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->

#### Check normality assumption of standardized residuals

``` r
# Histogram: years, with normal curve
h <- hist(f_fit_stres)
# code to add normal curve
x <- f_fit_stres
xfit <- seq(min(x), max(x), length = 40)
yfit <- dnorm(xfit, mean = mean(x), sd = sd(x))
yfit <- yfit*diff(h$mids[1:2])*length(x)
lines(xfit, yfit, col="blue")
```

![](6_linear_regression_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

``` r
# Normal probability plot
qqnorm(f_fit_stres, pch = 16, main = "Normal Probability Plot", xlab = "Normal Scores", ylab = "Standardized Residuals")
qqline(f_fit_stres, col = "red")
```

![](6_linear_regression_files/figure-gfm/unnamed-chunk-10-2.png)<!-- -->

``` r
# Shapiro-Wilk test
shapiro.test(f_fit_stres)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  f_fit_stres
    ## W = 0.92385, p-value = 0.06273

## Determine the best model

1.  simplicity  
2.  max R-squared  
3.  min error  
4.  significant predictors(p-value)  
5.  residual assumptions meet  
6.  meaningful predictors  
7.  (additional) add new predictor while not dropping adj-R-squared  
8.  mind your explanation: correlation, whether say causation depends
    (not sufficient)

## Supplemental test

#### Hypothesis test: if βi is less than / greater than / equal to a secific value

Is there support for the claim that the avg sales price increases by
more than \$50 for each additional bidder at an auction? find the
p_value  
It’s asking to perform t.test() on β1:  
H0: β1 \<= 50  
H1: β1 \> 50

``` r
clock <- read_excel("C:/Users/Yvonne/Desktop/UMN Courses/6121/Clocks.xlsx")
clm <- lm(Price ~ Bidders, data = clock)
summary(clm)
```

    ## 
    ## Call:
    ## lm(formula = Price ~ Bidders, data = clock)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -516.31 -355.27  -29.49  302.76  688.23 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)   806.40     230.68   3.496  0.00149 **
    ## Bidders        54.64      23.23   2.352  0.02540 * 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 367.2 on 30 degrees of freedom
    ## Multiple R-squared:  0.1557, Adjusted R-squared:  0.1276 
    ## F-statistic: 5.534 on 1 and 30 DF,  p-value: 0.0254

``` r
# extract coefficient and its standard error
coef_value <- clm$coefficients[2]
# std_error <- sqrt(diag(vcov(clm))["Bidders"])
std_error <- 23.23 # you can just use summary() to find

# calculate the t statistic
t_statistic <- (coef_value - 50) / std_error

# get the df
df <- clm$df.residual
# calculate the p-value for a one-tailed test
# compare this p_value with the significance level designated
p_value <- pt(t_statistic, 30, lower.tail = FALSE)
p_value 
```

    ##   Bidders 
    ## 0.4215785

``` r
# NOTE that lower.tail = TRUE / FALSE depends on Ha
# if Ha: β1 < β0 >>> TRUE
# if Ha: β1 > β0 >>> FALSE
```
