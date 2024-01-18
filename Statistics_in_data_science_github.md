Statistics in Data Science
================

# Before we start, remind yourself of the following poins

Here I’ve listed some most important points to notice:  
1. NEVER ACCEPT H0  
2. whenever I DON’T know σ of the population, use T-DISTRIBUTION,
regardless of the sample size  
3. break even situation \>\>\> average charge \* E(X) of event
happening  
4. P(X = x) on any given points on a conti. distri. must be 0; we can
only get the prob. within the interval  
5. if given 2 set of nominal-level variables, we should use TABLE
(contingency table?) rather than a boxplot since the latter requires an
at least ordinal-level data  
6. distinguish bar chart from histogram:  
(1) x-axis of a bar chart is categorical data, e.g., types, regions,
species, … (2) x-axis of a histogram is continuous data type, e.g., age,
length, years, …  
7. interpretation of p-value is irrelevant to the significance level

## Read the dataset

(can change the path to where your file is saved)  
setwd(‘C:/Users/Yvonne/Desktop/UMN Courses/6121’)

### read excel

library(readxl)  
qc \<- read_excel(‘Quiz-Credit.xlsx’)

### read csv

data \<- read.csv(‘title.csv’, header = T)

### read txt

library(readxl)  
data \<- read.table(‘title.txt’)

# CODE WORTH NOTICING

## Binomial: discrete data

``` r
# P(X = 3) within 5 trails and prob. to succeed is 0.6
dbinom(3, 5, 0.6)
```

    ## [1] 0.3456

``` r
# P(X <= 3) = P(0) + P(1) + P(2) + P(3) within 5 trails and prob. to succeed is 0.6
# note that since it's a discrete r.v., endpoint should count
pbinom(3, 5, 0.6)
```

    ## [1] 0.66304

``` r
# note the cut point for discrete variable
# P(X >= 4) = 1 - P(X<=3)
1 - pbinom(3, 12, 0.1)
```

    ## [1] 0.02563747

``` r
# the value x to obtain at least the given cumulative prob. 
qbinom(0.8, 5, 0.6)
```

    ## [1] 4

## You can derive qnorm() using pnorm(), and vice versa

``` r
# interval upper and lower bounds
lower_bound <- qnorm(0.05, mean = 96, sd = 8.5)
upper_bound <- qnorm(0.95, mean = 96, sd = 8.5)

# insert bounds to pnorm() to get cumulative probability
l_prob <- pnorm(lower_bound, mean = 96, 8.5)
u_prob <- pnorm(upper_bound, mean = 96, 8.5)
```

# ESTIMATION

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

Note that we need to interpret “in the context of the situation”.

Thus do not write this:  
85% of the time this procedure will give us an interval that contains
the parameter(here is the mean of the process).

But write this:  
1. 85% of the time this procedure will give us an interval that contains
the true mean age of patients from the given process.  
2. We have a degree of belief of 85% that we obtained an interval that
contains the true mean age of patients from the given process.

# HYPOTHESIS TESTING

## Hypothesis test for a process mean μ

``` r
# H0: μ <= 5, its opposite (status quo)
# Ha: μ > 5, the claim want to establish

# t.test(data$column_name, alternative = "greater"/"two.sided"/"less", mu)
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

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
# Probability Plot
qqnorm(x, pch = 20)
qqline(x, col = "blue")
```

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->

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

# TWO GROUPS TESTING

## Testing for paired samples

H0: (μ1 - μ2) = 0  
Ha: (μ1 - μ2) \> or \< or != 0

Assumptions:  
1. random sampling  
2. stability over time  
3. if n1 \< 30 or n2 \< 30, X1, X2 ~ N(., .)

90% CI for paired samples

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

By setting (μ1 - μ2), it becomes the same as the hypothesis we did
before.

``` r
# t.test((data_column1 - data_column2), alternative = "two.sided")
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

## Testing for independent samples, UNEQUAL variances

H0: (μ1 - μ2) = (μ1 - μ2)0  
Ha: (μ1 - μ2) \> or \< or != (μ1 - μ2)0

Assumptions:  
1. random sampling  
2. stability over time  
3. if n1 \< 30 or n2 \< 30, X1, X2 ~ N(., .)

``` r
library(readxl)
uk <- read_excel("C:/Users/Yvonne/Desktop/UMN Courses/6121/Smoking.xlsx", na="NA", col_names = TRUE)

# IMPORTANT! explicitly identify variables as nominal [Factor type in R]
uk$married <- factor(uk$married)
uk$smoke <- factor(uk$smoke)

# with attach(), we can directly type the column name to refer to the dataframe
attach(uk)
```

90% CI for the difference between two Means, Unequal variances

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

Check whether equal variance before t.test, since we need to decide the
parameter

``` r
library(car)
```

    ## Loading required package: carData

``` r
# leveneTest(Y, X)
leveneTest(age, married)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##         Df F value    Pr(>F)    
    ## group    1  181.98 < 2.2e-16 ***
    ##       1689                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Do the t.test based on the results of the levene’s test.

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
# var.equal = F is the default
```

Note that although we always do the checking for the assumptions after
our test, yet if you’re gonna use the result of your assumption as a
parameter for the function, like here we need to decide var.equal = F or
T when using t.test(), thus need to check equal variance first.

## Testing for independent samples, EQUAL variances

H0: (μ1 - μ2) = (μ1 - μ2)0  
Ha: (μ1 - μ2) \> or \< or != (μ1 - μ2)0

Assumptions:  
1. random sampling  
2. stability over time  
3. if n1 \< 30 or n2 \< 30, X1, X2 ~ N(., .)

Check whether equal variance.

``` r
library(car)

# leveneTest(interval_variable, nominal_variable)
leveneTest(age, married)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##         Df F value    Pr(>F)    
    ## group    1  181.98 < 2.2e-16 ***
    ##       1689                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Do the t.test based on the results of the levene’s test.

``` r
# two-tailed test of H0: (mu1 - mu2) = 0
t.test(age ~ married, alternative = "two.sided", var.equal = T)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  age by married
    ## t = 2.8877, df = 1689, p-value = 0.00393
    ## alternative hypothesis: true difference in means between group Married and group Unmarried is not equal to 0
    ## 95 percent confidence interval:
    ##  0.8430462 4.4130209
    ## sample estimates:
    ##   mean in group Married mean in group Unmarried 
    ##                51.09659                48.46856

``` r
# var.equal = F is the default
```

# ONEWAY ANOVA

Oneway ANOVA is the special case for multiple indep. samples with equal
variances.  
Also can be seen as the extension of two group with equal variances.

Assumptions:  
1. random sampling  
2. stability over time  
3. for each level j of Xj, j = 1, 2, …, k  
(1) if any nj \< 30, all Xj ~ N(., .)  
(2) σ1 = σ2 = … = σk (equal variance assumption)

Sampling distribution:  
F = MSB/MSW = ((SSB/k-1)/k-1)/((SSW/n-k)/n-k) ~ F(k-1, n-k)

## Conduct oneway ANOVA

Note that the followings are the two equivalent representations:

Test of relationships:  
H0: no relationships  
Ha: relationships

Test of means:  
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

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

## Checking assumptions

### Testing normality

Just as we did for the hypothesis test, need to check whether the
normality assumption meet.  
We check normality of residuals.  
Residual = actual - category mean = Yij - (Y-bar)j

H0: residuals ~ N(., .)  
Ha: residuals not ~ N(., .)

not normal distributed residuals indicates there’s still signal in
residuals which we don’t want to miss.

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

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

``` r
# normal probability plot as a check on the normality assumption
qqnorm(fit$residuals, pch = 20)
qqline(fit$residuals, col = "blue")
```

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-19-2.png)<!-- -->

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

### Testing equal variances

Levene’s test of equal variances:  
H0: σ1 = σ2 = … = σk for k groups  
Ha: at least one pair has unequal variances

Assumptions:  
1. random sampling  
2. stability over time

Sampling distribution: F ~ F(k-1, n-k)

``` r
library(car)

# Levene's test
leveneTest(NumbVisitors, SnowType)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##       Df F value Pr(>F)
    ## group  2  1.0519 0.3794
    ##       12

## On the conclusion of a relationship i.e. ANOVA Ha is accepted

If ANOVA Ha is accepted and checkings for normality also meet…  
Then “which” pairs have the relationship and “how many differ” ??  
-\> Use Tukey’s HSD(honest significant difference) test to check.

H0: no relationship i.e. μi = μj, for each i! = j being checked  
Ha: relationship i.e. μi != μj

Assumptions: (same as ANOVA since Tukey’s test is based on accpeting
ANOVA Ha)  
1. random sampling  
2. stability over time  
3. for each level j of Xj, j = 1, 2, …, k  
(1) if any nj \< 30, all Xj ~ N(., .)  
(2) σ1 = σ2 = … = σk

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

## Supplement: oneway ANOVA as a special case for two groups with equal variances

Although the test statistics are different, the decisions to reject H0
are the same.

``` r
smoking <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Smoking.xlsx')
smoking$married <- as.factor(smoking$married)
attach(smoking)
```

    ## The following objects are masked from uk:
    ## 
    ##     age, amtWeekdays, amtWeekends, grossIncome, maritalStatus, married,
    ##     record, region, smoke

``` r
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

# LINEAR REGRESSION

Assumptions:  
1. random sampling  
2. stability over time  
3. εi ~ N(mean = 0, sd = σ) constant (over all possible values of the
predictors)

## State the linear model

### Explicitly state the linear model (that is of interest to the companym, business, …)

Volume = β0 + β1 \* Income + ε  
(Remember ε)  
(Use the actual response variable name rather than solely Y)

### Explicitly identify the fitted model

Volume = 36.537 + 0.00218 \* Income  
(Remember DON’T ADD ε)  
(Use the fitted coefficients rather than typing β1_hat)

## Interpretation

### Practical significance

#### Coefficient of determination, R-squared

The square of correlation coefficient  
The proportion of the variability in Y that is accounted for by the
linear relationship between X and Y

#### Correlation Ciefficient, R

The (square root + sign) of the coefficient of determination  
Used to check each pair of the variables to see whether collinearity
exists  
R \<= 2: can interpret without reservation  
2 \< R \<= 7: interpret cautiously  
R \> 7: do not interpret βi_hat and βj_hat (i!=j), whether drop one or
not depends

#### Standard error of the residuals, S_sub_ε_hat

The standard deviation of εi_hat around the regression line  
= measure of unexplained variation of the response variable Y  
= measure of the residual variability(sd) around our model  
= sqrt(variance of the residuals)  
= sqrt(MSE).  
It’s in the unit of Y.

### Statistical significance

#### p-value for model as a whole (global T-test)

There is a chance of XX% that we would get a sample result as extreme as
the one we obtain if there’s no relationship between Y and any
predictors.  
DON’T USE P-VALUE AS MODEL SELECTION SINCE IT GIVES THE COMPARISON OF K
PREDICTORS TO NONE

#### p-value for individual predictor (individual T-test)

There is a chance of XX% that we would get a sample result as extreme as
the one we obtain if there’s no relationship between Xi and Y, holding
other predictors fixed.

### β0, βi

Volume = β0 + β1 (Income) + ε

β0:  
Purchase volume when the Income = 0, since this value of Income is well
outside the range of values (minimum Income = \$12,000 with s = \$6039),
β0 has no interpretation in the problem context.

Note that β0_hat is the value of μY_hat when all the {Xi} = 0  
we can interpret it only if the 2 conditions meet:  
1. it makes logical sense for all the {Xi} = 0  
2. {Xi} = 0 within the range of data

β1:  
The increase in mean monthly purchase volume (in \$s) for every \$1
increase in family income for the process of Moose customers using their
charge card.  
(… “holding other predictors fixed” needed for multiple regression)

### 85% CI for β1

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

## Visualization

Do the scatter plot w/ fitted linear regression line.

``` r
# plot(X_data, Y_data, type = "p"): scatter plot
# plot(X_data, Y_data, type = "l"): line plot

# I've adjusted some of the parameter in the functions but not big deal
ff <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/FastFood.xlsx')

plot(ff$Income, ff$Revenue, pch = 16, main = "Revenue ~ Income", xlab = "Income", ylab = "Revenue")
# regression line
abline(lm(Revenue ~ Income, data = ff), lty=2, lwd = 2, col="red") 
```

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

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
interval = “confidence” for Statistical inference  
interval = “prediction” for Predictive tasks

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

### Check assupmtion of mean 0, constant standard deviation across all X

``` r
# standardized residual plot vs. individual Xi
f_fit_stres <- rstandard(f_fit)
plot(ff$Income, f_fit_stres, pch = 16, main = "Standardized Residual vs. Individual Income (individual Xi)", xlab = "individual Income", ylab = "Standardized Residuals")
abline(0,0, lty=2, col="red")
```

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

``` r
# standardized residual plot vs. fitted Yi
plot(f_fit$fitted.values, f_fit_stres, pch = 16, main = "Standardized Residual vs. Fitted Revenue (fitted Yi)", xlab = "fitted Revenue", ylab = "Standardized Residuals")
abline(0, 0, lty=2, col="red")
```

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-31-2.png)<!-- -->

### Check normality assumption of standardized residuals

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

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

``` r
# Normal probability plot
qqnorm(f_fit_stres, pch = 16, main = "Normal Probability Plot", xlab = "Normal Scores", ylab = "Standardized Residuals")
qqline(f_fit_stres, col = "red")
```

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-32-2.png)<!-- -->

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

## supplemental test

### Hypothesis test: if βi is less than / greater than / equal to a secific value

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

# NON-LINEAR REGRESSION

## Polynomial Regression

``` r
library(readxl)
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

![](Statistics_in_data_science_github_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->

# CATEGORICAL PREDICTORS

``` r
p <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Polishing.xlsx')

# 1
p$type <- as.factor(p$type)
p_fit <- lm(time ~ diam + type, data = p)
summary(p_fit)
```

    ## 
    ## Call:
    ## lm(formula = time ~ diam + type, data = p)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -24.084  -6.949  -1.967   6.149  31.635 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  -1.1880     5.0170  -0.237 0.813728    
    ## diam          2.9992     0.4636   6.469 3.22e-08 ***
    ## type2        16.9825     4.6756   3.632 0.000634 ***
    ## type3         9.5477     5.0851   1.878 0.065949 .  
    ## type4         7.7619     4.9898   1.556 0.125763    
    ## type5        -7.1759     4.6722  -1.536 0.130520    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.77 on 53 degrees of freedom
    ## Multiple R-squared:  0.6502, Adjusted R-squared:  0.6172 
    ## F-statistic: 19.71 on 5 and 53 DF,  p-value: 4.828e-11

``` r
# time = -1.1880 + 2.9992*diam + 16.9825*casserole + 9.5477*dish + 7.7619*tray - 7.1759*plate

# 2
p_fit2 <- lm(time ~ diam, data = p)
summary(p_fit2)
```

    ## 
    ## Call:
    ## lm(formula = time ~ diam, data = p)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -28.037  -8.287  -2.705   8.315  43.438 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  -1.9547     5.4020  -0.362    0.719    
    ## diam          3.4567     0.4667   7.407 6.67e-10 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 13.69 on 57 degrees of freedom
    ## Multiple R-squared:  0.4905, Adjusted R-squared:  0.4815 
    ## F-statistic: 54.86 on 1 and 57 DF,  p-value: 6.67e-10

``` r
# time = -1.9547 + 3.4567*diam

# 3 
p$type <- as.factor(p$type)
p_fit3 <- aov(p$time ~ p$type)
p_fit3_lm <- lm(time ~ type, data = p)
summary(p_fit3)
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## p$type       4   7846  1961.4   8.067 3.56e-05 ***
    ## Residuals   54  13129   243.1                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# can use summary() to see global p-value, R-squared, se, ... if needed
summary(p_fit3_lm) 
```

    ## 
    ## Call:
    ## lm(formula = time ~ type, data = p)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -28.573  -9.367  -3.344   7.712  59.977 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   27.122      3.251   8.342 2.78e-11 ***
    ## type2         26.133      5.906   4.425 4.73e-05 ***
    ## type3          7.888      6.731   1.172 0.246381    
    ## type4         22.281      5.906   3.772 0.000403 ***
    ## type5         -2.928      6.131  -0.478 0.634894    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 15.59 on 54 degrees of freedom
    ## Multiple R-squared:  0.3741, Adjusted R-squared:  0.3277 
    ## F-statistic: 8.067 on 4 and 54 DF,  p-value: 3.562e-05

``` r
# time = 27.122 + 26.133*casserole + 7.888*dish + 22.281*tray - 2.928*plate
```

1 = bowl  
2 = casserole  
3 = dish  
4 = tray  
5 = plate

## Interpretation

### β0, βi

time = β0 + β1*diam + β2*casserole + β3*dish + β4*tray + β5*plate + ε  
time = -1.1880 + 2.9992*diam + 16.9825*casserole + 9.5477*dish +
7.7619*tray - 7.1759*plate  
(if bowl as a reference)

β0: (baseline)  
The mean grinding and polishing time(in minutes) in 0 diameter(in
inches) for baseline type, here is the bowl.  
Yet it’s not logically meaningful, so β0 has no interpretation in the
problem context here.

β1:  
The increase in mean grinding and polishing time(in minutes) for every 1
inches of diameter increase for “all types” of products.  
(β1 won’t change if we shift the baseline type)

β2:  
difference in the mean grinding and polishing time(in minutes) for
casseroles(type2) compared to bowls(type1, baseline) at all levels of
diameter of item(in inches).

β3, β4, β5 the similar…

### p-value for model as a whole (global T-test)

Global test, see p-value of 4.828e-11

H0: β1 = β2 = β3 = β4 = β5 = 0; no relationship  
H1: at least one is not 0; relationship

And p-value, R-squared, … for the global test is not going to change.

There is a chance of XX% that we would get a sample result as extreme as
the one we obtain if there’s no relationship between Y and any of
predictors.  
DON’T USE P-VALUE AS MODEL SELECTION SINCE IT GIVES THE COMPARISON OF K
PREDICTORS TO NONE

### p-value for individual predictor (individual T-test)

For categorical variables, use the smallest p-value to determine whether
categorical variable has relationship with Y.

H0: type i has no difference with the baseline type  
H1: type i has difference with the baseline type

# INTERACTIONS

``` r
p <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/Polishing.xlsx')

# set type 5 (plate) as reference
p$type <- as.factor(p$type)
p <- within(p, type <- relevel(type, ref = 5))

p_fit_inter <- lm(time ~ diam + type + diam*type, data = p)
summary(p_fit_inter)
```

    ## 
    ## Call:
    ## lm(formula = time ~ diam + type + diam * type, data = p)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -20.453  -5.953  -2.592   5.009  32.862 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)  17.41075   12.21709   1.425  0.16046   
    ## diam          0.62491    1.07514   0.581  0.56375   
    ## type1       -10.62474   14.56155  -0.730  0.46908   
    ## type2        28.41439   26.54669   1.070  0.28970   
    ## type3       -22.84067   21.29184  -1.073  0.28864   
    ## type4       -31.39773   15.55788  -2.018  0.04907 * 
    ## diam:type1    1.52955    1.34287   1.139  0.26024   
    ## diam:type2   -0.03004    2.15439  -0.014  0.98893   
    ## diam:type3    3.92621    2.18973   1.793  0.07914 . 
    ## diam:type4    3.81417    1.24636   3.060  0.00358 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 10.83 on 49 degrees of freedom
    ## Multiple R-squared:  0.7258, Adjusted R-squared:  0.6755 
    ## F-statistic: 14.41 on 9 and 49 DF,  p-value: 4.96e-11

## Interpretation

### β0, βi

(type 5 is the reference)  
time = β0 + β1*diam + β2*type1 + β3*type2 + β4*type3 + β5*type4 +  
β6*(diam*type1) + β7*(diam*type2) + β8*(diam*type3) + β9*(diam\*type4) +
ε

plate(type 1): β0 + β1*diam + ε  
bowl(type 2): (β0+β2) + (β1+β6)*diam + ε

#### Intercept items

β0: (baseline, here is type 5)  
the mean grinding and polishing time(in minutes) in 0 diameter(in
inches) for baseline type, here is the plate.  
Yet it’s not logically meaningful, so β0 has no interpretation in the
problem context here

β2:  
difference in the mean grinding and polishing time(in minutes) for
bowl(type2) compared to plate(type1, baseline) at diam = 0  
Yet it’s not logically meaningful, so β0 has no interpretation in the
problem context here.

#### Slope items

β1:  
The increase in mean grinding and polishing time(in minutes) for every 1
inches of diameter increase for baseline type, here is the plate.

β6:  
Difference in slope for bowl vs. plate  
i.e.,  
For each additional inch in diameter for plate and bowl, the time of
polishing bowls increases by 1.52 more than that time of polishing
plate.

Others are similar…

### p-value for model as a whole (global T-test)

Global test, see p-value of 4.828e-11

H0: β1 = β2 = β3 = … β9 = 0; no relationship  
H1: at least one is not 0; relationship

And p-value, R-squared, … for the global test is not going to change.

There is a chance of XX% that we would get a sample result as extreme as
the one we obtain if there’s no relationship between Y and any of
predictors.  
DON’T USE P-VALUE AS MODEL SELECTION SINCE IT GIVES THE COMPARISON OF K
PREDICTORS TO NONE

### p-value for individual predictor (individual T-test)

For categorical variables, use the smallest p-value to determine whether
categorical variable has relationship with Y.

## Another example

``` r
re <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/RealEstate.xlsx')

table(re$Status)
```

    ## 
    ## Foreclosure     Regular  Short Sale 
    ##         162         103         516

``` r
re$Status <- as.factor(re$Status)
relm <- lm(Price ~ Bedrooms + Bathrooms + Size + Status, data = re)
summary(relm)
```

    ## 
    ## Call:
    ## lm(formula = Price ~ Bedrooms + Bathrooms + Size + Status, data = re)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1243571  -104020   -11043    65370  3902344 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       21595.39   38467.52   0.561  0.57469    
    ## Bedrooms         -88542.29   13104.93  -6.756 2.78e-11 ***
    ## Bathrooms         47260.03   16311.00   2.897  0.00387 ** 
    ## Size                293.32      17.06  17.195  < 2e-16 ***
    ## StatusRegular    208722.97   30641.27   6.812 1.93e-11 ***
    ## StatusShort Sale -20767.89   21768.32  -0.954  0.34036    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 241100 on 775 degrees of freedom
    ## Multiple R-squared:  0.5259, Adjusted R-squared:  0.5229 
    ## F-statistic: 171.9 on 5 and 775 DF,  p-value: < 2.2e-16

``` r
relm_inter <- lm(Price ~ Bedrooms + Bathrooms + (Size*Status), data = re)
# or if writing in this ways is still the same result
# relm_inter <- lm(Price ~ Bedrooms + Bathrooms + Size + Status + (Size*Status), data = re)
summary(relm_inter)
```

    ## 
    ## Call:
    ## lm(formula = Price ~ Bedrooms + Bathrooms + (Size * Status), 
    ##     data = re)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -980757  -92815  -19945   65506 2853982 
    ## 
    ## Coefficients:
    ##                         Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)             81725.61   45605.51   1.792 0.073522 .  
    ## Bedrooms               -92367.21   11754.72  -7.858 1.31e-14 ***
    ## Bathrooms               49150.69   14668.09   3.351 0.000845 ***
    ## Size                      263.70      22.31  11.822  < 2e-16 ***
    ## StatusRegular         -488289.92   66327.27  -7.362 4.65e-13 ***
    ## StatusShort Sale        27468.91   44361.86   0.619 0.535966    
    ## Size:StatusRegular        363.83      32.33  11.254  < 2e-16 ***
    ## Size:StatusShort Sale     -29.04      22.84  -1.272 0.203849    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 215900 on 773 degrees of freedom
    ## Multiple R-squared:  0.6207, Adjusted R-squared:  0.6173 
    ## F-statistic: 180.7 on 7 and 773 DF,  p-value: < 2.2e-16

# CHI-SQUARE DISTRIBUTION

## About chi-square distribution: sum of squared residuals

residual of each observation = (Oi-Ei) / sqrt(Ei), thus,  
sum of residual of each observation squared  
= X^2  
= sum_k(((Oi-Ei) / sqrt(Ei))^2)  
= sum_k(((Oi-Ei)^2 / Ei)) ~ chisq for all X^2 \>= 0

NOTE the parameter in chisq.test() when performing specific test  
It’s different for test for goodness of fit and test of independence

### Details about residuals ~ N(0, σ) and chi-square

Normality of residuals means that they are distributed symmetrically
around zero, with no skewness or kurtosis. This assumption implies that
the model captures the main patterns and sources of variation in the
data, and that the errors are random and independent.

Normality of residuals is important for several reasons.  
First,  
it affects the accuracy and confidence of the hypothesis tests and
confidence intervals for the regression coefficients, as they are based
on the standard errors of the estimates, which depend on the normality
of residuals.  
Second,  
it affects the validity of the model selection criteria, like R-squared,
adjusted R-squared, and the Akaike information criterion (AIC), which
measure the goodness of fit and the trade-off between complexity and
simplicity of the model.  
(AIC could be a criteria for backward, forward, stepwise method of model
selection)  
Third,  
it affects the detection and treatment of outliers and influential
points, which are observations that have a large impact on the model fit
and the parameter estimates.

Residual(ε) is a process parameter…

### Sum up

The reason we use chi-square distribution for test of goodness of fit
and test of independence is that, we want to check the deviation of
observations from expected values to see whether that observations come
from a specific distribution, and chi-square has the property to do this
under the normality of residual

assumption:  
z-score ~ N(0, σ) and sum_k(z-score) ~ chisq(df = k - 1),  
thus, with the assumption of residuals ~ N(0, σ),  
we use sum_k(residuals^2) ~ chisq(df = k - 1) for chi-square test

## Chi-squared one-sample goodness of fit test

Can be used to evaluate the hypothesis that a sample is taken from a
population with an assumed specific probability distribution.  
(e.g. whether a die is fair or not \>\>\> uniform probability
distribution)

H0: {p1, p2, …, pk} = {p10, p20, …, pk0}  
H1: at least one pi != pi0

Assumption:  
1. Random sampling  
2. Stability over time  
3. Ei \>= 5 for all i = 1, …, k, where Ei is the expected count in
category i under H0 = n\*p0

Sampling distribution:  
X^2 = sum_k(((Oi-Ei)^2 / Ei)) ~ chisq(df = k - 1) for all X^2 \>= 0

(e.g. whether a die is fair or not)

``` r
# actual number of absences per day M-F
ObsCounts <- c(15, 12, 9, 9, 15)
# expected number if all days are the same
ExpCounts <- rep(12, times = 5)
# expected probabilities
ExpProb <- ExpCounts/sum(ExpCounts)

# chi-square one-sample goodness of fit test 
resultM  <- chisq.test(ObsCounts, p = ExpProb)
resultM
```

    ## 
    ##  Chi-squared test for given probabilities
    ## 
    ## data:  ObsCounts
    ## X-squared = 3, df = 4, p-value = 0.5578

``` r
# cell by cell contributions to observed Chi-Square value: ((O-E)^2)/E
(resultM$residuals)^2
```

    ## [1] 0.75 0.00 0.75 0.75 0.75

``` r
# cell-by-cell residuals with signs of differences; residual = (O-E)/sqrt(E)
(resultM$residuals)
```

    ## [1]  0.8660254  0.0000000 -0.8660254 -0.8660254  0.8660254

## Chi-squared test for independence

Be used to test relationship between 2 variables  
H0: no relationship (in problem context)  
H1: relationship (in problem context)

Assumption:  
1. Random sampling  
2. Stability over time  
3. Eij \>= 5 for all i, j (i rows, j cols)

Sampling distribution:  
X^2 = sum(((Oij-Eij)^2 / Eij)) ~ chisq(df = (i-1)\*(j-1)) for all X^2
\>= 0

NOTE:  
X^2 doesn’t have real meaning in the practical situation;  
we only use it to get the p-value and perform the test,  
thus,  
HOW TO SUM UP AND GET THE APPROPRIATE X^2 DEPENDS THE QUESTION YOU GET

(e.g. shopping in a store has relationship with advertisement)

``` r
library(readxl)
Transit<-read_excel("C:/Users/Yvonne/Desktop/UMN Courses/6121/Transit.xlsx", na="NA", col_names = TRUE)

# create and save contingency table
# remember to drop index column if having one
# table uses cross-classifying factors to build a contingency table of the counts at each combination of factor levels
TransitTable <- table(Transit)
TransitTable
```

    ##         Distance
    ## Class    1-100 101-200 201-300 301-400 401-500
    ##   First      6       8      15      21      10
    ##   Second    14      16      17      14       6
    ##   Third     21      18      16      12       6

``` r
# chi-square test of independence
resultT <- chisq.test(TransitTable)
resultT
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  TransitTable
    ## X-squared = 15.923, df = 8, p-value = 0.0435

``` r
# cell-by-cell contributions to observed Chi-Square value: ((O-E)^2)/E
(resultT$residuals)^2
```

    ##         Distance
    ## Class         1-100    101-200    201-300    301-400    401-500
    ##   First  3.22682927 1.67936508 0.02500000 3.37659574 1.75151515
    ##   Second 0.00511285 0.26474058 0.05263682 0.19339632 0.25466757
    ##   Third  2.43376044 0.46502935 0.13187215 1.54905421 0.51318804

``` r
# cell-by-cell residuals with signs of differences; residual = (O-E)/sqrt(E)
(resultT$residuals)
```

    ##         Distance
    ## Class         1-100    101-200    201-300    301-400    401-500
    ##   First  -1.7963377 -1.2959032  0.1581139  1.8375516  1.3234482
    ##   Second  0.0715042  0.5145295  0.2294271 -0.4397685 -0.5046460
    ##   Third   1.5600514  0.6819306 -0.3631420 -1.2446101 -0.7163714

``` r
# cell-by-cell expected values
resultT$expected
```

    ##         Distance
    ## Class     1-100 101-200 201-300 301-400 401-500
    ##   First  12.300   12.60   14.40  14.100    6.60
    ##   Second 13.735   14.07   16.08  15.745    7.37
    ##   Third  14.965   15.33   17.52  17.155    8.03
