INTERACTIONS
================

# Codes

Remember that one should always included the main effects when
considering interactions.

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.3.3

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

# Interpretation

### β0, βi

(type 5 is the reference)  
time = β0 + β1*diam + β2*type1 + β3*type2 + β4*type3 + β5*type4 +  
β6*(diam*type1) + β7*(diam*type2) + β8*(diam*type3) + β9*(diam\*type4) +
ε

plate(type 5): β0 + β1*diam + ε  
bowl(type 1): (β0+β2) + (β1+β6)*diam + ε

#### Intercept items

β0: (baseline, here is type 5)  
the mean grinding and polishing time(in minutes) in 0 diameter(in
inches) for baseline type, here is the plate.  
Yet it’s not logically meaningful, so the intercept has no
interpretation in the problem context here

β2:  
difference in the mean grinding and polishing time(in minutes) for
bowl(type2) compared to plate(type1, baseline) at diam = 0  
Yet it’s not logically meaningful, so the intercept has no
interpretation in the problem context here.

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

Interpretation of the global p-value: There is a chance of XX% that we
would get a sample result as extreme as the one we obtain if there’s no
relationship between Y and any of predictors.  
DON’T USE P-VALUE AS MODEL SELECTION SINCE IT GIVES THE COMPARISON OF K
PREDICTORS TO NONE

### p-value for individual predictor (individual T-test)

For categorical variables, use the smallest p-value to determine whether
categorical variable has relationship with Y.

Similar as in the categorical_predictor, Use of the individual p-value:
Note that to determine whether there’s difference between type and Y, we
need to look at the lowest p-value since even if there are other higher
p-values, as long as we have one type suggests difference exists, we
would conclude difference exists.

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
