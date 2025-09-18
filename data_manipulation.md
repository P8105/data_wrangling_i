Data Manipulation
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

Let’s import the datasets.

``` r
litters_df = 
  read_csv("data/FAS_litters.csv", na = c("NA", ".", ""))
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = 
  janitor::clean_names(litters_df)

pups_df = 
  read_csv("data/FAS_pups.csv", skip = 3, na = c("NA", ".", ""))
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df = 
  janitor::clean_names(pups_df)
```

## `select`

`select` selects variables from my dataframe.

select specific variables

``` r
select(litters_df, group, litter_number, gd0_weight)
```

    ## # A tibble: 49 × 3
    ##    group litter_number   gd0_weight
    ##    <chr> <chr>                <dbl>
    ##  1 Con7  #85                   19.7
    ##  2 Con7  #1/2/95/2             27  
    ##  3 Con7  #5/5/3/83/3-3         26  
    ##  4 Con7  #5/4/2/95/2           28.5
    ##  5 Con7  #4/2/95/3-3           NA  
    ##  6 Con7  #2/2/95/3-2           NA  
    ##  7 Con7  #1/5/3/83/3-3/2       NA  
    ##  8 Con8  #3/83/3-3             NA  
    ##  9 Con8  #2/95/3               NA  
    ## 10 Con8  #3/5/2/2/95           28.5
    ## # ℹ 39 more rows

select a range

``` r
select(litters_df, group:gd_of_birth)
```

    ## # A tibble: 49 × 5
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>
    ##  1 Con7  #85                   19.7        34.7          20
    ##  2 Con7  #1/2/95/2             27          42            19
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19
    ##  5 Con7  #4/2/95/3-3           NA          NA            20
    ##  6 Con7  #2/2/95/3-2           NA          NA            20
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20
    ##  8 Con8  #3/83/3-3             NA          NA            20
    ##  9 Con8  #2/95/3               NA          NA            20
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20
    ## # ℹ 39 more rows

select by removal

``` r
select(litters_df, -group)
```

    ## # A tibble: 49 × 7
    ##    litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 #85                   19.7        34.7          20               3
    ##  2 #1/2/95/2             27          42            19               8
    ##  3 #5/5/3/83/3-3         26          41.4          19               6
    ##  4 #5/4/2/95/2           28.5        44.1          19               5
    ##  5 #4/2/95/3-3           NA          NA            20               6
    ##  6 #2/2/95/3-2           NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 #3/83/3-3             NA          NA            20               9
    ##  9 #2/95/3               NA          NA            20               8
    ## 10 #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
select(litters_df, -(group:gd_of_birth))
```

    ## # A tibble: 49 × 3
    ##    pups_born_alive pups_dead_birth pups_survive
    ##              <dbl>           <dbl>        <dbl>
    ##  1               3               4            3
    ##  2               8               0            7
    ##  3               6               0            5
    ##  4               5               1            4
    ##  5               6               0            6
    ##  6               6               0            4
    ##  7               9               0            9
    ##  8               9               1            8
    ##  9               8               0            8
    ## 10               8               0            8
    ## # ℹ 39 more rows

select by prefix

``` r
select(litters_df, group, starts_with("gd"))
```

    ## # A tibble: 49 × 4
    ##    group gd0_weight gd18_weight gd_of_birth
    ##    <chr>      <dbl>       <dbl>       <dbl>
    ##  1 Con7        19.7        34.7          20
    ##  2 Con7        27          42            19
    ##  3 Con7        26          41.4          19
    ##  4 Con7        28.5        44.1          19
    ##  5 Con7        NA          NA            20
    ##  6 Con7        NA          NA            20
    ##  7 Con7        NA          NA            20
    ##  8 Con8        NA          NA            20
    ##  9 Con8        NA          NA            20
    ## 10 Con8        28.5        NA            20
    ## # ℹ 39 more rows

rearrange by selecting

``` r
select(litters_df, starts_with("gd"), group)
```

    ## # A tibble: 49 × 4
    ##    gd0_weight gd18_weight gd_of_birth group
    ##         <dbl>       <dbl>       <dbl> <chr>
    ##  1       19.7        34.7          20 Con7 
    ##  2       27          42            19 Con7 
    ##  3       26          41.4          19 Con7 
    ##  4       28.5        44.1          19 Con7 
    ##  5       NA          NA            20 Con7 
    ##  6       NA          NA            20 Con7 
    ##  7       NA          NA            20 Con7 
    ##  8       NA          NA            20 Con8 
    ##  9       NA          NA            20 Con8 
    ## 10       28.5        NA            20 Con8 
    ## # ℹ 39 more rows

``` r
select(litters_df, litter_number, everything())
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr>      <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 #85             Con7        19.7        34.7          20               3
    ##  2 #1/2/95/2       Con7        27          42            19               8
    ##  3 #5/5/3/83/3-3   Con7        26          41.4          19               6
    ##  4 #5/4/2/95/2     Con7        28.5        44.1          19               5
    ##  5 #4/2/95/3-3     Con7        NA          NA            20               6
    ##  6 #2/2/95/3-2     Con7        NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2 Con7        NA          NA            20               9
    ##  8 #3/83/3-3       Con8        NA          NA            20               9
    ##  9 #2/95/3         Con8        NA          NA            20               8
    ## 10 #3/5/2/2/95     Con8        28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
relocate(litters_df, litter_number)
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr>      <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 #85             Con7        19.7        34.7          20               3
    ##  2 #1/2/95/2       Con7        27          42            19               8
    ##  3 #5/5/3/83/3-3   Con7        26          41.4          19               6
    ##  4 #5/4/2/95/2     Con7        28.5        44.1          19               5
    ##  5 #4/2/95/3-3     Con7        NA          NA            20               6
    ##  6 #2/2/95/3-2     Con7        NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2 Con7        NA          NA            20               9
    ##  8 #3/83/3-3       Con8        NA          NA            20               9
    ##  9 #2/95/3         Con8        NA          NA            20               8
    ## 10 #3/5/2/2/95     Con8        28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

rename by selecting

``` r
select(litters_df, GROUP = group, everything())
```

    ## # A tibble: 49 × 8
    ##    GROUP litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
rename(litters_df, GROUP = group)
```

    ## # A tibble: 49 × 8
    ##    GROUP litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

what if i only select one variable…?

``` r
select(litters_df, group)
```

    ## # A tibble: 49 × 1
    ##    group
    ##    <chr>
    ##  1 Con7 
    ##  2 Con7 
    ##  3 Con7 
    ##  4 Con7 
    ##  5 Con7 
    ##  6 Con7 
    ##  7 Con7 
    ##  8 Con8 
    ##  9 Con8 
    ## 10 Con8 
    ## # ℹ 39 more rows

LA code:

``` r
select(pups_df, litter_number, sex, pd_ears)
```

    ## # A tibble: 313 × 3
    ##    litter_number   sex pd_ears
    ##    <chr>         <dbl>   <dbl>
    ##  1 #85               1       4
    ##  2 #85               1       4
    ##  3 #1/2/95/2         1       5
    ##  4 #1/2/95/2         1       5
    ##  5 #5/5/3/83/3-3     1       5
    ##  6 #5/5/3/83/3-3     1       5
    ##  7 #5/4/2/95/2       1      NA
    ##  8 #4/2/95/3-3       1       4
    ##  9 #4/2/95/3-3       1       4
    ## 10 #2/2/95/3-2       1       4
    ## # ℹ 303 more rows

## `filter`

use `filter()` to filter out rows.

``` r
filter(litters_df, gd_of_birth == 20, pups_born_alive == 6)
```

    ## # A tibble: 3 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #4/2/95/3-3         NA            NA          20               6
    ## 2 Con7  #2/2/95/3-2         NA            NA          20               6
    ## 3 Low8  #99                 23.5          39          20               6
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

look for more than five pups alive

``` r
filter(litters_df, pups_born_alive > 5)
```

    ## # A tibble: 41 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2             27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  3 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  4 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  5 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  6 Con8  #3/83/3-3             NA          NA            20               9
    ##  7 Con8  #2/95/3               NA          NA            20               8
    ##  8 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  9 Con8  #5/4/3/83/3           28          NA            19               9
    ## 10 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## # ℹ 31 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive >= 5)
```

    ## # A tibble: 46 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2             27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  4 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  5 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  6 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  7 Con8  #3/83/3-3             NA          NA            20               9
    ##  8 Con8  #2/95/3               NA          NA            20               8
    ##  9 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## 10 Con8  #5/4/3/83/3           28          NA            19               9
    ## # ℹ 36 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive <= 5)
```

    ## # A tibble: 8 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 3 Con8  #2/2/95/2           NA          NA            19               5
    ## 4 Mod7  #3/82/3-2           28          45.9          20               5
    ## 5 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 6 Mod7  #106                21.7        37.8          20               5
    ## 7 Low7  #111                25.5        44.6          20               3
    ## 8 Low8  #4/84               21.8        35.2          20               4
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

look for non-matches

``` r
filter(litters_df, pups_born_alive != 6)
```

    ## # A tibble: 43 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  4 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  5 Con8  #3/83/3-3             NA          NA            20               9
    ##  6 Con8  #2/95/3               NA          NA            20               8
    ##  7 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  8 Con8  #5/4/3/83/3           28          NA            19               9
    ##  9 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 10 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## # ℹ 33 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

match to a set

``` r
filter(litters_df, group %in% c("Con7", "Con8"))
```

    ## # A tibble: 15 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## 11 Con8  #5/4/3/83/3           28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 14 Con8  #2/2/95/2             NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

filter out missing rows with `drop_na()`

``` r
drop_na(litters_df)
```

    ## # A tibble: 31 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Mod7  #59                 17          33.4          19               8
    ##  6 Mod7  #103                21.4        42.1          19               9
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Mod7  #94/2               24.4        42.9          19               7
    ## # ℹ 21 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
drop_na(litters_df, gd0_weight)
```

    ## # A tibble: 34 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  6 Con8  #5/4/3/83/3         28          NA            19               9
    ##  7 Mod7  #59                 17          33.4          19               8
    ##  8 Mod7  #103                21.4        42.1          19               9
    ##  9 Mod7  #3/82/3-2           28          45.9          20               5
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## # ℹ 24 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(pups_df, sex == 1)
```

    ## # A tibble: 155 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #85               1       4      13        7      11
    ##  2 #85               1       4      13        7      12
    ##  3 #1/2/95/2         1       5      13        7       9
    ##  4 #1/2/95/2         1       5      13        8      10
    ##  5 #5/5/3/83/3-3     1       5      13        8      10
    ##  6 #5/5/3/83/3-3     1       5      14        6       9
    ##  7 #5/4/2/95/2       1      NA      14        5       9
    ##  8 #4/2/95/3-3       1       4      13        6       8
    ##  9 #4/2/95/3-3       1       4      13        7       9
    ## 10 #2/2/95/3-2       1       4      NA        8      10
    ## # ℹ 145 more rows

``` r
filter(pups_df, pd_walk < 11, sex == 2)
```

    ## # A tibble: 127 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #1/2/95/2         2       4      13        7       9
    ##  2 #1/2/95/2         2       4      13        7      10
    ##  3 #1/2/95/2         2       5      13        8      10
    ##  4 #1/2/95/2         2       5      13        8      10
    ##  5 #1/2/95/2         2       5      13        6      10
    ##  6 #5/5/3/83/3-3     2       5      13        8      10
    ##  7 #5/5/3/83/3-3     2       5      14        7      10
    ##  8 #5/5/3/83/3-3     2       5      14        8      10
    ##  9 #5/4/2/95/2       2      NA      14        7      10
    ## 10 #5/4/2/95/2       2      NA      14        7      10
    ## # ℹ 117 more rows

## `mutate`

use `mutate()` to create or modify variables.

``` r
mutate(
  litters_df, 
  wt_gain = gd18_weight - gd0_weight, 
  group = str_to_lower(group)
)
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                   19.7        34.7          20               3
    ##  2 con7  #1/2/95/2             27          42            19               8
    ##  3 con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 con8  #3/83/3-3             NA          NA            20               9
    ##  9 con8  #2/95/3               NA          NA            20               8
    ## 10 con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

## `arrange`

`arrange()` arranges things

``` r
arrange(litters_df, pups_born_alive)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Low7  #111                25.5        44.6          20               3
    ##  3 Low8  #4/84               21.8        35.2          20               4
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  8 Mod7  #106                21.7        37.8          20               5
    ##  9 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 10 Con7  #4/2/95/3-3         NA          NA            20               6
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
arrange(litters_df, desc(group), pups_born_alive)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod8  #7/82-3-2           26.9        43.2          20               7
    ##  2 Mod8  #97                 24.5        42.8          20               8
    ##  3 Mod8  #5/93/2             NA          NA            19               8
    ##  4 Mod8  #7/110/3-2          27.5        46            19               8
    ##  5 Mod8  #82/4               33.4        52.7          20               8
    ##  6 Mod8  #2/95/2             28.5        44.5          20               9
    ##  7 Mod8  #5/93               NA          41.1          20              11
    ##  8 Mod7  #3/82/3-2           28          45.9          20               5
    ##  9 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 10 Mod7  #106                21.7        37.8          20               5
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
arrange(litters_df, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod7  #59                 17          33.4          19               8
    ##  2 Mod7  #62                 19.5        35.9          19               7
    ##  3 Con7  #85                 19.7        34.7          20               3
    ##  4 Low8  #100                20          39.2          20               8
    ##  5 Mod7  #103                21.4        42.1          19               9
    ##  6 Mod7  #106                21.7        37.8          20               5
    ##  7 Low8  #53                 21.8        37.2          20               8
    ##  8 Low8  #4/84               21.8        35.2          20               4
    ##  9 Low7  #85/2               22.2        38.5          20               8
    ## 10 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## PIPES

Let’s see a bad way to do things

``` r
litters_df = 
  read_csv("data/FAS_litters.csv", na = c("NA", ".", ""))
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = 
  janitor::clean_names(litters_df)

litters_df2 = 
  select(litters_df, group, litter_number, starts_with("gd"))

litters_df3 = 
  drop_na(litters_df2)

litters_df4 =
  mutate(
    litters_df3,
    wt_gain = gd18_weight - gd0_weight
  )
```

``` r
litters_df = 
  select(
    janitor::clean_names(
      read_csv("data/FAS_litters.csv", na = c("NA", ".", ""))
    ),
    group, litter_number, starts_with("gd")
  )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Use pipes to avoid all this

``` r
litters_df = 
  read_csv("data/FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  select(group, litter_number, starts_with("gd")) |> 
  drop_na() |> 
  mutate(
    wt_gain = gd18_weight - gd0_weight
  )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Import and tidy the pups data in a piped chain of commands.

``` r
pups_df = 
  read_csv("data/FAS_pups.csv", skip = 3, na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  select(-pd_ears) |> 
  filter(sex == 1) |> 
  mutate(pd_pivot_ge7 = pd_pivot >= 7)
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df |> 
  lm(wt_gain ~ gd_of_birth, data = _)
```

    ## 
    ## Call:
    ## lm(formula = wt_gain ~ gd_of_birth, data = litters_df)
    ## 
    ## Coefficients:
    ## (Intercept)  gd_of_birth  
    ##        2.84         0.74
