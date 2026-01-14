Paired Samples t-Test
================
For Leonardo Monte Külls

## GitHub Documents

The file shows how to implement a paired samples t-Test for an
experimental design of ‘before’ and ‘after’ experimental data for which
an observed value is compared. The question to be answered is, whether
there is a significant difference in the mean of the observed values.

The file contains the theory and an example implemented in R in a format
that can be published on Github.

## Including Code

The paired samples t-test is used to compare the means between two
related groups of samples. There are two groups of observed values
(i.e., a pair of values) for the same sample. A paired t-test can be
used to compare the mean before and after a treatment.

Paired t-test analysis is performed as follows:

- Calculate the mean of the group before and of the group after
- Calculate the difference (d) between each pair of values
- Compute the mean (m) and the standard deviation (s) of d
- Compare the average difference to 0. If there is any significant
  difference between the two pairs of samples, then the mean of d (m) is
  expected to differ clearly from 0.

**Important**: Paired t-test can be used only when the difference d is
normally distributed. This can be checked using Shapiro-Wilk test.

## Hypotheses

The research question is formulated as a hypothesis:

- whether the mean difference (m) is equal to 0
- whether the mean difference (m) is less than 0
- whether the mean difference (m) is greather than 0

In statistics, we can define the corresponding null hypothesis ($H_0$)
as follow:

$$H_0: m=0$$ $$H_0: m≤0$$ $$H_0:m≥0$$ The corresponding alternative
hypotheses ($H_a$ or $H_1$) are as follows:

$$H_a:m≠0$$ (different)

$$H_a:m>0$$ (greater)

$$H_a:m<0$$ (less)

Note that:

Hypotheses 1) are called two-tailed tests Hypotheses 2) and 3) are
called one-tailed tests

## Theory and test statistic

t-test statistisc values can be calculated using the following formula:

$$t=\frac{m}{s/\sqrt(n)}$$

where:

- m is the mean of the differences
- n is the sample size (i.e., size of d).
- s is the standard deviation of d

We can compute the p-value corresponding to the absolute value of the
t-test statistics (\|t\|) for the degrees of freedom (df): df=n−1.

If the p-value is inferior or equal to 0.05, we can conclude that the
difference between the two paired samples are significantly different.

## R function to compute paired t-test

To perform paired samples t-test comparing the means of two paired
samples (x and y), the R function t.test() can be used as follows:

    t.test(x, y, paired = TRUE, alternative = "two.sided")

with x,y: numeric vectors, paired: a logical value specifying that we
want to compute a paired t-test, alternative: the alternative
hypothesis. Allowed value is one of “two.sided” (default), “greater” or
“less” (see above)

## Example

``` r
# Data in two numeric vectors
# Observation before treatment
before <-c(200.1, 190.9, 192.7, 213, 241.4, 196.9, 172.2, 185.5, 205.2, 193.7)
# Observation after treatment
after <-c(392.9, 393.2, 345.1, 393, 434, 427.9, 422, 383.9, 392.3, 352.2)

# Create a data frame
testdata <- data.frame( 
                group = rep(c("before", "after"), each = 10),
                observation = c(before,  after)
                )
```

The data can be printed as a table.

``` r
# Print data
print(testdata)
```

    ##     group observation
    ## 1  before       200.1
    ## 2  before       190.9
    ## 3  before       192.7
    ## 4  before       213.0
    ## 5  before       241.4
    ## 6  before       196.9
    ## 7  before       172.2
    ## 8  before       185.5
    ## 9  before       205.2
    ## 10 before       193.7
    ## 11  after       392.9
    ## 12  after       393.2
    ## 13  after       345.1
    ## 14  after       393.0
    ## 15  after       434.0
    ## 16  after       427.9
    ## 17  after       422.0
    ## 18  after       383.9
    ## 19  after       392.3
    ## 20  after       352.2

## Compute

You can also embed plots, for example:

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  d
    ## W = 0.94536, p-value = 0.6141

From the output, the p-value is greater than the significance level 0.05
implying that the distribution of the differences (d) are not
significantly different from normal distribution. In other words, we can
assume the normality.

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.

## Do the t-Test

### Method

Compute paired t-test: The data are saved in two different numeric
vectors.

``` r
# Compute t-test
res <- t.test(before, after, paired = TRUE)
res
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  before and after
    ## t = -20.883, df = 9, p-value = 6.2e-09
    ## alternative hypothesis: true mean difference is not equal to 0
    ## 95 percent confidence interval:
    ##  -215.5581 -173.4219
    ## sample estimates:
    ## mean difference 
    ##         -194.49

The results are:

- t is the t-test statistic value (t = 20.88),
- df is the degrees of freedom (df= 9),
- p-value is the significance level of the t-test (p-value =
  6.210^{-9}).
- conf.int is the confidence interval (conf.int) of the mean differences
  at 95% is also shown (conf.int= \[173.42, 215.56\])
- sample estimates is the mean differences between pairs (mean =
  194.49).

### Note

If you want to test whether the average weight before treatment is less
than the average weight after treatment, type this:

``` r
resless <- t.test(before, after, paired = TRUE, alternative = "less")
res
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  before and after
    ## t = -20.883, df = 9, p-value = 6.2e-09
    ## alternative hypothesis: true mean difference is not equal to 0
    ## 95 percent confidence interval:
    ##  -215.5581 -173.4219
    ## sample estimates:
    ## mean difference 
    ##         -194.49

Or, if you want to test whether the average weight before treatment is
greater than the average weight after treatment, type this

``` r
t.test(before, after, paired = TRUE, alternative = "greater")
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  before and after
    ## t = -20.883, df = 9, p-value = 1
    ## alternative hypothesis: true mean difference is greater than 0
    ## 95 percent confidence interval:
    ##  -211.5623       Inf
    ## sample estimates:
    ## mean difference 
    ##         -194.49

## Interpretation

The p-value of the test is $6.210^{-9}$, which is less than the
significance level alpha = 0.05. We can then reject null hypothesis
$H_0$ that the values are the same and that the differences between
pairs are 0 or close to zero and conclude that the mean of the
differences of the paired observations before treatment is significantly
different from the average observation after treatment with a p-value =
$6.210^{-9}$.

The result of t.test() function is a list containing the following
components:

- statistic: the value of the t test statistics
- parameter: the degrees of freedom for the t test statistics
- p.value: the p-value for the test
- conf.int: a confidence interval for the mean appropriate to the
  specified alternative hypothesis.
- estimate: the means of the two groups being compared (in the case of
  independent t test) or difference in means (in the case of paired t
  test).

The format of the R code to use for getting these values is as follow:

### Accessing the results

``` r
### printing the p-value
res$p.value
```

    ## [1] 6.200298e-09

``` r
# printing the mean
res$estimate
```

    ## mean difference 
    ##         -194.49

``` r
# printing the confidence interval
res$conf.int
```

    ## [1] -215.5581 -173.4219
    ## attr(,"conf.level")
    ## [1] 0.95

## Plotting the results

The results can be plotted as boxplots, or using special libraries for
plotting pair t-Test results.

### Boxplots

The two groups can be plotted as boxplots.

![](Paired-Samples-t-Test-Github_files/figure-gfm/plotbox-1.png)<!-- -->

### Library for Paired data (showing the relationships)

You first need to install the library PairedData.

![](Paired-Samples-t-Test-Github_files/figure-gfm/pairedplot-1.png)<!-- -->

### Normality

The normality can be visualized by a histogram and a normal distribution
fitted to the data.

``` r
hist(d)
```

![](Paired-Samples-t-Test-Github_files/figure-gfm/normality-1.png)<!-- -->

``` r
m<-mean(d)
std<-sqrt(var(d))
hist(d, density=20, breaks=20, prob=TRUE, 
     xlab="differences", 
     main="normal curve over histogram")
curve(dnorm(x, mean=m, sd=std), 
      col="darkblue", lwd=2, add=TRUE, yaxt="n")
```

![](Paired-Samples-t-Test-Github_files/figure-gfm/normality-2.png)<!-- -->

### QQ-PLot

A quantile plot shows whether the quantiles of the observations
correspond to the quantiles of a normal distribution that should follow
a straight line.

``` r
qqnorm(d, pch = 1, frame = FALSE)
qqline(d, col = "steelblue", lwd = 2)
```

![](Paired-Samples-t-Test-Github_files/figure-gfm/qqplot-1.png)<!-- -->

### QQ-Plot with car

A QQ-Plot with the library(car) also shows the confidence limits.

``` r
library("car")
```

    ## Lade nötiges Paket: carData

``` r
qqPlot(d)
```

![](Paired-Samples-t-Test-Github_files/figure-gfm/qqplotcar-1.png)<!-- -->

    ## [1] 7 3
