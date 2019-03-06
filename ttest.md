One-Sample T-Test
================

R has a built-in function to perform the one-sample t-test. In this notebook we will illustrate how to perform the one-sample t-test using the built-in function, as well as performing the test by basic calculations.

Using Built-in Function
-----------------------

If you are familar with R, you will know that there are countless of packages in R that can perform all kinds of statistical procedures. The function that we will use here, `t.test()`, sits nicely in the `stats` package that comes in base R, so you don't need to install or load any package to use this function.

The `t.test()` function can be used for both one-sample and two-sample tests. Here we will focus only on the one-sample case. To see the details of the function, we can type `?t.test` in the console.

To use this function, we need to provide it some data. Let's randomly generate some data first:

``` r
set.seed(1000) #Set random seed so we can regenerate example
data = rt(50, df = 19) #Randomly generate data from the t-distribution
```

There are a couple functions that we used here. `set.seed()` is to fix a random seed in R so we can reproduce the same random data everytime we run the code. `rt()` is a function to generate random data from the t-distribution. There are many ways we can generate random data in R (and in statistics in general), and the most common way is to generate data through a known probability distribution.

We know that the data is generated from a distribution with mean 0. But suppose we want to test the hypothesis that the population mean should be larger than 1, i.e. *H*<sub>0</sub> : *μ* = 1 vs *H*<sub>1</sub> : *μ* &gt; 1.

``` r
t.test(x = data, alternative = 'greater', mu = 1)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  data
    ## t = -6.8999, df = 49, p-value = 1
    ## alternative hypothesis: true mean is greater than 1
    ## 95 percent confidence interval:
    ##  -0.2613992        Inf
    ## sample estimates:
    ##   mean of x 
    ## -0.01481726

The `alternative` arguement determines the direction of the test, while `mu` specifies the hypothesize mean By default, `alternative` is set as "two-sided", while `mu` is set as 0. In the output, `t` is the calculated test statistics. The above result shows that we fail to reject the null hypothesis, because our p-value is big (we will not be able to reject the null hypothesis for any value of alpha in this example).

Using Basic Calculations
------------------------

In the (unlikely) event that you don't have the data, but only have the summary statistics (mean and standard deviation), we can still perform the t-test using basic calculations. Let reproduce the example from the text.

``` r
mu0 = 12 #The hypothesized mean
s = 5 #The sample standard deviation
xbar = 13.5 #The sample mean
n = 50 #The sample size
```

To calculate the test statistics, we can just directly use the test statistics formula

``` r
test.stat = (xbar - mu0)/(s/sqrt(n))
print(test.stat)
```

    ## [1] 2.12132

To get the critical value, we can use the `qt()` function (since we now assume the t-distribution):

``` r
crit.val = qt(0.05, df = n-1, lower.tail = F) 
print(crit.val)
```

    ## [1] 1.676551

The `q` in `qt()` stands for quantile. The `df` argument is for the degrees of freedom, which is n-1 for the one-sample t-test. The argument `lower.tail = F` is to tell R that we want to find the critical value which gives a probability of 0.05 on the upper tail. If we were to do a lower-tailed test, the command will be `qt(0.05, df = n-1, lower.tail = T)` (or by default, `lower.tail` is TRUE if there is no input for this argument). Notice this critical value is the exact value coming from the t-distribution, as oppose to just an approximation when using a printed table.

To obtain the p-value, we can use the `pt()` function:

``` r
p.val = pt(test.stat, df = n-1, lower.tail = F)
print(p.val)
```

    ## [1] 0.01948903

The `p` in `pt()` stands for probability, or distribution function in general. The p-value is less than *α* = 0.05, and the test statistics is larger than the critical value, hence we will reject the null hypothesis.

If this were a two-tailed test, remember we need to multiply the p-value by 2:

``` r
p.val2 = 2*pt(abs(test.stat), df = n-1, lower.tail = F) #If we were to do a two-tailed test
print(p.val2)
```

    ## [1] 0.03897806
