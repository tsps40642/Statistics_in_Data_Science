BASIC DISTRIBUTION
================

For details of each distribution, refer to the pdf file

# Discrete distribution

## Bernoulli

X = “success” or “failure”, ∈ {0, 1}  
1. Parameters: p  
2. Mean: p  
3. Sd: sqrt(p\*q)

## Binomial

X = \# of successes in n Ber. trials, ∈ {0, …, n}  
1. Parameters: n, p  
2. Mean: n*p  
3. Sd: sqrt(n*p\*q)

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

#### You can derive qnorm() using pnorm(), and vice versa

``` r
# interval upper and lower bounds
lower_bound <- qnorm(0.05, mean = 96, sd = 8.5)
upper_bound <- qnorm(0.95, mean = 96, sd = 8.5)

# insert bounds to pnorm() to get cumulative probability
l_prob <- pnorm(lower_bound, mean = 96, 8.5)
u_prob <- pnorm(upper_bound, mean = 96, 8.5)
```

#### Distribution of p_hat, the proportion of success in n trials

1.  p_hat ∈ {0/n, 1/n, …, n/n}  
2.  Mean: np/n = p  
3.  Sd: sqrt((p*q)/n) = sqrt(n*p\*q)/n

## Poisson

X = \# of successes in a given interval, ∈ \[0, $∞$)  
1. Parameter: λ  
2. Mean: λ  
3. Sd: sqrt(λ)

## Negative Binomial

X = \# of trials until the rth success occurs (i.e. there are r-1
successes in X trials before the last success)  
1. Parameters: r, p  
2. Mean: r/p  
3. Sd: sqrt((r\*q)/p<sup>2</sup>)

# Conti. distribution

## Normal

1.  Parameters: X~N(μ, σ)  
2.  Mean: μ  
3.  Sd: σ

#### Standard normal distribution

1.  Parameters: X~N(0, 1)  
2.  Mean: 0  
3.  Sd: 1

## T distribution

Note that we use T instead of N(0, 1) due to unknown σ, not because
small sample size!!  
1. Parameters: X~T(df = n-1) 2. Mean: 0  
3. Sd: \>1, indicating a larger variation than N(0, 1)  
4. Shape depends on degrees of freedom (df)

#### Degree of freedom

1.  In statistics, df is the number of values in the final calculation
    of a statistic that are free to vary  
    i.e. the number of INDEPENDNET pieces of information that go into
    the estimate of a parameter  
2.  df is a function of sample size n s.t. as n -\> $∞$, t -\> N(0, 1)

# Sampling error vs. non-sampling error

## Sampling error:

Arises from the fact that we are taking a subset of information from the
population/process

1.  Definition: Sampling error refers to the variability between a
    sample statistic (such as a sample mean or proportion) and the true
    population parameter that it represents.  
2.  Cause: Arise from the fact that we’re taking subset of information
    from the process; the sample would not match what exists in the
    process necessarily.  
3.  Nature: Sampling error is a random error, meaning it can occur by
    chance, and its magnitude can vary from one sample to another.
4.  Reducibility: Increasing the sample size generally reduces sampling
    error, as larger samples tend to provide more accurate estimates of
    population parameters.  
5.  Examples: If you were to take multiple random samples from the same
    population and calculate the mean each time, the variability in
    those sample means relative to the true population mean is the
    sampling error.

## Non-sampling error:

Bias, due to problems with sampling and /or data collection process

1.  Definition: Non-sampling error encompasses all errors in survey or
    data collection that are not related to the act of sampling. It
    includes errors introduced during data processing, measurement,
    response bias, and other factors.  
2.  Causes: Non-sampling errors can result from mistakes in data entry,
    faulty measurement instruments, respondent bias, non-response bias,
    or errors in data analysis.  
3.  Nature: Non-sampling errors can be systematic or random. Systematic
    errors are consistent and tend to affect the entire dataset
    consistently, while random errors are unpredictable and may vary
    from one observation to another.  
4.  Reducibility: While some non-sampling errors may be reduced through
    improved survey design and data collection methods, others may be
    inherent and challenging to eliminate completely.  
5.  Examples: Mistakes in questionnaire wording, interviewer bias,
    errors in data coding, and issues related to non-response (when
    certain groups are less likely to participate in a survey) are
    examples of non-sampling errors.

# Sampling distribution

## Concepts

1.  Probability distribution vs. sampling distribution vs. sample
    distribution:

    - Probability distribution: the possible values of X and their
      likelihoods, use parameters  
    - Sampling distribution: the possible statistic values (e.g., X_bar)
      and their likelihoods  
    - Sample distribution: the observed values of X and their
      frequencies, use statistics

2.  Sampling distribution must be a prob. ditri., not vice versa

3.  As a characteristic of the process, the sampling distr. is unknown

4.  Sampling distri. is capturing the variability of statistic due to
    sampling error:

    - SE = sd/sqrt(n)  
    - SE measures the sampling error  
    - SE arise from the fact that X_bar is calculated from a sbuset of a
      population/process

5.  So we need to set up assumptions when using:

    - Random sampling  
    - Stability over time  
    - Other specific assumptions

## Some sampling distrinutions for statistics

1.  For sample proportion p_hat, which used to estimate process
    proportion p
    - Assume:
      - Random sampling  
      - Stability over time  
    - then X = \# of success ~ B(np, sqrt(npq))  
    - so p_hat ~ B(p, (sqrt(npq)/n)=sqrt(pq/n))
2.  for X_bar
    - Assume:
      - Random sampling  
      - Stability over time  
      - if n\<30 X~N(., .): we add this assumptions that X is sampled
        from N(., .) when sample size is less than 30  
    - in normal: X_bar~N(μ, σ/sqrt(n))  
    - in t:
      - \[t-value of X_bar = (X_bar-μ) / (se=s/sqrt(n))\]~t(df=n-1)

## Inference procedure

1.  Make assumptions: some common assumptions are
    1.  Random sampling  
    2.  Stability over time  
2.  Identify the sampling distribution
3.  Check the assumptions as able
4.  Use the sampling distribution and sample to do the inference

### 2 common inference questions

1.  Estimation: what is the parameter?  
2.  Testing: how does parameter compare to a specific value of
    parameter?
