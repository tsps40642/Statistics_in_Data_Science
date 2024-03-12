# Statistics_in_Data_Science

## Overview
This is a notebook to introduce statistical methods for data science.  
I focus on introducing hypothesis, assumptions, and coding for each of the statiscal tests.  
We will go step by step to look deeper into statistics for data analysis.   

The following is the outline and some bullet points I would like to mention in the corresponding section:  
- BASIC    
- BASIC DISTRIBUTION  
    - rbinom
    - dbinom
    - pbinom
    - qbinom
    - how to get upper bound and lower bound for interval from the codes above   
- ESTIMATION
    - Sample distribution vs. Sampling distribution
    - 100(1-α)% CI for a process mean μ
    - 100(1-α)% CI for a process proportion p
    - Interpret the 100(1-α)% CI
- HYPOTHESIS TESTING
    - Hypothesis test for a process mean μ
    - Hypothesis test for a process proportion p
    - Testing normality
    - Interpret p-value
- TWO GROUPS TESTING
    - Testing for paired samples
    - Testing for independent samples, UNEQUAL variances
    - Testing for independent samples, EQUAL variances
- ONEWAY ANOVA
    - Conduct oneway ANOVA
    - Checking assumptions
    - On the conclusion of a relationship i.e. ANOVA Ha is accepted
    - Supplement: oneway ANOVA as a special case for two groups with equal variances
- LINEAR REGRESSION
    - State the linear model
    - Interpretation
    - ASA (American Statistical Association) Statement: Principles
    - Global T-test: for the whole model
    - Individual two-tailed t tests: for each of the variable
    - Point estimation: estimate yi|{X_sub_i}
    - Interval estimation: predictors
    - Interval estimation for predicting µ_sub_Y|{X_sub_i}
    - Residuals
    - Check assupmtion about ε
    - Determine the best model
    - supplemental test: Hypothesis test: if βi is less than / greater than / equal to a secific value
- NON-LINEAR REGRESSION
    - Polynomial Regression  
- CATEGORICAL PREDICTORS
    - Interpretation: β0, βi, p-value for model as a whole (global T-test), p-value for individual predictor (individual T-test)  
- INTERACTIONS
    - Interpretation: β0, βi, p-value for model as a whole (global T-test), p-value for individual predictor (individual T-test)  
- CHI-SQUARE DISTRIBUTION
    - About chi-square distribution: sum of squared residuals
    - Chi-squared one-sample goodness of fit test
    - Chi-squared test for independence
