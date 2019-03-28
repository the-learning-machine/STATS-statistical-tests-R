Two-Sample Z-Test
================

R doesn't have a built-in function to conduct the two-sample z-test. Fortunately the math is simple enough for us to carry out the test using only a few functions. Let start with same data.

``` r
mu0 = 0 #The hypothesized difference
xbar1 = 3.36 #The sample mean for group 1
xbar2 = 3.03 #The sample mean for group 2
sigma1 = 0.45 #The population standard deviation for group 1
sigma2 = 0.41 #The population standard deviation for group 2
n1 = 50 #The sample size for group 1
n2 = 45 #The sample size for gorup 2
```

To calculate the test statistics, we can just directly use the test statistics formula

``` r
test.stat = ((xbar1 - xbar2) - mu0)/sqrt(sigma1^2/n1 + sigma2^2/n2)
print(test.stat)
```

    ## [1] 3.739979

To get the critical value, we can use the `qnorm()` function (since we now assume the standard normal distribution):

``` r
crit.val = qnorm(0.025, lower.tail = F)
print(crit.val)
```

    ## [1] 1.959964

The argument `lower.tail = F` is to tell R that we want to find the critical value which gives a probability of 0.025 on the upper tail.

To obtain the p-value, we can use the `pnorm()` function (remember, for a two-tailed test, we need to multiple the resulting probability by 2):

``` r
p.val = 2*pnorm(abs(test.stat), lower.tail = F)
print(p.val)
```

    ## [1] 0.0001840358
