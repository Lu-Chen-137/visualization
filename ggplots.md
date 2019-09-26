ggplot
================

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2017-01-01",
                      date_max = "2017-12-31") %>%
  mutate(
    name = recode(id, USW00094728 = "CentralPark_NY", 
                      USC00519397 = "Waikiki_HA",
                      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'crul':
    ##   method                 from
    ##   as.character.form_file httr

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## file path:          /Users/kellychen/Library/Caches/rnoaa/ghcnd/USW00094728.dly

    ## file last updated:  2019-09-26 10:26:37

    ## file min/max dates: 1869-01-01 / 2019-09-30

    ## file path:          /Users/kellychen/Library/Caches/rnoaa/ghcnd/USC00519397.dly

    ## file last updated:  2019-09-26 10:26:46

    ## file min/max dates: 1965-01-01 / 2019-09-30

    ## file path:          /Users/kellychen/Library/Caches/rnoaa/ghcnd/USS0023B17S.dly

    ## file last updated:  2019-09-26 10:26:49

    ## file min/max dates: 1999-09-01 / 2019-09-30

create a ggplot
---------------

``` r
ggplot(weather_df, aes(x= tmin, y = tmax)) + #definding the dataset itself, what's the x-axis, and what's the y-axis;
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplots_files/figure-markdown_github/unnamed-chunk-1-1.png) saving initial plots

``` r
#weather_df %>% filter(name ==)
scatterplot = 
   weather_df %>% 
  ggplot(aes(x= tmin, y = tmax)) + geom_point()

scatterplot
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplots_files/figure-markdown_github/unnamed-chunk-2-1.png)

adding color

``` r
weather_df %>% 
  ggplot(aes(x= tmin, y = tmax)) + 
  geom_point(aes(color=name), alpha = 0.4) #alpha statemtn: adjusting the transparency, range[0,1]
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplots_files/figure-markdown_github/unnamed-chunk-3-1.png) why do 'aes' positions matter?

``` r
weather_df %>% 
  ggplot(aes(x= tmin, y = tmax)) + 
  geom_point(aes(color=name), alpha = 0.4) +
  geom_smooth(se.=FALSE) #giving a smooth curve about the scatterplot; #se.=FALSE about confidence interval
```

    ## Warning: Ignoring unknown parameters: se.

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplots_files/figure-markdown_github/unnamed-chunk-4-1.png) vs.

``` r
weather_df %>% 
  ggplot(aes(x= tmin, y = tmax, color = name)) + 
  geom_point(alpha = 0.4) +
  geom_smooth(se = FALSE)  #giving a smooth curve about the scatterplot; #se =FALSE:get rid of the confidence interval shown in the graph
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplots_files/figure-markdown_github/unnamed-chunk-5-1.png)

``` r
  facet_grid(~ name) #facet_grid: separate the data by name
```

    ## <ggproto object: Class FacetGrid, Facet, gg>
    ##     compute_layout: function
    ##     draw_back: function
    ##     draw_front: function
    ##     draw_labels: function
    ##     draw_panels: function
    ##     finish_data: function
    ##     init_scales: function
    ##     map_data: function
    ##     params: list
    ##     setup_data: function
    ##     setup_params: function
    ##     shrink: TRUE
    ##     train_scales: function
    ##     vars: function
    ##     super:  <ggproto object: Class FacetGrid, Facet, gg>

``` r
weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point() +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplots_files/figure-markdown_github/unnamed-chunk-6-1.png)

``` r
ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_smooth(size =2, se = FALSE) 
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

![](ggplots_files/figure-markdown_github/unnamed-chunk-7-1.png)

2d density

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_bin2d()
```

    ## Warning: Removed 15 rows containing non-finite values (stat_bin2d).

![](ggplots_files/figure-markdown_github/unnamed-chunk-8-1.png)

``` r
  #geom_hex(): after install.packages("hexbin")
```

more kinds of plots!
--------------------

``` r
weather_df %>% 
  ggplot(aes(x = tmax, fill = name)) + #color: tells you the bar outside, if you want to color the inside part, use fill
  geom_histogram() +
  facet_grid(~name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 3 rows containing non-finite values (stat_bin).

![](ggplots_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
weather_df %>% 
  ggplot(aes(x = tmax, fill = name)) +
  geom_density(alpha = 0.3) 
```

    ## Warning: Removed 3 rows containing non-finite values (stat_density).

![](ggplots_files/figure-markdown_github/unnamed-chunk-10-1.png)

``` r
  #facet_grid(~name)
```

``` r
  ggplot(weather_df, aes(x = name, y = tmax)) + geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](ggplots_files/figure-markdown_github/unnamed-chunk-11-1.png)

``` r
ggplot(weather_df, aes(x = name, y = tmax)) + 
  geom_violin(aes(fill = name), color = "blue", alpha = .5) + 
  stat_summary(fun.y = median, geom = "point", color = "blue", size = 4)
```

    ## Warning: Removed 3 rows containing non-finite values (stat_ydensity).

    ## Warning: Removed 3 rows containing non-finite values (stat_summary).

![](ggplots_files/figure-markdown_github/unnamed-chunk-12-1.png)

``` r
weather_df %>% 
ggplot(aes(x = tmax, y = name)) + 
  geom_density_ridges(scale = .85)
```

    ## Picking joint bandwidth of 1.84

    ## Warning: Removed 3 rows containing non-finite values (stat_density_ridges).

![](ggplots_files/figure-markdown_github/unnamed-chunk-13-1.png)

saving a plot
-------------

``` r
ggp_ridge_temp = 
  weather_df %>% 
  ggplot(aes( x= tmax, y= name)) +
  geom_density_ridges()

ggsave("ggplot_temp_ridge.pdf",
       ggp_ridge_temp)
```

    ## Saving 7 x 5 in image

    ## Picking joint bandwidth of 1.84

    ## Warning: Removed 3 rows containing non-finite values (stat_density_ridges).

``` r
weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(alpha = 0.4) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplots_files/figure-markdown_github/unnamed-chunk-15-1.png)
