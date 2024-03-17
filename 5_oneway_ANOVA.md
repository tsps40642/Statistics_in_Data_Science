ONEWAY ANOVA
================

# Oneway ANOVA

#### Concepts

1.  Oneway ANOVA is the special case for multiple indep. samples with
    equal variances  
2.  Also can be seen as the extension of two group with equal
    variances  
3.  “Oneway” indicates we only have one nominal variable in predictor

#### Assumptions:

1.  random sampling  
2.  stability over time  
3.  for each level j of Xj, j = 1, 2, …, k
    1)  if any nj \< 30, all Xj ~ N(., .)  
    2)  σ1 = σ2 = … = σk (equal variance assumption)

#### Between-groups and within-groups

1.  SS_total = SS_between + SS_within = Total variation
2.  df_total = df_between + df_within  
3.  MS_between = SS_between/df_between  
4.  MS_within = SS_within/df_within

#### Sampling distribution

F statistic = MS_between/MS_within = ((SSB/k-1)/k-1)/((SSW/n-k)/n-k) ~
F(k-1, n-k)

# Conduct oneway ANOVA

## State hypotheses

Note that the followings are the two equivalent and interchangable
representations:

#### Test of relationships

H0: no relationships  
- means that the hypothesized prob. distri. for Y for each of the levels
of predictors are the same  
Ha: relationships  
- means that relation translate to the difference in means of Y
depending on the level of X

#### Test of means

H0: μ1 = μ2 = … = μk for k groups  
Ha: at least two μi differ

``` r
# Are the mean number of daily visitors the same for three types of snow conditions?

# input data
NumbVisitors <- 
  c(1210, 1080, 1537, 941, 2107, 1149, 862, 1870, 1528, 1382, 2846, 1638, 2019, 1178, 2233)
SnowType <- 
  c(rep("Powder", times = 4), rep("Machine Made", times = 6), rep("Packed", times = 5))

# explicitly identify SnowType as a nominal variable i.e. factor type in R
SnowType <- factor(SnowType) # no difference to as.factor()

# use anova() to perform the test
fit <- aov(NumbVisitors ~ SnowType)
summary(fit)
```

    ##             Df  Sum Sq Mean Sq F value Pr(>F)  
    ## SnowType     2 1468909  734455   3.126 0.0807 .
    ## Residuals   12 2819077  234923                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# plot the means by snow type
print(model.tables(fit, "means"))
```

    ## Tables of means
    ## Grand mean
    ##      
    ## 1572 
    ## 
    ##  SnowType 
    ##     Machine Made Packed Powder
    ##             1483   1983   1192
    ## rep            6      5      4

``` r
boxplot(NumbVisitors ~ SnowType)
```

![](5_oneway_ANOVA_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

## Checking assumptions

#### Testing normality

H0: residuals ~ N(., .)  
Ha: residuals not ~ N(., .)

1.  Just as we did for the hypothesis test, need to check whether the
    normality assumption meet
    - We check normality of residuals  
    - Residual = actual - category mean = Y_ij - (Y-bar)\_j
2.  Pooling all the data together and do the normality check
    simultaneously for all groups has the advantages:
    - Only need to do a single set of test  
    - can do the test with all the data we have \>\> more powerful test
      of the assumption
3.  Non-normal distributed residuals indicates there’s still signal in
    residuals which we don’t want to miss

``` r
# Histogram of residuals
h <- hist(fit$residuals, breaks = 10, ylim = c(0, 3))
# then add normal curve
x <- fit$residuals
xfit <- seq(min(x), max(x), length = 40)
yfit <- dnorm(xfit, mean = mean(x), sd = sd(x))
yfit <- yfit*diff(h$mids[1:2])*length(x)
lines(xfit, yfit, col="blue", lwd = 1.5)
```

![](5_oneway_ANOVA_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
# normal probability plot as a check on the normality assumption
qqnorm(fit$residuals, pch = 20)
qqline(fit$residuals, col = "blue")
```

![](5_oneway_ANOVA_files/figure-gfm/unnamed-chunk-2-2.png)<!-- -->

``` r
# Goodness of fit test: Shapiro-Wil test
# to the extent that Ha = TRUE, W has a high value, and p-value is correspondingly low
shapiro.test(fit$residuals)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  fit$residuals
    ## W = 0.98745, p-value = 0.9974

#### Testing equal variances: Levene’s test of equal variances

H0: σ1 = σ2 = … = σk for k groups  
Ha: at least one pair has unequal variances

##### Assumptions

1.  random sampling  
2.  stability over time

##### Sampling distribution

F ~ F(k-1, n-k)

``` r
library(car)
```

    ## Loading required package: carData

``` r
# Levene's test
leveneTest(NumbVisitors, SnowType)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value Pr(>F)
    ## group  2  1.0519 0.3794
    ##       12

## On the conclusion of a relationship i.e. ANOVA Ha is accepted

If ANOVA Ha is accepted and checks for normality also meet…  
Then “which” pairs have the relationship and “how many differ” ??  
-\> Use Tukey’s HSD(honest significant difference) test to check.

#### Tukey’s HSD test

H0: no relationship i.e. μi = μj, for each i! = j being checked  
Ha: relationship i.e. μi != μj  
if p-value \< α, reject H0 for that pair

##### Assumptions: (same as ANOVA since Tukey’s test is based on accpeting ANOVA Ha)

1.  random sampling  
2.  stability over time  
3.  for each level j of Xj, j = 1, 2, …, k
    1)  if any nj \< 30, all Xj ~ N(., .)  
    2)  σ1 = σ2 = … = σk

``` r
# Tukey's HSD test checks all the pairwise contrasts
TukeyHSD(fit, conf.level = .90)
```

    ##   Tukey multiple comparisons of means
    ##     90% family-wise confidence level
    ## 
    ## Fit: aov(formula = NumbVisitors ~ SnowType)
    ## 
    ## $SnowType
    ##                       diff        lwr        upr     p adj
    ## Packed-Machine Made  499.8  -165.1191 1164.71913 0.2440083
    ## Powder-Machine Made -291.0  -999.8062  417.80617 0.6324909
    ## Powder-Packed       -790.8 -1527.4130  -54.18702 0.0753350

Note that the p-adj is already the adjusted p-values for each pair.

# Supplement: oneway ANOVA as a special case for two groups with equal variances

Although the test statistics are different, the decisions to reject H0
are the same

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
smoking <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Smoking.xlsx')
smoking$married <- as.factor(smoking$married)
attach(smoking)

# t.test for two indept. groups with equal variances
leveneTest(smoking$age, smoking$married) #equal variance
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##         Df F value    Pr(>F)    
    ## group    1  181.98 < 2.2e-16 ***
    ##       1689                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
t.test(age ~ married, alternative = "greater", var.equal = T)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  age by married
    ## t = 2.8877, df = 1689, p-value = 0.001965
    ## alternative hypothesis: true difference in means between group Married and group Unmarried is greater than 0
    ## 95 percent confidence interval:
    ##  1.130277      Inf
    ## sample estimates:
    ##   mean in group Married mean in group Unmarried 
    ##                51.09659                48.46856

``` r
# ANOVA
fit_1 <- aov(age ~ married)
summary(fit_1)
```

    ##               Df Sum Sq Mean Sq F value  Pr(>F)   
    ## married        1   2915  2914.9   8.339 0.00393 **
    ## Residuals   1689 590393   349.6                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
