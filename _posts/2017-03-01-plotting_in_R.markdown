---
layout: post
title: Week 8- intro to data exploration and analysis in R
date:   2017-03-01
author: Gaurav Kandlikar
---

#Revisiting the Cocoli Dataset

Last week we began to explore the forest dynamics data from Cocoli,
Panama. Today, we will explore how we can use R and the package
`ggplot2` to visualize some of the dynamics of the forest. We begin by
importing in the dataset and exploring its structure (remember to
**change your file path as required**!):

    cocoli_dat <- read.table("~/Desktop/eeb-177/class-assignments/21Feb/cocoli.txt", header = TRUE)
    # we can look at the structure with str()
    str(cocoli_dat)

    ## 'data.frame':    9466 obs. of  22 variables:
    ##  $ tag   : Factor w/ 9466 levels "-47493","000001",..: 2 3 4 5 6 7 8 9 10 11 ...
    ##  $ spcode: Factor w/ 177 levels "*","ACACME","ACALDI",..: 131 49 67 131 48 131 129 154 139 49 ...
    ##  $ x     : num  3 0.1 1.3 2.2 3.5 4.3 3.7 4.1 1.5 0.9 ...
    ##  $ y     : num  0.9 0.6 2.3 3.4 3.7 4.7 7 6.3 11 14 ...
    ##  $ dbh1  : int  171 13 26 10 14 12 15 78 29 28 ...
    ##  $ dbh2  : int  267 14 33 17 15 26 19 78 30 29 ...
    ##  $ dbh3  : int  277 17 39 19 15 25 19 78 29 33 ...
    ##  $ recr1 : Factor w/ 3 levels "*","A","P": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ recr2 : Factor w/ 5 levels "*","A","B","D",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ recr3 : Factor w/ 4 levels "*","A","B","D": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ pom1  : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ pom2  : int  2 1 2 1 1 2 1 1 1 1 ...
    ##  $ pom3  : int  2 1 2 1 1 2 1 1 1 1 ...
    ##  $ code1 : Factor w/ 8 levels "*","B","L","M",..: 1 1 4 1 5 1 4 5 4 1 ...
    ##  $ code2 : Factor w/ 25 levels "*","B","BL","BQ",..: 1 1 14 1 15 1 14 15 14 1 ...
    ##  $ code3 : Factor w/ 19 levels "*","B","BM","D",..: 1 1 13 1 13 13 13 13 13 1 ...
    ##  $ mult1 : int  1 1 2 1 2 1 2 2 2 1 ...
    ##  $ mult2 : int  1 1 2 1 2 1 2 2 2 1 ...
    ##  $ mult3 : int  1 1 2 1 2 2 2 2 2 1 ...
    ##  $ date1 : Factor w/ 23 levels "11/02/1994","11/04/1994",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ date2 : Factor w/ 18 levels "11/10/1997","11/11/1997",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ date3 : Factor w/ 30 levels "11/04/1998","11/23/1998",..: 2 2 2 2 2 2 2 2 2 2 ...

    # we can inspect the dimensions of a dataframe with dim()
    dim(cocoli_dat)

    ## [1] 9466   22

    # and we can peek at the first few lines with head()
    head(cocoli_dat)

    ##      tag spcode   x   y dbh1 dbh2 dbh3 recr1 recr2 recr3 pom1 pom2 pom3
    ## 1 000001 PROTTE 3.0 0.9  171  267  277     A     A     A    1    2    2
    ## 2 000002 COCCPA 0.1 0.6   13   14   17     A     A     A    1    1    1
    ## 3 000003 EUGEPR 1.3 2.3   26   33   39     A     A     A    1    2    2
    ## 4 000004 PROTTE 2.2 3.4   10   17   19     A     A     A    1    1    1
    ## 5 000005 CLAVME 3.5 3.7   14   15   15     A     A     A    1    1    1
    ## 6 000006 PROTTE 4.3 4.7   12   26   25     A     A     A    1    2    2
    ##   code1 code2 code3 mult1 mult2 mult3      date1      date2      date3
    ## 1     *     *     *     1     1     1 11/02/1994 11/11/1997 11/23/1998
    ## 2     *     *     *     1     1     1 11/02/1994 11/11/1997 11/23/1998
    ## 3     M     M     M     2     2     2 11/02/1994 11/11/1997 11/23/1998
    ## 4     *     *     *     1     1     1 11/02/1994 11/11/1997 11/23/1998
    ## 5    ML    ML     M     2     2     2 11/02/1994 11/11/1997 11/23/1998
    ## 6     *     *     M     1     1     2 11/02/1994 11/11/1997 11/23/1998

### Tree sizes in 1994

Our first goal with this dataframe last week was to make a histogram
that showed the distribution of plant sizes at the first census in 1994.
We can begin to build such a plot in `R` by extracting the relavant
column of data and plotting a histogram. In this section we explore how
to extract data from dataframes and how to name and index vectors in
`R`.

    # use the "data_frame$col_name" notation to extract the DBH1 column
    # save the column as a new vector called sizes_in_1994:
    sizes_in_1994 <- cocoli_dat$dbh1

    # use the cocoli_dat$tag column to name all of the values in the vector
    # that we just created
    names(sizes_in_1994) <- cocoli_dat$tag

    # view the first few elements of the vector
    # NOTE: in R, vectors begin at element 1-
    # not element 0, as in pythong

    sizes_in_1994[1:10]

    ## 000001 000002 000003 000004 000005 000006 000007 000008 000009 000010 
    ##    171     13     26     10     14     12     15     78     29     28

    # we can also subset vectors by the name of an element
    sizes_in_1994["000001"] # subset a vector by name

    ## 000001 
    ##    171

    # Let us now also extract the tree sizes in 1997 and 1998
    sizes_in_1997 <- cocoli_dat$dbh2
    names(sizes_in_1997) <- cocoli_dat$tag

    sizes_in_1998 <- cocoli_dat$dbh3
    names(sizes_in_1998) <- cocoli_dat$tag

### Summary statistics and intro to plotting with base R

It is often useful to generate basic summary statistics on any data
before you begin an analysis. We can start to develop a sense for our
data with the `R` function `summary()`, which returns the minimum,
maximum, mean, and quantiles of a vector:

    summary(sizes_in_1994)

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   -9.00   13.00   21.00   54.36   42.00 1992.00

Performing mathematical operations on vectors of data is very
straightforward in R. One of our goals with the Cocoli dataset was to
calculate the yearly relative growth rate of each individual across the
first census. Recall that the RGR is calculated as follows:

$RGR = \\frac{DBH\_{t+1} - DBH\_t}{DBH\_t}$

To conver this to estimate to RGR to a yearly measure, we need to then
divide the calculated RGR by the number of years- here, three years
passed between the first two censuses.

    ## Calculate RGR between 1997-1994
    # (size in 1997 - size in 1994)/size in 1994

    yearly_RGR = ((cocoli_dat$dbh2-cocoli_dat$dbh1)/cocoli_dat$dbh1)/3

    # print out the first 10 values of RGR
    yearly_RGR[1:10]

    ##  [1] 0.18713450 0.02564103 0.08974359 0.23333333 0.02380952 0.38888889
    ##  [7] 0.08888889 0.00000000 0.01149425 0.01190476

Now that we have a `yearly_RGR` vector, we can add it to the end of our
`cocoli_dat` dataframe:

    # add the RGR column
    cocoli_dat$rgr1 = yearly_RGR

    # note that we now have 23 columns - the 23rd column is the calculated RGR
    dim(cocoli_dat)

    ## [1] 9466   23

We can quickly make a histogram from our data- but the default histogram
is a little ugly and will take a lot of work to improve:

    hist(cocoli_dat$dbh1, xlab = "DBH in 1994 (mm)", main = "Distributions of sizes in 1994")

![]({{ site.url }}/images/ex8_files/figure-markdown_strict/unnamed-chunk-6-1.png) For example,
we had said that logging the y-axis would be beneficial in this instance
because we can't get any meaningful information about the biggest tree
sizes from the default histogram- however, logging the Y-axis of a
histogram with base-R can get quite tedious:

    hist_data <- hist(cocoli_dat$dbh1, plot = FALSE)
    # the object hist_data has a vector called "counts" that represents the height of the bar
    hist_data$counts

    ##  [1] 1283 7108  558  194  120   55   26   33   23   22   13   11    5    4
    ## [15]    6    1    3    0    0    0    1

    # NOTE: there are several areas of the histogram with height zero
    # we can't take a log of 0...
    # so let's take a log10 of counts+1
    hist_data$counts <- log10(hist_data$counts+1)

    # plot the histogram, with a modified y-axis label
    plot(hist_data, ylab = "log10(Frequency)")

![]({{ site.url }}/images/ex8_files/figure-markdown_strict/unnamed-chunk-7-1.png)

That was a lot of effort just to log the Y-axis. Luckily, the `R`
package `ggplot2` can come to the rescue. ggplot2 is a fantastic
plotting package developed by Hadley Wickham, who has developed a number
of super valuable utility packages for R. If you continue to use R for
your work, you will find it helpful to follow his contributions.

### A histogram with ggplot2

You will be exposed to many parts of ggplot2 in an exercise towards the
end of this document. For now, let's see how to make a histogram with a
logged y-axis using ggplot2:

    # load in the library
    library(ggplot2)

    # one line of code for a histogram with logged y axis!
    ggplot(data = cocoli_dat) + geom_histogram(aes(dbh1)) + scale_y_log10()

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 6 rows containing missing values (geom_bar).

![]({{ site.url }}/images/ex8_files/figure-markdown_strict/unnamed-chunk-8-1.png)

### Optional: introduction to dplyr

Another flexible and valuable library developed by Hadley Wickham is
`dplyr`, which you will find useful if you are ever subsetting,
modifying, or rearranging data frames. In this section I introduce how
to use dplyr to quickly move things around in our dataframe. If you plan
to use this library for your own work, I recommend that you read []().

#### Goal 1: order the dataset by descending size in 1994

    # load in the library
    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    # you will often see the following notation in dplyr:
    # ____ %>% _____
    # The '%>%' structure in R is identical to pipes (|) in the shell.
    # The item to the left of the pipe is passed as the input to the command to  the right.

    # the following line uses dplyr functions to show us a rearranged
    # data frame:

    # note: the negative sign within arrange (i.e. arrange(-dbh1)) 
    # arranges in descending order; the default is for ascending order
    cocoli_dat %>% arrange(-dbh1)

    ##         tag spcode     x     y dbh1 dbh2 dbh3 recr1 recr2 recr3 pom1 pom2
    ## 1    001563 CAVAPL 105.3  46.4 1992 1640 1650     A     A     A    1    1
    ## 2    009158 FICUCI  64.5  44.6 1597 1635 1637     A     A     A    1    1
    ## 3    007512 CAVAPL  97.1  75.1 1580 1576 1573     A     A     A    1    1
    ## 4    003264 CAVAPL  33.2  51.7 1520 1574 1604     A     A     A    1    1
    ## 5    000098 CAVAPL  16.1   7.4 1500 1515 1517     A     A     A    1    2
    ## 6    003423 CAVAPL  31.5  92.0 1390 1437 1434     A     A     A    1    1
 
    ##      pom3 code1 code2 code3 mult1 mult2 mult3      date1      date2
    ## 1       1     B     B     B     1     1     1 11/23/1994 11/18/1997
    ## 2       1     B     B     B     1     1     1 11/08/1994 11/12/1997
    ## 3       1     B     B     B     1     1     1 11/18/1994 11/12/1997
    ## 4       1     B     B     B     1     1     1 11/07/1994 11/14/1997
    ## 5       2     B     B     B     1     1     1 11/02/1994 11/11/1997
    ## 6       1     B     B     B     1     1     1 11/08/1994 11/17/1997
    ##           date3          rgr1
    ## 1    12/22/1998 -5.890228e-02
    ## 2    12/02/1998  7.931538e-03
    ## 3    12/04/1998 -8.438819e-04
    ## 4    12/25/1998  1.184211e-02
    ## 5    11/23/1998  3.333333e-03
    ## 6    12/27/1998  1.127098e-02

#### Goal 2: Make a new dataframe that shows the total DBH occupied by each species in 1994

Our goal here is to group `cocoli_dat` by the species code, and for each
species, determine how much area it's trees occupied in 1994. For
example, if a species was represented by two individuals with DBHs 50mm
and 100mm, we could calculate its **basal area** as
*π* \* *r*<sup>2</sup>, or
*π* \* (50/2)<sup>2</sup> + *π* \* (100/2)<sup>2</sup> (the total area
would be 9817.4770425):

    # we use the dplyr functions group_by(), select(), summarize(), and arrange()

    cocoli_dat %>% group_by(spcode) %>% 
      summarize(total_1994_area = sum(pi*(dbh1/2)^2)) %>%
      arrange(-total_1994_area)

    ## # A tibble: 177 × 2
    ##    spcode total_1994_area
    ##    <fctr>           <dbl>
    ## 1  ANACEX        41958831
    ## 2  CAVAPL        30138191
    ## 3  CAL2CA         6238677
    ## 4  FICUIN         5233467
    ## 5  TRI2PL         5153060
    ## 6  PSE1SE         5129527
    ## 7  ANTITR         3462317
    ## 8  SCH1ZO         3446711
    ## 9  SPONMO         2817339
    ## 10 ELAEOL         2271054
    ## # ... with 167 more rows

Note that if you read the pipe operator `%>%` as the English word
"then", the `dplyr` code reads almost like a sentence:

    cocoli_dat %>% group_by(spcode) %>% 
      summarize(total_1994_area = sum(pi*(dbh1/2)^2)) %>%
      arrange(-total_1994_area)

    Take the cocoli dat, then group by the species code, then summarize for each species the basal area using the formula above, then arrange the area in descending order.

There's a lot more you can do with dplyr- I refer you to [this
tutorial](https://genomicsclass.github.io/book/pages/dyplr_tutorial.html)
for a thorough guide.

Homework
--------

1.  Create a new Rmarkdown file within your `exercise-8` directory.
2.  In this Rmarkdown file, complete the `ggplot2` exercises at [this
    link](http://tutorials.iq.harvard.edu/R/Rgraphics/Rgraphics.html).
3.  NOTE: the exercise above will require you to install a few extra
    packages and download a dataset.
4.  Knit your RMarkdown file into a `markdown` file and push to github.
5.  NOTE: we should be able to see your code AND YOUR OUTPUT GRAPHS in
    your markdown file on github.
6.  HINT: you will need to upload both a `.md` file and a directory that
    includes the images.
7.  HINT: to make a `.md` file output, make sure that the following line
    is included in your YAML header: `output: md_document`.
