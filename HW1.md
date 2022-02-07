HW1
================
Jacob Elrod

# Loading in:

Load in and rename Dataset, make sex a factor for convienence

``` r
library(readxl)
df <- read_excel("~/Copy of Head morphology.xlsx")


str(df)
```

    ## tibble [358 x 8] (S3: tbl_df/tbl/data.frame)
    ##  $ SPECIES         : chr [1:358] "R. grahamii" "R. grahamii" "R. grahamii" "R. grahamii" ...
    ##  $ SVL_(cm)        : num [1:358] 26 43 37.5 62.5 19 19 19 20 19.5 18 ...
    ##  $ SEX             : chr [1:358] "male" "male" "female" "male" ...
    ##  $ TEETH_NUMBER    : num [1:358] 24 NA 24 26 24 23 NA NA 27 NA ...
    ##  $ head_width_(mm) : num [1:358] 6.5 6.8 6.4 11.3 5.8 5.3 5.3 5.2 4.9 5 ...
    ##  $ head_length_(mm): num [1:358] 10.3 13.8 11.9 14.5 10 9.8 10 9.8 10 9.4 ...
    ##  $ jaw_length_(mm) : num [1:358] 11 15 12.4 15 10 10.2 9.8 9 9 9.2 ...
    ##  $ GAPE_INDEX      : num [1:358] 56.8 73.2 62 83.4 51.4 ...

``` r
df$SEX<- as.factor(df$SEX) 
```

# Split, Apply, Combine

Take dataset, omit NAs, group by sex and species, take the mean of teeth
number by sex and species

``` r
df_sum<-df%>% na.omit() %>% group_by(SEX ,SPECIES) %>% summarize(mean_teeth = mean(TEETH_NUMBER))
```

    ## `summarise()` has grouped output by 'SEX'. You can override using the `.groups` argument.

``` r
df_sum
```

    ## # A tibble: 10 x 3
    ## # Groups:   SEX [2]
    ##    SEX    SPECIES                mean_teeth
    ##    <fct>  <chr>                       <dbl>
    ##  1 female R. grahamii                  24.5
    ##  2 female R. septemvittata             24.3
    ##  3 female T. eques                     36.8
    ##  4 female T. mel eat crayfish          34.4
    ##  5 female T. mel No eat crayfish       33.2
    ##  6 male   R. grahamii                  24.5
    ##  7 male   R. septemvittata             24.7
    ##  8 male   T. eques                     37.3
    ##  9 male   T. mel eat crayfish          34.6
    ## 10 male   T. mel No eat crayfish       31.9

# Plots

First a density plot to show distributions of teeth number, to see if
data is normal just in case we eventually want to do statistical tests.

``` r
ggplot(df, aes(x = TEETH_NUMBER, fill = factor(SPECIES), )) + geom_density(alpha = .5)
```

    ## Warning: Removed 26 rows containing non-finite values (stat_density).

![](HW1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Next, a point plot using the average of each species, colored by sex.

``` r
ggplot(df_sum, aes(x= SPECIES, y= mean_teeth, col = SEX))+geom_point()
```

![](HW1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

By the analysis and visualization, we can see that the overall mean and
median of the number of teeth in snakes doesn’t seem to vary much by sex
of the snake(except for potentially T.Mel no eat crayfish), but clearly
differs by species. This could be further proven by a statistic test
that compares means such as a t test for the male and female, and
perhaps and ANOVA for the species.
