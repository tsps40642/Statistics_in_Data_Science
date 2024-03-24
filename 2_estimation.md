ESTIMATION
================

# Estimation

1.  Point estimation  
2.  Interval estimation (we will focus on this in this chapter)

# Confidence interval

CI = best point estimation +- test statistics at stated CI \* estimated
variability of sampling distribution

## Sample distribution vs. Sampling distribution

``` r
# mean = 1000, sd = 20, 975 is some threshold

# what is the probability that a single jar will have a weight < 975 grams?
# for a single data point in a sample >> sample distribution >> use sd
pnorm(975, 1000, 20)
```

    ## [1] 0.1056498

``` r
# what is the probability that a sample of 5 jars will have a weight < 975 grams?
# for the means of multiple samples >> sampling distribution >> use se = sd/sqrt(sample_size)
pnorm(975, 1000, 20/sqrt(5))
```

    ## [1] 0.002594304

## 100(1-α)% CI for a process mean μ

``` r
# t.test(data$column_name, conf.level = ???)

library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
salmon <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Quiz-Salmon.xlsx')
t.test(salmon$Weight, conf.level = 0.9)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  salmon$Weight
    ## t = 22.201, df = 19, p-value = 4.734e-15
    ## alternative hypothesis: true mean is not equal to 0
    ## 90 percent confidence interval:
    ##  47.15781 55.12404
    ## sample estimates:
    ## mean of x 
    ##  51.14092

Or in the long way, build by yourself

``` r
# note that we need to use t-distribution since unknown σ
# note that we use se of sampling distribution of the βi_hat 

sal_mean <- mean(salmon$Weight)
sal_se <- sd(salmon$Weight)/sqrt(length(salmon$Weight))
ub <- sal_mean + qt(0.95, df = (length(salmon$Weight)-1))*sal_se
lb <- sal_mean - qt(0.95, df = (length(salmon$Weight)-1))*sal_se
lb
```

    ## [1] 47.15781

``` r
ub
```

    ## [1] 55.12404

## 100(1-α)% CI for a process proportion p

Since the proportion p ~ B(n, p) which is discrete, we use conti.
approximation for Binomial sampling distri.

``` r
# use table() to check the amount of each category first
table(salmon$Weight)
```

    ## 
    ## 31.3795722120214 35.4610274774093 39.3186258092767 42.7075879847066 
    ##                1                1                1                1 
    ## 42.9617450851195 47.0064050721468  47.913600558106 48.7515343276592 
    ##                1                1                1                1 
    ## 50.1741330519177 50.3156264621219 52.1888331722141 52.5384293837856 
    ##                1                1                1                1 
    ## 52.8625208073223 54.6537469887068 55.9045256844079 56.9004836993347 
    ##                1                1                1                1 
    ## 59.8232223244284 62.5114725912245 62.7251002605274 76.7203056550535 
    ##                1                1                1                1

``` r
# prop.test(the amount of the category we're interested in, sample size, conf.level = ???)
prop.test(12, 20, conf.level = 0.9)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  12 out of 20, null probability 0.5
    ## X-squared = 0.45, df = 1, p-value = 0.5023
    ## alternative hypothesis: true p is not equal to 0.5
    ## 90 percent confidence interval:
    ##  0.3951858 0.7778833
    ## sample estimates:
    ##   p 
    ## 0.6

``` r
# this is a chi-squared statistic with Yates' continuity correction 
# if using the formula in the text book would get a different results, not sure whether it's due to that formula based on the conti. approximation of the binomial sampling distribution. But skip here. 
```

## Interpret the 100(1-α)% CI

1.  Note that we place the interval around X_bar(statistic) instead of
    μ(parameter), and there is a (1-α)\*100% chance of getting an
    interval that captures μ
    - (1-α)\*100% of the time creating this interval around X_bar would
      give us an interval containing μ  
    - α\*100% of the time creating this interval around X_bar would give
      us an interval NOT containing μ
2.  Interpret “in the context of the situation”, for example:
    - 85% of the time this procedure will give us an interval that
      contains the true mean age of patients from the given process  
    - We have a degree of belief of 85% that we obtained an interval
      that contains the true mean age of patients from the given process
