Untitled
================

# Points worth notcing

Here I’ve listed some most important points to notice:  
1. NEVER ACCEPT H0  
2. whenever I DON’T know σ of the population, use T-DISTRIBUTION,
regardless of the sample size  
3. break even situation \>\> average charge \* E(X) of event happening  
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

#### read excel

library(readxl)  
qc \<- read_excel(‘Quiz-Credit.xlsx’)

#### read csv

data \<- read.csv(‘title.csv’, header = T)

#### read txt

library(readxl)  
data \<- read.table(‘title.txt’)
