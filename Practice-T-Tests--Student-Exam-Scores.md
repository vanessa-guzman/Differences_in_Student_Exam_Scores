Practice T-Tests–Student Exam Scores
================
2023-02-9

# Practice T-Tests–Student Exam Scores

An independent samples t-test was conducted in order to determine
whether there were statistically significant differences in test scores
between students who completed the exam preparation course and students
who did not

## Loading Packages

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.4.0     ✔ purrr   1.0.1
    ## ✔ tibble  3.1.8     ✔ dplyr   1.1.0
    ## ✔ tidyr   1.3.0     ✔ stringr 1.5.0
    ## ✔ readr   2.1.3     ✔ forcats 1.0.0
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
# used to import the data

library(stats)
# used for t.test()

library(car)
```

    ## Loading required package: carData
    ## 
    ## Attaching package: 'car'
    ## 
    ## The following object is masked from 'package:dplyr':
    ## 
    ##     recode
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     some

``` r
# used for leveneTest()
```

## Importing Data

``` r
student_exams <- read_csv('exams.csv')
```

    ## Rows: 1000 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (5): gender, race/ethnicity, parental level of education, lunch, test pr...
    ## dbl (3): math score, reading score, writing score
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## Checking Variance

Variance will be checked using **boxplot()** and **leveneTest()**.

### Math

``` r
boxplot(student_exams$`math score` ~ student_exams$`test preparation course`)
```

![](Practice-T-Tests--Student-Exam-Scores_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
leveneTest(student_exams$`math score` ~ student_exams$`test preparation course`)
```

    ## Warning in leveneTest.default(y = y, group = group, ...): group coerced to
    ## factor.

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##        Df F value Pr(>F)
    ## group   1  0.1482 0.7003
    ##       998

``` r
# p-value = 0.7003, this is greater than .05, so assume homogeneity of variance
```

### Reading

``` r
boxplot(student_exams$`reading score` ~ student_exams$`test preparation course`)
```

![](Practice-T-Tests--Student-Exam-Scores_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
leveneTest(student_exams$`reading score` ~ student_exams$`test preparation course`)
```

    ## Warning in leveneTest.default(y = y, group = group, ...): group coerced to
    ## factor.

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##        Df F value Pr(>F)
    ## group   1  2.4079  0.121
    ##       998

``` r
# p-value = 0.121, this is greater than .05, so assume homogeneity of variance
```

### Writing

``` r
boxplot(student_exams$`writing score` ~ student_exams$`test preparation course`)
```

![](Practice-T-Tests--Student-Exam-Scores_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
leveneTest(student_exams$`writing score` ~ student_exams$`test preparation course`) 
```

    ## Warning in leveneTest.default(y = y, group = group, ...): group coerced to
    ## factor.

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##        Df F value Pr(>F)
    ## group   1  1.8103 0.1788
    ##       998

``` r
# p-value = 0.1788, this is greater than .05, so assume homogeneity of variance
```

## Independent Samples T-Test

### Math

``` r
t.test(student_exams$`math score` ~ student_exams$`test preparation course`, mu=0, alt= "two.sided", conf=0.95, var.eq=TRUE, paired=F)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  student_exams$`math score` by student_exams$`test preparation course`
    ## t = 4.8486, df = 998, p-value = 1.441e-06
    ## alternative hypothesis: true difference in means between group completed and group none is not equal to 0
    ## 95 percent confidence interval:
    ##  2.945571 6.950872
    ## sample estimates:
    ## mean in group completed      mean in group none 
    ##                69.68657                64.73835

``` r
# P-value = 1.441e-06, this is less than 0.05, so the difference between the two groups is significant
```

### Reading

``` r
t.test(student_exams$`reading score` ~ student_exams$`test preparation course`, mu=0, alt= "two.sided", conf=0.95, var.eq=TRUE, paired=F)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  student_exams$`reading score` by student_exams$`test preparation course`
    ## t = 7.9881, df = 998, p-value = 3.759e-15
    ## alternative hypothesis: true difference in means between group completed and group none is not equal to 0
    ## 95 percent confidence interval:
    ##  5.771058 9.529851
    ## sample estimates:
    ## mean in group completed      mean in group none 
    ##                74.08955                66.43910

``` r
# P-value = 3.759e-15, this is less than 0.05, so the difference between the two groups is significant
```

### Writing

``` r
t.test(student_exams$`writing score` ~ student_exams$`test preparation course`, mu=0, alt= "two.sided", conf=0.95, var.eq=TRUE, paired=F)
```

    ## 
    ##  Two Sample t-test
    ## 
    ## data:  student_exams$`writing score` by student_exams$`test preparation course`
    ## t = 10.507, df = 998, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means between group completed and group none is not equal to 0
    ## 95 percent confidence interval:
    ##   8.47925 12.37381
    ## sample estimates:
    ## mean in group completed      mean in group none 
    ##                74.67164                64.24511

``` r
# P-value < 2.2e-16, this is less than 0.05, so the difference between the two groups is significant
```
