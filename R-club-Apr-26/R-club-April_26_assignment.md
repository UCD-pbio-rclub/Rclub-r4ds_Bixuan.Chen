# R-club-April 26



> 3.2.4 Exercises
> Q1


```r
library(tidyverse)
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
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

```r
library(ggplot2)

mpg
```

```
## # A tibble: 234 × 11
##    manufacturer      model displ  year   cyl      trans   drv   cty   hwy
##           <chr>      <chr> <dbl> <int> <int>      <chr> <chr> <int> <int>
## 1          audi         a4   1.8  1999     4   auto(l5)     f    18    29
## 2          audi         a4   1.8  1999     4 manual(m5)     f    21    29
## 3          audi         a4   2.0  2008     4 manual(m6)     f    20    31
## 4          audi         a4   2.0  2008     4   auto(av)     f    21    30
## 5          audi         a4   2.8  1999     6   auto(l5)     f    16    26
## 6          audi         a4   2.8  1999     6 manual(m5)     f    18    26
## 7          audi         a4   3.1  2008     6   auto(av)     f    18    27
## 8          audi a4 quattro   1.8  1999     4 manual(m5)     4    18    26
## 9          audi a4 quattro   1.8  1999     4   auto(l5)     4    16    25
## 10         audi a4 quattro   2.0  2008     4 manual(m6)     4    20    28
## # ... with 224 more rows, and 2 more variables: fl <chr>, class <chr>
```

```r
ggplot(data = mpg)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
#no plot was generated. 
#234 rows, 11 columns.

?mpg

#drv: drv. f = front-wheel drive, r = rear wheel drive, 4 = 4wd

ggplot(data = mpg) +
  geom_point(mapping = aes(x = class , y = drv))
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-1-2.png)<!-- -->

```r
#Since both variales are factors, no values. Scatterplot is not helpful. 
```

> 3.3.1 Exercises
> Q1


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = "blue"))
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), color = "blue")
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-2-2.png)<!-- -->

> Q2


```r
?mpg

#Categorical: manufacturer, model, trans, drv, fl, class
#Continuous: displ, year, cyl, cty, hwy
```

> Q3


```r
#color
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = cty))
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = class))
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-4-2.png)<!-- -->

```r
#size
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, size = cty))
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-4-3.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, size = class))
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-4-4.png)<!-- -->

```r
#shape
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, shape = class))
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

```
## Warning: Removed 62 rows containing missing values (geom_point).
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-4-5.png)<!-- -->

> Q4


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, shape = class, color = class))
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

```
## Warning: Removed 62 rows containing missing values (geom_point).
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

> Q5


```r
?geom_point

#strock decides the width of edges of shape. 
```

> Q6


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = class, y = hwy, shape = class, color = displ < 5))
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

```
## Warning: Removed 62 rows containing missing values (geom_point).
```

```
## Warning: The shape palette can deal with a maximum of 6 discrete values
## because more than 6 becomes difficult to discriminate; you have 7.
## Consider specifying shapes manually if you must have them.
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

> 3.5.1
> Q1 


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ cty)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
#Too many subplots.
```

> Q2


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_grid(drv ~ cyl)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = drv, y = cyl))
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-9-2.png)<!-- -->

> Q3


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_grid(drv ~ .)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_grid(. ~ cyl)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-10-2.png)<!-- -->

> Q4


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ class, nrow = 2)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

> Q5


```r
?facet_wrap
#to separate subplots into rows/columns we need
#since the rows and columns decided by the number of data in the variables. 
```


```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_grid(drv ~ cyl)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_grid(cyl~drv)
```

![](R-club-April_26_assignment_files/figure-html/unnamed-chunk-13-2.png)<!-- -->

