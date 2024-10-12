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

wide format to long format.. originally it is the id with different
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
