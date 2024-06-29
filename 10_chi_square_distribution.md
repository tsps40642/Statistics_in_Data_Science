CHI-SQUARE DISTRIBUTION
================

# About chi-square distribution: sum of squared residuals

residual of each observation = (Oi-Ei) / sqrt(Ei), thus,  
sum of residual of each observation squared  
= X^2  
= sum_k(((Oi-Ei) / sqrt(Ei))^2)  
= sum_k(((Oi-Ei)^2 / Ei)) ~ chisq for all X^2 \>= 0

NOTE the parameter in chisq.test() when performing specific test  
It’s different for test for goodness of fit and test of independence

## Details about residuals ~ N(0, σ) and chi-square

Normality of residuals means that they are distributed symmetrically
around zero, with no skewness or kurtosis. This assumption implies that
the model captures the main patterns and sources of variation in the
data, and that the errors are random and independent.

Normality of residuals is important for several reasons:  
1. It affects the accuracy and confidence of the hypothesis tests and
confidence intervals for the regression coefficients, as they are based
on the standard errors of the estimates, which depend on the normality
of residuals.  
2. It affects the validity of the model selection criteria, like
R-squared, adjusted R-squared, and the Akaike information criterion
(AIC), which measure the goodness of fit and the trade-off between
complexity and simplicity of the model.  
(AIC could be a criteria for backward, forward, stepwise method of model
selection)  
3. It affects the detection and treatment of outliers and influential
points, which are observations that have a large impact on the model fit
and the parameter estimates.

Residual(ε) is a process parameter

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

# Two scenarios to use Chi-squared tests

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
category i  

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
```

    ## Warning: package 'readxl' was built under R version 4.3.3

``` r
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
