TWO GROUPS TESTING
================

# Testing scenarios

## 2 variables

The concept for two groups is:  
for two groups, two sets of observations, do they differ on X, where X
is the stat. we are interested in understanding

``` r
#                                   Y(dependent var.)    X's(indep. var.)
# ---------------------------------------------------------------------------
# Two groups    1. Paired 
#               2. Indep, unequal var.   Interval       Nominal(2 levels)
#               3. Indep, equal var. 
# ---------------------------------------------------------------------------
# Oneway ANOVA                           Interval           Nominal
# 
# - is the special case for multiple indep. samples w. equal variances  
# - also can be seen as the extension of two group w. equal variances 
# ---------------------------------------------------------------------------
# LR                                     Interval           Interval 
# ---------------------------------------------------------------------------
# Twoway Table                            Nominal            Nominal 
```

## Multiple predictor

``` r
#                              Y(dependent var.)        X's(indep. var.)
# ---------------------------------------------------------------------------
# Multiple regression              interval          at least one interval 
# ---------------------------------------------------------------------------
# Logistic regression           Nominal(2 levels)    at least one interval
```

# Testing: two groups

## Testing logic

1.  Set up hypothesis
    - H0: no relationship  
    - Ha: relationship  
    - what would it mean to be “no relationship” between the
      variables?  
2.  Identify test stat. sensitive to the existence of relationship  
3.  Make assump. to allow identifying sampling distr. for that stat.  
4.  Use sample data + sampling distri. to conduct the test

## Inference for two means

1.  Parameter: (μ_1-μ_2) = diff. between two process means  
2.  Statistic: (X_1_bar-X_2_bar) = diff. between two sample means  
3.  If can match up individual sampled from the two process \>\> paired
    samples
    - If can’t, if the variances differ in the two processes \>\>
      indep., unequal var.  
    - If can’t, if the variances equal in the two processes \>\> indep.,
      equal var.

# Codes

## Testing for paired samples

H0: (μ1 - μ2) = 0  
Ha: (μ1 - μ2) \> or \< or != 0

#### Assumptions:

1.  random sampling  
2.  stability over time  
3.  if n1 \< 30 or n2 \< 30, X1, X2 ~ N(., .)

#### 90% CI for paired samples

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
cloth <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Prices.xlsx')
attach(cloth)

t.test((StoreA-StoreB), conf.level = 0.9)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  (StoreA - StoreB)
    ## t = 0.69078, df = 9, p-value = 0.5071
    ## alternative hypothesis: true mean is not equal to 0
    ## 90 percent confidence interval:
    ##  -1.984438  4.384438
    ## sample estimates:
    ## mean of x 
    ##       1.2

``` r
# or can write it by yourself
d <- StoreA-StoreB # the difference for each pair
d_bar <- sum(d)/10 # mean for all the differences
sd_d <- sqrt(sum((d-d_bar)^2)/(10-1)) # sd for all the differences

# need to use se to construct CI
d_bar + qt(0.95, 9)*sd_d/sqrt(10) # upper bound
```

    ## [1] 4.384438

``` r
d_bar - qt(0.95, 9)*sd_d/sqrt(10) # lower bound
```

    ## [1] -1.984438

#### Do the t.test

There are 2 ways to do, they have the same results:  
1. Use x, y, paired = T  
2. By setting (μ1 - μ2), it becomes the same as the hypothesis we did
before

``` r
# t.test((data_column1 - data_column2), alternative = "two.sided")

# they are equivalent 
# t.test(StoreA, StoreB, alternative = "two.sided", paired = T) 
t.test((StoreA-StoreB), alternative = "two.sided") 
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  (StoreA - StoreB)
    ## t = 0.69078, df = 9, p-value = 0.5071
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  -2.729763  5.129763
    ## sample estimates:
    ## mean of x 
    ##       1.2

## Testing for independent samples, EQUAL / UNEQUAL variances

H0: (μ1 - μ2) = (μ1 - μ2)0  
Ha: (μ1 - μ2) \> or \< or != (μ1 - μ2)0

#### Assumptions

1.  random sampling  
2.  stability over time  
3.  if n1 \< 30 or n2 \< 30, X1, X2 ~ N(., .)

``` r
library(readxl)
uk <- read_excel("C:/Users/Yvonne/Desktop/UMN Courses/6121/Smoking.xlsx", na="NA", col_names = TRUE)

# IMPORTANT! explicitly identify variables as nominal [Factor type in R]
uk$married <- factor(uk$married)
uk$smoke <- factor(uk$smoke)

# with attach(), we can directly type the column name to refer to the dataframe
attach(uk)
```

#### 90% CI for the difference between two means, unequal variances

``` r
# confidence interval
t.test(age ~ married, conf.level = 0.90)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  age by married
    ## t = 2.846, df = 1422.2, p-value = 0.004491
    ## alternative hypothesis: true difference in means between group Married and group Unmarried is not equal to 0
    ## 90 percent confidence interval:
    ##  1.108138 4.147930
    ## sample estimates:
    ##   mean in group Married mean in group Unmarried 
    ##                51.09659                48.46856

#### Check whether equal variance before t.test: Levene’s test

H0: homogeneity  
Ha: heterogeneity

``` r
library(car)
```

    ## Loading required package: carData

``` r
# leveneTest(Y, X)
leveneTest(age, married) # reject H0 >> unequal variance 
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##         Df F value    Pr(>F)    
    ## group    1  181.98 < 2.2e-16 ***
    ##       1689                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

#### Do the t.test based on the results of the levene’s test.

``` r
# two-tailed test of H0: (mu1 - mu2) = 0
t.test(age ~ married, alternative = "two.sided")
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  age by married
    ## t = 2.846, df = 1422.2, p-value = 0.004491
    ## alternative hypothesis: true difference in means between group Married and group Unmarried is not equal to 0
    ## 95 percent confidence interval:
    ##  0.8166039 4.4394633
    ## sample estimates:
    ##   mean in group Married mean in group Unmarried 
    ##                51.09659                48.46856

``` r
# var.equal = F is the default so don't need to type 
```
