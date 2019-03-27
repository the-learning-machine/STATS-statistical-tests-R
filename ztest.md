One-Sample Z-Test
================

R doesn't have a built-in function to conduct the one-sample z-test. Fortunately the math is simple enough for us to carry out the test using only a few functions. Let start with same data.

``` r
mu0 = 12 #The hypothesized value
sigma = 2.6 #The population standard deviation
xbar = 13.5 #The sample mean
n = 50 #The sample size
```

To calculate the test statistics, we can just directly use the test statistics formula

``` r
test.stat = (xbar - mu0)/(sigma/sqrt(n))
print(test.stat)
```

    ## [1] 4.079462

To get the critical value, we can use the `qnorm()` function (since we now assume the standard normal distribution):

``` r
crit.val = qnorm(0.05, lower.tail = F)
print(crit.val)
```

    ## [1] 1.644854

The argument `lower.tail = F` is to tell R that we want to find the critical value which gives a probability of 0.05 on the upper tail. If we were to do a lower-tailed test, the command will be `qnorm(0.05, lower.tail = T)` (or by default, `lower.tail` is TRUE if there is no input for this argument).

To obtain the p-value, we can use the `pnorm()` function:

``` r
p.val = pnorm(test.stat, lower.tail = F)
print(p.val)
```

    ## [1] 2.257e-05

If this were a two-tailed test, remember we need to multiply the p-value by 2:

``` r
p.val2 = 2*pnorm(abs(test.stat), lower.tail = F) #If we were to do a two-tailed test
print(p.val2)
```

    ## [1] 4.514001e-05
