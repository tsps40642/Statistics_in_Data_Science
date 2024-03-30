CATEGORICAL PREDICTORS
================

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

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

1.  Interpretation of the global p-value: There is a chance of XX% that
    we would get a sample result as extreme as the one we obtain if
    there’s no relationship between Y and any of predictors. DON’T USE
    P-VALUE AS MODEL SELECTION SINCE IT GIVES THE COMPARISON OF K
    PREDICTORS TO NONE.  
2.  Note that to determine whether there’s difference between type and
    Y, we need to look at the lowest p-value since even if there are
    other higher p-values, as long as we have one type suggests
    difference exists, we would conclude difference exists.

### p-value for individual predictor (individual T-test)

For categorical variables, use the smallest p-value to determine whether
categorical variable has relationship with Y.

H0: type i has no difference with the baseline type  
H1: type i has difference with the baseline type
