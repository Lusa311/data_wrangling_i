Tidy Data
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

## `pivot_longer`

load the PULSE data

``` r
pulse_data=
  haven::read_sas("./data_import_examples/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()
```

wide format to LONG format.. originally it is the id with different
weights(readable version), but we want to change it to different weight
as the rows(one id might appear many times) (tidy version)- which is
easier to analyze

``` r
pulse_data_tidy = 
    pulse_data %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, #specifiy the column range
    names_to = "visit", #where those column go into, so we are now creating a new column named visit for those original weight columns
    names_prefix = "bdi_score_", #delete this part of the name, make it clearer
    values_to = "bdi" #we put the values for different weights into this new columns
  )
```

rewrite, combine, and extend (to add a mutate)

``` r
pulse_data = 
  haven::read_sas("./data_import_examples/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit", 
    names_prefix = "bdi_score_", 
    values_to = "bdi" 
  ) %>% 
  relocate(id,visit) %>%  #put id and visit to the first or second columns
  mutate(visit = recode(visit , "bl" = "00m") )  #change this bl variable into 00m
```

## `pivot wider`

Make up some data!

``` r
analysis_result = 
  tibble(
    group = c("treatment", "treatment","placebo", "placebo"),
    time = c("pre", "post", "pre", "post"),
    mean = c(4,8,3.5,4)
  )#tibble is for we making up data by ourselves
analysis_result %>% 
  pivot_wider(
    names_from = "time", #make the "pre" and "post" from time as the column variables
    values_from = "mean" #make the "treatment" and "placebo" from group as the row variables
  )
```

    ## # A tibble: 2 × 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

## binding rows

Using the LotR data.

First step: import each table.

``` r
fellowship_ring = 
  readxl::read_excel("./data_import_examples/LotR_Words.xlsx", range = "B3:D6") %>% 
  mutate(movie = "fellowship_ring")#add a new column named movie with the data called fellowship_ring

two_towers = 
  readxl::read_excel("./data_import_examples/LotR_Words.xlsx", range = "F3:H6") %>% 
  mutate(movie = "two-towers")

return_king = 
  readxl::read_excel("./data_import_examples/LotR_Words.xlsx", range = "J3:L6") %>% 
  mutate(movie = "return_king")
```

Bind all rows together

``` r
lotr_tidy = 
  bind_rows(fellowship_ring, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  relocate(movie) %>% #put movie to the first column
  pivot_longer(
    female:male, # these two column names need to go into the variables
    names_to = "gender",
    values_to = "words")
```
