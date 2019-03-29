Paired T-Test
================

The paired t-test shares the same built-in function in R with the one-sample t-test. In this notebook we will illustrate how to perform the paired t-test using the built-in function, as well as performing the test by basic calculations.

Using Built-in Function
-----------------------

In our notebook for the one-sample t-test, we described the usage of the `t.test()` function. The `t.test()` function can be used for both one-sample and two-sample tests. Here we will focus on the two-sample case, and in particular, paired data. To see the details of the function, we can type `?t.test` in the console.

To use this function, we need to provide it some data. Let's start by creating the data:

``` r
before = c(164, 137, 133, 157, 173, 143, 144, 180, 178, 154) #Data collected before treatment
after = c(129, 127, 122, 120, 129, 122, 125, 139, 145, 139) #Data collected after treatment
D0 = 0 #hypothesized difference
```

``` r
test = t.test(x = before, y = after, alternative = 'greater', mu = D0, paired = TRUE)
print(test)
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  before and after
    ## t = 6.5764, df = 9, p-value = 5.1e-05
    ## alternative hypothesis: true difference in means is greater than 0
    ## 95 percent confidence interval:
    ##  19.18552      Inf
    ## sample estimates:
    ## mean of the differences 
    ##                    26.6

Since we are testing if the before treatment data is larger than the after treatment data, we set the alternative to 'greater'. Note that we now set the argument `paired` to `TRUE`. This is to tell R that the two data sets we inputted are paired data.

Suppose we want to extract the various numerical results from the test, such as the p-value or the test statistics. We can do so by extracting the corresponding values from the output:

``` r
test$statistic     #Test statistics
```

    ##        t 
    ## 6.576427

``` r
test$parameter     #Degrees of freedom
```

    ## df 
    ##  9

``` r
test$p.value       #p-value
```

    ## [1] 5.10044e-05

Using Basic Calculations
------------------------

Of course, we can also perform the hypothesis test via simple math. To calculate the test statistics, we can just directly use the test statistics formula:

``` r
di = before - after #Calculate the differences for each pair
d.bar = mean(di) #Mean of differences
s.d = sd(di) #Sample standard deviation of the differences
n = length(before) #Number of data pairs
alpha = 0.01     #Alpha value
test.stat = (d.bar - D0)/(s.d/sqrt(n))
print(test.stat)
```

    ## [1] 6.576427

To get the critical value, we can use the `qt()` function (for the t-distribution):

``` r
crit.val = qt(p = alpha, df = n - 1, lower.tail = F)
print(crit.val)
```

    ## [1] 2.821438

The argument `df` is for the degrees of freedom, where the argument `lower.tail = F` is to tell R that we want to find the critical value which gives a probability of 0.01 on the upper tail. If we were to do a lower-tailed test, the command will be `qt(p = 0.01, df = n - 1, lower.tail = T)` (or by default, `lower.tail` is TRUE if there is no input for this argument).

To obtain the p-value, we can use the `pt()` function:

``` r
p.val = pt(test.stat, df = n - 1, lower.tail = F)
print(p.val)
```

    ## [1] 5.10044e-05

If this were a two-tailed test, remember we need to multiply the p-value by 2:

``` r
p.val2 = 2*pt(abs(test.stat), df = n - 1, lower.tail = F) #If we were to do a two-tailed test
print(p.val2)
```

    ## [1] 0.0001020088
