Mini Data Analysis Deliverable 2 - Ishana Lodhia
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.2.3

    ## Warning: package 'ggplot2' was built under R version 4.2.3

    ## Warning: package 'tibble' was built under R version 4.2.3

    ## Warning: package 'readr' was built under R version 4.2.3

    ## Warning: package 'purrr' was built under R version 4.2.3

    ## Warning: package 'dplyr' was built under R version 4.2.3

    ## Warning: package 'lubridate' was built under R version 4.2.3

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

**Slightly modified**  
1. Was there a specific period of time where more trees were planted
around VGH? On which street(s)?  
\* Upon first glance there are quite a few NA observations for date
planted - first check out how much data there is to work with.  
2. What is the distribution of tree height around VGH?  
3. What is the distribution of tree diameter around VGH?  
4. Which genus of tree has the largest diameter and greatest height?  
<!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

## Filter the data

**Copied step from Milestone 1**  
Specify the streets enclosing the Vancouver Coastal Health medical
campus which includes the hospital as well as nearby medical/research
buildings (i.e., Vancouver Prostate Centre - my lab!).

``` r
# Here I am specifying the neighbourhood VGH is located in, as well as the streets surrounding the medical campus (and making sure the trees are actually planted on those streets according to the 'on_street' variable)

FiltVanTrees <- vancouver_trees %>%
  filter(neighbourhood_name == "FAIRVIEW") %>%
  filter(std_street %in% c("OAK ST", "W 10TH AV", "W 12TH AV", "ASH ST")) %>%
  filter(on_street %in% c("ASH ST", "W 12TH AV", "W 10TH AV", "HEATHER ST", "LAUREL ST", "OAK ST"))
```

## Research question 1

1.  Was there a specific period of time where more trees were planted
    around VGH? On which street(s)?

**Option 2 (summarizing)** Here I am computing the number of NA
observations to understand how much data is missing for the categorical
variable *date_planted* to address my first research question. This
informs whether the answers to research question 1 are going to be
representative of the trees in the VGH area or only represent a subset
that had this data recorded.

``` r
FiltVanTrees %>% 
  count(NA_status = is.na(date_planted))
```

    ## # A tibble: 2 × 2
    ##   NA_status     n
    ##   <lgl>     <int>
    ## 1 FALSE       145
    ## 2 TRUE        304

The output shows that 304 out of 449 trees do not have a recorded plant
date, but 145 do. This sample size is still large enough to carry out an
analysis, but it should be noted that only a subset of trees around VGH
could be further analyzed for this research question. In the code chunk
below, we can filter out the trees without a plant date to obtain this
subset.

``` r
# Filter out missing date values
NArm_VanTrees <- FiltVanTrees %>% filter(!is.na(date_planted))
```

**Option 7 (graphing)** To visualize when trees were planted around VGH
and on which streets, a density plot describing tree planting over time
based based on the dates recorded is utilized. Customizing alpha
transparency allows for the key streets to be differentiated in areas
where their density curves overlap.

``` r
# First need to extract the year from the dates provided for ease of graphing time variable
NArm_VanTrees$date_planted <- as.numeric(format(NArm_VanTrees$date_planted, "%Y"))

# Count the number of unique year observations for each of the major streets
DateCounts <- NArm_VanTrees %>%
  group_by(std_street) %>%
  count(date_planted)

# Density plot
DateCounts %>%
  ggplot(aes(date_planted)) +
  geom_density(aes(fill = std_street), alpha = 0.3) +
  scale_x_continuous(breaks = seq(1980,2020,5),
                     limits = c(1980,2020)) +
  labs(y = "Density",
       x = "Time (yrs)",
       title = "Density of trees planted around VGH over time") +
  scale_fill_discrete(name = "street") +
  theme(plot.title = element_text(hjust = 0.5))
```

![](mini_data_analysis_m2_files/figure-gfm/density%20plot%20for%20trees%20planted%20overtime,%20street-stratified-1.png)<!-- -->

The above data shows that most of the trees surrounding VGH were planted
within a 10-year period between 2000 and 2010 with some trees planted as
early as 1990 on Ash St. In this cohort, the greatest density of trees
were planted on W 12th and Oak St and the time span was relatively
narrow compared to W 10th, where planting was consistent between
1990s-2015 based on the broad curve, and Ash St, which had two spikes of
planting (1990s and early-2000s).

## Research question 2

2.  What is the distribution of tree height around VGH?

**Option 3 (summarizing)** According to the City of Vancouver Open Data
Portal, the height range ID is coded as such:  
0-10 for every 10 feet (e.g., 0 = 0-10 ft, 1 = 10-20 ft, 2 = 20-30 ft,
and 10 = 100+ ft)  
Therefore, we can create a new categorical variable (called
‘tree_height’) defining in feet what each height range ID number
corresponds to.

``` r
# The cut() function allows us to divide and categorize a numerical variable into bins with custom categorical labels (in this case, the height range in feet)
FiltVanTrees <- FiltVanTrees %>%
  mutate(tree_height = cut(height_range_id,
                           breaks = c(0,1,2,3,4,5,6,7,8),
                           labels = c("10-20", "20-30", "30-40", "40-50", "50-60", "60-70", "70-80", "80-90")))
```

**Option 8 (graphing)** To determine what the distribution of tree
height is, we can use a barplot with an overlaid dotplot to source the
heights to the street. The two geom layers used to visualize this are
geom_bar and geom_dotplot.

``` r
FiltVanTrees %>%
  ggplot(aes(tree_height)) +
  geom_bar(fill = "skyblue", alpha = 0.8) +
  geom_dotplot(aes(fill = std_street), dotsize = 0.35, position = "dodge") +
  scale_fill_discrete(name = "street") +
  labs(x = "Tree height (ft)",
       y = "Count",
       title = "Tree height range") +
  theme(plot.title = element_text(hjust = 0.5))
```

    ## Bin width defaults to 1/30 of the range of the data. Pick better value with
    ## `binwidth`.

![](mini_data_analysis_m2_files/figure-gfm/graph%20height%20range%20distribution-1.png)<!-- -->

The above data shows that most trees are between 40-50 feet (and on W
12th Ave), with 20-30-foot trees (especially on W 10th Ave) as another
frequently recorded height. The tallest trees recorded are 80-90 feet
and found on W 10th Ave.

## Research question 3

3.  What is the distribution of tree diameter around VGH?

**Option 1 (summarizing)** Before graphing the distribution, it is
helpful to see a snapshot of tree diameter by calculating the range
(minimum and maximum diameter), mean (average diameter), median
(mid-point in list of all diameters recorded), and standard deviation
(dispersion of diameter measurements with respect to the mean). We can
then see where these exact statistical values fall on the distribution
plot below, without needing to extrapolate an estimate - the statistics
and visual complement each other.

``` r
# Range
range(FiltVanTrees$diameter, na.rm = TRUE)
```

    ## [1]  3 72

``` r
# Mean
mean(FiltVanTrees$diameter, na.rm = TRUE)
```

    ## [1] 11.68797

``` r
# Median
median(FiltVanTrees$diameter, na.rm = TRUE)
```

    ## [1] 10

``` r
# Standard deviation
sd(FiltVanTrees$diameter, na.rm = TRUE)
```

    ## [1] 8.406393

For the VGH cohort, the diameter ranges from 3 to 72 inches, the mean
and median diameters are 11.7 inches and 10 inches, respectively, with a
standard deviation of 8.4 inches.

**Option 8 (graphing)** To determine what the distribution of tree
diameter is, we can use a histogram with an overlaid density curve (to
illustrate a more continuous distribution). The two geom layers used to
visualize this are geom_histogram and geom_density.

``` r
FiltVanTrees %>%
  ggplot(aes(diameter, after_stat(density))) +
  geom_histogram(fill = "skyblue", alpha = 0.7) +
  scale_x_continuous(breaks = seq(0,75,5),
                     limits = c(0,75)) +
  geom_density(col = "darkblue", linewidth = 0.8) +
  labs(x = "Diameter of tree at breast height (in)",
       y = "Density",
       title = "Tree diameter range") +
  theme(plot.title = element_text(hjust = 0.5))
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 2 rows containing missing values (`geom_bar()`).

![](mini_data_analysis_m2_files/figure-gfm/tree%20diameter%20range%20plot-1.png)<!-- -->

The above data shows that overall, tree diameter is around 3-15 inches,
with a few trees recorded to have as large as 45-73-foot diameters!

## Research question 4

4.  Which genus of tree has the largest diameter and greatest height?

Based on the above analyses, we know what the distribution of tree
height and diameter are. To further extract information from these
variables, we can determine what kind of tree has the tallest height and
largest diameter.

**Option 2 (summarizing)** Here I am computing the number of different
kinds of trees that are present within the categorical variable
*genus_name* of my VGH-specific *vancouver_trees* dataset to understand
how many records of height and diameter per tree type I have to work
with when determining which is tallest and has the greatest diameter.

``` r
FiltVanTrees %>% 
  count(genus_name)
```

    ## # A tibble: 25 × 2
    ##    genus_name         n
    ##    <chr>          <int>
    ##  1 ACER             222
    ##  2 AMELANCHIER        1
    ##  3 BETULA             2
    ##  4 CARPINUS          18
    ##  5 CATALPA            5
    ##  6 CERCIDIPHYLLUM     2
    ##  7 CHAMAECYPARIS      3
    ##  8 FAGUS             45
    ##  9 FRAXINUS          22
    ## 10 GLEDITSIA          1
    ## # ℹ 15 more rows

The above tibble shows us that this cohort contains 25 different genera
of trees and counts per genus recorded ranges from 1 entry (e.g.,
Amelanchier, Gleditsia) to upwards of 222 entries (Acer).

**Option 8 (graphing)** We can use a boxplot to show the distribution of
diameter and height for the different trees, since multiple entries of
varying height and diameter were recorded per genus according to the
above count summaries. An overlaid jitter plot is useful to see where
each individual record falls around the overall distribution. The two
geom layers used are geom_boxplot and geom_jitter.

``` r
# Tree diameter
FiltVanTrees %>%
  ggplot(aes(genus_name, diameter)) +
  geom_boxplot(fill = "skyblue", alpha = 0.7) +
  geom_jitter(width = 0.3, alpha = 0.5, size = 1) +
  labs(x = "Genus",
       y = "Diameter of tree at breast height (in)",
       title = "Tree diameter by genus") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5)) +
  theme(axis.text.x = element_text(angle = 70, vjust = 1, hjust=1))
```

![](mini_data_analysis_m2_files/figure-gfm/tree%20diameter%20by%20genus-1.png)<!-- -->

``` r
# Tree height
FiltVanTrees %>%
  ggplot(aes(genus_name, height_range_id)) +
  geom_boxplot(fill = "skyblue", alpha = 0.7) +
  geom_jitter(width = 0.3, alpha = 0.5, size = 1) +
  labs(x = "Genus",
       y = "Height range ID",
       title = "Tree height by genus") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5)) +
  theme(axis.text.x = element_text(angle = 70, vjust = 1, hjust=1))
```

![](mini_data_analysis_m2_files/figure-gfm/tree%20height%20by%20genus-1.png)<!-- -->

``` r
# Here using height range ID over the categorical bins created to define height in feet is favourable to generate a boxplot which requires a continuous y-axis (numbers 1-10 over discrete bins).
```

Based on the above data, we can get a better understanding of the height
and diameter range for each genera and compare their ranges to each
other. In the diameter plot, Robinia by far has the largest diameter
(\>70 in) and the second tallest height (70-80 ft). However,
technically, there is an Ulmus tree recorded with a height of \>80 ft!

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

As briefly summarized in the findings below each of the above graphs,
the operations I performed to address my research questions were
successful in answering them and can be fed into follow-up analyses. In
my first question, I asked if there was a specific period of time (with
corresponding links to the major streets) in which more trees were
planted around the VGH campus. While I kept in mind that I was only
working with a subset of the VGH trees, since 68% of the cohort did not
have a plant date recorded, I found that most were planted within a
10-year period between 2000 and 2010, with the greatest density being
planted on W 12th Ave and Oak St. Next, I was interested in the
distribution of height and diameter in the larger VGH tree cohort.
Regarding height, the data revealed that most trees are between 40-50 ft
and found on W 12th Ave, with 20-30-foot trees (predominantly on W 10th
Ave) following in second. The shortest trees fell in the 10-20 ft bin,
while the largest can be seen at 80-90 feet. In terms of diameter, 3-15
inches is frequently observed, with a few trees as large as 45-73 ft in
diameter. Lastly, I asked which genus has the largest diameter and
greatest height which I determined was Robinia (Black Locust) at \>70
inches in diameter and second tallest in height (70-80 ft) to the Ulmus
(American Elm) trees (\>80 ft).

It is worth nothing that diameter and height may have a predictive
relationship, in that taller heights in Robinia and Ulmus also revealed
larger diameters; therefore, I can refine my analysis by investigating
whether these variables may have a positive relationship. In my density
plot of trees planted around VGH over time, it would also be interesting
to see whether a specific type of tree was more popular during the
planting spike between 2000-2010 on W 12th Ave and Oak St, as well as
the two spikes seen on Ash St in the 1990s and early-2000s.

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

My data is tidy! Each of the first 8 **columns** specify a distinct
**variable** (i.e., genus name, height range identifier, longitude),
each **row** represents an **observation** for each tree recorded around
VGH (marked by their tree ID), and each **cell** contains a single
**value** based on the variable it falls under and the tree it is about.

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

``` r
# BEFORE (vancouver_trees is a tidy dataset)

TidyVanTrees <- FiltVanTrees %>%
  select(tree_id, genus_name, species_name, height_range_id, diameter, curb, longitude, latitude)

print(TidyVanTrees, n = 10)
```

    ## # A tibble: 449 × 8
    ##    tree_id genus_name     species_name height_range_id diameter curb  longitude
    ##      <dbl> <chr>          <chr>                  <dbl>    <dbl> <chr>     <dbl>
    ##  1  162057 CHAMAECYPARIS  SPECIES                    5     12   Y           NA 
    ##  2  163167 FRAXINUS       EXCELSIOR                  4     21   Y         -123.
    ##  3  223127 MAGNOLIA       KOBUS                      1      3   Y         -123.
    ##  4  223129 CERCIDIPHYLLUM JAPONICUM                  2      3   Y         -123.
    ##  5   11690 ACER           RUBRUM                     3     20.4 Y         -123.
    ##  6   11696 ACER           RUBRUM                     3     16.6 Y         -123.
    ##  7   11717 ACER           PLATANOIDES                2     13   Y         -123.
    ##  8   11718 ACER           PLATANOIDES                2     14   Y         -123.
    ##  9   11728 ACER           RUBRUM                     3     17   Y         -123.
    ## 10   11739 ACER           RUBRUM                     4     18   Y         -123.
    ## # ℹ 439 more rows
    ## # ℹ 1 more variable: latitude <dbl>

``` r
# AFTER (making vancouver_trees untidy)

UntidyVanTrees <- TidyVanTrees %>%
  pivot_wider(names_from = "genus_name",
              values_from = "diameter")

print(UntidyVanTrees, n = 10)
```

    ## # A tibble: 449 × 31
    ##    tree_id species_name height_range_id curb  longitude latitude CHAMAECYPARIS
    ##      <dbl> <chr>                  <dbl> <chr>     <dbl>    <dbl>         <dbl>
    ##  1  162057 SPECIES                    5 Y           NA      NA              12
    ##  2  163167 EXCELSIOR                  4 Y         -123.     49.3            NA
    ##  3  223127 KOBUS                      1 Y         -123.     49.3            NA
    ##  4  223129 JAPONICUM                  2 Y         -123.     49.3            NA
    ##  5   11690 RUBRUM                     3 Y         -123.     49.3            NA
    ##  6   11696 RUBRUM                     3 Y         -123.     49.3            NA
    ##  7   11717 PLATANOIDES                2 Y         -123.     49.3            NA
    ##  8   11718 PLATANOIDES                2 Y         -123.     49.3            NA
    ##  9   11728 RUBRUM                     3 Y         -123.     49.3            NA
    ## 10   11739 RUBRUM                     4 Y         -123.     49.3            NA
    ## # ℹ 439 more rows
    ## # ℹ 24 more variables: FRAXINUS <dbl>, MAGNOLIA <dbl>, CERCIDIPHYLLUM <dbl>,
    ## #   ACER <dbl>, ULMUS <dbl>, CARPINUS <dbl>, PRUNUS <dbl>, QUERCUS <dbl>,
    ## #   FAGUS <dbl>, NYSSA <dbl>, PICEA <dbl>, SYRINGA <dbl>, PYRUS <dbl>,
    ## #   MALUS <dbl>, CATALPA <dbl>, TSUGA <dbl>, BETULA <dbl>, GLEDITSIA <dbl>,
    ## #   THUJA <dbl>, AMELANCHIER <dbl>, SORBUS <dbl>, ROBINIA <dbl>, RHUS <dbl>,
    ## #   PSEUDOTSUGA <dbl>

By turning genus names, which were originally listed as individual rows
into individual columns, and assigning values from diameter, the tibble
gains 23 columns. This matrix-style format is very difficult to read and
compare diameters for each genus as you need to scroll across the table
and see past all the NA entries.

``` r
# Tidying data back to original state

RETidyVanTrees <- UntidyVanTrees %>%
  pivot_longer(cols = c(-"tree_id", -"species_name", -"height_range_id", -"curb", -"longitude", -"latitude"),
               names_to = "genus_name",
               values_to = "diameter", values_drop_na = TRUE) %>%
  select(tree_id, genus_name, species_name, height_range_id, diameter, everything()) # Reordering columns to original arrangement

print(RETidyVanTrees, n = 10)  
```

    ## # A tibble: 449 × 8
    ##    tree_id genus_name     species_name height_range_id diameter curb  longitude
    ##      <dbl> <chr>          <chr>                  <dbl>    <dbl> <chr>     <dbl>
    ##  1  162057 CHAMAECYPARIS  SPECIES                    5     12   Y           NA 
    ##  2  163167 FRAXINUS       EXCELSIOR                  4     21   Y         -123.
    ##  3  223127 MAGNOLIA       KOBUS                      1      3   Y         -123.
    ##  4  223129 CERCIDIPHYLLUM JAPONICUM                  2      3   Y         -123.
    ##  5   11690 ACER           RUBRUM                     3     20.4 Y         -123.
    ##  6   11696 ACER           RUBRUM                     3     16.6 Y         -123.
    ##  7   11717 ACER           PLATANOIDES                2     13   Y         -123.
    ##  8   11718 ACER           PLATANOIDES                2     14   Y         -123.
    ##  9   11728 ACER           RUBRUM                     3     17   Y         -123.
    ## 10   11739 ACER           RUBRUM                     4     18   Y         -123.
    ## # ℹ 439 more rows
    ## # ℹ 1 more variable: latitude <dbl>

Now genus names are listed as rows under one column and all diameter
values are in another column - much easier to compare two columns
against each other rather than 23 columns.  
<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  Was there a specific period of time where more trees were planted
    around VGH? On which street(s)?
2.  Which genus of tree has the largest diameter and greatest height?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

Paraphrased from above: Since W 12th Ave and Oak St saw a planting spike
between 2000 and 2010, I can further ascertain whether a specific type
of tree was more popular during this period. Also, I could determine
which trees were planted during the two spikes for Ash St in the 1990s
and early-2000s - perhaps different genera were preferred in landscaping
at the end of the 20th Century.  
Diameter and height may have a predictive relationship, in that taller
heights in Robinia and Ulmus correspond with larger diameters. Thus, I
can generate a model to determine whether these variables have a
positive relationship.

<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

``` r
# Filter rows for trees found in the Fairview neighbourhood and located on the streets directly surrounding VGH

VanCohort <- vancouver_trees %>%
  filter(neighbourhood_name == "FAIRVIEW") %>%
  filter(std_street %in% c("OAK ST", "W 10TH AV", "W 12TH AV", "ASH ST")) %>%
  filter(on_street %in% c("ASH ST", "W 12TH AV", "W 10TH AV", "HEATHER ST", "LAUREL ST", "OAK ST"))

# For question 1
# The tsibble package allows for ease of extracting year and month from the date entries. I used a more complicated method in my earlier analysis to extract only year (before knowing about tsibble), but this is an opportunity to apply tsibble here and include months (even though the previous analysis only accounted for year).

Q1VanCohort <- VanCohort %>%
  filter(!is.na(date_planted)) %>%
  mutate(time_planted = tsibble::yearmonth(date_planted)) %>%
  select(std_street, genus_name, common_name, on_street, time_planted) # Only interested in these few columns to address my question.

# For question 2
Q2VanCohort <- VanCohort %>%
  select(std_street, genus_name, common_name, on_street, height_range_id, diameter) %>%
  select(height_range_id, diameter, everything()) # Technically in the code line just above I could get the same result by listing height_range_id and diameter before the other variables, but this line is just to feature how columns can be reordered by listing specific variables followed by the everything() function instead of typing out the other variables.
```

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: Which genus of tree has the largest diameter and
greatest height?  
As I mentioned in section 2.3, although my previous analysis answered
this question, it sparked a follow-up question regarding whether there
is a predictive positive relationship between diameter and tree height
which I will address in the below analysis.

**Variable of interest**: diameter

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

Here I am fitting a linear model to make predictions on tree diameter
based on tree height to determine whether these two variables have a
predictive positive relationship. In this situation I am using the
height range identifiers since the categorical bins are not continuous
(hard to get linear model line). **Reminder**: 0-10 for every 10 feet
(e.g., 0 = 0-10 ft, 1 = 10-20 ft, 2 = 20-30 ft, and 10 = 100+ ft).

``` r
(lm_VanTrees <- lm(diameter ~ height_range_id, data = Q2VanCohort))
```

    ## 
    ## Call:
    ## lm(formula = diameter ~ height_range_id, data = Q2VanCohort)
    ## 
    ## Coefficients:
    ##     (Intercept)  height_range_id  
    ##          -1.633            4.168

The model prints to screen two coefficients - the intercept, which is
-1.63, and the slope, which is 4.17.

I also graphed my linear model to visualize how tree height influences
diameter and vice versa.

``` r
ggplot(Q2VanCohort, aes(x = height_range_id, y = diameter)) +
  geom_point(alpha = 0.5) +
  geom_smooth(method = "lm") +
  labs(x = "Height range ID",
       y = "Diameter") +
  theme_classic()
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](mini_data_analysis_m2_files/figure-gfm/linear%20model%20plotted-1.png)<!-- -->

There appears to be some positive relationship between greater height
and larger diameter in the VGH tree cohort. However, there is still a
broad range of tree diameters within each height bin - for example,
trees 40-50 ft tall were anywhere from \<10 inches in diameter to around
30 inches.

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

Using the augment() function in the *broom* package, I wanted to see how
well the linear model could predict the diameter of trees within the VGH
cohort based on height range.

``` r
# The predicted diameter based on the model is referred to as '.fitted' in the below tibble.

library(broom)
```

    ## Warning: package 'broom' was built under R version 4.2.3

``` r
augment(lm_VanTrees) %>%
  print(n = 10)
```

    ## # A tibble: 449 × 8
    ##    diameter height_range_id .fitted .resid    .hat .sigma   .cooksd .std.resid
    ##       <dbl>           <dbl>   <dbl>  <dbl>   <dbl>  <dbl>     <dbl>      <dbl>
    ##  1     12                 5   19.2  -7.21  0.00511   5.18 0.00498      -1.39  
    ##  2     21                 4   15.0   5.96  0.00280   5.18 0.00186       1.15  
    ##  3      3                 1    2.54  0.465 0.00649   5.19 0.0000264     0.0900
    ##  4      3                 2    6.70 -3.70  0.00349   5.19 0.000897     -0.715 
    ##  5     20.4               3   10.9   9.53  0.00226   5.17 0.00384       1.84  
    ##  6     16.6               3   10.9   5.73  0.00226   5.18 0.00139       1.11  
    ##  7     13                 2    6.70  6.30  0.00349   5.18 0.00259       1.22  
    ##  8     14                 2    6.70  7.30  0.00349   5.18 0.00348       1.41  
    ##  9     17                 3   10.9   6.13  0.00226   5.18 0.00159       1.18  
    ## 10     18                 4   15.0   2.96  0.00280   5.19 0.000459      0.572 
    ## # ℹ 439 more rows

Overall, the prediction varied in accuracy between fitted diameter and
actual diameter. Perhaps this is in part due to the reporting of tree
height in 10-foot bins which may be too broad a range for predicting
another measure - some of the smallest trees recorded were around 10
feet!

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
# Load 'here' package 
library(here)
```

    ## Warning: package 'here' was built under R version 4.2.3

    ## here() starts at C:/Users/ishan/OneDrive/EXMED MSC/STAT 545A/mda-ishanainformatics

``` r
# Write FiltVanTrees table into a csv file
write_csv(FiltVanTrees, here("output/FiltVanTrees.csv"))
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
# Save model object as RDS
saveRDS(lm_VanTrees, "output/lm_VanTrees.rds")

# Load rds file into object
lm_VanTrees <- readRDS("output/lm_VanTrees.rds")
```

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
