# R-club-Jun 07




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
read_csv("a,b,c
1,2,3
4,5,6")
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

```r
read_csv("The first line of metadata
  The second line of metadata
  x,y,z
  1,2,3", skip = 2)
```

```
## # A tibble: 1 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
```

```r
read_csv("# A comment I want to skip
  x,y,z
  1,2,3", comment = "#")
```

```
## # A tibble: 1 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
```

```r
read_csv("1,2,3\n4,5,6", col_names = FALSE)
```

```
## # A tibble: 2 × 3
##      X1    X2    X3
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

```r
read_csv("1,2,3\n4,5,6", col_names = c("x", "y", "z"))
```

```
## # A tibble: 2 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

```r
read_csv("a,b,c\n1,2,.", na = ".")
```

```
## # A tibble: 1 × 3
##       a     b     c
##   <int> <int> <chr>
## 1     1     2  <NA>
```


```r
#11.2.2

#Q.1
#read.delim()

#Q.2

#Q.3
#fwf_widths()?

#Q.4
read_delim("x,y\n1,'a,b'", delim = ",", quote = "'")
```

```
## # A tibble: 1 × 2
##       x     y
##   <int> <chr>
## 1     1   a,b
```

```r
#Q.5
read_csv("a,b\n1,2,3\n4,5,6")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 2 columns 3 columns literal data
##   2  -- 2 columns 3 columns literal data
```

```
## # A tibble: 2 × 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

```r
#Only two columns but three valus for each row. 

read_csv("a,b,c\n1,2\n1,2,3,4")
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 3 columns 2 columns literal data
##   2  -- 3 columns 4 columns literal data
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

```r
#Numbers in each row are different

read_csv("a,b\n1")
```

```
## Warning: 1 parsing failure.
## row col  expected    actual         file
##   1  -- 2 columns 1 columns literal data
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <chr>
## 1     1  <NA>
```

```r
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 × 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

```r
read_delim("a;b\n1;3", delim = ";")
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <int>
## 1     1     3
```


```r
#11.3.5

#Q.1
#parse_double, parse_number, parse_character

#Q.2
parse_number("10,000", locale = locale(decimal_mark = ","))
```

```
## [1] 10
```

```r
parse_number("10,000", locale = locale(grouping_mark = ","))
```

```
## [1] 10000
```

```r
parse_number("10,000", locale = locale(decimal_mark = ","))
```

```
## [1] 10
```

```r
parse_number("10.00", locale = locale(grouping_mark = "."))
```

```
## [1] 1000
```

```r
#Q.3
parse_date("10/11/01", locale = locale(date_format = "%m/%d/%y"))
```

```
## [1] "2001-10-11"
```

```r
parse_time("10:11:01", locale = locale(time_format = "%H/%M/%S"))
```

```
## Warning: 1 parsing failure.
## row col   expected   actual
##   1  -- time like  10:11:01
```

```
## NA
```

```r
#Q.4

#Q.5
#read_csv() reads comma delimited files
#read_csv2() reads semicolon separated files (when "," is used as the decimal place)

#Q.6

#Q.7
d1 <- "January 1, 2010"
parse_date(d1, "%B %d, %Y")
```

```
## [1] "2010-01-01"
```

```r
d2 <- "2015-Mar-07"
parse_date(d2, "%Y-%b-%d")
```

```
## [1] "2015-03-07"
```

```r
d3 <- "06-Jun-2017"
parse_date(d3, "%d-%b-%Y")
```

```
## [1] "2017-06-06"
```

```r
d4 <- c("August 19 (2015)", "July 1 (2015)")
parse_date(d4, "%B %d (%Y)", "%B %d (%Y)")
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
d5 <- "12/30/14"
parse_date(d5, "%m/%d/%y")
```

```
## [1] "2014-12-30"
```

```r
t1 <- "1705"
parse_time(t1, "%H%M")
```

```
## 17:05:00
```

```r
t2 <- "11:15:10.12 PM"
parse_time(t2, "%I:%M:%OS %p")
```

```
## 23:15:10.12
```

