HYPOTHESIS TESTING
================

# Hypothesis testing

## General logic

1.  Set up 2 competing hypotheses
    - H0: status quo, thing want to defeat, old one, etc.  
    - Ha: the claim we want to establish  
    - Note that the hypotheses are for the process/population, so use
      the parameters, not statistics
2.  Sometimes how to to set up hypotheses might be tricky
    - e.g. we want to see whether the current strategy is useful or
      not  
    - H0: useful / Ha: not useful  
    - we place hypothesis like this because we can never accept H0, but
      can reject H0 and accept Ha; if we place the hypotheses in the
      opposite way, we can only conclude “accept useful” and never have
      a chance to conclude “not useful”, and it’s not helpful for a
      company to fix their problem. Just consider they will only want to
      conduct hypothesis testing to improve the current strategy
      (i.e. find some problems) if they think it’s not good enough, so
      telling them “it’s good” is not adding values

## Possible testing results

``` r
#          Fail to reject H0        Reject H0 (accept Ha)
# ------------------------------------------------------------------
# H0=T         (correct)            α = type I error
#                                  (wrongly reject H0)
# ------------------------------------------------------------------
# H0=F    β = type II error               (correct)
#        (wrongly not reject H0)
```

### α and β

1.  We can control α in making decision since we start by assuming H0=T,
    so sometimes we will put a more serious consequense as H0 since we
    can control  
2.  Ha isn’t associated with any specific value of μ greater than μ_0,
    so it’s not clear what distribution under Ha is the best one to use
    s.t. μ_a \> μ_0. Thus we can’t determine β  
3.  Increasing n decrease both errors since more information is
    collected  
4.  Reducing α will increasing β \>\> there’s a trade off, consider the
    consequence of each error  
5.  Since we never accept H0 \>\> we never make type II error directly

### Practical example

1.  Since presumption of innocence (law), set it as H0
    - H0: innocent, i.e. not guilty  
    - Ha: guilty
2.  Possible outcomes:
    - guilty  
    - fail to say guilty \>\> but can’t say innocent, since we never
      accept H0  
    - and notice here type I error has a more serious consequence than
      II

### Power

The power of a hypothesis test is: (either is correct)  
- the probability of correctly accepting Ha when it’s true  
- the probability of correctly rejecting H0 when it’s false  
- both means you made the correct decision

# Codes

## Hypothesis test for a process mean μ

``` r
# H0: μ <= 5, its opposite (status quo)
# Ha: μ > 5, the claim want to establish

# t.test(data$column_name, alternative = "greater"/"two.sided"/"less", mu)
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
salmon <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Quiz-Salmon.xlsx')
t.test(salmon$Weight, alternative = "greater", mu = 47)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  salmon$Weight
    ## t = 1.7976, df = 19, p-value = 0.04407
    ## alternative hypothesis: true mean is greater than 47
    ## 95 percent confidence interval:
    ##  47.15781      Inf
    ## sample estimates:
    ## mean of x 
    ##  51.14092

## Hypothesis test for a process proportion p

``` r
# use table() to check the amount of each category first
vacc <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Quiz-Vaccine.xlsx')
table(vacc$Completed)
```

    ## 
    ##  0  1 
    ## 33 17

``` r
# prop.test(the amount of the category we're interested in, sample size, alternative)
# prop.test(17, 50, alternative = "greater"/"two.sided"/"less", p = 0.1)
prop.test(17, 50, alternative = "greater", p = 0.1)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  17 out of 50, null probability 0.1
    ## X-squared = 29.389, df = 1, p-value = 2.961e-08
    ## alternative hypothesis: true p is greater than 0.1
    ## 95 percent confidence interval:
    ##  0.2318345 1.0000000
    ## sample estimates:
    ##    p 
    ## 0.34

## Testing normality

``` r
# H0: X ~ N(., .), normally distributed 
# Ha: X != N(., .), NOT normally distributed 

# histogram
h <- hist(salmon$Weight, breaks = 10)
# then add normal curve
x <- salmon$Weight
xfit <- seq(min(x), max(x), length = 40)
yfit <- dnorm(xfit, mean = mean(x), sd = sd(x))
yfit <- yfit*diff(h$mids[5:6])*length(x)
lines(xfit, yfit, col="blue", lwd = 1.5)
```

![](3_hypothesis_testing_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
# Probability Plot
qqnorm(x, pch = 20)
qqline(x, col = "blue")
```

![](3_hypothesis_testing_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->

``` r
# Goodness of fit test: Shapiro-Wil test here
# to the extent that Ha = TRUE, W has a high value, and p-value is correspondingly low
# 0 < W < 1
shapiro.test(x)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  x
    ## W = 0.97645, p-value = 0.8806

## Interpret p-value

Note that the interpretation of p-value is irrelevant to the
significance level.

P-value is something that help us to decided whether to reject H0 or not
given a specific α

1.  The probability of obtaining *a test statistic* more extreme than
    the actual sample value given *H0* is true.  
2.  the probability of getting *a sample statistic value* as or more
    extreme than the value that we observe in the sample, if *H0* is
    true.  
3.  Observed level of significance: it’s the smallest value of α for
    which H0 will be rejected.  
4.  p-value \<= α \>\>\>\> we can say we have enough evidence to reject
    H0.
