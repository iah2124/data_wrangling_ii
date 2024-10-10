datawranglingii
================
Iris Hart
2024-10-10

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

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

Get pieces I actually need.

``` r
marj_use_df =
drug_use_html |>
  html_table() |>
first() |>
  slice(-1)
```

``` r
view(marj_use_df)
```

Try on your own - NYC cost of living

``` r
nyc_cost_df = 
  read_html("https://www.bestplaces.net/cost_of_living/city/new_york/new_york") |>
  html_table(header = TRUE) |> 
  first()
```

``` r
nyc_cost_df
```

    ## # A tibble: 8 × 4
    ##   Categories       `New York` `New York` `United States`
    ##   <chr>            <chr>      <chr>      <chr>          
    ## 1 Overall          172.5      121.5      100.0          
    ## 2 Grocery          116.6      103.8      100            
    ## 3 Health           127.6      120.7      100.0          
    ## 4 Housing          224.3      127.9      100.0          
    ## 5 Median Home Cost $677,200   $413,600   $338,100       
    ## 6 Utilities        150.5%     115.9%     100.0%         
    ## 7 Transportation   181.1      140.7      100.0          
    ## 8 Miscellaneous    136.4      121.8      100.0

star wars example

``` r
swm_url = "https://www.imdb.com/list/ls070150896/"

swm_html = 
  read_html(swm_url)
```

``` r
title_vec = 
  swm_html |>
  html_elements(".ipc-title-link-wrapper .ipc-title__text") |>
  html_text()

metascore_vec =
  swm_html |>
  html_elements(".dli-title-metadata-item:nth-child(2)") |>
  html_text()
  
runtime_vec = 
  swm_html |>
  html_elements(".metacritic-score-box") |>
  html_text()

swm_df = 
  tibble(
    title = title_vec,
    score = metascore_vec,
    runtime = runtime_vec)
```

``` r
url = "http://books.toscrape.com"

books_html = read_html(url)

books_titles = 
  books_html |>
   html_elements("h3") |>
  html_text2()
  
books_stars = 
  books_html |>
  html_elements(".star-rating") |>
  html_attr("class")

books_price = 
  books_html |>
  html_elements(".price_color") |>
  html_text()

books = tibble(
  title = books_titles,
  stars = books_stars,
  price = books_price
)
```
