---
title: "My data exploration workflow"
subtitle: ""
excerpt: "In this post, I share a basic workflow I tend to use when I start to explore a new dataset."
date: 2023-11-10
author: "Henriette Arndt"
draft: false
images:
series:
tags:
categories:
layout: single
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />

A necessary first step of almost any data analysis is to get familiar with the data through basic descriptive statistics and visualizations. In today's post, I want to share a basic workflow that I tend to use when I start to explore a new set of data. 

We will use the packages janitor, psych, and kableExtra to create print-ready tables showing frequencies (for categorical factors) and descriptive statistics (for numerical variables). We will also use the `ggpairs()` function from theGGally package to produce plot matrices that are useful for exploring the distribution of, and basic relationships between different variables in a dataset. 

## Get to grips with the dataset

As an example we'll use the starwars data in the dplyr package, which contains information on the characters from all seven Star Wars movies. 


```r
# Load library and data
library(tidyverse)
data(starwars)

# View dataset contents
glimpse(starwars)
```

```
## Rows: 87
## Columns: 14
## $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Or…
## $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 2…
## $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.…
## $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", N…
## $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "…
## $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue",…
## $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, …
## $ sex        <chr> "male", "none", "none", "male", "female", "male", "female",…
## $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "femini…
## $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "T…
## $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Huma…
## $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return…
## $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp…
## $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",…
```
As we can see, this dataset contains information on 14 variables (Columns) about 87 Star Wars characters (Rows), including categorical factors such as gender and species, as well as numerical variables such as height, mass, and birth year.

## Summarize data
### Categorical variables

For category counts, I like to use [`tabyl( )`](https://cran.r-project.org/web/packages/janitor/vignettes/tabyls.html) from the janitor package, which provides frequency tables that are highly customizable and tie in well with other tidyverse functions. To look at just one variable, we can create a simple one-way frequency table: 


```r
library(janitor)

one_way <- starwars %>% 
  tabyl(species)
head(one_way, 15)
```

```
##    species  n    percent valid_percent
##     Aleena  1 0.01149425    0.01204819
##   Besalisk  1 0.01149425    0.01204819
##     Cerean  1 0.01149425    0.01204819
##   Chagrian  1 0.01149425    0.01204819
##   Clawdite  1 0.01149425    0.01204819
##      Droid  6 0.06896552    0.07228916
##        Dug  1 0.01149425    0.01204819
##       Ewok  1 0.01149425    0.01204819
##  Geonosian  1 0.01149425    0.01204819
##     Gungan  3 0.03448276    0.03614458
##      Human 35 0.40229885    0.42168675
##       Hutt  1 0.01149425    0.01204819
##   Iktotchi  1 0.01149425    0.01204819
##    Kaleesh  1 0.01149425    0.01204819
##   Kaminoan  2 0.02298851    0.02409639
```
Above, I printed just the first 15 rows, but the full table lists all 37 different species in the dataset, how often each of them occur, and their frequency as part of the total count (including NAs) and valid count (excluding NAs). As you can see, most species only include one character. With 35 and 6 characters each, Humans and Droids are the most frequently appearing species — so I'll focus on those in the following. 

You may also wonder about the gender of the characters in this dataset, and whether the gender distribution is similar across Humans and Droids. We can look into it using a simple two-way frequency table produced by `tabyl()`. In the example below, I also use a series of `adorn_()` functions to add more useful features to the two-way table, which ends up looking like this: 


```r
two_way <- starwars %>%
   # include only Humans and Droids
   filter(species %in% c("Human", "Droid")) %>%
   tabyl(species, gender) %>%
   adorn_totals("row") %>% # bottom row with total counts
   adorn_percentages("row") %>% # proportions by row
   adorn_pct_formatting(digits = 0) %>% # round to whole number
   adorn_ns() %>% # include raw counts
   adorn_title("combined") # include both var names in first cell
 
 two_way
```

```
##  species/gender feminine masculine
##           Droid 17%  (1)  83%  (5)
##           Human 26%  (9)  74% (26)
##           Total 24% (10)  76% (31)
```
Now, since `tabyl()` is based on tidyverse functions, we can pass on the output to the `kbl()` function from the [kableExtra](https://cran.r-project.org/web/packages/kableExtra/vignettes/awesome_table_in_html.html) package to produce an HTML table formatted for print, that we can copy from the R-Study Viewer directly into a Word document. 


```r
library(kableExtra)

two_way %>%
   kbl() %>%
   # apply print theme and font formatting
   kable_classic(html_font = "Times New Roman",
                 font_size = 12,
                 full_width = F)
```

<table class=" lightable-classic" style="font-size: 12px; font-family: Times New Roman; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> species/gender </th>
   <th style="text-align:left;"> feminine </th>
   <th style="text-align:left;"> masculine </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Droid </td>
   <td style="text-align:left;"> 17%  (1) </td>
   <td style="text-align:left;"> 83%  (5) </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Human </td>
   <td style="text-align:left;"> 26%  (9) </td>
   <td style="text-align:left;"> 74% (26) </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 24% (10) </td>
   <td style="text-align:left;"> 76% (31) </td>
  </tr>
</tbody>
</table>

Perhaps unsurprisingly, the vast majority of  Human and Droid Star Wars characters are male (76%), although the distribution is even more skewed among Droids than Humans (83% vs. 74%). I'll leave it up to you how you want to interpret that information 😉 

### Numerical variables
For summarizing numerical variables, I tend to use the `describe()` function from the [psych](https://www.rdocumentation.org/packages/psych/versions/1.0-17/topics/00psych-package) package. This gives a good range of descriptive statistics, but I manually add columns for the lower and upper bounds of the 95% confidence interval around the mean and the number of missing values (NAs), since those are usually required in publications in my field. 


```r
library(psych)

descr <- starwars %>%
  select(where(is.numeric)) %>% 
  describe() %>% 
  # turn into tibble for further formatting
  as_tibble(
    # add column with rownames (= variable names)
    rownames = "varname") %>%
  select(-vars) %>%
  # count NAs 
  mutate(missing = nrow(starwars) - n,
         .after = n) %>%
  # calculate 95% CI around mean
  mutate(CImin = mean - (1.96 * se),
         CImax = mean + (1.96 * se),
         .after = mean)
descr
```

```
## # A tibble: 3 × 16
##   varname     n missing  mean CImin CImax    sd median trimmed   mad   min   max
##   <chr>   <dbl>   <dbl> <dbl> <dbl> <dbl> <dbl>  <dbl>   <dbl> <dbl> <dbl> <dbl>
## 1 height     81       6 174.  167.   182.  34.8    180   178.   19.3    66   264
## 2 mass       59      28  97.3  54.1  141. 169.      79    75.4  16.3    15  1358
## 3 birth_…    43      44  87.6  41.3  134. 155.      52    54.2  29.7     8   896
## # … with 4 more variables: range <dbl>, skew <dbl>, kurtosis <dbl>, se <dbl>
```

Now we have a nice data frame with our descriptive statistics. As before, `kbl()` is a useful way to turn this data frame into a formatted table ready to be copy-pasted into your main document. These are the steps I normally go through to produce a table compliant with publication guidelines in my field:


```r
descr %>%
   # select and order summary stats
   select(varname, n, missing, min, max, mean, CImin, CImax, 
          sd, skew, kurtosis) %>%
   # change row names
   mutate(varname = factor(varname,
          # current names
          levels = c("height", "mass", "birth_year"),
          # names in print
          labels = c("Height", "Weight", "Birth Year"))) %>%
   kbl(digits = 2, # round to two decimals
       # change column names
       col.names = c("Variable", "n", "NA", "min", "max", "mean",
                   "CImin", "CImax", "sd", "skewn.", "kurt.")) %>%
   # apply print theme and font formatting
   kable_classic(html_font = "Times New Roman",
                 font_size = 12,
                 full_width = F)
```

<table class=" lightable-classic" style="font-size: 12px; font-family: Times New Roman; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:right;"> n </th>
   <th style="text-align:right;"> NA </th>
   <th style="text-align:right;"> min </th>
   <th style="text-align:right;"> max </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> CImin </th>
   <th style="text-align:right;"> CImax </th>
   <th style="text-align:right;"> sd </th>
   <th style="text-align:right;"> skewn. </th>
   <th style="text-align:right;"> kurt. </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Height </td>
   <td style="text-align:right;"> 81 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 66 </td>
   <td style="text-align:right;"> 264 </td>
   <td style="text-align:right;"> 174.36 </td>
   <td style="text-align:right;"> 166.79 </td>
   <td style="text-align:right;"> 181.93 </td>
   <td style="text-align:right;"> 34.77 </td>
   <td style="text-align:right;"> -1.03 </td>
   <td style="text-align:right;"> 1.78 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Weight </td>
   <td style="text-align:right;"> 59 </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 1358 </td>
   <td style="text-align:right;"> 97.31 </td>
   <td style="text-align:right;"> 54.07 </td>
   <td style="text-align:right;"> 140.55 </td>
   <td style="text-align:right;"> 169.46 </td>
   <td style="text-align:right;"> 6.97 </td>
   <td style="text-align:right;"> 48.93 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Birth Year </td>
   <td style="text-align:right;"> 43 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 896 </td>
   <td style="text-align:right;"> 87.57 </td>
   <td style="text-align:right;"> 41.33 </td>
   <td style="text-align:right;"> 133.80 </td>
   <td style="text-align:right;"> 154.69 </td>
   <td style="text-align:right;"> 4.15 </td>
   <td style="text-align:right;"> 17.17 </td>
  </tr>
</tbody>
</table>

As indicated by the skewness and kurtosis values, height seems to be roughly normally distributed, but something weird is going on with Weight and Birth Year. Visualizations are a good way to figure out what might be happening here. 

## Visualize data
Instead of plotting each variable separately, I like to use the `ggpairs()` function from the GGally package [equivalent to `pairs()` from baseR] to produce plot matrices that are useful for exploring the distribution of, and basic relationships between different variables in a dataset. 

Since the plot will become difficult to read the more variables we include, we'll just focus on some we've already looked at. I excluded the species variable here, because `ggpairs()` by default will not plot variables with more than 15 categories. 


```r
library(GGally)

starwars %>%
   select(height, mass, birth_year, gender) %>%
   ggpairs()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="1152" />

These visualizations make it very easy to understand the way in which the mass and birth_year data are non-normally distributed: Both variables contain outliers at the very upper end of the value range. If we remove these, we can see that mass and birth_year are now somewhat more normally distributed, and both show significant correlations with height (but not with each other).


```r
starwars %>%
   # de-select outliers
   filter(mass < 1000,
          birth_year < 250) %>%
   select(height, mass, birth_year, gender) %>%
   ggpairs()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="1152" />

Of course there are many more ways to get familiar with a new dataset, but I wanted to show you some of the most basic steps that I go through today. In my next post, I will show you my preferred way to visualize the distribution of numerical variables.

## Full code
As always, you can download an R script with the entire code for this post from [my GitHub page](https://github.com/arndthen/rblog).
