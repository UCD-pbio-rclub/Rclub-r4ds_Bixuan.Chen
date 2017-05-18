# R-club-May-17




```r
library(nycflights13)
```

```
## Warning: package 'nycflights13' was built under R version 3.2.5
```

```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 3.2.5
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Warning: package 'ggplot2' was built under R version 3.2.5
```

```
## Warning: package 'tibble' was built under R version 3.2.5
```

```
## Warning: package 'tidyr' was built under R version 3.2.5
```

```
## Warning: package 'readr' was built under R version 3.2.5
```

```
## Warning: package 'purrr' was built under R version 3.2.5
```

```
## Warning: package 'dplyr' was built under R version 3.2.5
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```


```r
summarise(flights, delay = mean(dep_delay, na.rm = TRUE))
```

```
## # A tibble: 1 × 1
##      delay
##      <dbl>
## 1 12.63907
```

```r
by_dest <- group_by(flights, dest)
delay <- summarise(by_dest,
  count = n(),
  dist = mean(distance, na.rm = TRUE),
  delay = mean(arr_delay, na.rm = TRUE)
)

delay <- filter(delay, count > 20, dest != "HNL")

ggplot(data = delay, mapping = aes(x = dist, y = delay)) +
  geom_point(aes(size = count), alpha = 1/3) +
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](R-club-May-17_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
delays <- flights %>% 
  group_by(dest) %>% 
  summarise(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(count > 20, dest != "HNL")
```

Q 5.6.7

```r
#1. 

#2. 
not_cancelled <- flights %>% 
  filter(!is.na(dep_delay), !is.na(arr_delay))

not_cancelled %>%
  group_by(dest) %>%
  summarise(n())
```

```
## # A tibble: 104 × 2
##     dest `n()`
##    <chr> <int>
## 1    ABQ   254
## 2    ACK   264
## 3    ALB   418
## 4    ANC     8
## 5    ATL 16837
## 6    AUS  2411
## 7    AVL   261
## 8    BDL   412
## 9    BGR   358
## 10   BHM   269
## # ... with 94 more rows
```

```r
not_cancelled %>%
  group_by(tailnum) %>%
  summarise(sum(distance))
```

```
## # A tibble: 4,037 × 2
##    tailnum `sum(distance)`
##      <chr>           <dbl>
## 1   D942DN            3418
## 2   N0EGMQ          239143
## 3   N10156          109664
## 4   N102UW           25722
## 5   N103US           24619
## 6   N104UW           24616
## 7   N10575          139903
## 8   N105UW           23618
## 9   N107US           21677
## 10  N108UW           32070
## # ... with 4,027 more rows
```

```r
#3. Not every flight with "NA" in arr_delay was canceled. dep_delay is more important. 


#4. 
cancelled <- flights %>% 
  filter(is.na(dep_time))

cancelled %>%
  group_by(year, month, day) %>%
  summarise(n()) 
```

```
## Source: local data frame [358 x 4]
## Groups: year, month [?]
## 
##     year month   day `n()`
##    <int> <int> <int> <int>
## 1   2013     1     1     4
## 2   2013     1     2     8
## 3   2013     1     3    10
## 4   2013     1     4     6
## 5   2013     1     5     3
## 6   2013     1     6     1
## 7   2013     1     7     3
## 8   2013     1     8     4
## 9   2013     1     9     5
## 10  2013     1    10     3
## # ... with 348 more rows
```

```r
cancelled <- flights %>% 
  filter(is.na(dep_time))

not_cancelled <- flights %>%
  filter(!is.na(dep_time))

by_day_cancelled <- group_by(cancelled, year, month, day)
by_day_not_cancelled <- group_by(not_cancelled, year, month, day)
total_flights <- group_by(flights, year, month, day)

correlation <- summarise(total_flights, 
    prop_cancelled = n()/sum(n()),
    ave_delay = mean(dep_delay, na.rm = TRUE))

not_cancelled %>% 
  group_by(year, month, day) %>% 
  summarise(hour_perc = mean(arr_delay > 60))
```

```
## Source: local data frame [365 x 4]
## Groups: year, month [?]
## 
##     year month   day  hour_perc
##    <int> <int> <int>      <dbl>
## 1   2013     1     1         NA
## 2   2013     1     2         NA
## 3   2013     1     3         NA
## 4   2013     1     4         NA
## 5   2013     1     5 0.03486750
## 6   2013     1     6         NA
## 7   2013     1     7 0.03333333
## 8   2013     1     8         NA
## 9   2013     1     9         NA
## 10  2013     1    10 0.01829925
## # ... with 355 more rows
```

```r
#5. 
not_cancelled <- flights %>%
  filter(!is.na(dep_time))

not_cancelled %>%
  group_by(carrier) %>%
  summarise(delay_time = mean(arr_delay > 0 & !is.na(arr_delay))) %>%
  arrange(desc(delay_time))
```

```
## # A tibble: 16 × 2
##    carrier delay_time
##      <chr>      <dbl>
## 1       FL  0.5946031
## 2       F9  0.5747801
## 3       EV  0.4767505
## 4       YV  0.4733945
## 5       MQ  0.4646902
## 6       WN  0.4389638
## 7       B6  0.4358397
## 8       UA  0.3832767
## 9       9E  0.3810864
## 10      US  0.3697982
## 11      OO  0.3448276
## 12      DL  0.3436486
## 13      VX  0.3402845
## 14      AA  0.3335930
## 15      HA  0.2836257
## 16      AS  0.2654494
```

```r
flights %>% 
  group_by(carrier, dest) %>% 
  summarise(n())
```

```
## Source: local data frame [314 x 3]
## Groups: carrier [?]
## 
##    carrier  dest `n()`
##      <chr> <chr> <int>
## 1       9E   ATL    59
## 2       9E   AUS     2
## 3       9E   AVL    10
## 4       9E   BGR     1
## 5       9E   BNA   474
## 6       9E   BOS   914
## 7       9E   BTV     2
## 8       9E   BUF   833
## 9       9E   BWI   856
## 10      9E   CAE     3
## # ... with 304 more rows
```

```r
#6. 
count(flights, dest, sort = TRUE)
```

```
## # A tibble: 105 × 2
##     dest     n
##    <chr> <int>
## 1    ORD 17283
## 2    ATL 17215
## 3    LAX 16174
## 4    BOS 15508
## 5    MCO 14082
## 6    CLT 14064
## 7    SFO 13331
## 8    FLL 12055
## 9    MIA 11728
## 10   DCA  9705
## # ... with 95 more rows
```

Q 5.7.1

```r
#1. 

#2. 
flights %>%
  group_by(tailnum) %>%
  summarise(ave_delay_time = mean(arr_delay[arr_delay >0 & !is.na(arr_delay)])) %>%
  arrange(desc(ave_delay_time)) 
```

```
## # A tibble: 4,044 × 2
##    tailnum ave_delay_time
##      <chr>          <dbl>
## 1   N844MH            320
## 2   N911DA            294
## 3   N922EV            276
## 4   N665MQ            270
## 5   N587NW            264
## 6   N828AW            229
## 7   N851NW            219
## 8   N928DN            201
## 9   N305AS            196
## 10  N907MQ            191
## # ... with 4,034 more rows
```

```r
#3. 
  flights %>%
  group_by(month, day) %>% 
  summarise(num_delay = sum(!is.na(arr_delay) & arr_delay > 0)) %>%
  arrange(desc(num_delay))
```

```
## Source: local data frame [365 x 3]
## Groups: month [12]
## 
##    month   day num_delay
##    <int> <int>     <int>
## 1     12    17       780
## 2      4    25       750
## 3     12     9       745
## 4      6    13       742
## 5      3     8       715
## 6      7     1       698
## 7      8    13       691
## 8      8     8       690
## 9      8     9       689
## 10     4    22       685
## # ... with 355 more rows
```

```r
#4. 
  flights %>%
    group_by(dest) %>%
    summarise(minute = sum(arr_delay, na.rm = TRUE))
```

```
## # A tibble: 105 × 2
##     dest minute
##    <chr>  <dbl>
## 1    ABQ   1113
## 2    ACK   1281
## 3    ALB   6018
## 4    ANC    -20
## 5    ATL 190260
## 6    AUS  14514
## 7    AVL   2089
## 8    BDL   2904
## 9    BGR   2874
## 10   BHM   4540
## # ... with 95 more rows
```

```r
  flights %>%
    group_by(tailnum) %>%
    summarise(prop_delay = mean(arr_delay > 0, na.rm = TRUE))
```

```
## # A tibble: 4,044 × 2
##    tailnum prop_delay
##      <chr>      <dbl>
## 1   D942DN  0.7500000
## 2   N0EGMQ  0.4517045
## 3   N10156  0.5034483
## 4   N102UW  0.2916667
## 5   N103US  0.2826087
## 6   N104UW  0.2826087
## 7   N10575  0.4721190
## 8   N105UW  0.3555556
## 9   N107US  0.2439024
## 10  N108UW  0.3500000
## # ... with 4,034 more rows
```

```r
#5. 
  ?lag()
```

```
## Help on topic 'lag' was found in the following packages:
## 
##   Package               Library
##   dplyr                 /Library/Frameworks/R.framework/Versions/3.2/Resources/library
##   stats                 /Library/Frameworks/R.framework/Versions/3.2/Resources/library
## 
## 
## Using the first match ...
```

```r
#6. 

#7. ???
 flights %>%
     group_by(dest, carrier) %>%
     count(carrier)
```

```
## Source: local data frame [314 x 3]
## Groups: dest [?]
## 
##     dest carrier     n
##    <chr>   <chr> <int>
## 1    ABQ      B6   254
## 2    ACK      B6   265
## 3    ALB      EV   439
## 4    ANC      UA     8
## 5    ATL      9E    59
## 6    ATL      DL 10571
## 7    ATL      EV  1764
## 8    ATL      FL  2337
## 9    ATL      MQ  2322
## 10   ATL      UA   103
## # ... with 304 more rows
```

```r
#8. 
```

