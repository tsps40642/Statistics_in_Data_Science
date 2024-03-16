BASIC
================

# Points worth notcing

## Some most important points to notice:

1.  Never accept H0  
2.  Whenever I don’t know σ of the population, use T-DISTRIBUTION,
    regardless of the sample size  
3.  Break even situation means average charge \* E(X) of event
    happening  
4.  P(X = x) on any given points on a conti. distri. must be 0; we can
    only get the prob. within the interval  
5.  If given 2 set of nominal-level variables, we should use table
    rather than a boxplot since the latter requires an at least
    ordinal-level data  
6.  Interpretation of p-value is irrelevant to the significance level

## Basic concepts of Statistics

1.  Process: a mechanism that generates data over time  
    (e.g., the generating process for customers’ experience of and
    reactions to our products)

2.  Statistics transform data into information. There are 2 main fields:

    1.  Descriptive statistics: organize and summarize info. into a
        usable form  
    2.  Inferential statistics: draw conclusions (estimates,
        predictions, …) about a process based on sample data

3.  Important definitions:

    1.  Populations: all members from a process at a given point in time
        (i.e. a cross-section of a process, like “all cars in the US
        this month”)  
    2.  Sample: a subset of the population/process  
    3.  Census: a sample of an entire population of interest. Note that
        population is generally finite, while process is theoretically
        infinite, and we conceivably could access every member in the
        population, called census  
    4.  Measurement: values (numerical, textual, date, …) obtained from
        each member of the sample. The dataset is a collection of
        measurements (which are known values) we got from the sample  
    5.  Parameter: number that refers to some characteristic of a
        process. Usually unknown and needs estimation  
    6.  Statistic: number that refers to some characteristic of a
        sample. The statistic is a summary of measurements

4.  Sampling methods: all with a goal to not having systematic bias

    1.  Simple random sampling  
    2.  stratified sampling  
    3.  Cluster sampling

5.  Data types: note that the latter order can also do things in the
    former order

    1.  Nominal:
        - categorical, measurements are in distinct categories without
          given order  
        - can calculate: frequency, proportions, mode  
        - can use: barchart, table  
    2.  Ordinal:
        - measurement categories are ordered  
        - can calculate: 1., median, percentiles, range, IQ range  
        - can use: 1., barchart with ordered values, box plot  
    3.  Interval:
        - quantitative, difference between scale values are comparable  
        - can calculate: 1., 2., mean, sd, skewness  
        - can use: 1., 2., histogram, scatter plot

6.  Graphs:

    1.  Distinguish barchart from histogram:
        - x-axis of a barchart is nominal/ordinal data, e.g., types,
          regions, species, …
        - x-axis of a histogram is interval data, e.g., age, length,
          years, …  
    2.  Distribution shape:
        - Symmetric  
        - Positively (right) skewed  
        - Negatively (left) skewed

7.  Numerical description

    1.  Central tendency: mean, median, mode  
    2.  Variability: variance, sd, range, IQ (interquartile) range
        (Q3-Q1)  
        (More info. about using n-1 in variance, you can refer to:
        <https://web.ma.utexas.edu/users/mks/M358KInstr/SampleSDPf.pdf>.
        Basically it’s saying that using n–1 in variance for a sample
        helps to make sure the number derived is a better representation
        of the whole population)  
    3.  Relative standing: z-score (unit-free), percentile (Q1, Q2, Q3)

8.  Empirical rule: 68-95-99.7 for bell-shaped distributions

9.  Estimators vs. Estimates

    1.  Estimators are functions of sample data drawn from an unknown
        population  
    2.  Estimates are numeric values computed by estimators based on the
        sample data

10. Probability vs. Proportion

    1.  Probability:
        - is an unknown situation  
        - a probability distribution represents a possible event and its
          uncertain likelihood  
    2.  Proportion:
        - is a known statistic  
        - we use a histogram/barchart to describes known sample
          information

11. Parameters (interval data)

    1.  Expectation:
        - Estimated by y_bar = (Σ(i=1 to n) y_i)/n for observations y_1,
          …, y_n  
        - E(X+Y) = E(X) + E(Y)  
        - E(aY) = a\*E(Y), a = const.  
    2.  Variance:
        - Estimated by (Σ(i=i to n) (y_i-y_bar)<sup>2</sup>)/n  
        - Var(X) = E\[(X-E(X))<sup>2</sup>\] = E(X<sup>2</sup>) -
          \[E(X)\]<sup>2</sup> = Σ(i=1 to n) \[(x_i-μ)<sup>2</sup> \*
          P(x_i)\]  
        - Var(X+Y) = Var(X) + Var(Y), if X and Y are independent  
        - Var(aX+b) = a<sup>2</sup>\*Var(X), a, b = const.

# Basic probability theories

## Central limit theorem (CLT)

#### Theorem description

Let X_1, …, X_n denote a random sample of n independent observations
from an arbitrary form of distribution with the first moment and second
moments existing and finite, i.e. E(X_i)=μ and
Var(X_i)=σ<sup>2</sup>\<$∞$. Define
$\overline{X_n}=\frac{1}{n}\sum_{x = 1}^{n}X_i$, then the distribution
function of the r.v. W_n = ($\overline{X_n}$-μ)/(σ/n) -\> distribution
function of N(0, 1) as n -\> $∞$  
(For other expressions and detail, refer to pdf files)

#### Simple description

CLT states that, under appropriate conditions, the distribution of a
normalized version of the sample mean converges to a standard normal
distribution

#### Business applications of CLT

(TBD) 1. Note that since the computational ability for modern computers
have increased significantly, in practice, we use T distribution
(details in basic_distribution) if σ unknown, not relying on CLT (it’s
like a holdover from a pre-computer age)  
2. So concluded form 1., we use T is not because small n, is because
unknown σ

## Law of large number (LLN)

#### Theorem description

(weak LLN: converge in probability / strong LLN: converge almost surely.
Here we will skip their difference)  
{X_i, i=1 to n} are i.i.d. r.v. from an arbitrary form of distribution
with the first moment and second moments existing and finite,
i.e. E(X_i)=μ and Var(X_i)=σ<sup>2</sup>\<$∞$. Let
$\overline{X_n}=\frac{1}{n}\sum_{x = 1}^{n}X_i$, then $\overline{X_n}$
-\> μ as n -\> $∞$  
(For other expressions and detail, refer to pdf files)

#### Simple description

1.  LLN states that, as more observations are collected in a repeated
    experiment, the proportion of occurrence of A, Prop(A), converges to
    a fixed value defined as its probability P(A)  
2.  LLN states that, the average of the results obtained from a large
    number of i.i.d. random samples converges to a fixed value  
3.  LLN is important because it guarantees stable long-term results for
    the averages of some random events  
4.  LLN operates by washing out deviations under the required conditions

#### Business applications of LLN

(TBD)

# Basic codes: read in the dataset

``` r
# change the path to where your file is saved 
# so that you can use relative path (although I still use abs. paths below)
setwd('C:/Users/Yvonne/Desktop/UMN Courses/6121')  
```

#### read excel

``` r
library(readxl)  
data <- read_excel('C:/Users/Yvonne/Desktop/UMN Courses/6121/gpa.xlsx') 
```

#### read csv

``` r
data <- read.csv('C:/Users/Yvonne/Desktop/UMN Courses/6121/Books.csv', header = T) 
```

#### read txt

``` r
library(readxl)   
data <- read.table('C:/Users/Yvonne/Desktop/UMN Courses/6121/data.txt')  
```

    ## Warning in read.table("C:/Users/Yvonne/Desktop/UMN Courses/6121/data.txt"):
    ## incomplete final line found by readTableHeader on 'C:/Users/Yvonne/Desktop/UMN
    ## Courses/6121/data.txt'
