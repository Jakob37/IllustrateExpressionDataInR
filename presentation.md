Visualizing expression data in R
========================================================
author: Jakob Willforss
date: XX November 2019
autosize: true

The Tidyverse
========================================================

* Collection of R packages with common syntax
* Includes:
    - ggplot2
    - dplyr
    - readr
* Are often used in conjunction with base R, but do provide a more intuitive user case (opinions differ)
* Here: We will focus on ggplot2, and do minimal processing to get the data in shape for that
* test

Tidyverse vs. base R
========================================================

* Alternative solutions shown in exercises
* Reading tables
    * Base R: read.table
    * Tidyverse: read_tsv
* Extracting rows
    * df[df$target == "cond", ]
    * df %>% filter(target == "cond")
* Select columns
    * df[, c("col1", "col2")]
    * df %>% select(col1, col2)

Reading the design matrix
========================================================


```r
library(tidyverse)  # Importing the Tidyverse library
design_df <- read_tsv("spikein_data/input_design.tsv")
head(design_df)
```

```
# A tibble: 6 x 3
  sample group time 
  <chr>  <dbl> <chr>
1 dil9_5     9 late 
2 dil9_4     9 late 
3 dil8_1     8 late 
4 dil8_2     8 late 
5 dil8_3     8 late 
6 dil9_3     9 late 
```

Reading the expression matrix
========================================================


```r
data_df <- read_tsv("spikein_data/SpikeinWorkshop_nobatch_stats.tsv")
head(data_df, 2)
```

```
# A tibble: 2 x 28
  `Average RT` `Cluster ID` `Peptide Sequen… `External IDs` Charge
         <dbl>        <dbl> <chr>            <chr>           <dbl>
1         61.3      1.42e12 AAAAAAGDVFKDVQE… Q99Z57              3
2         99.0      1.42e12 AAADYLEVPLYTYLG… P69949              2
# … with 23 more variables: `Average m/z` <dbl>, P.Value <dbl>,
#   adj.P.Val <dbl>, log2Fold <dbl>, AvgExpr <dbl>, dil9_5 <dbl>,
#   dil9_4 <dbl>, dil8_1 <dbl>, dil8_2 <dbl>, dil8_3 <dbl>, dil9_3 <dbl>,
#   dil8_5 <dbl>, dil8_4 <dbl>, dil9_2 <dbl>, dil9_1 <dbl>, dil8_6 <dbl>,
#   dil9_6 <dbl>, dil8_7 <dbl>, dil9_7 <dbl>, dil8_8 <dbl>, dil9_8 <dbl>,
#   dil8_9 <dbl>, dil9_9 <dbl>
```

Explore the data frame further
========================================================


```r
str(data_df)
```

```
Classes 'spec_tbl_df', 'tbl_df', 'tbl' and 'data.frame':	4008 obs. of  28 variables:
 $ Average RT      : num  61.3 99 99 42.7 52.4 ...
 $ Cluster ID      : num  1.42e+12 1.42e+12 1.42e+12 1.42e+12 1.42e+12 ...
 $ Peptide Sequence: chr  "AAAAAAGDVFKDVQEAK" "AAADYLEVPLYTYLGGFNTK" "AAADYLEVPLYTYLGGFNTK" "AAAEALAQEQAAK" ...
 $ External IDs    : chr  "Q99Z57" "P69949" "P69949" "Q9A1Z8" ...
 $ Charge          : num  3 2 3 2 2 2 3 2 2 2 ...
 $ Average m/z     : num  555 1104 736 636 603 ...
 $ P.Value         : num  0.677 0.377 0.687 0.884 0.983 ...
 $ adj.P.Val       : num  1 1 1 1 1 ...
 $ log2Fold        : num  0.52036 -0.46058 0.28254 -0.08969 -0.00894 ...
 $ AvgExpr         : num  17.6 17.4 14.3 17.9 18.9 ...
 $ dil9_5          : num  NA NA NA 18.7 19.1 ...
 $ dil9_4          : num  18 NA NA 18.2 18.7 ...
 $ dil8_1          : num  NA NA NA 17.9 19 ...
 $ dil8_2          : num  NA NA NA 18.5 19 ...
 $ dil8_3          : num  18.7 NA NA 18.8 19.4 ...
 $ dil9_3          : num  NA NA NA 18.9 19.4 ...
 $ dil8_5          : num  16.6 NA NA 16 17.3 ...
 $ dil8_4          : num  17 NA NA 15.7 17.6 ...
 $ dil9_2          : num  NA NA NA 15 17.6 ...
 $ dil9_1          : num  NA NA NA 15.6 17.5 ...
 $ dil8_6          : num  NA 18.1 NA 19 20.1 ...
 $ dil9_6          : num  NA 17.7 14.8 18.7 19.4 ...
 $ dil8_7          : num  NA 17.7 14.4 18.7 19.6 ...
 $ dil9_7          : num  NA 16.6 13.7 18.1 19.1 ...
 $ dil8_8          : num  NA 17.1 13.3 18.2 19.2 ...
 $ dil9_8          : num  NA 17.7 14.8 18.8 19.8 ...
 $ dil8_9          : num  NA 17.6 14.8 18.4 19.4 ...
 $ dil9_9          : num  NA 16.6 NA 18.5 19.7 ...
 - attr(*, "spec")=
  .. cols(
  ..   `Average RT` = col_double(),
  ..   `Cluster ID` = col_number(),
  ..   `Peptide Sequence` = col_character(),
  ..   `External IDs` = col_character(),
  ..   Charge = col_double(),
  ..   `Average m/z` = col_double(),
  ..   P.Value = col_double(),
  ..   adj.P.Val = col_double(),
  ..   log2Fold = col_double(),
  ..   AvgExpr = col_double(),
  ..   dil9_5 = col_double(),
  ..   dil9_4 = col_double(),
  ..   dil8_1 = col_double(),
  ..   dil8_2 = col_double(),
  ..   dil8_3 = col_double(),
  ..   dil9_3 = col_double(),
  ..   dil8_5 = col_double(),
  ..   dil8_4 = col_double(),
  ..   dil9_2 = col_double(),
  ..   dil9_1 = col_double(),
  ..   dil8_6 = col_double(),
  ..   dil9_6 = col_double(),
  ..   dil8_7 = col_double(),
  ..   dil9_7 = col_double(),
  ..   dil8_8 = col_double(),
  ..   dil9_8 = col_double(),
  ..   dil8_9 = col_double(),
  ..   dil9_9 = col_double()
  .. )
```

Explore the data frame further
========================================================


```r
dim(data_df)
```

```
[1] 4008   28
```

```r
colnames(data_df)
```

```
 [1] "Average RT"       "Cluster ID"       "Peptide Sequence"
 [4] "External IDs"     "Charge"           "Average m/z"     
 [7] "P.Value"          "adj.P.Val"        "log2Fold"        
[10] "AvgExpr"          "dil9_5"           "dil9_4"          
[13] "dil8_1"           "dil8_2"           "dil8_3"          
[16] "dil9_3"           "dil8_5"           "dil8_4"          
[19] "dil9_2"           "dil9_1"           "dil8_6"          
[22] "dil9_6"           "dil8_7"           "dil9_7"          
[25] "dil8_8"           "dil9_8"           "dil8_9"          
[28] "dil9_9"          
```

ggplot2
========================================================

ggplot2 is a flexible framework used to produce a range of visualizations.

The basic structure:

```
ggplot(input_data, aes(aesthetics)) + geom_pattern()
```

* `input_data`: A data frame in long format
* `aes`: Aesthetics - linking columns to characteristics of interest
* `geoms`: Geometics - specify how to visualize the data through the aesthetics

Example:


```r
theme_set(theme_classic())  # Changes the default figure style
ggplot(data_df, aes(x=P.Value)) + geom_histogram(bins=100)
```

![plot of chunk unnamed-chunk-5](presentation-figure/unnamed-chunk-5-1.png)

Adjusting the histogram
========================================================

Showing average expression instead


```r
ggplot(data_df, aes(x=P.Value)) + geom_histogram(bins=100)
```

![plot of chunk unnamed-chunk-6](presentation-figure/unnamed-chunk-6-1.png)

```r
ggplot(data_df, aes(x=AvgExpr)) + geom_histogram(bins=30)
```

![plot of chunk unnamed-chunk-6](presentation-figure/unnamed-chunk-6-2.png)

Adjusting the histogram
========================================================

Highlighting entries with p < 0.05


```r
data_df$IsSig <- data_df$P.Value < 0.05
ggplot(data_df, aes(x=P.Value, fill=IsSig)) + geom_histogram(bins=100)
```

![plot of chunk unnamed-chunk-7](presentation-figure/unnamed-chunk-7-1.png)

```r
ggplot(data_df, aes(x=AvgExpr, fill=IsSig)) + geom_histogram(bins=100)
```

![plot of chunk unnamed-chunk-7](presentation-figure/unnamed-chunk-7-2.png)

One more example: Volcano plot
========================================================

A volcano plots illustrates fold in X-direction and p-value in Y-direction.
The p-value transformed to log10-scale for a nicer visual emphasis on low values.


```r
ggplot(data_df, aes(x=log2Fold, y=-log10(P.Value))) + geom_point()
```

![plot of chunk unnamed-chunk-8](presentation-figure/unnamed-chunk-8-1.png)

Making the volcano plot look more appealing
========================================================

A volcano plots illustrates fold in X-direction and p-value in Y-direction.
The p-value transformed to log10-scale for a nicer visual emphasis on low values.


```r
custom_colors <- c("#AAAAAA", "#0C81A2")
ggplot(data_df, aes(x=log2Fold, y=-log10(P.Value), color=IsSig)) + 
    geom_point(alpha=0.6, size=5) +   # Make dots partially transparent
    scale_color_manual(values=custom_colors) +  # Assign custom color scale
    ggtitle("My Volcano") +      # Set title
    xlab("Fold (log2)") +     # Set x label
    ylab("-log10(p-value)")   # Set y label
```

![plot of chunk unnamed-chunk-9](presentation-figure/unnamed-chunk-9-1.png)

Same approach, to an MA plot
========================================================

An MA plot is similar to the volcano plot, but emphasises average expression
rather than significance.


```r
custom_colors <- c("#AAAAAA", "#0C81A2")
ggplot(data_df, aes(x=AvgExpr, y=log2Fold, color=IsSig)) + 
    geom_point(alpha=0.6, size=5) +   # Make dots partially transparent
    scale_color_manual(values=custom_colors) +  # Assign custom color scale
    ggtitle("My MA") +      # Set title
    xlab("Expression") +     # Set x label
    ylab("Log2-fold")   # Set y label
```

![plot of chunk unnamed-chunk-10](presentation-figure/unnamed-chunk-10-1.png)

Exercises part 1
========================================================

Do the section one in the hands-on exercises!

Total intensities in samples
========================================================

Wide-format vs. long format
========================================================

Illustrate and transform between the formats

Density plot
========================================================

Boxplots and violins
========================================================

Heatmap
========================================================

Exercises part 2: Working with long format
========================================================

Do the section one in the hands-on exercises!

Exercises part 3: Special cases
========================================================

Do the section one in the hands-on exercises!

Writing figures to file
========================================================

Combining multiple figures
========================================================

PCA plot
========================================================

Survival curve
========================================================





