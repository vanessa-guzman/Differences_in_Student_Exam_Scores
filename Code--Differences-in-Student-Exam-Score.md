Code- Differences in Student Exam Scores
================
2023-02-08

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

## Importing the Data

The data contains information about student exam scores in 3 subjects
(math, reading, and writing). It also includes demographic information
about the students and whether or not students completed an exam
preparation course. The data can be found on
[kaggle](https://www.kaggle.com/datasets/whenamancodes/students-performance-in-exams).

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

## Inspecting the Data

``` r
head(student_exams)
```

    ## # A tibble: 6 × 8
    ##   gender `race/ethnicity` parental level…¹ lunch test …² math …³ readi…⁴ writi…⁵
    ##   <chr>  <chr>            <chr>            <chr> <chr>     <dbl>   <dbl>   <dbl>
    ## 1 male   group A          high school      stan… comple…      67      67      63
    ## 2 female group D          some high school free… none         40      59      55
    ## 3 male   group E          some college     free… none         59      60      50
    ## 4 male   group B          high school      stan… none         77      78      68
    ## 5 male   group E          associate's deg… stan… comple…      78      73      68
    ## 6 female group D          high school      stan… none         63      77      76
    ## # … with abbreviated variable names ¹​`parental level of education`,
    ## #   ²​`test preparation course`, ³​`math score`, ⁴​`reading score`,
    ## #   ⁵​`writing score`

``` r
dim(student_exams)
```

    ## [1] 1000    8

``` r
str(student_exams)
```

    ## spc_tbl_ [1,000 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ gender                     : chr [1:1000] "male" "female" "male" "male" ...
    ##  $ race/ethnicity             : chr [1:1000] "group A" "group D" "group E" "group B" ...
    ##  $ parental level of education: chr [1:1000] "high school" "some high school" "some college" "high school" ...
    ##  $ lunch                      : chr [1:1000] "standard" "free/reduced" "free/reduced" "standard" ...
    ##  $ test preparation course    : chr [1:1000] "completed" "none" "none" "none" ...
    ##  $ math score                 : num [1:1000] 67 40 59 77 78 63 62 93 63 47 ...
    ##  $ reading score              : num [1:1000] 67 59 60 78 73 77 59 88 56 42 ...
    ##  $ writing score              : num [1:1000] 63 55 50 68 68 76 63 84 65 45 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   gender = col_character(),
    ##   ..   `race/ethnicity` = col_character(),
    ##   ..   `parental level of education` = col_character(),
    ##   ..   lunch = col_character(),
    ##   ..   `test preparation course` = col_character(),
    ##   ..   `math score` = col_double(),
    ##   ..   `reading score` = col_double(),
    ##   ..   `writing score` = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

## Exploring and Analyzing the Data

### Descriptive Statistics of the Students

``` r
student_exams %>% group_by(gender) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))
```

    ## # A tibble: 2 × 2
    ##   gender Percentage
    ##   <chr>       <dbl>
    ## 1 female       48.3
    ## 2 male         51.7

``` r
# gender percentage

student_exams %>% group_by(`race/ethnicity`) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))
```

    ## # A tibble: 5 × 2
    ##   `race/ethnicity` Percentage
    ##   <chr>                 <dbl>
    ## 1 group A                 7.9
    ## 2 group B                20.5
    ## 3 group C                32.3
    ## 4 group D                26.2
    ## 5 group E                13.1

``` r
# race/ ethnicity percentage

student_exams$`parental level of education` <- ordered(student_exams$`parental level of education`, levels = c("some high school", "high school", "some college", "associate's degree", "bachelor's degree", "master's degree"))
#Reordering parents' levels of education

student_exams %>% group_by(`parental level of education`) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))
```

    ## # A tibble: 6 × 2
    ##   `parental level of education` Percentage
    ##   <ord>                              <dbl>
    ## 1 some high school                    19.1
    ## 2 high school                         20.2
    ## 3 some college                        22.2
    ## 4 associate's degree                  20.3
    ## 5 bachelor's degree                   11.2
    ## 6 master's degree                      7

``` r
# parent level of education percentage

student_exams %>% group_by(lunch) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))
```

    ## # A tibble: 2 × 2
    ##   lunch        Percentage
    ##   <chr>             <dbl>
    ## 1 free/reduced       34.8
    ## 2 standard           65.2

``` r
# lunch percentage

student_exams %>% group_by(`test preparation course`) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))
```

    ## # A tibble: 2 × 2
    ##   `test preparation course` Percentage
    ##   <chr>                          <dbl>
    ## 1 completed                       33.5
    ## 2 none                            66.5

``` r
# Test preparation course count
```

### Comparing Exam Scores

Comparing exams course based on whether the test preparation course was
completed.

``` r
student_exams %>% group_by(`test preparation course`) %>% summarize(avg_math = mean(`math score`), avg_reading = mean(`reading score`), avg_writing = mean(`writing score`))
```

    ## # A tibble: 2 × 4
    ##   `test preparation course` avg_math avg_reading avg_writing
    ##   <chr>                        <dbl>       <dbl>       <dbl>
    ## 1 completed                     69.7        74.1        74.7
    ## 2 none                          64.7        66.4        64.2

Comparing exams course based on gender, race/ethnicity, parental
education level, and lunch.

``` r
student_exams %>% group_by(gender) %>% summarize(avg_math = mean(`math score`), avg_reading = mean(`reading score`), avg_writing = mean(`writing score`))
```

    ## # A tibble: 2 × 4
    ##   gender avg_math avg_reading avg_writing
    ##   <chr>     <dbl>       <dbl>       <dbl>
    ## 1 female     63.2        71.9        71.7
    ## 2 male       69.4        66.3        64.0

``` r
student_exams %>% group_by(`parental level of education`) %>% summarize(avg_math = mean(`math score`), avg_reading = mean(`reading score`), avg_writing = mean(`writing score`))
```

    ## # A tibble: 6 × 4
    ##   `parental level of education` avg_math avg_reading avg_writing
    ##   <ord>                            <dbl>       <dbl>       <dbl>
    ## 1 some high school                  60.7        64.4        62.5
    ## 2 high school                       65.2        67.4        64.8
    ## 3 some college                      65.3        68.0        66.7
    ## 4 associate's degree                69.5        71.0        70.1
    ## 5 bachelor's degree                 71.5        74.0        74.4
    ## 6 master's degree                   71.6        75.4        75.9

## Exporting Data

Data will be used to create visualizations on Tableau.

### Assigning Variables

``` r
gender_percentage <- student_exams %>% group_by(gender) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))

race_eth_percentage <- student_exams %>% group_by(`race/ethnicity`) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))

parent_edu_percentage <- student_exams %>% group_by(`parental level of education`) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))

lunch_percentage <- student_exams %>% group_by(lunch) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))

test_prep_percentage <- student_exams %>% group_by(`test preparation course`) %>% summarize(Percentage = round(100 * n()/nrow(student_exams),2))

test_prep_averages <- student_exams %>% group_by(`test preparation course`) %>% summarize(avg_math = mean(`math score`), avg_reading = mean(`reading score`), avg_writing = mean(`writing score`))

gender_averages <- student_exams %>% group_by(gender) %>% summarize(avg_math = mean(`math score`), avg_reading = mean(`reading score`), avg_writing = mean(`writing score`))

parent_edu_averages <- student_exams %>% group_by(`parental level of education`) %>% summarize(avg_math = mean(`math score`), avg_reading = mean(`reading score`), avg_writing = mean(`writing score`))
```

### Exporting Data

``` r
write.csv(gender_percentage, "~/Desktop/Projects- PUBLIC/Student Test Scores/gender_percentage.csv")

write.csv(race_eth_percentage, "~/Desktop/Projects- PUBLIC/Student Test Scores/race_eth_percentage.csv")

write.csv(parent_edu_percentage, "~/Desktop/Projects- PUBLIC/Student Test Scores/parent_edu_percentage.csv")

write.csv(lunch_percentage, "~/Desktop/Projects- PUBLIC/Student Test Scores/lunch_percentage.csv")

write.csv(test_prep_percentage, "~/Desktop/Projects- PUBLIC/Student Test Scores/test_prep_percentage.csv")

write.csv(test_prep_averages, "~/Desktop/Projects- PUBLIC/Student Test Scores/test_prep_averages.csv")

write.csv(gender_averages, "~/Desktop/Projects- PUBLIC/Student Test Scores/gender_averages.csv")

write.csv(parent_edu_averages, "~/Desktop/Projects- PUBLIC/Student Test Scores/parent_edu_averages.csv")
```

## Links to Tableau and Presentation

Tableau link
[here](https://public.tableau.com/app/profile/vanessa4875/viz/StudentExamScores/StudentDemographics_1).

Presentation on [Google
Slides](https://docs.google.com/presentation/d/1Wb9RZexRPE4cv4MiQllDhPUfzxv-rtiEOLy2ymaiUKk/edit#slide=id.g2071186f1f7_0_85).
