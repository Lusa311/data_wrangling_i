Data Manipulation
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

## Load in the FAS Litters Data

``` r
litters_df=read_csv("./data_import_examples/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df=janitor::clean_names(litters_df)
```

## `select`

choose some columns and not others.

``` r
select(litters_df,group, litter_number)#I only want to keep the columns of "group" and "litter_number"
```

    ## # A tibble: 49 × 2
    ##    group litter_number  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # ℹ 39 more rows

``` r
select(litters_df,group:gd_of_birth)#I can select the range of columns, so in this case I keep the columns from group to gd_of_birth
```

    ## # A tibble: 49 × 5
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>           <chr>      <chr>             <dbl>
    ##  1 Con7  #85             19.7       34.7                 20
    ##  2 Con7  #1/2/95/2       27         42                   19
    ##  3 Con7  #5/5/3/83/3-3   26         41.4                 19
    ##  4 Con7  #5/4/2/95/2     28.5       44.1                 19
    ##  5 Con7  #4/2/95/3-3     <NA>       <NA>                 20
    ##  6 Con7  #2/2/95/3-2     <NA>       <NA>                 20
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20
    ##  8 Con8  #3/83/3-3       <NA>       <NA>                 20
    ##  9 Con8  #2/95/3         <NA>       <NA>                 20
    ## 10 Con8  #3/5/2/2/95     28.5       <NA>                 20
    ## # ℹ 39 more rows

``` r
select(litters_df,-litter_number)#we drop the litter_number column by add a "-"
```

    ## # A tibble: 49 × 7
    ##    group gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_birth
    ##    <chr> <chr>      <chr>             <dbl>           <dbl>           <dbl>
    ##  1 Con7  19.7       34.7                 20               3               4
    ##  2 Con7  27         42                   19               8               0
    ##  3 Con7  26         41.4                 19               6               0
    ##  4 Con7  28.5       44.1                 19               5               1
    ##  5 Con7  <NA>       <NA>                 20               6               0
    ##  6 Con7  <NA>       <NA>                 20               6               0
    ##  7 Con7  <NA>       <NA>                 20               9               0
    ##  8 Con8  <NA>       <NA>                 20               9               1
    ##  9 Con8  <NA>       <NA>                 20               8               0
    ## 10 Con8  28.5       <NA>                 20               8               0
    ## # ℹ 39 more rows
    ## # ℹ 1 more variable: pups_survive <dbl>

Remaining columns…

``` r
select(litters_df,GROUP = group, LITTER_NUmBer = litter_number)#We will rename these two columns but only keep these two columns
```

    ## # A tibble: 49 × 2
    ##    GROUP LITTER_NUmBer  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # ℹ 39 more rows

``` r
rename(litters_df,GROUP = group, LITTER_NUmBer = litter_number)#We can rename these two columns but keep the rest of the columns at the same time
```

    ## # A tibble: 49 × 8
    ##    GROUP LITTER_NUmBer   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2       27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3   26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2     28.5       44.1                 19               5
    ##  5 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  6 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  8 Con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  9 Con8  #2/95/3         <NA>       <NA>                 20               8
    ## 10 Con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

?select_helpers

``` r
select(litters_df,starts_with("gd"))#select the variable that have gd
```

    ## # A tibble: 49 × 3
    ##    gd0_weight gd18_weight gd_of_birth
    ##    <chr>      <chr>             <dbl>
    ##  1 19.7       34.7                 20
    ##  2 27         42                   19
    ##  3 26         41.4                 19
    ##  4 28.5       44.1                 19
    ##  5 <NA>       <NA>                 20
    ##  6 <NA>       <NA>                 20
    ##  7 <NA>       <NA>                 20
    ##  8 <NA>       <NA>                 20
    ##  9 <NA>       <NA>                 20
    ## 10 28.5       <NA>                 20
    ## # ℹ 39 more rows

``` r
select(litters_df,litter_number,everything())#put the litter_number to the first column, and keep everything
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr> <chr>      <chr>             <dbl>           <dbl>
    ##  1 #85             Con7  19.7       34.7                 20               3
    ##  2 #1/2/95/2       Con7  27         42                   19               8
    ##  3 #5/5/3/83/3-3   Con7  26         41.4                 19               6
    ##  4 #5/4/2/95/2     Con7  28.5       44.1                 19               5
    ##  5 #4/2/95/3-3     Con7  <NA>       <NA>                 20               6
    ##  6 #2/2/95/3-2     Con7  <NA>       <NA>                 20               6
    ##  7 #1/5/3/83/3-3/2 Con7  <NA>       <NA>                 20               9
    ##  8 #3/83/3-3       Con8  <NA>       <NA>                 20               9
    ##  9 #2/95/3         Con8  <NA>       <NA>                 20               8
    ## 10 #3/5/2/2/95     Con8  28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
relocate(litters_df,litter_number)#relocatet litter_number to the first column, and not changing eveything else
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr> <chr>      <chr>             <dbl>           <dbl>
    ##  1 #85             Con7  19.7       34.7                 20               3
    ##  2 #1/2/95/2       Con7  27         42                   19               8
    ##  3 #5/5/3/83/3-3   Con7  26         41.4                 19               6
    ##  4 #5/4/2/95/2     Con7  28.5       44.1                 19               5
    ##  5 #4/2/95/3-3     Con7  <NA>       <NA>                 20               6
    ##  6 #2/2/95/3-2     Con7  <NA>       <NA>                 20               6
    ##  7 #1/5/3/83/3-3/2 Con7  <NA>       <NA>                 20               9
    ##  8 #3/83/3-3       Con8  <NA>       <NA>                 20               9
    ##  9 #2/95/3         Con8  <NA>       <NA>                 20               8
    ## 10 #3/5/2/2/95     Con8  28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

\##`filter`

``` r
filter(litters_df, gd0_weight < 22) #only keep the people with gd0_weight less than 22
```

    ## # A tibble: 10 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Mod7  #59           17         33.4                 19               8
    ##  3 Mod7  #103          21.4       42.1                 19               9
    ##  4 Mod7  #8/110/3-2    .          .                    20               9
    ##  5 Mod7  #106          21.7       37.8                 20               5
    ##  6 Mod7  #62           19.5       35.9                 19               7
    ##  7 Mod8  #5/93/2       .          .                    19               8
    ##  8 Low8  #53           21.8       37.2                 20               8
    ##  9 Low8  #100          20         39.2                 20               8
    ## 10 Low8  #4/84         21.8       35.2                 20               4
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, gd0_weight >= 22)
```

    ## # A tibble: 26 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2     27         42                   19               8
    ##  2 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ##  3 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  4 Con8  #3/5/2/2/95   28.5       <NA>                 20               8
    ##  5 Con8  #5/4/3/83/3   28         <NA>                 19               9
    ##  6 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  7 Mod7  #4/2/95/2     23.5       <NA>                 19               9
    ##  8 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ##  9 Mod7  #94/2         24.4       42.9                 19               7
    ## 10 Low7  #84/2         24.3       40.8                 20               8
    ## # ℹ 16 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, gd_of_birth==20)#we use double = is because we are not asking the variable to go into 20, we are just asking are these two thing equal to each others
```

    ## # A tibble: 32 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  3 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  4 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  5 Con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  6 Con8  #2/95/3         <NA>       <NA>                 20               8
    ##  7 Con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ##  8 Con8  #1/6/2/2/95-2   <NA>       <NA>                 20               7
    ##  9 Con8  #3/5/3/83/3-3-2 <NA>       <NA>                 20               8
    ## 10 Con8  #3/6/2/2/95-3   <NA>       <NA>                 20               7
    ## # ℹ 22 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, !(gd_of_birth==20))#show the people that have gd_of_birth is not equal to 20
```

    ## # A tibble: 17 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2     27         42                   19               8
    ##  2 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ##  3 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  4 Con8  #5/4/3/83/3   28         <NA>                 19               9
    ##  5 Con8  #2/2/95/2     <NA>       <NA>                 19               5
    ##  6 Mod7  #59           17         33.4                 19               8
    ##  7 Mod7  #103          21.4       42.1                 19               9
    ##  8 Mod7  #1/82/3-2     <NA>       <NA>                 19               6
    ##  9 Mod7  #3/83/3-2     <NA>       <NA>                 19               8
    ## 10 Mod7  #4/2/95/2     23.5       <NA>                 19               9
    ## 11 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ## 12 Mod7  #94/2         24.4       42.9                 19               7
    ## 13 Mod7  #62           19.5       35.9                 19               7
    ## 14 Low7  #112          23.9       40.5                 19               6
    ## 15 Mod8  #5/93/2       .          .                    19               8
    ## 16 Mod8  #7/110/3-2    27.5       46                   19               8
    ## 17 Low8  #79           25.4       43.8                 19               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, gd0_weight >= 22, gd_of_birth==20)
```

    ## # A tibble: 16 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con8  #3/5/2/2/95   28.5       <NA>                 20               8
    ##  2 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  3 Low7  #84/2         24.3       40.8                 20               8
    ##  4 Low7  #107          22.6       42.4                 20               9
    ##  5 Low7  #85/2         22.2       38.5                 20               8
    ##  6 Low7  #98           23.8       43.8                 20               9
    ##  7 Low7  #102          22.6       43.3                 20              11
    ##  8 Low7  #101          23.8       42.7                 20               9
    ##  9 Low7  #111          25.5       44.6                 20               3
    ## 10 Mod8  #97           24.5       42.8                 20               8
    ## 11 Mod8  #7/82-3-2     26.9       43.2                 20               7
    ## 12 Mod8  #2/95/2       28.5       44.5                 20               9
    ## 13 Mod8  #82/4         33.4       52.7                 20               8
    ## 14 Low8  #108          25.6       47.5                 20               8
    ## 15 Low8  #99           23.5       39                   20               6
    ## 16 Low8  #110          25.5       42.7                 20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group =="Mod8") #only keeps the rows with Mod8
```

    ## # A tibble: 7 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ## 1 Mod8  #97           24.5       42.8                 20               8
    ## 2 Mod8  #5/93         <NA>       41.1                 20              11
    ## 3 Mod8  #5/93/2       .          .                    19               8
    ## 4 Mod8  #7/82-3-2     26.9       43.2                 20               7
    ## 5 Mod8  #7/110/3-2    27.5       46                   19               8
    ## 6 Mod8  #2/95/2       28.5       44.5                 20               9
    ## 7 Mod8  #82/4         33.4       52.7                 20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group %in% c("Mod8","Mod7")) #keep the groups either Mod8 or Mod7
```

    ## # A tibble: 19 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Mod7  #59           17         33.4                 19               8
    ##  2 Mod7  #103          21.4       42.1                 19               9
    ##  3 Mod7  #1/82/3-2     <NA>       <NA>                 19               6
    ##  4 Mod7  #3/83/3-2     <NA>       <NA>                 19               8
    ##  5 Mod7  #2/95/2-2     <NA>       <NA>                 20               7
    ##  6 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  7 Mod7  #4/2/95/2     23.5       <NA>                 19               9
    ##  8 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ##  9 Mod7  #8/110/3-2    .          .                    20               9
    ## 10 Mod7  #106          21.7       37.8                 20               5
    ## 11 Mod7  #94/2         24.4       42.9                 19               7
    ## 12 Mod7  #62           19.5       35.9                 19               7
    ## 13 Mod8  #97           24.5       42.8                 20               8
    ## 14 Mod8  #5/93         <NA>       41.1                 20              11
    ## 15 Mod8  #5/93/2       .          .                    19               8
    ## 16 Mod8  #7/82-3-2     26.9       43.2                 20               7
    ## 17 Mod8  #7/110/3-2    27.5       46                   19               8
    ## 18 Mod8  #2/95/2       28.5       44.5                 20               9
    ## 19 Mod8  #82/4         33.4       52.7                 20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

\##`mutate`

``` r
mutate(
  litters_df,
  wt_gain = as.numeric(gd18_weight) - as.numeric(gd0_weight), #we can create a new variable through mutate
  #I added the as.numeric as only numeric numbers can minus each others, there was a categorical number before
  group = str_to_lower(group))#at the same time we can also adjust the existing variable "group" to lower case
```

    ## Warning: There were 2 warnings in `mutate()`.
    ## The first warning was:
    ## ℹ In argument: `wt_gain = as.numeric(gd18_weight) - as.numeric(gd0_weight)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion
    ## ℹ Run `dplyr::last_dplyr_warnings()` to see the 1 remaining warning.

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 con7  #85             19.7       34.7                 20               3
    ##  2 con7  #1/2/95/2       27         42                   19               8
    ##  3 con7  #5/5/3/83/3-3   26         41.4                 19               6
    ##  4 con7  #5/4/2/95/2     28.5       44.1                 19               5
    ##  5 con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  6 con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  7 con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  8 con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  9 con8  #2/95/3         <NA>       <NA>                 20               8
    ## 10 con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

## `arrange`

putting things in order

``` r
arrange(litters_df,pups_born_alive) #arrange the dataset by order of pups_born_alive
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>         <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85           19.7       34.7                 20               3
    ##  2 Low7  #111          25.5       44.6                 20               3
    ##  3 Low8  #4/84         21.8       35.2                 20               4
    ##  4 Con7  #5/4/2/95/2   28.5       44.1                 19               5
    ##  5 Con8  #2/2/95/2     <NA>       <NA>                 19               5
    ##  6 Mod7  #3/82/3-2     28         45.9                 20               5
    ##  7 Mod7  #5/3/83/5-2   22.6       37                   19               5
    ##  8 Mod7  #106          21.7       37.8                 20               5
    ##  9 Con7  #5/5/3/83/3-3 26         41.4                 19               6
    ## 10 Con7  #4/2/95/3-3   <NA>       <NA>                 20               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

we can also write arrange(litters_df, pups_born_alive, gd0_weight) it
arrange the order for pups_born_alive first, and then gd0_weight

## `%>%`

``` r
litters_data_raw = read_csv("./data_import_examples/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_clean_name = janitor::clean_names(litters_data_raw)
litters_data_selected = select (litters_clean_name, -pups_survive)#select the dataset except pups_survive
litters_mutated = mutate(litters_data_selected, wt_gain = as.numeric(gd18_weight) - as.numeric(gd0_weight))
```

    ## Warning: There were 2 warnings in `mutate()`.
    ## The first warning was:
    ## ℹ In argument: `wt_gain = as.numeric(gd18_weight) - as.numeric(gd0_weight)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion
    ## ℹ Run `dplyr::last_dplyr_warnings()` to see the 1 remaining warning.

``` r
litters_without_missing = drop_na(litters_mutated, gd0_weight)
```

USE THE PIPE OPERATOR INSTEAD

``` r
litters_df = 
  read_csv("./data_import_examples/FAS_litters.csv") %>% 
  janitor::clean_names() %>% #we don' t need to fill in the bracket, becuase we are using pipe operator, so whatever it comes out for the read_csv will be the thing in the bracket
  select(-pups_survive) %>% 
  mutate(wt_gain = as.numeric(gd18_weight) - as.numeric(gd0_weight)) %>% 
  drop_na(gd0_weight)
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## Warning: There were 2 warnings in `mutate()`.
    ## The first warning was:
    ## ℹ In argument: `wt_gain = as.numeric(gd18_weight) - as.numeric(gd0_weight)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion
    ## ℹ Run `dplyr::last_dplyr_warnings()` to see the 1 remaining warning.
