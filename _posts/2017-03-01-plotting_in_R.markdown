Revisiting the Cocoli Dataset
-----------------------------

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
    ## 7    000132 CAVAPL   2.6  33.6 1363 1355 1361     A     A     A    1    1
    ## 8    004158 FICUIN  31.5 240.9 1360 1416 1414     A     A     A    1    1
    ## 9    007607 CAVAPL  87.4 117.1 1354 1423 1455     A     A     A    1    1
    ## 10   000074 CAVAPL  10.8  17.9 1332 1375 1361     A     A     A    1    1
    ## 11   041958 CAVAPL   0.1 164.6 1317 1363 1339     A     A     A    1    1
    ## 12   003605 ANACEX  37.7 130.6 1262 1305 1262     A     A     A    1    1
    ## 13   010399 CAVAPL 128.0  69.4 1260 1322 1412     A     A     A    1    2
    ## 14   010995 CAVAPL 170.5  34.5 1218 1263 1360     A     A     A    1    1
    ## 15   002288 CAVAPL 151.6  70.9 1205 1302 1255     A     A     A    1    1
    ## 16   008468 ANACEX  93.8 295.5 1191 1210 1223     A     A     A    1    1
    ## 17   003792 CAVAPL  37.8 160.5 1150 1178 1194     A     A     A    1    1
    ## 18   000817 CAVAPL  18.0 155.6 1130 1187 1180     A     A     A    1    1
    ## 19   011142 FICUIN 177.2  62.4 1107 1111 1138     A     A     A    1    1
    ## 20   006402 FICUIN  57.8  99.0 1103 1211 1114     A     A     A    1    1
    ## 21   010009 PSE1SE  60.5 260.5 1085 1140 1137     A     A     A    1    1
    ## 22   011340 ANACEX 189.3  99.4 1078 1193   -9     A     A     *    1    2
    ## 23   002205 CAVAPL 155.9  59.1 1064 1065 1127     A     A     A    1    1
    ## 24   010968 CAVAPL 163.5  39.0 1064 1169 1194     A     A     A    1    1
    ## 25   002196 PSE1SE 151.2  58.4 1052 1070 1163     A     A     A    1    1
    ## 26   011336 CAVAPL 180.9  87.5 1045 1081   -9     A     A     *    1    2
    ## 27   006983 CAVAPL  44.1 228.2 1041 1110 1131     A     A     A    1    1
    ## 28   008516 ANACEX 189.2  13.5 1031 1040 1055     A     A     A    1    1
    ## 29   009610 ANACEX  78.6 176.0 1030   -1   -1     A     D     D    1    0
    ## 30   006628 ANACEX  50.5 154.4 1013 1058 1015     A     A     A    1    1
    ## 31   010397 PSE1SE 126.8  68.9 1008   -9 1052     A     *     A    1    0
    ## 32   007068 ANACEX  47.6 259.9  980 1020 1030     A     A     A    1    1
    ## 33   000943 ANACEX   5.3 195.7  977 1040 1039     A     A     A    1    1
    ## 34   009346 PSE1SE  71.2  90.5  971  991  995     A     A     A    1    1
    ## 35   007646 ANACEX  83.6 125.5  965  970 1006     A     A     A    1    1
    ## 36   009375 FICUIN  64.0 109.1  965  971  977     A     A     A    1    1
    ## 37   009399 ANACEX  76.2 101.1  950  952  960     A     A     A    1    1
    ## 38   006745 ANACEX  57.5 166.2  942  971  975     A     A     A    1    1
    ## 39   003060 PSE1SE  31.8  14.8  928  959  958     A     A     A    1    1
    ## 40   008313 CAVAPL  98.8 255.5  923  966  995     A     A     A    1    1
    ## 41   000790 ANACEX   9.3 145.5  913  945  948     A     A     A    1    1
    ## 42   000219 CAVAPL   1.8  48.4  910  957  982     A     A     A    1    1
    ## 43   006528 ANACEX  52.5 121.6  907  931  935     A     A     A    1    1
    ## 44   003187 CAL2CA  34.7  38.0  903   -9  910     A     *     A    1    0
    ## 45   006093 CAVAPL  59.1   5.1  887  947  978     A     A     A    1    1
    ## 46   003348 FICUIN  32.0  76.9  885  901  893     A     A     A    1    1
    ## 47   008458 ANACEX  87.2 287.6  874  890  892     A     A     A    1    1
    ## 48   001087 ANACEX   4.4 224.0  871  917  923     A     A     A    1    1
    ## 49   000993 ANACEX  18.5 183.5  863  865  886     A     A     A    1    1
    ## 50   003759 ANACEX  34.2 169.3  862  874  873     A     A     A    1    1
    ## 51   009261 CAVAPL  69.3  77.4  860  892  900     A     A     A    1    1
    ## 52   000868 CAVAPL   9.6 168.0  858  879  880     A     A     A    1    1
    ## 53   002094 ANACEX 147.3  23.5  857  915  920     A     A     A    1    1
    ## 54   010899 ANACEX 169.5  15.3  857  864  930     A     A     A    1    1
    ## 55   009969 ANACEX  70.2 250.5  844  871  875     A     A     A    1    1
    ## 56   008177 ANACEX  94.0 236.0  843  889  890     A     A     A    1    1
    ## 57   011234 CAVAPL 198.6  58.2  839  875  898     A     A     A    1    1
    ## 58   009979 ANACEX  79.4 255.9  838  845  850     A     A     A    1    1
    ## 59   010271 ANACEX 121.7  42.0  830  835  840     A     A     A    1    1
    ## 60   001155 ANACEX  16.8 238.3  820  861  864     A     A     A    1    2
    ## 61   002306 ANACEX 158.9  61.4  818  799  799     A     A     A    1    2
    ## 62   010453 ANACEX 124.3  85.3  817  826  847     A     A     A    1    1
    ## 63   000750 ANACEX  13.7 137.5  816  849  831     A     A     A    1    1
    ## 64   003575 FICUIN  27.0 121.3  810  810  815     A     A     A    1    1
    ## 65   009697 ANACEX  72.6 192.5  808  831  824     A     A     A    1    1
    ## 66   011085 ANACEX 160.6  70.4  808  845  841     A     A     A    1    1
    ## 67   004031 ANACEX  35.3 215.5  796  815  710     A     A     A    1    2
    ## 68   009963 ANACEX  69.2 243.6  795  804  805     A     A     A    1    1
    ## 69   007552 ANACEX  88.1  81.6  794  796  794     A     A     A    1    1
    ## 70   000485 ANACEX   9.8  81.8  790  800  803     A     A     A    1    1
    ## 71   003086 PSE1SE  38.5   1.3  785  831  802     A     A     A    1    1
    ## 72   008089 ANACEX  90.4 204.4  778  805  779     A     A     A    1    2
    ## 73   011186 STERAP 176.3  98.6  778  788  802     A     A     A    1    1
    ## 74   009809 ANACEX  72.5 219.6  775  786  800     A     A     A    1    1
    ## 75   006376 ANACEX  40.7  98.8  772  793  814     A     A     A    1    1
    ## 76   003250 ANACEX  29.6  42.2  766   -9  776     A     *     A    1    0
    ## 77   009336 CAL2CA  70.4  83.3  764  801  770     A     A     A    1    2
    ## 78   006865 ANACEX  41.6 203.7  761  693  690     A     A     A    1    1
    ## 79   006465 ANACEX  56.3 106.2  760  766  769     A     A     A    1    1
    ## 80   002023 ANACEX 145.4   7.6  759  810  821     A     A     A    1    2
    ## 81   006406 ANACEX  55.5  93.6  758  761  758     A     A     A    1    1
    ## 82   001564 PSE1SE 106.9  47.7  743  822  839     A     A     A    1    1
    ## 83   007747 SPONMO  81.3 159.9  738  738  725     A     A     A    1    1
    ## 84   004211 ANACEX  24.4 272.9  732  736  736     A     A     A    1    1
    ## 85   001253 TERMAM   1.4 279.4  730  787  778     A     A     A    1    1
    ## 86   010880 ANACEX 164.2   5.8  716  780  822     A     A     A    1    1
    ## 87   008318 ANACEX  98.2 259.3  713  712  715     A     A     A    1    1
    ## 88   007196 ANACEX  44.7 289.8  711  712  710     A     A     A    1    1
    ## 89   004073 ANACEX  22.4 225.7  705  735  750     A     A     A    1    1
    ## 90   000457 SPONMO   7.9  98.9  699  706  714     A     A     A    1    2
    ## 91   010389 ANACEX 129.7  79.8  696  722  721     A     A     A    1    1
    ## 92   011285 ANACEX 189.4  60.0  692  720  721     A     A     A    1    2
    ## 93   009133 CHR2CA  72.3  39.7  691   -9  680     A     *     A    1    0
    ## 94   000857 ANACEX   6.8 173.8  690  582  580     A     A     A    1    1
    ## 95   003959 ANACEX  20.4 207.5  688  702  695     A     A     A    1    1
    ## 96   000776 ANACEX   1.3 154.6  682  697  709     A     A     A    1    1
    ## 97   001010 ANACEX   1.1 218.9  678  690  700     A     A     A    1    1
    ## 98   003650 ANACEX  25.5 157.2  668  680  674     A     A     A    1    1
    ## 99   007870 CAL2CA  90.4 168.7  668  660  665     A     A     A    1    1
    ## 100  009248 ANACEX  60.4  65.6  665  667  668     A     A     A    1    1
    ## 101  010928 ANACEX 174.6   9.7  656  715  702     A     A     A    1    1
    ## 102  009714 ANACEX  75.3 194.0  655  667  674     A     A     A    1    1
    ## 103  002054 ANACEX 156.8   0.3  654  655  656     A     A     A    1    1
    ## 104  008157 ANACEX  90.7 222.2  654  660  662     A     A     A    1    1
    ## 105  000949 ANACEX   6.7 191.0  653  691  694     A     A     A    1    1
    ## 106  001731 CAVAPL 100.7  90.5  648  653  654     A     A     A    1    1
    ## 107  009897 ANACEX  73.6 231.1  646  675  687     A     A     A    1    1
    ## 108  004335 SPONMO  35.3 292.6  643  660  655     A     A     A    1    1
    ## 109  004313 ANACEX  32.8 280.4  636  964  646     A     A     A    1    1
    ## 110  003201 ANACEX  36.9  21.0  629   -9  655     A     *     A    1    0
    ## 111  010004 ANACEX  76.5 240.6  626  688  677     A     A     A    1    1
    ## 112  007305 CAVAPL  98.7   3.8  623  645  650     A     A     A    1    1
    ## 113  006445 ANACEX  50.3 100.4  622  661  661     A     A     A    1    1
    ## 114  011021 CEDROD 162.7  48.7  622  677  643     A     A     A    1    1
    ## 115  003729 ANACEX  21.4 174.2  615  625  621     A     A     A    1    1
    ## 116  009746 ANACEX  64.0 209.4  615  637  644     A     A     A    1    1
    ## 117  011387 INGAFA  64.8 204.8  615   -1   -1     A     D     D    1    0
    ## 118  007253 ANACEX  58.7 285.9  614  615  613     A     A     A    1    1
    ## 119  007814 CAVAPL  97.7 149.3  610  667  671     A     A     A    1    1
    ## 120  011236 ANACEX 197.6  50.2  610  620  622     A     A     A    1    1
    ## 121  008597 OCHRPY 193.5  31.6  608  655  628     A     A     A    1    1
    ## 122  010024 ANACEX  66.9 266.1  602  630  627     A     A     A    1    1
    ## 123  006045 ANACEX  47.4   1.7  600  648  646     A     A     A    1    1
    ## 124  008237 ANACEX  80.6 253.7  597  607  620     A     A     A    1    1
    ## 125  009677 AST2GR  67.2 183.1  596  622  626     A     A     A    1    2
    ## 126  006203 ANACEX  44.3  56.2  592  602  601     A     A     A    1    1
    ## 127  009628 SPONMO  60.4 180.5  592  595   -9     A     A     *    1    1
    ## 128  008532 ANACEX 192.1  15.0  589  639  639     A     A     A    1    1
    ## 129  000726 TET4JO   6.4 128.2  585  590  660     A     A     A    1    1
    ## 130  000923 CEDROD  19.3 167.7  582  570  585     A     A     A    1    2
    ## 131  003530 ANACEX  35.5 103.7  580  585  575     A     A     A    1    1
    ## 132  003018 ANACEX  23.2  10.5  565  590  590     A     A     A    1    1
    ## 133  009074 SWIEMA  77.8  10.6  562  619  618     A     A     A    1    1
    ## 134  001264 AST2GR   6.9 269.5  553  590  601     A     A     A    1    1
    ## 135  010108 ANACEX  68.5 285.6  552  617  633     A     A     A    1    1
    ## 136  008360 ANACEX  83.1 272.4  544  571  575     A     A     A    1    1
    ## 137  010438 BURSSI 139.0  67.9  544  555  638     A     A     A    1    1
    ## 138  006993 ANACEX  46.1 232.9  541  560  560     A     A     A    1    1
    ## 139  001659 POCHQU 109.1  66.4  539  610  485     A     A     A    1    2
    ## 140  004007 ANACEX  34.7 211.2  537  555  536     A     A     A    1    1
    ## 141  003723 CAVAPL  22.4 169.4  531  531  540     A     A     A    1    1
    ## 142  004150 BURSSI  28.7 245.4  523  525  555     A     A     A    1    1
    ## 143  008297 ANACEX  94.1 247.2  523  523  518     A     A     A    1    1
    ## 144  011381 ANACEX 195.8  81.5  521  525  530     A     A     A    1    1
    ## 145  003114 TRI2HI  25.2  39.9  520  531  529     A     A     A    1    1
    ## 146  009686 ZUELGU  72.9 189.7  510  512  518     A     A     A    1    1
    ## 147  001300 SPONMO  19.6 278.5  506  519  522     A     A     A    1    1
    ## 148  009492 SPONMO  62.1 159.3  506  521  521     A     A     A    1    1
    ## 149  010414 SPONMO 130.1  65.1  500  503  503     A     A     A    1    1
    ## 150  007902 ANTITR  95.8 162.4  492  494  493     A     A     A    1    1
    ## 151  009309 CAL2CA  77.4  65.3  491   -9  495     A     *     A    1    0
    ## 152  010286 BURSSI 129.6  56.0  488  510  517     A     A     A    1    1
    ## 153  011167 SPONMO 169.5  84.6  488  490  504     A     A     A    1    1
    ## 154  006274 SPONMO  44.1  61.6  487  490  493     A     A     A    1    1
    ## 155  008528 ANACEX 190.9   0.0  487  488  494     A     A     A    1    1
    ## 156  011017 CAL2CA 162.1  44.1  486  486  515     A     A     A    1    1
    ## 157  000039 TRI2HI  14.5   0.9  485  490  491     A     A     A    1    1
    ## 158  001748 SPONMO 105.3  89.2  482  502  501     A     A     A    1    2
    ## 159  007804 TRI2HI  96.2 159.1  481  480  480     A     A     A    1    1
    ## 160  002326 ANACEX 144.3  96.1  479  481  479     A     A     A    1    1
    ## 161  006187 ANDIIN  42.3  49.3  479  632  498     A     A     A    1    2
    ## 162  006229 ANACEX  51.6  45.8  477  479  477     A     A     A    1    1
    ## 163  010245 ANACEX 130.1  25.3  475  513  500     A     A     A    1    1
    ## 164  009354 AST2GR  78.7  91.7  474  480  482     A     A     A    1    1
    ## 165  004250 CEDROD  31.7 275.6  468  482  485     A     A     A    1    1
    ## 166  006673 ANACEX  43.0 167.6  463  487  488     A     A     A    1    1
    ## 167  011348 ANTITR 187.4  84.7  461  466  446     A     A     A    1    1
    ## 168  006397 ANACEX  51.4  91.3  455  470  470     A     A     A    1    1
    ## 169  000966 ANACEX  13.4 183.3  453  455  443     A     A     A    1    1
    ## 170  002104 SCH1ZO 151.5  31.4  448   -1   -1     A     D     D    1    0
    ## 171  006949 CAL2CA  58.2 215.4  448  451  455     A     A     A    1    1
    ## 172  011148 ANTITR 162.9  90.4  445  453  471     A     A     A    1    1
    ## 173  003805 ANACEX  24.7 185.3  438  448  451     A     A     A    1    1
    ## 174  006989 ANACEX  46.3 239.6  437  479  459     A     A     A    1    1
    ## 175  003625 DALBRE  22.4 144.8  436  436  438     A     A     A    1    1
    ## 176  006860 CAL2CA  55.9 188.4  436  445  445     A     A     A    1    1
    ## 177  002296 ANACEX 154.4  79.1  433  439  439     A     A     A    1    2
    ## 178  003719 ANACEX  22.5 165.6  432  432  431     A     A     A    1    1
    ## 179  008216 ANACEX  96.7 221.8  431  436  445     A     A     A    1    1
    ## 180  009421 CAL2CA  66.7 131.4  431  452  452     A     A     A    1    1
    ## 181  000986 ANACEX  16.3 189.3  430  450  454     A     A     A    1    1
    ## 182  003580 ANACEX  31.4 122.5  430  438  448     A     A     A    1    1
    ## 183  011131 SCH1ZO 170.5  75.7  430   -1   -1     A     D     D    1    0
    ## 184  000573 ANTITR   0.8 102.8  425  432  431     A     A     A    1    1
    ## 185  007892 BURSSI  97.5 173.2  425  428  424     A     A     A    1    1
    ## 186  009233 STERAP  78.0  49.1  425  440  388     A     A     A    1    1
    ## 187  001107 SCH1ZO   5.8 237.9  424  422  417     A     A     A    1    1
    ## 188  006751 BURSSI  40.6 180.4  423  439  439     A     A     A    1    1
    ## 189  006942 ANACEX  52.6 215.5  421  422  421     A     A     A    1    1
    ## 190  007423 TRI2HI  87.6  40.3  421  427  435     A     A     A    1    1
    ## 191  004179 SCH1ZO  39.5 255.5  420  420  422     A     A     A    1    1
    ## 192  009179 TRI2HI  65.3  53.6  420  431  423     A     A     A    1    1
    ## 193  007898 ZUELGU  96.9 168.0  418  425  425     A     A     A    1    1
    ## 194  008368 ANACEX  88.1 273.3  417  421  418     A     A     A    1    1
    ## 195  000815 ANACEX  11.8 159.3  415  418  417     A     A     A    1    1
    ## 196  003672 ANTITR  30.3 140.3  415  420  414     A     A     A    1    1
    ## 197  008638 CAL2CA 136.5  69.9  415  431  440     A     A     A    1    1
    ## 198  009436 ANACEX  70.3 128.8  413  413  415     A     A     A    1    1
    ## 199  010997 AST2GR 173.9  32.7  412  418  443     A     A     A    1    1
    ## 200  011287 SCH1ZO 186.0  60.1  412  420  421     A     A     A    1    1
    ## 201  006082 SCH1ZO  58.2  12.8  407  422  392     A     A     A    1    1
    ## 202  010998 BROSAL 174.6  32.9  405  428  410     A     A     A    1    1
    ## 203  006156 SCH1ZO  54.4  27.1  401  401  401     A     A     A    1    1
    ## 204  002338 ELAEOL 149.3  87.2  400  403  380     A     A     A    1    1
    ## 205  001461 SCH1ZO 119.5  11.5  399  403  401     A     A     A    1    1
    ## 206  006125 SCH1ZO  46.8  38.9  399  399  398     A     A     A    1    1
    ## 207  000895 JAC1CA  12.5 177.1  398  649  649     A     A     A    1    2
    ## 208  006308 SCH1ZO  49.3  70.1  397  393  388     A     A     A    1    1
    ## 209  011203 SCH1ZO 188.2  45.3  397  400  400     A     A     A    1    1
    ## 210  009873 CAL2CA  68.9 235.9  394  399  403     A     A     A    1    1
    ## 211  011282 ANTITR 186.2  66.7  394  394   -1     A     A     D    1    1
    ## 212  011069 SCH1ZO 179.9  49.8  390  404  399     A     A     A    1    1
    ## 213  002096 ANTITR 148.6  24.6  388  390   -1     A     A     D    1    2
    ## 214  011065 SCH1ZO 177.6  50.6  386  364  377     A     A     A    1    1
    ## 215  010515 CAL2CA 135.5  94.2  385  385  395     A     A     A    1    1
    ## 216  006823 BURSSI  53.2 180.1  383  372   -1     A     A     D    1    1
    ## 217  003343 ANDIIN  33.7  68.9  379  388  384     A     A     A    1    1
    ## 218  010249 SCH1ZO 132.3  32.3  377  381  371     A     A     A    1    1
    ## 219  002103 SAP1SA 153.8  26.7  375  405  415     A     A     A    1    1
    ## 220  009014 ANTITR  64.3  13.8  374  364   -1     A     A     D    1    1
    ## 221  009491 CAL2CA  61.4 159.4  372  381  382     A     A     A    1    2
    ## 222  010965 SCH1ZO 161.3  35.8  371  371  373     A     A     A    1    1
    ## 223  006958 POCHSE  56.3 206.9  369  365  367     A     A     A    1    1
    ## 224  008396 CAL2CA  90.7 266.6  368  369  365     A     A     A    1    1
    ## 225  002150 CAL2CA 143.6  47.5  365  371  372     A     A     A    1    1
    ## 226  001479 DALBRE 117.1   4.6  364  396  377     A     A     A    1    2
    ## 227  002040 SCH1ZO 158.1  17.7  364  364  357     A     A     A    1    1
    ## 228  000269 CHR2CA  15.8  59.3  360  400  297     A     A     A    1    2
    ## 229  000603 ANTITR   6.9 118.7  360  362  377     A     A     A    1    2
    ## 230  002031 ELAEOL 153.5   5.1  360  362  341     A     A     A    1    2
    ## 231  002345 ELAEOL 154.2  84.4  360  363  320     A     A     A    1    1
    ## 232  007563 CAL2CA  90.1  87.3  360  360  358     A     A     A    1    1
    ## 233  009409 ELAEOL  62.1 136.2  360  380  383     A     A     A    1    1
    ## 234  010218 AST2GR 128.8  34.2  360  395  395     A     A     A    1    1
    ## 235  011319 ANTITR 197.5  74.5  360  360  358     A     A     A    1    1
    ## 236  007056 DALBRE  44.1 240.1  359  359  366     A     A     A    1    1
    ## 237  007345 AST2GR  91.4  20.0  359  385  414     A     A     A    1    1
    ## 238  001381 CAL2CA  17.3 294.2  357  375  386     A     A     A    1    1
    ## 239  008098 ANTITR  94.0 211.5  357  357  355     A     A     A    1    1
    ## 240  010138 ANTITR 120.8   5.5  357  375  486     A     A     A    1    1
    ## 241  003678 ELAEOL  23.4 235.5  355  355  350     A     A     A    1    1
    ## 242  006561 ELAEOL  51.7 135.2  355  373  250     A     A     A    1    1
    ## 243  009554 SCH1ZO  64.8 173.3  355  355  365     A     A     A    1    1
    ## 244  010467 SCH1ZO 125.3  95.3  355  356  356     A     A     A    1    1
    ## 245  011247 SCH1ZO 198.8  49.6  355  360  361     A     A     A    1    1
    ## 246  002362 ANACEX 159.0  84.3  353  385  397     A     A     A    1    1
    ## 247  006212 CAL2CA  49.3  53.7  353  358  360     A     A     A    1    1
    ## 248  000050 BURSSI  13.7   8.9  352  362  371     A     A     A    1    1
    ## 249  001108 INGAFA   6.0 239.7  352  379  383     A     A     A    1    1
    ## 250  009050 PSE1SE  71.6   0.2  352  356  357     A     A     A    1    1
    ## 251  006407 ANACEX  55.5  94.9  351  354  349     A     A     A    1    1
    ## 252  000671 PSE1SE  17.1 105.7  350  374  381     A     A     A    1    1
    ## 253  006519 ELAEOL  47.1 128.7  350  362  240     A     A     A    1    1
    ## 254  001269 SCH1ZO   7.4 260.5  348  350  354     A     A     A    1    1
    ## 255  010283 ALBIAD 123.9  54.3  348   -1   -1     A     D     D    1    0
    ## 256  003847 ANACEX  27.1 181.7  346  373  383     A     A     A    1    1
    ## 257  006343 ELAEOL  57.6  74.8  345  362  329     A     A     A    1    1
    ## 258  007288 SCH1ZO  93.2   7.4  345  345  345     A     A     A    1    1
    ## 259  009755 CAL2CA  69.7 219.1  345  350  354     A     A     A    1    1
    ## 260  000213 TAB1RO  16.4  23.4  344  344  343     A     A     A    1    1
    ## 261  007304 ELAEOL  96.2   4.5  344  334  330     A     A     A    1    1
    ## 262  009389 CAL2CA  72.0 116.3  344  372  373     A     A     A    1    1
    ## 263  010386 CAL2CA 121.3  79.7  344  370  374     A     A     A    1    1
    ## 264  003068 TRI2HI  34.9  18.3  342  343  341     A     A     A    1    1
    ## 265  007279 ANTITR  88.5  14.6  341  343  341     A     A     A    1    1
    ## 266  010408 CAL2CA 131.5  63.6  341  387  393     A     A     A    1    1
    ## 267  003357 ELAEOL  38.9  70.5  340   -9  260     A     *     A    1    0
    ## 268  007371 AST2GR  96.7  39.7  339  376  380     A     A     A    1    1
    ## 269  001642 SCH1ZO 100.1  68.8  338   -1   -1     A     D     D    1    0
    ## 270  006635 TRI2PL  59.6 153.4  338  344  335     A     A     A    1    1
    ## 271  007627 ANACEX  94.5 117.4  338  363  357     A     A     A    1    1
    ## 272  007882 CAL2CA  91.9 178.3  338  343  343     A     A     A    1    1
    ## 273  007813 SCH1ZO  97.4 145.5  337  340  340     A     A     A    1    1
    ## 274  000788 TRI2HI   5.6 154.1  336  334  337     A     A     A    1    1
    ## 275  001167 MACLTI   0.3 251.7  335  355  355     A     A     A    1    1
    ## 276  001306 MACRGL  18.6 269.6  334  336  338     A     A     A    1    1
    ## 277  007011 CAL2CA  48.9 222.7  334  342  343     A     A     A    1    1
    ## 278  011302 CAL2CA 190.4  71.8  332  332  333     A     A     A    1    1
    ## 279  000209 CAVAPL  17.0  28.7  331  331  335     A     A     A    1    1
    ## 280  003236 TRI2HI  29.1  59.8  330   -9  333     A     *     A    1    0
    ## 281  000927 ANTITR   0.2 182.0  329  335  330     A     A     A    1    1
    ## 282  007320 BURSSI  84.4  35.1  329  332  341     A     A     A    1    1
    ## 283  009485 ELAEOL  64.8 147.1  328  318  324     A     A     A    1    1
    ## 284  008426 TRI2PL  97.4 268.5  327   -1   -1     A     D     D    1    0
    ## 285  003261 DALBRE  32.0  49.2  326  322  327     A     A     A    1    1
    ## 286  004106 CAVAPL  37.5 234.3  325   -1   -1     A     D     D    1    0
    ## 287  007723 ELAEOL  97.5 124.0  325  325  322     A     A     A    1    1
    ## 288  000811 ANACEX  14.0 156.1  324  338  336     A     A     A    1    1
    ## 289  003317 CAL2CA  29.7  73.5  324  335  336     A     A     A    1    1
    ## 290  007162 SCH1ZO  54.7 272.7  324  325  322     A     A     A    1    1
    ## 291  004120 TRI2HI  38.6 223.9  323  325  325     A     A     A    1    1
    ## 292  001518 AST2GR 114.0  39.3  322  352  357     A     A     A    1    1
    ## 293  003419 SPONRA  32.2  89.1  321  335  334     A     A     A    1    1
    ## 294  010195 TRI2PL 139.7   5.7  321  332  336     A     A     A    1    1
    ## 295  000179 CAL2CA  11.1  35.7  320  345  344     A     A     A    1    1
    ## 296  002312 ELAEOL 144.4  83.0  320  321  280     A     A     A    1    1
    ## 297  003434 TRI2PL  36.6  91.5  320  323  332     A     A     A    1    1
    ## 298  007070 ELAEOL  49.8 250.6  320  315  240     A     A     A    1    1
    ## 299  007713 ELAEOL  97.3 132.6  320  320  322     A     A     A    1    2
    ## 300  010430 SCH1ZO 138.0  79.6  319  319  326     A     A     A    1    1
    ## 301  009123 ANTITR  74.7  20.6  318  318  312     A     A     A    1    1
    ## 302  001778 ANTITR 116.0  88.8  317  320  320     A     A     A    1    1
    ## 303  000014 TRI2PL   3.8  19.7  316  323  330     A     A     A    1    2
    ## 304  007282 CHR2CA  87.0   3.1  316  335  336     A     A     A    1    1
    ## 305  007329 CAL2CA  89.7  36.8  315  319  327     A     A     A    1    1
    ## 306  006390 ELAEOL  49.5  84.4  313  313  313     A     A     A    1    2
    ## 307  009422 ELAEOL  65.4 133.8  313  317  318     A     A     A    1    1
    ## 308  010390 EUGESA 128.5  70.3  313  316  316     A     A     A    1    1
    ## 309  006021 EUGESA  44.0  16.2  312  325  315     A     A     A    1    1
    ## 310  007966 SPONMO  89.7 186.8  312  332  333     A     A     A    1    1
    ## 311  006581 SCH1ZO  59.7 133.7  311  308  309     A     A     A    1    1
    ## 312  003574 DALBRE  28.2 128.3  310  314  323     A     A     A    1    1
    ## 313  003934 ANNOPU  38.9 180.5  310   -1   -1     A     D     D    1    0
    ## 314  007614 ELAEOL  85.2 106.3  310  310  310     A     A     A    1    1
    ## 315  009040 BURSSI  69.3   8.5  310  323  326     A     A     A    1    1
    ## 316  002093 TRI2HI 145.4  22.9  309  309   -1     A     A     D    1    2
    ## 317  011003 CAL2CA 174.6  38.0  308  308  330     A     A     A    1    1
    ## 318  011042 GUARGL 168.7  41.9  308  303   -1     A     A     D    1    1
    ## 319  009825 CAL2CA  75.5 214.6  305  296  302     A     A     A    1    1
    ## 320  010950 ELAEOL 176.3   8.8  305  305  303     A     A     A    1    2
    ## 321  004119 ANTITR  37.0 223.9  302  305  303     A     A     A    1    1
    ## 322  000135 DALBRE   2.5  34.3  301  316  322     A     A     A    1    2
    ## 323  000829 ANACEX  16.5 140.7  301  313  311     A     A     A    1    1
    ## 324  001776 EUGESA 119.8  92.6  300  330  323     A     A     A    1    2
    ## 325  007180 CEDROD  57.5 266.4  299  301  298     A     A     A    1    1
    ## 326  007344 SCH1ZO  89.2  21.8  299  304  298     A     A     A    1    1
    ## 327  011249 ANACEX 199.8  40.6  298  316  319     A     A     A    1    1
    ## 328  008488 GUARGL 183.5   3.3  297  300  300     A     A     A    1    1
    ## 329  008611 COLUGL 196.3  35.1  297  325  327     A     A     A    1    2
    ## 330  010064 ANACEX  74.7 276.6  297  297  297     A     A     A    1    1
    ## 331  011194 ANACEX 180.3  45.3  295  308  308     A     A     A    1    1
    ## 332  004147 ANTITR  26.7 250.4  292  310  305     A     A     A    1    1
    ## 333  004078 MACRGL  29.1 238.4  291  295  289     A     A     A    1    1
    ## 334  009152 ANTITR  75.3  23.8  291  295  293     A     A     A    1    1
    ## 335  009393 ELAEOL  79.9 118.5  290  291  293     A     A     A    1    1
    ## 336  003473 SCH1ZO  26.6 107.7  288  320  327     A     A     A    1    1
    ## 337  006381 ANACEX  49.4  85.7  288  313  304     A     A     A    1    1
    ## 338  007226 TAB1RO  54.8 281.2  287  297  295     A     A     A    1    1
    ## 339  011320 CAL2CA 197.3  74.2  285  300  302     A     A     A    1    1
    ## 340  006589 TRI2PL  56.6 128.3  283  293  290     A     A     A    1    1
    ## 341  007820 ANACEX  98.4 145.0  282  286  297     A     A     A    1    2
    ## 342  010094 SPONMO  61.5 292.6  282  290  291     A     A     A    1    2
    ## 343  011371 CAVAPL 199.4  93.8  282  285  286     A     A     A    1    1
    ## 344  000620 INGAFA   6.8 105.2  281  285  291     A     A     A    1    2
    ## 345  006210 CAL2CA  47.7  53.7  281  394  382     A     A     A    1    1
    ## 346  008493 GUARGL 181.7  10.0  281  290  293     A     A     A    1    1
    ## 347  002302 SCH1ZO 155.9  66.3  280  283  283     A     A     A    1    1
    ## 348  006750 ELAEOL  55.3 161.3  280  280  230     A     A     A    1    1
    ## 349  007392 SPONMO  97.7  24.8  280   -1   -1     A     D     D    1    0
    ## 350  001732 SCH1ZO 100.2  93.1  279   -1   -1     A     D     D    1    0
    ## 351  008001 AST2GR  98.6 198.7  279  281  287     A     A     A    1    1
    ## 352  009788 SPONRA  74.2 202.0  279  290  290     A     A     A    1    1
    ## 353  006736 CAL2CA  56.3 175.3  278  295  299     A     A     A    1    1
    ## 354  009021 TRI2PL  60.7  18.2  278   -1   -1     A     D     D    1    0
    ## 355  009199 TRI2PL  74.7  40.5  278   -1   -1     A     D     D    1    0
    ## 356  000440 BROSAL   3.4  90.4  277  314  325     A     A     A    1    1
    ## 357  001483 ALBIAD 103.9  26.8  277  328  343     A     A     A    1    1
    ## 358  004218 SCH1ZO  29.6 276.1  277  277  277     A     A     A    1    1
    ## 359  009366 TRI2PL  78.3  84.4  277  230  232     A     A     A    1    1
    ## 360  000695 ELAEOL   0.7 126.1  276  307  295     A     A     A    1    2
    ## 361  011154 GUARGL 162.8  96.3  275  283  280     A     A     A    1    1
    ## 362  001173 CAS1MO   2.5 255.7  274  308  309     A     A     A    1    1
    ## 363  011144 CAL2CA 164.6  80.2  273  282  285     A     A     A    1    1
    ## 364  001365 ANTITR  13.4 290.5  272  303  305     A     A     A    1    1
    ## 365  003714 TRI2PL  37.5 144.7  272  274  273     A     A     A    1    1
    ## 366  007193 TRI2PL  43.2 283.1  272  286  289     A     A     A    1    1
    ## 367  003289 ANTITR  36.0  42.7  270  333  289     A     A     A    1    1
    ## 368  007598 TRI2PL  96.4  82.4  270  275  275     A     A     A    1    1
    ## 369  010318 EUGESA 133.0  49.2  270  273  278     A     A     A    1    1
    ## 370  011001 PROTTE 171.2  39.8  270  272   -1     A     A     D    1    2
    ## 371  000306 TRI2PL   4.4  66.2  267  299  303     A     A     A    1    1
    ## 372  001432 SCH1ZO 114.7   7.4  267  271  267     A     A     A    1    1
    ## 373  001510 ANTITR 114.4  23.6  267  306  278     A     A     A    1    2
    ## 374  011098 ALSEBL 167.3  70.9  267  267  274     A     A     A    1    1
    ## 375  011379 TRI2HI 198.6  80.1  266  272  282     A     A     A    1    1
    ## 376  002353 TRI2PL 158.3  97.4  265  265  265     A     A     A    1    1
    ## 377  006068 POUTCA  52.8  11.4  265  280  280     A     A     A    1    1
    ## 378  006073 TRI2PL  52.2  17.4  265   -1   -1     A     D     D    1    0
    ## 379  007061 SWARS1  40.1 251.5  265  270  270     A     A     A    1    1
    ## 380  000468 CAVAPL   5.3  93.5  264  255   -1     A     A     D    1    1
    ## 381  001752 CAL2CA 110.8  81.3  262  271  280     A     A     A    1    1
    ## 382  003413 ELAEOL  32.0  84.6  262  270  269     A     A     A    1    1
    ## 383  006777 CAL2CA  41.8 196.9  262  270  270     A     A     A    1    1
    ## 384  007385 GUAZUL  98.8  27.6  262  287  284     A     A     A    1    1
    ## 385  000323 TRI2PL   9.7  70.2  261  263  258     A     A     A    1    1
    ## 386  010459 CAVAPL 121.8  92.6  261  269  286     A     A     A    1    1
    ## 387  002035 TRI2PL 151.2  14.8  260  266  272     A     A     A    1    1
    ## 388  011338 ANTITR 181.2  96.5  260  260  259     A     A     A    1    1
    ## 389  009076 BURSSI  75.3  12.5  259  257   -1     A     A     D    1    1
    ## 390  007309 ANTITR  84.7  23.0  258  259  258     A     A     A    1    1
    ## 391  009122 ANTITR  69.7  24.1  258  261  261     A     A     A    1    1
    ## 392  001584 TRI2PL 114.4  49.7  257  262   -1     A     A     D    1    1
    ## 393  003738 TRI2PL  28.6 178.7  257  257   -1     A     A     D    1    1
    ## 394  007093 SCH1ZO  55.3 259.9  257  255  260     A     A     A    1    1
    ## 395  010393 BROSAL 125.3  70.5  257  282  288     A     A     A    1    1
    ## 396  008383 CAL2CA  89.2 264.0  256  272  262     A     A     A    1    2
    ## 397  009471 TRI2PL  79.8 128.7  256  262  258     A     A     A    1    1
    ## 398  003424 ANTITR  31.6  92.6  255  258  254     A     A     A    1    1
    ## 399  010470 CAL2CA 129.1  98.6  255  255  268     A     A     A    1    1
    ## 400  001160 ZUELGU   3.7 244.2  254  261  262     A     A     A    1    1
    ## 401  010464 CAL2CA 121.9  99.8  254  266  269     A     A     A    1    1
    ## 402  002237 TRI2PL 142.3  65.4  252  256  261     A     A     A    1    1
    ## 403  007310 ANTITR  82.7  25.4  252  267  258     A     A     A    1    1
    ## 404  007612 ELAEOL  85.6 110.5  252  254  250     A     A     A    1    2
    ## 405  002282 SCH1ZO 152.0  68.6  251   -1   -1     A     D     D    1    0
    ## 406  004270 BURSSI  23.7 282.3  251  252  248     A     A     A    1    1
    ## 407  000351 TRI2PL   9.1  63.4  250  259  263     A     A     A    1    2
    ## 408  001621 CAL2CA 118.7  54.2  250  256  257     A     A     A    1    1
    ## 409  008494 ELAEOL 184.7   8.8  250  252  253     A     A     A    1    2
    ## 410  009099 GUARGL  60.6  36.1  250  250  249     A     A     A    1    1
    ## 411  009109 ANTITR  67.3  30.5  250  253  247     A     A     A    1    1
    ## 412  000575 TRI2PL   3.8 102.2  249  254  254     A     A     A    1    1
    ## 413  003396 ANACEX  25.2  89.3  247  252  252     A     A     A    1    1
    ## 414  007914 TRI2PL  82.1 189.8  246  265  263     A     A     A    1    2
    ## 415  008425 MANGIN  95.1 266.1  245  245  245     A     A     A    1    1
    ## 416  011059 ELAEOL 172.5  59.6  245  246  248     A     A     A    1    2
    ## 417  001482 ANTITR 102.8  25.8  244  278  275     A     A     A    1    2
    ## 418  007978 TRI2PL  85.4 185.3  243   -1   -1     A     D     D    1    0
    ## 419  010504 TRI2PL 130.1  95.6  242  242  243     A     A     A    1    1
    ## 420  000066 TRI2PL  14.3  13.4  241  258  260     A     A     A    1    1
    ## 421  001199 FICUMA  10.3 251.2  241  280  292     A     A     A    1    1
    ## 422  009131 BROSAL  71.2  36.4  241  254  261     A     A     A    1    1
    ## 423  007001 ELAEOL  46.2 246.4  240  236  225     A     A     A    1    2
    ## 424  010139 ELAEOL 124.7   6.7  240  242  315     A     A     A    1    1
    ## 425  001610 AST2GR 115.5  33.4  239  252  269     A     A     A    1    1
    ## 426  007728 CAL2CA  84.8 142.2  239  243  244     A     A     A    1    1
    ## 427  002346 GUARGL 152.4  84.6  238  241  275     A     A     A    1    2
    ## 428  011056 TRI2PL 171.7  55.3  238  241  237     A     A     A    1    1
    ## 429  001591 CECRLO 110.4  55.4  237   -1   -1     A     D     D    1    0
    ## 430  002077 TRI2PL 140.6  36.2  237  255  254     A     A     A    1    1
    ## 431  000566 CAL2CA  16.4  82.5  236  504  510     A     A     A    1    2
    ## 432  009475 BROSAL  76.3 120.9  236  236  238     A     A     A    1    1
    ## 433  004098 TRI2PL  34.7 230.2  235   -1   -1     A     D     D    1    0
    ## 434  003277 POUTCA  38.6  50.6  234  235  240     A     A     A    1    1
    ## 435  003454 ALBIAD  24.4 111.1  234  245  248     A     A     A    1    1
    ## 436  003442 HEISCO  39.4  82.3  233  233  242     A     A     A    1    1
    ## 437  008101 HEISCO  92.6 216.7  233  240  236     A     A     A    1    1
    ## 438  003617 TRI2PL  38.3 120.6  232  241  242     A     A     A    1    1
    ## 439  010153 AST2GR 127.0  17.8  232  259  272     A     A     A    1    1
    ## 440  011307 CAL2CA 190.0  77.5  232  255  241     A     A     A    1    1
    ## 441  000682 TRI2PL  15.7 104.9  231  231  233     A     A     A    1    1
    ## 442  004214 ANACEX  23.8 279.4  231  215  218     A     A     A    1    1
    ## 443  001490 TRI2HI 102.0  37.3  230  238  235     A     A     A    1    1
    ## 444  003845 DENDAR  29.2 188.5  230  243  244     A     A     A    1    1
    ## 445  009072 ANTITR  77.4  19.6  230  233  234     A     A     A    1    1
    ## 446  007524 ANTITR  97.6  66.5  229   -1   -1     A     D     D    1    0
    ## 447  009067 ANTITR  74.1  16.4  229  229  228     A     A     A    1    1
    ## 448  011170 ANTITR 172.6  80.2  229  229  229     A     A     A    1    1
    ## 449  001464 ANTITR 116.5  11.6  228  242  241     A     A     A    1    2
    ## 450  009095 TRI2PL  60.5  30.3  228  235  239     A     A     A    1    1
    ## 451  006138 POUTCA  48.1  28.7  227  230  232     A     A     A    1    1
    ## 452  006690 TRI2PL  48.4 179.9  227  232  232     A     A     A    1    1
    ## 453  007420 TRI2PL  88.8  49.1  227  241  243     A     A     A    1    1
    ## 454  003037 BURSSI  22.7  19.6  226  257  265     A     A     A    1    1
    ## 455  011224 GUARGL 194.0  52.5  226  226  227     A     A     A    1    1
    ## 456  000968 TRI2PL  13.5 184.2  225  233  234     A     A     A    1    1
    ## 457  003408 ANNOPU  25.2  81.8  225   -1   -1     A     D     D    1    0
    ## 458  007233 CAL2CA  53.9 291.1  225  237  235     A     A     A    1    1
    ## 459  006163 POUTCA  52.9  34.2  223  232  230     A     A     A    1    1
    ## 460  006285 TRI2PL  41.2  67.2  223  227  223     A     A     A    1    1
    ## 461  007854 TRI2PL  89.0 167.9  223  232  235     A     A     A    1    1
    ## 462  009002 TRI2PL  61.2   3.7  222  226  228     A     A     A    1    1
    ## 463  001098 ORMOMA   0.8 230.8  221  223  223     A     A     A    1    1
    ## 464  001493 TRI2PL 107.8  39.1  221  235  238     A     A     A    1    1
    ## 465  001511 CAL2CA 110.4  27.3  221  237  242     A     A     A    1    2
    ## 466  007585 TRI2PL  97.9  94.8  221  260  224     A     A     A    1    1
    ## 467  001333 MACRGL   1.4 299.1  220  235  237     A     A     A    1    1
    ## 468  002251 SCH1ZO 143.0  76.6  220  230  221     A     A     A    1    1
    ## 469  003562 TRI2PL  29.5 135.3  220  228  218     A     A     A    1    1
    ## 470  006512 TRI2PL  49.4 132.9  220  213  208     A     A     A    1    1
    ## 471  007540 ANACEX  82.2  99.9  220  221  226     A     A     A    1    1
    ## 472  009088 GUARGL  63.0  23.1  220  229  233     A     A     A    1    1
    ## 473  003608 TRI2PL  35.4 133.3  219  220  221     A     A     A    1    1
    ## 474  003898 TRI2PL  31.4 196.2  219  219  227     A     A     A    1    1
    ## 475  009704 CAL2CA  79.2 195.2  219  222  222     A     A     A    1    1
    ## 476  007386 SPONMO  99.8  22.6  218  223  221     A     A     A    1    1
    ## 477  000557 ANNOPU  15.7  88.1  216   -1   -1     A     D     D    1    0
    ## 478  001555 BROSAL 107.7  59.7  216  259  268     A     A     A    1    1
    ## 479  006374 TRI2PL  44.1  91.2  216  217  221     A     A     A    1    1
    ## 480  009282 BROSAL  65.5  60.1  216  223  224     A     A     A    1    1
    ## 481  008021 PROTTE  96.0 189.8  215  224  227     A     A     A    1    1
    ## 482  008525 POUTCA 185.8   0.0  215  221  222     A     A     A    1    1
    ## 483  009052 PROTTE  73.6   5.4  215  254  230     A     A     A    1    2
    ## 484  009973 CAL2CA  70.2 254.3  215  217  217     A     A     A    1    1
    ## 485  001341 CECRPE   5.9 291.4  214   -1   -1     A     D     D    1    0
    ## 486  007620 ANACEX  85.1 102.4  214  224  226     A     A     A    1    1
    ## 487  000452 SWARS1   9.7  95.5  213  248  235     A     A     A    1    2
    ## 488  002083 ANTITR 148.1  32.6  213  217  218     A     A     A    1    1
    ## 489  004268 SWARS1  23.7 283.4  213  215  240     A     A     A    1    1
    ## 490  000166 ANTITR  12.1  29.9  212  222  221     A     A     A    1    1
    ## 491  006329 AST2GR  52.5  69.8  212  217  217     A     A     A    1    1
    ## 492  007098 SWARS1  55.4 240.3  212  221  220     A     A     A    1    1
    ## 493  006596 CAL2CA  59.8 122.4  211  228  235     A     A     A    1    1
    ## 494  009590 CAL2CA  74.0 172.2  211  248  256     A     A     A    1    1
    ## 495  001537 CAL2CA 101.6  45.6  210  226  231     A     A     A    1    1
    ## 496  009378 ANACEX  60.6 117.8  210   -1   -1     A     D     D    1    0
    ## 497  004282 ANDIIN  24.4 291.9  209  213  218     A     A     A    1    1
    ## 498  007271 SWARS1  82.9  19.8  209  216  222     A     A     A    1    1
    ## 499  009629 ANNOPU  60.2 183.4  209  212  212     A     A     A    1    1
    ## 500  009298 TRI2PL  73.9  70.8  208  208  209     A     A     A    1    1
    ## 501  009398 ELAEOL  78.9 107.5  208  209  212     A     A     A    1    1
    ## 502  009546 ORMOMA  63.6 162.4  208  218  221     A     A     A    1    1
    ## 503  011229 TRI2PL 194.9  58.7  208  217  216     A     A     A    1    1
    ## 504  003134 TRI2PL  29.8  28.7  207  220  223     A     A     A    1    1
    ## 505  004176 CECROB  32.6 256.8  206   -9   -1     A     *     D    1    0
    ## 506  007573 TRI2PL  93.5  90.8  206  206  206     A     A     A    1    1
    ## 507  000265 TRI2PL  15.4  56.6  205  225  225     A     A     A    1    1
    ## 508  001559 ALBIAD 106.5  53.4  205  220  217     A     A     A    1    1
    ## 509  003008 CAL2CA  24.4   6.1  205  217  222     A     A     A    1    1
    ## 510  010297 TRI2PL 129.1  51.9  205  222  225     A     A     A    1    1
    ## 511  000373 TRIPCU  11.6  70.3  204  221  224     A     A     A    1    1
    ## 512  002278 CAL2CA 150.5  61.6  203  215  220     A     A     A    1    1
    ## 513  000031 SWARS1   9.6   8.3  201  205  205     A     A     A    1    1
    ## 514  000602 INGAFA   6.5 117.9  201  205  206     A     A     A    1    1
    ## 515  001456 ALBIAD 110.3  18.6  201  217   -1     A     A     D    1    1
    ## 516  002352 TRI2PL 157.7  96.6  201  203  203     A     A     A    1    1
    ## 517  003572 TRI2PL  26.2 125.4  201  201  198     A     A     A    1    1
    ## 518  001548 CAL2CA 104.5  51.8  200  211  208     A     A     A    1    1
    ## 519  004102 TRI2PL  33.2 235.6  200   -1   -1     A     D     D    1    0
    ## 520  009091 INGAFA  60.4  27.4  200  243  245     A     A     A    1    1
    ## 521  003440 INGAVE  35.3  83.5  199  203  199     A     A     A    1    1
    ## 522  003756 PROTTE  32.3 165.5  199  200  212     A     A     A    1    1
    ## 523  004071 POUTCA  24.8 221.1  199  199  197     A     A     A    1    1
    ## 524  007527 GUARGL  98.5  63.9  199  199  199     A     A     A    1    1
    ## 525  011137 POSOLA 178.8  71.2  199  202  202     A     A     A    1    1
    ## 526  000759 HEISCO  15.5 134.8  198  201  204     A     A     A    1    1
    ## 527  001214 CECRLO  19.5 258.9  198   -1   -1     A     D     D    1    0
    ## 528  008166 ANDIIN  92.4 226.5  198  200  200     A     A     A    1    1
    ## 529  002294 GUARGL 150.3  75.8  197   -1   -1     A     D     D    1    0
    ## 530  001310 ANACEX  18.7 261.5  196  213  213     A     A     A    1    1
    ## 531  001644 SWARS1 101.8  70.6  196  215  217     A     A     A    1    2
    ## 532  006084 PIT1RU  59.1  10.0  196  219  199     A     A     A    1    1
    ## 533  001308 CECRLO  19.8 261.1  195   -1   -1     A     D     D    1    0
    ## 534  004206 SWARS1  20.4 267.5  195  195  196     A     A     A    1    1
    ## 535  006455 TRI2PL  58.7 115.9  195  209  210     A     A     A    1    1
    ## 536  009401 ANACEX  78.6 101.2  195  198  201     A     A     A    1    2
    ## 537  009997 CAL2CA  77.6 253.4  195  195  195     A     A     A    1    1
    ## 538  009015 GUARGL  63.1  15.5  194  196  198     A     A     A    1    1
    ## 539  009905 COPAAR  73.5 235.6  194  200  203     A     A     A    1    1
    ## 540  010207 CAL2CA 122.5  30.5  194  201  211     A     A     A    1    1
    ## 541  011050 DENDAR 174.1  47.8  194  211  222     A     A     A    1    1
    ## 542  000526 ANACEX  14.6  95.1  193  194   -1     A     A     D    1    1
    ## 543  001380 MACRGL  17.9 290.6  193  209  212     A     A     A    1    1
    ## 544  003095 ANTITR  23.2  24.4  193  200  205     A     A     A    1    1
    ## 545  006420 AST2GR  43.6 111.2  193  197  198     A     A     A    1    1
    ## 546  006975 BROSAL  45.0 226.0  193  199  206     A     A     A    1    1
    ## 547  007134 ANACEX  47.3 274.9  193  190  190     A     A     A    1    1
    ## 548  000836 INGAFA   4.2 165.8  192  197  197     A     A     A    1    1
    ## 549  001509 CAL2CA 114.7  22.9  192  197  195     A     A     A    1    1
    ## 550  006111 TRI2PL  41.2  31.3  192  200  201     A     A     A    1    1
    ## 551  011263 TRI2PL 182.4  64.2  192  198  199     A     A     A    1    1
    ## 552  001026 PROTTE   5.4 204.7  191  190  197     A     A     A    1    1
    ## 553  001288 HIRTAM  14.7 276.2  191  195  196     A     A     A    1    1
    ## 554  003055 CAL2CA  30.2   5.2  191  197  220     A     A     A    1    1
    ## 555  009566 DALBRE  69.1 176.9  191  202  202     A     A     A    1    1
    ## 556  010306 AST2GR 129.7  42.4  191  199  199     A     A     A    1    1
    ## 557  010468 TRI2PL 127.3  99.4  191  191  191     A     A     A    1    1
    ## 558  007378 ANTITR  95.6  33.5  190  194  195     A     A     A    1    1
    ## 559  008241 TRI2PL  83.6 252.8  190  162  163     A     A     A    1    1
    ## 560  010201 TRIPCU 120.1  24.6  190  192  192     A     A     A    1    1
    ## 561  011090 TRI2PL 164.5  72.7  190  197  198     A     A     A    1    1
    ## 562  006108 ANTITR  43.2  27.7  189  199  190     A     A     A    1    1
    ## 563  009201 TRI2PL  70.5  45.1  189  195  198     A     A     A    1    1
    ## 564  009952 TRI2PL  65.1 256.6  189  189  192     A     A     A    1    1
    ## 565  000669 PROTTE  19.0 113.3  188  191  194     A     A     A    1    2
    ## 566  004089 TRI2HI  29.5 224.4  188  190  187     A     A     A    1    1
    ## 567  009423 INGAFA  69.5 134.4  188  225  228     A     A     A    1    1
    ## 568  010197 GUARGL 135.1   3.8  188  191  193     A     A     A    1    1
    ## 569  001650 CAL2CA 108.1  75.9  187  203  207     A     A     A    1    2
    ## 570  003497 PROTTE  33.2 109.8  187  190  189     A     A     A    1    1
    ## 571  007303 PROTTE  96.3   8.5  187   -1   -1     A     D     D    1    0
    ## 572  007940 TRI2PL  89.8 190.9  187   -1   -1     A     D     D    1    0
    ## 573  010154 ANTITR 129.7  18.4  187  195  192     A     A     A    1    1
    ## 574  003996 IXORFL  34.1 201.5  186  188  188     A     A     A    1    1
    ## 575  004124 SWARS1  20.2 240.8  186  187  194     A     A     A    1    1
    ## 576  007526 ANTITR  98.5  64.8  186  186  187     A     A     A    1    1
    ## 577  009274 ANTITR  68.4  66.4  185   -1   -1     A     D     D    1    0
    ## 578  010109 TRI2PL  65.7 287.4  185  191  162     A     A     A    1    2
    ## 579  011172 ZUELGU 170.4  83.4  185   -1   -1     A     D     D    1    0
    ## 580  007318 ANTITR  81.2  33.2  184  193  198     A     A     A    1    1
    ## 581  003420 HEISCO  32.8  89.8  183  185  186     A     A     A    1    1
    ## 582  004327 ANTITR  31.0 299.8  183  187  188     A     A     A    1    1
    ## 583  000243 SWARS1  10.9  45.1  182  190  190     A     A     A    1    1
    ## 584  008526 PROTTE 185.2   3.0  182  182  187     A     A     A    1    1
    ## 585  009227 TRI2PL  78.4  50.6  182  182  181     A     A     A    1    1
    ## 586  000133 ALSEBL   4.8  32.6  181  188  190     A     A     A    1    1
    ## 587  001766 ANTITR 116.2  95.2  181  210  215     A     A     A    1    2
    ## 588  003242 SWARS1  27.5  54.4  181  184  185     A     A     A    1    1
    ## 589  003702 COPAAR  39.6 150.8  181  183  184     A     A     A    1    1
    ## 590  004117 ANACEX  35.6 223.7  181  185  191     A     A     A    1    1
    ## 591  006225 CAL2CA  49.5  42.8  181  193  186     A     A     A    1    1
    ## 592  006776 TRI2PL  41.3 195.8  181  202  204     A     A     A    1    1
    ## 593  009081 POSOLA  75.3   0.8  181  181  184     A     A     A    1    1
    ## 594  010148 TRI2PL 121.3  19.6  181  183  183     A     A     A    1    1
    ## 595  002297 TRI2PL 158.9  75.5  180  180  190     A     A     A    1    2
    ## 596  006026 POUTCA  49.4  16.1  180  325  280     A     A     A    1    1
    ## 597  006707 TRI2PL  46.4 169.9  180  180  181     A     A     A    1    1
    ## 598  007026 TRI2PL  51.3 226.5  180  183  188     A     A     A    1    1
    ## 599  008586 ANNOSP 186.5  29.5  180  187  190     A     A     A    1    1
    ## 600  009780 GUARGL  72.4 200.2  180  182  182     A     A     A    1    1
    ## 601  009867 ANACEX  64.5 231.8  180  181  181     A     A     A    1    1
    ## 602  011141 ELAEOL 175.5  68.3  180  183  187     A     A     A    1    1
    ## 603  001250 LUEHSE   4.3 266.2  179  223  223     A     A     A    1    1
    ## 604  001313 SWARS1  15.2 262.4  179  184  185     A     A     A    1    1
    ## 605  007637 TRI2PL  99.8 114.2  179  183  177     A     A     A    1    1
    ## 606  009175 TRI2PL  65.4  55.4  179  187  189     A     A     A    1    1
    ## 607  011309 GUARGL 199.9  76.0  179  179  175     A     A     A    1    1
    ## 608  002200 CAL2CA 153.2  58.7  178  181  180     A     A     A    1    1
    ## 609  003798 CAL2CA  24.2 180.1  178  180  179     A     A     A    1    1
    ## 610  004333 CAL2CA  38.8 298.7  178  180  185     A     A     A    1    1
    ## 611  008255 CAL2CA  88.9 258.5  178  176  173     A     A     A    1    1
    ## 612  003983 TRI2PL  25.9 214.3  177  182  182     A     A     A    1    1
    ## 613  007259 PROTTE  82.5   0.0  177  183  184     A     A     A    1    1
    ## 614  007352 AST2GR  92.5  27.2  177  181   -1     A     A     D    1    1
    ## 615  001003 SWARS1   0.3 211.8  176  180  188     A     A     A    1    1
    ## 616  010272 TRI2PL 122.1  43.7  176  195  185     A     A     A    1    1
    ## 617  000941 HEISCO   9.7 195.5  175  185  192     A     A     A    1    1
    ## 618  001715 TRIPCU 118.6  63.8  175  199  189     A     A     A    1    2
    ## 619  004317 COPAAR  34.1 286.6  175  180  189     A     A     A    1    1
    ## 620  008475 POUTCA  98.5 291.4  175  505  500     A     A     A    1    2
    ## 621  008572 NECTMA 183.6  37.4  175  188  199     A     A     A    1    1
    ## 622  006018 ANTITR  42.4  19.0  174  124  165     A     A     A    1    1
    ## 623  010162 GUARGL 127.6  14.9  174  184  180     A     A     A    1    1
    ## 624  011011 TERMOB 176.2  33.4  174  188  193     A     A     A    1    1
    ## 625  000659 PROTTE  10.4 118.6  173  188  203     A     A     A    1    2
    ## 626  001404 TRI2PL 102.8  18.9  173  173  174     A     A     A    1    1
    ## 627  001756 AST2GR 113.6  85.8  173  182  185     A     A     A    1    1
    ## 628  007333 ANTITR  85.9  34.7  173  182  178     A     A     A    1    1
    ## 629  008088 BROSAL  93.4 201.5  173  185  193     A     A     A    1    1
    ## 630  009138 TRI2PL  76.0  39.8  173  170   -1     A     A     D    1    1
    ## 631  009210 TRI2PL  71.0  54.7  173  185  185     A     A     A    1    1
    ## 632  003632 TRI2PL  23.0 148.7  172  173  172     A     A     A    1    1
    ## 633  006987 SWARS1  41.6 238.3  172  175  175     A     A     A    1    1
    ## 634  007053 IXORFL  59.4 220.1  172  177  180     A     A     A    1    1
    ## 635  007291 GUARGL  90.1  15.7  172  178  178     A     A     A    1    1
    ## 636  000001 PROTTE   3.0   0.9  171  267  277     A     A     A    1    2
    ## 637  006705 TRI2PL  45.7 165.6  171  171  171     A     A     A    1    1
    ## 638  007074 MACRGL  49.1 254.6  171  173  174     A     A     A    1    1
    ## 639  002331 TRI2PL 147.3  99.8  170  180  175     A     A     A    1    1
    ## 640  002347 HYMECO 151.2  85.2  170  176  178     A     A     A    1    1
    ## 641  007544 TRI2PL  87.4  93.7  170  179  179     A     A     A    1    1
    ## 642  009658 ADE1TR  66.9 197.5  170  174  174     A     A     A    1    1
    ## 643  006630 CAL2CA  50.2 155.8  169  173  172     A     A     A    1    1
    ## 644  009129 ANTITR  70.6  26.2  169  162  163     A     A     A    1    1
    ## 645  009675 TRI2PL  66.3 180.4  169  188  192     A     A     A    1    1
    ## 646  010953 POSOLA 175.3   7.0  169  174  170     A     A     A    1    1
    ## 647  000223 PROTTE   4.3  45.6  168  198  194     A     A     A    1    1
    ## 648  006012 CAL2CA  40.8  11.4  168  174  180     A     A     A    1    1
    ## 649  006671 HEISCO  43.1 166.7  168  170  171     A     A     A    1    1
    ## 650  007062 PROTTE  40.1 257.2  168  176  172     A     A     A    1    1
    ## 651  007908 TRI2PL  82.2 183.4  168  173  171     A     A     A    1    1
    ## 652  000643 ANDIIN  13.2 104.9  167  168  167     A     A     A    1    1
    ## 653  000871 CHR2CA   5.2 163.0  167  170  170     A     A     A    1    1
    ## 654  006666 TRI2PL  40.5 167.5  167  177  177     A     A     A    1    2
    ## 655  007891 TRI2PL  97.2 170.7  167  174  174     A     A     A    1    1
    ## 656  009884 TRI2PL  68.2 220.3  167  176  178     A     A     A    1    1
    ## 657  003602 ANDIIN  35.6 136.8  166  167  167     A     A     A    1    1
    ## 658  010478 LUEHSP 127.6  81.1  166  166  170     A     A     A    1    1
    ## 659  001430 TRI2PL 110.8   7.9  165  171  171     A     A     A    1    1
    ## 660  001133 HEISCO  13.5 226.9  164  187  188     A     A     A    1    2
    ## 661  006130 TRI2PL  48.4  25.7  164  185  176     A     A     A    1    1
    ## 662  006622 TRI2PL  52.2 143.6  164  167  166     A     A     A    1    1
    ## 663  007592 EUGECO  99.7  87.7  164  167  167     A     A     A    1    2
    ## 664  009080 EUGECO  76.9   0.5  164  167  166     A     A     A    1    1
    ## 665  009125 ANTITR  71.1  21.8  164  165   -1     A     A     D    1    1
    ## 666  007299 ANDIIN  98.6  17.0  163  162  162     A     A     A    1    1
    ## 667  011066 SWARS1 179.6  54.6  163  163  163     A     A     A    1    1
    ## 668  001520 TRI2PL 119.1  36.6  162  179  182     A     A     A    1    1
    ## 669  004049 TRI2PL  36.3 212.1  162  170  169     A     A     A    1    1
    ## 670  008457 SWARS1  89.8 294.1  162  162  161     A     A     A    1    1
    ## 671  010056 BROSAL  72.8 275.5  162  171  171     A     A     A    1    1
    ## 672  010313 RAUVLI 132.9  43.3  162  163  163     A     A     A    1    1
    ## 673  003309 CECRLO  24.5  72.6  161   -9  181     A     *     A    1    0
    ## 674  008037 TRI2PL  81.1 202.4  161  173  176     A     A     A    1    1
    ## 675  009802 ANACEX  74.7 206.4  161  142  173     A     A     A    1    1
    ## 676  001084 GUARGL   1.4 224.1  160  158  160     A     A     A    1    1
    ## 677  001134 PROTTE  10.8 229.0  160  176  180     A     A     A    1    1
    ## 678  001347 SWARS1   8.7 280.8  160  175  192     A     A     A    1    1
    ## 679  001515 TRI2PL 114.8  36.5  160   -1   -1     A     D     D    1    0
    ## 680  006046 TRI2PL  48.9   3.7  160  160  165     A     A     A    1    1
    ## 681  007290 ELAEOL  91.8  10.3  160  165  172     A     A     A    1    1
    ## 682  009066 TRI2PL  74.2  18.5  160  165  166     A     A     A    1    1
    ## 683  009146 GUARGL  77.7  25.4  160  162  163     A     A     A    1    1
    ## 684  011099 ANACEX 165.5  71.1  160  163  162     A     A     A    1    1
    ## 685  000789 HEISCO   6.8 154.6  159  162  165     A     A     A    1    1
    ## 686  007767 TRI2PL  85.0 146.8  159  173  178     A     A     A    1    1
    ## 687  009915 BROSAL  79.0 237.5  159  166  175     A     A     A    1    1
    ## 688  001480 GUARGL 100.7  21.6  158  167  169     A     A     A    1    1
    ## 689  001779 CAL2CA 118.0  89.7  158  161  163     A     A     A    1    1
    ## 690  010210 CAL2CA 123.3  35.9  158  160  163     A     A     A    1    1
    ## 691  001665 ANTITR 105.1  64.1  157  188  187     A     A     A    1    2
    ## 692  003510 DIPHRO  31.7 115.2  156  158  162     A     A     A    1    1
    ## 693  004079 SWARS2  28.3 230.4  156  160  160     A     A     A    1    1
    ## 694  004161 SWARS1  30.8 240.7  156  157  164     A     A     A    1    1
    ## 695  010974 AST2GR 169.6  36.9  156  156  160     A     A     A    1    1
    ## 696  011193 BROSAL 179.2  80.0  156  165  169     A     A     A    1    1
    ## 697  001463 GUARGL 116.1  11.4  155  158  160     A     A     A    1    2
    ## 698  006447 TRI2PL  52.1 105.7  155  152  154     A     A     A    1    1
    ## 699  009371 VITECO  64.6 102.5  155  155  157     A     A     A    1    1
    ## 700  009644 FARAOC  61.2 195.0  155  157   -1     A     A     D    1    1
    ## 701  002146 TRI2PL 143.1  44.4  154  163  166     A     A     A    1    1
    ## 702  006440 TRI2PL  45.4 106.4  154  160  160     A     A     A    1    1
    ## 703  007471 COCCPA  81.0  62.5  154  157  170     A     A     A    1    1
    ## 704  007806 CAL2CA  97.8 159.5  154  155  155     A     A     A    1    1
    ## 705  011201 CEDROD 187.1  53.6  154  162  152     A     A     A    1    1
    ## 706  003726 TRI2PL  24.6 167.5  153  153  153     A     A     A    1    1
    ## 707  003731 TRI2PL  21.9 179.8  153  153  150     A     A     A    1    1
    ## 708  008097 POUTCA  94.2 211.3  153  157  157     A     A     A    1    1
    ## 709  009079 ANTITR  76.0   8.4  153  157  165     A     A     A    1    1
    ## 710  010250 GUARGL 134.3  31.9  153  155  155     A     A     A    1    1
    ## 711  000701 HEISCO   0.8 132.1  152  157  159     A     A     A    1    1
    ## 712  001007 PROTTE   0.6 216.4  152  155  155     A     A     A    1    1
    ## 713  001176 CAVAPL   2.3 257.8  152  156  162     A     A     A    1    1
    ## 714  003534 TRI2PL  39.1 103.1  152  152  152     A     A     A    1    1
    ## 715  006205 TRI2PL  46.1  57.6  152  142  142     A     A     A    1    1
    ## 716  007944 COPAAR  85.9 193.0  152  166  171     A     A     A    1    1
    ## 717  008587 CAVAPL 188.2  28.7  152   48   48     A     A     A    1    1
    ## 718  011063 ANDIIN 179.7  59.7  152  152  150     A     A     A    1    1
    ## 719  007671 TRI2PL  83.7 139.6  151  173  173     A     A     A    1    1
    ## 720  007764 TRI2PL  86.4 154.8  151  155  155     A     A     A    1    1
    ## 721  009245 TRI2PL  60.2  63.1  151  152  152     A     A     A    1    1
    ## 722  011341 BROSAL 187.4  90.3  151  160  157     A     A     A    1    1
    ## 723  001531 TRIPCU 100.1  43.4  150  160  163     A     A     A    1    1
    ## 724  001634 CAL2CA 116.2  44.7  150  155  155     A     A     A    1    1
    ## 725  002293 POSOLA 153.8  73.1  150  155  157     A     A     A    1    1
    ## 726  003255 TRI2PL  30.1  40.2  150  160  162     A     A     A    1    1
    ## 727  007518 ALBIAD  96.3  70.5  150  168  176     A     A     A    1    1
    ## 728  008293 DENDAR  90.5 247.4  150  150  150     A     A     A    1    1
    ## 729  010188 GUARGL 139.8  16.8  150  151  156     A     A     A    1    1
    ## 730  010189 CAL2CA 135.2  10.3  150  168  179     A     A     A    1    1
    ## 731  010879 GUARGL 160.1   4.9  150  151  151     A     A     A    1    1
    ## 732  006259 TRI2PL  58.9  53.4  149  149  151     A     A     A    1    1
    ## 733  006411 CAL2CA  59.4  87.3  149  150  150     A     A     A    1    1
    ## 734  006720 PROTTE  52.3 166.3  149  163  164     A     A     A    1    1
    ## 735  010376 BUNCOD 120.7  65.8  149  150  153     A     A     A    1    1
    ## 736  010959 GUARGL 160.4  23.6  149  149  145     A     A     A    1    1
    ## 737  011034 PROTTE 165.1  50.2  149  149  147     A     A     A    1    1
    ## 738  000670 AST2GR  18.4 112.1  148  149  149     A     A     A    1    1
    ## 739  000958 SWARS1   7.5 180.9  148  157  158     A     A     A    1    2
    ## 740  001211 DALBRE  17.5 258.7  148  159  153     A     A     A    1    1
    ## 741  001567 AST2GR 105.4  49.5  148  157  172     A     A     A    1    2
    ## 742  002027 GUARGL 147.6   7.3  148  151  151     A     A     A    1    1
    ## 743  002147 FICUBU 143.1  41.9  148  148  128     A     A     A    1    1
    ## 744  007215 CAL2CA  46.1 299.5  148  152  151     A     A     A    1    1
    ## 745  008176 TRI2PL  94.2 234.2  148  152  152     A     A     A    1    1
    ## 746  008187 BROSAL  96.3 236.3  148  160  170     A     A     A    1    1
    ## 747  001758 BURSSI 110.1  88.8  147  147   -1     A     A     D    1    1
    ## 748  003145 ANTITR  32.7  20.7  147  157  157     A     A     A    1    1
    ## 749  007825 TRI2PL  81.0 160.6  147  158  158     A     A     A    1    1
    ## 750  008417 CAL2CA  96.8 271.7  147  147  147     A     A     A    1    1
    ## 751  010015 TRI2PL  62.1 268.1  147  150  152     A     A     A    1    1
    ## 752  000401 TRI2PL  18.0  60.6  146  150  154     A     A     A    1    1
    ## 753  001259 GUARGL   7.2 276.3  146  155  163     A     A     A    1    1
    ## 754  006457 TRI2PL  59.7 110.9  146  145  145     A     A     A    1    1
    ## 755  010485 TRI2PL 133.4  85.4  146  160  155     A     A     A    1    1
    ## 756  001577 CAL2CA 110.7  44.8  145  150  158     A     A     A    1    1
    ## 757  004085 PROTTE  27.3 229.6  145  153  143     A     A     A    1    1
    ## 758  004132 TRI2PL  23.6 248.5  145  150  148     A     A     A    1    1
    ## 759  010358 RAUVLI 139.4  48.9  145  150  150     A     A     A    1    1
    ## 760  010987 ANNOSP 172.0  24.7  145  149  152     A     A     A    1    1
    ## 761  000448 POSOLA   2.5  96.7  144  144  143     A     A     A    1    1
    ## 762  004105 COPAAR  37.5 234.8  144  156  147     A     A     A    1    1
    ## 763  004122 PROTTE  23.7 240.3  144  145  142     A     A     A    1    1
    ## 764  008058 CAL2CA  84.0 212.4  144  172  173     A     A     A    1    1
    ## 765  009496 CASECO  65.2 151.3  144  147  149     A     A     A    1    1
    ## 766  010320 ANTITR 133.1  49.8  144  167  160     A     A     A    1    1
    ## 767  010475 TRI2PL 129.0  91.9  144  150  150     A     A     A    1    1
    ## 768  000182 CAL2CA  11.0  37.7  143  171  174     A     A     A    1    2
    ## 769  001188 SOROAF  15.3 231.0  143  143  145     A     A     A    1    1
    ## 770  002349 MATAGL 154.7  90.4  143  148  149     A     A     A    1    1
    ## 771  008175 PROTTE  92.1 233.3  143  155  153     A     A     A    1    1
    ## 772  008445 LACIAG  83.1 296.3  143  143  142     A     A     A    1    1
    ## 773  010461 TRI2PL 120.3  95.3  143  151  148     A     A     A    1    1
    ## 774  008364 TRI2PL  83.1 278.0  142   -1   -1     A     D     D    1    0
    ## 775  009671 BROSAL  65.1 187.8  142  161  162     A     A     A    1    1
    ## 776  000605 SWARS1   6.2 119.9  141  146  149     A     A     A    1    1
    ## 777  006098 GUARGL  58.1   4.3  141  147  146     A     A     A    1    1
    ## 778  008420 TRI2PL  99.2 265.6  141  140  135     A     A     A    1    1
    ## 779  000982 SWARS1  16.5 192.1  140  131  131     A     A     A    1    1
    ## 780  001569 AST2GR 108.9  48.4  140  144  145     A     A     A    1    1
    ## 781  003203 ANTITR  36.1  24.5  140  141  141     A     A     A    1    1
    ## 782  003386 COCCPA  20.5  97.0  140  148  150     A     A     A    1    1
    ## 783  003584 PROTTE  34.7 124.7  140  152  150     A     A     A    1    1
    ## 784  006375 TRI2PL  40.2  98.7  140  142  143     A     A     A    1    1
    ## 785  007323 ANTITR  81.4  36.1  140  140  143     A     A     A    1    1
    ## 786  009062 ANTITR  72.2  12.5  140  140  139     A     A     A    1    1
    ## 787  009078 TRI2PL  79.7  11.9  140  144  147     A     A     A    1    1
    ## 788  011124 FARAOC 170.6  72.8  140  139  138     A     A     A    1    1
    ## 789  000112 TRI2PL  17.6   4.4  139  139  139     A     A     A    1    1
    ## 790  003457 TRI2PL  21.2 112.6  139  140  140     A     A     A    1    1
    ## 791  006835 TRI2PL  54.9 187.4  139  156  158     A     A     A    1    1
    ## 792  010120 CAL2CA  73.5 283.1  139  141  142     A     A     A    1    1
    ## 793  003784 CAL2CA  39.1 172.5  138  138  140     A     A     A    1    1
    ## 794  006008 TRI2PL  42.3  18.3  138  152  156     A     A     A    1    1
    ## 795  008466 GUARGL  94.5 291.5  138   -1   -1     A     D     D    1    0
    ## 796  011028 TRI2PL 162.7  57.3  138  147  147     A     A     A    1    1
    ## 797  001508 SLOATE 111.7  23.6  137  142  145     A     A     A    1    1
    ## 798  001774 CAL2CA 116.8  90.2  137  146  146     A     A     A    1    2
    ## 799  004165 SWARS1  30.2 245.3  137  140  139     A     A     A    1    1
    ## 800  008443 POUTCA  84.5 290.0  137  136  140     A     A     A    1    1
    ## 801  001579 CAL2CA 113.3  44.2  136  155  153     A     A     A    1    2
    ## 802  008624 MYRILO 199.3  29.4  136  155  162     A     A     A    1    1
    ## 803  010132 NECTGL  77.5 293.2  136  149  154     A     A     A    1    1
    ## 804  010895 AST2GR 161.5  19.7  136  140  143     A     A     A    1    1
    ## 805  011076 CASECO 163.0  65.4  136   -1   -1     A     D     D    1    0
    ## 806  001413 TRI2PL 105.0  13.9  135  150  149     A     A     A    1    1
    ## 807  001770 STEMGR 116.2  99.7  135  145  148     A     A     A    1    2
    ## 808  002140 PHOECI 158.9  28.2  135   26   27     A     A     A    1    1
    ## 809  003104 ANTITR  21.6  32.2  135  151  149     A     A     A    1    1
    ## 810  003274 ANNOSP  36.4  55.6  135  141  150     A     A     A    1    1
    ## 811  003761 COU2CU  30.8 171.9  135  137  136     A     A     A    1    1
    ## 812  007494 GUARGL  87.1  64.3  135  148  138     A     A     A    1    1
    ## 813  007528 ANDIIN  97.8  63.5  135  135  140     A     A     A    1    1
    ## 814  008357 VITECO  80.4 272.7  135  135  138     A     A     A    1    1
    ## 815  009383 TRI2PL  71.2 100.9  135  143  144     A     A     A    1    1
    ## 816  009680 POSOLA  70.8 184.9  135  145  146     A     A     A    1    1
    ## 817  010240 AST2GR 134.4  22.5  135  137  137     A     A     A    1    1
    ## 818  010491 ANACEX 134.7  86.1  135  135  135     A     A     A    1    1
    ## 819  000326 CUPASY   6.8  74.1  134  138  141     A     A     A    1    1
    ## 820  000967 TRI2PL  12.8 183.4  134  137  137     A     A     A    1    1
    ## 821  001373 SWARS1  17.9 299.5  134  154  160     A     A     A    1    1
    ## 822  001506 ANTITR 112.5  21.1  134  135  134     A     A     A    1    1
    ## 823  007478 TRI2PL  83.6  70.3  134  140  140     A     A     A    1    1
    ## 824  007855 TRI2PL  87.9 160.0  134  140  147     A     A     A    1    1
    ## 825  009167 TRI2PL  64.1  55.1  134  137  138     A     A     A    1    1
    ## 826  009420 NEEADE  69.7 131.4  134  139  139     A     A     A    1    1
    ## 827  000830 TRI2PL  16.1 141.1  133  139  130     A     A     A    1    1
    ## 828  001488 ANTITR 103.5  33.2  133  127  127     A     A     A    1    1
    ## 829  006218 GUARGL  55.2  71.8  133  135  135     A     A     A    1    1
    ## 830  009098 GUARGL  64.6  35.6  133  133  132     A     A     A    1    1
    ## 831  009128  NEEA2  73.4  26.6  133  135  139     A     A     A    1    1
    ## 832  009919 BROSAL  75.0 233.5  133  152  152     A     A     A    1    1
    ## 833  010409 AST2GR 132.5  64.9  133  140  145     A     A     A    1    1
    ## 834  000375 TRI2PL  12.4  79.5  132  135  134     A     A     A    1    1
    ## 835  000809 TRI2PL  12.5 154.8  132  133  133     A     A     A    1    1
    ## 836  001265 BROSAL   8.8 269.5  132  149  157     A     A     A    1    1
    ## 837  001316 SWARS1   3.1 285.9  132  134  135     A     A     A    1    1
    ## 838  001453 TRIPCU 111.9  17.6  132  148  157     A     A     A    1    1
    ## 839  001568 CAL2CA 108.4  48.7  132  135  135     A     A     A    1    1
    ## 840  001633 GUARGL 115.8  41.0  132  139  132     A     A     A    1    1
    ## 841  001751 STEMGR 105.9  83.0  132  137  135     A     A     A    1    1
    ## 842  002181 CAVAPL 147.0  52.2  132  139  145     A     A     A    1    1
    ## 843  007050 CAL2CA  56.3 230.3  132  132  133     A     A     A    1    1
    ## 844  010460 BROSAL 124.7  92.7  132  134  138     A     A     A    1    1
    ## 845  011068 BROSAL 175.4  49.8  132  132  130     A     A     A    1    1
    ## 846  001231 CASECO  19.6 242.5  131   -1   -1     A     D     D    1    0
    ## 847  002015 GUARGL 148.7  16.3  131  132  133     A     A     A    1    1
    ## 848  003040 TRI2PL  29.5  16.4  131  137  140     A     A     A    1    1
    ## 849  009947 DENDAR  64.3 251.3  131  133  138     A     A     A    1    1
    ## 850  000771 FARAOC   3.8 140.1  130  134  155     A     A     A    1    2
    ## 851  004066 SWARS1  24.8 220.1  130  131  131     A     A     A    1    1
    ## 852  006939 AST2GR  54.4 212.8  130  133  131     A     A     A    1    1
    ## 853  007300 ALBIAD  97.7  14.4  130  132  132     A     A     A    1    1
    ## 854  010223 SPONMO 125.3  27.7  130  134  135     A     A     A    1    1
    ## 855  010379 TRIPCU 124.7  69.7  130  130  130     A     A     A    1    1
    ## 856  000143 CASEGU   9.7  38.6  129   32   35     A     A     A    1    1
    ## 857  000183 TRI2PL  14.1  39.1  129  133  133     A     A     A    1    1
    ## 858  000884 TRI2PL  11.0 165.7  129  135  130     A     A     A    1    2
    ## 859  001417 AST2GR 108.0   5.1  129  143  150     A     A     A    1    1
    ## 860  003112 ANTITR  24.7  38.1  129  143  145     A     A     A    1    1
    ## 861  006036 INGAFA  48.2  11.0  129  158  145     A     A     A    1    1
    ## 862  006182 TRIPCU  41.8  43.4  129  129  131     A     A     A    1    1
    ## 863  008202 LACIAG  98.8 230.6  129  130  133     A     A     A    1    1
    ## 864  010035 POSOLA  69.5 264.4  129  134  131     A     A     A    1    1
    ## 865  000118 PROTTE  17.5   3.2  128  157  160     A     A     A    1    1
    ## 866  001379 SWARS1  18.9 296.6  128  139  150     A     A     A    1    1
    ## 867  002243 SWARS2 143.0  69.3  128  135  138     A     A     A    1    1
    ## 868  007717 ANDIIN  95.2 129.5  128  123  124     A     A     A    1    1
    ## 869  010309 BROSAL 130.1  40.9  128  133  138     A     A     A    1    1
    ## 870  000873 TRI2PL   7.4 162.8  127  140  144     A     A     A    1    1
    ## 871  010368 ANNOSP 120.4  60.5  127  128  130     A     A     A    1    1
    ## 872  000139 CAL2CA   6.7  35.2  126  240  247     A     A     A    1    1
    ## 873  003192 SWARS1  39.0  25.3  126  132  135     A     A     A    1    1
    ## 874  011158 CAL2CA 166.3  90.3  126  130  130     A     A     A    1    1
    ## 875  000257 TRI2PL  12.3  58.2  125  140  141     A     A     A    1    1
    ## 876  001516 BROSAL 113.2  36.6  125  141  143     A     A     A    1    1
    ## 877  003393 HEISCO  25.6  91.2  125  133  134     A     A     A    1    1
    ## 878  004256 CHR2CA  37.1 274.2  125  127  125     A     A     A    1    1
    ## 879  007810 TRI2PL  95.8 154.9  125  130  133     A     A     A    1    1
    ## 880  009482 CECRLO  64.8 141.6  125  187  184     A     A     A    1    2
    ## 881  010954 POSOLA 179.9   9.7  125  126  130     A     A     A    1    1
    ## 882  003797 TRI2PL  39.4 161.5  124  125  127     A     A     A    1    1
    ## 883  009604 CASECO  72.2 177.3  124  130  131     A     A     A    1    1
    ## 884  010095 FARAOC  60.6 294.2  124  124  124     A     A     A    1    1
    ## 885  010328 CAL2CA 133.6  55.4  124  125  126     A     A     A    1    1
    ## 886  000924 TRI2PL  17.5 160.3  123  131  131     A     A     A    1    1
    ## 887  003017 ANTITR  23.6  10.5  123  124  123     A     A     A    1    1
    ## 888  003216 ANNOSP  21.1  53.3  123  136  138     A     A     A    1    1
    ## 889  007557 TRI2PL  91.5  83.2  123  130  141     A     A     A    1    1
    ## 890  009332 TRI2PL  67.4  99.9  123  132  134     A     A     A    1    1
    ## 891  010030 TRI2PL  67.9 268.0  123  127  128     A     A     A    1    1
    ## 892  011246 TRI2PL 195.2  48.7  123  128  128     A     A     A    1    1
    ## 893  000608 INGAVE   9.4 118.9  122    0   -1     A     B     D    1    2
    ## 894  001393 AST2GR 104.7   4.0  122  122  123     A     A     A    1    1
    ## 895  006629 BROSAL  53.6 153.7  122  146  146     A     A     A    1    1
    ## 896  006677 CAL2CA  40.3 174.8  122  125  127     A     A     A    1    1
    ## 897  008079 CAL2CA  87.5 200.4  122  124  124     A     A     A    1    1
    ## 898  009304 COU2CU  79.7  77.4  122  127  129     A     A     A    1    1
    ## 899  009942 AST2GR  60.6 246.0  122  123  123     A     A     A    1    1
    ## 900  002010 TRI2PL 144.6  16.0  121  122  130     A     A     A    1    1
    ## 901  002163 TRIPCU 141.5  56.8  121  140  142     A     A     A    1    1
    ## 902  006172 NEEADE  55.6  36.2  121  121  124     A     A     A    1    1
    ## 903  007251 POSOLA  56.2 291.8  121  121  121     A     A     A    1    1
    ## 904  007459 TRIPCU  96.6  49.2  121  136  143     A     A     A    1    1
    ## 905  008355 CAL2CA  82.5 269.8  121  121  121     A     A     A    1    1
    ## 906  001028 PROTTE   8.4 203.8  120   -9  165     A     *     A    1    0
    ## 907  002089 TRI2PL 148.6  22.2  120  129  130     A     A     A    1    1
    ## 908  003106 ANTITR  24.7  33.3  120  123  123     A     A     A    1    1
    ## 909  003189 TRI2PL  35.3  39.2  120  122  121     A     A     A    1    1
    ## 910  007428 CORDAL  92.7  40.8  120   -1   -1     A     D     D    1    0
    ## 911  009246 TRI2PL  63.5  64.7  120  125  124     A     A     A    1    1
    ## 912  009784 TRI2PL  71.2 202.2  120  131  132     A     A     A    1    1
    ## 913  011120 CHR2CA 170.0  68.4  120  123  122     A     A     A    1    1
    ## 914  002187 CASEGU 148.9  40.9  119  124  120     A     A     A    1    1
    ## 915  002203 INGAFA 158.1  57.2  119  128  131     A     A     A    1    1
    ## 916  002204 PROTTE 155.8  58.4  119  122  122     A     A     A    1    1
    ## 917  006999 SWARS1  47.3 226.6  119  120  120     A     A     A    1    1
    ## 918  007704 GUARGL  92.0 136.2  119  123  126     A     A     A    1    1
    ## 919  000539 CAL1WA  19.8  98.7  118  121  121     A     A     A    1    1
    ## 920  000562 TRI2PL  19.3  80.3  118  118   -1     A     A     D    1    1
    ## 921  000867 ANDIIN   9.8 168.8  118  137  138     A     A     A    1    1
    ## 922  000898 TRI2PL  17.4 175.5  118  119  120     A     A     A    1    1
    ## 923  004326 FARAOC  30.1 297.4  118  124  124     A     A     A    1    1
    ## 924  006158 TRI2PL  51.3  30.1  118  135  126     A     A     A    1    1
    ## 925  010276 EUGECO 120.9  47.5  118  118  118     A     A     A    1    1
    ## 926  004141 HEISCO  20.4 255.5  117  117  117     A     A     A    1    1
    ## 927  006501 COU2CU  44.9 136.5  117  117  117     A     A     A    1    1
    ## 928  006580 COU2CU  56.6 134.0  117  108  108     A     A     A    1    1
    ## 929  006872 COU2CU  44.8 206.1  117  118  118     A     A     A    1    1
    ## 930  008578 CAVAPL 185.4  35.9  117  123  124     A     A     A    1    1
    ## 931  009741 PROTTE  62.7 206.2  117  145  146     A     A     A    1    1
    ## 932  001153 CASSEL  14.6 237.2  116  108  110     A     A     A    1    1
    ## 933  001208 CEDROD  13.7 257.6  116  149  154     A     A     A    1    1
    ## 934  001717 CORDAL 116.5  63.1  116  116  117     A     A     A    1    1
    ## 935  006162 AST2GR  51.1  34.0  116  131  131     A     A     A    1    1
    ## 936  007396 AST2GR  80.1  47.7  116  126  132     A     A     A    1    1
    ## 937  008173 BROSAL  91.7 230.8  116  120  122     A     A     A    1    1
    ## 938  008539 POSOLA 191.4  17.6  116  127  131     A     A     A    1    1
    ## 939  000042 ARDIRE  10.3   3.7  115  118  127     A     A     A    1    1
    ## 940  000755 POUTCA  17.6 130.9  115  118  120     A     A     A    1    1
    ## 941  000992 TRI2PL  16.0 181.3  115  116  116     A     A     A    1    1
    ## 942  001278 AST2GR  12.3 264.4  115  116  116     A     A     A    1    1
    ## 943  003137 CASEGU  26.3  21.6  115  122  120     A     A     A    1    1
    ## 944  003846 CAL2CA  29.6 186.3  115  115  115     A     A     A    1    1
    ## 945  006551 TRI2PL  50.5 130.1  115  122  124     A     A     A    1    1
    ## 946  007242 POSOLA  50.0 299.9  115  117  117     A     A     A    1    1
    ## 947  007689 TRI2PL  88.9 129.9  115  129  133     A     A     A    1    1
    ## 948  007881 SWARS1  92.2 179.2  115  117  117     A     A     A    1    1
    ## 949  009112 TRIPCU  69.9  31.3  115  122  123     A     A     A    1    1
    ## 950  009313 TRI2PL  77.5  69.7  115  118  122     A     A     A    1    1
    ## 951  000327 COCCPA   7.9  73.2  114  128  129     A     A     A    1    1
    ## 952  006152 BROSAL  50.3  27.5  114  123  116     A     A     A    1    1
    ## 953  001242 FARAOC   7.6 224.1  113  113  113     A     A     A    1    1
    ## 954  001477 GUARGL 116.8   0.8  113  116  117     A     A     A    1    1
    ## 955  006545 COU2CU  54.7 128.0  113   97   98     A     A     A    1    1
    ## 956  007766 TRI2PL  88.4 145.6  113  120  119     A     A     A    1    1
    ## 957  009119 ANDIIN  69.6  27.4  113  113  113     A     A     A    1    1
    ## 958  000138 SWARS1   7.8  36.0  112  122  127     A     A     A    1    1
    ## 959  001439 TRI2PL 111.7  12.7  112  116  116     A     A     A    1    1
    ## 960  006611 FARAOC  49.7 145.2  112  104  112     A     A     A    1    1
    ## 961  007179 CAL2CA  57.1 265.4  112  110  108     A     A     A    1    1
    ## 962  007347 TRI2PL  90.9  24.1  112  112  118     A     A     A    1    1
    ## 963  010169 AST2GR 132.4   3.4  112  116  116     A     A     A    1    1
    ## 964  000459 AST2GR   9.2  99.8  111  112  112     A     A     A    1    1
    ## 965  001314 XYL1FR   3.6 281.1  111  120  127     A     A     A    1    1
    ## 966  003599 FARAOC  33.5 138.3  111  111  113     A     A     A    1    1
    ## 967  003641 ANDIIN  21.6 152.6  111  112  112     A     A     A    1    1
    ## 968  003930 HIRTAM  37.5 188.7  111  113  118     A     A     A    1    1
    ## 969  004320 SWARS1  30.7 288.5  111  119  118     A     A     A    1    1
    ## 970  006332 COCCPA  50.3  72.3  111  117  117     A     A     A    1    2
    ## 971  006336 GUARGL  54.2  75.9  111  114  114     A     A     A    1    1
    ## 972  007408 TRIPCU  85.4  59.8  111  117  118     A     A     A    1    2
    ## 973  007470 GUARGL  82.2  60.3  111  112  113     A     A     A    1    1
    ## 974  007502 GUARGL  91.2  66.7  111  112  110     A     A     A    1    1
    ## 975  008361 CAL2CA  81.9 272.6  111  111  110     A     A     A    1    1
    ## 976  008427 CAL2CA  95.4 261.1  111  111  111     A     A     A    1    1
    ## 977  009208 CAL2CA  72.5  50.4  111  112  112     A     A     A    1    1
    ## 978  010270 GUARGL 121.9  40.4  111  113  113     A     A     A    1    1
    ## 979  000157 SWARS1  11.5  24.9  110  121  126     A     A     A    1    1
    ## 980  003409 ALBIAD  26.9  83.3  110   -9  108     A     *     A    1    0
    ## 981  004264 XYL1FR  38.6 264.4  110  116  107     A     A     A    1    1
    ## 982  006547 TRI2PL  54.8 126.2  110  108  113     A     A     A    1    1
    ## 983  006816 ANDIIN  48.4 180.1  110  118  121     A     A     A    1    1
    ## 984  006940 IXORFL  54.9 212.7  110  131  135     A     A     A    1    1
    ## 985  007797 CRYOWA  91.5 159.9  110   99   97     A     A     A    1    1
    ## 986  008041 TRI2PL  81.3 206.8  110  115  117     A     A     A    1    1
    ## 987  009222 TRI2PL  77.0  57.5  110   77   79     A     A     A    1    2
    ## 988  009594 SWARS2  70.9 171.9  110  119  122     A     A     A    1    1
    ## 989  010008 POSOLA  63.1 260.9  110  111  111     A     A     A    1    1
    ## 990  010124 FARAOC  70.9 292.9  110  113  113     A     A     A    1    1
    ## 991  010171 COU2CU 133.0   5.5  110  115  115     A     A     A    1    1
    ## 992  000344 ARDIRE   7.2  64.0  109  110  115     A     A     A    1    1
    ## 993  000920 TRI2PL  18.7 169.9  109  109  109     A     A     A    1    1
    ## 994  001201 ALCHCO  14.0 254.3  109  120  121     A     A     A    1    1
    ## 995  003933 CASECO  39.9 181.4  109  112  112     A     A     A    1    1
    ## 996  003948 HEISCO  23.9 201.2  109  120  124     A     A     A    1    1
    ## 997  006425 GUARGL  42.4 114.6  109  108  108     A     A     A    1    1
    ## 998  007442 TRIPCU  91.6  53.9  109  109  112     A     A     A    1    1
    ## 999  007712 FARAOC  95.0 139.4  109  110  112     A     A     A    1    1
    ## 1000 007726 TRI2PL  83.3 144.2  109  117  120     A     A     A    1    1
    ## 1001 008581 CAVAPL 187.9  34.1  109  124  124     A     A     A    1    1
    ## 1002 009516 PROTTE  70.1 143.2  109  139  144     A     A     A    1    1
    ## 1003 009996 TRI2PL  77.6 254.4  109  115  117     A     A     A    1    1
    ## 1004 010131 BROSAL  75.0 299.0  109  123  125     A     A     A    1    1
    ## 1005 010164 TRI2PL 128.9  14.6  109  116  121     A     A     A    1    1
    ## 1006 000124 TRI2PL   2.7  23.5  108  111  111     A     A     A    1    1
    ## 1007 000778 FARAOC   2.7 152.0  108  114  116     A     A     A    1    1
    ## 1008 000870 TRI2PL   5.4 161.0  108  125  110     A     A     A    1    1
    ## 1009 001165 CUPASY   0.1 246.2  108  115  115     A     A     A    1    1
    ## 1010 003078 TRI2PL  35.6  19.4  108  114  115     A     A     A    1    1
    ## 1011 003815 TRI2PL  21.3 193.3  108  111  112     A     A     A    1    1
    ## 1012 003944 TRI2PL  39.4 183.2  108  123  126     A     A     A    1    1
    ## 1013 007101 LUEHSE  40.2 262.1  108  108  107     A     A     A    1    1
    ## 1014 001487 TRI2PL 102.1  33.2  107  120  123     A     A     A    1    2
    ## 1015 001587 AST2GR 112.3  51.5  107  110  112     A     A     A    1    1
    ## 1016 001750 TRIPCU 109.4  88.0  107  118  118     A     A     A    1    2
    ## 1017 003400 CAL2CA  27.6  89.6  107  110  110     A     A     A    1    1
    ## 1018 006820 TRI2PL  49.5 182.5  107  113  115     A     A     A    1    1
    ## 1019 007850 CRYOWA  88.6 165.7  107  108  109     A     A     A    1    1
    ## 1020 009384 TRI2PL  74.4 104.3  107  117  122     A     A     A    1    1
    ## 1021 009721 PROTTE  75.8 188.3  107  112  114     A     A     A    1    1
    ## 1022 010289 TRIPCU 129.0  59.5  107  112  113     A     A     A    1    1
    ## 1023 011016 AST2GR 161.2  43.8  107  110  107     A     A     A    1    1
    ## 1024 001017 PROTTE   9.3 210.5  106  109  112     A     A     A    1    1
    ## 1025 002182 CAL2CA 149.6  54.6  106  106  110     A     A     A    1    1
    ## 1026 002217 NEEADE 155.2  47.4  106  116  107     A     A     A    1    1
    ## 1027 002348 PROTTE 150.4  85.9  106  139  145     A     A     A    1    1
    ## 1028 003057 FARAOC  32.3   6.7  106  104  105     A     A     A    1    1
    ## 1029 006601 COU2CU  41.3 140.6  106  116  107     A     A     A    1    1
    ## 1030 007353 GUARGL  91.0  28.5  106  106  108     A     A     A    1    1
    ## 1031 007511 GUARGL  99.0  75.8  106  106  106     A     A     A    1    1
    ## 1032 007722 ANTITR  95.9 120.1  106  106  106     A     A     A    1    1
    ## 1033 009334 BROSAL  66.6  81.5  106  112  115     A     A     A    1    1
    ## 1034 000130 POUTCA   4.7  29.7  105  112  114     A     A     A    1    1
    ## 1035 000528 TAB1RO  10.1  98.2  105  105  105     A     A     A    1    1
    ## 1036 003460 CUPASY  21.9 115.7  105  110  119     A     A     A    1    1
    ## 1037 007745 TRI2PL  81.9 155.2  105  112  114     A     A     A    1    1
    ## 1038 007818 SLOATE  97.6 147.9  105  106  106     A     A     A    1    1
    ## 1039 008296 GUARGL  94.3 248.5  105  110  110     A     A     A    1    1
    ## 1040 009372 TRI2PL  61.1 107.2  105  115  114     A     A     A    1    1
    ## 1041 010070 POSOLA  77.6 279.7  105  107  108     A     A     A    1    1
    ## 1042 010365 AST2GR 139.3  42.2  105  110  110     A     A     A    1    1
    ## 1043 011145 PROTTE 160.6  83.4  105  105  107     A     A     A    1    1
    ## 1044 001207 ANACEX  14.3 260.0  104  111  111     A     A     A    1    1
    ## 1045 001744 AST2GR 105.0  91.0  104  105  108     A     A     A    1    1
    ## 1046 003942 COU2CU  35.2 184.4  104  107  107     A     A     A    1    1
    ## 1047 004340 CAL2CA  39.5 291.4  104  115  122     A     A     A    1    1
    ## 1048 006123 COU2CU  45.2  39.4  104  112  107     A     A     A    1    1
    ## 1049 006144 SWARS1  46.2  24.1  104  113  114     A     A     A    1    1
    ## 1050 007424 ALBIAD  86.4  44.8  104   -1   -1     A     D     D    1    0
    ## 1051 000998 HEISCO   4.3 205.3  103  108  109     A     A     A    1    1
    ## 1052 004238 SWARS1  27.6 262.8  103  107  107     A     A     A    1    1
    ## 1053 007229 PROTTE  54.3 285.9  103  103  103     A     A     A    1    1
    ## 1054 007298 GUARGL  95.7  19.6  103  108  108     A     A     A    1    1
    ## 1055 007373 GUARGL  97.2  38.7  103  103  106     A     A     A    1    1
    ## 1056 007464 PIT1RU  99.9  46.8  103  106  103     A     A     A    1    1
    ## 1057 007492 TRIPCU  88.4  61.6  103  110  110     A     A     A    1    1
    ## 1058 009124 PROTTE  70.6  21.0  103  113  111     A     A     A    1    1
    ## 1059 010102 FARAOC  60.7 299.0  103  109  112     A     A     A    1    1
    ## 1060 010423 POUTCA 130.6  72.2  103  107  110     A     A     A    1    1
    ## 1061 011195 POSOLA 184.6  48.2  103  121  122     A     A     A    1    1
    ## 1062 002142 BROSAL 140.8  41.9  102  110  115     A     A     A    1    1
    ## 1063 003537 CUPASY  22.4 121.6  102  103   82     A     A     A    1    1
    ## 1064 003611 TRI2PL  37.6 134.6  102   -1   -1     A     D     D    1    0
    ## 1065 004067 PROTTE  22.4 221.0  102  106  106     A     A     A    1    1
    ## 1066 007694 PROTTE  93.4 126.8  102  124  126     A     A     A    1    1
    ## 1067 008452 SWARS1  88.5 299.4  102  107  111     A     A     A    1    1
    ## 1068 000756 FARAOC  15.1 130.1  101  111  115     A     A     A    1    1
    ## 1069 001462 COLUGL 118.9  11.0  101  116  116     A     A     A    1    2
    ## 1070 001615 VITECO 115.7  53.0  101  109  106     A     A     A    1    2
    ## 1071 003303 CAL2CA  24.5  62.9  101   -9  265     A     *     A    1    0
    ## 1072 003496 BROSAL  32.1 108.6  101  102  102     A     A     A    1    1
    ## 1073 004082 SWARS1  25.5 228.2  101  103  102     A     A     A    1    1
    ## 1074 006494 FARAOC  41.7 138.9  101  105  105     A     A     A    1    1
    ## 1075 007785 STEMGR  93.9 150.1  101  102  101     A     A     A    1    1
    ## 1076 000979 TRI2PL  11.3 198.4  100  107  108     A     A     A    1    1
    ## 1077 001398 TRIPCU 104.8  13.5  100  102  102     A     A     A    1    2
    ## 1078 001475 BROSAL 118.9   9.7  100  111  114     A     A     A    1    1
    ## 1079 001705 TRIPCU 116.1  66.7  100  111  101     A     A     A    1    1
    ## 1080 006393 TRI2PL  54.8  85.1  100  101  101     A     A     A    1    1
    ## 1081 006604 POSOLA  44.0 149.4  100  104  104     A     A     A    1    1
    ## 1082 006909 BROSAL  47.0 205.6  100  117  124     A     A     A    1    1
    ## 1083 007841 TRI2PL  89.5 179.9  100  106  107     A     A     A    1    1
    ## 1084 010057 POSOLA  70.6 275.4  100  101  101     A     A     A    1    1
    ## 1085 000200 SWARS1  18.3  26.2   99   99  101     A     A     A    1    1
    ## 1086 000214 SWARS1   0.4  42.8   99   96   96     A     A     A    1    1
    ## 1087 003751 HEISCO  27.6 162.1   99  105  109     A     A     A    1    1
    ## 1088 003835 CAL2CA  26.5 192.6   99  105  107     A     A     A    1    1
    ## 1089 004036 TRI2PL  39.7 219.8   99  105  106     A     A     A    1    1
    ## 1090 007203 FARAOC  40.1 296.0   99  104  104     A     A     A    1    1
    ## 1091 007230 PROTTE  51.1 285.1   99  106  106     A     A     A    1    1
    ## 1092 007645 CAL2CA  84.8 123.7   99  100   99     A     A     A    1    1
    ## 1093 008595 PICRLA 192.7  22.5   99  105  107     A     A     A    1    1
    ## 1094 01093A COU2CU   0.7 228.7   99  100  101     A     A     A    1    1
    ## 1095 001503 TRIPCU 109.2  23.0   98  107  112     A     A     A    1    1
    ## 1096 008577 PIPERE 186.0  37.6   98  109  111     A     A     A    1    1
    ## 1097 008601 MACRGL 192.4  36.0   98  101  104     A     A     A    1    1
    ## 1098 009322 TRI2PL  60.8  83.6   98  102  101     A     A     A    1    1
    ## 1099 010985 ALSEBL 172.0  22.3   98  115  106     A     A     A    1    1
    ## 1100 011303 CAL2CA 190.8  75.0   98  100  104     A     A     A    1    1
    ## 1101 001366 MACRGL  10.3 293.1   97  100  102     A     A     A    1    1
    ## 1102 001402 TRI2PL 101.8   5.2   97  105  108     A     A     A    1    1
    ## 1103 006013 COU2CU  40.5  13.8   97  100   99     A     A     A    1    1
    ## 1104 010281 CAL2CA 123.3  53.4   97  106  106     A     A     A    1    1
    ## 1105 010961 IXORFL 160.4  30.2   97  100  100     A     A     A    1    1
    ## 1106 011221 LUEHSP 191.3  49.7   97   -1   -1     A     D     D    1    0
    ## 1107 003272 BURSSI  31.7  59.1   96  100  103     A     A     A    1    1
    ## 1108 006627 POSOLA  50.5 147.7   96   99  100     A     A     A    1    1
    ## 1109 008354 COCCPA  82.3 266.5   96  101  104     A     A     A    1    1
    ## 1110 009818 INGAFA  79.9 218.5   96  117  123     A     A     A    1    1
    ## 1111 000046 AST2GR  13.3   3.3   95   99  102     A     A     A    1    1
    ## 1112 000480 TRI2PL   9.5  87.2   95  106  106     A     A     A    1    1
    ## 1113 001063 SWARS1  16.1 213.6   95  102  105     A     A     A    1    1
    ## 1114 001486 VITECO 100.5  34.2   95   95   97     A     A     A    1    1
    ## 1115 003373 POSOLA  23.3  85.3   95   96   96     A     A     A    1    1
    ## 1116 003577 AST2GR  29.8 124.6   95  108   96     A     A     A    1    1
    ## 1117 007639 ANACEX  82.3 121.4   95   97   97     A     A     A    1    1
    ## 1118 007829 TRI2PL  84.4 168.6   95  108  115     A     A     A    1    1
    ## 1119 009084 CAL2CA  62.1  21.2   95   97   99     A     A     A    1    1
    ## 1120 009114 TRI2PL  65.9  25.1   95   95   -1     A     A     D    1    1
    ## 1121 009154 CUPASY  64.0  42.1   95   23   19     A     A     A    1    1
    ## 1122 009236 PROTTE  79.6  44.4   95  105  109     A     A     A    1    2
    ## 1123 011357 GUARGL 193.5  82.4   95   97   97     A     A     A    1    1
    ## 1124 003135 CAL2CA  27.3  20.1   94  113  105     A     A     A    1    1
    ## 1125 004220 TRI2TU  25.2 271.7   94   95   95     A     A     A    1    1
    ## 1126 007463 CASEGU  99.1  47.1   94   99   99     A     A     A    1    1
    ## 1127 007905 IXORFL  82.4 182.2   94   95   91     A     A     A    1    1
    ## 1128 010216 GUARGL 126.3  34.8   94   97   99     A     A     A    1    1
    ## 1129 001457 BROSAL 114.6  18.7   93  100  103     A     A     A    1    1
    ## 1130 001507 TRI2PL 110.1  20.8   93  100  100     A     A     A    1    2
    ## 1131 002260 ANACEX 147.3  71.4   93   95   95     A     A     A    1    1
    ## 1132 002277 CAL2CA 153.3  60.1   93  100  111     A     A     A    1    1
    ## 1133 003516 GUARGL  36.2 110.5   93   97   96     A     A     A    1    1
    ## 1134 006268 GUARGL  56.3  41.5   93   96   98     A     A     A    1    1
    ## 1135 006377 TRI2PL  47.3  95.4   93   94   94     A     A     A    1    1
    ## 1136 006610 GUARGL  47.3 150.1   93   95   95     A     A     A    1    1
    ## 1137 007075 HEISCO  49.8 245.9   93  100  103     A     A     A    1    1
    ## 1138 007166 COU2CU  50.8 278.0   93  102  104     A     A     A    1    1
    ## 1139 009347 FARAOC  71.3  92.5   93   -1   -1     A     D     D    1    0
    ## 1140 010222 GUARGL 125.2  27.4   93   95   92     A     A     A    1    1
    ## 1141 002009 PROTTE 143.6  11.9   92   99  100     A     A     A    1    1
    ## 1142 003843 POSOLA  25.3 185.6   92   95   95     A     A     A    1    1
    ## 1143 004086 PROTTE  25.3 221.4   92   94   93     A     A     A    1    1
    ## 1144 004278 POSOLA  22.6 290.6   92   95   96     A     A     A    1    1
    ## 1145 006748 BROSAL  57.1 168.7   92  118  112     A     A     A    1    1
    ## 1146 006842 COU2CU  54.1 198.7   92  101  101     A     A     A    1    1
    ## 1147 007515 COU2CU  97.5  79.8   92  100  100     A     A     A    1    1
    ## 1148 007727 TRI2PL  84.7 143.8   92   99   99     A     A     A    1    1
    ## 1149 008338 BROSAL  98.2 248.0   92  107  113     A     A     A    1    1
    ## 1150 009034 CAL2CA  65.9  12.5   92   94   94     A     A     A    1    1
    ## 1151 010133 FARAOC  75.2 288.5   92   98  101     A     A     A    1    1
    ## 1152 010917 CAL2CA 169.1   0.8   92   92   93     A     A     A    1    1
    ## 1153 011062 GUARGL 177.5  59.0   92   95   84     A     A     A    1    1
    ## 1154 011274 SWARS1 187.4  76.4   92   92   98     A     A     A    1    1
    ## 1155 011339 CASESY 183.0  96.7   92  102  104     A     A     A    1    1
    ## 1156 000134 SWARS2   2.2  34.4   91   97   98     A     A     A    1    1
    ## 1157 000196 SWARS1  16.4  33.9   91   95   95     A     A     A    1    1
    ## 1158 000233 CAL2CA   5.3  52.1   91  124  132     A     A     A    1    1
    ## 1159 000749 TRI2PL  13.8 139.5   91   93   94     A     A     A    1    1
    ## 1160 003066 TRI2PL  33.6  19.2   91   94   96     A     A     A    1    1
    ## 1161 003479 PROTTE  25.4 103.6   91  109  101     A     A     A    1    1
    ## 1162 003651 COU2CU  28.8 157.3   91   94   96     A     A     A    1    1
    ## 1163 003748 OENOMA  25.1 161.3   91   92   89     A     A     A    1    1
    ## 1164 004314 POUTCA  31.1 283.0   91   92   92     A     A     A    1    1
    ## 1165 006019 TRI2PL  43.8  18.5   91   96   92     A     A     A    1    1
    ## 1166 006863 BROSAL  56.0 180.2   91  100  104     A     A     A    1    1
    ## 1167 008310 IXORFL  93.0 259.2   91   97  104     A     A     A    1    1
    ## 1168 009887 BROSAL  67.5 224.8   91   96  101     A     A     A    1    1
    ## 1169 000617 AST2GR   5.9 114.8   90   91   91     A     A     A    1    1
    ## 1170 000804 TRI2PL  12.4 140.7   90   -1   -1     A     D     D    1    0
    ## 1171 001238 LUEHSE   3.3 263.2   90  107  119     A     A     A    1    1
    ## 1172 001392 BROSAL 101.9   0.5   90   94   96     A     A     A    1    1
    ## 1173 003477 AST2GR  27.5 100.8   90   94   95     A     A     A    1    1
    ## 1174 003590 LINDLA  33.8 126.3   90   90   90     A     A     A    1    1
    ## 1175 006325 PROTTE  51.6  61.2   90   93   92     A     A     A    1    1
    ## 1176 007260 GUARGL  83.6   4.8   90   92   95     A     A     A    1    1
    ## 1177 008541 MYRCGA 193.8  18.9   90  104  108     A     A     A    1    1
    ## 1178 009045 SPONRA  65.7   1.3   90   92   -1     A     A     D    1    1
    ## 1179 010067 TRI2PL  78.8 275.3   90  100  100     A     A     A    1    1
    ## 1180 000394 TRIPCU  18.1  68.6   89   88   88     A     A     A    1    1
    ## 1181 000929 SWARS1   3.6 186.1   89   89   90     A     A     A    1    1
    ## 1182 001436 TRI2PL 112.4  11.2   89   94   97     A     A     A    1    1
    ## 1183 001773 AST2GR 119.8  90.9   89   91   92     A     A     A    1    1
    ## 1184 002020 BROSAL 145.9  13.4   89   97   97     A     A     A    1    1
    ## 1185 006339 PICRLA  53.6  79.9   89   91   91     A     A     A    1    2
    ## 1186 006964 POSOLA  57.5 201.5   89  104  106     A     A     A    1    1
    ## 1187 007223 FARAOC  46.7 280.4   89   88   90     A     A     A    1    1
    ## 1188 007835 POSOLA  83.8 175.3   89   99   97     A     A     A    1    1
    ## 1189 008465 HEISCO  94.1 288.5   89   94   97     A     A     A    1    1
    ## 1190 009115 TRI2PL  66.6  26.4   89   91  101     A     A     A    1    1
    ## 1191 010075 TRI2PL  76.2 274.2   89   92   92     A     A     A    1    1
    ## 1192 010307 GUARGL 128.8  42.2   89   92   93     A     A     A    1    1
    ## 1193 000529 CUPASY  12.4  99.8   88   89   89     A     A     A    1    1
    ## 1194 003422 POSOLA  31.9  92.4   88   95   97     A     A     A    1    1
    ## 1195 003515 GUARGL  36.2 115.1   88   91   91     A     A     A    1    1
    ## 1196 003806 SWARS1  24.2 185.7   88   88   87     A     A     A    1    1
    ## 1197 006933 COU2CU  54.8 201.9   88  104   93     A     A     A    1    2
    ## 1198 007091 FARAOC  51.7 254.8   88   88   89     A     A     A    1    2
    ## 1199 007348 GUARGL  94.1  23.8   88   89   89     A     A     A    1    1
    ## 1200 007365 NECTMA  93.5  36.4   88   88  100     A     A     A    1    1
    ## 1201 007684 TRI2PL  89.0 126.1   88  100  101     A     A     A    1    1
    ## 1202 008109 COPAAR  95.1 215.7   88   86   85     A     A     A    1    1
    ## 1203 008580 PIPERE 188.3  30.0   88   88   88     A     A     A    1    1
    ## 1204 009875 TRI2PL  68.6 231.0   88   90   90     A     A     A    1    1
    ## 1205 010335 TRIPCU 132.2  59.2   88   90   94     A     A     A    1    1
    ## 1206 002339 LONCLA 148.2  86.9   87   92   90     A     A     A    1    1
    ## 1207 003284 TRI2PL  37.6  49.1   87   97   86     A     A     A    1    1
    ## 1208 003606 MANGIN  35.6 132.3   87   92   88     A     A     A    1    1
    ## 1209 004104 SOROAF  38.2 230.2   87   87   82     A     A     A    1    1
    ## 1210 006479 COU2CU  43.8 123.4   87   90   91     A     A     A    1    1
    ## 1211 010101 FARAOC  60.6 297.8   87   91   91     A     A     A    1    1
    ## 1212 010377 TRIPCU 121.9  69.8   87   87   88     A     A     A    1    1
    ## 1213 011015 ANACEX 179.9  24.6   87   99  103     A     A     A    1    1
    ## 1214 011180 SWARS1 173.0  98.3   87   87   85     A     A     A    1    1
    ## 1215 000838 SWARS1   0.3 166.4   86   95   96     A     A     A    1    1
    ## 1216 003281 PROTTE  38.4  45.2   86   91   89     A     A     A    1    1
    ## 1217 007137 COU2CU  48.5 269.5   86   93   93     A     A     A    1    1
    ## 1218 007277 GUARGL  86.1  18.1   86   85   85     A     A     A    1    1
    ## 1219 007484 FARAOC  82.0  79.3   86   91   92     A     A     A    1    1
    ## 1220 008227 FARAOC  82.5 242.5   86   93   95     A     A     A    1    1
    ## 1221 009094 COCCPA  61.6  30.7   86   90   92     A     A     A    1    1
    ## 1222 009310 TRI2PL  75.2  66.1   86   87   88     A     A     A    1    1
    ## 1223 009948 LACIAG  61.3 250.1   86   94   95     A     A     A    1    1
    ## 1224 010049 GUARGL  73.1 268.0   86   86   86     A     A     A    1    1
    ## 1225 010089 GUARGL  62.1 289.8   86   87   87     A     A     A    1    1
    ## 1226 010118 PROTTE  71.7 284.3   86   87   87     A     A     A    1    1
    ## 1227 010979 PIPERE 167.7  20.2   86   87   86     A     A     A    1    1
    ## 1228 011008 AST2GR 176.7  38.3   86   93   94     A     A     A    1    1
    ## 1229 003084 INGAFA  37.3   8.7   85  102  105     A     A     A    1    1
    ## 1230 003384 COU2CU  23.8  92.7   85   88   90     A     A     A    1    1
    ## 1231 006597 CAL2CA  58.1 122.2   85   93   -1     A     A     D    1    1
    ## 1232 010098 PROTTE  64.7 291.8   85   89   89     A     A     A    1    1
    ## 1233 010299 GUARGL 128.4  40.5   85   88   88     A     A     A    1    1
    ## 1234 001219 TRIPCU  18.7 245.4   84   86   92     A     A     A    1    1
    ## 1235 001545 NECTMA 101.2  53.4   84    0   -1     A     B     D    1    2
    ## 1236 001602 PSE1SE 116.9  58.6   84   94   94     A     A     A    1    1
    ## 1237 003041 MARGNO  27.4  23.4   84   90   87     A     A     A    1    1
    ## 1238 006167 GUARGL  52.1  39.6   84  115   84     A     A     A    1    1
    ## 1239 006582 COU2CU  59.0 132.3   84   62   63     A     A     A    1    1
    ## 1240 007103 PIPERE  41.3 263.3   84   87   87     A     A     A    1    1
    ## 1241 007319 TRIPCU  80.9  34.9   84  106  105     A     A     A    1    1
    ## 1242 007874 TRI2PL  94.7 170.1   84   90   90     A     A     A    1    1
    ## 1243 011060 POSOLA 172.8  56.9   84   87   87     A     A     A    1    1
    ## 1244 000177 CAL2CA  14.0  36.4   83   76   76     A     A     A    1    1
    ## 1245 001079 HEISCO  18.9 203.6   83   90   94     A     A     A    1    1
    ## 1246 001130 SWARS1  12.0 221.7   83   97   87     A     A     A    1    1
    ## 1247 001144 CHR2CA  12.7 234.7   83   83   83     A     A     A    1    1
    ## 1248 002343 POSOLA 150.1  80.6   83   85   86     A     A     A    1    1
    ## 1249 003417 CHR2CA  31.2  87.6   83   83   -1     A     A     D    1    1
    ## 1250 003658 CAL2CA  29.9 145.3   83   84   82     A     A     A    1    1
    ## 1251 006094 FARAOC  57.7   5.8   83   93   90     A     A     A    1    1
    ## 1252 006653 FARAOC  40.1 160.9   83   84   82     A     A     A    1    1
    ## 1253 007199 FARAOC  43.6 294.1   83   81   84     A     A     A    1    1
    ## 1254 009487 BROSAL  61.3 150.5   83   95   98     A     A     A    1    1
    ## 1255 009547 PROTTE  61.7 162.8   83   90   92     A     A     A    1    1
    ## 1256 010026 TRI2PL  65.3 265.7   83   85   87     A     A     A    1    1
    ## 1257 010174 AST2GR 133.9   8.5   83   88   88     A     A     A    1    1
    ## 1258 010184 COU2CU 137.9  15.4   83   86   92     A     A     A    1    1
    ## 1259 010217 CLAVME 128.3  33.7   83   84   84     A     A     A    1    1
    ## 1260 011176 HEISCO 173.8  94.8   83   96   98     A     A     A    1    1
    ## 1261 000563 TRI2PL  18.7  80.8   82   83   82     A     A     A    1    1
    ## 1262 002292 SLOATE 151.3  73.7   82   85   82     A     A     A    1    1
    ## 1263 006099 CUPASY  59.6   3.3   82   96   85     A     A     A    1    1
    ## 1264 006967 BROSAL  59.7 201.9   82  105  111     A     A     A    1    1
    ## 1265 007368 TRIPCU  92.2  39.9   82   87   82     A     A     A    1    1
    ## 1266 007662 TRI2PL  82.8 133.5   82   83   80     A     A     A    1    1
    ## 1267 008151 FARAOC  88.4 228.0   82   88   91     A     A     A    1    1
    ## 1268 008582 PIPERE 186.1  25.5   82   83   84     A     A     A    1    1
    ## 1269 009249 COU2CU  61.3  67.5   82   82   81     A     A     A    1    2
    ## 1270 010234 AST2GR 130.0  24.8   82   93   15     A     A     A    1    1
    ## 1271 000839 POSOLA   1.7 169.8   81   86   85     A     A     A    1    1
    ## 1272 001088 AST2GR   4.5 225.7   81   83   83     A     A     A    1    2
    ## 1273 001517 CUPASY 111.0  36.3   81   82   81     A     A     A    1    1
    ## 1274 003197 TRI2PL  35.5  29.0   81   85   86     A     A     A    1    1
    ## 1275 004153 SWARS1  26.4 246.6   81   81   88     A     A     A    1    1
    ## 1276 004168 MACRGL  32.8 250.6   81   81   86     A     A     A    1    1
    ## 1277 004323 POSOLA  33.1 294.0   81   90   90     A     A     A    1    1
    ## 1278 006728 FARAOC  51.6 176.4   81   94   95     A     A     A    1    1
    ## 1279 007883 TRI2PL  99.0 176.0   81   91   92     A     A     A    1    1
    ## 1280 008330 LACIAG  99.5 251.6   81   85   85     A     A     A    1    1
    ## 1281 009272 ANNOHA  69.7  65.2   81   82   81     A     A     A    1    1
    ## 1282 009529 PROTTE  79.7 157.0   81  108  113     A     A     A    1    2
    ## 1283 009609 HYMECO  72.9 179.1   81   81   82     A     A     A    1    1
    ## 1284 009678 POSOLA  72.3 181.5   81   97  100     A     A     A    1    1
    ## 1285 010277 ANTITR 124.5  48.7   81   84   84     A     A     A    1    1
    ## 1286 011151 HYMECO 160.8  90.9   81   83   83     A     A     A    1    1
    ## 1287 011369 IXORFL 197.5  90.1   81   82   89     A     A     A    1    1
    ## 1288 002157 TRIPCU 140.1  53.6   80   80   80     A     A     A    1    1
    ## 1289 003553 TRI2PL  21.0 134.2   80   82   84     A     A     A    1    1
    ## 1290 003585 CUPASY  33.6 121.8   80   82   84     A     A     A    1    1
    ## 1291 003615 FARAOC  39.8 128.1   80   80   -1     A     A     D    1    1
    ## 1292 003732 SWARS1  23.6 179.0   80   81   84     A     A     A    1    1
    ## 1293 006409 TRI2PL  58.9  88.7   80   80   82     A     A     A    1    1
    ## 1294 006499 COU2CU  43.7 136.9   80   82   -1     A     A     D    1    1
    ## 1295 006753 HEISCO  40.3 183.7   80   94   97     A     A     A    1    1
    ## 1296 006881 COU2CU  40.6 218.1   80   80   -1     A     A     D    1    1
    ## 1297 007096 BROSAL  57.7 246.3   80   83   85     A     A     A    1    1
    ## 1298 007252 PSYCG3  57.2 294.7   80   -1   -1     A     D     D    1    0
    ## 1299 008454 SWARS1  87.8 290.7   80   86   84     A     A     A    1    1
    ## 1300 009624 PROTTE  76.5 169.5   80   88   88     A     A     A    1    1
    ## 1301 010106 LACIAG  69.1 296.8   80   84   90     A     A     A    1    1
    ## 1302 010146 GUARGL 120.6  15.4   80   82   81     A     A     A    1    1
    ## 1303 010231 GUARGL 130.3  20.5   80   81   83     A     A     A    1    1
    ## 1304 011219 PIPERE 194.3  42.0   80   88   80     A     A     A    1    1
    ## 1305 011347 PHOECI 185.2  81.1   80   85   90     A     A     A    1    1
    ## 1306 000723 PROTTE   5.1 133.1   79   83   85     A     A     A    1    1
    ## 1307 000827 FARAOC  17.5 149.4   79   83   84     A     A     A    1    1
    ## 1308 002116 PROTTE 153.5  37.4   79   72   73     A     A     A    1    1
    ## 1309 002360 LACIAG 158.6  81.6   79   83   83     A     A     A    1    1
    ## 1310 003082 COU2CU  38.2  10.7   79   81   83     A     A     A    1    1
    ## 1311 003839 CHR2CA  29.7 191.5   79   82   82     A     A     A    1    1
    ## 1312 006550 CUPASY  53.1 130.1   79   -9   83     A     *     A    1    0
    ## 1313 006740 TRI2PL  57.3 177.0   79   85   85     A     A     A    1    1
    ## 1314 006980 TRI2PL  40.9 228.7   79   79   80     A     A     A    1    1
    ## 1315 007072 FARAOC  45.3 254.1   79   76   75     A     A     A    1    1
    ## 1316 007125 FARAOC  45.6 278.6   79   80   80     A     A     A    1    1
    ## 1317 007430 RAUVLI  90.1  42.7   79   81   88     A     A     A    1    1
    ## 1318 007851 PROTTE  85.3 166.4   79   79   78     A     A     A    1    1
    ## 1319 007893 NEEADE  97.7 165.1   79   79   82     A     A     A    1    1
    ## 1320 008075 PICRLA  85.8 212.2   79   84   84     A     A     A    1    1
    ## 1321 008082 IXORFL  85.6 203.5   79   81   83     A     A     A    1    1
    ## 1322 008558 LACIAG 182.9  22.7   79   90   90     A     A     A    1    1
    ## 1323 009186 TAB1RO  69.2  41.6   79   79   76     A     A     A    1    1
    ## 1324 009352 EUGECO  73.0  98.4   79   83   83     A     A     A    1    1
    ## 1325 009408 COU2CU  62.4 130.6   79   80   80     A     A     A    1    1
    ## 1326 009521 LUEHSE  72.1 145.3   79  129  144     A     A     A    1    1
    ## 1327 010137 AST2GR 123.7   4.8   79   82   85     A     A     A    1    1
    ## 1328 000008 SWARS1   4.1   6.3   78   78   78     A     A     A    1    1
    ## 1329 000777 FARAOC   3.5 153.9   78   80   79     A     A     A    1    1
    ## 1330 001146 TERNTE  13.3 236.1   78   -1   -1     A     D     D    1    0
    ## 1331 001170 PROTTE   3.0 253.5   78   85   85     A     A     A    1    1
    ## 1332 003365 COU2CU  36.5  61.2   78   78   81     A     A     A    1    1
    ## 1333 003452 COU2CU  21.5 109.7   78   78   76     A     A     A    1    1
    ## 1334 006291 POSOLA  43.1  75.0   78   79   78     A     A     A    1    1
    ## 1335 007145 COU2CU  47.8 263.0   78   79   80     A     A     A    1    1
    ## 1336 007341 GUARGL  85.1  23.8   78   77   77     A     A     A    1    1
    ## 1337 007622 GUARGL  92.7 100.0   78   78   78     A     A     A    1    1
    ## 1338 007884 BROSAL  95.1 177.1   78   81   83     A     A     A    1    1
    ## 1339 008505 LACIAG 183.6  19.3   78   80   83     A     A     A    1    1
    ## 1340 009121 CUPASY  69.5  21.7   78   68   68     A     A     A    1    1
    ## 1341 010373 GENIAM 124.8  64.6   78   79   79     A     A     A    1    1
    ## 1342 001182 AST2GR   6.4 257.2   77   76   77     A     A     A    1    1
    ## 1343 001643 SWARS1 103.9  69.3   77   71   71     A     A     A    1    1
    ## 1344 001707 ANTITR 119.3  67.7   77   89   87     A     A     A    1    1
    ## 1345 001722 ZUELGU 100.8  82.3   77   -1   -1     A     D     D    1    0
    ## 1346 002074 POSOLA 142.9  33.1   77    0   11     A     B     A    1    2
    ## 1347 002223 AST2GR 156.6  44.6   77   77   78     A     A     A    1    1
    ## 1348 003358 CUPASY  36.8  72.7   77   83   81     A     A     A    1    1
    ## 1349 003378 COU2CU  22.2  89.4   77   -1   -1     A     D     D    1    0
    ## 1350 003390 COU2CU  25.9  97.2   77   75   72     A     A     A    1    1
    ## 1351 003671 POSOLA  31.9 140.8   77   78   77     A     A     A    1    1
    ## 1352 003872 ARDIRE  31.9 184.8   77   79   79     A     A     A    1    1
    ## 1353 004109 SWARS1  37.1 226.9   77   69   69     A     A     A    1    1
    ## 1354 006735 FARAOC  58.5 176.2   77   77   77     A     A     A    1    1
    ## 1355 007132 HEISCO  47.3 271.2   77   90   95     A     A     A    1    1
    ## 1356 007136 FARAOC  46.2 269.9   77   84   85     A     A     A    1    1
    ## 1357 007370 CHOMSP  95.3  38.7   77   76   -1     A     A     D    1    1
    ## 1358 007779 TRI2PL  94.3 146.5   77   81   85     A     A     A    1    1
    ## 1359 007836 TRI2PL  84.8 177.7   77   64   66     A     A     A    1    1
    ## 1360 008073 PROTTE  85.8 219.3   77   81   86     A     A     A    1    1
    ## 1361 009750 BROSAL  60.9 216.1   77   87   90     A     A     A    1    1
    ## 1362 010127 GUARGL  73.4 292.5   77   90   80     A     A     A    1    1
    ## 1363 010940 TROPRA 171.0  18.6   77   81   81     A     A     A    1    1
    ## 1364 002000 POSOLA 140.5   0.5   76   81   82     A     A     A    1    1
    ## 1365 002330 COU2CU 148.5  96.3   76   74   75     A     A     A    1    1
    ## 1366 003619 AST2GR  36.8 124.7   76   -1   -1     A     D     D    1    0
    ## 1367 003871 AST2GR  34.0 185.5   76   79   78     A     A     A    1    1
    ## 1368 004174 MACRGL  34.5 252.9   76   78   78     A     A     A    1    1
    ## 1369 006378 GUARGL  49.0  99.5   76   77   78     A     A     A    1    1
    ## 1370 006620 COU2CU  50.7 140.3   76   76   77     A     A     A    1    1
    ## 1371 006797 STERAP  47.1 192.0   76   88   93     A     A     A    1    2
    ## 1372 006971 FARAOC  44.8 222.8   76   74   74     A     A     A    1    1
    ## 1373 007231 FARAOC  50.0 288.6   76   76   76     A     A     A    1    1
    ## 1374 007316 AST2GR  82.8  26.4   76   75   75     A     A     A    1    1
    ## 1375 007350 AST2GR  93.1  21.3   76   76   75     A     A     A    1    1
    ## 1376 007498 AST2GR  92.5  62.6   76   78   78     A     A     A    1    1
    ## 1377 007532 IXORFL  84.0  88.7   76   83   85     A     A     A    1    1
    ## 1378 008243 HEISCO  82.1 255.2   76   86   91     A     A     A    1    1
    ## 1379 008294 PIT1RU  90.2 249.1   76   82   86     A     A     A    1    1
    ## 1380 008533 GUARGL 192.5  14.9   76   77   79     A     A     A    1    1
    ## 1381 009396 IXORFL  77.0 112.9   76   84   87     A     A     A    1    1
    ## 1382 009418 PROTTE  69.9 137.6   76   88   90     A     A     A    1    1
    ## 1383 009949 POSOLA  62.4 256.5   76   80   82     A     A     A    1    1
    ## 1384 010209 AST2GR 122.6  33.7   76   78   78     A     A     A    1    1
    ## 1385 000155 CUPASY   6.7  23.7   75   79   57     A     A     A    1    1
    ## 1386 000381 COU2CU  15.5  78.6   75   87   78     A     A     A    1    1
    ## 1387 000621 TAB1RO   6.0 105.3   75   75   74     A     A     A    1    1
    ## 1388 000728 COCCPA   5.3 128.9   75   79   80     A     A     A    1    1
    ## 1389 000957 SWARS1   9.7 180.9   75   76   77     A     A     A    1    1
    ## 1390 001008 FARAOC   0.1 216.8   75   76   76     A     A     A    1    1
    ## 1391 001277 LUEHSE  10.4 264.0   75   75   73     A     A     A    1    1
    ## 1392 001738 STEMGR 101.3  98.5   75   -1   -1     A     D     D    1    0
    ## 1393 003535 COU2CU  23.8 120.4   75   75   76     A     A     A    1    1
    ## 1394 003982 SWARS1  26.5 211.5   75   82   85     A     A     A    1    1
    ## 1395 004332 FARAOC  36.8 299.9   75   77   78     A     A     A    1    1
    ## 1396 006085 AST2GR  55.5   9.1   75   66   64     A     A     A    1    1
    ## 1397 006473 TRI2PL  57.1 108.1   75   77   78     A     A     A    1    1
    ## 1398 007119 SWARS1  43.8 279.9   75   78   79     A     A     A    1    1
    ## 1399 007210 BROSAL  42.8 297.3   75   76   76     A     A     A    1    1
    ## 1400 007996 FARAOC  93.5 195.3   75   87   89     A     A     A    1    1
    ## 1401 008071 GUARGL  85.0 218.1   75   85   89     A     A     A    1    1
    ## 1402 008571 MICOAR 184.8  38.7   75   87   87     A     A     A    1    1
    ## 1403 009860 INGAFA  62.9 231.2   75   81   81     A     A     A    1    1
    ## 1404 010019 FARAOC  66.5 275.1   75   -1   -1     A     D     D    1    0
    ## 1405 010427 BROSAL 132.6  79.2   75   81   88     A     A     A    1    1
    ## 1406 011298 FARAOC 192.4  67.3   75   80   82     A     A     A    1    1
    ## 1407 000091 CAL2CA  16.7  11.6   74   88   88     A     A     A    1    1
    ## 1408 000449 SWARS1   0.9  95.2   74   75   77     A     A     A    1    1
    ## 1409 001723 POUTCA 104.9  84.8   74   80   80     A     A     A    1    2
    ## 1410 002016 GUARGL 149.5  18.0   74   72   75     A     A     A    1    1
    ## 1411 002169 SAP1SA 149.1  55.7   74   75   75     A     A     A    1    1
    ## 1412 002258 ALIBED 148.1  79.8   74   79   81     A     A     A    1    1
    ## 1413 003020 CASEGU  22.2  10.1   74   72   79     A     A     A    1    2
    ## 1414 003033 TRI2PL  20.3  15.2   74   88   90     A     A     A    1    1
    ## 1415 004033 COU2CU  35.5 217.7   74   76   76     A     A     A    1    2
    ## 1416 006028 CUPASY  46.4  17.7   74   38   -1     A     A     D    1    1
    ## 1417 006088 CUPASY  58.5   9.1   74   74   79     A     A     A    1    1
    ## 1418 006132 TRI2PL  46.7  27.3   74   76   74     A     A     A    1    1
    ## 1419 006211 COU2CU  48.4  54.6   74   76   73     A     A     A    1    2
    ## 1420 006564 COU2CU  50.0 139.2   74   80   75     A     A     A    1    1
    ## 1421 007987 FARAOC  92.4 191.2   74   70   80     A     A     A    1    1
    ## 1422 008538 PIPERE 190.9  17.2   74   18   47     A     A     A    1    1
    ## 1423 008543 COPAAR 194.3  17.8   74   85   85     A     A     A    1    1
    ## 1424 009144 TRIPCU  78.8  34.4   74   -1   -1     A     D     D    1    0
    ## 1425 010080 CUPASY  75.4 261.5   74   80   80     A     A     A    1    1
    ## 1426 010105 PROTTE  69.0 299.4   74   79   80     A     A     A    1    1
    ## 1427 010937 PIPERE 170.8  16.8   74   70    0     A     A     B    1    1
    ## 1428 011009 SWARS1 176.7  38.8   74   74   74     A     A     A    1    1
    ## 1429 000026 COCCPA   6.8  10.5   73   73   74     A     A     A    1    1
    ## 1430 000028 SWARS1   9.7  13.7   73   76   76     A     A     A    1    1
    ## 1431 000748 TRI2PL  11.5 138.2   73   75   71     A     A     A    1    2
    ## 1432 000881 LACIAG  14.4 161.5   73   74   74     A     A     A    1    2
    ## 1433 003760 AST2GR  33.0 170.7   73   73   71     A     A     A    1    1
    ## 1434 004169 MACRGL  31.1 251.8   73   77   83     A     A     A    1    1
    ## 1435 006272 POSOLA  44.9  61.2   73   76   77     A     A     A    1    1
    ## 1436 006701 SLOATE  49.1 173.8   73   85   85     A     A     A    1    1
    ## 1437 006702 EUGEPR  49.9 173.8   73   17   16     A     A     A    1    1
    ## 1438 007495 ALSEBL  93.8  60.6   73   84   88     A     A     A    1    1
    ## 1439 008266 LACIAG  89.4 252.6   73   73   75     A     A     A    1    1
    ## 1440 008632 COU2CU  59.9  64.5   73   76   77     A     A     A    1    1
    ## 1441 009716 TRI2PL  77.5 194.8   73   83   85     A     A     A    1    1
    ## 1442 011134 TRIPCU 172.9  78.2   73    0    0     A     B     B    1    2
    ## 1443 000890 PROTTE  12.5 173.8   72   78   79     A     A     A    1    1
    ## 1444 001046 LACIAG  11.3 207.9   72   71   75     A     A     A    1    1
    ## 1445 001216 BROSAL  17.7 254.4   72   79   85     A     A     A    1    1
    ## 1446 001474 GUARGL 116.9   7.7   72   72   72     A     A     A    1    1
    ## 1447 001655 TRIPCU 108.3  70.0   72   93   94     A     A     A    1    2
    ## 1448 002105 LUEHSE 153.0  33.9   72   73   70     A     A     A    1    2
    ## 1449 003108 ALBIAD  24.4  36.2   72   73   74     A     A     A    1    1
    ## 1450 003204 COU2CU  39.5  22.6   72   82   81     A     A     A    1    1
    ## 1451 004219 ALIBED  28.9 270.3   72   72   70     A     A     A    1    2
    ## 1452 006137 COU2CU  48.2  29.4   72   74   67     A     A     A    1    1
    ## 1453 006277 COU2CU  40.1  63.5   72   81   78     A     A     A    1    2
    ## 1454 006934 FARAOC  52.4 205.7   72   73   74     A     A     A    1    1
    ## 1455 007102 PIPERE  40.4 263.3   72   64   64     A     A     A    1    1
    ## 1456 007514 TRIPCU  96.2  75.1   72   76   76     A     A     A    1    1
    ## 1457 007640 TRI2PL  80.0 123.9   72   72   72     A     A     A    1    1
    ## 1458 007706 SOROAF  90.1 139.4   72   74   74     A     A     A    1    1
    ## 1459 007731 TRI2PL  80.6 146.2   72   70   70     A     A     A    1    1
    ## 1460 007738 ALBIAD  82.3 146.5   72   22   22     A     A     A    1    1
    ## 1461 008153 SOROAF  87.9 223.1   72   73   73     A     A     A    1    1
    ## 1462 008561 PIPERE 181.1  27.0   72   16   17     A     A     A    1    1
    ## 1463 008621 PIPERE 198.0  27.4   72   72   44     A     A     A    1    1
    ## 1464 009232 POSOLA  78.0  49.7   72   79   82     A     A     A    1    1
    ## 1465 009823 ADE1TR  75.4 210.2   72   72   75     A     A     A    1    2
    ## 1466 010199 GUARGL 139.4   3.3   72   73   75     A     A     A    1    1
    ## 1467 010513 BROSAL 139.8  97.3   72   75   77     A     A     A    1    1
    ## 1468 011279 SLOATE 187.4  71.2   72   68   70     A     A     A    1    1
    ## 1469 000201 CAL2CA  15.8  25.4   71   64   64     A     A     A    1    1
    ## 1470 000848 HEISCO   4.5 179.0   71   75   78     A     A     A    1    1
    ## 1471 001183 FARAOC   6.4 253.8   71   -1   -1     A     D     D    1    0
    ## 1472 002041 PIPERE 157.3  12.5   71   72   73     A     A     A    1    1
    ## 1473 004057 CUPASY  36.9 208.8   71   75   75     A     A     A    1    1
    ## 1474 004072 SOROAF  22.4 225.2   71   72   75     A     A     A    1    1
    ## 1475 006514 ALIBED  48.7 131.2   71   -1   -1     A     D     D    1    0
    ## 1476 006874 CUPASY  40.7 212.5   71   74   74     A     A     A    1    1
    ## 1477 007489 TRIPCU  89.5  73.2   71   69   71     A     A     A    1    1
    ## 1478 007729 TRI2PL  84.3 145.2   71   -1   -1     A     D     D    1    0
    ## 1479 008142 PIPERE  89.1 239.9   71   73   81     A     A     A    1    1
    ## 1480 008286 CHR2CA  89.1 243.5   71   72   75     A     A     A    1    1
    ## 1481 008513 CASESY 185.0  13.4   71   81   85     A     A     A    1    1
    ## 1482 009130 GUARGL  71.2  34.8   71   71   72     A     A     A    1    1
    ## 1483 009142 AST2GR  75.7  31.4   71   71   72     A     A     A    1    1
    ## 1484 009386 CUPASY  71.8 111.2   71   72   73     A     A     A    1    1
    ## 1485 010010 FARAOC  62.7 264.4   71   73   73     A     A     A    1    1
    ## 1486 010894 GENIAM 160.4  19.8   71   77   79     A     A     A    1    1
    ## 1487 010935 PIPERE 170.3  15.8   71   71   70     A     A     A    1    1
    ## 1488 000390 ANTITR  17.6  72.9   70   88   92     A     A     A    1    2
    ## 1489 000913 HEISCO  17.0 166.5   70   77   80     A     A     A    1    1
    ## 1490 001085 SOROAF   1.5 223.6   70   68   69     A     A     A    1    1
    ## 1491 001421 TRIPCU 112.6   2.5   70   72   70     A     A     A    1    2
    ## 1492 001589 TRIPCU 113.7  54.3   70   81   83     A     A     A    1    2
    ## 1493 003401 BROSAL  28.5  89.9   70   70   69     A     A     A    1    1
    ## 1494 003406 COU2CU  26.7  81.4   70   70   -1     A     A     D    1    1
    ## 1495 003499 SOROAF  34.3 109.3   70   75   73     A     A     A    1    1
    ## 1496 003603 COU2CU  38.8 136.3   70   70   71     A     A     A    1    1
    ## 1497 003703 COPAAR  37.5 145.3   70   70   71     A     A     A    1    1
    ## 1498 004232 PIPERE  29.6 269.9   70   71   73     A     A     A    1    2
    ## 1499 006490 COU2CU  41.9 133.2   70   79   62     A     A     A    1    1
    ## 1500 006773 FARAOC  40.0 194.7   70   70   70     A     A     A    1    1
    ## 1501 006801 BROSAL  48.3 186.4   70   10   68     A     A     A    1    2
    ## 1502 007177 MACRGL  59.9 272.1   70   77   80     A     A     A    1    1
    ## 1503 007222 BROSAL  47.4 286.9   70   70   70     A     A     A    1    1
    ## 1504 007400 TRIPCU  83.6  55.1   70   70   69     A     A     A    1    1
    ## 1505 007658 PICRLA  82.4 127.7   70   70   71     A     A     A    1    1
    ## 1506 008276 FARAOC  89.3 248.6   70   77   77     A     A     A    1    1
    ## 1507 009126 GUARGL  71.4  24.1   70   70   68     A     A     A    1    1
    ## 1508 009361 TRI2PL  79.1  89.8   70   77   78     A     A     A    1    1
    ## 1509 009524 ALIBED  73.6 157.2   70   70   70     A     A     A    1    1
    ## 1510 009762 INGAFA  68.4 213.2   70   74   77     A     A     A    1    1
    ## 1511 009824 FARAOC  76.0 211.6   70   73   73     A     A     A    1    1
    ## 1512 000011 CAL2CA   4.0  13.2   69   69   65     A     A     A    1    1
    ## 1513 000591 AST2GR   2.4 113.5   69   67   66     A     A     A    1    1
    ## 1514 002030 TRIPCU 154.8   2.4   69   60   63     A     A     A    1    1
    ## 1515 002073 TRIPCU 142.9  32.5   69   71   69     A     A     A    1    1
    ## 1516 002216 BROSAL 155.8  47.2   69   69   73     A     A     A    1    1
    ## 1517 003597 BROSAL  34.1 131.8   69   60   62     A     A     A    1    1
    ## 1518 003711 FARAOC  36.4 143.4   69   76   76     A     A     A    1    1
    ## 1519 003922 FARAOC  37.0 192.0   69   73   75     A     A     A    1    1
    ## 1520 004022 COPAAR  30.8 216.7   69   72   71     A     A     A    1    1
    ## 1521 004316 COU2CU  34.6 284.8   69   -1   -1     A     D     D    1    0
    ## 1522 006405 TRI2PL  56.3  91.8   69   68   70     A     A     A    1    1
    ## 1523 006593 FARAOC  59.4 129.9   69   61   59     A     A     A    1    1
    ## 1524 006786 COPAAR  45.3 197.2   69   69   69     A     A     A    1    1
    ## 1525 006827 TRI2PL  51.0 183.8   69   73   75     A     A     A    1    1
    ## 1526 006844 GUARGL  58.7 196.4   69   76   80     A     A     A    1    1
    ## 1527 006935 COU2CU  50.3 206.2   69   73   73     A     A     A    1    1
    ## 1528 006984 FARAOC  44.3 230.6   69   58   62     A     A     A    1    1
    ## 1529 007161 CAL2CA  51.1 274.9   69   -1   -1     A     D     D    1    0
    ## 1530 007377 INGAVE  96.6  33.4   69   68   66     A     A     A    1    1
    ## 1531 007862 BUNCOD  93.6 160.5   69   72   72     A     A     A    1    1
    ## 1532 008014 OENOMA  99.8 193.3   69   92   99     A     A     A    1    1
    ## 1533 008269 ORMOMA  89.3 245.6   69   69   64     A     A     A    1    1
    ## 1534 009257 CUPASY  61.2  79.4   69   71   71     A     A     A    1    1
    ## 1535 009715 MYRCGA  76.5 193.9   69   78   80     A     A     A    1    1
    ## 1536 011394 BURSSI 116.2  74.8   69   72   -1     A     A     D    1    1
    ## 1537 000025 SWARS1   6.0  10.1   68   29   35     A     A     A    1    1
    ## 1538 000101 GUARGL  16.1   8.1   68   69   69     A     A     A    1    1
    ## 1539 000114 MYRCGA  18.8   4.8   68   74   -1     A     A     D    1    1
    ## 1540 000191 TRIPCU  19.7  38.1   68   63   65     A     A     A    1    1
    ## 1541 002099 TRIPCU 151.2  24.4   68   75   67     A     A     A    1    2
    ## 1542 002213 TROPRA 155.6  54.8   68   68   65     A     A     A    1    1
    ## 1543 003195 COU2CU  36.9  25.8   68   69   69     A     A     A    1    1
    ## 1544 003221 GENIAM  23.1  59.2   68   72   72     A     A     A    1    1
    ## 1545 006206 COU2CU  46.5  59.6   68   -1   -1     A     D     D    1    0
    ## 1546 007219 INGAFA  49.0 290.8   68   68   68     A     A     A    1    1
    ## 1547 007281 GUARGL  85.3   2.4   68   67   69     A     A     A    1    1
    ## 1548 008133 CUPASY  84.8 225.9   68   62   66     A     A     A    1    1
    ## 1549 008181 CASESY  92.4 239.4   68   76   81     A     A     A    1    1
    ## 1550 008244 POUTCA  83.1 258.9   68   70   71     A     A     A    1    1
    ## 1551 008324 HIRTAM  96.5 252.8   68   68   65     A     A     A    1    1
    ## 1552 008610 ANTITR 198.9  35.0   68   75   77     A     A     A    1    1
    ## 1553 010884 PIPERE 160.7   9.0   68   69   68     A     A     A    1    1
    ## 1554 010964 GUARGL 162.0  32.3   68   73   70     A     A     A    1    1
    ## 1555 011250 SOROAF 197.0  40.2   68   68   -1     A     A     D    1    1
    ## 1556 000435 TRI2PL   1.6  88.8   67   71   75     A     A     A    1    1
    ## 1557 000612 FARAOC   5.2 111.2   67   73   76     A     A     A    1    1
    ## 1558 000833 COU2CU  19.7 144.6   67   -1   -1     A     D     D    1    0
    ## 1559 001489 ALIBED 103.8  33.4   67   67   64     A     A     A    1    1
    ## 1560 003316 BROSAL  27.6  72.4   67   74   73     A     A     A    1    1
    ## 1561 003487 COU2CU  33.0 103.2   67   70   72     A     A     A    1    1
    ## 1562 003549 COU2CU  23.8 126.8   67   -1   -1     A     D     D    1    0
    ## 1563 006976 FARAOC  44.8 226.3   67   76   78     A     A     A    1    1
    ## 1564 007362 ANNOHA  91.7  32.4   67   62   67     A     A     A    1    1
    ## 1565 007507 COU2CU  92.3  79.4   67   72   74     A     A     A    1    1
    ## 1566 007638 LONCLA  95.0 107.9   67   67   67     A     A     A    1    1
    ## 1567 008642 COU2CU 132.7  65.7   67   67   67     A     A     A    1    1
    ## 1568 009190 AST2GR  68.5  43.9   67   67   67     A     A     A    1    1
    ## 1569 009251 CUPASY  60.6  68.4   67   70   71     A     A     A    1    1
    ## 1570 009260 TRI2PL  63.4  77.9   67   68   -1     A     A     D    1    1
    ## 1571 009928 GUARGL  76.7 227.9   67   23   -1     A     A     D    1    1
    ## 1572 011143 PICRLA 175.5  61.4   67   59   59     A     A     A    1    1
    ## 1573 000120 COCCPA   3.1  20.5   66   69   65     A     A     A    1    1
    ## 1574 000509 SLOATE  12.4  86.2   66   67   66     A     A     A    1    1
    ## 1575 000742 BROSAL  14.8 132.5   66   68   67     A     A     A    1    1
    ## 1576 000950 SOROAF   7.1 192.2   66   67   69     A     A     A    1    1
    ## 1577 001011 NEEADE   4.5 219.5   66   61   60     A     A     A    1    1
    ## 1578 001057 SOROAF  19.4 219.2   66   67   68     A     A     A    1    1
    ## 1579 001781 SWARS1 116.5  80.1   66   68   72     A     A     A    1    1
    ## 1580 006165 TRI2PL  54.2  31.7   66   72   68     A     A     A    1    1
    ## 1581 006433 ALIBED  45.1 110.5   66   70   70     A     A     A    1    1
    ## 1582 006800 COU2CU  49.2 186.2   66   64   64     A     A     A    1    1
    ## 1583 006955 IXORFL  55.4 210.7   66   72   75     A     A     A    1    1
    ## 1584 007128 COU2CU  47.5 279.0   66   66   66     A     A     A    1    1
    ## 1585 007429 MARGNO  91.3  42.3   66   70   68     A     A     A    1    1
    ## 1586 007460 NECTMA  96.5  49.7   66   74   86     A     A     A    1    1
    ## 1587 007600 PICRLA  81.3 100.5   66   66   69     A     A     A    1    2
    ## 1588 008139 TRI2PL  80.3 237.9   66   70   71     A     A     A    1    1
    ## 1589 008620 PIPERE 198.9  32.4   66   73   64     A     A     A    1    1
    ## 1590 009527 FARAOC  75.1 159.7   66   68   68     A     A     A    1    1
    ## 1591 009545 FICUMA  65.2 109.0   66   66   67     A     A     A    1    1
    ## 1592 009766 FARAOC  68.0 205.5   66   70   70     A     A     A    1    1
    ## 1593 010410 PROCCR 133.6  63.5   66   66   67     A     A     A    1    1
    ## 1594 011361 POSOLA 192.7  94.6   66   66   66     A     A     A    1    1
    ## 1595 000141 IXORFL   6.8  39.3   65   76   77     A     A     A    1    1
    ## 1596 000195 CAL2CA  15.8  31.4   65   -9   66     A     *     A    1    0
    ## 1597 000241 AST2GR  13.4  43.3   65   75   81     A     A     A    1    1
    ## 1598 001246 TRI4GA   0.7 268.5   65  143  155     A     A     A    1    1
    ## 1599 001276 BROSAL  12.1 259.9   65   67   65     A     A     A    1    1
    ## 1600 001410 PROTTE 107.4  19.2   65   67   68     A     A     A    1    1
    ## 1601 002137 BROSAL 159.4  27.7   65   65   65     A     A     A    1    1
    ## 1602 002197 SWARS1 151.1  59.8   65   63   64     A     A     A    1    1
    ## 1603 002351 SWARS1 158.7  96.6   65   65   66     A     A     A    1    1
    ## 1604 003092 CUPASY  21.8  21.8   65   56   62     A     A     A    1    1
    ## 1605 003247 GENIAM  27.5  41.4   65   80   84     A     A     A    1    1
    ## 1606 003421 COU2CU  34.5  89.5   65   68   70     A     A     A    1    1
    ## 1607 003467 AST2GR  29.5 119.7   65   69   64     A     A     A    1    1
    ## 1608 003977 SWARS1  27.9 218.3   65   65   66     A     A     A    1    1
    ## 1609 004299 FARAOC  29.3 293.3   65   68   69     A     A     A    1    1
    ## 1610 006250 COU2CU  57.4  59.0   65   70   71     A     A     A    1    1
    ## 1611 006558 FARAOC  53.5 131.9   65   67   61     A     A     A    1    1
    ## 1612 007181 COU2CU  55.9 265.2   65   65   65     A     A     A    1    1
    ## 1613 007480 CUPASY  84.8  73.8   65   66   65     A     A     A    1    1
    ## 1614 007583 COU2CU  96.3  91.9   65   62   67     A     A     A    1    1
    ## 1615 007886 SLOATE  95.1 179.0   65   65   65     A     A     A    1    1
    ## 1616 008464 SWARS1  93.2 289.2   65   70   74     A     A     A    1    1
    ## 1617 008606 PIPERE 194.8  36.5   65   18    0     A     A     B    1    1
    ## 1618 009030 BROSAL  68.6  18.1   65   68   68     A     A     A    1    1
    ## 1619 010282 CAL2CA 123.7  54.0   65   68   70     A     A     A    1    1
    ## 1620 010944 CASESY 179.1  16.2   65   70   71     A     A     A    1    1
    ## 1621 011225 TRI2PL 191.0  55.9   65   69   70     A     A     A    1    1
    ## 1622 000395 LUEHSE  19.9  68.8   64   83   96     A     A     A    1    1
    ## 1623 000431 GUARGL   0.4  87.4   64   67   68     A     A     A    1    1
    ## 1624 000786 TRI2PL   7.8 158.8   64   64   64     A     A     A    1    1
    ## 1625 001109 BROSAL   8.6 239.7   64   62   64     A     A     A    1    1
    ## 1626 001345 PIPERE   9.5 286.4   64   18   17     A     A     A    1    1
    ## 1627 002005 GUARGL 143.9   7.2   64   64   64     A     A     A    1    1
    ## 1628 002135 BROSAL 159.0  25.1   64   70   71     A     A     A    1    2
    ## 1629 002198 TRI2PL 148.2  55.4   64   70   66     A     A     A    1    1
    ## 1630 003139 BUNCOD  26.3  22.0   64   70   75     A     A     A    1    1
    ## 1631 003146 CASEGU  31.4  20.3   64   65   65     A     A     A    1    1
    ## 1632 003858 COU2CU  33.6 180.5   64   56   71     A     A     A    1    1
    ## 1633 004154 COU2CU  26.3 248.6   64   54   51     A     A     A    1    1
    ## 1634 004345 FARAOC  36.7 288.0   64   68   66     A     A     A    1    1
    ## 1635 006051 POUTCA  51.9   3.4   64   70   68     A     A     A    1    1
    ## 1636 006634 TRI2PL  58.6 154.3   64   65   67     A     A     A    1    2
    ## 1637 006836 COU2CU  53.7 188.0   64   61   62     A     A     A    1    1
    ## 1638 007357 TRIPCU  93.8  30.7   64   67   63     A     A     A    1    1
    ## 1639 007733 TRIPCU  81.4 149.8   64   84   86     A     A     A    1    1
    ## 1640 008159 SOROAF  90.1 224.9   64   64   69     A     A     A    1    1
    ## 1641 008618 PIPERE 195.0  31.1   64   38   41     A     A     A    1    1
    ## 1642 009046 SLOATE  66.0   3.6   64   67   70     A     A     A    1    1
    ## 1643 009150 CUPASY  76.7  20.6   64   68   65     A     A     A    1    1
    ## 1644 009939 MACRGL  62.4 243.7   64   65   67     A     A     A    1    1
    ## 1645 010096 FARAOC  62.1 293.7   64   65   66     A     A     A    1    1
    ## 1646 010456 CUPARU 124.5  89.6   64   64   66     A     A     A    1    1
    ## 1647 011092 BROSAL 163.8  71.6   64   69   70     A     A     A    1    1
    ## 1648 000227 COCCPA   6.8  56.9   63   72   74     A     A     A    1    2
    ## 1649 000708 PICRLA   0.5 137.8   63   62   64     A     A     A    1    1
    ## 1650 000891 OENOMA  11.8 173.4   63   64    0     A     A     B    1    1
    ## 1651 001050 PROTTE  13.8 211.0   63   64   64     A     A     A    1    1
    ## 1652 001143 SOROAF  12.7 232.5   63   -1   -1     A     D     D    1    0
    ## 1653 001658 CUPASY 106.5  72.1   63   64   70     A     A     A    1    1
    ## 1654 002219 GUARGL 159.5  48.7   63   66   66     A     A     A    1    1
    ## 1655 003344 IXORFL  34.6  68.9   63   81   81     A     A     A    1    1
    ## 1656 003471 FARAOC  28.1 105.3   63   64   65     A     A     A    1    1
    ## 1657 003565 COU2CU  25.1 138.9   63   65   67     A     A     A    1    1
    ## 1658 006279 COU2CU  41.5  64.9   63   68   68     A     A     A    1    1
    ## 1659 006281 COU2CU  44.4  64.4   63   75   74     A     A     A    1    1
    ## 1660 006554 COU2CU  50.6 131.4   63   65   66     A     A     A    1    1
    ## 1661 006641 TRI2PL  58.3 149.1   63   65   67     A     A     A    1    1
    ## 1662 007107 PIPERE  42.8 264.9   63   64   65     A     A     A    1    1
    ## 1663 007255 CAL2CA  59.9 288.5   63   62   64     A     A     A    1    1
    ## 1664 007967 TRI2PL  85.0 180.3   63   63   65     A     A     A    1    1
    ## 1665 008110 HIRTRA  99.9 217.2   63   36   38     A     A     A    1    1
    ## 1666 008627 PIPERE 196.3  22.1   63   64   65     A     A     A    1    1
    ## 1667 009333 FARAOC  69.8  80.8   63   69   70     A     A     A    1    1
    ## 1668 009438 PHOECI  72.2 130.9   63   72   78     A     A     A    1    1
    ## 1669 011007 AST2GR 176.1  37.6   63   69   60     A     A     A    1    1
    ## 1670 011243 CASECO 199.0  46.6   63   62   63     A     A     A    1    1
    ## 1671 000076 SWARS1  14.4  18.4   62   67   69     A     A     A    1    1
    ## 1672 000099 SWARS1  15.4   5.2   62   63   64     A     A     A    1    1
    ## 1673 000173 AST2GR  11.4  34.0   62   67   66     A     A     A    1    1
    ## 1674 000176 CASEGU  14.7  32.9   62   63   63     A     A     A    1    1
    ## 1675 000211 INGAVE  15.4  21.4   62   62   63     A     A     A    1    1
    ## 1676 000825 CUPASY  16.0 147.1   62   62   62     A     A     A    1    1
    ## 1677 001118 COU2CU   9.9 226.8   62   65   67     A     A     A    1    1
    ## 1678 001708 CASEGU 119.7  61.8   62   68   67     A     A     A    1    1
    ## 1679 002279 ALIBED 153.7  67.6   62   65   63     A     A     A    1    1
    ## 1680 004013 TERMOB  32.8 213.7   62   63   69     A     A     A    1    1
    ## 1681 004069 FARAOC  20.2 224.7   62   67   63     A     A     A    1    1
    ## 1682 004258 MACRGL  35.1 268.0   62   71   76     A     A     A    1    1
    ## 1683 006436 HEISCO  45.0 111.9   62   72   77     A     A     A    1    1
    ## 1684 006739 CASECO  59.6 177.8   62   68   68     A     A     A    1    1
    ## 1685 006859 COU2CU  55.6 187.6   62   71   73     A     A     A    1    1
    ## 1686 006956 COPAAR  58.2 214.7   62   65   64     A     A     A    1    1
    ## 1687 006982 FARAOC  44.9 229.0   62   61   61     A     A     A    1    1
    ## 1688 007077 FARAOC  47.3 240.0   62   56   56     A     A     A    1    1
    ## 1689 007869 BROSAL  92.6 166.2   62   69   72     A     A     A    1    1
    ## 1690 009283 PROTTE  65.9  61.5   62   68   64     A     A     A    1    1
    ## 1691 009391 BROSAL  76.8 116.8   62   67   71     A     A     A    1    1
    ## 1692 009589 EUGECO  74.6 167.5   62   62   62     A     A     A    1    1
    ## 1693 010135 GUARGL  77.8 287.1   62   67   70     A     A     A    1    1
    ## 1694 010268 IXORFL 139.9  24.1   62   67   77     A     A     A    1    2
    ## 1695 010369 ZUELGU 122.3  61.7   62   64   64     A     A     A    1    1
    ## 1696 010375 NEEADE 122.1  66.1   62   66   66     A     A     A    1    1
    ## 1697 011327 IXORFL 197.7  69.9   62   62   64     A     A     A    1    2
    ## 1698 000218 SWARS1   0.4  47.6   61   67   67     A     A     A    1    1
    ## 1699 000661 COU2CU  18.0 115.3   61   64   64     A     A     A    1    1
    ## 1700 001198 BROSAL  13.9 247.3   61   59   60     A     A     A    1    1
    ## 1701 001469 TRIPCU 116.8   5.2   61   67   68     A     A     A    1    2
    ## 1702 001544 TRI2HI 100.8  52.0   61   76   80     A     A     A    1    1
    ## 1703 001600 ANTITR 115.5  58.7   61   61   60     A     A     A    1    1
    ## 1704 001688 INGAVE 113.5  76.2   61   66   61     A     A     A    1    1
    ## 1705 002047 MYRILO 159.6  11.6   61   -1   -1     A     D     D    1    0
    ## 1706 002218 DENDAR 155.9  47.7   61   61   59     A     A     A    1    1
    ## 1707 003119 CUPASY  27.6  30.3   61   59   65     A     A     A    1    1
    ## 1708 003395 AST2GR  25.8  85.3   61   59   59     A     A     A    1    1
    ## 1709 003720 SWARS1  21.2 165.3   61   63   66     A     A     A    1    1
    ## 1710 003735 COU2CU  25.5 179.8   61   62   63     A     A     A    1    1
    ## 1711 003750 COU2CU  26.3 164.3   61   62   67     A     A     A    1    1
    ## 1712 003766 HYMECO  33.6 173.2   61   61   59     A     A     A    1    1
    ## 1713 003830 COU2CU  24.6 197.7   61   64   64     A     A     A    1    1
    ## 1714 004043 TRI2PL  38.8 210.7   61   77   76     A     A     A    1    1
    ## 1715 006030 COU2CU  45.2  13.1   61   63   80     A     A     A    1    1
    ## 1716 006105 COU2CU  45.1  20.7   61   72   72     A     A     A    1    1
    ## 1717 006315 COU2CU  48.3  65.4   61   74   74     A     A     A    1    1
    ## 1718 006907 HEISCO  49.5 205.9   61   79   84     A     A     A    1    1
    ## 1719 007009 TRI2PL  49.6 223.6   61   65   70     A     A     A    1    1
    ## 1720 007081 FARAOC  47.5 243.6   61   59   60     A     A     A    1    1
    ## 1721 007144 BROSAL  48.7 262.7   61   62   65     A     A     A    1    1
    ## 1722 007276 TRI2PL  85.5  17.4   61   59   60     A     A     A    1    1
    ## 1723 007399 AST2GR  84.8  51.4   61   63   63     A     A     A    1    1
    ## 1724 007584 FARAOC  95.2  94.9   61   61   66     A     A     A    1    1
    ## 1725 009264 FARAOC  67.9  75.8   61   67   69     A     A     A    1    1
    ## 1726 009443 AST2GR  72.5 133.6   61   64   65     A     A     A    1    1
    ## 1727 009508 PROTTE  66.6 143.5   61   84   88     A     A     A    1    1
    ## 1728 010128 GUARGL  70.6 295.4   61   61   61     A     A     A    1    1
    ## 1729 010413 COU2CU 132.3  65.6   61   62   66     A     A     A    1    1
    ## 1730 000359 STEMGR  12.7  65.0   60   58   65     A     A     A    1    1
    ## 1731 001560 CAL2CA 109.9  45.8   60   61   61     A     A     A    1    1
    ## 1732 001698 CAVAPL 118.6  78.1   60   62   62     A     A     A    1    1
    ## 1733 002361 HEISCO 157.9  84.0   60   66   75     A     A     A    1    1
    ## 1734 003061 CAL2CA  31.3  15.3   60   74   74     A     A     A    1    1
    ## 1735 003083 GUARGL  35.8   9.5   60   50   55     A     A     A    1    1
    ## 1736 003524 COU2CU  38.5 109.6   60   -1   -1     A     D     D    1    0
    ## 1737 003586 POSOLA  33.8 125.8   60   67   66     A     A     A    1    1
    ## 1738 004083 FARAOC  25.4 228.9   60   60   59     A     A     A    1    1
    ## 1739 004344 FARAOC  32.5 289.1   60   62   62     A     A     A    1    1
    ## 1740 006124 ALIBED  45.7  39.2   60   60   60     A     A     A    1    1
    ## 1741 006521 GUARGL  45.6 121.9   60   65   67     A     A     A    1    1
    ## 1742 007538 COU2CU  80.7  96.2   60   65   67     A     A     A    1    1
    ## 1743 008086 FARAOC  94.8 201.8   60   67   69     A     A     A    1    1
    ## 1744 008350 IXORFL  84.2 261.3   60   60   60     A     A     A    1    1
    ## 1745 008492 CHR2CA 181.3   5.1   60   60   60     A     A     A    1    1
    ## 1746 008495 CASESY 180.1  13.6   60   66   69     A     A     A    1    1
    ## 1747 008551 ALIBED 198.6   7.4   60   60   62     A     A     A    1    1
    ## 1748 009501 AST2GR  69.1 148.3   60   73   76     A     A     A    1    1
    ## 1749 009757 ANNOHA  65.2 211.5   60   66   66     A     A     A    1    1
    ## 1750 010385 RAUVLI 120.6  79.8   60   61   62     A     A     A    1    1
    ## 1751 011168 ANNOHA 169.6  82.0   60   64   64     A     A     A    1    1
    ## 1752 011267 CAL2CA 180.9  70.2   60   63   63     A     A     A    1    1
    ## 1753 000184 AST2GR  14.8  39.5   59   68   73     A     A     A    1    1
    ## 1754 000302 TRIPCU   0.7  60.5   59   95  100     A     A     A    1    1
    ## 1755 000613 PROTTE   5.1 110.2   59   68   69     A     A     A    1    1
    ## 1756 001054 PROTTE  13.3 214.0   59   60   63     A     A     A    1    1
    ## 1757 001195 AST2GR  11.8 248.6   59   -1   -1     A     D     D    1    0
    ## 1758 003483 COU2CU  34.5 101.0   59   64   66     A     A     A    1    1
    ## 1759 003707 CUPASY  37.5 140.9   59   62   64     A     A     A    1    1
    ## 1760 004001 POSOLA  32.5 200.4   59   61   63     A     A     A    1    1
    ## 1761 006583 HIRTAM  58.5 132.2   59   70   71     A     A     A    1    1
    ## 1762 006824 COU2CU  51.6 181.3   59   56   58     A     A     A    1    1
    ## 1763 006828 AST2GR  54.9 185.4   59   -1   -1     A     D     D    1    0
    ## 1764 007121 FARAOC  49.4 275.1   59   59   56     A     A     A    1    1
    ## 1765 007402 TRIPCU  81.2  56.7   59   54   59     A     A     A    1    1
    ## 1766 008347 PROTTE  80.9 260.7   59   67   65     A     A     A    1    1
    ## 1767 008598 PIPERE 192.5  30.8   59    0   -1     A     B     D    1    2
    ## 1768 009331 EUGEPR  61.9  99.7   59   60   61     A     A     A    1    1
    ## 1769 009364 PROTTE  75.4  81.0   59   -1   -1     A     D     D    1    0
    ## 1770 009724 SLOATE  79.8 186.3   59   59   60     A     A     A    1    1
    ## 1771 010890 PIPERE 161.5  11.1   59   49   51     A     A     A    1    1
    ## 1772 010990 IXORFL 170.1  28.8   59   80   74     A     A     A    1    1
    ## 1773 011067 POSOLA 179.4  45.2   59   74   74     A     A     A    1    1
    ## 1774 000035 AST2GR   6.7   0.3   58   61   60     A     A     A    1    1
    ## 1775 000171 ALBIAD  12.9  30.0   58   66   68     A     A     A    1    1
    ## 1776 000516 COU2CU  10.1  93.4   58   66   66     A     A     A    1    1
    ## 1777 000519 FARAOC  13.6  94.5   58   62   63     A     A     A    1    1
    ## 1778 000592 FARAOC   4.8 114.8   58   63   65     A     A     A    1    1
    ## 1779 001451 TRIPCU 111.3  16.4   58   58   58     A     A     A    1    1
    ## 1780 001599 GENIAM 115.5  58.4   58   64   62     A     A     A    1    1
    ## 1781 003568 COU2CU  28.4 138.7   58   61   63     A     A     A    1    1
    ## 1782 003998 ANDIIN  34.2 200.4   58   59   60     A     A     A    1    1
    ## 1783 006076 GUARGL  59.8  18.6   58   59   59     A     A     A    1    1
    ## 1784 006621 COU2CU  50.1 143.5   58   58   59     A     A     A    1    1
    ## 1785 006848 COU2CU  57.4 199.8   58   64   64     A     A     A    1    1
    ## 1786 006904 CUPASY  49.4 213.6   58   -9   58     A     *     A    1    0
    ## 1787 007014 FARAOC  54.6 220.1   58   -1   -1     A     D     D    1    0
    ## 1788 007589 BROSAL  95.0  87.1   58   61   62     A     A     A    1    1
    ## 1789 007595 PROTTE  98.8  80.0   58   60   63     A     A     A    1    1
    ## 1790 007861 INGAVE  94.1 160.1   58   58   59     A     A     A    1    1
    ## 1791 008122 ALIBED  95.8 201.8   58   77   78     A     A     A    1    2
    ## 1792 008619 PIPERE 195.1  32.2   58   70   73     A     A     A    1    1
    ## 1793 008622 PIPERE 197.4  28.3   58   57   63     A     A     A    1    1
    ## 1794 009077 CUPASY  75.6  13.6   58   60   63     A     A     A    1    1
    ## 1795 010002 FARAOC  75.6 247.8   58   60   65     A     A     A    1    2
    ## 1796 010115 COU2CU  66.7 284.7   58   61   65     A     A     A    1    1
    ## 1797 010265 TRIPCU 137.8  20.1   58   11   32     A     A     A    1    1
    ## 1798 010501 TRI2PL 133.4  96.5   58   60   62     A     A     A    1    1
    ## 1799 010891 PIPERE 161.3  13.0   58   54   58     A     A     A    1    1
    ## 1800 000153 PROTTE   8.4  20.0   57   78   85     A     A     A    1    1
    ## 1801 000300 EUGEPR   3.1  61.7   57   58   58     A     A     A    1    1
    ## 1802 000380 INGAVE  15.8  75.5   57   61   59     A     A     A    1    1
    ## 1803 000632 PROTTE   6.6 104.1   57   57   58     A     A     A    1    1
    ## 1804 000813 TRI2PL  10.4 156.5   57   59   59     A     A     A    1    1
    ## 1805 000953 TRI2PL  14.4 168.5   57   60   60     A     A     A    1    1
    ## 1806 001171 AEGIPA   4.6 253.8   57   -1   -1     A     D     D    1    0
    ## 1807 001251 AST2GR   0.8 271.3   57   59   62     A     A     A    1    1
    ## 1808 001353 FARAOC  11.2 280.0   57   62   -1     A     A     D    1    1
    ## 1809 001532 AST2GR 101.6  43.5   57   57   56     A     A     A    1    1
    ## 1810 001594 BURSSI 111.9  59.2   57   60   65     A     A     A    1    2
    ## 1811 001626 TRIPCU 117.4  46.8   57   66   65     A     A     A    1    1
    ## 1812 002002 TRIPCU 144.8   1.5   57   57   86     A     A     A    1    1
    ## 1813 003573 COU2CU  25.3 126.3   57   61   63     A     A     A    1    1
    ## 1814 003626 PROTTE  24.8 143.1   57   60   65     A     A     A    1    1
    ## 1815 003667 CUPASY  26.2 142.1   57   42   41     A     A     A    1    1
    ## 1816 003804 IXORFL  24.8 182.3   57   58   57     A     A     A    1    1
    ## 1817 003974 SOROAF  20.0 215.8   57   57   58     A     A     A    1    1
    ## 1818 006884 TRI2PL  44.5 218.9   57   63   66     A     A     A    1    1
    ## 1819 007006 COU2CU  45.2 221.1   57   63   61     A     A     A    1    1
    ## 1820 007113      *  42.3 269.9   57   -1   -1     A     D     D    1    0
    ## 1821 007369 CAL2CA  95.5  35.3   57   58   57     A     A     A    1    1
    ## 1822 007384 TRIPCU  97.8  28.1   57   68   68     A     A     A    1    1
    ## 1823 007435 CASEGU  94.9  43.9   57   57   56     A     A     A    1    1
    ## 1824 007530 PROTTE  81.8  82.9   57   59   59     A     A     A    1    1
    ## 1825 008128 CUPASY  84.5 223.1   57   62   62     A     A     A    1    1
    ## 1826 008184 SOROAF  94.4 239.9   57   60   59     A     A     A    1    1
    ## 1827 009137 AST2GR  74.3  37.1   57   57   56     A     A     A    1    1
    ## 1828 009342 DALBRE  71.5  85.7   57   61   61     A     A     A    1    1
    ## 1829 009553 POUTCA  63.1 166.7   57   65   66     A     A     A    1    1
    ## 1830 010200 AST2GR 123.8  21.2   57   57   57     A     A     A    1    1
    ## 1831 010333 TRI2PL 131.2  58.5   57   59   74     A     A     A    1    1
    ## 1832 010901 CASESY 165.2  15.4   57   65   65     A     A     A    1    1
    ## 1833 010914 PIPERE 169.1   9.0   57   59   61     A     A     A    1    1
    ## 1834 010929 PIPERE 172.6   7.9   57   58   59     A     A     A    1    1
    ## 1835 010951 PIPERE 177.0  13.2   57   57   57     A     A     A    1    1
    ## 1836 011333 BROSAL 195.4  60.3   57   62   63     A     A     A    1    1
    ## 1837 000075 COU2CU  13.0  19.5   56   26   27     A     A     A    1    1
    ## 1838 000387 CAVAPL  15.2  74.1   56   67   68     A     A     A    1    1
    ## 1839 000879 FARAOC  10.8 163.1   56   58   58     A     A     A    1    1
    ## 1840 001497 AST2GR 105.6  34.4   56   56   57     A     A     A    1    1
    ## 1841 002224 NEEADE 159.9  43.3   56   56   51     A     A     A    1    1
    ## 1842 003446 SOROAF  20.3 102.7   56   56   56     A     A     A    1    1
    ## 1843 003899 COU2CU  30.3 196.2   56   61   62     A     A     A    1    1
    ## 1844 003949 FARAOC  22.6 200.8   56   58   59     A     A     A    1    1
    ## 1845 004251 IXORFL  32.8 279.7   56   60   64     A     A     A    1    1
    ## 1846 006637 GUARGL  55.3 147.5   56   58   57     A     A     A    1    1
    ## 1847 006687 AST2GR  47.7 175.7   56   56   59     A     A     A    1    1
    ## 1848 006998 COU2CU  47.9 225.4   56   -1   -1     A     D     D    1    0
    ## 1849 007111 PIPERE  41.6 266.9   56   57   57     A     A     A    1    1
    ## 1850 007114 FARAOC  43.4 270.6   56   51   53     A     A     A    1    1
    ## 1851 007485 SWARS1  82.9  78.9   56   60   60     A     A     A    1    1
    ## 1852 008499 PIPERE 181.2  18.8   56   41   60     A     A     A    1    1
    ## 1853 009031 COU2CU  68.8  10.1   56   66   67     A     A     A    1    1
    ## 1854 009056 ALIBED  74.7   6.4   56   58   -1     A     A     D    1    1
    ## 1855 009295 TRI2PL  74.0  65.4   56   60   62     A     A     A    1    1
    ## 1856 009498 POSOLA  65.4 154.8   56   56   59     A     A     A    1    1
    ## 1857 009727 GUARGL  75.4 182.5   56   70   73     A     A     A    1    1
    ## 1858 010193 POSOLA 139.3   8.3   56   57   59     A     A     A    1    1
    ## 1859 010892 TRIPCU 161.2  17.7   56   61   61     A     A     A    1    1
    ## 1860 010902 TRIPCU 165.5  18.8   56   56   57     A     A     A    1    1
    ## 1861 011014 TRIPCU 175.4  24.6   56   63   67     A     A     A    1    1
    ## 1862 000279 SWARS1  17.7  54.3   55   62   65     A     A     A    1    1
    ## 1863 000660 CUPASY  19.9 116.6   55   24   25     A     A     A    1    1
    ## 1864 001101 TRIPCU   2.1 234.3   55   54   56     A     A     A    1    1
    ## 1865 001252 GUARGL   2.2 277.3   55   57   61     A     A     A    1    2
    ## 1866 001358 PIPEA1  14.1 282.6   55   57   59     A     A     A    1    1
    ## 1867 001361 MACRGL  13.6 286.7   55   65   66     A     A     A    1    1
    ## 1868 002095 GUARGL 147.5  24.1   55   55   61     A     A     A    1    1
    ## 1869 003567 TRI2PL  27.8 139.6   55   60   60     A     A     A    1    1
    ## 1870 003648 FARAOC  21.8 158.2   55   58   63     A     A     A    1    1
    ## 1871 003664 PICRLA  29.7 146.4   55   58   58     A     A     A    1    1
    ## 1872 006605 PROTTE  40.8 152.8   55   55   55     A     A     A    1    1
    ## 1873 006733 HIRTAM  53.9 179.9   55   62   64     A     A     A    1    1
    ## 1874 006852 FARAOC  57.5 197.5   55   63   63     A     A     A    1    1
    ## 1875 006920 INGAFA  47.8 204.0   55   -1   -1     A     D     D    1    0
    ## 1876 007268 TRIPCU  84.4  13.2   55   52   58     A     A     A    1    1
    ## 1877 007285 CAVAPL  92.4   5.2   55   56   55     A     A     A    1    1
    ## 1878 007297 COU2CU  97.2  18.0   55   55   59     A     A     A    1    1
    ## 1879 008002 SOROAF  98.3 197.3   55   58   58     A     A     A    1    1
    ## 1880 009223 AST2GR  75.8  58.7   55   55   57     A     A     A    1    1
    ## 1881 009382 ANNOHA  69.8 101.4   55   56   57     A     A     A    1    1
    ## 1882 009531 AST2GR  78.1 150.2   55   70   78     A     A     A    1    1
    ## 1883 009743 FARAOC  60.8 206.1   55   58   54     A     A     A    1    1
    ## 1884 009839 TRI2PL  77.4 200.2   55   55   57     A     A     A    1    1
    ## 1885 010352 ANNOHA 138.4  52.4   55   55   55     A     A     A    1    1
    ## 1886 010885 PIPERE 162.2   9.5   55   57   57     A     A     A    1    1
    ## 1887 010948 PIPERE 175.8  17.4   55   58   58     A     A     A    1    1
    ## 1888 000016 FARAOC   4.0  18.7   54   57   57     A     A     A    1    1
    ## 1889 000570 AST2GR   0.9 100.2   54   53   52     A     A     A    1    1
    ## 1890 000807 FARAOC  13.2 149.7   54   54   54     A     A     A    1    1
    ## 1891 000876 POUTCA  10.7 161.0   54    0   -1     A     B     D    1    2
    ## 1892 001363 MACRGL  13.6 288.6   54   70   72     A     A     A    1    1
    ## 1893 001590 AST2GR 113.4  55.1   54   61   61     A     A     A    1    1
    ## 1894 001649 CUPASY 101.6  79.4   54   53   56     A     A     A    1    1
    ## 1895 001725 FARAOC 102.9  85.5   54   55   56     A     A     A    1    1
    ## 1896 001730 SOROAF 102.2  91.3   54   54   57     A     A     A    1    1
    ## 1897 001767 CLAVME 115.9  96.8   54   58   57     A     A     A    1    1
    ## 1898 001777 CAL2CA 119.2  91.9   54   55   55     A     A     A    1    1
    ## 1899 002029 POSOLA 147.1   7.2   54   55   54     A     A     A    1    1
    ## 1900 002327 PIPERE 143.6  96.0   54   54   56     A     A     A    1    1
    ## 1901 002335 TRIPCU 149.9  97.2   54   54   55     A     A     A    1    1
    ## 1902 003571 PICRLA  28.5 131.7   54   58   58     A     A     A    1    1
    ## 1903 003593 ALIBED  30.5 133.3   54   57   56     A     A     A    1    1
    ## 1904 003733 PICRLA  26.8 177.5   54   65   68     A     A     A    1    1
    ## 1905 004190 FARAOC  39.2 258.2   54   55   56     A     A     A    1    1
    ## 1906 006535 COCCPA  54.1 125.5   54   56   56     A     A     A    1    1
    ## 1907 006709 HEISCO  49.1 160.7   54   68   68     A     A     A    1    1
    ## 1908 006818 COU2CU  49.6 184.6   54   -1   -1     A     D     D    1    0
    ## 1909 006991 FARAOC  49.6 230.3   54   60   60     A     A     A    1    1
    ## 1910 006997 COU2CU  49.2 225.5   54   54   54     A     A     A    1    1
    ## 1911 007241 COU2CU  50.1 298.6   54   56   62     A     A     A    1    1
    ## 1912 007455 PROTTE  99.9  45.8   54   62   62     A     A     A    1    1
    ## 1913 008585 PIPERE 185.5  29.8   54   55   55     A     A     A    1    1
    ## 1914 008615 TROPRA 196.8  38.7   54   64   66     A     A     A    1    1
    ## 1915 009400 TRI2PL  78.6 102.9   54   63   64     A     A     A    1    1
    ## 1916 009886 FARAOC  65.5 224.5   54   55   56     A     A     A    1    1
    ## 1917 010994 GUARGL 170.4  34.0   54   55   55     A     A     A    1    1
    ## 1918 011212 BROSAL 191.9  40.9   54   54   55     A     A     A    1    1
    ## 1919 011321 FARAOC 198.1  74.7   54   54   57     A     A     A    1    1
    ## 1920 000404 SWARS1  15.7  63.3   53   66   68     A     A     A    1    1
    ## 1921 000624 NEEADE   9.8 108.8   53   55   57     A     A     A    1    1
    ## 1922 000645 COU2CU  14.1 104.3   53   54   57     A     A     A    1    1
    ## 1923 000684 COU2CU   3.8 120.5   53   60   66     A     A     A    1    1
    ## 1924 000785 COU2CU   6.6 159.8   53   59   60     A     A     A    1    1
    ## 1925 001628 TRI2PL 119.0  49.8   53   55   60     A     A     A    1    1
    ## 1926 002283 COU2CU   0.4  59.5   53   51   53     A     A     A    1    1
    ## 1927 003089 SLOATE  36.2   4.6   53   53   58     A     A     A    1    1
    ## 1928 003100 CASEGU  21.2  26.7   53   54   54     A     A     A    1    1
    ## 1929 003433 COU2CU  37.5  96.3   53   59   63     A     A     A    1    1
    ## 1930 003476 CHR2CA  31.5  62.6   53   53   52     A     A     A    1    1
    ## 1931 003656 IXORFL  28.6 154.2   53   58   61     A     A     A    1    1
    ## 1932 003668 IXORFL  25.2 144.8   53   56   62     A     A     A    1    1
    ## 1933 003894 POSOLA  33.8 195.3   53   57   60     A     A     A    1    1
    ## 1934 004129 COU2CU  20.6 245.7   53   55   57     A     A     A    1    1
    ## 1935 004253 PROTTE  39.5 279.5   53   56   59     A     A     A    1    1
    ## 1936 006059 ALIBED  53.7   8.3   53   46   47     A     A     A    1    1
    ## 1937 006127 INGAFA  45.4  31.1   53   54   54     A     A     A    1    1
    ## 1938 006265 POSOLA  55.3  41.9   53   53   56     A     A     A    1    1
    ## 1939 006618 COU2CU  47.2 142.5   53   55   54     A     A     A    1    1
    ## 1940 006847 ALIBED  55.2 199.2   53   55   55     A     A     A    1    1
    ## 1941 007018 FARAOC  50.4 223.7   53   56   57     A     A     A    1    1
    ## 1942 007084 FARAOC  52.7 240.5   53   54   55     A     A     A    1    1
    ## 1943 007336 CUPASY  86.8  25.7   53   53   52     A     A     A    1    1
    ## 1944 007497 CUPASY  91.2  62.6   53   55   54     A     A     A    1    1
    ## 1945 007601 TRI2PL  84.8 102.3   53   57   62     A     A     A    1    1
    ## 1946 008054 CUPASY  80.7 213.8   53   55   55     A     A     A    1    1
    ## 1947 008463 SWARS1  93.4 282.9   53   52   53     A     A     A    1    1
    ## 1948 008478 BROSAL  98.7 286.5   53   54   57     A     A     A    1    1
    ## 1949 008535 PROTTE 194.8  11.8   53   58   58     A     A     A    1    1
    ## 1950 008603 PIPERE 192.5  39.9   53   39   43     A     A     A    1    1
    ## 1951 009083 TRIPCU  79.7  58.5   53   53   53     A     A     A    1    1
    ## 1952 009345 GUARGL  73.3  87.4   53   59   59     A     A     A    1    1
    ## 1953 009517 CECRLO  74.5 143.8   53   60   61     A     A     A    1    1
    ## 1954 009995 PROTTE  77.5 254.8   53   53   54     A     A     A    1    1
    ## 1955 010178 POSOLA 133.8  15.3   53   54   54     A     A     A    1    1
    ## 1956 010182 BROSAL 134.8  19.4   53   54   55     A     A     A    1    1
    ## 1957 011209 PIPERE 193.7  40.8   53   57   59     A     A     A    1    1
    ## 1958 011226 TRIPCU 191.3  56.6   53   61   63     A     A     A    1    1
    ## 1959 000136 COU2CU   0.2  35.6   52   54   54     A     A     A    1    1
    ## 1960 000892 BROSAL  13.9 175.4   52   53   53     A     A     A    1    1
    ## 1961 000928 HEISCO   4.5 183.4   52   58   60     A     A     A    1    1
    ## 1962 001120 IXORFL   6.9 219.8   52   49   51     A     A     A    1    1
    ## 1963 001141 SWARS1  14.5 230.5   52   54   54     A     A     A    1    1
    ## 1964 001364 BROSAL  14.7 287.4   52   52   57     A     A     A    1    1
    ## 1965 001551 NEEADE 104.1  59.9   52   52   51     A     A     A    1    1
    ## 1966 003315 FARAOC  29.3  77.5   52   61   63     A     A     A    1    1
    ## 1967 003361 COU2CU  37.6  67.6   52   48   51     A     A     A    1    1
    ## 1968 003644 TRI2PL  24.1 152.7   52   54   52     A     A     A    1    1
    ## 1969 004236 SOROAF  25.2 263.7   52   54   57     A     A     A    1    1
    ## 1970 006107 COU2CU  40.3  27.1   52   57   62     A     A     A    1    1
    ## 1971 006169 PROTTE  58.7  36.3   52   55   57     A     A     A    1    1
    ## 1972 006349 COU2CU  57.4  60.1   52   60   61     A     A     A    1    1
    ## 1973 006429 ANNOHA  41.2 117.4   52   47   49     A     A     A    1    2
    ## 1974 006456 INGAFA  57.5 115.2   52   53   53     A     A     A    1    1
    ## 1975 007104 FARAOC  40.7 264.4   52   55   56     A     A     A    1    1
    ## 1976 007361 TRI2PL  90.6  31.1   52   52   54     A     A     A    1    1
    ## 1977 007575 FARAOC  94.2  93.7   52   52   53     A     A     A    1    1
    ## 1978 007885 ALBIAD  95.3 179.0   52   61   63     A     A     A    1    1
    ## 1979 008205 TRI2PL  95.8 233.0   52   52   56     A     A     A    1    1
    ## 1980 008520 FARAOC 186.0   5.2   52   53   54     A     A     A    1    1
    ## 1981 009100 EUGEPR  61.5  39.5   52   55   53     A     A     A    1    1
    ## 1982 009499 PIPERE  68.9 145.7   52   53   45     A     A     A    1    2
    ## 1983 009530 ALBIAD  79.0 156.9   52   52   53     A     A     A    1    1
    ## 1984 009635 GUARGL  62.2 182.5   52   59   60     A     A     A    1    1
    ## 1985 009793 FARAOC  70.1 207.9   52   56   53     A     A     A    1    1
    ## 1986 010011 BROSAL  62.9 265.5   52   52   56     A     A     A    1    1
    ## 1987 010260 ALIBED 136.9  32.3   52   55   58     A     A     A    1    1
    ## 1988 010343 TRIPCU 137.8  59.8   52   56   73     A     A     A    1    1
    ## 1989 010426 ALIBED 130.3  76.1   52   55   56     A     A     A    1    1
    ## 1990 010477 CAL2CA 127.7  88.4   52   53   53     A     A     A    1    2
    ## 1991 011040 COPAAR 169.2  48.4   52   54   51     A     A     A    1    1
    ## 1992 011273 ANNOHA 188.1  75.7   52   55   58     A     A     A    1    1
    ## 1993 000137 COU2CU   0.2  37.9   51   57   59     A     A     A    1    1
    ## 1994 000393 CAVAPL  17.6  68.8   51   53   55     A     A     A    1    1
    ## 1995 000438 HEISCO   2.7  87.1   51   64   66     A     A     A    1    1
    ## 1996 000565 CASECO  15.0  82.4   51   55   50     A     A     A    1    1
    ## 1997 000654 COU2CU  10.1 112.1   51   52   52     A     A     A    1    2
    ## 1998 000685 FARAOC   3.7 120.1   51   54   57     A     A     A    1    1
    ## 1999 000691 PROTTE   2.8 122.1   51   51   51     A     A     A    1    1
    ## 2000 001255 CASESY   4.0 277.5   51   62   63     A     A     A    1    1
    ## 2001 001334 HIRTAM   4.4 298.8   51   66   69     A     A     A    1    1
    ## 2002 001630 BROSAL 118.5  48.1   51   54   58     A     A     A    1    1
    ## 2003 001719 BROSAL 103.5  80.9   51   55   57     A     A     A    1    2
    ## 2004 002102 GUARGL 151.9  28.5   51   52   53     A     A     A    1    1
    ## 2005 002159 TRIPCU 142.3  53.6   51   61   58     A     A     A    1    1
    ## 2006 003006 BROSAL  24.5   2.1   51   52   -1     A     A     D    1    1
    ## 2007 003096 CAL2CA  24.3  24.8   51   57   61     A     A     A    1    1
    ## 2008 003688 TRI2PL  30.2 154.7   51   53   69     A     A     A    1    1
    ## 2009 004088 FARAOC  27.6 223.8   51   -1   -1     A     D     D    1    0
    ## 2010 004198 MACRGL  37.0 243.3   51   57   57     A     A     A    1    1
    ## 2011 006311 GUARGL  48.0  74.7   51   -1   -1     A     D     D    1    0
    ## 2012 006626 CHR2CA  51.5 147.2   51   52   52     A     A     A    1    1
    ## 2013 006796 ACALDI  45.4 190.7   51   71   72     A     A     A    1    1
    ## 2014 007338 GUARGL  87.7  21.0   51   50   51     A     A     A    1    1
    ## 2015 007349 GUARGL  94.3  24.8   51   51   51     A     A     A    1    1
    ## 2016 007381 TRIPCU  95.3  27.9   51   53   54     A     A     A    1    1
    ## 2017 007454 ANTITR  96.6  54.7   51   56   58     A     A     A    1    1
    ## 2018 007822 BROSAL  99.9 143.5   51   52   53     A     A     A    1    1
    ## 2019 007840 BROSAL  85.6 179.3   51   53   55     A     A     A    1    1
    ## 2020 009696 BROSAL  74.5 192.5   51   51   50     A     A     A    1    1
    ## 2021 009790 FARAOC  72.8 202.0   51   54   58     A     A     A    1    1
    ## 2022 009880 INGAFA  66.1 234.7   51   53   55     A     A     A    1    2
    ## 2023 009940 FARAOC  63.1 245.7   51   54   33     A     A     A    1    1
    ## 2024 010225 CUPASY 125.2  29.9   51   52   52     A     A     A    1    1
    ## 2025 010930 PROTTE 172.7  11.1   51   62   61     A     A     A    1    1
    ## 2026 011215 LACIAG 190.5  42.3   51   52   54     A     A     A    1    1
    ## 2027 011272 CHR2CA 184.8  78.9   51   52   49     A     A     A    1    1
    ## 2028 000159 CAL2CA  14.7  22.4   50   51   51     A     A     A    1    1
    ## 2029 000362 ANTITR  14.0  62.3   50   68   75     A     A     A    1    1
    ## 2030 000386 ANTITR  15.5  73.4   50   85   91     A     A     A    1    1
    ## 2031 000576 PROTTE   2.6 106.3   50    0    0     A     B     B    1    2
    ## 2032 000985 FARAOC  16.0 186.3   50   52   53     A     A     A    1    1
    ## 2033 001062 SOROAF  16.2 213.1   50   64   64     A     A     A    1    1
    ## 2034 001232 MICOBO   0.5 260.5   50   50   -1     A     A     D    1    1
    ## 2035 001549 RAUVLI 103.1  55.1   50   59   59     A     A     A    1    2
    ## 2036 001741 SWARS1 104.7  99.2   50   52   52     A     A     A    1    1
    ## 2037 001746 TRIPCU 108.3  94.3   50   52   50     A     A     A    1    1
    ## 2038 003141 CUPASY  27.1  23.2   50   45   52     A     A     A    1    1
    ## 2039 003639 COU2CU  24.6 147.6   50   50   55     A     A     A    1    1
    ## 2040 003802 COU2CU  22.5 184.9   50   51   51     A     A     A    1    1
    ## 2041 003952 SOROAF  22.1 203.6   50   51   50     A     A     A    1    1
    ## 2042 003992 FARAOC  28.7 209.6   50   57   57     A     A     A    1    1
    ## 2043 006140 SWARS1  44.6 200.6   50   53   58     A     A     A    1    1
    ## 2044 006150 PICRLA  53.2  20.2   50   67   68     A     A     A    1    1
    ## 2045 006943 CUPASY  51.3 215.7   50   48   48     A     A     A    1    2
    ## 2046 007178 SLOATE  57.3 265.2   50   51   50     A     A     A    1    1
    ## 2047 007534 COU2CU  82.8  87.1   50   55   54     A     A     A    1    1
    ## 2048 007751 STEMGR  84.3 158.3   50   50   51     A     A     A    1    1
    ## 2049 007907 HEISCO  82.0 183.1   50   60   62     A     A     A    1    1
    ## 2050 008117 PROTTE  99.9 201.3   50   52   51     A     A     A    1    1
    ## 2051 008545 CAVAPL 199.5  19.1   50   60   62     A     A     A    1    1
    ## 2052 009041 COU2CU  68.8   8.3   50   55   57     A     A     A    1    1
    ## 2053 009625 ALBIAD  77.5 162.6   50   52   53     A     A     A    1    1
    ## 2054 009764 FARAOC  69.3 205.3   50   59   59     A     A     A    1    1
    ## 2055 009978 ALIBED  74.4 259.9   50   55   59     A     A     A    1    1
    ## 2056 010000 BROSAL  79.8 245.9   50   52   55     A     A     A    1    1
    ## 2057 010044 FARAOC  73.7 266.1   50   54   57     A     A     A    1    1
    ## 2058 010187 CUPASY 139.8  18.7   50   52   53     A     A     A    1    1
    ## 2059 010981 NECTGL 168.9  21.5   50   63   67     A     A     A    1    1
    ## 2060 000024 POSOLA   8.6  19.3   49   55   54     A     A     A    1    1
    ## 2061 000082 ACALDI  17.1  17.5   49   23   44     A     A     A    1    1
    ## 2062 000568 BROSAL  18.3  83.1   49   51   51     A     A     A    1    1
    ## 2063 000834 SOROAF   4.3 160.6   49   54   52     A     A     A    1    1
    ## 2064 000922 INGAFA  19.3 168.7   49   50   -1     A     A     D    1    2
    ## 2065 001196 NEEADE  12.8 248.7   49   47   52     A     A     A    1    1
    ## 2066 002085 BROSAL 145.0  27.5   49   49   50     A     A     A    1    1
    ## 2067 002167 BROSAL 143.0  59.6   49   62   63     A     A     A    1    1
    ## 2068 002168 CASECO 142.3  59.2   49   55   55     A     A     A    1    1
    ## 2069 002233 COU2CU 143.8  64.1   49   51   53     A     A     A    1    1
    ## 2070 003480 FARAOC  26.4 104.8   49   50   50     A     A     A    1    1
    ## 2071 003583 FARAOC  32.2 124.8   49   50   51     A     A     A    1    1
    ## 2072 003728 CUPASY  24.0 170.5   49   50   52     A     A     A    1    1
    ## 2073 003860 COU2CU  31.1 183.1   49   52   54     A     A     A    1    1
    ## 2074 006016 ALBIAD  41.3  19.8   49   50   50     A     A     A    1    1
    ## 2075 006179 PICRLA  56.8  24.3   49   68   67     A     A     A    1    1
    ## 2076 006540 HEISCO  51.1 126.0   49   56   60     A     A     A    1    1
    ## 2077 006853 FARAOC  57.6 191.6   49   55   56     A     A     A    1    1
    ## 2078 007017 FARAOC  50.4 221.8   49   53   55     A     A     A    1    1
    ## 2079 007032 COU2CU  50.2 236.8   49   -1   -1     A     D     D    1    0
    ## 2080 007468 PROTTE  99.9  43.7   49   52   53     A     A     A    1    1
    ## 2081 007537 ANNOHA  80.6  94.7   49   54   55     A     A     A    1    1
    ## 2082 007913 TRI2PL  81.5 187.7   49   46   46     A     A     A    1    1
    ## 2083 008125 CUPASY  84.3 221.3   49   53   56     A     A     A    1    1
    ## 2084 009694 PROTTE  70.2 192.4   49   49   54     A     A     A    1    1
    ## 2085 009747 GUARGL  64.1 215.3   49   28   28     A     A     A    1    2
    ## 2086 009794 SWARS2  70.3 209.1   49   49   63     A     A     A    1    1
    ## 2087 009814 CUPASY  77.0 215.9   49   54   54     A     A     A    1    1
    ## 2088 010034 ALIBED  67.7 262.7   49   49   48     A     A     A    1    1
    ## 2089 010062 FARAOC  71.8 279.1   49   52   52     A     A     A    1    1
    ## 2090 010103 FARAOC  66.3 298.8   49   52   52     A     A     A    1    1
    ## 2091 000618 COCCPA   9.8 114.5   48   55   54     A     A     A    1    1
    ## 2092 000662 FARAOC  15.8 118.2   48   -1   -1     A     D     D    1    0
    ## 2093 001016 LACIAG   9.2 218.3   48   52   52     A     A     A    1    1
    ## 2094 001254 BROSAL   2.7 279.0   48   49   54     A     A     A    1    1
    ## 2095 001279 MACRGL  14.0 263.8   48   -1   -1     A     D     D    1    0
    ## 2096 001336 CAVAPL   5.6 295.6   48   -1   -1     A     D     D    1    0
    ## 2097 001575 BROSAL 110.5  41.0   48   48   51     A     A     A    1    1
    ## 2098 002154 ANNOHA 142.5  50.3   48   48   52     A     A     A    1    1
    ## 2099 002264 CASECO 149.2  74.5   48   55   52     A     A     A    1    2
    ## 2100 002355 MATAGL 158.5  85.6   48   49   49     A     A     A    1    1
    ## 2101 004134 FARAOC  21.5 253.5   48   50   51     A     A     A    1    1
    ## 2102 004208 MACRGL  24.3 269.7   48   49   49     A     A     A    1    1
    ## 2103 004321 NEEADE  34.6 290.2   48   54   53     A     A     A    1    1
    ## 2104 006190 SOROAF  44.7  48.5   48   53   54     A     A     A    1    2
    ## 2105 006454 FARAOC  58.7 115.5   48   55   56     A     A     A    1    1
    ## 2106 006559 COU2CU  53.0 130.8   48   53   53     A     A     A    1    1
    ## 2107 006640 EUGEPR  44.6 200.8   48   49   49     A     A     A    1    1
    ## 2108 006686 IXORFL  41.5 177.3   48   54   57     A     A     A    1    1
    ## 2109 006734 FARAOC  58.6 175.4   48   48   48     A     A     A    1    1
    ## 2110 006770 CHR2CA  41.9 190.5   48   69   61     A     A     A    1    1
    ## 2111 006806 ACALDI  45.7 189.1   48   52   53     A     A     A    1    1
    ## 2112 006821 CHR2CA  48.3 182.4   48   48   48     A     A     A    1    2
    ## 2113 006855 FARAOC  55.9 193.5   48   48   48     A     A     A    1    1
    ## 2114 006871 BROSAL  44.0 206.2   48   52   53     A     A     A    1    1
    ## 2115 006937 TRIPCU  51.4 211.7   48   60   60     A     A     A    1    1
    ## 2116 006957 ANNOHA  59.9 212.2   48   52   52     A     A     A    1    1
    ## 2117 007085 FARAOC  50.3 240.1   48   50   55     A     A     A    1    1
    ## 2118 007115 FARAOC  41.1 273.6   48   49   49     A     A     A    1    1
    ## 2119 007122 INGAFA  46.4 275.2   48   50    0     A     A     B    1    1
    ## 2120 007124 COU2CU  46.5 276.7   48   48   46     A     A     A    1    1
    ## 2121 007221 PROTTE  47.1 293.8   48   48   49     A     A     A    1    1
    ## 2122 007239 FARAOC  52.4 294.8   48   51   51     A     A     A    1    1
    ## 2123 007506 CHR2CA  91.1  76.2   48   52   57     A     A     A    1    1
    ## 2124 007550 FARAOC  87.0  87.2   48   54   57     A     A     A    1    1
    ## 2125 007582 COU2CU  95.6  91.2   48   50   51     A     A     A    1    1
    ## 2126 007792 PROTTE  91.5 154.3   48   52   53     A     A     A    1    1
    ## 2127 007857 BROSAL  87.3 161.2   48   54   58     A     A     A    1    1
    ## 2128 007969 ALIBED  88.5 183.2   48   48   48     A     A     A    1    1
    ## 2129 008044 PIT1RU  82.4 209.5   48   -1   -1     A     D     D    1    0
    ## 2130 008316 INGAFA  95.7 258.4   48   48   48     A     A     A    1    1
    ## 2131 008369 ANNOHA  89.9 271.6   48   -1   -1     A     D     D    1    0
    ## 2132 008514 PIPERE 186.3  14.9   48   42   43     A     A     A    1    1
    ## 2133 009101 COU2CU  62.0  38.0   48   52   53     A     A     A    1    1
    ## 2134 009278 TRI2PL  68.2  69.7   48   52   53     A     A     A    1    1
    ## 2135 009284 EUGEPR  65.3  63.9   48   48   43     A     A     A    1    1
    ## 2136 009374 ALIBED  61.4 108.2   48   50   50     A     A     A    1    1
    ## 2137 009387 ANNOHA  73.1 112.8   48   58   61     A     A     A    1    1
    ## 2138 009497 PROTTE  65.4 152.5   48   58   58     A     A     A    1    1
    ## 2139 009536 AST2GR  77.5 147.4   48   60   64     A     A     A    1    1
    ## 2140 009655 FARAOC  66.6 195.6   48   50   52     A     A     A    1    1
    ## 2141 009717 ADE1TR  79.6 192.3   48   52   53     A     A     A    1    1
    ## 2142 009810 FARAOC  73.4 219.9   48   48   -1     A     A     D    1    1
    ## 2143 010229 CHR2CA 128.9  26.7   48   49   49     A     A     A    1    1
    ## 2144 010294 ALIBED 125.3  54.5   48   48   48     A     A     A    1    1
    ## 2145 010321 CAL2CA 134.7  49.5   48   49   -1     A     A     D    1    1
    ## 2146 010497 BROSAL 134.1  92.0   48   49   49     A     A     A    1    1
    ## 2147 011237 PICRLA 196.5  51.1   48   48   48     A     A     A    1    1
    ## 2148 011276 ALIBED 186.1  76.5   48   -1   -1     A     D     D    1    0
    ## 2149 000290 GENIAM  16.3  45.9   47   53   50     A     A     A    1    1
    ## 2150 000780 COU2CU   0.4 159.8   47   49   53     A     A     A    1    2
    ## 2151 000812 FARAOC  10.9 156.1   47   51   52     A     A     A    1    1
    ## 2152 000883 FARAOC  11.4 165.2   47   48   54     A     A     A    1    1
    ## 2153 001325 PIPERE   4.0 292.9   47   61   64     A     A     A    1    1
    ## 2154 001753 COU2CU 113.8  83.3   47   56   56     A     A     A    1    1
    ## 2155 002060 FARAOC 159.4   9.1   47   54   56     A     A     A    1    1
    ## 2156 002337 PIPERE 149.2  90.9   47   52   54     A     A     A    1    1
    ## 2157 003098 CAL2CA  21.4  25.2   47   57   56     A     A     A    1    1
    ## 2158 003360 PROTTE  35.3  66.9   47   48   48     A     A     A    1    1
    ## 2159 003387 COU2CU  21.1  98.8   47   49   55     A     A     A    1    1
    ## 2160 003514 GUARGL  31.5 119.2   47   49   52     A     A     A    1    1
    ## 2161 003569 INGAFA  25.4 133.9   47   48   46     A     A     A    1    1
    ## 2162 003833 AST2GR  25.2 196.5   47   48   52     A     A     A    1    1
    ## 2163 004237 THEVAH  26.5 264.2   47   51   51     A     A     A    1    1
    ## 2164 006058 FARAOC  53.5   9.6   47   51   54     A     A     A    1    1
    ## 2165 006116 TRIPCU  43.7  30.7   47   48   50     A     A     A    1    1
    ## 2166 006283 ALIBED  44.9  63.1   47   50   49     A     A     A    1    1
    ## 2167 006419 CUPASY  43.9 109.7   47   19   19     A     A     A    1    1
    ## 2168 006755 COU2CU  40.2 187.3   47   49   50     A     A     A    1    1
    ## 2169 007246 COU2CU  56.6 295.6   47   29   29     A     A     A    1    1
    ## 2170 007531 ALIBED  80.2  89.5   47   47   49     A     A     A    1    1
    ## 2171 007916 BROSAL  84.2 188.8   47   47   45     A     A     A    1    1
    ## 2172 008407 BROSAL  90.1 276.6   47   47   47     A     A     A    1    1
    ## 2173 008549 GUARGL 197.2  13.9   47   47   49     A     A     A    1    1
    ## 2174 008625 PIPERE 199.6  20.5   47   58   59     A     A     A    1    1
    ## 2175 009327 FARAOC  63.0  89.1   47   52   53     A     A     A    1    1
    ## 2176 009355 FARAOC  77.4  90.2   47   52   53     A     A     A    1    1
    ## 2177 009674 SWARS1  66.9 180.5   47   49   50     A     A     A    1    1
    ## 2178 009740 FARAOC  64.5 203.0   47   62   65     A     A     A    1    1
    ## 2179 009965 GUARGL  70.5 246.1   47   50   52     A     A     A    1    1
    ## 2180 010255 POSOLA 135.0  36.3   47   51   49     A     A     A    1    1
    ## 2181 010319 GUARGL 133.3  49.3   47   -1   -1     A     D     D    1    0
    ## 2182 010350 POUTCA 139.8  54.9   47   52   58     A     A     A    1    1
    ## 2183 011171 NEEADE 170.4  81.5   47   45   46     A     A     A    1    1
    ## 2184 011190 PIPERE 175.3  86.3   47   50   52     A     A     A    1    1
    ## 2185 011251 PIT1RU 195.6  40.1   47    0    0     A     B     B    1    2
    ## 2186 011355 NEEADE 190.6  81.7   47   47   50     A     A     A    1    1
    ## 2187 000989 ANNOHA  17.9 187.9   46   48   50     A     A     A    1    2
    ## 2188 001189 AST2GR   7.7 241.0   46   46   46     A     A     A    1    1
    ## 2189 001448 MYRCGA 114.1  12.3   46   -1   -1     A     D     D    1    0
    ## 2190 001542 BROSAL 104.8  50.8   46   55   55     A     A     A    1    1
    ## 2191 001709 PSE1SE 118.9  61.1   46   -1   -1     A     D     D    1    0
    ## 2192 001768 CLAVME 115.5  98.1   46   46   47     A     A     A    1    1
    ## 2193 002004 GUARGL 141.1   5.1   46   50   50     A     A     A    1    1
    ## 2194 002092 COPAAR 145.5  22.1   46   50   50     A     A     A    1    1
    ## 2195 003379 COU2CU  24.5  87.9   46   60   61     A     A     A    1    1
    ## 2196 003474 GUARGL  27.2 107.4   46   47   47     A     A     A    1    1
    ## 2197 003492 GUARGL  30.3 107.5   46   46   46     A     A     A    1    1
    ## 2198 003744 CUPASY  26.4 167.4   46   12   13     A     A     A    1    1
    ## 2199 004140 COU2CU  20.8 255.8   46   48   50     A     A     A    1    1
    ## 2200 006135 EUGEPR  46.4  29.5   46   48   50     A     A     A    1    1
    ## 2201 006354 COU2CU  56.3  64.7   46   54   53     A     A     A    1    2
    ## 2202 006612 PROTTE  47.4 146.1   46   59   52     A     A     A    1    1
    ## 2203 006954 ANNOHA  58.7 217.3   46   46   48     A     A     A    1    1
    ## 2204 007141 COU2CU  45.1 262.3   46   49   52     A     A     A    1    1
    ## 2205 008477 BROSAL  99.6 286.7   46   47   48     A     A     A    1    1
    ## 2206 009205 AST2GR  71.4  48.5   46   48   48     A     A     A    1    1
    ## 2207 009414 BACTMA  66.0 137.8   46   44   45     A     A     A    1    1
    ## 2208 009557 PIPERE  63.8 174.1   46   56   60     A     A     A    1    1
    ## 2209 009665 FARAOC  65.6 194.2   46   50   53     A     A     A    1    1
    ## 2210 009906 GUARGL  72.9 235.6   46   49   49     A     A     A    1    1
    ## 2211 009974 EUGEPR  70.6 258.6   46   48   48     A     A     A    1    1
    ## 2212 010172 GUARGL 122.7  33.8   46   46   46     A     A     A    1    1
    ## 2213 010316 GUARGL 130.4  46.8   46   46   47     A     A     A    1    1
    ## 2214 010353 BROSAL 137.3  47.5   46   48   56     A     A     A    1    1
    ## 2215 010396 CASEGU 125.3  66.0   46   46   36     A     A     A    1    1
    ## 2216 010945 PIPERE 175.4  16.4   46   46   46     A     A     A    1    1
    ## 2217 010983 LUEHSE 172.8  20.7   46   -1   -1     A     D     D    1    0
    ## 2218 000169 AST2GR  11.7  28.4   45   48   50     A     A     A    1    1
    ## 2219 000469 GUARGL   5.8  93.5   45   46   46     A     A     A    1    1
    ## 2220 000885 FARAOC  11.6 166.4   45   47   49     A     A     A    1    1
    ## 2221 000888 FARAOC  11.7 170.5   45   52   54     A     A     A    1    1
    ## 2222 000921 INGAFA  18.7 169.6   45   48   46     A     A     A    1    1
    ## 2223 001317 PIPECU   0.7 285.1   45   56   56     A     A     A    1    1
    ## 2224 001328 COU2CU   0.4 292.4   45   49   49     A     A     A    1    1
    ## 2225 002166      * 141.1  59.9   45   48   50     A     A     A    1    1
    ## 2226 003080 CAL2CA  37.7  19.3   45   46   -1     A     A     D    1    1
    ## 2227 003111 CASECO  24.8  39.1   45   46   49     A     A     A    1    1
    ## 2228 003132 CUPASY  29.6  28.1   45   48   50     A     A     A    1    1
    ## 2229 003335 ANTITR  33.2  60.4   45   52   54     A     A     A    1    1
    ## 2230 003338 ALIBED  31.6  62.9   45   49   49     A     A     A    1    1
    ## 2231 003392 TRI2PL  27.8  90.3   45   42   42     A     A     A    1    1
    ## 2232 003618 COU2CU  35.2 124.7   45   41   50     A     A     A    1    1
    ## 2233 003772 COU2CU  30.3 177.2   45   -1   -1     A     D     D    1    0
    ## 2234 003773 COPAAR  31.4 178.8   45   49   50     A     A     A    1    1
    ## 2235 006117 BROSAL  42.2  37.2   45   48   49     A     A     A    1    1
    ## 2236 006719 DENDAR  46.6 163.1   45   45   46     A     A     A    1    1
    ## 2237 006780 BROSAL  46.8 196.8   45   47   47     A     A     A    1    1
    ## 2238 006817 COU2CU  47.1 181.4   45   45   45     A     A     A    1    1
    ## 2239 007131 COU2CU  48.3 271.4   45   46   47     A     A     A    1    1
    ## 2240 007140 THEVAH  47.2 260.5   45   46   47     A     A     A    1    1
    ## 2241 007217 FARAOC  48.6 299.9   45   44   45     A     A     A    1    1
    ## 2242 007394 AST2GR  80.6  45.4   45   45   44     A     A     A    1    1
    ## 2243 007474 AST2GR  81.0  66.2   45   41   40     A     A     A    1    1
    ## 2244 007536 ANNOHA  82.7  90.7   45   45   46     A     A     A    1    1
    ## 2245 007698 GUARGL  91.3 130.1   45   48   48     A     A     A    1    1
    ## 2246 008238 SOROAF  84.7 254.4   45   47   48     A     A     A    1    1
    ## 2247 009369 EUGEPR  61.2 101.8   45   49   49     A     A     A    1    1
    ## 2248 009417 AST2GR  69.6 138.3   45   46   46     A     A     A    1    1
    ## 2249 009510 CUPASY  70.5 143.8   45   45   46     A     A     A    1    1
    ## 2250 010384 MATAGL 122.6  76.1   45   45   46     A     A     A    1    1
    ## 2251 010387 PSE1SE 120.3  61.0   45   -1   -1     A     D     D    1    0
    ## 2252 010388 CASECO 122.0  79.9   45   50   51     A     A     A    1    1
    ## 2253 010407 BROSAL 134.2  60.1   45   45   54     A     A     A    1    1
    ## 2254 011025 PROTTE 160.4  58.6   45   48   49     A     A     A    1    1
    ## 2255 011041 PROTTE 167.0  40.7   45   49   49     A     A     A    1    1
    ## 2256 011179 PICRLA 173.2  99.9   45   14   14     A     A     A    1    1
    ## 2257 011261 LACIAG 180.3  61.4   45   48   50     A     A     A    1    1
    ## 2258 011329 BACTBA 198.4  67.7   45   44   45     A     A     A    1    1
    ## 2259 000145 SWARS1   7.9  31.7   44   54   55     A     A     A    1    1
    ## 2260 000574 SOROAF   0.3 103.8   44   45   45     A     A     A    1    1
    ## 2261 000676 SCH2MO  19.6 108.0   44   50   50     A     A     A    1    1
    ## 2262 000767 FARAOC  17.9 128.0   44   53   53     A     A     A    1    1
    ## 2263 001222 BROSAL  19.1 240.5   44   44   44     A     A     A    1    1
    ## 2264 001229 ALIBED   4.7  96.2   44   42   44     A     A     A    1    1
    ## 2265 001411 ANNOHA 106.1  10.4   44   46   45     A     A     A    1    1
    ## 2266 001492 BROSAL 106.3  39.9   44   50   52     A     A     A    1    1
    ## 2267 001733 COU2CU 103.0  92.9   44   46   45     A     A     A    1    1
    ## 2268 001739 PROTTE 102.9  99.7   44   46   45     A     A     A    1    2
    ## 2269 002045 PIPERE 158.5  13.0   44   45   47     A     A     A    1    1
    ## 2270 002118 ALIBED 151.0  33.1   44   45   46     A     A     A    1    1
    ## 2271 002194 DENDAR 153.3  54.7   44   46   46     A     A     A    1    1
    ## 2272 002321 BROSAL 140.1  90.2   44   51   58     A     A     A    1    1
    ## 2273 003214 CAL2CA  20.8  52.5   44   56   61     A     A     A    1    1
    ## 2274 003254 NEEADE  31.6  40.3   44   46   49     A     A     A    1    1
    ## 2275 003431 ALIBED  33.6  98.5   44   55   55     A     A     A    1    1
    ## 2276 003447 AST2GR  22.0 103.4   44   44   44     A     A     A    1    1
    ## 2277 003478 COU2CU  25.1 102.0   44   44   -1     A     A     D    1    1
    ## 2278 003705 COU2CU  35.2 146.2   44   -1   -1     A     D     D    1    0
    ## 2279 003779 FARAOC  38.7 179.4   44   45   49     A     A     A    1    1
    ## 2280 003850 COU2CU  27.6 184.8   44   50   55     A     A     A    1    1
    ## 2281 003907 SWARS1  36.2 196.0   44   46   49     A     A     A    1    1
    ## 2282 004223 PIPERE  29.4 265.7   44   47   49     A     A     A    1    1
    ## 2283 006106 ANNOHA  41.7  20.6   44   44   47     A     A     A    1    1
    ## 2284 006614 PIPERE  46.1 146.4   44   59   59     A     A     A    1    1
    ## 2285 006802 COU2CU  47.3 185.2   44   14   -1     A     A     D    1    2
    ## 2286 006902 CUPASY  47.3 214.8   44   21   23     A     A     A    1    2
    ## 2287 006910 COU2CU  45.4 206.8   44   46   46     A     A     A    1    1
    ## 2288 007082 COU2CU  48.7 244.5   44   -1   -1     A     D     D    1    0
    ## 2289 007448 LUEHSE  94.1  58.9   44   44   45     A     A     A    1    1
    ## 2290 007591 NEEADE  99.1  89.8   44   44   44     A     A     A    1    1
    ## 2291 007784 CASECO  93.7 149.4   44   45   48     A     A     A    1    1
    ## 2292 007812 SOROAF  98.6 152.5   44   45   45     A     A     A    1    1
    ## 2293 007894 PROTTE  95.4 165.6   44   52   53     A     A     A    1    1
    ## 2294 008449 GUARGL  84.3 297.7   44   47   50     A     A     A    1    1
    ## 2295 008604 LUEHSE 193.8  39.0   44   16   19     A     A     A    1    1
    ## 2296 009012 POSOLA  60.3  10.1   44   46   49     A     A     A    1    1
    ## 2297 009044 ANNOHA  67.8   0.9   44   44   47     A     A     A    1    1
    ## 2298 009181 EUGEPR  69.0  49.7   44   45   42     A     A     A    1    1
    ## 2299 009341 GUARGL  72.5  85.1   44   47   -1     A     A     D    1    1
    ## 2300 009356 CUPASY  75.5  90.9   44   49   49     A     A     A    1    2
    ## 2301 009360 CLAVME  75.1  87.1   44   48   48     A     A     A    1    1
    ## 2302 009439 TAB1RO  70.2 130.3   44   44   44     A     A     A    1    1
    ## 2303 009975 FARAOC  70.7 259.1   44   45   53     A     A     A    1    1
    ## 2304 009994 ALIBED  75.1 253.5   44   41   43     A     A     A    1    1
    ## 2305 010129 ALIBED  72.7 299.2   44   47   47     A     A     A    1    1
    ## 2306 010349 ANNOHA 136.3  54.5   44   44   44     A     A     A    1    1
    ## 2307 010952 PIPERE 179.9  14.9   44   44   44     A     A     A    1    1
    ## 2308 011006 TAB1RO 177.6  36.2   44   50   50     A     A     A    1    1
    ## 2309 011312 BROSAL 195.7  78.8   44   44   50     A     A     A    1    1
    ## 2310 011345 CAL2CA 188.6  86.3   44   45   52     A     A     A    1    1
    ## 2311 011364 BROSAL 199.9  95.2   44   44   44     A     A     A    1    1
    ## 2312 000369 CASECO  10.9  69.0   43   57   59     A     A     A    1    1
    ## 2313 000391 NEEADE  15.6  65.6   43   47   48     A     A     A    1    1
    ## 2314 000571 ANNOHA   0.6 101.1   43   42   42     A     A     A    1    1
    ## 2315 000729 BACTMA   6.4 128.5   43   40   41     A     A     A    1    1
    ## 2316 000866 SOROAF   8.3 169.8   43   46   46     A     A     A    1    1
    ## 2317 000878 FARAOC  11.3 162.5   43   44   44     A     A     A    1    1
    ## 2318 000910 INGAFA  17.8 174.1   43   43   -1     A     A     D    1    2
    ## 2319 000944 IXORFL   5.1 199.8   43   43   44     A     A     A    1    1
    ## 2320 001478 ALIBED 116.8   3.0   43   44   44     A     A     A    1    1
    ## 2321 001608 TRIPCU 119.1  56.3   43   54   57     A     A     A    1    1
    ## 2322 002076 CAL2CA 144.7  32.8   43   46   47     A     A     A    1    1
    ## 2323 002138 TRIPCU 156.6  24.8   43   44   45     A     A     A    1    1
    ## 2324 002245 BROSAL 142.3  70.4   43   51   51     A     A     A    1    1
    ## 2325 002308 PROTTE 156.7  62.4   43   47   48     A     A     A    1    1
    ## 2326 003129 CHR2CA  26.6  29.9   43   49   49     A     A     A    1    1
    ## 2327 003550 PICRLA  23.8 130.1   43   48   50     A     A     A    1    1
    ## 2328 003674 COU2CU  33.8 143.4   43   46   44     A     A     A    1    1
    ## 2329 003676 CUPASY  32.5 145.9   43   45   45     A     A     A    1    1
    ## 2330 004076 FARAOC  22.9 228.5   43   43   45     A     A     A    1    1
    ## 2331 004087 HEISCO  26.2 222.3   43   44   46     A     A     A    1    1
    ## 2332 004118 HEISCO  37.0 224.8   43   53   57     A     A     A    1    1
    ## 2333 004205 PIPERE  22.3 263.9   43   43   42     A     A     A    1    1
    ## 2334 004289 FARAOC  27.2 297.5   43   52   54     A     A     A    1    1
    ## 2335 006114 ALIBED  40.0  34.0   43   44   44     A     A     A    1    2
    ## 2336 006171 EUGEPR  56.5  36.4   43   47   44     A     A     A    1    1
    ## 2337 006207 COU2CU  49.3  58.4   43   48   51     A     A     A    1    2
    ## 2338 006384 FARAOC  45.3  86.2   43   46   48     A     A     A    1    1
    ## 2339 007109 PROTTE  43.4 262.9   43   40   40     A     A     A    1    1
    ## 2340 007236 FARAOC  51.1 293.5   43   47   47     A     A     A    1    1
    ## 2341 007339 AST2GR  87.1  21.0   43   40   41     A     A     A    1    1
    ## 2342 007416 CORDAL  86.7  46.6   43   51   48     A     A     A    1    1
    ## 2343 007462 AST2GR  98.7  47.4   43   44   44     A     A     A    1    1
    ## 2344 007677 SOROAF  87.7 134.5   43   44   47     A     A     A    1    1
    ## 2345 007705 POUTCA  91.2 137.4   43   46   43     A     A     A    1    1
    ## 2346 007774 HIRTAM  90.1 143.6   43   43   45     A     A     A    1    1
    ## 2347 007833 BROSAL  84.7 173.6   43   43   56     A     A     A    1    1
    ## 2348 008503 PIPERE 181.5  19.4   43   43   43     A     A     A    1    1
    ## 2349 009001 CUPASY  63.9   2.3   43   44   46     A     A     A    1    1
    ## 2350 009003 PROTTE  64.6   3.0   43   45   47     A     A     A    1    1
    ## 2351 009103 POSOLA  64.8  36.2   43   47   56     A     A     A    1    1
    ## 2352 009171 TRIPCU  64.5  59.8   43   43   46     A     A     A    1    1
    ## 2353 009255 ANNOHA  63.2  74.7   43   44   47     A     A     A    1    1
    ## 2354 009778 AMAICO  69.3 203.0   43   49   50     A     A     A    1    1
    ## 2355 010078 ALIBED  75.3 268.7   43   44   43     A     A     A    1    1
    ## 2356 010145 FARAOC 125.2  35.2   43   46   50     A     A     A    1    1
    ## 2357 010924 TROPRA 172.8   5.7   43   43   44     A     A     A    1    1
    ## 2358 000256 EUGEPR  10.7  59.2   42   46   46     A     A     A    1    1
    ## 2359 000409 ANNOHA  18.2  62.3   42   46   45     A     A     A    1    1
    ## 2360 000512 COU2CU  13.5  90.6   42   47   50     A     A     A    1    1
    ## 2361 000683 FARAOC  17.7 104.7   42   41   42     A     A     A    1    1
    ## 2362 001000 CASESY   3.4 212.1   42   43   43     A     A     A    1    1
    ## 2363 001262 HIRTRA   9.5 271.5   42   42   48     A     A     A    1    1
    ## 2364 001263 BACTMA   0.5 238.7   42   40   40     A     A     A    1    1
    ## 2365 001318 ZUELGU   0.6 287.8   42   52   54     A     A     A    1    1
    ## 2366 001385 CAVAPL  19.8 291.2   42   53   56     A     A     A    1    1
    ## 2367 001521 AST2GR 119.0  36.9   42   42   43     A     A     A    1    1
    ## 2368 001609 CASEGU 117.5  56.5   42   44   44     A     A     A    1    1
    ## 2369 001645 TRIPCU 101.5  72.0   42   46   48     A     A     A    1    1
    ## 2370 001697 VITECO 118.4  79.7   42   42   45     A     A     A    1    1
    ## 2371 001765 BROSAL 118.5  95.6   42   45   40     A     A     A    1    1
    ## 2372 002285 AST2GR 151.2  69.8   42   50   53     A     A     A    1    1
    ## 2373 003209 CASECO  22.8  43.5   42   57   60     A     A     A    1    1
    ## 2374 003304 RANDFO  24.8  67.5   42   46   36     A     A     A    1    1
    ## 2375 003371 BROSAL  24.8  82.7   42   47   47     A     A     A    1    1
    ## 2376 003490 HEISCO  31.2 105.8   42   48   56     A     A     A    1    1
    ## 2377 003494 COU2CU  30.9 109.0   42   44   45     A     A     A    1    1
    ## 2378 003543 COCCPA  24.8 124.8   42   49   51     A     A     A    1    1
    ## 2379 003781 COU2CU  38.7 170.4   42   45   56     A     A     A    1    1
    ## 2380 003859 COU2CU  30.6 181.5   42   48   52     A     A     A    1    1
    ## 2381 004009 BROSAL  30.8 210.1   42   44   48     A     A     A    1    1
    ## 2382 004254 PIPERE  36.5 270.4   42   50   54     A     A     A    1    1
    ## 2383 004280 COU2CU  21.2 293.1   42   45   41     A     A     A    1    1
    ## 2384 006208 COU2CU  49.0  56.6   42   45   42     A     A     A    1    1
    ## 2385 006307 COU2CU  47.4  78.5   42   44   40     A     A     A    1    1
    ## 2386 006648 GUARGL  57.5 144.9   42   42   43     A     A     A    1    1
    ## 2387 006691 FARAOC  49.6 179.1   42   43   43     A     A     A    1    1
    ## 2388 006898 COU2CU  45.7 211.9   42   48   51     A     A     A    1    1
    ## 2389 006988 FARAOC  49.0 235.3   42   44   45     A     A     A    1    2
    ## 2390 007067 COU2CU  48.7 258.6   42   44   44     A     A     A    1    1
    ## 2391 007227 FARAOC  50.3 280.5   42   47   48     A     A     A    1    1
    ## 2392 007307 CUPASY  81.8  23.0   42   42   42     A     A     A    1    1
    ## 2393 007432 MATAGL  91.2  44.6   42   44   45     A     A     A    1    1
    ## 2394 007499 ANTITR  92.4  63.7   42   -1   -1     A     D     D    1    0
    ## 2395 007509 PROTTE  94.2  77.7   42   50   53     A     A     A    1    1
    ## 2396 008061 CUPASY  80.3 217.3   42   44   46     A     A     A    1    1
    ## 2397 008119 PROTTE  96.0 200.5   42   -1   -1     A     D     D    1    0
    ## 2398 008186 POSOLA  98.8 236.0   42   45   48     A     A     A    1    1
    ## 2399 008363 BACTBA  82.5 279.8   42   41   42     A     A     A    1    1
    ## 2400 008480 ALIBED 180.1   3.8   42   42   -1     A     A     D    1    1
    ## 2401 008694 ANNOHA  80.2 112.6   42   42   43     A     A     A    1    1
    ## 2402 009059 COPAAR  71.1  14.9   42   43   42     A     A     A    1    1
    ## 2403 009195 TRIPCU  72.8  44.8   42    0   19     A     B     A    1    2
    ## 2404 009370 ALIBED  64.8 103.5   42   45   46     A     A     A    1    1
    ## 2405 009404 TURNPA  63.0 125.3   42   44   43     A     A     A    1    1
    ## 2406 009415 BACTMA  66.7 139.4   42   40   43     A     A     A    1    1
    ## 2407 009425 ANNOHA  69.4 133.7   42   43   43     A     A     A    1    1
    ## 2408 009429 GUARGL  65.5 129.2   42   43   42     A     A     A    1    1
    ## 2409 009470 FARAOC  79.7 129.0   42   34   36     A     A     A    1    1
    ## 2410 009558 TRIPCU  63.9 175.6   42   43   43     A     A     A    1    1
    ## 2411 009653 FARAOC  69.3 195.1   42   47   48     A     A     A    1    1
    ## 2412 009863 TAB1RO  61.1 232.5   42   46   46     A     A     A    1    1
    ## 2413 010022 TRI2PL  67.5 265.9   42   50   55     A     A     A    1    1
    ## 2414 010088 AST2GR  60.2 288.3   42   42   42     A     A     A    1    1
    ## 2415 010159 PROTTE 126.4  11.9   42   48   48     A     A     A    1    1
    ## 2416 010192 FARAOC 138.5   5.1   42   48   49     A     A     A    1    1
    ## 2417 010228 AST2GR 129.4  27.1   42   42   42     A     A     A    1    1
    ## 2418 010370 CASEGU 120.2  63.8   42   42   48     A     A     A    1    1
    ## 2419 010481 AST2GR 128.2  83.2   42   42   46     A     A     A    1    1
    ## 2420 010883 PIPERE 160.1   8.9   42   35   35     A     A     A    1    1
    ## 2421 010909 PIPERE 167.4  14.1   42   43   46     A     A     A    1    1
    ## 2422 011031 CHR2CA 167.1  59.9   42   43   44     A     A     A    1    1
    ## 2423 011311 LACIAG 195.6  77.4   42   41   44     A     A     A    1    1
    ## 2424 000033 FARAOC   9.1   6.3   41   49   49     A     A     A    1    1
    ## 2425 000058 CUPASY  10.2  10.5   41   44   45     A     A     A    1    1
    ## 2426 000236 ANNOHA   7.0  46.4   41   37   42     A     A     A    1    1
    ## 2427 000405 AST2GR  17.4  64.3   41   44   45     A     A     A    1    1
    ## 2428 000791 BACTMA   6.3 146.1   41   40   41     A     A     A    1    1
    ## 2429 001119 CLAVME   9.8 221.2   41   -1   -1     A     D     D    1    0
    ## 2430 001202 HEISCO  13.7 256.2   41   44   49     A     A     A    1    1
    ## 2431 001299 SWARS1  16.5 279.8   41   45   46     A     A     A    1    1
    ## 2432 001319 PROTTE   0.3 288.3   41   45   49     A     A     A    1    1
    ## 2433 001437 TRI2PL 111.9  10.4   41   41   -1     A     A     D    1    1
    ## 2434 001522 CUPASY 119.4  30.9   41   45   50     A     A     A    1    1
    ## 2435 001574 PHOECI 109.0  43.7   41   25   25     A     A     A    1    1
    ## 2436 001712 CAL2CA 115.6  61.7   41   41   42     A     A     A    1    1
    ## 2437 001771 ALBIAD 116.4  99.3   41   43   42     A     A     A    1    1
    ## 2438 002307 TRIPCU 155.9  61.4   41   42   45     A     A     A    1    1
    ## 2439 003048 ANNOHA  29.4  10.6   41   41   41     A     A     A    1    1
    ## 2440 003101 CUPASY  20.1  28.7   41   47   50     A     A     A    1    1
    ## 2441 003142 COU2CU  27.3  24.2   41   48   49     A     A     A    1    1
    ## 2442 003208 HEISCO  20.6  43.4   41   58   59     A     A     A    1    1
    ## 2443 003445 COU2CU  24.1 100.5   41   -1   -1     A     D     D    1    0
    ## 2444 003566 HEISCO  27.8 139.8   41   47   52     A     A     A    1    1
    ## 2445 003631 PROTTE  21.8 149.0   41   43   40     A     A     A    1    1
    ## 2446 003827 COU2CU  24.3 195.5   41   42   -1     A     A     D    1    1
    ## 2447 003967 SOROAF  21.1 214.8   41   46   46     A     A     A    1    1
    ## 2448 004144 COU2CU  21.3 259.5   41   45   45     A     A     A    1    1
    ## 2449 004231 SWARS1  26.5 268.5   41   43   44     A     A     A    1    1
    ## 2450 004265 GUARGL  39.7 263.8   41   45   46     A     A     A    1    1
    ## 2451 004342 ANNOHA  37.1 285.2   41   33   35     A     A     A    1    1
    ## 2452 006032 TRI2PL  46.9  14.0   41   41   45     A     A     A    1    1
    ## 2453 006361 IXORFL  40.7  84.0   41   45   39     A     A     A    1    1
    ## 2454 006396 TAB1RO  54.9  89.5   41   43   43     A     A     A    1    1
    ## 2455 006477 CUPASY  42.4 123.9   41   45   46     A     A     A    1    1
    ## 2456 006657 CASESY  41.2 163.6   41   48   51     A     A     A    1    1
    ## 2457 006911 COU2CU  46.4 209.3   41   45   42     A     A     A    1    1
    ## 2458 007235 ANNOHA  51.5 292.6   41   40   41     A     A     A    1    1
    ## 2459 007258 PIPERE  55.5 284.5   41   11   -1     A     A     D    1    1
    ## 2460 007343 EUGEPR  89.7  22.9   41   46   44     A     A     A    1    1
    ## 2461 007406 TRIPCU  89.9  56.0   41   58   64     A     A     A    1    1
    ## 2462 007619 GUARGL  85.4 100.5   41   42   42     A     A     A    1    1
    ## 2463 008043 CUPASY  80.9 208.5   41   44   43     A     A     A    1    1
    ## 2464 008333 FARAOC  98.5 245.1   41   41   47     A     A     A    1    1
    ## 2465 008497 CASESY 181.8  14.7   41   41   43     A     A     A    1    1
    ## 2466 008602 BROSAL 191.4  39.4   41   42   42     A     A     A    1    1
    ## 2467 009147 FARAOC  76.4  26.1   41   46   48     A     A     A    1    1
    ## 2468 009241 COU2CU  61.2  60.8   41   45   49     A     A     A    1    1
    ## 2469 009427 AST2GR  65.3 128.0   41   41   41     A     A     A    1    1
    ## 2470 009455 FARAOC  79.1 130.9   41   42   42     A     A     A    1    1
    ## 2471 009548 CUPARU  63.8 164.4   41   42   44     A     A     A    1    1
    ## 2472 009649 FARAOC  64.8 199.5   41   41   43     A     A     A    1    1
    ## 2473 009659 FARAOC  66.4 198.8   41   47   50     A     A     A    1    1
    ## 2474 009883 TRI2PL  65.8 227.0   41   41   41     A     A     A    1    1
    ## 2475 011002 FARAOC 172.7  39.7   41   43   44     A     A     A    1    1
    ## 2476 011315 SOROAF 197.5  70.3   41   51   49     A     A     A    1    1
    ## 2477 011337 POSOLA 181.1  93.7   41   43   34     A     A     A    1    1
    ## 2478 011384 SWARS1 197.6  82.3   41   41   41     A     A     A    1    1
    ## 2479 011400 ANNOHA 110.1  55.7   41   45   -1     A     A     D    1    1
    ## 2480 000229 SWARS1   9.8  58.4   40   45   46     A     A     A    1    1
    ## 2481 000274 POCHSE  19.3  59.8   40   -1   -1     A     D     D    1    0
    ## 2482 000371 LUEHSE  14.7  69.3   40   57   63     A     A     A    1    1
    ## 2483 000376 CUPASY  12.4  79.2   40   43   45     A     A     A    1    1
    ## 2484 000416 COU2CU   0.1  83.9   40   42   42     A     A     A    1    1
    ## 2485 000665 LACIAG  16.2 112.5   40   39   40     A     A     A    1    1
    ## 2486 000887 HEISCO  13.7 171.6   40   42   45     A     A     A    1    1
    ## 2487 000901 SOROAF  17.5 179.1   40   41   44     A     A     A    1    1
    ## 2488 000911 FARAOC  19.9 173.9   40   -1   -1     A     D     D    1    0
    ## 2489 000930 SOROAF   5.2 191.2   40   39   42     A     A     A    1    1
    ## 2490 001012 BROSAL   2.6 217.0   40   43   43     A     A     A    1    1
    ## 2491 001052 HEISCO  11.0 210.8   40   40   44     A     A     A    1    1
    ## 2492 001291 COU2CU  11.4 276.8   40   35   41     A     A     A    1    1
    ## 2493 001307 COU2CU  19.2 262.2   40   45   47     A     A     A    1    1
    ## 2494 001519 ANNOHA 114.1  38.9   40   40   36     A     A     A    1    1
    ## 2495 001721 FARAOC 100.1  81.2   40   48   48     A     A     A    1    1
    ## 2496 002006 PROTTE 140.8  12.4   40   45   47     A     A     A    1    1
    ## 2497 002050 BROSAL 157.8   5.1   40   41   42     A     A     A    1    1
    ## 2498 002158 NEEADE 141.2  54.1   40   40   41     A     A     A    1    1
    ## 2499 002172 AST2GR 145.1  57.5   40   53   59     A     A     A    1    1
    ## 2500 002301 PIPERE 156.6  66.0   40   45   48     A     A     A    1    1
    ## 2501 002329 PROTTE 144.6  99.9   40   44   42     A     A     A    1    2
    ## 2502 003054 PROTTE  27.8   2.0   40   48   48     A     A     A    1    1
    ## 2503 003256 TRI2PL  30.8  40.9   40   46   47     A     A     A    1    1
    ## 2504 003280 ANNOHA  39.5  54.6   40   43   47     A     A     A    1    1
    ## 2505 003620 COU2CU  37.6 123.4   40   46   49     A     A     A    1    1
    ## 2506 003725 TRI2PL  23.9 168.6   40   43   45     A     A     A    1    1
    ## 2507 003780 PROTTE  39.4 177.0   40   43   43     A     A     A    1    1
    ## 2508 003836 COU2CU  25.1 194.4   40   43   52     A     A     A    1    1
    ## 2509 003919 FARAOC  35.4 191.2   40   41   41     A     A     A    1    1
    ## 2510 003965 SOROAF  21.7 211.8   40   45   50     A     A     A    1    2
    ## 2511 004055 PROTTE  35.2 205.4   40   40   41     A     A     A    1    1
    ## 2512 004151 OURALU  28.7 246.3   40   41   40     A     A     A    1    1
    ## 2513 004298 FARAOC  29.3 294.2   40   45   43     A     A     A    1    1
    ## 2514 004304 COU2CU  26.5 286.8   40   45   47     A     A     A    1    1
    ## 2515 006007 FARAOC  41.3   9.4   40   49   49     A     A     A    1    1
    ## 2516 006095 PIPERE  55.1   4.8   40   40   42     A     A     A    1    1
    ## 2517 006382 PROTTE  45.6  85.0   40   40   40     A     A     A    1    1
    ## 2518 006472 PROTTE  58.5 108.9   40   41   46     A     A     A    1    1
    ## 2519 006638 COU2CU  58.5 146.1   40   40   42     A     A     A    1    1
    ## 2520 006970 COU2CU  40.7 223.2   40   44   46     A     A     A    1    1
    ## 2521 007005 BACTBA  48.1 220.7   40   40   40     A     A     A    1    1
    ## 2522 007013 FARAOC  47.3 222.7   40   45   48     A     A     A    1    1
    ## 2523 007021 POSOLA  53.6 225.5   40   43   43     A     A     A    1    1
    ## 2524 007071 FARAOC  46.2 253.2   40   45   48     A     A     A    1    1
    ## 2525 007184 COU2CU  57.5 261.9   40   46   46     A     A     A    1    1
    ## 2526 007356 BROSAL  94.9  30.1   40   48   49     A     A     A    1    1
    ## 2527 007363 TRI2PL  90.4  33.1   40   12   12     A     A     A    1    1
    ## 2528 007449 LUEHSE  98.3  55.4   40   40   39     A     A     A    1    1
    ## 2529 007476 COU2CU  82.6  69.7   40   65   48     A     A     A    1    1
    ## 2530 007686 EUGEPR  85.5 127.6   40   40   40     A     A     A    1    1
    ## 2531 007703 BACTBA  92.1 135.5   40   41   41     A     A     A    1    1
    ## 2532 007788 ACACME  91.7 151.6   40    0   -1     A     B     D    1    2
    ## 2533 007968 BACTBA  87.4 184.5   40   44   44     A     A     A    1    1
    ## 2534 008032 BACTMA  99.9 183.9   40   32   32     A     A     A    1    1
    ## 2535 008099 PROTTE  93.1 212.6   40   47   47     A     A     A    1    1
    ## 2536 008607 CAL2CA 194.4  35.3   40   44   -1     A     A     D    1    1
    ## 2537 009134 EUGEPR  73.0  39.8   40   -1   -1     A     D     D    1    0
    ## 2538 009187 GUARGL  66.3  40.1   40   41   17     A     A     A    1    1
    ## 2539 009271 PROTTE  69.8  72.7   40   46   45     A     A     A    1    1
    ## 2540 009584 RANDFO  65.9 162.2   40   42   42     A     A     A    1    1
    ## 2541 009642 FARAOC  63.8 196.0   40   46   49     A     A     A    1    1
    ## 2542 009682 ANNOHA  74.2 185.2   40   40   43     A     A     A    1    1
    ## 2543 009687 ANNOHA  74.2 189.4   40   45   50     A     A     A    1    1
    ## 2544 009862 HEISCO  60.9 231.8   40   46   50     A     A     A    1    1
    ## 2545 009956 COU2CU  67.1 254.2   40   47   50     A     A     A    1    1
    ## 2546 010043 ALIBED  74.5 261.9   40   40   41     A     A     A    1    1
    ## 2547 010065 PROTTE  74.0 279.4   40   44   44     A     A     A    1    1
    ## 2548 010122 BROSAL  72.8 290.1   40   40   43     A     A     A    1    1
    ## 2549 010202 TRIPCU 123.6  22.9   40   46   46     A     A     A    1    1
    ## 2550 010302 FARAOC 127.6  43.3   40   50   55     A     A     A    1    1
    ## 2551 011083 TRI2PL 164.4  70.2   40   -1   -1     A     D     D    1    0
    ## 2552 011188 SWARS1 176.9  92.4   40   42   43     A     A     A    1    1
    ## 2553 011199 ALIBED 188.3  59.7   40   40   40     A     A     A    1    1
    ## 2554 011222 HIRTRA 193.5  50.0   40   40   14     A     A     A    1    1
    ## 2555 011343 PROTTE 187.2  93.3   40   40   45     A     A     A    1    2
    ## 2556 011403 CAL2CA 114.6  47.9   40   43   42     A     A     A    1    1
    ## 2557 000110 COU2CU  16.0   3.0   39   44   48     A     A     A    1    1
    ## 2558 000170 CUPASY  14.2  29.1   39   33   44     A     A     A    1    1
    ## 2559 000254 EUGEPR  13.5  55.9   39   28   30     A     A     A    1    1
    ## 2560 000355 PICRLA  14.6  60.6   39   51   52     A     A     A    1    1
    ## 2561 000745 FARAOC  11.7 139.9   39   41   43     A     A     A    1    1
    ## 2562 000987 TRIPCU  15.7 188.0   39   43   47     A     A     A    1    1
    ## 2563 001045 COU2CU  11.2 207.5   39   42   42     A     A     A    1    1
    ## 2564 001102 PIPERE   2.2 266.4   39   -1   -1     A     D     D    1    0
    ## 2565 001123 BACTMA  10.6 223.2   39   38    0     A     A     B    1    1
    ## 2566 001197 PROTTE  13.8 249.9   39   41   40     A     A     A    1    1
    ## 2567 001394 GUARGL 102.0  11.3   39   40   46     A     A     A    1    1
    ## 2568 001586 BROSAL 113.4  51.1   39   39   41     A     A     A    1    1
    ## 2569 002067 SLOATE 141.8  27.3   39   42   50     A     A     A    1    1
    ## 2570 002161 ALIBED 143.3  52.2   39   37   38     A     A     A    1    1
    ## 2571 002238 ALBIAD 141.2  66.3   39   43   43     A     A     A    1    1
    ## 2572 002239 AST2GR 140.5  65.6   39   43   44     A     A     A    1    1
    ## 2573 003013 FARAOC  21.6   8.8   39   44   45     A     A     A    1    1
    ## 2574 003109 AST2GR  23.8  36.5   39   43   45     A     A     A    1    1
    ## 2575 003200 EUGEPR  38.8  27.1   39   45   48     A     A     A    1    1
    ## 2576 003407 EUGEPR  25.7  80.7   39   43   43     A     A     A    1    1
    ## 2577 003570 COU2CU  27.4 134.3   39   42   43     A     A     A    1    1
    ## 2578 003622 HEISCO  39.5 124.5   39   43   47     A     A     A    1    1
    ## 2579 003624 CUPASY  20.4 142.4   39   41   42     A     A     A    1    1
    ## 2580 003638 COU2CU  24.7 148.0   39   42   46     A     A     A    1    1
    ## 2581 003722 MYRCGA  21.2 169.2   39   -1   -1     A     D     D    1    0
    ## 2582 003984 SWARS1  25.7 214.4   39   41   42     A     A     A    1    1
    ## 2583 004135 LUEHSE  21.0 254.0   39   39   42     A     A     A    1    1
    ## 2584 004189 CHR2CA  36.5 259.1   39   43   45     A     A     A    1    1
    ## 2585 004235 MACRGL  25.0 261.3   39   41   38     A     A     A    1    1
    ## 2586 004243 PIPERE  31.7 267.9   39   51   49     A     A     A    1    1
    ## 2587 004283 COU2CU  23.8 295.3   39   45   50     A     A     A    1    1
    ## 2588 004290 COU2CU  27.8 297.3   39   39   36     A     A     A    1    1
    ## 2589 006110 COU2CU  42.7  26.0   39   43   49     A     A     A    1    1
    ## 2590 006151 FARAOC  50.5  21.8   39   43   48     A     A     A    1    1
    ## 2591 006173 GUARGL  56.6  30.4   39   -1   -1     A     D     D    1    0
    ## 2592 006282 FARAOC  44.6  63.2   39   46   48     A     A     A    1    1
    ## 2593 006877 BROSAL  42.2 215.3   39   43   45     A     A     A    1    1
    ## 2594 007194 ANNOHA  43.2 283.6   39   40   38     A     A     A    1    1
    ## 2595 007313 GUARGL  83.6  29.9   39   33   33     A     A     A    1    1
    ## 2596 007447 TRIPCU  91.6  58.9   39    0   12     A     B     A    1    2
    ## 2597 007496 INGAVE  91.4  60.2   39   49   52     A     A     A    1    1
    ## 2598 007762 TRI2PL  86.5 153.4   39   43   43     A     A     A    1    2
    ## 2599 007948 CHR2CA  88.3 193.8   39   43   44     A     A     A    1    1
    ## 2600 008076 GUARGL  85.0 213.5   39   38   38     A     A     A    1    1
    ## 2601 008165 LACIAG  92.4 226.2   39   39   40     A     A     A    1    1
    ## 2602 008275 HIRTAM  86.6 248.8   39   39   39     A     A     A    1    1
    ## 2603 008298 SWARS1  93.4 247.6   39   41   43     A     A     A    1    1
    ## 2604 008336 FARAOC  95.1 248.6   39   45   48     A     A     A    1    1
    ## 2605 008409 BACTMA  94.2 279.7   39   40   41     A     A     A    1    1
    ## 2606 008540 PIPERE 190.9  18.6   39   39   39     A     A     A    1    1
    ## 2607 008546 ALIBED 199.7  10.1   39   -1   -1     A     D     D    1    0
    ## 2608 009073 TRI2PL  78.6  17.1   39   33   33     A     A     A    1    1
    ## 2609 009157 AST2GR  62.5  43.6   39   39   41     A     A     A    1    1
    ## 2610 009406 BACTBA  60.5 127.4   39   39   40     A     A     A    1    1
    ## 2611 009426 TROPRA  69.1 126.2   39   37   32     A     A     A    1    1
    ## 2612 009571 ALIBED  66.8 179.8   39   39   39     A     A     A    1    1
    ## 2613 009618 PROTTE  78.1 179.3   39   40   39     A     A     A    1    1
    ## 2614 009761 COU2CU  68.4 213.4   39   41   44     A     A     A    1    1
    ## 2615 009768 DALBRE  65.9 206.2   39   42   41     A     A     A    1    1
    ## 2616 009808 TAB1RO  71.5 216.8   39   45   45     A     A     A    1    1
    ## 2617 009826 GUARGL  79.8 214.5   39   39   40     A     A     A    1    1
    ## 2618 010028 FARAOC  65.4 269.4   39   40   42     A     A     A    1    1
    ## 2619 010211 SOROAF 121.9  36.0   39   43   47     A     A     A    1    1
    ## 2620 010443 ANTITR 139.7  62.9   39   39   43     A     A     A    1    1
    ## 2621 010458 CLAVME 120.4  93.3   39   39   51     A     A     A    1    1
    ## 2622 01096A BACTMA   4.4 229.1   39   35    0     A     A     B    1    1
    ## 2623 000045 POSOLA  13.7   2.4   38   45   46     A     A     A    1    1
    ## 2624 000089 FARAOC  19.5  17.3   38   60   59     A     A     A    1    1
    ## 2625 000199 PROTTE  18.2  25.7   38   42   46     A     A     A    1    1
    ## 2626 000307 FISSFE   2.3  65.7   38   43   43     A     A     A    1    1
    ## 2627 000377 COU2CU  13.2  76.3   38   39   38     A     A     A    1    1
    ## 2628 000463 SWARS1   8.4  90.8   38   38   41     A     A     A    1    1
    ## 2629 000511 COU2CU  14.3  90.1   38   43   43     A     A     A    1    1
    ## 2630 001018 SOROAF   5.7 213.8   38   41   41     A     A     A    1    1
    ## 2631 001020 SOROAF   9.2 206.1   38   41   41     A     A     A    1    1
    ## 2632 001187 HEISCO   8.7 249.6   38   45   48     A     A     A    1    1
    ## 2633 001210 COU2CU  14.1 168.5   38   41   43     A     A     A    1    1
    ## 2634 001235 PIPERE   6.9 298.7   38   33   39     A     A     A    1    1
    ## 2635 001329 ALIBED   1.2 295.4   38   48   50     A     A     A    1    1
    ## 2636 001377 EUGEPR  17.9 297.1   38   44   44     A     A     A    1    1
    ## 2637 001391 BROSAL  15.8 283.3   38   42   42     A     A     A    1    1
    ## 2638 001657 BROSAL 105.4  72.3   38   39   40     A     A     A    1    1
    ## 2639 002051 HIRTRA 156.9   5.3   38   43   44     A     A     A    1    1
    ## 2640 002177 COU2CU 149.5  57.7   38   39   41     A     A     A    1    1
    ## 2641 002185 CUPASY 147.7  47.7   38   38   39     A     A     A    1    1
    ## 2642 002232 TURNPA 143.0  64.1   38   44   45     A     A     A    1    1
    ## 2643 002252 FARAOC 141.3  76.0   38   43   54     A     A     A    1    1
    ## 2644 002356 PROTTE 158.6  89.7   38   38   39     A     A     A    1    1
    ## 2645 003085 OURALU  39.5   1.2   38   41   43     A     A     A    1    1
    ## 2646 003130 COU2CU  28.8  27.0   38   49   51     A     A     A    1    1
    ## 2647 003155 TRI2PL  33.3  22.8   38   43   46     A     A     A    1    1
    ## 2648 003323 RINOLI  26.2  60.7   38   39   41     A     A     A    1    1
    ## 2649 003592 COU2CU  30.7 131.2   38   40   41     A     A     A    1    1
    ## 2650 003660 PROTTE  27.2 145.4   38   41   40     A     A     A    1    1
    ## 2651 003777 PROTTE  37.3 178.2   38   -1   -1     A     D     D    1    0
    ## 2652 003834 COU2CU  25.3 198.7   38   -1   -1     A     D     D    1    0
    ## 2653 003841 TRI2PL  28.5 185.9   38   42   47     A     A     A    1    1
    ## 2654 003897 PROTTE  33.1 196.3   38   40   42     A     A     A    1    1
    ## 2655 004005 FARAOC  34.5 206.7   38   38   37     A     A     A    1    1
    ## 2656 004012 COU2CU  30.3 214.9   38   43   47     A     A     A    1    1
    ## 2657 004121 COU2CU  39.6 221.9   38   38   39     A     A     A    1    1
    ## 2658 004143 LUEHSE  20.7 259.7   38   -9   14     A     *     A    1    0
    ## 2659 004312 FARAOC  28.6 284.6   38   41   42     A     A     A    1    1
    ## 2660 006202 COU2CU  44.5  59.1   38   40   41     A     A     A    1    1
    ## 2661 006344 COU2CU  56.3  65.8   38   49   47     A     A     A    1    1
    ## 2662 006510 COU2CU  46.2 134.4   38   40   42     A     A     A    1    1
    ## 2663 006557 COU2CU  54.0 132.6   38   -1   -1     A     D     D    1    0
    ## 2664 006726 IXORFL  52.2 173.7   38   42   45     A     A     A    1    1
    ## 2665 006732 ALIBED  52.1 180.0   38   38   39     A     A     A    1    1
    ## 2666 006973 COU2CU  42.3 222.4   38   43   44     A     A     A    1    1
    ## 2667 007024 SLOATE  50.7 229.5   38   28   28     A     A     A    1    1
    ## 2668 007108 PROTTE  43.3 263.2   38   39   39     A     A     A    1    1
    ## 2669 007445 AST2GR  92.2  55.7   38   41   43     A     A     A    1    1
    ## 2670 007732 CHR2CA  80.7 147.7   38   29   30     A     A     A    1    1
    ## 2671 007783 FARAOC  92.9 148.8   38   42   43     A     A     A    1    1
    ## 2672 007803 TRI2PL  95.0 157.3   38   47   47     A     A     A    1    1
    ## 2673 007997 SOROAF  90.8 195.1   38   40   40     A     A     A    1    2
    ## 2674 008083 ALIBED  89.0 204.8   38   39   38     A     A     A    1    1
    ## 2675 008264 COU2CU  89.1 253.5   38   42   44     A     A     A    1    1
    ## 2676 008550 BROSAL 199.2   9.6   38   40   40     A     A     A    1    1
    ## 2677 008589 LACIAG 189.6  21.3   38   44   45     A     A     A    1    1
    ## 2678 008605 LUEHSE 194.8  36.7   38   62   68     A     A     A    1    1
    ## 2679 009022 PROTTE  60.2  18.6   38   42   45     A     A     A    1    1
    ## 2680 009029 FARAOC  67.3  18.9   38   44   44     A     A     A    1    1
    ## 2681 009057 RANDFO  70.9  10.5   38   38   38     A     A     A    1    1
    ## 2682 009161 AST2GR  64.8  50.9   38    0    0     A     B     B    1    2
    ## 2683 009328 ALBIAD  64.8  86.8   38   39   39     A     A     A    1    1
    ## 2684 009567 BACTMA  67.5 178.0   38   38   47     A     A     A    1    1
    ## 2685 009745 BROSAL  62.4 208.2   38   44   45     A     A     A    1    1
    ## 2686 009991 BROSAL  75.3 250.6   38   39   36     A     A     A    1    1
    ## 2687 010055 BACTBA  73.6 275.4   38   41   40     A     A     A    1    1
    ## 2688 010069 BACTBA  76.7 279.8   38   37   37     A     A     A    1    1
    ## 2689 010121 FARAOC  70.4 289.5   38   45   48     A     A     A    1    1
    ## 2690 010267 GUARGL 137.0  24.9   38   40   40     A     A     A    1    1
    ## 2691 010339 TRIPCU 137.6  56.2   38   41   46     A     A     A    1    1
    ## 2692 010912 PIPERE 166.1   6.3   38   39   40     A     A     A    1    1
    ## 2693 011029 ALIBED 169.8  55.1   38   32   31     A     A     A    1    1
    ## 2694 011189 HIRTAM 178.9  93.5   38   42   39     A     A     A    1    1
    ## 2695 011223 ANNOHA 193.4  50.9   38   41   41     A     A     A    1    1
    ## 2696 011259 LUEHSE 198.9  41.3   38   52   55     A     A     A    1    1
    ## 2697 011317 PICRLA 196.9  72.8   38   41   42     A     A     A    1    1
    ## 2698 000180 AST2GR  10.4  37.3   37   45   45     A     A     A    1    1
    ## 2699 000230 EUGEPR   9.6  51.7   37   43   47     A     A     A    1    1
    ## 2700 000244 TRIPCU  11.1  48.4   37   46   55     A     A     A    1    1
    ## 2701 000253 TRIPCU  14.5  56.2   37   -9    0     A     *     B    1    0
    ## 2702 000262 ANTITR  17.5  55.9   37   40   43     A     A     A    1    1
    ## 2703 000647 HEISCO  13.4 105.4   37   41   44     A     A     A    1    1
    ## 2704 000769 BACTMA  16.0 124.5   37   39   37     A     A     A    1    1
    ## 2705 001100 BACTMA   0.9 234.6   37   33   33     A     A     A    1    1
    ## 2706 001256 FARAOC   5.8 279.4   37   40   44     A     A     A    1    1
    ## 2707 001285 FARAOC  12.0 271.3   37   45   46     A     A     A    1    1
    ## 2708 001407 IXORFL 109.1  16.1   37   38   45     A     A     A    1    2
    ## 2709 001611 TRI2PL 119.1  51.2   37   37   41     A     A     A    1    2
    ## 2710 001651 AST2GR 105.2  77.7   37   38   38     A     A     A    1    1
    ## 2711 001718 AST2GR 116.7  63.5   37   46   51     A     A     A    1    1
    ## 2712 001754 ALIBED 114.3  83.3   37   37   32     A     A     A    1    2
    ## 2713 002079 TRI2PL 145.0  39.3   37   37   37     A     A     A    1    1
    ## 2714 002262 SOROAF 145.7  70.7   37   38   41     A     A     A    1    1
    ## 2715 003062 PIT1RU  31.2  15.6   37   42   43     A     A     A    1    1
    ## 2716 003159 EUGEPR  34.2  25.7   37   40   40     A     A     A    1    1
    ## 2717 003391 COU2CU  28.8  90.2   37   39   43     A     A     A    1    1
    ## 2718 003511 FARAOC  31.4 115.4   37   40   42     A     A     A    1    1
    ## 2719 003647 SWARS1  20.7 155.2   37   40   47     A     A     A    1    1
    ## 2720 004113 SOROAF  36.7 220.4   37   39   39     A     A     A    1    1
    ## 2721 004177 BROSAL  33.8 258.6   37   46   45     A     A     A    1    1
    ## 2722 004245 BACTMA  34.6 270.2   37   38   38     A     A     A    1    1
    ## 2723 006122 PIT1RU  43.7  37.7   37   42   43     A     A     A    1    1
    ## 2724 006318 EUGEPR  47.6  60.5   37   38   39     A     A     A    1    1
    ## 2725 006692 FARAOC  48.1 178.0   37   42   43     A     A     A    1    1
    ## 2726 006767 PROTTE  41.9 188.0   37   58   64     A     A     A    1    1
    ## 2727 006858 FARAOC  56.3 186.2   37   45   46     A     A     A    1    1
    ## 2728 006977 IXORFL  43.8 225.5   37   39   39     A     A     A    1    1
    ## 2729 007034 FARAOC  50.1 240.0   37   39   41     A     A     A    1    1
    ## 2730 007154 PROTTE  53.3 266.8   37   38   39     A     A     A    1    1
    ## 2731 007224 ANNOHA  46.7 283.3   37   35   38     A     A     A    1    1
    ## 2732 007228 BACTC2  54.7 284.6   37   36   36     A     A     A    1    1
    ## 2733 007446 LOPIDA  90.8  59.2   37   -1   -1     A     D     D    1    0
    ## 2734 007452 CASECO  99.9  57.4   37   38   39     A     A     A    1    1
    ## 2735 007740 ACACME  80.1 152.4   37   36   36     A     A     A    1    1
    ## 2736 007743 SOROAF  82.4 154.3   37   40   44     A     A     A    1    1
    ## 2737 007834 BROSAL  83.9 173.0   37   40   43     A     A     A    1    1
    ## 2738 007895 PROTTE  96.7 169.8   37   37   39     A     A     A    1    1
    ## 2739 008064 CUPASY  84.6 216.7   37   40   41     A     A     A    1    1
    ## 2740 008093 BROSAL  90.1 205.3   37   38   38     A     A     A    1    1
    ## 2741 008113 PROTTE  98.3 207.1   37   41   43     A     A     A    1    1
    ## 2742 008285 CHR2CA  89.7 243.4   37   40   48     A     A     A    1    1
    ## 2743 008371 PROTTE  89.1 265.6   37   38   41     A     A     A    1    1
    ## 2744 008416 ALIBED  98.6 271.9   37   -1   -1     A     D     D    1    0
    ## 2745 008695 BROSAL  81.7 177.1   37   37   41     A     A     A    1    1
    ## 2746 009023 TRIPCU  63.7  18.7   37   38   38     A     A     A    1    1
    ## 2747 009104 CUPASY  65.6  36.2   37   37   35     A     A     A    1    1
    ## 2748 009170 ANNOHA  60.3  59.4   37   38   38     A     A     A    1    1
    ## 2749 009434 PROTTE  74.0 125.2   37   43   43     A     A     A    1    1
    ## 2750 009728 BROSAL  78.6 182.8   37   37   37     A     A     A    1    1
    ## 2751 009763 ALIBED  69.8 205.7   37   38   39     A     A     A    1    1
    ## 2752 009811 GUARGR  73.7 217.5   37   43   46     A     A     A    1    1
    ## 2753 009866 BROSAL  64.5 231.5   37   38   38     A     A     A    1    1
    ## 2754 009874 GUARGL  69.5 230.7   37   39   39     A     A     A    1    1
    ## 2755 009976 ALIBED  72.5 259.3   37   38   38     A     A     A    1    1
    ## 2756 010134 GUARGL  76.3 289.8   37   38   39     A     A     A    1    1
    ## 2757 010156 COU2CU 128.9  16.3   37   39   39     A     A     A    1    1
    ## 2758 010227 AST2GR 126.9  27.7   37   39   39     A     A     A    1    1
    ## 2759 010275 POSOLA 121.4  45.2   37   39   42     A     A     A    1    1
    ## 2760 010278 STYLST 121.7  47.0   37   38   37     A     A     A    1    1
    ## 2761 010378 INGAVE 122.0  68.0   37   -1   -1     A     D     D    1    0
    ## 2762 011022 HIRTAM 163.6  48.4   37   41   41     A     A     A    1    1
    ## 2763 011023 POSOLA 161.8  51.2   37   44   43     A     A     A    1    1
    ## 2764 011093 LACIAG 162.6  75.5   37   41   42     A     A     A    1    1
    ## 2765 011109 HIRTRA 169.3  71.6   37   44   43     A     A     A    1    1
    ## 2766 000115 ANNOHA  19.8   2.5   36   37   41     A     A     A    1    1
    ## 2767 000490 EUGECO   7.4  84.5   36   37   40     A     A     A    1    1
    ## 2768 000533 BROSAL  19.0  95.3   36   36   35     A     A     A    1    1
    ## 2769 000549 EUGEPR  16.0  94.8   36   38   38     A     A     A    1    1
    ## 2770 000604 OURALU   5.5 119.9   36   36   38     A     A     A    1    1
    ## 2771 000692 FARAOC   3.2 121.5   36   38   39     A     A     A    1    1
    ## 2772 000757 FARAOC  15.8 131.6   36   37   38     A     A     A    1    1
    ## 2773 000773 BACTMA   0.2 150.2   36   38   37     A     A     A    1    1
    ## 2774 000799 HEISCO   5.3 142.1   36   -1   -1     A     D     D    1    0
    ## 2775 000805 BROSAL  10.8 145.1   36   36   37     A     A     A    1    1
    ## 2776 000841 COCCPA   0.2 171.5   36   38   40     A     A     A    1    1
    ## 2777 000994 CUPASY  19.5 183.5   36   37   41     A     A     A    1    1
    ## 2778 001058 BROSAL  16.6 210.7   36   37   39     A     A     A    1    2
    ## 2779 001099 BACTMA   0.4 231.5   36   32   32     A     A     A    1    1
    ## 2780 001145 FARAOC  14.5 235.7   36   34   36     A     A     A    1    1
    ## 2781 001260 ORMOMA   7.3 273.3   36   36   36     A     A     A    1    1
    ## 2782 001419 ALIBED 109.4   7.6   36   36   38     A     A     A    1    1
    ## 2783 002039 GUARGL 152.1  19.1   36   36   36     A     A     A    1    1
    ## 2784 002061 EUGEPR 142.4  20.6   36   40   43     A     A     A    1    1
    ## 2785 002084 TRIPCU 148.0  27.1   36   46   48     A     A     A    1    1
    ## 2786 002261 COU2CU 145.1  70.2   36   42   45     A     A     A    1    1
    ## 2787 003149 TRIPCU  31.4  20.9   36   40   41     A     A     A    1    1
    ## 2788 003290 ANNOHA  35.5  42.9   36   34   35     A     A     A    1    1
    ## 2789 003295 EUGEPR  21.3  60.3   36   40   38     A     A     A    1    1
    ## 2790 003359 CASESY  35.6  74.2   36   42   44     A     A     A    1    1
    ## 2791 003612 SOROAF  38.0 125.4   36   37   38     A     A     A    1    1
    ## 2792 003803 COCCPA  23.7 182.5   36   38   45     A     A     A    1    2
    ## 2793 003812 IXORFL  23.0 189.7   36   41   46     A     A     A    1    1
    ## 2794 003848 HEISCO  26.2 184.1   36   39   43     A     A     A    1    1
    ## 2795 004212 MACRGL  24.5 275.4   36   37   40     A     A     A    1    1
    ## 2796 006066 LACIAG  54.5  10.6   36   38   38     A     A     A    1    1
    ## 2797 006133 EUGEPR  45.9  27.3   36   36   37     A     A     A    1    1
    ## 2798 006154 ANNOHA  50.2  29.2   36   36   38     A     A     A    1    1
    ## 2799 006428 PROTTE  41.2 117.1   36   35   37     A     A     A    1    1
    ## 2800 006489 COU2CU  43.0 133.6   36   37   42     A     A     A    1    1
    ## 2801 006606 BACTBA  42.0 153.5   36   39   39     A     A     A    1    1
    ## 2802 006693 FARAOC  48.0 178.3   36   36   37     A     A     A    1    1
    ## 2803 006766 COU2CU  42.1 187.3   36   28   26     A     A     A    1    1
    ## 2804 006768 ACALDI  44.0 191.2   36   35   35     A     A     A    1    1
    ## 2805 006926 COU2CU  50.1 200.2   36    0    0     A     B     B    1    2
    ## 2806 007004 COU2CU  48.9 221.3   36   39   40     A     A     A    1    1
    ## 2807 007020 INGAFA  53.5 221.0   36   40   40     A     A     A    1    1
    ## 2808 007159 PROTTE  50.2 272.5   36   39   39     A     A     A    1    1
    ## 2809 007418 ALIBED  87.4  49.0   36   25   25     A     A     A    1    1
    ## 2810 007488 EUGEPR  89.3  78.7   36   27   27     A     A     A    1    1
    ## 2811 007529 COCCPA  84.0  80.8   36   36   38     A     A     A    1    1
    ## 2812 007641 POSOLA  83.4 124.8   36   27   28     A     A     A    1    1
    ## 2813 007659 ANNOHA  82.1 130.3   36   36   38     A     A     A    1    1
    ## 2814 007768 MYROFR  89.4 148.5   36   -1   -1     A     D     D    1    0
    ## 2815 007971 IXORFL  90.9 180.6   36   49   53     A     A     A    1    1
    ## 2816 008378 PROTTE  86.1 260.7   36   40   41     A     A     A    1    1
    ## 2817 008410 PROTTE  92.4 276.7   36   40   40     A     A     A    1    1
    ## 2818 008461 PROTTE  88.3 282.6   36   45   48     A     A     A    1    1
    ## 2819 008471 CUPASY  96.5 295.0   36   37   37     A     A     A    1    1
    ## 2820 009108 BACTMA  68.2  30.8   36   37   37     A     A     A    1    1
    ## 2821 009173 AST2GR  65.7  55.5   36   38   38     A     A     A    1    1
    ## 2822 009335 FARAOC  65.4  81.1   36   44   46     A     A     A    1    1
    ## 2823 009428 EUGEPR  65.2 128.6   36   38   39     A     A     A    1    1
    ## 2824 009562 BROSAL  60.5 179.9   36   38   39     A     A     A    1    1
    ## 2825 009667 FARAOC  69.0 193.8   36   -1   -1     A     D     D    1    0
    ## 2826 009771 EUGEPR  66.7 208.9   36   38   39     A     A     A    1    1
    ## 2827 009806 CUPASY  73.4 215.7   36   42   46     A     A     A    1    1
    ## 2828 009848 ADE1TR  79.3 201.8   36   43   46     A     A     A    1    1
    ## 2829 009972 BACTMA  70.3 253.9   36   36    0     A     A     B    1    1
    ## 2830 010235 PROTTE 132.6  24.0   36   41   45     A     A     A    1    1
    ## 2831 010262 FARAOC 138.8  34.8   36   37   42     A     A     A    1    1
    ## 2832 010284 CAL2CA 124.7  51.2   36   36   -1     A     A     D    1    1
    ## 2833 010325 BROSAL 133.5  54.9   36   42   47     A     A     A    1    1
    ## 2834 010331 RANDFO 131.1  56.2   36   37   39     A     A     A    1    1
    ## 2835 010332 GUAZUL 130.6  58.8   36   37   37     A     A     A    1    1
    ## 2836 010445 CORDAL 121.1  80.7   36   39   42     A     A     A    1    2
    ## 2837 010473 TRIPCU 129.3  94.3   36   36   41     A     A     A    1    1
    ## 2838 010522 LACIAG 139.4  87.3   36   42   47     A     A     A    1    1
    ## 2839 010926 FARAOC 170.1   9.8   36   43   44     A     A     A    1    1
    ## 2840 010956 STERAP 164.9  20.1   36   37   38     A     A     A    1    1
    ## 2841 010986 PIPERE 170.3  24.5   36   39   38     A     A     A    1    1
    ## 2842 011255 PICRLA 196.5  44.1   36   48   49     A     A     A    1    1
    ## 2843 011266 POUTCA 183.2  70.4   36   37   38     A     A     A    1    1
    ## 2844 011288 TRIPCU 185.6  60.8   36   38   40     A     A     A    1    1
    ## 2845 000152 SWARS1   5.5  29.8   35   47   55     A     A     A    1    1
    ## 2846 000336 EUGEPR   8.0  66.6   35   39   40     A     A     A    1    2
    ## 2847 000363 ANTITR  12.1  62.2   35   49   50     A     A     A    1    1
    ## 2848 000502 COU2CU  11.3  89.5   35   49   49     A     A     A    1    1
    ## 2849 000507 EUGEPR  14.7  86.4   35   39   40     A     A     A    1    1
    ## 2850 000535 COU2CU  14.8  97.4   35   39   40     A     A     A    1    1
    ## 2851 000554 COU2CU  15.8  86.6   35   34   34     A     A     A    1    1
    ## 2852 000622 ANNOHA   5.1 105.3   35   32   37     A     A     A    1    1
    ## 2853 000711 PIPERE   6.9 135.1   35   36   36     A     A     A    1    1
    ## 2854 000724 PICRLA   5.4 134.2   35   34   -1     A     A     D    1    1
    ## 2855 000798 FARAOC   5.7 141.5   35   36   40     A     A     A    1    1
    ## 2856 000969 SOROAF  10.5 185.4   35   35   35     A     A     A    1    1
    ## 2857 001177 COU2CU   2.7 259.4   35   41   45     A     A     A    1    1
    ## 2858 001304 COU2CU  15.2 267.9   35   36   36     A     A     A    1    1
    ## 2859 002066 HIRTRA 140.2  27.2   35   40   40     A     A     A    1    1
    ## 2860 002100 TRIPCU 153.1  25.1   35   38    0     A     A     B    1    1
    ## 2861 002106 RINOLI 154.1  34.9   35   35   35     A     A     A    1    1
    ## 2862 002162 BROSAL 143.0  57.1   35   43   47     A     A     A    1    1
    ## 2863 002178 PROTTE 149.1  57.0   35   44   46     A     A     A    1    1
    ## 2864 002244 ALIBED 143.5  68.2   35   36   36     A     A     A    1    1
    ## 2865 002332 IXORFL 147.7  99.3   35   40   40     A     A     A    1    1
    ## 2866 003058 SOROAF  31.9   8.8   35   39   39     A     A     A    1    1
    ## 2867 003175 CASEGU  32.8  32.3   35   37   40     A     A     A    1    1
    ## 2868 003258 ACALDI  38.0  58.6   35   35    0     A     A     B    1    1
    ## 2869 003283 FARAOC  37.4  45.2   35   49   -1     A     A     D    1    1
    ## 2870 003397 COU2CU  26.2  89.9   35   39   40     A     A     A    1    1
    ## 2871 003438 PROTTE  37.6  80.4   35   35   36     A     A     A    1    1
    ## 2872 003554 COU2CU  22.2 134.6   35   37   38     A     A     A    1    1
    ## 2873 003739 ALIBED  27.7 171.0   35   36   36     A     A     A    1    1
    ## 2874 003906 INGAVE  37.9 196.5   35   40   48     A     A     A    1    1
    ## 2875 003928 BACTMA  39.5 185.3   35   30    0     A     A     B    1    1
    ## 2876 004226 MACRGL  27.4 267.1   35   42   57     A     A     A    1    1
    ## 2877 004287 HEISCO  26.6 297.3   35   41   41     A     A     A    1    1
    ## 2878 006077 COU2CU  55.6  10.0   35   41   42     A     A     A    1    1
    ## 2879 006092 ALIBED  59.8  63.6   35   36   37     A     A     A    1    1
    ## 2880 006131 COU2CU  47.2  25.9   35   36   40     A     A     A    1    1
    ## 2881 006160 GUARGL  50.3  32.7   35   33   -1     A     A     D    1    1
    ## 2882 006164 COU2CU  54.9  34.6   35   39   42     A     A     A    1    1
    ## 2883 006176 COU2CU  56.2  26.4   35   -1   -1     A     D     D    1    0
    ## 2884 006256 COU2CU  57.6  56.5   35   39   42     A     A     A    1    1
    ## 2885 006607 ALIBED  42.2 153.5   35   39   39     A     A     A    1    1
    ## 2886 006619 EUGEPR  53.6 140.2   35   35   37     A     A     A    1    1
    ## 2887 006779 PIPERE  49.5 194.9   35   38   37     A     A     A    1    1
    ## 2888 006814 ACALDI  48.9 187.5   35   -1   -1     A     D     D    1    0
    ## 2889 006913 CUPASY  47.3 206.6   35   43   48     A     A     A    1    1
    ## 2890 007202 BACTBA  43.9 295.6   35   39   39     A     A     A    1    1
    ## 2891 007413 GUARGL  85.2  51.5   35   33   35     A     A     A    1    1
    ## 2892 007415 AST2GR  89.3  52.0   35   35   -1     A     A     D    1    1
    ## 2893 007450 LUEHSE  97.3  56.6   35   -1   -1     A     D     D    1    0
    ## 2894 007541 GUARGL  84.8  98.6   35   36   35     A     A     A    1    1
    ## 2895 007691 PROTTE  85.6 123.7   35   38   39     A     A     A    1    1
    ## 2896 007700 GUARGL  90.8 130.9   35   36   36     A     A     A    1    1
    ## 2897 007720 TROPRA  99.0 127.5   35   41   43     A     A     A    1    1
    ## 2898 007866 POUTCA  93.3 161.9   35   37   40     A     A     A    1    1
    ## 2899 007904 BROSAL  98.5 163.2   35   41   43     A     A     A    1    1
    ## 2900 008034 CUPASY  84.8 200.0   35   40   38     A     A     A    1    1
    ## 2901 008100 BROSAL  93.4 212.8   35   36   36     A     A     A    1    1
    ## 2902 008287 FARAOC  92.4 240.0   35   39   39     A     A     A    1    1
    ## 2903 008327 ALIBED  96.2 253.6   35   39   38     A     A     A    1    1
    ## 2904 008414 BACTMA  97.4 278.7   35   35   33     A     A     A    1    1
    ## 2905 008693 ALIBED  80.2 110.9   35   -1   -1     A     D     D    1    0
    ## 2906 009457 SOROAF  76.0 131.2   35   38   38     A     A     A    1    1
    ## 2907 009507 BACTMA  65.6 142.6   35   45   46     A     A     A    1    1
    ## 2908 009528 CHR2CA  77.4 159.9   35   36   37     A     A     A    1    2
    ## 2909 009538 TRI2PL  79.6 149.4   35   50   53     A     A     A    1    1
    ## 2910 009569 BACTMA  66.0 175.6   35   34   38     A     A     A    1    1
    ## 2911 009586 EUGEPR  73.3 161.2   35   38   38     A     A     A    1    1
    ## 2912 009690 SOROAF  72.9 191.1   35   36   39     A     A     A    1    1
    ## 2913 009695 MYRCGA  72.6 193.3   35   36   36     A     A     A    1    1
    ## 2914 009859 FARAOC  64.4 230.8   35    0   -1     A     B     D    1    2
    ## 2915 009891 ADE1TR  74.0 220.5   35   47   51     A     A     A    1    1
    ## 2916 009920 CHR2CA  76.5 234.8   35   36   39     A     A     A    1    2
    ## 2917 009933 PROTTE  77.3 223.9   35   41   42     A     A     A    1    1
    ## 2918 009946 BACTMA  63.1 248.5   35    0   -1     A     B     D    1    2
    ## 2919 010150 EUGEPR 127.2  16.4   35   44   40     A     A     A    1    1
    ## 2920 010237 PROTTE 134.2  24.2   35   39   39     A     A     A    1    1
    ## 2921 010417 GENIAM 131.3  68.9   35   38   38     A     A     A    1    1
    ## 2922 010431 ANNOHA 135.9  71.2   35   35   23     A     A     A    1    1
    ## 2923 010520 TRI2PL 137.9  89.7   35   38   41     A     A     A    1    1
    ## 2924 010947 PIPERE 175.8  17.0   35   35   36     A     A     A    1    1
    ## 2925 010978 RANDFO 168.5  27.8   35   41   41     A     A     A    1    1
    ## 2926 011358 BROSAL 194.5  87.2   35   37   42     A     A     A    1    1
    ## 2927 000051 POUTCA  13.8   8.4   34   36   37     A     A     A    1    1
    ## 2928 000140 COU2CU   6.6  35.6   34   37   40     A     A     A    1    1
    ## 2929 000151 NEEADE   6.7  26.5   34   34   35     A     A     A    1    1
    ## 2930 000205 COU2CU  19.2  27.7   34   38   38     A     A     A    1    1
    ## 2931 000260 EUGEPR  12.9  56.7   34   36   40     A     A     A    1    1
    ## 2932 000569 EUGEPR  18.7  82.7   34   39   37     A     A     A    1    1
    ## 2933 000674 COU2CU  16.6 109.8   34   31    0     A     A     B    1    1
    ## 2934 000775 IXORFL   0.2 154.2   34   37   38     A     A     A    1    1
    ## 2935 000819 SWARS1  15.2 155.9   34   36   37     A     A     A    1    1
    ## 2936 000974 FARAOC  12.5 190.3   34   42   43     A     A     A    1    1
    ## 2937 001128 SOROAF  13.4 222.9   34   39   -1     A     A     D    1    1
    ## 2938 001169 CLAVME   0.9 254.6   34   36   43     A     A     A    1    1
    ## 2939 001471 MYRCGA 116.2   8.7   34   34   34     A     A     A    1    1
    ## 2940 001473 DENDAR 115.6   8.9   34   34   31     A     A     A    1    1
    ## 2941 001547 CUPASY 103.7  53.6   34   38   41     A     A     A    1    1
    ## 2942 001553 AST2GR 108.4  56.2   34   36   35     A     A     A    1    2
    ## 2943 001671 AST2GR 111.6  61.8   34   38   40     A     A     A    1    2
    ## 2944 001679 AST2GR 114.9  69.9   34   39   47     A     A     A    1    1
    ## 2945 002134 RINOLI 159.2  34.8   34   35   35     A     A     A    1    1
    ## 2946 002318 HEISCO 143.5  89.3   34   48   55     A     A     A    1    1
    ## 2947 003071 BACTMA  38.8  15.9   34   32   31     A     A     A    1    1
    ## 2948 003243 COU2CU  28.8  52.9   34   45   46     A     A     A    1    1
    ## 2949 003336 SWARS1  30.3  61.7   34   36   38     A     A     A    1    1
    ## 2950 003493 EUGEPR  30.2 107.9   34   37   38     A     A     A    1    1
    ## 2951 003504 COU2CU  31.1 111.1   34   37   40     A     A     A    1    1
    ## 2952 003558 SWARS1  22.5 138.9   34   37   39     A     A     A    1    1
    ## 2953 003685 COU2CU  32.5 150.5   34   40   37     A     A     A    1    1
    ## 2954 003892 PROTTE  33.3 191.8   34   39   42     A     A     A    1    1
    ## 2955 003950 PROTTE  21.5 201.2   34   34   37     A     A     A    1    1
    ## 2956 003963 SWARS1  24.1 211.1   34   37   40     A     A     A    1    1
    ## 2957 003971 SOROAF  21.5 216.0   34   37   37     A     A     A    1    1
    ## 2958 003994 FARAOC  26.7 200.5   34   38   39     A     A     A    1    1
    ## 2959 004024 BROSAL  32.2 217.8   34   37   38     A     A     A    1    1
    ## 2960 004246 BACTMA  33.5 271.5   34   33   31     A     A     A    1    1
    ## 2961 004294 POSOLA  25.8 293.3   34   38   38     A     A     A    1    1
    ## 2962 006010 TURNPA  42.5   5.6   34   37   36     A     A     A    1    1
    ## 2963 006038 CUPASY  47.9  12.1   34   38   39     A     A     A    1    1
    ## 2964 006049 BROSAL  50.1   2.1   34   40   38     A     A     A    1    1
    ## 2965 006147 COU2CU  48.1  21.8   34   38   46     A     A     A    1    1
    ## 2966 006246 FARAOC  58.0  56.4   34   39   42     A     A     A    1    1
    ## 2967 006253 FARAOC  58.5  57.8   34   36   38     A     A     A    1    1
    ## 2968 006845 FARAOC  57.8 196.4   34   45   45     A     A     A    1    1
    ## 2969 006891 COU2CU  46.5 218.6   34   42   46     A     A     A    1    1
    ## 2970 006906 FARAOC  49.8 211.8   34   39   39     A     A     A    1    1
    ## 2971 007010 COU2CU  49.1 222.4   34   35   36     A     A     A    1    1
    ## 2972 007306 CUPASY  83.2  20.2   34   34   34     A     A     A    1    1
    ## 2973 007376 CUPASY  95.8  32.1   34   38   41     A     A     A    1    1
    ## 2974 007752 AST2GR  89.2 156.3   34   36   37     A     A     A    1    1
    ## 2975 007923 BROSAL  83.4 195.4   34   35   36     A     A     A    1    1
    ## 2976 007939 HEISCO  87.4 197.6   34   44   50     A     A     A    1    1
    ## 2977 007986 BACTMA  94.5 190.2   34   33   33     A     A     A    1    1
    ## 2978 007995 FARAOC  94.6 191.5   34   48   53     A     A     A    1    1
    ## 2979 008026 BACTMA  97.1 187.1   34   35   37     A     A     A    1    1
    ## 2980 008312 BROSAL  99.2 255.2   34   34   35     A     A     A    1    1
    ## 2981 008319 POUTCA  98.6 257.5   34   36   40     A     A     A    1    1
    ## 2982 009032 CUPASY  65.4  10.7   34   42   46     A     A     A    1    1
    ## 2983 009039 FARAOC  65.1   7.7   34   42   44     A     A     A    1    1
    ## 2984 009093 FARAOC  64.6  30.6   34   39   41     A     A     A    1    1
    ## 2985 009240 COU2CU  63.4  61.2   34   37   37     A     A     A    1    1
    ## 2986 009330 COPAAR  60.1  96.7   34   35   34     A     A     A    1    1
    ## 2987 009368 ALIBED  64.5  95.6   34   35   35     A     A     A    1    1
    ## 2988 009431 SWARS1  68.8 129.3   34   34   34     A     A     A    1    1
    ## 2989 009433 ALIBED  72.5 121.2   34   34   34     A     A     A    1    1
    ## 2990 009480 PIPERE  60.3 142.3   34   39   41     A     A     A    1    1
    ## 2991 009564 BACTMA  63.8 177.6   34   34   34     A     A     A    1    1
    ## 2992 009638 INGAFA  61.3 187.1   34   35   26     A     A     A    1    1
    ## 2993 009646 BROSAL  62.4 198.8   34   39   41     A     A     A    1    1
    ## 2994 009648 FARAOC  63.8 199.1   34   42   43     A     A     A    1    1
    ## 2995 009669 COU2CU  67.0 185.9   34   35   37     A     A     A    1    1
    ## 2996 009730 FARAOC  64.2 200.2   34   44   46     A     A     A    1    1
    ## 2997 009754 CUPASY  65.4 218.8   34   39   43     A     A     A    1    1
    ## 2998 009773 COU2CU  69.7 206.2   34   44   48     A     A     A    1    1
    ## 2999 009792 CUPASY  73.6 202.8   34   37   40     A     A     A    1    1
    ## 3000 009864 HEISCO  61.4 234.1   34   39   43     A     A     A    1    1
    ## 3001 009912 FARAOC  75.5 237.5   34   38   38     A     A     A    1    1
    ## 3002 009913 GENIAM  75.5 239.8   34   -1   -1     A     D     D    1    0
    ## 3003 009954 CAL2CA  66.9 250.4   34   -1   -1     A     D     D    1    0
    ## 3004 009980 ALIBED  77.1 255.7   34   34   35     A     A     A    1    1
    ## 3005 010036 HIRTRA  73.6 260.1   34   35   34     A     A     A    1    1
    ## 3006 010176 TRI2PL 132.5  14.8   34   35   16     A     A     A    1    1
    ## 3007 010242 TRIPCU 133.7  26.6   34   34   35     A     A     A    1    1
    ## 3008 010324 AST2GR 130.0  53.4   34   35   35     A     A     A    1    1
    ## 3009 010479 ALIBED 126.5  82.5   34   39   36     A     A     A    1    1
    ## 3010 010488 EUGECO 134.1  89.3   34   40   44     A     A     A    1    1
    ## 3011 010943 PIPERE 179.4  15.3   34    0    0     A     B     B    1    2
    ## 3012 011064 CHR2CA 178.6  50.9   34   37   36     A     A     A    1    1
    ## 3013 011135 HIRTRA 175.4  75.2   34   37   36     A     A     A    1    1
    ## 3014 011164 HIRTAM 169.1  93.1   34   39   43     A     A     A    1    1
    ## 3015 011178 PICRLA 170.3  96.8   34   41   44     A     A     A    1    1
    ## 3016 011388 ALIBED  63.3 265.8   34   34   32     A     A     A    1    1
    ## 3017 000013 BACTMA   2.0  16.2   33   38    0     A     A     B    1    1
    ## 3018 000168 COU2CU  16.2 274.2   33   34   32     A     A     A    1    1
    ## 3019 000240 PROTTE  10.4  41.5   33   32   33     A     A     A    1    1
    ## 3020 000310 CUPASY   1.5  68.8   33   36   37     A     A     A    1    1
    ## 3021 000406 BROSAL  19.0  64.6   33   34   34     A     A     A    1    1
    ## 3022 000436 COU2CU   1.8  89.1   33   37   38     A     A     A    1    1
    ## 3023 000627 EUGEPR   9.3 100.5   33   34   35     A     A     A    1    1
    ## 3024 000628 EUGEPR   5.4 102.5   33   34   35     A     A     A    1    1
    ## 3025 000681 POSOLA  16.8 102.6   33   32   33     A     A     A    1    1
    ## 3026 000945 POSOLA   5.3 199.9   33   37   40     A     A     A    1    1
    ## 3027 000976 FARAOC  11.9 193.2   33   34   35     A     A     A    1    1
    ## 3028 001049 SOROAF  13.7 207.3   33   35   36     A     A     A    1    1
    ## 3029 001053 PROTTE  12.6 212.7   33   33   34     A     A     A    1    1
    ## 3030 001082 FARAOC  17.3 202.4   33   36   38     A     A     A    1    1
    ## 3031 001086 BACTMA   0.2 224.7   33   33   34     A     A     A    1    1
    ## 3032 001166 AST2GR   2.5 250.5   33   32   34     A     A     A    1    1
    ## 3033 001270 SOROAF   5.9 262.4   33   36   38     A     A     A    1    1
    ## 3034 001337 CAVAPL   6.1 295.9   33   -1   -1     A     D     D    1    0
    ## 3035 001467 EUGEPR 116.0  14.3   33   37   38     A     A     A    1    1
    ## 3036 001476 GUARGL 118.0   1.9   33   -1   -1     A     D     D    1    0
    ## 3037 001554 CUPASY 106.4  58.0   33   36   36     A     A     A    1    1
    ## 3038 001737 IXORFL 100.3  97.4   33   38   39     A     A     A    1    1
    ## 3039 002070 PICRLA 143.9  29.0   33   33   35     A     A     A    1    1
    ## 3040 002119 RINOLI 152.7  34.7   33   33   34     A     A     A    1    1
    ## 3041 002129 RINOLI 159.3  30.9   33   -1   -1     A     D     D    1    0
    ## 3042 002231 INGAVE 142.2  63.6   33   36   38     A     A     A    1    1
    ## 3043 003002 AST2GR  20.8   2.4   33   33   33     A     A     A    1    1
    ## 3044 003220 COU2CU  20.1  58.9   33   38   39     A     A     A    1    1
    ## 3045 003351 FARAOC  33.4  79.5   33   35   36     A     A     A    1    1
    ## 3046 003370 EUGEPR  24.7  83.9   33   35   39     A     A     A    1    1
    ## 3047 003380 COU2CU  20.7  91.0   33   44   49     A     A     A    1    1
    ## 3048 003402 COU2CU  29.7  87.8   33   35   36     A     A     A    1    1
    ## 3049 003591 COU2CU  32.6 131.1   33   36   37     A     A     A    1    1
    ## 3050 003708 COU2CU  35.4 140.4   33   36   35     A     A     A    1    1
    ## 3051 003775 BROSAL  34.8 179.2   33   37   41     A     A     A    1    1
    ## 3052 003980 SOROAF  27.5 210.7   33   35   37     A     A     A    1    1
    ## 3053 003993 SOROAF  29.2 209.6   33   32   34     A     A     A    1    1
    ## 3054 004164 MACRGL  34.7 245.4   33   33   34     A     A     A    1    1
    ## 3055 004167 MACRGL  33.2 251.0   33   35   36     A     A     A    1    1
    ## 3056 004224 BROSAL  28.1 265.6   33   34   35     A     A     A    1    1
    ## 3057 004252 INGAFA  35.5 275.5   33   34   35     A     A     A    1    1
    ## 3058 004261 FARAOC  39.7 266.9   33   37   39     A     A     A    1    1
    ## 3059 004279 SWARS1  21.4 291.6   33   43   39     A     A     A    1    1
    ## 3060 006157 EUGEPR  51.7  30.2   33   33   35     A     A     A    1    1
    ## 3061 006217 PROTTE  48.4  46.2   33   35   38     A     A     A    1    1
    ## 3062 006234 CUPASY  53.5  53.8   33   33   36     A     A     A    1    1
    ## 3063 006423 SOROAF  40.1 114.2   33   36   38     A     A     A    1    1
    ## 3064 006527 COU2CU  48.6 123.7   33   36   38     A     A     A    1    1
    ## 3065 006833 COU2CU  52.3 188.9   33   32   33     A     A     A    1    1
    ## 3066 006932 HIRTRA  54.8 203.1   33    0   -1     A     B     D    1    2
    ## 3067 007190 COU2CU  40.8 280.1   33   37   41     A     A     A    1    1
    ## 3068 007286 EUGEPR  90.8   9.1   33   33   36     A     A     A    1    1
    ## 3069 007374 RANDFO  98.9  32.6   33   34   33     A     A     A    1    1
    ## 3070 007523 AST2GR  98.6  69.6   33   36   37     A     A     A    1    1
    ## 3071 007890 ANNOHA  97.9 170.3   33   34   33     A     A     A    1    1
    ## 3072 007959 ANNOHA  86.7 187.7   33   39   40     A     A     A    1    1
    ## 3073 007972 BACTMA  91.6 183.3   33   32   32     A     A     A    1    1
    ## 3074 007992 LACIAG  92.6 194.8   33   35   37     A     A     A    1    1
    ## 3075 008025 ALIBED  99.0 187.4   33   34   35     A     A     A    1    1
    ## 3076 008033 BACTMA  99.5 182.4   33   32   33     A     A     A    1    1
    ## 3077 008104 PROTTE  91.7 217.5   33   39   42     A     A     A    1    1
    ## 3078 008163 HIRTRA  93.3 221.4   33   32   33     A     A     A    1    1
    ## 3079 008208 BROSAL  97.5 231.9   33   30   30     A     A     A    1    1
    ## 3080 008233 SOROAF  84.3 249.8   33   35   36     A     A     A    1    1
    ## 3081 008415 BROSAL  99.1 276.4   33   33   35     A     A     A    1    1
    ## 3082 008484 LACIAG 182.7   4.0   33   35   36     A     A     A    1    1
    ## 3083 008593 CAVAPL 190.2  21.0   33   -1   -1     A     D     D    1    0
    ## 3084 009016 SOROAF  61.9  16.0   33   38   39     A     A     A    1    1
    ## 3085 009159 BROSAL  64.0  46.3   33   36   37     A     A     A    1    1
    ## 3086 009178 AST2GR  69.1  58.1   33   33    0     A     A     B    1    2
    ## 3087 009216 COU2CU  70.2  57.5   33   37   41     A     A     A    1    2
    ## 3088 009234 CASEGU  77.9  47.5   33   33   32     A     A     A    1    1
    ## 3089 009285 FARAOC  68.2  64.6   33   36   37     A     A     A    1    1
    ## 3090 009286 EUGEPR  68.8  62.9   33   36   35     A     A     A    1    1
    ## 3091 009325 PROTTE  64.5  82.8   33   34   36     A     A     A    1    1
    ## 3092 009381 TAB1RO  67.8 108.6   33   37   37     A     A     A    1    1
    ## 3093 009395 PROTTE  79.6 113.5   33   35   37     A     A     A    1    1
    ## 3094 009397 ALIBED  78.9 109.1   33   32   31     A     A     A    1    1
    ## 3095 009903 POSOLA  73.9 231.7   33   33   33     A     A     A    1    1
    ## 3096 010041 FARAOC  73.9 263.6   33   33   33     A     A     A    1    1
    ## 3097 010097 BACTBA  63.4 294.7   33   33   33     A     A     A    1    1
    ## 3098 010256 CAL2CA 137.5  36.3   33   38   38     A     A     A    1    1
    ## 3099 010273 FICUBU 122.0  44.2   33   35   37     A     A     A    1    1
    ## 3100 010938 PIPERE 171.5  17.3   33   34   11     A     A     A    1    1
    ## 3101 01094A BACTMA   0.1 228.8   33   33    0     A     A     B    1    1
    ## 3102 010977 GUARGL 165.0  34.2   33   36   32     A     A     A    1    1
    ## 3103 010996 PIPERE 174.4  33.8   33   33   33     A     A     A    1    1
    ## 3104 011123 HIRTRA 171.6  70.7   33   33   36     A     A     A    1    1
    ## 3105 011159 COPAAR 165.2  90.9   33   37   36     A     A     A    1    1
    ## 3106 011228 IXORFL 193.7  58.2   33   33   35     A     A     A    1    1
    ## 3107 011257 AST2GR 199.2  44.0   33   39   38     A     A     A    1    1
    ## 3108 011382 ANNOHA 196.1  82.5   33   34   35     A     A     A    1    1
    ## 3109 011409 ALIBED 125.1  64.7   33   32   36     A     A     A    1    1
    ## 3110 000095 CUPASY  17.0  14.5   32   34   34     A     A     A    1    1
    ## 3111 000234 AST2GR   6.5  49.4   32   14   14     A     A     A    1    2
    ## 3112 000285 COCCPA  19.0  53.9   32   30   28     A     A     A    1    1
    ## 3113 000322 EUGEPR   5.4  79.6   32   32   32     A     A     A    1    1
    ## 3114 000348 TRIPCU   8.0  63.4   32   44   46     A     A     A    1    1
    ## 3115 000415 PROTTE   1.0  82.6   32   47   51     A     A     A    1    1
    ## 3116 000551 HEISCO  18.3  94.7   32   45   49     A     A     A    1    1
    ## 3117 000556 COU2CU  15.2  88.1   32   35   36     A     A     A    1    1
    ## 3118 000561 EUGEPR  19.1  87.5   32   42   42     A     A     A    1    1
    ## 3119 000714 SOROAF   6.1 138.9   32   34   34     A     A     A    1    1
    ## 3120 000765 OURALU  17.5 129.4   32   33   34     A     A     A    1    1
    ## 3121 000816 INGAFA  13.0 156.2   32   -1   -1     A     D     D    1    0
    ## 3122 001148 BROSAL  11.5 236.1   32   32   32     A     A     A    1    1
    ## 3123 001184 PIPERE   9.6 254.5   32   33   27     A     A     A    1    1
    ## 3124 001305 PROTTE  16.1 267.5   32   32   33     A     A     A    1    1
    ## 3125 001426 ALIBED 114.4   5.2   32   32   32     A     A     A    1    1
    ## 3126 001470 MYRCGA 116.2   5.9   32   33   39     A     A     A    1    1
    ## 3127 001592 CASEGU 114.5  56.3   32   -1   -1     A     D     D    1    0
    ## 3128 001623 EUGEPR 117.1  45.1   32   33   33     A     A     A    1    1
    ## 3129 001647 CUPASY 103.4  72.5   32   34   35     A     A     A    1    1
    ## 3130 002034 SENNDA 151.9  10.3   32   33   33     A     A     A    1    1
    ## 3131 002038 BROSAL 153.1  15.7   32   32   34     A     A     A    1    1
    ## 3132 002215 COPAAR 159.1  53.7   32   32   32     A     A     A    1    1
    ## 3133 002230 TROPRA 141.2  63.7   32   38   39     A     A     A    1    1
    ## 3134 002378 BACTMA 150.7  86.6   32   26    0     A     A     B    1    1
    ## 3135 002379 BACTMA 148.1  88.0   32   32   31     A     A     A    1    1
    ## 3136 003009 ALIBED  23.3   5.6   32   33   33     A     A     A    1    1
    ## 3137 003047 COU2CU  27.3  17.7   32   36   38     A     A     A    1    1
    ## 3138 003059 HEISCO  30.4  12.5   32   49   55     A     A     A    1    1
    ## 3139 003065 CASEGU  32.4  16.8   32   36   37     A     A     A    1    1
    ## 3140 003151 COLUHE  32.5  21.2   32   36   38     A     A     A    1    1
    ## 3141 003230 COU2CU  27.6  55.3   32   35   36     A     A     A    1    1
    ## 3142 003362 COU2CU  39.5  66.4   32   55   58     A     A     A    1    1
    ## 3143 003367 OURALU  21.1  84.1   32   32   34     A     A     A    1    1
    ## 3144 003368 FARAOC  22.7  83.4   32   33   -1     A     A     D    1    1
    ## 3145 003410 BROSAL  33.6  80.7   32   34   33     A     A     A    1    1
    ## 3146 003436 COU2CU  35.1  92.3   32   35   38     A     A     A    1    1
    ## 3147 003450 SWARS1  21.6 105.9   32   34   34     A     A     A    1    1
    ## 3148 003533 SOROAF  39.2 103.1   32   35   35     A     A     A    1    1
    ## 3149 003587 COU2CU  31.1 125.9   32   36   39     A     A     A    1    1
    ## 3150 003783 COU2CU  34.5 299.7   32    0   10     A     B     A    1    2
    ## 3151 003810 SOROAF  22.2 189.8   32   34   35     A     A     A    1    1
    ## 3152 003831 COU2CU  28.9 195.5   32   35   39     A     A     A    1    1
    ## 3153 003856 EUGEPR  29.2 182.5   32   32   32     A     A     A    1    1
    ## 3154 003955 SOROAF  23.8 205.1   32   33    0     A     A     B    1    1
    ## 3155 004130 FARAOC  23.6 249.6   32   33   35     A     A     A    1    1
    ## 3156 004145 MARGNO  28.7 250.8   32   32   33     A     A     A    1    1
    ## 3157 004248 PROTTE  33.8 273.3   32   36   36     A     A     A    1    1
    ## 3158 004263 PIPERE  37.3 260.1   32   30   32     A     A     A    1    1
    ## 3159 004310 SOROAF  25.1 282.0   32   32   30     A     A     A    1    1
    ## 3160 004318 FARAOC  30.1 286.7   32   -1   -1     A     D     D    1    0
    ## 3161 004337 SOROAF  36.1 294.7   32   34   35     A     A     A    1    1
    ## 3162 006002 LACIAG  40.3   2.5   32   39   41     A     A     A    1    1
    ## 3163 006067 EUGEPR  52.5  10.8   32   31   32     A     A     A    1    1
    ## 3164 006233 EUGEPR  50.4  54.0   32   32   31     A     A     A    1    1
    ## 3165 006386 PROTTE  46.3  80.7   32   36   39     A     A     A    1    2
    ## 3166 006404 EUGEPR  58.8  90.3   32   36   39     A     A     A    1    2
    ## 3167 006418 HEISCO  42.6 109.3   32   33   35     A     A     A    1    1
    ## 3168 006532 FARAOC  54.5 124.6   32   32   32     A     A     A    1    1
    ## 3169 006623 HEISCO  54.7 143.7   32   34   37     A     A     A    1    1
    ## 3170 006651 PIPERE  44.5 160.1   32   36   40     A     A     A    1    1
    ## 3171 006763 TRI2PL  44.8 188.3   32   39   43     A     A     A    1    1
    ## 3172 006875 COU2CU  40.1 214.9   32   32   33     A     A     A    1    1
    ## 3173 006981 SOROAF  41.0 229.8   32   33    0     A     A     B    1    1
    ## 3174 007158 FARAOC  51.6 270.0   32   36   40     A     A     A    1    1
    ## 3175 007335 BROSAL  86.9  26.2   32   32   34     A     A     A    1    1
    ## 3176 007398 AST2GR  81.1  52.5   32   32   32     A     A     A    1    1
    ## 3177 007405 TRIPCU  83.5  57.4   32   37   41     A     A     A    1    1
    ## 3178 007439 MACLTI  92.4  50.7   32   32   33     A     A     A    1    1
    ## 3179 007467 TRIPCU  95.8  44.1   32   32   34     A     A     A    1    1
    ## 3180 007648 SOROAF  80.2 126.5   32   32   31     A     A     A    1    1
    ## 3181 007878 ALBIAD  91.4 176.2   32   34   36     A     A     A    1    1
    ## 3182 008060 ALIBED  80.1 215.3   32   31   33     A     A     A    1    1
    ## 3183 008094 HIRTRA  90.3 206.6   32   32   33     A     A     A    1    1
    ## 3184 008170 EUGEPR  92.4 229.6   32   32   33     A     A     A    1    1
    ## 3185 008193 LACIAG  96.4 238.3   32   17   18     A     A     A    1    1
    ## 3186 008222 PROTTE  99.6 222.6   32   36   36     A     A     A    1    1
    ## 3187 008247 LACIAG  88.5 255.1   32   36   37     A     A     A    1    1
    ## 3188 008314 POSOLA  97.6 255.5   32   35   40     A     A     A    1    1
    ## 3189 008321 PROTTE  95.1 251.8   32   34   35     A     A     A    1    1
    ## 3190 008370 SOROAF  89.9 272.8   32   34   37     A     A     A    1    1
    ## 3191 008395 PROTTE  93.4 265.5   32   32   32     A     A     A    1    1
    ## 3192 008444 GUARGL  83.8 296.1   32   33   33     A     A     A    1    1
    ## 3193 008446 TAB1RO  81.9 295.4   32   33   33     A     A     A    1    1
    ## 3194 008530 POSOLA 193.0   4.9   32   33   33     A     A     A    1    1
    ## 3195 008560 PIPERE 183.8  26.2   32   11   12     A     A     A    1    1
    ## 3196 009007 PROTTE  63.7   8.0   32   36   37     A     A     A    1    1
    ## 3197 009230 AST2GR  75.4  46.6   32   34   31     A     A     A    1    1
    ## 3198 009349 PSE1SE  70.7  94.1   32   -1   -1     A     D     D    1    0
    ## 3199 009363 FARAOC  78.3  87.5   32   35   36     A     A     A    1    1
    ## 3200 009388 GUARGL  72.3 115.8   32   37   38     A     A     A    1    1
    ## 3201 009390 ORMOMA  71.4 119.3   32   32   34     A     A     A    1    1
    ## 3202 009630 ALIBED  61.4 184.0   32   33   33     A     A     A    1    1
    ## 3203 009683 PROTTE  71.8 187.8   32   40   41     A     A     A    1    1
    ## 3204 009756 POSOLA  69.8 210.4   32   33   35     A     A     A    1    1
    ## 3205 009819 BROSAL  79.0 216.5   32   35   35     A     A     A    1    1
    ## 3206 009967 POSOLA  71.6 247.1   32   34   34     A     A     A    1    1
    ## 3207 010085 TAB1RO  63.8 286.1   32   32   33     A     A     A    1    1
    ## 3208 010151 PROTTE 125.4  15.1   32   34   37     A     A     A    1    1
    ## 3209 010194 CLAVME 138.9   7.6   32   33   33     A     A     A    1    1
    ## 3210 010212 ALIBED 123.3  37.1   32   32   32     A     A     A    1    1
    ## 3211 010338 BROSAL 138.3  56.2   32   36   40     A     A     A    1    1
    ## 3212 010374 NEEADE 123.0  65.8   32   26   31     A     A     A    1    1
    ## 3213 010403 APHEDE 129.8  60.2   32   32   17     A     A     A    1    1
    ## 3214 010492 TRIPCU 133.6  90.1   32   32   32     A     A     A    1    1
    ## 3215 010904 PIPERE 169.7  10.7   32   32   21     A     A     A    1    1
    ## 3216 010958 ALIBED 160.7  23.4   32   32   34     A     A     A    1    1
    ## 3217 010960 PROTTE 164.5  29.8   32   33   33     A     A     A    1    1
    ## 3218 011055 ALIBED 173.3  55.2   32   -1   -1     A     D     D    1    0
    ## 3219 011118 IXORFL 171.4  67.0   32   38   41     A     A     A    1    1
    ## 3220 011127 FARAOC 174.5  74.9   32   36   36     A     A     A    1    1
    ## 3221 011217 AST2GR 190.7  44.5   32   33   33     A     A     A    1    1
    ## 3222 011258 PICRLA 199.7  44.0   32   38   38     A     A     A    1    1
    ## 3223 000043 GUARGL  12.2   4.6   31   36   36     A     A     A    1    1
    ## 3224 000057 BACTMA  11.6  10.8   31   40   40     A     A     A    1    1
    ## 3225 000163 IXORFL  12.2  25.1   31   29   30     A     A     A    1    1
    ## 3226 000189 EUGEPR  18.2  39.1   31   33   37     A     A     A    1    1
    ## 3227 000420 ANNOHA   4.8  84.9   31   43   43     A     A     A    1    1
    ## 3228 000450 EUGEPR   3.8  97.6   31   29   30     A     A     A    1    1
    ## 3229 000522 COU2CU  14.7  93.2   31   -1   -1     A     D     D    1    0
    ## 3230 000545 COU2CU  16.4  91.3   31   35   36     A     A     A    1    2
    ## 3231 000611 CHR2CA   8.1 111.0   31   30   30     A     A     A    1    1
    ## 3232 000677 PIPERE  17.7 106.9   31   34   34     A     A     A    1    2
    ## 3233 000740 EUGEPR  14.3 129.0   31   15   10     A     A     A    1    1
    ## 3234 000794 CLAVME   9.1 149.3   31   31   31     A     A     A    1    1
    ## 3235 000843 SOROAF   0.8 174.0   31   36   33     A     A     A    1    2
    ## 3236 000909 CLAVME  16.5 172.2   31   52   40     A     A     A    1    2
    ## 3237 000990 FARAOC  19.5 180.4   31   35   37     A     A     A    1    1
    ## 3238 001002 FARAOC   1.8 211.6   31   33   35     A     A     A    1    1
    ## 3239 001066 SOROAF  18.7 212.8   31   -9   32     A     *     A    1    0
    ## 3240 001172 SOROAF   4.8 256.3   31   32   30     A     A     A    1    1
    ## 3241 001186 AST2GR   6.7 249.2   31    0   -1     A     B     D    1    2
    ## 3242 001209 POSOLA  13.4 258.0   31   31   31     A     A     A    1    1
    ## 3243 001274 COU2CU   6.3 263.4   31   31   33     A     A     A    1    1
    ## 3244 001295 COU2CU  16.5 275.6   31   31   35     A     A     A    1    2
    ## 3245 001672 TRIPCU 112.8  63.0   31   38   38     A     A     A    1    1
    ## 3246 001727 EUGEPR 101.8  87.5   31   31   33     A     A     A    1    1
    ## 3247 002065 STYLST 141.4  26.1   31   32   32     A     A     A    1    1
    ## 3248 002117 ALIBED 150.7  34.4   31   29   24     A     A     A    1    1
    ## 3249 002122 ALIBED 155.1  35.2   31   30   30     A     A     A    1    1
    ## 3250 002131 RINOLI 155.5  31.1   31   31   24     A     A     A    1    1
    ## 3251 002173 CUPASY 145.6  58.8   31   40   49     A     A     A    1    1
    ## 3252 002314 BACTMA 143.9  92.4   31   23   25     A     A     A    1    1
    ## 3253 003087 POSOLA  37.7   1.9   31   39   39     A     A     A    1    1
    ## 3254 003167 FARAOC  34.7  27.2   31   42   44     A     A     A    1    1
    ## 3255 003202 EUGEPR  35.3  23.3   31   35   37     A     A     A    1    1
    ## 3256 003265 FARAOC  30.1  54.3   31   33   34     A     A     A    1    1
    ## 3257 003334 EUGEPR  29.8  62.1   31   32   34     A     A     A    1    1
    ## 3258 003451 COU2CU  20.7 106.0   31   32   32     A     A     A    1    1
    ## 3259 003706 COCCPA  36.9 147.0   31   34   34     A     A     A    1    1
    ## 3260 003800 PROTTE  20.9 181.7   31   35   36     A     A     A    1    1
    ## 3261 003880 COU2CU  34.7 190.6   31   31   30     A     A     A    1    1
    ## 3262 003938 COU2CU  36.0 181.4   31   33   39     A     A     A    1    1
    ## 3263 003973 SOROAF  21.5 219.8   31   31   30     A     A     A    1    1
    ## 3264 004004 FARAOC  32.5 203.8   31   37   38     A     A     A    1    1
    ## 3265 004193 BROSAL  37.5 254.6   31   31   31     A     A     A    1    1
    ## 3266 004210 BROSAL  24.5 266.5   31   31   30     A     A     A    1    1
    ## 3267 004215 BACTBA  23.5 279.7   31   32   32     A     A     A    1    1
    ## 3268 004222 MACRGL  29.3 272.1   31   39   39     A     A     A    1    1
    ## 3269 004286 PROTTE  25.9 295.7   31   36   39     A     A     A    1    1
    ## 3270 004325 COU2CU  30.2 295.3   31   33   34     A     A     A    1    1
    ## 3271 006011 CUPASY  40.1  10.5   31   33   37     A     A     A    1    1
    ## 3272 006074 GUARGL  52.4  17.5   31   31   12     A     A     A    1    1
    ## 3273 006200 GUARGL  40.1  58.5   31   32   32     A     A     A    1    1
    ## 3274 006263 GUARGL  55.3  49.3   31   31   33     A     A     A    1    1
    ## 3275 006450 FARAOC  54.5 108.1   31   37   40     A     A     A    1    1
    ## 3276 006464 CLAVME  57.0 105.2   31   31   32     A     A     A    1    1
    ## 3277 006480 PIPERE  40.8 125.6   31   38   38     A     A     A    1    1
    ## 3278 006506 HEISCO  45.0 137.1   31   36   38     A     A     A    1    1
    ## 3279 006507 TRIPCU  46.5 139.5   31   39   40     A     A     A    1    1
    ## 3280 006576 AMAICO  57.4 130.6   31   32   32     A     A     A    1    1
    ## 3281 006878 BROSAL  42.2 215.5   31   34   36     A     A     A    1    1
    ## 3282 006886 CUPASY  45.2 215.3   31   34   33     A     A     A    1    1
    ## 3283 007003 HEISCO  49.2 220.1   31   31   44     A     A     A    1    1
    ## 3284 007130 COU2CU  48.5 271.4   31   30   30     A     A     A    1    2
    ## 3285 007195 HIRTRA  40.6 288.3   31   32   31     A     A     A    1    1
    ## 3286 007238 HIRTRA  51.0 294.9   31    0    0     A     B     B    1    2
    ## 3287 007364 EUGEPR  92.5  33.9   31   31   33     A     A     A    1    1
    ## 3288 007486 COU2CU  84.2  79.3   31   32   -1     A     A     D    1    1
    ## 3289 007603 PROTTE  81.1 116.8   31   37   40     A     A     A    1    1
    ## 3290 007623 ALIBED  90.5 100.2   31   31   33     A     A     A    1    1
    ## 3291 007663 AST2GR  80.0 136.1   31   31   31     A     A     A    1    1
    ## 3292 007695 SOROAF  94.2 128.7   31   34   32     A     A     A    1    1
    ## 3293 007852 ANNOHA  85.4 167.1   31   31   32     A     A     A    1    1
    ## 3294 007975 BACTMA  94.1 182.5   31   30   30     A     A     A    1    1
    ## 3295 008009 MYRCGA  95.9 191.3   31   32   34     A     A     A    1    1
    ## 3296 008116 PROTTE  99.3 208.1   31   29   30     A     A     A    1    1
    ## 3297 008131 ALIBED  82.3 223.1   31   32   32     A     A     A    1    1
    ## 3298 008226 POSOLA  81.8 244.9   31   31   31     A     A     A    1    1
    ## 3299 008340 LACIAG  97.7 240.3   31   33   28     A     A     A    1    1
    ## 3300 008406 BROSAL  90.8 275.1   31   31   31     A     A     A    1    1
    ## 3301 008470 SIPAPA  93.0 297.5   31   32   33     A     A     A    1    1
    ## 3302 008511 PIPERE 187.3  16.5   31   33   35     A     A     A    1    1
    ## 3303 008521 SWARS1 187.4   9.0   31   32   32     A     A     A    1    1
    ## 3304 008529 SWARS1 190.1   4.0   31   33   33     A     A     A    1    1
    ## 3305 009004 AST2GR  61.3   9.5   31   25   27     A     A     A    1    2
    ## 3306 009096 AST2GR  61.0  34.8   31   31   32     A     A     A    1    1
    ## 3307 009145 OURALU  78.5  25.7   31   34   35     A     A     A    1    1
    ## 3308 009323 ALIBED  61.4  83.3   31   31   30     A     A     A    1    1
    ## 3309 009461 AST2GR  78.3 134.2   31   31   32     A     A     A    1    1
    ## 3310 009478 PROTTE  79.3 123.6   31   35   34     A     A     A    1    1
    ## 3311 009598 PROTTE  73.9 174.4   31   -1   -1     A     D     D    1    0
    ## 3312 009619 ALIBED  78.2 170.2   31   33   32     A     A     A    1    1
    ## 3313 009882 COU2CU  66.8 225.6   31   -9   33     A     *     A    1    0
    ## 3314 009899 SWARS1  72.1 234.7   31   36   36     A     A     A    1    1
    ## 3315 010081 ANNOHA  65.2 282.2   31   33   33     A     A     A    1    1
    ## 3316 010147 EUGEPR 120.7  18.2   31   32   31     A     A     A    1    2
    ## 3317 010334 AST2GR 131.6  60.0   31   31   33     A     A     A    1    2
    ## 3318 010435 ANNOHA 135.9  67.2   31   32   38     A     A     A    1    1
    ## 3319 010439 ZUELGU 135.8  60.6   31   31   32     A     A     A    1    1
    ## 3320 010972 TRI2PL 166.3  36.2   31   41   45     A     A     A    1    1
    ## 3321 011032 ALIBED 167.8  57.8   31   32   -1     A     A     D    1    1
    ## 3322 011053 PROTTE 172.1  50.3   31   -1   -1     A     D     D    1    0
    ## 3323 011103 HIRTRA 165.0  74.9   31   35   38     A     A     A    1    1
    ## 3324 011204 XYL1FR 188.6  49.4   31   36   18     A     A     A    1    1
    ## 3325 011356 ALIBED 193.7  82.7   31   32   32     A     A     A    1    1
    ## 3326 000070 FARAOC  12.7  15.3   30   33   34     A     A     A    1    1
    ## 3327 000235 ANNOHA   8.4  47.9   30   34   33     A     A     A    1    1
    ## 3328 000250 RANDFO  11.8  54.4   30   14   15     A     A     A    1    1
    ## 3329 000441 EUGEPR   0.8  91.4   30   32   32     A     A     A    1    1
    ## 3330 000498 COU2CU  12.9  86.0   30   28   30     A     A     A    1    1
    ## 3331 000594 SOROAF   0.6 118.8   30   32   31     A     A     A    1    1
    ## 3332 000625 EUGEPR   9.4 108.9   30   31   31     A     A     A    1    1
    ## 3333 000679 COU2CU  15.2 100.3   30   31   31     A     A     A    1    1
    ## 3334 000758 SWARS1  15.4 133.1   30   29   29     A     A     A    1    1
    ## 3335 000808 FARAOC  14.5 149.1   30   32   32     A     A     A    1    1
    ## 3336 000845 CLAVME   3.7 175.3   30   29   29     A     A     A    1    1
    ## 3337 000903 BROSAL  19.7 177.8   30   32   32     A     A     A    1    1
    ## 3338 001024 SOROAF   8.4 200.9   30   35   38     A     A     A    1    1
    ## 3339 001056 SOROAF  19.3 215.6   30   38   38     A     A     A    1    1
    ## 3340 001072 SOROAF  16.3 208.4   30   31   32     A     A     A    1    1
    ## 3341 001257 BACTMA   7.2 229.7   30   30    0     A     A     B    1    1
    ## 3342 001275 COU2CU   9.8 264.9   30   31   32     A     A     A    1    1
    ## 3343 001399 PROTTE 104.5  12.2   30   31   30     A     A     A    1    1
    ## 3344 001435 OCOTRU 113.2  10.2   30   30   30     A     A     A    1    1
    ## 3345 001438 EUGEPR 110.1  11.2   30   29    0     A     A     B    1    1
    ## 3346 001619 NEEADE 117.6  53.7   30   33   36     A     A     A    1    1
    ## 3347 001629 CAL2CA 117.3  48.2   30   31   35     A     A     A    1    1
    ## 3348 001632 BROSAL 116.1  40.0   30   37   36     A     A     A    1    1
    ## 3349 001674 BURSSI 114.8  64.8   30   -1   -1     A     D     D    1    0
    ## 3350 001689 BROSAL 113.1  75.9   30   33   33     A     A     A    1    1
    ## 3351 002080 TROPRA 149.6  36.4   30   32   39     A     A     A    1    1
    ## 3352 002127 RINOLI 159.1  37.7   30   31   31     A     A     A    1    1
    ## 3353 002254 CASECO 142.2  78.4   30   33   33     A     A     A    1    1
    ## 3354 002257 ALIBED 145.4  77.5   30   33   34     A     A     A    1    1
    ## 3355 002316 SWARS1 140.6  88.7   30   35   37     A     A     A    1    1
    ## 3356 002319 BACTMA 144.2  91.5   30   25   28     A     A     A    1    1
    ## 3357 002323 HEISCO 142.2  93.1   30   36   37     A     A     A    1    1
    ## 3358 002334 SOROAF 149.1  98.3   30   35   36     A     A     A    1    1
    ## 3359 002371 BACTMA 154.4  92.9   30   32   30     A     A     A    1    1
    ## 3360 002375 BACTMA 154.0  88.2   30   27   28     A     A     A    1    1
    ## 3361 003023 EUGEPR  22.3  12.6   30   36   36     A     A     A    1    1
    ## 3362 003035 PIT1RU  21.6  17.5   30   34   36     A     A     A    1    1
    ## 3363 003042 COU2CU  28.2  19.5   30   34   37     A     A     A    1    1
    ## 3364 003117 TRI2PL  28.7  36.8   30   38   38     A     A     A    1    1
    ## 3365 003147 OURALU  31.1  20.6   30   32   32     A     A     A    1    1
    ## 3366 003205 EUGEPR  39.1  22.0   30   33   33     A     A     A    1    1
    ## 3367 003219 COU2CU  20.8  56.7   30   39   43     A     A     A    1    1
    ## 3368 003241 EUGEPR  28.1  51.6   30   36   41     A     A     A    1    1
    ## 3369 003260 GUARGL  31.7  47.5   30   38   38     A     A     A    1    1
    ## 3370 003266 CLAVME  30.8  54.8   30   34   32     A     A     A    1    1
    ## 3371 003267 CUPASY  33.0  53.5   30   37   35     A     A     A    1    1
    ## 3372 003278 BROSAL  38.0  51.8   30   39   29     A     A     A    1    1
    ## 3373 003398 COU2CU  27.4  88.1   30   33   35     A     A     A    1    1
    ## 3374 003399 COU2CU  27.8  88.6   30   35   38     A     A     A    1    1
    ## 3375 003427 HEISCO  32.5  96.7   30   36   40     A     A     A    1    1
    ## 3376 003475 AMAICO  24.8 108.6   30   31   30     A     A     A    1    1
    ## 3377 003503 BROSAL  32.8 111.1   30   -1   -1     A     D     D    1    0
    ## 3378 003529 SOROAF  36.2 100.5   30   39   41     A     A     A    1    1
    ## 3379 003536 CHR2CA  22.6 120.7   30   30   30     A     A     A    1    1
    ## 3380 003581 OURALU  30.6 121.9   30   33   32     A     A     A    1    1
    ## 3381 003640 SWARS1  21.3 152.4   30   31   31     A     A     A    1    1
    ## 3382 003684 IXORFL  33.5 146.7   30   37   37     A     A     A    1    1
    ## 3383 003718 ALIBED  24.0 164.2   30   30   -1     A     A     D    1    1
    ## 3384 003908 PROTTE  36.0 196.3   30   43   46     A     A     A    1    1
    ## 3385 003935 COU2CU  37.5 180.2   30   34   37     A     A     A    1    1
    ## 3386 003947 COU2CU  37.5 182.6   30   33   35     A     A     A    1    1
    ## 3387 004192 BACTMA  35.7 253.7   30   33   32     A     A     A    1    1
    ## 3388 004247 BACTMA  32.8 274.0   30   32    0     A     A     B    1    1
    ## 3389 004259 BACTMA  37.4 269.8   30   -1   -1     A     D     D    1    0
    ## 3390 004260 BACTMA  38.8 267.8   30   31   30     A     A     A    1    1
    ## 3391 004292 FARAOC  28.0 290.6   30   35   38     A     A     A    1    1
    ## 3392 004334 HIRTRA  39.6 290.3   30   41   42     A     A     A    1    1
    ## 3393 004354 TROPRA  33.4  72.8   30   30   30     A     A     A    1    1
    ## 3394 006079 PROTTE  56.4  13.2   30   32   32     A     A     A    1    1
    ## 3395 006080 CLAVME  56.7  14.2   30   34   35     A     A     A    1    1
    ## 3396 006177 COPAAR  59.8  28.7   30   32   34     A     A     A    1    1
    ## 3397 006431 HEISCO  46.2 110.1   30   33   35     A     A     A    1    1
    ## 3398 006592 CHR2CA  59.2 130.0   30   30   31     A     A     A    1    1
    ## 3399 006602 ANNOHA  40.1 142.4   30   31   34     A     A     A    1    1
    ## 3400 006868 BROSAL  42.2 206.9   30   -1   -1     A     D     D    1    0
    ## 3401 006895 COU2CU  46.8 218.1   30   33   30     A     A     A    1    1
    ## 3402 006923 PROTTE  49.6 201.0   30   48   41     A     A     A    1    1
    ## 3403 006947 PROTTE  54.3 219.3   30   38   39     A     A     A    1    1
    ## 3404 007220 FARAOC  45.6 293.5   30   31   31     A     A     A    1    1
    ## 3405 007234 HIRTRA  52.4 290.3   30    0   -1     A     B     D    1    2
    ## 3406 007317 BROSAL  82.5  31.2   30   32   33     A     A     A    1    1
    ## 3407 007325 BROSAL  83.4  38.2   30   29   29     A     A     A    1    1
    ## 3408 007332 TRI2PL  85.0  34.8   30   37   42     A     A     A    1    1
    ## 3409 007412 AST2GR  85.8  50.1   30   -1   -1     A     D     D    1    0
    ## 3410 007520 LACIAG  99.2  71.4   30   30   -1     A     A     D    1    1
    ## 3411 007590 FARAOC  95.6  87.1   30   33   35     A     A     A    1    1
    ## 3412 007649 PROTTE  81.0 126.7   30   30   29     A     A     A    1    1
    ## 3413 007685 STYLST  86.4 126.3   30   30   30     A     A     A    1    1
    ## 3414 007769 EUGEPR  85.1 142.4   30   30   30     A     A     A    1    1
    ## 3415 007846 SOROAF  86.8 170.6   30   31   31     A     A     A    1    1
    ## 3416 007909 PROTTE  83.0 184.5   30   46   52     A     A     A    1    1
    ## 3417 008195 LACIAG  97.2 239.6   30   30   30     A     A     A    1    1
    ## 3418 008288 PIPERE  93.7 243.0   30   30   30     A     A     A    1    1
    ## 3419 008343 NEEADE  97.2 243.0   30   30   31     A     A     A    1    1
    ## 3420 008481 ALIBED 180.2   4.5   30   31   -1     A     A     D    1    1
    ## 3421 008531 PROTTE 190.7   9.5   30   -1   -1     A     D     D    1    0
    ## 3422 008616 CAL2CA 197.4  39.4   30   -9   15     A     *     A    1    0
    ## 3423 009097 TRIPCU  64.8  31.5   30   30   15     A     A     A    1    1
    ## 3424 009228 ANNOHA  75.1  45.7   30   33   38     A     A     A    1    1
    ## 3425 009239 COU2CU  63.2  60.5   30   31   31     A     A     A    1    1
    ## 3426 009277 EUGEPR  65.1  67.6   30   30   26     A     A     A    1    1
    ## 3427 009338 ANNOHA  73.6  84.2   30   32   32     A     A     A    1    1
    ## 3428 009552 GUARGL  63.8 169.7   30   37   38     A     A     A    1    1
    ## 3429 009611 BROSAL  79.4 176.2   30   34   38     A     A     A    1    1
    ## 3430 009907 ALIBED  73.1 235.8   30   31   31     A     A     A    1    1
    ## 3431 010045 BROSAL  72.3 267.0   30   31   32     A     A     A    1    1
    ## 3432 010117 FARAOC  74.9 280.1   30   35   37     A     A     A    1    1
    ## 3433 010219 STYLST 129.6  32.7   30   32   30     A     A     A    1    1
    ## 3434 010251 CHR2CA 134.8  31.3   30   31   31     A     A     A    1    1
    ## 3435 010290 OCOTRU 127.3  50.4   30   39   40     A     A     A    1    1
    ## 3436 010311 ALIBED 131.7  44.4   30   32   32     A     A     A    1    1
    ## 3437 010314 OURALU 133.7  46.3   30   31   31     A     A     A    1    1
    ## 3438 010337 TRIPCU 134.4  57.2   30   32   32     A     A     A    1    1
    ## 3439 010342 PROTTE 137.4  59.5   30   31   37     A     A     A    1    1
    ## 3440 010372 COU2CU 123.5  63.8   30   31   32     A     A     A    1    1
    ## 3441 010441 TRIPCU 139.4  64.5   30   34   44     A     A     A    1    1
    ## 3442 010915 PIPERE 168.7   7.4   30   32    0     A     A     B    1    1
    ## 3443 010942 PIPERE 174.7  18.5   30   31    0     A     A     B    1    1
    ## 3444 011037 HIRTRA 169.7  54.8   30   36   37     A     A     A    1    1
    ## 3445 011095 SWARS1 168.7  71.1   30   34   35     A     A     A    1    1
    ## 3446 011322 ANNOHA 199.0  73.0   30   36   36     A     A     A    1    1
    ## 3447 011410 LACIAG 179.9   3.3   30   41   41     A     A     A    1    2
    ## 3448 000009 RINOLI   1.5  11.0   29   30   29     A     A     A    1    1
    ## 3449 000146 AST2GR   5.1  32.6   29   19   20     A     A     A    1    1
    ## 3450 000228 THEVAH   8.6  56.4   29   43   45     A     A     A    1    1
    ## 3451 000232 EUGEPR   6.3  54.2   29   27   33     A     A     A    1    1
    ## 3452 000292 CUPARU  19.1  41.5   29   47   47     A     A     A    1    1
    ## 3453 000332 FARAOC   8.1  69.6   29   38   40     A     A     A    1    1
    ## 3454 000357 SWARS1  12.2  64.8   29    0    0     A     B     B    1    2
    ## 3455 000385 AST2GR  19.0  70.7   29   38   38     A     A     A    1    1
    ## 3456 000399 AST2GR  19.2  60.8   29   29   31     A     A     A    1    1
    ## 3457 000437 HEISCO   2.7  85.9   29   38   38     A     A     A    1    1
    ## 3458 000462 COU2CU   8.7  90.6   29   29   30     A     A     A    1    1
    ## 3459 000483 COU2CU   6.6  86.4   29   37   40     A     A     A    1    1
    ## 3460 000735 COU2CU   5.4 123.9   29   34   36     A     A     A    1    1
    ## 3461 000810 PICRLA  12.6 154.0   29   29   29     A     A     A    1    1
    ## 3462 000853 SOROAF   9.6 171.0   29   28   29     A     A     A    1    1
    ## 3463 000872 FARAOC   6.2 162.8   29   28   28     A     A     A    1    1
    ## 3464 000983 EUGEPR  19.4 186.2   29   27   28     A     A     A    1    1
    ## 3465 001019 SOROAF   7.2 214.4   29   29   29     A     A     A    1    1
    ## 3466 001047 SOROAF  14.1 208.5   29   34   35     A     A     A    1    1
    ## 3467 001071 SWARS1  16.5 207.9   29   27   29     A     A     A    1    1
    ## 3468 001103 COU2CU   4.2 233.3   29   29   30     A     A     A    1    1
    ## 3469 001124 PICRLA  10.8 224.9   29   27   28     A     A     A    1    1
    ## 3470 001266 COU2CU   8.5 268.8   29   39   42     A     A     A    1    1
    ## 3471 001290 COU2CU  10.3 276.8   29   34   37     A     A     A    1    1
    ## 3472 001343 PIPERE   6.4 285.8   29   11   17     A     A     A    1    1
    ## 3473 001395 POSOLA 100.6  10.9   29   27   28     A     A     A    1    1
    ## 3474 001409 NEEADE 107.1  16.3   29   30   30     A     A     A    1    1
    ## 3475 001576 TRI2PL 111.3  42.0   29   32   34     A     A     A    1    2
    ## 3476 001669 APHEDE 113.9  60.3   29   -1   -1     A     D     D    1    0
    ## 3477 001711 ALIBED 116.0  61.6   29   29   31     A     A     A    1    1
    ## 3478 001775 IXORFL 117.7  93.4   29   30   36     A     A     A    1    1
    ## 3479 002001 IXORFL 143.7   2.2   29   30   30     A     A     A    1    1
    ## 3480 002043 TRIPCU 156.0  14.8   29   36   38     A     A     A    1    1
    ## 3481 002130 CHR2CA 156.7  30.5   29   28   30     A     A     A    1    1
    ## 3482 002186 ANNOHA 149.7  47.5   29   28   28     A     A     A    1    1
    ## 3483 002372 BACTMA 151.5  92.9   29   31   29     A     A     A    1    1
    ## 3484 003107 EUGEPR  24.5  35.4   29   30   36     A     A     A    1    1
    ## 3485 003154 COU2CU  33.3  22.4   29   33   41     A     A     A    1    1
    ## 3486 003263 COPAAR  34.8  50.8   29   30   30     A     A     A    1    1
    ## 3487 003298 RINOLI  21.3  62.2   29   37   38     A     A     A    1    1
    ## 3488 003346 CUPASY  31.3  74.7   29   27   29     A     A     A    1    1
    ## 3489 003411 EUGEPR  30.9  80.9   29   17   16     A     A     A    1    1
    ## 3490 003418 EUGEPR  31.0  87.8   29   31   31     A     A     A    1    1
    ## 3491 003453 OCOTRU  24.1 109.7   29    0    0     A     B     B    1    2
    ## 3492 003745 SOROAF  25.9 167.8   29   31   32     A     A     A    1    1
    ## 3493 003788 CLAVME  37.7 166.1   29   30   29     A     A     A    1    1
    ## 3494 003790 FARAOC  36.1 167.6   29   32   38     A     A     A    1    1
    ## 3495 003844 COU2CU  26.5 189.0   29   30   28     A     A     A    1    1
    ## 3496 003867 COU2CU  32.9 182.5   29   31   33     A     A     A    1    1
    ## 3497 003902 COU2CU  31.1 198.5   29   31   36     A     A     A    1    1
    ## 3498 003914 PROTTE  39.8 191.2   29   38   45     A     A     A    1    1
    ## 3499 003924 COU2CU  37.6 194.3   29   30   28     A     A     A    1    1
    ## 3500 004047 AST2GR  35.2 211.2   29   31   31     A     A     A    1    1
    ## 3501 004093 BACTMA  30.3 222.6   29   30   31     A     A     A    1    1
    ## 3502 004116 BROSAL  35.4 222.0   29   29   30     A     A     A    1    1
    ## 3503 004306 FARAOC  25.1 289.3   29   20   21     A     A     A    1    1
    ## 3504 004348 ALIBED  37.2 282.4   29    0    0     A     B     B    1    2
    ## 3505 006029 ANNOHA  45.5  11.4   29   14   14     A     A     A    1    1
    ## 3506 006113 SOROAF  41.0  32.3   29   35   37     A     A     A    1    1
    ## 3507 006166 EUGEPR  50.5  36.5   29   29   30     A     A     A    1    1
    ## 3508 006284 COU2CU  43.8  62.8   29   32   36     A     A     A    1    1
    ## 3509 006295 PIPERE  44.0  78.7   29   30   30     A     A     A    1    2
    ## 3510 006299 PROTTE  43.4  76.5   29   31   31     A     A     A    1    1
    ## 3511 006322 COU2CU  48.2  64.6   29   30   30     A     A     A    1    1
    ## 3512 006323 EUGEPR  48.7  64.7   29   28   30     A     A     A    1    1
    ## 3513 006328 SLOATE  51.0  68.1   29   27   28     A     A     A    1    1
    ## 3514 006437 HEISCO  49.1 111.9   29   32   35     A     A     A    1    1
    ## 3515 006439 ALIBED  47.2 106.1   29   30   30     A     A     A    1    1
    ## 3516 006442 ALIBED  45.5 109.2   29   28   29     A     A     A    1    1
    ## 3517 006467 EUGEPR  56.2 108.0   29   31   33     A     A     A    1    1
    ## 3518 006497 HEISCO  44.8 138.6   29   22   26     A     A     A    1    1
    ## 3519 006549 ALIBED  54.7 130.6   29   20   -1     A     A     D    1    1
    ## 3520 006774 INGAFA  42.1 194.2   29   -1   -1     A     D     D    1    0
    ## 3521 006782 COU2CU  47.4 197.7   29   35   38     A     A     A    1    1
    ## 3522 006861 COU2CU  55.6 188.9   29   33   32     A     A     A    1    1
    ## 3523 006938 PROTTE  51.3 214.3   29   38   39     A     A     A    1    1
    ## 3524 006963 EUGEPR  58.9 200.3   29   39   35     A     A     A    1    1
    ## 3525 007037 TAB1RO  54.6 238.2   29   31   31     A     A     A    1    1
    ## 3526 007099 BACTMA  42.4 260.8   29    0    0     A     B     B    1    2
    ## 3527 007138 EUGEPR  49.9 269.9   29   29   30     A     A     A    1    2
    ## 3528 007209 HIRTRA  43.8 296.9   29   -1   -1     A     D     D    1    0
    ## 3529 007272 EUGEPR  84.8  19.0   29   29   31     A     A     A    1    1
    ## 3530 007327 TRIPCU  84.8  39.3   29   36   37     A     A     A    1    1
    ## 3531 007342 TRIPCU  87.4  24.3   29   41   34     A     A     A    1    1
    ## 3532 007441 ACACME  91.2  51.2   29    0   -1     A     B     D    1    2
    ## 3533 007521 TRIPCU  97.6  71.3   29   45   55     A     A     A    1    1
    ## 3534 007570 CHR2CA  91.6  87.9   29   29   32     A     A     A    1    1
    ## 3535 007693 CLAVME  91.0 124.7   29   31   31     A     A     A    1    1
    ## 3536 007716 FARAOC  95.0 128.6   29   31   35     A     A     A    1    1
    ## 3537 007755 PROCCR  86.2 159.9   29   -1   -1     A     D     D    1    0
    ## 3538 007815 AST2GR  99.0 147.8   29   32   32     A     A     A    1    1
    ## 3539 007924 SOROAF  83.4 195.9   29   33   32     A     A     A    1    1
    ## 3540 008020 BROSAL  95.8 187.7   29   30   30     A     A     A    1    1
    ## 3541 008047 PIPERE  84.7 207.8   29   31   32     A     A     A    1    1
    ## 3542 008179 BROSAL  90.0 238.5   29   30   30     A     A     A    1    1
    ## 3543 008235 ALIBED  84.0 250.6   29   29   30     A     A     A    1    1
    ## 3544 008329 PROTTE  99.7 252.6   29   29   30     A     A     A    1    1
    ## 3545 008335 FARAOC  95.1 246.8   29   28   28     A     A     A    1    1
    ## 3546 008490 PROTTE 183.4   5.8   29   29   30     A     A     A    1    1
    ## 3547 008579 PICRLA 188.5  31.0   29   33   34     A     A     A    1    1
    ## 3548 008613 ANTITR 196.1  37.7   29    0   16     A     B     A    1    2
    ## 3549 009219 AST2GR  78.9  55.8   29   30   31     A     A     A    1    1
    ## 3550 009359 PROTTE  76.5  85.1   29   33   34     A     A     A    1    1
    ## 3551 009588 FARAOC  70.3 162.5   29   34   37     A     A     A    1    1
    ## 3552 009751 BROSAL  62.4 218.3   29   31   31     A     A     A    1    1
    ## 3553 009772 ALIBED  69.2 209.9   29   29   30     A     A     A    1    1
    ## 3554 009812 INGAFA  78.2 215.2   29   30   -1     A     A     D    1    1
    ## 3555 009890 SOROAF  66.1 222.4   29   34   35     A     A     A    1    1
    ## 3556 009945 BACTMA  63.7 249.0   29   -9    0     A     *     B    1    0
    ## 3557 009966 TAB1RO  71.0 248.8   29   26   28     A     A     A    1    1
    ## 3558 010090 PROTTE  63.1 288.9   29   31   32     A     A     A    1    1
    ## 3559 010246 COPAAR 134.6  28.4   29   30   32     A     A     A    1    1
    ## 3560 010340 BROSAL 135.7  57.9   29   32   39     A     A     A    1    1
    ## 3561 010502 ANNOHA 132.2  96.5   29   34   35     A     A     A    1    1
    ## 3562 010886 TRIPCU 163.6   8.4   29   30   31     A     A     A    1    1
    ## 3563 011033 COPAAR 168.7  50.8   29   29   29     A     A     A    1    1
    ## 3564 011073 BROSAL 175.1  44.2   29   29   28     A     A     A    1    1
    ## 3565 011125 HIRTRA 170.8  72.5   29   34   35     A     A     A    1    1
    ## 3566 011208 AST2GR 194.0  40.5   29   36   38     A     A     A    1    1
    ## 3567 011213 INGAVE 191.1  40.1   29   27   27     A     A     A    1    1
    ## 3568 011253 BROSAL 196.6  41.6   29   32   34     A     A     A    1    1
    ## 3569 011366 BROSAL 197.3  98.8   29   29   29     A     A     A    1    1
    ## 3570 011397 CASEAR 111.1  68.8   29   19   19     A     A     A    1    1
    ## 3571 000010 COCCPA   0.9  14.0   28   29   33     A     A     A    1    1
    ## 3572 000048 BACTMA  13.1   5.8   28   31   30     A     A     A    1    1
    ## 3573 000054 EUGEPR  12.1   9.9   28   20   22     A     A     A    1    1
    ## 3574 000088 CUPASY  19.0  18.2   28   31   34     A     A     A    1    1
    ## 3575 000090 PROTTE  18.0  11.8   28   34   35     A     A     A    1    1
    ## 3576 000111 BACTMA  15.3   4.8   28   31   31     A     A     A    1    1
    ## 3577 000193 EUGEPR  17.3  37.1   28   21   21     A     A     A    1    1
    ## 3578 000249 BACTMA   4.0 231.6   28   28   28     A     A     A    1    1
    ## 3579 000276 COCCPA  17.2  52.0   28   36   38     A     A     A    1    1
    ## 3580 000288 IXORFL  18.8  45.0   28   36   37     A     A     A    1    1
    ## 3581 000477 SWARS1   7.1  85.6   28   33   31     A     A     A    1    2
    ## 3582 000552 COU2CU  19.7  91.7   28   33   34     A     A     A    1    1
    ## 3583 000577 EUGEPR   1.4 105.4   28   29   31     A     A     A    1    1
    ## 3584 000649 ANNOHA  11.0 109.1   28   34   36     A     A     A    1    1
    ## 3585 000737 BACTMA  12.8 123.9   28   -9    0     A     *     B    1    0
    ## 3586 000738 FARAOC  13.7 125.4   28   38   42     A     A     A    1    1
    ## 3587 000823 LACIAG  17.8 157.5   28   31   32     A     A     A    1    1
    ## 3588 000882 SWARS1  14.5 166.1   28   27   27     A     A     A    1    1
    ## 3589 000971 PROTTE  12.2 188.8   28   35   37     A     A     A    1    1
    ## 3590 000973 PIPERE  13.7 190.7   28   26   27     A     A     A    1    2
    ## 3591 001122 PROTTE  11.5 220.1   28   29    0     A     A     B    1    1
    ## 3592 001138 FARAOC  14.6 229.6   28   29   30     A     A     A    1    1
    ## 3593 001139 HYMECO  14.6 227.9   28   26   -1     A     A     D    1    2
    ## 3594 001237 SOROAF   3.7 263.2   28   33   34     A     A     A    1    1
    ## 3595 001455 PROTTE 111.4  18.1   28   29   30     A     A     A    1    1
    ## 3596 001460 EUGECO 117.2  18.2   28   29   30     A     A     A    1    2
    ## 3597 001677 TRIPCU 111.4  66.3   28   14   15     A     A     A    1    1
    ## 3598 001682 AST2GR 111.4  70.2   28   29   32     A     A     A    1    1
    ## 3599 001684 AST2GR 111.4  72.1   28   28   26     A     A     A    1    2
    ## 3600 001694 BURSSI 116.7  77.6   28   16   16     A     A     A    1    1
    ## 3601 001720 FARAOC 102.1  80.5   28   36   50     A     A     A    1    1
    ## 3602 002014 EUGEPR 140.3  18.0   28   32   35     A     A     A    1    1
    ## 3603 002044 PIPERE 157.6  13.4   28   28   29     A     A     A    1    1
    ## 3604 002064 ALIBED 141.3  24.7   28   28   29     A     A     A    1    1
    ## 3605 002101 OURALU 151.4  27.3   28   28   28     A     A     A    1    1
    ## 3606 002114 BROSAL 154.3  39.7   28   30   31     A     A     A    1    1
    ## 3607 002115 ANNOHA 154.6  38.9   28   27   29     A     A     A    1    1
    ## 3608 002121 RINOLI 155.8  35.4   28   28   28     A     A     A    1    1
    ## 3609 002126 RINOLI 159.8  38.8   28   14   16     A     A     A    1    1
    ## 3610 002144 CUPASY 142.3  43.5   28   31   26     A     A     A    1    1
    ## 3611 002156 ALIBED 142.0  50.3   28   26   29     A     A     A    1    1
    ## 3612 002212 ANNOHA 156.2  52.8   28   29   29     A     A     A    1    1
    ## 3613 002214 BROSAL 155.8  54.7   28   32   36     A     A     A    1    1
    ## 3614 002289 BROSAL 150.7  70.7   28   34   35     A     A     A    1    1
    ## 3615 002303 ANNOHA 159.8  67.7   28   28   29     A     A     A    1    1
    ## 3616 003056 HEISCO  30.7   6.4   28   32   34     A     A     A    1    1
    ## 3617 003122 SWARS2  28.4  34.0   28   34   35     A     A     A    1    1
    ## 3618 003199 COU2CU  38.7  28.4   28   29   32     A     A     A    1    1
    ## 3619 003238 ANTITR  29.7  57.6   28   30   29     A     A     A    1    1
    ## 3620 003246 EUGEPR  20.0  64.4   28   33   35     A     A     A    1    1
    ## 3621 003268 CASECO  34.7  52.9   28   31   -1     A     A     D    1    1
    ## 3622 003366 EUGEPR  24.8  80.6   28   30   30     A     A     A    1    1
    ## 3623 003426 BROSAL  34.2  93.9   28   29   29     A     A     A    1    1
    ## 3624 003449 COU2CU  23.3 105.2   28   31   31     A     A     A    1    1
    ## 3625 003642 PROTTE  24.3 153.6   28   32   35     A     A     A    1    1
    ## 3626 003652 PICRLA  28.5 150.9   28   28   30     A     A     A    1    1
    ## 3627 003986 SOROAF  29.0 214.1   28   35   39     A     A     A    1    1
    ## 3628 004039 COU2CU  38.1 218.0   28   28   33     A     A     A    1    1
    ## 3629 004101 BROSAL  37.1 237.0   28   29   -1     A     A     D    1    1
    ## 3630 004126 FARAOC  21.8 244.3   28   29   30     A     A     A    1    1
    ## 3631 004157 THEVAH  29.9 249.3   28   29   30     A     A     A    1    1
    ## 3632 004202 COU2CU  20.5 262.8   28   29   39     A     A     A    1    1
    ## 3633 006027 EUGEPR  47.5  15.4   28   28   30     A     A     A    1    1
    ## 3634 006031 COU2CU  45.2  14.9   28   -9   41     A     *     A    1    0
    ## 3635 006060 POSOLA  50.7  11.2   28   28   31     A     A     A    1    1
    ## 3636 006075 ALIBED  59.0  18.5   28   59   50     A     A     A    1    1
    ## 3637 006081 SOROAF  59.7  14.0   28   32   34     A     A     A    1    1
    ## 3638 006321 TRI2TO  46.7  64.7   28   28   29     A     A     A    1    1
    ## 3639 006367 SOROAF  44.9  85.4   28   19   19     A     A     A    1    1
    ## 3640 006417 COU2CU  43.1 104.9   28   29   31     A     A     A    1    1
    ## 3641 006446 HEISCO  54.4 103.8   28   35   38     A     A     A    1    1
    ## 3642 006508 COU2CU  46.8 130.2   28   27   28     A     A     A    1    1
    ## 3643 006515 COU2CU  46.2 132.3   28   13   14     A     A     A    1    1
    ## 3644 006518 COU2CU  47.3 127.5   28   30   35     A     A     A    1    1
    ## 3645 006617 PROTTE  45.2 142.3   28   30   32     A     A     A    1    1
    ## 3646 006647 COU2CU  56.9 144.6   28   32   33     A     A     A    1    1
    ## 3647 006656 HEISCO  41.2 163.0   28   20   21     A     A     A    1    1
    ## 3648 006772 EUGEPR  40.1 194.2   28   31   33     A     A     A    1    1
    ## 3649 006849 TRI2PL  58.3 199.8   28   29   31     A     A     A    1    1
    ## 3650 006870 FARAOC  44.6 208.6   28   29   30     A     A     A    1    1
    ## 3651 006908 BROSAL  47.0 205.2   28   35   35     A     A     A    1    1
    ## 3652 007033 PROTTE  50.1 238.7   28   31   32     A     A     A    1    1
    ## 3653 007064 COU2CU  45.7 257.2   28   28   29     A     A     A    1    1
    ## 3654 007200 STYLST  43.9 292.3   28   26   24     A     A     A    1    1
    ## 3655 007248 HIRTRA  59.3 297.0   28   28   28     A     A     A    1    1
    ## 3656 007314 ALIBED  84.9  29.0   28   25   25     A     A     A    1    1
    ## 3657 007501 TRIPCU  92.6  66.6   28   40   41     A     A     A    1    1
    ## 3658 007545 FARAOC  89.6  85.8   28   32   35     A     A     A    1    1
    ## 3659 007546 CUPASY  83.0  82.0   28   17    0     A     A     B    1    1
    ## 3660 007561 GUARGL  94.2  85.1   28   28   28     A     A     A    1    1
    ## 3661 007628 IXORFL  95.2 117.6   28   32   32     A     A     A    1    1
    ## 3662 007730 NEEADE  81.4 145.1   28   28   28     A     A     A    1    1
    ## 3663 007817 BROSAL  99.9 146.7   28   32   33     A     A     A    1    1
    ## 3664 007821 LACIAG  99.9 144.2   28   29   30     A     A     A    1    1
    ## 3665 007843 GUARGL  88.7 179.4   28   29   28     A     A     A    1    1
    ## 3666 008010 PROTTE  95.1 193.1   28   35   38     A     A     A    1    1
    ## 3667 008017 PIPERE  96.4 185.3   28   33   35     A     A     A    1    1
    ## 3668 008055 PROTTE  80.1 214.9   28   34   28     A     A     A    1    1
    ## 3669 008078 BROSAL  86.1 208.3   28   28   28     A     A     A    1    1
    ## 3670 008127 PROTTE  82.5 224.6   28   38   42     A     A     A    1    1
    ## 3671 008295 COPAAR  94.5 249.2   28   29   29     A     A     A    1    1
    ## 3672 008334 BROSAL  95.8 245.1   28   28   28     A     A     A    1    1
    ## 3673 008341 IXORFL  97.1 241.4   28   33   33     A     A     A    1    1
    ## 3674 008356 SLOATE  84.0 270.1   28   27   27     A     A     A    1    1
    ## 3675 008559 PIPERE 184.9  25.0   28   33   34     A     A     A    1    1
    ## 3676 008574 PIT1RU 188.7  35.1   28    0    0     A     B     B    1    2
    ## 3677 008590 PIPERE 193.7  19.9   28   -9   27     A     *     A    1    0
    ## 3678 009065 HIRTRA  72.5  18.7   28   29   32     A     A     A    1    1
    ## 3679 009494 POSOLA  63.7 159.5   28   32   32     A     A     A    1    1
    ## 3680 009543 INGAVE  75.1 140.1   28   34   35     A     A     A    1    1
    ## 3681 009608 BROSAL  70.9 179.9   28   28   30     A     A     A    1    1
    ## 3682 009633 PROTTE  63.5 181.2   28   29   29     A     A     A    1    1
    ## 3683 009718 HEISCO  79.1 191.2   28   36   38     A     A     A    1    1
    ## 3684 009781 TRI2PL  70.8 200.9   28   26   12     A     A     A    1    1
    ## 3685 009797 ALIBED  71.5 208.2   28   28   28     A     A     A    1    1
    ## 3686 009801 PROTTE  74.7 208.5   28   35   36     A     A     A    1    1
    ## 3687 009845 PROTTE  78.2 204.7   28   29   28     A     A     A    1    1
    ## 3688 009851 BROSAL  77.3 203.1   28   29   29     A     A     A    1    1
    ## 3689 009917 IXORFL  77.7 238.1   28   33   34     A     A     A    1    1
    ## 3690 009936 TROPRA  79.2 222.5   28   36   37     A     A     A    1    1
    ## 3691 009957 PROTTE  66.3 254.0   28   33   31     A     A     A    1    1
    ## 3692 009970 EUGEPR  70.1 250.8   28   28   28     A     A     A    1    1
    ## 3693 010076 FARAOC  78.2 274.3   28   32   35     A     A     A    1    1
    ## 3694 010083 FARAOC  63.9 284.4   28   35   36     A     A     A    1    1
    ## 3695 010170 PROTTE 134.6   4.0   28   29   30     A     A     A    1    1
    ## 3696 010288 INGAVE 128.5  59.2   28   34   36     A     A     A    1    1
    ## 3697 010303 IXORFL 128.6  44.5   28   34   36     A     A     A    1    1
    ## 3698 010336 TRIPCU 134.3  58.5   28   29   32     A     A     A    1    1
    ## 3699 010412 AST2GR 134.6  61.5   28   28   29     A     A     A    1    1
    ## 3700 010499 SOROAF 134.5  95.4   28   29   30     A     A     A    1    1
    ## 3701 011075 BROSAL 179.8  43.4   28   29   27     A     A     A    1    1
    ## 3702 011139 FARAOC 176.3  74.2   28   30   32     A     A     A    1    1
    ## 3703 011313 PROTTE 197.8  78.7   28   35   35     A     A     A    1    1
    ## 3704 011332 FARAOC 196.8  60.5   28   32   35     A     A     A    1    1
    ## 3705 000032 POSOLA   9.6   7.2   27   31   32     A     A     A    1    1
    ## 3706 000077 FARAOC  14.7  16.8   27   33   36     A     A     A    1    1
    ## 3707 000104 CLAVME  17.6   9.4   27   26   26     A     A     A    1    1
    ## 3708 000258 FARAOC  14.5  57.6   27   39   40     A     A     A    1    1
    ## 3709 000318 EUGEPR   4.5  77.1   27   28   28     A     A     A    1    1
    ## 3710 000335 CHOMSP   8.7  66.4   27   31   35     A     A     A    1    1
    ## 3711 000421 HEISCO   4.7  84.0   27   29   41     A     A     A    1    1
    ## 3712 000489 SOROAF   7.2  83.7   27   29   32     A     A     A    1    2
    ## 3713 000530 COU2CU  14.8  98.5   27   29   29     A     A     A    1    1
    ## 3714 000531 COU2CU  13.7  98.7   27   26   26     A     A     A    1    2
    ## 3715 000541 COU2CU  18.5  96.2   27   -1   -1     A     D     D    1    0
    ## 3716 000584 COU2CU   4.4 110.2   27   29   30     A     A     A    1    1
    ## 3717 000638 COU2CU  11.6 100.2   27   31   32     A     A     A    1    1
    ## 3718 000678 BROSAL  16.4 100.2   27   27   27     A     A     A    1    1
    ## 3719 000734 PIPERE   5.9 123.0   27   27   31     A     A     A    1    1
    ## 3720 000763 BACTMA  15.4 127.5   27   27   27     A     A     A    1    1
    ## 3721 000806 HEISCO  11.6 149.8   27   29   32     A     A     A    1    1
    ## 3722 000821 COU2CU  16.3 157.9   27   29   28     A     A     A    1    1
    ## 3723 000842 SOROAF   1.1 172.5   27   28   30     A     A     A    1    1
    ## 3724 001009 SOROAF   0.6 218.0   27   27   27     A     A     A    1    1
    ## 3725 001048 BROSAL  14.6 207.4   27   28   30     A     A     A    1    1
    ## 3726 001067 SOROAF  18.8 205.1   27   29   30     A     A     A    1    1
    ## 3727 001081 FARAOC  19.0 202.7   27   29   31     A     A     A    1    1
    ## 3728 001114 SOROAF   9.2 231.5   27   30   30     A     A     A    1    1
    ## 3729 001174 CLAVME   1.2 255.3   27   28   28     A     A     A    1    1
    ## 3730 001356 MACRGL  11.3 284.8   27   56   59     A     A     A    1    1
    ## 3731 001372 CLAVME  16.9 299.1   27   31   31     A     A     A    1    1
    ## 3732 001414 CUPASY 107.7  14.7   27   27   29     A     A     A    1    1
    ## 3733 001606 CASEGU 119.1  57.5   27   28   28     A     A     A    1    2
    ## 3734 001678 RANDFO 111.2  68.5   27   32   28     A     A     A    1    1
    ## 3735 001755 CUPASY 114.8  85.1   27   27   27     A     A     A    1    1
    ## 3736 002013 TRI2PL 141.6  16.8   27   -1   -1     A     D     D    1    0
    ## 3737 002057 POSOLA 155.4   0.8   27   28   30     A     A     A    1    1
    ## 3738 002128 RINOLI 157.5  37.0   27   27   22     A     A     A    1    1
    ## 3739 002265 EUGEPR 151.2  74.1   27   27   22     A     A     A    1    1
    ## 3740 002374 BACTMA 152.0  90.2   27   27   27     A     A     A    1    1
    ## 3741 003029 ALIBED  20.7  35.0   27   -1   -1     A     D     D    1    0
    ## 3742 003030 SAP1SA  23.8  16.8   27   35   35     A     A     A    1    1
    ## 3743 003088 PIT1RU  36.9   1.8   27   31   31     A     A     A    1    1
    ## 3744 003150 CUPASY  30.2  21.4   27   27   28     A     A     A    1    1
    ## 3745 003381 OCOTRU  20.2  91.5   27   28   30     A     A     A    1    1
    ## 3746 003456      *  26.1  55.9   27   -1   -1     A     D     D    1    0
    ## 3747 003555 COU2CU  23.3 135.1   27    0    0     A     B     B    1    2
    ## 3748 003819 COU2CU  22.4 193.9   27   30   34     A     A     A    1    1
    ## 3749 003966 SOROAF  21.4 212.4   27   29   30     A     A     A    1    1
    ## 3750 003987 POSOLA  29.7 213.2   27   30   30     A     A     A    1    1
    ## 3751 004040 COU2CU  38.7 217.6   27   29   33     A     A     A    1    1
    ## 3752 004046 ANNOHA  36.5 211.6   27   28   29     A     A     A    1    1
    ## 3753 004103 SOROAF  32.9 235.4   27   28   28     A     A     A    1    1
    ## 3754 004127 OURALU  23.2 244.9   27    0   17     A     B     A    1    2
    ## 3755 004137 OURALU  22.8 255.4   27   22   23     A     A     A    1    1
    ## 3756 004249 BROSAL  34.8 272.5   27   26   29     A     A     A    1    1
    ## 3757 004255 TURNPA  36.5 271.9   27   28   30     A     A     A    1    2
    ## 3758 004328 FARAOC  33.9 298.7   27   32   32     A     A     A    1    1
    ## 3759 006161 EUGEPR  50.9  33.8   27   28   30     A     A     A    1    1
    ## 3760 006180 EUGEPR  58.3  23.8   27   29   31     A     A     A    1    1
    ## 3761 006227 PROTTE  54.9  44.2   27   28   30     A     A     A    1    2
    ## 3762 006331 EUGEPR  53.7  67.2   27   27   27     A     A     A    1    1
    ## 3763 006495 EUGEPR  43.8 139.9   27   27   27     A     A     A    1    1
    ## 3764 006529 COU2CU  50.7 124.8   27   27   29     A     A     A    1    1
    ## 3765 006584 COU2CU  56.9 132.7   27   27   24     A     A     A    1    1
    ## 3766 006625 CHR2CA  53.0 145.6   27   35   27     A     A     A    1    1
    ## 3767 006655 SOROAF  40.1 163.6   27   21   22     A     A     A    1    1
    ## 3768 006743 BACTMA  56.7 174.8   27   24    0     A     A     B    1    1
    ## 3769 006936 TRI2PL  50.0 208.6   27   42   45     A     A     A    1    1
    ## 3770 006978 SOROAF  43.5 225.6   27   26   26     A     A     A    1    1
    ## 3771 006985 OURALU  44.9 232.5   27   25   30     A     A     A    1    1
    ## 3772 007049 EUGEPR  59.9 230.8   27   31   32     A     A     A    1    1
    ## 3773 007052 BACTMA  57.9 228.8   27   -1   -1     A     D     D    1    0
    ## 3774 007058 FARAOC  42.2 252.0   27   27   27     A     A     A    1    1
    ## 3775 007066 STYLST  47.3 259.6   27   27   31     A     A     A    1    1
    ## 3776 007167 HIRTRA  58.3 275.1   27   13    0     A     A     B    1    1
    ## 3777 007312 AST2GR  82.3  29.5   27   20   20     A     A     A    1    1
    ## 3778 007366 ACACME  90.2  36.3   27   29   29     A     A     A    1    1
    ## 3779 007491 TRIPCU  89.1  68.5   27   27   28     A     A     A    1    1
    ## 3780 007548 EUGEPR  85.3  88.4   27   27   29     A     A     A    1    1
    ## 3781 007551 COCCPA  89.5  81.1   27   31   35     A     A     A    1    1
    ## 3782 007597 FARAOC  95.0  82.6   27   32   35     A     A     A    1    1
    ## 3783 007860 TRI2PL  86.9 163.1   27   27   29     A     A     A    1    1
    ## 3784 007934 HIRTRA  85.0 198.6   27   25   28     A     A     A    1    1
    ## 3785 007949 CUPASY  89.5 193.7   27   32   32     A     A     A    1    1
    ## 3786 007964 BROSAL  89.8 187.6   27   31   35     A     A     A    1    2
    ## 3787 008245 SOROAF  82.8 256.7   27   28   29     A     A     A    1    1
    ## 3788 008254 SOROAF  89.6 259.6   27   31   35     A     A     A    1    1
    ## 3789 008268 FARAOC  87.9 251.8   27   32   33     A     A     A    1    1
    ## 3790 008273 EUGEPR  86.3 246.3   27   33   38     A     A     A    1    1
    ## 3791 008469 SWARS1  93.8 299.4   27   27   29     A     A     A    1    1
    ## 3792 008547 PROTTE 195.8  10.1   27   20    0     A     A     B    1    1
    ## 3793 008556 AST2GR 181.4  22.7   27    0   11     A     B     A    1    2
    ## 3794 008575 BROSAL 188.4  35.5   27   30   30     A     A     A    1    1
    ## 3795 008594 PIPERE 191.2  21.9   27   28   28     A     A     A    1    1
    ## 3796 009047 EUGEPR  67.2   4.8   27   28   30     A     A     A    1    1
    ## 3797 009153 TRI2PL  79.1  23.3   27   28   28     A     A     A    1    1
    ## 3798 009303 ANNOHA  79.3  78.7   27   28   30     A     A     A    1    1
    ## 3799 009316 SOROAF  79.2  60.1   27   29   31     A     A     A    1    1
    ## 3800 009358 PIPERE  77.0  85.9   27   36   37     A     A     A    1    1
    ## 3801 009376 PROTTE  61.9 114.1   27   29   30     A     A     A    1    1
    ## 3802 009458 POSOLA  77.4 132.5   27   27   27     A     A     A    1    1
    ## 3803 009469 SOROAF  78.8 128.3   27   27   27     A     A     A    1    1
    ## 3804 009537 SOROAF  79.4 149.4   27   31   32     A     A     A    1    1
    ## 3805 009568 HEISCO  67.8 175.2   27   31   33     A     A     A    1    1
    ## 3806 009626 SWARS1  75.7 163.8   27   30   31     A     A     A    1    1
    ## 3807 009650 FARAOC  64.8 197.2   27   27   27     A     A     A    1    1
    ## 3808 009661 BROSAL  68.7 197.2   27   27   28     A     A     A    1    1
    ## 3809 009681 BROSAL  72.2 183.8   27   30   31     A     A     A    1    1
    ## 3810 009731 FARAOC  60.2 203.8   27   32   32     A     A     A    1    1
    ## 3811 009736 MICOIM  62.6 202.7   27   27   26     A     A     A    1    1
    ## 3812 009902 CUPASY  74.2 232.5   27   29   32     A     A     A    1    1
    ## 3813 009918 PROTTE  79.5 230.9   27   29   30     A     A     A    1    1
    ## 3814 009959 BACTMA  69.7 254.5   27    0    0     A     B     B    1    2
    ## 3815 010177 DENDAR 134.5  12.9   27   32   32     A     A     A    1    1
    ## 3816 010261 ALIBED 136.4  33.0   27   27   27     A     A     A    1    1
    ## 3817 010285 CASEA1 120.5  55.3   27   29   31     A     A     A    1    1
    ## 3818 010322 TRIPCU 134.6  47.6   27   33   40     A     A     A    1    1
    ## 3819 010429 TROPRA 136.6  75.3   27   28   31     A     A     A    1    1
    ## 3820 010457 ALIBED 120.4  93.0   27   27   27     A     A     A    1    1
    ## 3821 010971 BROSAL 164.3  36.5   27   28   29     A     A     A    1    1
    ## 3822 010988 PIPERE 170.8  26.6   27   28   27     A     A     A    1    1
    ## 3823 011241 SOROAF 199.6  52.7   27   28    0     A     A     B    1    1
    ## 3824 000003 EUGEPR   1.3   2.3   26   33   39     A     A     A    1    2
    ## 3825 000021 POSOLA   7.1  15.6   26   25   25     A     A     A    1    2
    ## 3826 000087 PICRLA  19.4  19.8   26   39   41     A     A     A    1    1
    ## 3827 000203 ALBIAD  15.1  29.5   26   27   28     A     A     A    1    1
    ## 3828 000270 SWARS1  16.6  59.3   26   26   29     A     A     A    1    1
    ## 3829 000432 OURALU   0.2  88.8   26   26   26     A     A     A    1    1
    ## 3830 000481 COU2CU   9.8  86.1   26   32   34     A     A     A    1    1
    ## 3831 000484 COU2CU   6.9  87.3   26   31   33     A     A     A    1    2
    ## 3832 000504 COU2CU  14.6  88.5   26   18   27     A     A     A    1    1
    ## 3833 000559 COU2CU  18.7  89.3   26   33   36     A     A     A    1    1
    ## 3834 000907 SOROAF  18.7 171.1   26   28   28     A     A     A    1    1
    ## 3835 000956 EUGEPR   9.0 186.4   26   34   34     A     A     A    1    1
    ## 3836 001001 LACIAG   3.0 211.6   26   27   27     A     A     A    1    1
    ## 3837 001080 IXORFL  18.3 203.7   26   27   27     A     A     A    1    1
    ## 3838 001136 FARAOC  12.8 228.1   26   29   31     A     A     A    1    1
    ## 3839 001156 BACTMA   5.7 233.2   26   26   27     A     A     A    1    1
    ## 3840 001287 SWARS1  12.5 274.0   26    0    0     A     B     B    1    2
    ## 3841 001397 POSOLA 102.5  14.2   26   25   27     A     A     A    1    1
    ## 3842 001431 MYRCGA 112.0   6.6   26   26   28     A     A     A    1    1
    ## 3843 001533 PROTTE 104.3  44.6   26   31   33     A     A     A    1    1
    ## 3844 001539 PROTTE 102.3  45.8   26   29   31     A     A     A    1    1
    ## 3845 001668 CAL2CA 114.3  60.6   26   28   28     A     A     A    1    1
    ## 3846 001713 CAL2CA 117.2  64.3   26   33   36     A     A     A    1    2
    ## 3847 001714 AST2GR 117.5  64.7   26   33   35     A     A     A    1    2
    ## 3848 001763 ALBIPR 112.5  94.9   26   32   33     A     A     A    1    1
    ## 3849 002056 PROTTE 157.1   1.1   26   26   25     A     A     A    1    1
    ## 3850 002088 COPAAR 149.1  21.2   26   27   29     A     A     A    1    1
    ## 3851 002098 ANNOHA 150.9  23.2   26   27   28     A     A     A    1    1
    ## 3852 002143 EUGEPR 141.5  43.2   26   28   29     A     A     A    1    1
    ## 3853 002188 ANNOHA 146.6  41.9   26   12   12     A     A     A    1    1
    ## 3854 002192 FARAOC   6.7  92.4   26   34   37     A     A     A    1    1
    ## 3855 002248 ANNOHA 144.7  72.6   26   28   26     A     A     A    1    1
    ## 3856 002271 SOROAF 145.2  66.2   26   28   32     A     A     A    1    1
    ## 3857 002287 COU2CU 153.7  69.1   26   30   32     A     A     A    1    1
    ## 3858 003005 AST2GR  24.7   2.4   26   35   36     A     A     A    1    1
    ## 3859 003007 EUGEPR  20.3  15.9   26   27   25     A     A     A    1    1
    ## 3860 003010 SWARS1  22.7   6.3   26   35   33     A     A     A    1    1
    ## 3861 003016 EUGEPR  22.0   7.4   26   25   25     A     A     A    1    1
    ## 3862 003024 EUGEPR  20.6  13.6   26   30   30     A     A     A    1    1
    ## 3863 003031 FARAOC  22.5  16.6   26   28   29     A     A     A    1    1
    ## 3864 003045 FARAOC  29.2  17.6   26   37   35     A     A     A    1    1
    ## 3865 003075 BACTMA  37.2  16.5   26   -1   -1     A     D     D    1    0
    ## 3866 003102 BROSAL  24.3  29.9   26   32   34     A     A     A    1    1
    ## 3867 003121 PICRLA  26.1  32.8   26   28   30     A     A     A    1    1
    ## 3868 003352 BROSAL  39.3  75.6   26   27   29     A     A     A    1    1
    ## 3869 003374 COU2CU  22.5  86.2   26   34   34     A     A     A    1    1
    ## 3870 003430 COU2CU  34.6  98.9   26   34   37     A     A     A    1    1
    ## 3871 003459 FARAOC  23.9 115.2   26   -1   -1     A     D     D    1    0
    ## 3872 003527 SOROAF  37.7 101.4   26   30   31     A     A     A    1    1
    ## 3873 003630 EUGEPR  21.9 146.8   26   29   29     A     A     A    1    1
    ## 3874 003691 BROSAL  32.2 155.2   26   27   26     A     A     A    1    1
    ## 3875 003753 HEISCO  30.1 162.5   26   34   35     A     A     A    1    1
    ## 3876 003795 THEVAH  39.3 164.3   26   28   28     A     A     A    1    1
    ## 3877 003923 CASECO  37.2 194.0   26   35   38     A     A     A    1    1
    ## 3878 004017 SOROAF  33.7 213.0   26   27   27     A     A     A    1    1
    ## 3879 004092 SLOATE  30.2 221.7   26   26   27     A     A     A    1    1
    ## 3880 004111 EUGEPR  37.4 221.3   26   26   30     A     A     A    1    1
    ## 3881 004155 COU2CU  26.4 249.7   26   28   28     A     A     A    1    1
    ## 3882 004228 BACTMA  25.1 265.2   26    0   -1     A     B     D    1    2
    ## 3883 006065 PROTTE  46.1 244.9   26   25    0     A     A     B    1    1
    ## 3884 006178 EUGEPR  59.0  27.1   26   28   33     A     A     A    1    1
    ## 3885 006198 COU2CU  42.8  55.4   26   28   31     A     A     A    1    1
    ## 3886 006262 CUPASY  55.6  48.5   26   27   26     A     A     A    1    1
    ## 3887 006290 AST2GR  44.9  70.6   26   26   25     A     A     A    1    2
    ## 3888 006346 SOROAF  56.2  68.6   26   28   20     A     A     A    1    1
    ## 3889 006350 EUGEPR  57.2  61.0   26   27   26     A     A     A    1    1
    ## 3890 006371 CLAVME  41.2  89.8   26   27   27     A     A     A    1    1
    ## 3891 006373 EUGEPR  44.9  87.0   26   26   27     A     A     A    1    1
    ## 3892 006725 HEISCO  50.6 172.8   26   22   26     A     A     A    1    1
    ## 3893 006761 PROTTE  41.7 188.4   26   34   36     A     A     A    1    1
    ## 3894 006765 ACALDI  43.5 187.4   26   37   39     A     A     A    1    1
    ## 3895 006795 PROTTE  45.9 190.9   26   39   40     A     A     A    1    1
    ## 3896 007100 BACTMA  40.5 261.6   26   -1   -1     A     D     D    1    0
    ## 3897 007186 EUGEPR  55.2 264.3   26   27   27     A     A     A    1    1
    ## 3898 007324 BROSAL  80.2  36.8   26   28   28     A     A     A    1    1
    ## 3899 007351 GUARGL  93.4  25.0   26   30   -1     A     A     D    1    1
    ## 3900 007440 ACACME  91.3  50.8   26   -1   -1     A     D     D    1    0
    ## 3901 007475 EUGEPR  80.9  66.6   26   29   30     A     A     A    1    1
    ## 3902 007596 EUGEPR  95.7  80.8   26   27   27     A     A     A    1    1
    ## 3903 007613 BROSAL  86.8 105.7   26   26   27     A     A     A    1    1
    ## 3904 007678 PIPERE  88.0 134.2   26   30    0     A     A     B    1    1
    ## 3905 007679 PROTTE  89.8 134.6   26   29   28     A     A     A    1    1
    ## 3906 007744 AST2GR  82.4 152.8   26   34   30     A     A     A    1    1
    ## 3907 007796 PROTTE  90.4 155.1   26   30   30     A     A     A    1    1
    ## 3908 007816 GUARGL  98.8 147.6   26   28   29     A     A     A    1    1
    ## 3909 007915 FARAOC  82.7 189.1   26   32   35     A     A     A    1    1
    ## 3910 007921 BROSAL  81.2 194.2   26   27   27     A     A     A    1    1
    ## 3911 008056 CUPASY  84.1 214.9   26   26   25     A     A     A    1    1
    ## 3912 008249 ALIBED  88.9 256.1   26   26   27     A     A     A    1    1
    ## 3913 008251 SOROAF  86.3 255.6   26   35   37     A     A     A    1    1
    ## 3914 008381 EUGEPR  87.7 262.7   26   26   27     A     A     A    1    1
    ## 3915 008393 HIRTRA  93.5 263.5   26   26   27     A     A     A    1    1
    ## 3916 008435 PROTTE  98.7 263.3   26   26   26     A     A     A    1    1
    ## 3917 008483 PROTTE 182.4   5.0   26   26   26     A     A     A    1    1
    ## 3918 008487 PROTTE 183.2   2.9   26   26   27     A     A     A    1    1
    ## 3919 009020 EUGEPR  61.2  17.6   26   28   29     A     A     A    1    1
    ## 3920 009048 HEISCO  67.9   4.0   26   27   27     A     A     A    1    1
    ## 3921 009211 RANDFO  72.8  53.8   26   26   28     A     A     A    1    1
    ## 3922 009214 EUGEPR  72.5  56.2   26   26   25     A     A     A    1    1
    ## 3923 009259 FARAOC  62.6  79.5   26   28   30     A     A     A    1    1
    ## 3924 009269 AST2GR  65.4  71.6   26   28   28     A     A     A    1    1
    ## 3925 009362 FARAOC  79.0  88.8   26   29   31     A     A     A    1    1
    ## 3926 009612 INGAFA  76.1 175.4   26   32   35     A     A     A    1    1
    ## 3927 009623 CAVAPL  77.4 173.0   26   32   32     A     A     A    1    1
    ## 3928 009689 ALIBED  74.1 191.6   26   26   26     A     A     A    1    1
    ## 3929 009835 SOROAF  79.3 207.2   26   29   30     A     A     A    1    1
    ## 3930 009998 ANNOHA  78.4 254.9   26   28   31     A     A     A    1    1
    ## 3931 010005 POSOLA  75.5 240.5   26   26   28     A     A     A    1    1
    ## 3932 010099 GUARGL  64.1 291.5   26   -1   -1     A     D     D    1    0
    ## 3933 010185 ANNOHA 136.3  17.5   26   28   28     A     A     A    1    1
    ## 3934 010186 RINOLI 138.5  19.8   26   28   29     A     A     A    1    1
    ## 3935 010203 TROPRA 122.4  25.9   26   27   27     A     A     A    1    1
    ## 3936 010266 RANDFO 136.4  24.8   26   -1   -1     A     D     D    1    0
    ## 3937 010279 TRI2PL 124.2  51.1   26   -1   -1     A     D     D    1    0
    ## 3938 010293 TRIPCU 137.7  38.9   26   32   32     A     A     A    1    1
    ## 3939 010304 COPAAR 129.5  43.7   26   27   27     A     A     A    1    1
    ## 3940 010310 ALIBED 130.4  43.6   26   27   27     A     A     A    1    1
    ## 3941 010455 ANNOHA 124.1  89.5   26   26   26     A     A     A    1    1
    ## 3942 010489 BACTMA 133.9  87.4   26   26   26     A     A     A    1    1
    ## 3943 010512   ANN1 139.1  97.1   26   26   30     A     A     A    1    1
    ## 3944 010993 PROTTE 170.8  32.6   26   30    0     A     A     B    1    1
    ## 3945 011078 ORMOMA 161.9  68.2   26   29   28     A     A     A    1    1
    ## 3946 011115 PROTTE 167.7  69.6   26   29   30     A     A     A    1    1
    ## 3947 011252 PSYCHO 197.4  41.1   26   29   29     A     A     A    1    1
    ## 3948 011254 INGAVE 197.0  42.1   26    0    0     A     B     B    1    2
    ## 3949 011292 LACIAG 187.1  63.2   26   27   27     A     A     A    1    1
    ## 3950 011316 BROSAL 195.6  72.9   26   30   30     A     A     A    1    1
    ## 3951 011375 IXORFL 198.4  89.9   26   28   31     A     A     A    1    1
    ## 3952 011406 TRIPCU 139.2  30.6   26   20   21     A     A     A    1    1
    ## 3953 000067 SWARS1  14.6  12.6   25   27   29     A     A     A    1    1
    ## 3954 000106 PROTTE  18.1   8.1   25   34   39     A     A     A    1    1
    ## 3955 000116 COU2CU  18.9   2.5   25   29   30     A     A     A    1    1
    ## 3956 000162 POSOLA  12.4  25.3   25   25   28     A     A     A    1    1
    ## 3957 000188 AST2GR  16.3  39.3   25   34   38     A     A     A    1    1
    ## 3958 000245 EUGEPR  10.6  47.6   25   -9    0     A     *     B    1    0
    ## 3959 000247 CUPASY  11.1  53.0   25   24   25     A     A     A    1    1
    ## 3960 000275 EUGEPR  19.3  59.0   25   25   28     A     A     A    1    2
    ## 3961 000298 POSOLA  15.9  43.1   25   23   23     A     A     A    1    1
    ## 3962 000305 CAL2CA   2.0  62.9   25   28   30     A     A     A    1    2
    ## 3963 000315 COU2CU   0.4  70.5   25   24   25     A     A     A    1    1
    ## 3964 000428 HEISCO   0.9  85.5   25   29   31     A     A     A    1    1
    ## 3965 000482 ALIBED   7.4  86.7   25   26   27     A     A     A    1    1
    ## 3966 000501 FARAOC  10.3  87.5   25   26   27     A     A     A    1    1
    ## 3967 000534 EUGECO  16.7  95.1   25   25   25     A     A     A    1    1
    ## 3968 000599 CUPASY   3.7 116.9   25   -9   14     A     *     A    1    0
    ## 3969 000609 SOROAF   8.3 116.7   25   27   28     A     A     A    1    1
    ## 3970 000651 COU2CU  14.4 107.9   25   30   34     A     A     A    1    2
    ## 3971 000664 PIPERE  15.1 112.3   25   24   26     A     A     A    1    1
    ## 3972 000733 SOROAF   6.3 120.2   25   26   26     A     A     A    1    1
    ## 3973 000761 PROTTE  18.3 126.0   25   27   27     A     A     A    1    1
    ## 3974 000770 FARAOC   4.2 140.7   25   27   27     A     A     A    1    1
    ## 3975 000795 SOROAF   8.8 140.8   25   27   27     A     A     A    1    1
    ## 3976 000847 SOROAF   0.2 175.3   25   27   27     A     A     A    1    1
    ## 3977 000894 AST2GR  10.8 179.2   25   -1   -1     A     D     D    1    0
    ## 3978 000914 BROSAL  16.3 166.0   25   27   27     A     A     A    1    1
    ## 3979 000937 SOROAF   2.2 193.4   25   28   29     A     A     A    1    1
    ## 3980 000959 COU2CU   7.4 181.0   25   26   27     A     A     A    1    1
    ## 3981 000991 NEEADE  17.3 181.6   25   26   26     A     A     A    1    1
    ## 3982 001015 SOROAF  10.0 219.0   25   27   27     A     A     A    1    1
    ## 3983 001040 SOROAF  12.6 205.9   25   29   30     A     A     A    1    1
    ## 3984 001060 SOROAF  17.6 211.7   25   25   26     A     A     A    1    1
    ## 3985 001073 SOROAF  16.8 209.3   25   25   27     A     A     A    1    1
    ## 3986 001112 BACTMA   8.2 230.9   25   32    0     A     A     B    1    1
    ## 3987 001147 BROSAL  12.4 236.1   25   26   26     A     A     A    1    1
    ## 3988 001161 MYRCGA   4.6 242.1   25   26   -1     A     A     D    1    1
    ## 3989 001212 SWARS1  17.6 258.4   25   24   25     A     A     A    1    1
    ## 3990 001225 SOROAF  15.6 244.2   25   26    0     A     A     B    1    1
    ## 3991 001258 FARAOC   8.8 277.0   25   26   26     A     A     A    1    1
    ## 3992 001297 CLAVME  17.1 278.5   25   25   25     A     A     A    1    1
    ## 3993 001359 OURALU  14.2 281.7   25   29   29     A     A     A    1    1
    ## 3994 001387 SOROAF  15.2 236.5   25   23   24     A     A     A    1    1
    ## 3995 001495 BROSAL 108.4  31.7   25   26   28     A     A     A    1    1
    ## 3996 001526 ALIBED 116.0  20.6   25   -1   -1     A     D     D    1    0
    ## 3997 001573 BROSAL 105.6  43.9   25   30   30     A     A     A    1    1
    ## 3998 001680 RANDFO 114.9  66.7   25   24   23     A     A     A    1    1
    ## 3999 001681 STERAP 113.6  70.1   25   29   30     A     A     A    1    1
    ## 4000 001685 ACACME 111.9  73.5   25   26    0     A     A     B    1    1
    ## 4001 001695 ALIBED 117.2  79.3   25   28   29     A     A     A    1    1
    ## 4002 002025 NECTMA 149.2   9.1   25   26   25     A     A     A    1    1
    ## 4003 002124 RINOLI 157.2  39.4   25   27   27     A     A     A    1    1
    ## 4004 002133 RINOLI 157.0  33.4   25   27   28     A     A     A    1    1
    ## 4005 002179 PICRLA 148.5  56.4   25   27   30     A     A     A    1    1
    ## 4006 002221 CHR2CA 156.6  41.4   25   27   27     A     A     A    1    1
    ## 4007 002270 ALIBED 145.4  66.1   25   25   25     A     A     A    1    1
    ## 4008 002280 PROTTE 153.0  67.2   25   29   32     A     A     A    1    1
    ## 4009 003043 COU2CU  29.2  19.8   25   27   30     A     A     A    1    1
    ## 4010 003090 COCCPA  22.8  20.3   25   32   35     A     A     A    1    1
    ## 4011 003163 ANNOHA  31.2  28.8   25   27   28     A     A     A    1    1
    ## 4012 003207 TRI2PL  21.8  40.3   25   41   50     A     A     A    1    1
    ## 4013 003249 EUGEPR  25.6  44.2   25   26   22     A     A     A    1    1
    ## 4014 003302 RINOLI  24.3  63.3   25   29   29     A     A     A    1    1
    ## 4015 003498 COU2CU  34.1 109.6   25   30   32     A     A     A    1    1
    ## 4016 003545 COU2CU  23.2 126.4   25   28   32     A     A     A    1    1
    ## 4017 003551 COU2CU  21.7 130.4   25   31   34     A     A     A    1    1
    ## 4018 003645 SOROAF  24.7 155.2   25   30   30     A     A     A    1    1
    ## 4019 003680 FARAOC  31.3 149.0   25   28   29     A     A     A    1    1
    ## 4020 003697 SOROAF  37.6 249.0   25   12   14     A     A     A    1    1
    ## 4021 003715 COU2CU  37.6 142.9   25   28   33     A     A     A    1    1
    ## 4022 003767 COU2CU  34.0 175.5   25   25   28     A     A     A    1    1
    ## 4023 003813 FARAOC  23.4 191.1   25   31   36     A     A     A    1    1
    ## 4024 003864 HEISCO  34.3 184.9   25   28   36     A     A     A    1    1
    ## 4025 003887 MYRCGA  32.1 194.0   25   -1   -1     A     D     D    1    0
    ## 4026 003916 COU2CU  38.1 191.4   25   26   26     A     A     A    1    1
    ## 4027 003926 PROTTE  39.7 192.9   25   42   47     A     A     A    1    1
    ## 4028 004080 FARAOC  26.4 230.2   25   25   27     A     A     A    1    1
    ## 4029 004207 EUGEPR  21.5 269.7   25   28   33     A     A     A    1    1
    ## 4030 004257 INGAVE  38.4 265.8   25   -1   -1     A     D     D    1    0
    ## 4031 004277 FARAOC  23.8 290.2   25   25   25     A     A     A    1    1
    ## 4032 004305 FARAOC  25.2 288.2   25   29   31     A     A     A    1    1
    ## 4033 006001 AST2GR  40.5   0.6   25   25   26     A     A     A    1    1
    ## 4034 006044 POSOLA  46.6   1.3   25   29   31     A     A     A    1    1
    ## 4035 006199 COCCPA  40.1  56.3   25   28   30     A     A     A    1    1
    ## 4036 006226 COU2CU  48.7  42.5   25   36   35     A     A     A    1    1
    ## 4037 006248 COU2CU  55.8  57.3   25   25   27     A     A     A    1    1
    ## 4038 006264 HEISCO  55.1  41.3   25   27   31     A     A     A    1    1
    ## 4039 006273 COU2CU  44.7  61.2   25   27   30     A     A     A    1    1
    ## 4040 006294 PROTTE  41.9  79.7   25   23   24     A     A     A    1    1
    ## 4041 006326 CHR2CA  54.1  64.9   25   24   27     A     A     A    1    1
    ## 4042 006341 EUGEPR  57.6  79.9   25   26   26     A     A     A    1    1
    ## 4043 006351 COU2CU  55.8  60.8   25   29   32     A     A     A    1    1
    ## 4044 006355 ANNOHA  57.1  64.9   25   31   33     A     A     A    1    1
    ## 4045 006363 EUGEPR  44.9  83.5   25   28   29     A     A     A    1    1
    ## 4046 006370 PROTTE  42.4  86.0   25   26   27     A     A     A    1    1
    ## 4047 006556 COU2CU  50.2 133.7   25   27   27     A     A     A    1    1
    ## 4048 006574 COU2CU  59.6 139.4   25   26   27     A     A     A    1    1
    ## 4049 006660 FARAOC  44.7 162.2   25   35   37     A     A     A    1    1
    ## 4050 006769 PROTTE  43.0 190.5   25   41   44     A     A     A    1    1
    ## 4051 006790 SOROAF  49.6 196.6   25   43   46     A     A     A    1    1
    ## 4052 006882 PROTTE  42.2 219.5   25   27   27     A     A     A    1    1
    ## 4053 006900 RINOLI  46.2 213.5   25   25   24     A     A     A    1    1
    ## 4054 006919 PROTTE  46.8 204.6   25   32   30     A     A     A    1    1
    ## 4055 006960 COU2CU  57.5 209.8   25   27   27     A     A     A    1    1
    ## 4056 006972 COU2CU  42.8 222.4   25   28   28     A     A     A    1    1
    ## 4057 007278 PROTTE  85.3  12.7   25   24   26     A     A     A    1    1
    ## 4058 007315 POSOLA  84.7  27.7   25   25   23     A     A     A    1    1
    ## 4059 007444 LUEHSE  94.7  52.1   25   28   27     A     A     A    1    1
    ## 4060 007556 ALIBED  91.5  81.0   25   26   26     A     A     A    1    1
    ## 4061 007608 FARAOC  85.4 116.0   25   27   26     A     A     A    1    1
    ## 4062 007625 ALIBED  90.6 108.5   25   23   23     A     A     A    1    1
    ## 4063 007666 AST2GR  82.0 137.8   25   26   26     A     A     A    1    1
    ## 4064 007702 CLAVME  90.7 134.0   25   25   25     A     A     A    1    1
    ## 4065 007756 AST2GR  89.9 157.9   25   26   27     A     A     A    1    1
    ## 4066 007757 AST2GR  89.9 157.7   25   25   24     A     A     A    1    1
    ## 4067 007828 INGAVE  81.0 167.3   25   25   29     A     A     A    1    1
    ## 4068 007832 ALBIAD  82.2 170.2   25   14   15     A     A     A    1    1
    ## 4069 007856 AST2GR  87.4 164.6   25   25   25     A     A     A    1    1
    ## 4070 007903 ALBIAD  96.8 162.0   25   27   26     A     A     A    1    1
    ## 4071 007932 CUPASY  86.8 195.9   25   31   28     A     A     A    1    1
    ## 4072 007951 SOROAF  89.8 192.4   25   33   33     A     A     A    1    1
    ## 4073 007960 PROTTE  85.0 187.7   25   38   40     A     A     A    1    1
    ## 4074 008084 FARAOC  88.7 203.8   25   28   30     A     A     A    1    1
    ## 4075 008135 FARAOC  82.1 226.5   25   27   30     A     A     A    1    1
    ## 4076 008210 PROTTE  97.8 226.7   25   28   30     A     A     A    1    1
    ## 4077 008224 CHR2CA  82.6 240.2   25   26   30     A     A     A    1    1
    ## 4078 008229 COU2CU  82.5 245.7   25   32   36     A     A     A    1    1
    ## 4079 008303 HIRTRA  90.0 252.4   25   27   27     A     A     A    1    1
    ## 4080 008325 SWARS1  96.8 253.0   25   26   30     A     A     A    1    1
    ## 4081 008385 FARAOC  89.0 262.6   25   31   37     A     A     A    1    1
    ## 4082 008555 LUEHSE 182.2  21.7   25   -1   -1     A     D     D    1    0
    ## 4083 008643 HIRTRA 139.9  70.2   25   12   12     A     A     A    1    1
    ## 4084 009085 CLAVME  61.0  22.8   25   27   29     A     A     A    1    1
    ## 4085 009353 BROSAL  73.8  98.8   25   28   28     A     A     A    1    1
    ## 4086 009377 EUGEPR  64.0 110.8   25   26   26     A     A     A    1    1
    ## 4087 009540 SOROAF  79.8 147.0   25   25   25     A     A     A    1    1
    ## 4088 009595 BACTMA  70.6 172.0   25   22   22     A     A     A    1    1
    ## 4089 009615 POSOLA  77.2 177.3   25   26   25     A     A     A    1    1
    ## 4090 009643 FARAOC  63.9 196.4   25   29   29     A     A     A    1    1
    ## 4091 009722 PROTTE  77.0 186.7   25   30   32     A     A     A    1    1
    ## 4092 009733 FARAOC  62.2 204.1   25   28   29     A     A     A    1    1
    ## 4093 009748 RINOLI  60.9 215.9   25   26   -1     A     A     D    1    1
    ## 4094 009827 BROSAL  79.9 205.2   25   26   25     A     A     A    1    1
    ## 4095 009876 EUGEPR  68.1 230.2   25   26   26     A     A     A    1    1
    ## 4096 009878 INGAVE  67.1 230.5   25   25    0     A     A     B    1    1
    ## 4097 009894 THEVAH  74.5 228.6   25   -1   -1     A     D     D    1    0
    ## 4098 009971 IXORFL  70.2 252.0   25   28   30     A     A     A    1    1
    ## 4099 010021 MICOAR  66.6 277.0   25   26   28     A     A     A    1    1
    ## 4100 010029 PROTTE  66.9 268.5   25   26   29     A     A     A    1    1
    ## 4101 010063 PIPERE  71.8 278.4   25    0    0     A     B     B    1    2
    ## 4102 010079 FARAOC  75.6 260.6   25   31   33     A     A     A    1    1
    ## 4103 010110 GUARGL  68.5 287.0   25   26   26     A     A     A    1    1
    ## 4104 010116 INGAFA  69.4 284.6   25    0    0     A     B     B    1    2
    ## 4105 010180 PROTTE 131.7  17.5   25   30   33     A     A     A    1    1
    ## 4106 010205 CUPASY 123.5  30.5   25   25   27     A     A     A    1    1
    ## 4107 010233 PROTTE 130.9  22.3   25   28   28     A     A     A    1    1
    ## 4108 010241 PROTTE 134.5  25.9   25   29   31     A     A     A    1    1
    ## 4109 010383 MARGNO 124.4  75.4   25   25   25     A     A     A    1    1
    ## 4110 010406 CLAVME 129.6  61.6   25   31   26     A     A     A    1    1
    ## 4111 010411 ALIBED 133.7  62.4   25   26   29     A     A     A    1    1
    ## 4112 010419 CASECO 133.4  69.1   25   25   26     A     A     A    1    1
    ## 4113 010440 GENIAM 136.5  64.8   25   29   31     A     A     A    1    1
    ## 4114 010450 CASECO 123.0  83.5   25   25   27     A     A     A    1    1
    ## 4115 010503 TRI2PL 131.4  95.2   25   27   28     A     A     A    1    1
    ## 4116 010505 CUPASY 130.9  96.4   25   28   24     A     A     A    1    1
    ## 4117 010881 PROTTE 160.1   8.3   25   27   28     A     A     A    1    1
    ## 4118 011036 GUARGL 168.7  54.0   25   25   28     A     A     A    1    1
    ## 4119 011047 CLAVME 170.3  46.0   25   25   25     A     A     A    1    1
    ## 4120 011169 HEISCO 169.8  80.8   25   30   33     A     A     A    1    1
    ## 4121 011232 BROSAL 195.1  56.6   25   26   27     A     A     A    1    1
    ## 4122 011365 PROTTE 197.7  96.5   25   25   25     A     A     A    1    1
    ## 4123 000049 SLOATE  11.1   8.5   24   31   32     A     A     A    1    1
    ## 4124 000102 EUGEPR  15.8   9.4   24   38   28     A     A     A    1    1
    ## 4125 000178 EUGEPR  13.4  35.9   24   26   28     A     A     A    1    1
    ## 4126 000224 PROTTE   3.2  50.2   24   39   40     A     A     A    1    1
    ## 4127 000264 POUTCA  15.3  56.4   24   25    0     A     A     B    1    1
    ## 4128 000278 COU2CU  18.2  54.2   24   32   32     A     A     A    1    2
    ## 4129 000282 LUEHSE  18.5  54.9   24   25   27     A     A     A    1    1
    ## 4130 000297 AST2GR  15.9  41.8   24   27   28     A     A     A    1    1
    ## 4131 000337 EUGEPR   7.0  67.7   24   28   30     A     A     A    1    1
    ## 4132 000400 FARAOC  18.2  60.3   24   30   33     A     A     A    1    1
    ## 4133 000417 CUPASY   2.4  84.1   24   36   46     A     A     A    1    1
    ## 4134 000425 OURALU   1.1  82.4   24   25   23     A     A     A    1    1
    ## 4135 000474 AST2GR   8.7  92.6   24   27   27     A     A     A    1    1
    ## 4136 000494 FARAOC  13.2  80.9   24   28   29     A     A     A    1    1
    ## 4137 000499 BROSAL  12.0  85.2   24   25   26     A     A     A    1    1
    ## 4138 000518 EUGEPR  12.7  94.1   24   25   25     A     A     A    1    1
    ## 4139 000538 EUGEPR  19.3  98.9   24   25   26     A     A     A    1    1
    ## 4140 000543 EUGEPR  18.3  90.3   24   24   24     A     A     A    1    1
    ## 4141 000588 FARAOC   0.8 110.3   24   35   36     A     A     A    1    1
    ## 4142 000629 COCCPA   5.2 103.9   24   24   25     A     A     A    1    1
    ## 4143 000650 HIRTRA  12.2 109.9   24   24   -1     A     A     D    1    1
    ## 4144 000694 FARAOC   3.3 125.1   24   30   32     A     A     A    1    1
    ## 4145 000741 BROSAL  10.9 130.8   24   -1   -1     A     D     D    1    0
    ## 4146 000793 FARAOC   6.8 149.0   24   29   32     A     A     A    1    1
    ## 4147 000814 FARAOC  11.2 159.9   24   28   29     A     A     A    1    1
    ## 4148 000822 FARAOC  19.9 158.6   24   33   35     A     A     A    1    1
    ## 4149 000831 EUGEPR  16.1 140.7   24   23   25     A     A     A    1    1
    ## 4150 000851 EUGEPR   9.8 178.7   24   26   26     A     A     A    1    1
    ## 4151 000905 SOROAF  18.5 176.4   24   25   26     A     A     A    1    1
    ## 4152 000918 BROSAL  15.7 169.5   24   25   26     A     A     A    1    1
    ## 4153 000948 PROTTE   8.3 195.8   24   24   24     A     A     A    1    1
    ## 4154 001006 SOROAF   4.2 213.5   24   24   24     A     A     A    1    1
    ## 4155 001013 SOROAF   8.2 216.5   24   25   25     A     A     A    1    1
    ## 4156 001030 PROTTE  14.1 200.4   24   25   29     A     A     A    1    1
    ## 4157 001031 SOROAF  10.6 200.8   24   29   31     A     A     A    1    1
    ## 4158 001158 POUTCA  17.1 234.4   24   24   25     A     A     A    1    1
    ## 4159 001205 MACRGL  11.7 256.7   24   26   27     A     A     A    1    1
    ## 4160 001213 COU2CU  17.9 258.8   24   26   28     A     A     A    1    1
    ## 4161 001230 SWARS1  18.9 243.9   24   24   25     A     A     A    1    1
    ## 4162 001282 FARAOC  13.0 268.5   24   26   27     A     A     A    1    1
    ## 4163 001309 COU2CU  19.2 261.3   24   26   26     A     A     A    1    1
    ## 4164 001355 COU2CU  10.1 283.8   24   23   23     A     A     A    1    1
    ## 4165 001425 PROTTE 110.3   2.8   24   26   28     A     A     A    1    1
    ## 4166 001441 EUGEPR 110.1  14.3   24   26   25     A     A     A    1    1
    ## 4167 001459 COU2CU 116.3  19.1   24   24   25     A     A     A    1    1
    ## 4168 001484 EUGEPR 100.3  33.2   24   24   20     A     A     A    1    1
    ## 4169 001593 ALIBED 113.5  53.5   24   23   24     A     A     A    1    1
    ## 4170 001596 BROSAL 117.4  55.3   24   28   30     A     A     A    1    1
    ## 4171 001653 IXORFL 109.7  79.4   24   26   28     A     A     A    1    1
    ## 4172 001661 ALIBED 106.4  65.4   24   39   36     A     A     A    1    1
    ## 4173 001686 TRIPCU 114.6  72.4   24   42   46     A     A     A    1    1
    ## 4174 001742 CLAVME 106.5  97.5   24   25   25     A     A     A    1    1
    ## 4175 001749 FARAOC 105.0  89.1   24   28   31     A     A     A    1    1
    ## 4176 002024 POSOLA 153.3   5.8   24   24   23     A     A     A    1    1
    ## 4177 002053 PIPERE 157.9   6.3   24   25   25     A     A     A    1    1
    ## 4178 002075 RANDFO 140.8  33.6   24   26   30     A     A     A    1    1
    ## 4179 002180 CHR2CA 145.2  56.9   24   25   26     A     A     A    1    1
    ## 4180 002190 INGAFA 153.6  40.3   24   18   18     A     A     A    1    1
    ## 4181 002267 ALIBED 141.7  66.1   24   25   25     A     A     A    1    1
    ## 4182 002333 ALIBED 147.3  98.5   24   24   24     A     A     A    1    1
    ## 4183 002358 CASECO 158.7  87.8   24   24   23     A     A     A    1    1
    ## 4184 002376 BACTMA 153.8  90.5   24   21   22     A     A     A    1    1
    ## 4185 003014 BACTMA  23.5   7.4   24   25   25     A     A     A    1    1
    ## 4186 003019 CLAVME  22.6  10.3   24   25   25     A     A     A    1    1
    ## 4187 003136 CUPASY  26.8  21.1   24   24   28     A     A     A    1    1
    ## 4188 003176 CLAVME  30.1  34.3   24   27   27     A     A     A    1    1
    ## 4189 003210 PIT1RU  23.8  43.7   24    0    0     A     B     B    1    2
    ## 4190 003234 THEVAH  27.1  58.3   24   25   26     A     A     A    1    1
    ## 4191 003310 COU2CU  24.1  75.6   24   27   28     A     A     A    1    1
    ## 4192 003311 COCCPA  22.4  76.6   24   24   28     A     A     A    1    1
    ## 4193 003369 FARAOC  23.7  84.8   24   30   28     A     A     A    1    1
    ## 4194 003377 EUGEPR  21.2  88.8   24   37   39     A     A     A    1    1
    ## 4195 003404 COU2CU  28.7  80.3   24   29   32     A     A     A    1    1
    ## 4196 003564 EUGEPR  26.2 136.9   24   24   25     A     A     A    1    1
    ## 4197 003633 ANNOHA  23.3 148.8   24   24   27     A     A     A    1    1
    ## 4198 003737 ALIBED  28.6 179.2   24   27   27     A     A     A    1    1
    ## 4199 003746 CLAVME  25.6 168.9   24   -1   -1     A     D     D    1    0
    ## 4200 003814 ALIBED  21.8 190.5   24   25   25     A     A     A    1    2
    ## 4201 003824 EUGEPR  22.7 192.1   24   18   18     A     A     A    1    1
    ## 4202 003826 INGAFA  24.7 191.5   24   24   25     A     A     A    1    1
    ## 4203 003853 COU2CU  29.8 184.7   24   26   24     A     A     A    1    1
    ## 4204 003951 SOROAF  22.1 202.5   24   26   24     A     A     A    1    1
    ## 4205 003958 COU2CU  20.4 206.4   24   27   31     A     A     A    1    2
    ## 4206 004091 COU2CU  33.4 220.3   24   27   26     A     A     A    1    2
    ## 4207 004133 COU2CU  23.4 247.8   24   27   27     A     A     A    1    1
    ## 4208 004136 SLOATE  24.0 252.8   24   24   25     A     A     A    1    1
    ## 4209 004171 MACRGL  32.6 254.1   24   26   26     A     A     A    1    1
    ## 4210 004233 BROSAL  29.7 267.6   24   25   21     A     A     A    1    1
    ## 4211 004276 FARAOC  24.0 286.9   24   25   26     A     A     A    1    1
    ## 4212 004296 COU2CU  26.2 293.4   24   26   27     A     A     A    1    1
    ## 4213 006024 EUGEPR  47.0  20.5   24   27   29     A     A     A    1    1
    ## 4214 006034 THEVAH  49.2  10.2   24   27   28     A     A     A    1    1
    ## 4215 006069 PROTTE  51.5  15.2   24   30   33     A     A     A    1    1
    ## 4216 006078 PIPERE  55.6  13.2   24    0    0     A     B     B    1    2
    ## 4217 006096 COU2CU  56.1   4.8   24   27   30     A     A     A    1    1
    ## 4218 006148 TAB1RO  54.3  20.1   24   29   31     A     A     A    1    1
    ## 4219 006221 CUPASY  48.6  49.7   24   24   24     A     A     A    1    1
    ## 4220 006228 COU2CU  54.7  45.8   24   29   33     A     A     A    1    1
    ## 4221 006230 COU2CU  53.4  49.8   24   24   28     A     A     A    1    1
    ## 4222 006266 ANNOHA  55.6  41.9   24   29   34     A     A     A    1    1
    ## 4223 006289 BROSAL  43.5  73.2   24   22   22     A     A     A    1    1
    ## 4224 006303 PROTTE  45.9  79.2   24   27   26     A     A     A    1    1
    ## 4225 006357 BROSAL  44.5 201.0   24   24   26     A     A     A    1    1
    ## 4226 006516 FARAOC  49.2 125.1   24   39   42     A     A     A    1    1
    ## 4227 006544 ANNOHA  53.1 129.9   24   29   31     A     A     A    1    1
    ## 4228 006555 COU2CU  50.5 132.8   24   23   23     A     A     A    1    2
    ## 4229 006646 ALIBED  56.4 143.9   24   27   28     A     A     A    1    1
    ## 4230 006829 FARAOC  54.0 185.1   24   26   28     A     A     A    1    1
    ## 4231 006969 COU2CU  40.8 221.5   24   24   24     A     A     A    1    2
    ## 4232 007012 COU2CU  48.5 222.1   24   24   24     A     A     A    1    1
    ## 4233 007057 COCCPA  43.1 243.2   24   25   27     A     A     A    1    1
    ## 4234 007127 FARAOC  47.4 279.9   24   24   25     A     A     A    1    1
    ## 4235 007135 FARAOC  45.6 269.5   24   25   25     A     A     A    1    1
    ## 4236 007244 COU2CU  56.8 295.1   24   24   26     A     A     A    1    1
    ## 4237 007287 ALBIAD  91.8   9.7   24   28   27     A     A     A    1    1
    ## 4238 007395 GUARGL  80.2  47.2   24   26   28     A     A     A    1    1
    ## 4239 007401 NEEADE  82.6  55.9   24   26   27     A     A     A    1    1
    ## 4240 007508 ANNOHA  94.3  79.4   24   25   25     A     A     A    1    1
    ## 4241 007633 HEISCO  95.7 111.9   24   30   35     A     A     A    1    1
    ## 4242 007831 NEEADE  84.2 170.1   24   11   14     A     A     A    1    1
    ## 4243 007910 GUARGL  80.1 186.6   24   16   20     A     A     A    1    1
    ## 4244 007930 POUTCA  88.6 196.5   24   28   29     A     A     A    1    1
    ## 4245 007983 HIRTRA  93.8 188.4   24   24   23     A     A     A    1    2
    ## 4246 008068 HEISCO  89.0 216.2   24   27   31     A     A     A    1    1
    ## 4247 008112 PROTTE  95.5 210.1   24   26   28     A     A     A    1    1
    ## 4248 008123 HIRTRA  96.3 202.8   24   27   30     A     A     A    1    1
    ## 4249 008136 EUGEPR  84.9 230.6   24   25   27     A     A     A    1    1
    ## 4250 008260 SOROAF  87.4 258.0   24   30   31     A     A     A    1    1
    ## 4251 008274 PALIGU  85.7 247.6   24   -1   -1     A     D     D    1    0
    ## 4252 008278 COU2CU  87.9 247.0   24   25   27     A     A     A    1    1
    ## 4253 008306 ALIBED  94.3 253.7   24   24   25     A     A     A    1    1
    ## 4254 008309 SOROAF  90.0 255.4   24   33   35     A     A     A    1    1
    ## 4255 008401 THEVAH  94.8 276.7   24   24   25     A     A     A    1    1
    ## 4256 008552 ERY2PA 195.5   2.0   24   25   27     A     A     A    1    1
    ## 4257 009035 EUGECO  65.6  14.8   24   28   34     A     A     A    1    1
    ## 4258 009176 AST2GR  66.3  58.1   24   24   25     A     A     A    1    2
    ## 4259 009235 TRIPCU  78.2  40.8   24   24   25     A     A     A    1    1
    ## 4260 009252 EUGEPR  63.4  69.3   24   22   22     A     A     A    1    1
    ## 4261 009270 CUPASY  65.6  73.0   24   25   23     A     A     A    1    1
    ## 4262 009343 PROTTE  73.8  88.2   24   25   27     A     A     A    1    1
    ## 4263 009350 PROTTE  73.8  95.2   24   27   29     A     A     A    1    1
    ## 4264 009476 EUGEPR  76.0 124.8   24   26   28     A     A     A    1    1
    ## 4265 009500 AST2GR  67.9 146.1   24   27   27     A     A     A    1    1
    ## 4266 009542 NECTMA  75.5 140.2   24   37   39     A     A     A    1    1
    ## 4267 009617 SWARS1  77.3 179.7   24   29   30     A     A     A    1    1
    ## 4268 009632 COU2CU  63.7 181.7   24   24   24     A     A     A    1    1
    ## 4269 009737 BROSAL  64.0 204.1   24   26   26     A     A     A    1    1
    ## 4270 009753 HEISCO  63.7 219.8   24   29   30     A     A     A    1    1
    ## 4271 009770 EUGEPR  66.0 208.9   24   27   28     A     A     A    1    1
    ## 4272 009783 POSOLA  70.3 202.1   24   25   26     A     A     A    1    1
    ## 4273 009805 EUGEPR  74.5 211.5   24   24   20     A     A     A    1    1
    ## 4274 009822 GUARGL  79.1 210.2   24   25   25     A     A     A    1    1
    ## 4275 009849 EUGEPR  78.8 202.5   24   25   25     A     A     A    1    1
    ## 4276 009930 SOROAF  78.5 228.0   24   28   28     A     A     A    1    1
    ## 4277 009962 BACTMA  66.1 249.8   24    0   -1     A     B     D    1    2
    ## 4278 010042 TRIPCU  74.9 263.1   24   26   28     A     A     A    1    1
    ## 4279 010066 EUGEPR  74.7 278.7   24   26   26     A     A     A    1    1
    ## 4280 010112 PROTTE  66.2 281.6   24   27   28     A     A     A    1    1
    ## 4281 010161 EUGEPR 127.4  14.0   24   24   25     A     A     A    1    1
    ## 4282 010208 ANNOHA 121.1  34.9   24   25   27     A     A     A    1    1
    ## 4283 010213 GUARGL 123.7  37.2   24   24   24     A     A     A    1    1
    ## 4284 010224 GUARGL 126.2  28.6   24   26   26     A     A     A    1    1
    ## 4285 010257 PIT1RU 136.0  39.5   24    0    0     A     B     B    1    2
    ## 4286 010264 GUARGL 139.2  25.1   24   -9    0     A     *     B    1    0
    ## 4287 010345 INGAVE 139.8  57.2   24   25   25     A     A     A    1    1
    ## 4288 010356 FARAOC 138.8  48.7   24   24   28     A     A     A    1    2
    ## 4289 010452 HIRTRA 124.7  82.5   24   25   26     A     A     A    1    1
    ## 4290 010516 IXORFL 135.8  89.5   24   26   29     A     A     A    1    1
    ## 4291 010962 RINOLI 160.8  32.6   24   -1   -1     A     D     D    1    0
    ## 4292 010991 BROSAL 170.3  28.9   24   24   25     A     A     A    1    1
    ## 4293 010992 AST2GR 173.0  27.6   24   25   27     A     A     A    1    1
    ## 4294 011054 HIRTRA 174.3  50.9   24   27   27     A     A     A    1    1
    ## 4295 011071 BROSAL 175.5  40.0   24   24   24     A     A     A    1    1
    ## 4296 011086 AST2GR 162.2  74.8   24   24   22     A     A     A    1    1
    ## 4297 011091 PROTTE 162.9  72.4   24   24   25     A     A     A    1    1
    ## 4298 011239 PICRLA 199.0  54.7   24   28   29     A     A     A    1    1
    ## 4299 011390 PROTTE  63.8 279.0   24   10   10     A     A     A    1    1
    ## 4300 011402 ALIBED 114.5  41.7   24    0   -1     A     B     D    1    2
    ## 4301 000072 PICRLA  12.2  15.6   23   32   37     A     A     A    1    1
    ## 4302 000123 COU2CU   1.7  24.5   23   33   35     A     A     A    1    1
    ## 4303 000127 CUPASY   1.9  22.8   23   24   26     A     A     A    1    1
    ## 4304 000131 SLOATE   2.9  31.1   23   26   26     A     A     A    1    1
    ## 4305 000185 EUGEPR  13.1  37.1   23   27   30     A     A     A    1    1
    ## 4306 000242 SPONRA  12.7  39.9   23   28   30     A     A     A    1    1
    ## 4307 000268 SWARS1  15.9  58.6   23   25   25     A     A     A    1    1
    ## 4308 000330 HEISCO   6.2  65.4   23   28   32     A     A     A    1    1
    ## 4309 000340 STERAP   6.9  61.6   23   27   30     A     A     A    1    1
    ## 4310 000352 SWARS1   9.1  62.5   23   32   35     A     A     A    1    1
    ## 4311 000354 CLAVME   9.9  61.6   23   29   31     A     A     A    1    1
    ## 4312 000370 COU2CU  11.2  68.9   23   31   33     A     A     A    1    1
    ## 4313 000398 EUGEPR  18.0  67.5   23   24   22     A     A     A    1    1
    ## 4314 000478 SOROAF   5.4  85.8   23   23   22     A     A     A    1    1
    ## 4315 000500 COU2CU  10.2  86.2   23   -1   -1     A     D     D    1    0
    ## 4316 000544 FARAOC  17.6  90.7   23   27   27     A     A     A    1    1
    ## 4317 000590 OURALU   2.9 114.2   23   22   22     A     A     A    1    1
    ## 4318 000619 EUGEPR   9.5 112.2   23   25   26     A     A     A    1    1
    ## 4319 000653 ALIBED  11.6 111.1   23   23   23     A     A     A    1    1
    ## 4320 000666 INGAFA  15.4 113.0   23   21   21     A     A     A    1    1
    ## 4321 000686 ACALDI   0.4 122.5   23   -1   -1     A     D     D    1    0
    ## 4322 000916 PIT1RU  16.9 168.3   23   22   -1     A     A     D    1    2
    ## 4323 000951 ACALDI  16.7  17.7   23   38   40     A     A     A    1    1
    ## 4324 000980 AST2GR  16.0 197.5   23   23   23     A     A     A    1    1
    ## 4325 001043 SOROAF  11.6 207.5   23   27   27     A     A     A    1    1
    ## 4326 001083 FARAOC   1.2 220.9   23   29   32     A     A     A    1    1
    ## 4327 001244 COU2CU   1.0 266.3   23   27   31     A     A     A    1    1
    ## 4328 001322 POSOLA   2.2 288.8   23   26   26     A     A     A    1    1
    ## 4329 001370 SOROAF  16.9 295.2   23   28   31     A     A     A    1    1
    ## 4330 001390 FARAOC  16.6 280.9   23   23   24     A     A     A    1    1
    ## 4331 001458 EUGEPR 118.7  15.2   23   26   25     A     A     A    1    2
    ## 4332 001618 ALBIAD 116.3  54.9   23   24    0     A     A     B    1    1
    ## 4333 001691 NEEADE 112.3  75.8   23   30   30     A     A     A    1    1
    ## 4334 002109 RINOLI 153.3  36.0   23    0    0     A     B     B    1    2
    ## 4335 002132 RINOLI 155.3  32.0   23   23   26     A     A     A    1    1
    ## 4336 002195 BROSAL 153.4  53.3   23   25   26     A     A     A    1    1
    ## 4337 002222 RINOLI 155.4  41.1   23   22   22     A     A     A    1    1
    ## 4338 002291 BROSAL 150.2  73.0   23   24   24     A     A     A    1    1
    ## 4339 002325 BACTMA 151.3  96.7   23   23   23     A     A     A    1    1
    ## 4340 002359 HIRTRA 158.9  82.1   23   23   22     A     A     A    1    1
    ## 4341 002367 BACTMA 141.6  93.3   23   25   -1     A     A     D    1    1
    ## 4342 003022 INGAVE  23.4  13.0   23   25   25     A     A     A    1    1
    ## 4343 003069 FARAOC  34.0  17.4   23   31   34     A     A     A    1    1
    ## 4344 003099 EUGEPR  20.5  25.8   23    0    0     A     B     B    1    2
    ## 4345 003138 ALIBED  26.0  21.6   23   24   25     A     A     A    1    1
    ## 4346 003148 CLAVME  30.7  20.1   23   24   25     A     A     A    1    1
    ## 4347 003156 CHR2CA  33.1  23.6   23   23   24     A     A     A    1    1
    ##      pom3 code1 code2 code3 mult1 mult2 mult3      date1      date2
    ## 1       1     B     B     B     1     1     1 11/23/1994 11/18/1997
    ## 2       1     B     B     B     1     1     1 11/08/1994 11/12/1997
    ## 3       1     B     B     B     1     1     1 11/18/1994 11/12/1997
    ## 4       1     B     B     B     1     1     1 11/07/1994 11/14/1997
    ## 5       2     B     B     B     1     1     1 11/02/1994 11/11/1997
    ## 6       1     B     B     B     1     1     1 11/08/1994 11/17/1997
    ## 7       1     B     B     B     1     1     1 11/07/1994 11/12/1997
    ## 8       1     B     B     B     1     1     1 11/18/1994 11/27/1997
    ## 9       1     B     B     B     1     1     1 11/22/1994 11/13/1997
    ## 10      1     B     B     B     1     1     1 11/02/1994 11/11/1997
    ## 11      1     B     B     B     1     1     1 11/15/1994 11/21/1997
    ## 12      1     B     B     B     1     1     1 11/09/1994 12/17/1997
    ## 13      2     B     B     B     1     1     1 11/25/1994 11/25/1997
    ## 14      1     B     B     B     1     1     1 12/01/1994 11/21/1997
    ## 15      1     B     B     B     1     1     1 12/02/1994 11/21/1997
    ## 16      1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 17      1     B     B     B     1     1     1 11/14/1994 11/21/1997
    ## 18      2     B     B     B     1     1     1 11/15/1994 11/20/1997
    ## 19      1     B     B     B     1     1     1 12/02/1994 11/24/1997
    ## 20      1     B     B     B     1     1     1 11/08/1994 11/20/1997
    ## 21      1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 22      0     *     *     B     1     1    -9 12/06/1994 11/26/1997
    ## 23      1     B     B     B     1     1     1 12/01/1994 11/21/1997
    ## 24      2     *     *     B     1     1     1 12/01/1994 11/21/1997
    ## 25      1     B     B     B     1     1     1 12/01/1994 11/21/1997
    ## 26      0     *     *     B     1     1    -9 12/06/1994 11/26/1997
    ## 27      1     B     B     B     1     1     1 11/15/1994 11/26/1997
    ## 28      1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 29      0     *    DS    DS     1    -1    -1 11/14/1994 11/17/1997
    ## 30      1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 31      2     *     B     B     1    -9     1 11/25/1994 11/25/1997
    ## 32      1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 33      1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 34      1     B     B     B     1     1     1 11/08/1994 11/14/1997
    ## 35      1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 36      1     B    BQ     B     1     1     1 11/09/1994 11/14/1997
    ## 37      1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 38      1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 39      1     B     B     B     1     1     1 11/02/1994 11/26/1997
    ## 40      1     B     B     B     1     1     1 12/01/1994 11/18/1997
    ## 41      1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 42      1     B     B     B     1     1     1 11/09/1994 11/13/1997
    ## 43      1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 44      1     *     B     *     1    -9     1 11/07/1994 11/13/1997
    ## 45      1     B     B     B     1     1     1 11/04/1994 11/12/1997
    ## 46      1     B     B     B     1     1     1 11/08/1994 11/14/1997
    ## 47      1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 48      1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 49      1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 50      1     B     B     B     1     1     1 11/14/1994 11/21/1997
    ## 51      1     B     B     B     1     1     1 11/08/1994 11/13/1997
    ## 52      1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 53      1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 54      2     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 55      1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 56      1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 57      1     B     B     B     1     1     1 12/02/1994 11/25/1997
    ## 58      1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 59      1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 60      2     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 61      2     B     B     B     2     2     1 12/02/1994 11/21/1997
    ## 62      1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 63      1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 64      1     B     B     B     1     1     1 11/09/1994 12/17/1997
    ## 65      1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 66      1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 67      2     *     B     B     1     1     1 11/16/1994 11/24/1997
    ## 68      1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 69      1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 70      1     B     B     B     1     1     1 11/11/1994 11/18/1997
    ## 71      1     B     B     B     1     1     1 11/02/1994 11/26/1997
    ## 72      2     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 73      2     B     B     B     1     1     1 12/02/1994 11/25/1997
    ## 74      1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 75      1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 76      2     *     B     B     1    -9     1 11/07/1994 11/14/1997
    ## 77      2     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 78      1     Q     *     *     1     1     1 11/14/1994 11/26/1997
    ## 79      1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 80      2     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 81      1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 82      1     B     B     B     1     1     1 11/23/1994 11/18/1997
    ## 83      1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 84      1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 85      1     B     B    BM     1     3     4 11/18/1994 11/27/1997
    ## 86      2     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 87      1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 88      1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 89      1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 90      2     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 91      1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 92      2     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 93      2     *    BL     L     1    -9     1 11/07/1994 11/11/1997
    ## 94      1     B     B     B     1     1     1 11/15/1994 11/21/1997
    ## 95      1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 96      1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 97      1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 98      1     B     B     B     1     1     1 11/14/1994 12/17/1997
    ## 99      1     M     M     M     3     2     2 11/24/1994 11/14/1997
    ## 100     1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 101     1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 102     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 103     1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 104     1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 105     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 106     1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 107     1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 108     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 109     1     B     B     B     1     1     1 11/21/1994 11/27/1997
    ## 110     1     *     B     *     1    -9     1 11/07/1994 11/13/1997
    ## 111     1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 112     1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 113     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 114     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 115     1     B     B     B     1     1     1 11/14/1994 11/21/1997
    ## 116     1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 117     0     L    DC    DC     1    -1    -1 11/16/1994 11/18/1997
    ## 118     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 119     1     B     B     B     1     1     1 11/23/1994 11/14/1997
    ## 120     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 121     1     B     B     B     1     1     1 12/05/1994 11/25/1997
    ## 122     1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 123     1     M     M     M     2     2     2 11/04/1994 11/12/1997
    ## 124     1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 125     3     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 126     1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 127     0     *     *     B     1     1    -9 11/14/1994 11/17/1997
    ## 128     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 129     1     B     B    BM     1     7     7 11/14/1994 11/20/1997
    ## 130     2     *     B     B     1     1     1 11/15/1994 11/21/1997
    ## 131     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 132     1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 133     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 134     2     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 135     1     M     M     M     2     2     2 11/22/1994 11/20/1997
    ## 136     1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 137     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 138     1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 139     3     *     *     B     1     1     1 11/23/1994 11/18/1997
    ## 140     1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 141     1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 142     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 143     1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 144     1     M     M     M     2     2     2 12/06/1994 11/26/1997
    ## 145     1     B     B     B     1     1     1 11/07/1994 11/13/1997
    ## 146     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 147     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 148     1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 149     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 150     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 151     1     M     M     M     4    -9     4 11/08/1994 11/13/1997
    ## 152     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 153     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 154     1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 155     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 156     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 157     1     M     *     *     3     1     1 11/02/1994 11/11/1997
    ## 158     2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 159     1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 160     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 161     1     M     *     M     2     1     2 11/07/1994 11/19/1997
    ## 162     1     Q     Q     *     1     1     1 11/07/1994 11/19/1997
    ## 163     1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 164     2     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 165     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 166     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 167     1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 168     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 169     1     *     B     B     1     1     1 11/16/1994 11/21/1997
    ## 170     0     *    DC    DC     1    -1    -1 12/01/1994 11/20/1997
    ## 171     1     M     *     M     2     1     2 11/14/1994 11/26/1997
    ## 172     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 173     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 174     2     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 175     1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 176     1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 177     2     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 178     1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 179     1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 180     1     M     M     M     4     4     3 11/09/1994 11/17/1997
    ## 181     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 182     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 183     0     *    DC    DC     1    -1    -1 12/02/1994 11/24/1997
    ## 184     1     B     B     B     1     1     1 11/14/1994 11/19/1997
    ## 185     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 186     2     *     *     B     1     1     1 11/08/1994 11/12/1997
    ## 187     1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 188     1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 189     1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 190     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 191     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 192     1     B     B     B     1     1     1 11/08/1994 11/12/1997
    ## 193     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 194     1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 195     1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 196     1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 197     2     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 198     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 199     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 200     1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 201     1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 202     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 203     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 204     1     L     L     L     1     1     1 12/02/1994 11/24/1997
    ## 205     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 206     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 207     2     *     *     *     2     1     1 11/15/1994 11/21/1997
    ## 208     1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 209     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 210     1     M     M     M     3     3     3 11/16/1994 11/19/1997
    ## 211     0     *     *    DS     1     1    -1 12/05/1994 11/26/1997
    ## 212     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 213     0     *     L    DC     1     1    -1 12/01/1994 11/20/1997
    ## 214     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 215     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 216     0     *     *    DS     1     1    -1 11/11/1994 11/25/1997
    ## 217     1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 218     1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 219     1     B     B     B     1     1     1 12/01/1994 11/20/1997
    ## 220     0     *     *    DS     1     1    -1 11/07/1994 11/11/1997
    ## 221     2     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 222     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 223     1     Q     *     *     1     1     1 11/14/1994 11/26/1997
    ## 224     1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 225     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 226     2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 227     1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 228     3     *     *     B     1     1     1 11/09/1994 11/13/1997
    ## 229     2     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 230     2     L     L     L     1     1     1 11/30/1994 11/20/1997
    ## 231     1     L     L     L     1     1     1 12/02/1994 11/24/1997
    ## 232     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 233     1     L     L     L     1     1     1 11/09/1994 11/17/1997
    ## 234     2     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 235     1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 236     1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 237     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 238     2     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 239     1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 240     1     M     M     M     4     4     3 11/23/1994 11/19/1997
    ## 241     1     *     L     L     1     1     1 11/17/1994 11/26/1997
    ## 242     1     L     *     *     1     1     1 11/09/1994 11/21/1997
    ## 243     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 244     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 245     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 246     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 247     1     M     M     M     3     3     3 11/07/1994 11/19/1997
    ## 248     1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 249     1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 250     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 251     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 252     1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 253     1     L     *     L     1     1     1 11/09/1994 11/21/1997
    ## 254     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 255     0     *    DS    DC     1    -1    -1 11/25/1994 11/24/1997
    ## 256     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 257     1     L     *     L     1     1     1 11/07/1994 11/20/1997
    ## 258     1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 259     2     M     M     M     2     2     2 11/16/1994 11/18/1997
    ## 260     1     *     *     Q     1     1     1 11/07/1994 11/12/1997
    ## 261     1     L     L     *     1     1     1 11/17/1994 11/11/1997
    ## 262     1     M     M     M     2     2     2 11/09/1994 11/14/1997
    ## 263     1     M     M     M     2     2     2 11/25/1994 11/25/1997
    ## 264     1     *     M     M     2     2     2 11/02/1994 11/26/1997
    ## 265     1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 266     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 267     1     L     *     *     1    -9     1 11/08/1994 11/14/1997
    ## 268     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 269     0     *    DC    DC     1    -1    -1 11/23/1994 11/18/1997
    ## 270     1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 271     2     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 272     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 273     1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 274     1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 275     2     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 276     1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 277     1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 278     1     *     *     L     1     1     1 12/05/1994 11/26/1997
    ## 279     1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 280     2     *     B     L     1    -9     1 11/07/1994 11/14/1997
    ## 281     1     M     M     M     2     2     2 11/16/1994 11/21/1997
    ## 282     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 283     1     L     L     L     1     1     1 11/11/1994 11/17/1997
    ## 284     0     *    DS    DC     1    -1    -1 12/01/1994 11/19/1997
    ## 285     1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 286     0     *    DC    DC     1    -1    -1 11/17/1994 11/26/1997
    ## 287     1     L     *     *     1     1     1 11/23/1994 11/13/1997
    ## 288     1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 289     1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 290     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 291     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 292     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 293     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 294     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 295     1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 296     1     M     L     L     1     1     1 12/02/1994 11/24/1997
    ## 297     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 298     1     L     L     L     1     1     1 11/15/1994 11/27/1997
    ## 299     2     L     L     *     1     1     1 11/23/1994 11/13/1997
    ## 300     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 301     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 302     1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 303     2     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 304     1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 305     1     M     M     M     4     3     3 11/17/1994 11/12/1997
    ## 306     2     L     *     *     1     1     1 11/08/1994 11/20/1997
    ## 307     1     L     L     L     1     1     1 11/09/1994 11/17/1997
    ## 308     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 309     1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 310     1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 311     1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 312     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 313     0     M    DC    DC     2    -1    -1 11/15/1994 11/21/1997
    ## 314     1     L     L     L     1     1     1 11/22/1994 11/13/1997
    ## 315     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 316     0     *     *    DC     1     1    -1 12/01/1994 11/20/1997
    ## 317     1     M     *     M     5     1     3 12/01/1994 11/21/1997
    ## 318     0     *     Q    DS     1     1    -1 12/01/1994 11/24/1997
    ## 319     2     *     B    BM     1     2     2 11/16/1994 11/18/1997
    ## 320     3     L     *     *     1     1     1 11/30/1994 11/21/1997
    ## 321     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 322     2     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 323     1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 324     2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 325     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 326     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 327     1     *     *     Q     1     1     1 12/02/1994 11/25/1997
    ## 328     1     B     B     B     1     1     1 12/02/1994 11/24/1997
    ## 329     2     M     M     M     2     2     2 12/05/1994 11/25/1997
    ## 330     1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 331     1     *     M     M     2     2     2 12/02/1994 11/25/1997
    ## 332     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 333     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 334     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 335     1     L     L     L     1     1     1 11/09/1994 11/14/1997
    ## 336     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 337     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 338     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 339     1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 340     1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 341     2     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 342     2     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 343     1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 344     2     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 345     1     M     *     *     2     1     1 11/07/1994 11/19/1997
    ## 346     1     M     *     *     2     1     1 12/02/1994 11/24/1997
    ## 347     1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 348     1     L     *     L     1     1     1 11/11/1994 11/24/1997
    ## 349     0     *    DC    DN     1    -1    -1 11/17/1994 11/12/1997
    ## 350     0     *    DC    DC     1    -1    -1 11/24/1994 11/19/1997
    ## 351     1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 352     1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 353     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 354     0     *    DS    DS     1    -1    -1 11/07/1994 11/11/1997
    ## 355     0     *    DC    DC     1    -1    -1 11/08/1994 11/12/1997
    ## 356     1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 357     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 358     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 359     1     M     M     *     2     2     1 11/08/1994 11/14/1997
    ## 360     2     *     L     L     1     1     1 11/14/1994 11/20/1997
    ## 361     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 362     2     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 363     1     M     M     M     4     4     4 12/02/1994 11/25/1997
    ## 364     2     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 365     1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 366     1     M     *     *     3     1     1 11/16/1994 11/27/1997
    ## 367     2     M     M     M     2     2     2 11/07/1994 11/14/1997
    ## 368     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 369     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 370     0     *     *    DS     1     1    -1 12/01/1994 11/21/1997
    ## 371     1     *     M     M     1     3     4 11/10/1994 11/14/1997
    ## 372     1     L     L     *     1     1     1 11/22/1994 11/17/1997
    ## 373     3     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 374     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 375     1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 376     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 377     1     M     M     M     2     2     2 11/04/1994 11/12/1997
    ## 378     0     M    DC    DC     2    -1    -1 11/04/1994 11/12/1997
    ## 379     1     M     *     M     2     1     2 11/15/1994 11/27/1997
    ## 380     0     *     *    DS     1     1    -1 11/11/1994 11/18/1997
    ## 381     1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 382     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 383     1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 384     1     M     M     M     2     2     2 11/17/1994 11/12/1997
    ## 385     1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 386     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 387     1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 388     1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 389     0     *     *    DC     1     1    -1 11/07/1994 11/11/1997
    ## 390     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 391     1     *     *    LQ     1     1     1 11/07/1994 11/11/1997
    ## 392     0     M     *    DS     2     1    -1 11/23/1994 11/18/1997
    ## 393     0     *     *    DC     1     1    -1 11/14/1994 11/21/1997
    ## 394     1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 395     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 396     3     *     M    BM     1     2     2 12/01/1994 11/19/1997
    ## 397     1     *     *     Q     1     1     1 11/09/1994 11/17/1997
    ## 398     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 399     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 400     1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 401     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 402     1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 403     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 404     2     L     L     *     1     1     1 11/22/1994 11/13/1997
    ## 405     0     L    DC    DC     1    -1    -1 12/02/1994 11/21/1997
    ## 406     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 407     2     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 408     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 409     2     L     L     L     1     1     1 12/02/1994 11/24/1997
    ## 410     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 411     1     *     *     L     1     1     1 11/07/1994 11/11/1997
    ## 412     1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 413     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 414     3     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 415     1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 416     2     *     *     L     1     1     1 12/01/1994 11/24/1997
    ## 417     2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 418     0     L    DC    DC     1    -1    -1 11/24/1994 11/17/1997
    ## 419     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 420     1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 421     1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 422     1     *     *     Q     1     1     1 11/07/1994 11/11/1997
    ## 423     2     L     L     L     1     1     1 11/15/1994 11/27/1997
    ## 424     2     L     *     L     1     1     1 11/23/1994 11/19/1997
    ## 425     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 426     1    MQ     M    MQ     3     3     2 11/23/1994 11/14/1997
    ## 427     2     L     *     *     1     1     1 12/02/1994 11/24/1997
    ## 428     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 429     0     *    DS    DC     1    -1    -1 11/23/1994 11/18/1997
    ## 430     1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 431     3     M     *     *     2     1     1 11/11/1994 11/18/1997
    ## 432     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 433     0     L    DC    DT     1    -1    -1 11/17/1994 11/26/1997
    ## 434     1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 435     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 436     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 437     1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 438     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 439     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 440     1     M     M     M     3     4     4 12/05/1994 11/26/1997
    ## 441     1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 442     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 443     1     *     Q     *     2     1     1 11/22/1994 11/17/1997
    ## 444     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 445     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 446     0     *    DS    DC     1    -1    -1 11/18/1994 11/12/1997
    ## 447     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 448     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 449     2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 450     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 451     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 452     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 453     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 454     1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 455     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 456     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 457     0     *    DC    DC     1    -1    -1 11/08/1994 11/17/1997
    ## 458     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 459     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 460     1     *     *     L     1     1     1 11/07/1994 11/20/1997
    ## 461     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 462     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 463     1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 464     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 465     2     M     M     M     3     2     2 11/22/1994 11/17/1997
    ## 466     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 467     1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 468     1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 469     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 470     1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 471     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 472     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 473     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 474     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 475     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 476     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 477     0     *    DC    DC     1    -1    -1 11/11/1994 11/18/1997
    ## 478     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 479     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 480     1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 481     1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 482     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 483     3     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 484     1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 485     0     *    DC    DN     1    -1    -1 11/21/1994 11/28/1997
    ## 486     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 487     2     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 488     1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 489     1     M     M     M     5     4     5 11/21/1994 11/27/1997
    ## 490     1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 491     1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 492     1    MQ    MQ     M     2     2     2 11/15/1994 11/27/1997
    ## 493     1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 494     1     M     *     M     2     1     2 11/14/1994 11/17/1997
    ## 495     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 496     0     *    DS    DC     1    -1    -1 11/09/1994 11/14/1997
    ## 497     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 498     1     M     M     M     2     2     2 11/17/1994 11/11/1997
    ## 499     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 500     1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 501     1     L     L     L     1     1     1 11/09/1994 11/14/1997
    ## 502     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 503     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 504     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 505     0     *     B    DS     1    -9    -1 11/18/1994 11/27/1997
    ## 506     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 507     1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 508     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 509     1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 510     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 511     1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 512     1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 513     1     M    MQ     M     2     2     3 11/02/1994 11/11/1997
    ## 514     1     *     *     Q     1     1     1 11/14/1994 11/19/1997
    ## 515     0     *     Q    DS     1     1    -1 11/22/1994 11/17/1997
    ## 516     1     Q     *     Q     1     1     1 12/02/1994 11/24/1997
    ## 517     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 518     1     *     *     *     2     1     1 11/23/1994 11/18/1997
    ## 519     0     Q    DC    DN     1    -1    -1 11/17/1994 11/26/1997
    ## 520     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 521     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 522     1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 523     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 524     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 525     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 526     1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 527     0     *    DC    DC     1    -1    -1 11/18/1994 11/26/1997
    ## 528     1     M     *     M     3     1     3 11/29/1994 11/18/1997
    ## 529     0     *    DS    DC     1    -1    -1 12/02/1994 11/21/1997
    ## 530     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 531     2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 532     1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 533     0     *    DC    DN     1    -1    -1 11/18/1994 11/27/1997
    ## 534     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 535     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 536     2     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 537     1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 538     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 539     1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 540     1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 541     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 542     0     *     *    DS     1     1    -1 11/11/1994 11/18/1997
    ## 543     2     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 544     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 545     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 546     1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 547     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 548     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 549     1     B     B     B     1     1     1 11/22/1994 11/17/1997
    ## 550     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 551     1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 552     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 553     1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 554     1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 555     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 556     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 557     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 558     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 559     1     *     *     *     2     1     1 12/01/1994 11/18/1997
    ## 560     1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 561     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 562     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 563     1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 564     1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 565     2     M     M     M     2     2     2 11/14/1994 11/19/1997
    ## 566     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 567     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 568     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 569     2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 570     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 571     0     *    DS    DS     1    -1    -1 11/17/1994 11/11/1997
    ## 572     0     *    DS    DS     1    -1    -1 11/24/1994 11/17/1997
    ## 573     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 574     1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 575     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 576     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 577     0     *    DS    DS     1    -1    -1 11/08/1994 11/13/1997
    ## 578     2     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 579     0     *    DS    DS     1    -1    -1 12/02/1994 11/25/1997
    ## 580     1     *     *     Q     1     1     1 11/17/1994 11/12/1997
    ## 581     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 582     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 583     1     L     L     L     2     1     1 11/09/1994 11/13/1997
    ## 584     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 585     1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 586     1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 587     2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 588     1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 589     1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 590     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 591     1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 592     1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 593     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 594     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 595     2     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 596     2     M     Q     *     2     1     1 11/04/1994 11/12/1997
    ## 597     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 598     1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 599     1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 600     1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 601     1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 602     1     *     *     L     1     1     1 12/02/1994 11/24/1997
    ## 603     1     *     Q     *     1     1     1 11/18/1994 11/27/1997
    ## 604     1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 605     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 606     1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 607     1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 608     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 609     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 610     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 611     1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 612     1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 613     1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 614     0    ML   MLQ    DC     2     2    -1 11/17/1994 11/12/1997
    ## 615     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 616     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 617     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 618     2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 619     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 620     2     *     *     *     3     1     1 12/02/1994 11/19/1997
    ## 621     1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 622     1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 623     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 624     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 625     2     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 626     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 627     1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 628     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 629     1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 630     0     *     *    DC     1     1    -1 11/07/1994 11/11/1997
    ## 631     1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 632     1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 633     1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 634     1     *     *     L     1     1     1 11/15/1994 11/26/1997
    ## 635     1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 636     2     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 637     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 638     1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 639     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 640     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 641     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 642     1     Q     *     *     1     1     1 11/14/1994 11/17/1997
    ## 643     1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 644     1     *     *     Q     1     1     1 11/07/1994 11/11/1997
    ## 645     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 646     1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 647     1     B     B     B     1     1     1 11/09/1994 11/13/1997
    ## 648     1     *     *     *     2     1     1 11/04/1994 11/12/1997
    ## 649     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 650     1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 651     1     Q     *     *     1     1     1 11/24/1994 11/17/1997
    ## 652     1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 653     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 654     2     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 655     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 656     1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 657     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 658     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 659     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 660     2     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 661     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 662     1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 663     2     *     *     M     1     1     2 11/22/1994 11/13/1997
    ## 664     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 665     0     *     *    DS     1     1    -1 11/07/1994 11/11/1997
    ## 666     1     M     M     *     2     2     1 11/17/1994 11/11/1997
    ## 667     1     M     M     M     2     2     2 12/01/1994 11/24/1997
    ## 668     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 669     1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 670     1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 671     1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 672     1     M     *     *     2     1     1 11/25/1994 11/24/1997
    ## 673     2     *     *     B     1    -9     1 11/08/1994 11/14/1997
    ## 674     1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 675     1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 676     1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 677     1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 678     2     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 679     0     *    DS    DC     1    -1    -1 11/22/1994 11/17/1997
    ## 680     1     M     *     M     2     1     2 11/04/1994 11/12/1997
    ## 681     1     L     L     L     1     1     1 11/17/1994 11/11/1997
    ## 682     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 683     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 684     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 685     1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 686     1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 687     1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 688     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 689     1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 690     1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 691     2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 692     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 693     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 694     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 695     1     M    MQ     M     2     2     2 12/01/1994 11/21/1997
    ## 696     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 697     2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 698     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 699     1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 700     0     *     *    DS     1     1    -1 11/14/1994 11/17/1997
    ## 701     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 702     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 703     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 704     1     M     M     M     2     2     2 11/23/1994 11/14/1997
    ## 705     1     *     *     Q     1     1     1 12/02/1994 11/25/1997
    ## 706     1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 707     1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 708     1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 709     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 710     1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 711     1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 712     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 713     1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 714     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 715     1     *     L     *     1     1     1 11/07/1994 11/19/1997
    ## 716     1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 717     1     *     R     L     1     1     1 12/05/1994 11/25/1997
    ## 718     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 719     2     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 720     1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 721     1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 722     1     M     *     M     2     1     2 12/06/1994 11/26/1997
    ## 723     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 724     1     M     M     M     2     2     2 11/23/1994 11/18/1997
    ## 725     1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 726     1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 727     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 728     1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 729     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 730     1     M     M     M     2     2     2 11/23/1994 11/19/1997
    ## 731     1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 732     1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 733     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 734     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 735     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 736     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 737     1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 738     1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 739     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 740     1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 741     2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 742     1     M     M     M     4     4     4 11/30/1994 11/20/1997
    ## 743     2     *     *     *     2     1     1 12/01/1994 11/21/1997
    ## 744     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 745     1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 746     1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 747     0     *     *    DS     1     1    -1 11/24/1994 11/19/1997
    ## 748     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 749     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 750     1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 751     1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 752     1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 753     2     *   MLQ     M     2     3     2 11/18/1994 11/27/1997
    ## 754     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 755     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 756     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 757     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 758     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 759     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 760     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 761     1     *     Q     *     1     1     1 11/11/1994 11/18/1997
    ## 762     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 763     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 764     2     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 765     1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 766     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 767     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 768     2     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 769     1     M     M     M     2     2     2 11/17/1994 11/25/1997
    ## 770     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 771     1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 772     1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 773     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 774     0     *    DC    DC     1    -1    -1 12/01/1994 11/19/1997
    ## 775     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 776     1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 777     1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 778     1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 779     1     Q     *     Q     1     1     1 11/16/1994 11/21/1997
    ## 780     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 781     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 782     1     M     M     M     2     2     2 11/08/1994 11/17/1997
    ## 783     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 784     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 785     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 786     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 787     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 788     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 789     1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 790     1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 791     1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 792     1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 793     1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 794     1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 795     0     *    DS    DS     1    -1    -1 12/02/1994 11/19/1997
    ## 796     1     *     *     *     2     1     1 12/01/1994 11/24/1997
    ## 797     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 798     2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 799     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 800     1     M     M     *     2     2     1 12/02/1994 11/19/1997
    ## 801     2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 802     1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 803     1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 804     1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 805     0     *    DC    DC     1    -1    -1 12/02/1994 11/24/1997
    ## 806     1     *     L     *     1     1     1 11/22/1994 11/17/1997
    ## 807     2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 808     1     M     R     *     3     1     1 12/01/1994 11/20/1997
    ## 809     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 810     1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 811     1     *     Q     *     1     1     1 11/14/1994 11/21/1997
    ## 812     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 813     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 814     1     M     M     M     2     3     3 12/01/1994 11/19/1997
    ## 815     1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 816     1     M     M     *     2     2     1 11/14/1994 11/17/1997
    ## 817     1     *     Q     *     1     1     1 11/23/1994 11/24/1997
    ## 818     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 819     1     M     M     M     6     6     8 11/10/1994 11/14/1997
    ## 820     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 821     1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 822     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 823     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 824     1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 825     1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 826     1     M     M     M     3     3     3 11/09/1994 11/17/1997
    ## 827     1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 828     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 829     1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 830     1     *     *     L     1     1     1 11/07/1994 11/11/1997
    ## 831     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 832     1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 833     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 834     1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 835     1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 836     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 837     1     M     M     M     2     2     2 11/21/1994 11/28/1997
    ## 838     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 839     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 840     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 841     1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 842     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 843     1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 844     1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 845     2     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 846     0     *    DC    DC     1    -1    -1 11/18/1994 11/26/1997
    ## 847     1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 848     1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 849     1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 850     2     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 851     1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 852     1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 853     1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 854     1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 855     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 856     1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 857     1     *     M     M     1     2     2 11/07/1994 11/12/1997
    ## 858     3     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 859     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 860     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 861     1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 862     1     M     *     *     2     1     1 11/07/1994 11/19/1997
    ## 863     1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 864     1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 865     1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 866     1     M     *     *     2     1     1 11/21/1994 11/28/1997
    ## 867     1     M     M     M     2     2     2 12/02/1994 11/21/1997
    ## 868     1     M    MQ     M     2     2     2 11/23/1994 11/13/1997
    ## 869     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 870     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 871     1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 872     1     *     M     M     1     2     2 11/07/1994 11/12/1997
    ## 873     1     M     M     M     3     3     3 11/07/1994 11/13/1997
    ## 874     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 875     1     *     *     L     1     1     1 11/09/1994 11/13/1997
    ## 876     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 877     1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 878     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 879     1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 880     3     *     *     B     1     1     1 11/11/1994 11/17/1997
    ## 881     1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 882     1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 883     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 884     1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 885     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 886     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 887     1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 888     1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 889     1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 890     1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 891     1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 892     1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 893     0     *     R    DC     1    -1    -1 11/14/1994 11/19/1997
    ## 894     1     *     Q     *     1     1     1 11/22/1994 11/17/1997
    ## 895     1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 896     1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 897     1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 898     1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 899     1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 900     1     L     *     *     1     1     1 11/30/1994 11/20/1997
    ## 901     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 902     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 903     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 904     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 905     1     *     M     M     3     3     3 12/01/1994 11/19/1997
    ## 906     2     *     *     *     1    -9     1 11/16/1994 11/21/1997
    ## 907     1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 908     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 909     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 910     0     *    DC    DN     1    -1    -1 11/18/1994 11/12/1997
    ## 911     1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 912     1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 913     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 914     1     *     M     M     8     2     3 12/01/1994 11/21/1997
    ## 915     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 916     1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 917     1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 918     1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 919     1     M     M     M     2     2     2 11/11/1994 11/18/1997
    ## 920     0     *     *    DS     1     1    -1 11/11/1994 11/18/1997
    ## 921     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 922     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 923     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 924     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 925     1     *     *     Q     1     1     1 11/25/1994 11/24/1997
    ## 926     1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 927     1     L     L     L     1     1     1 11/09/1994 11/21/1997
    ## 928     1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 929     1     L     *     L     1     1     1 11/14/1994 11/26/1997
    ## 930     1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 931     1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 932     1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 933     1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 934     1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 935     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 936     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 937     1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 938     1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 939     1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 940     1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 941     1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 942     1     *     *     Q     1     1     1 11/18/1994 11/27/1997
    ## 943     1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 944     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 945     1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 946     1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 947     1     M     *     *     2     1     1 11/23/1994 11/13/1997
    ## 948     1     M     M     M     2     2     2 11/24/1994 11/14/1997
    ## 949     1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 950     1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 951     1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 952     1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 953     1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 954     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 955     1     M     R     *     2     1     1 11/09/1994 11/21/1997
    ## 956     1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 957     1     *     *    LQ     1     1     1 11/07/1994 11/11/1997
    ## 958     1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 959     1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 960     1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 961     1     Q     *     Q     1     1     1 11/16/1994 11/27/1997
    ## 962     1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 963     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 964     1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 965     1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 966     1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 967     1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 968     1     *     Q     *     1     1     1 11/15/1994 11/21/1997
    ## 969     1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 970     3     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 971     1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 972     2     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 973     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 974     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 975     1     *     *     Q     1     1     1 12/01/1994 11/19/1997
    ## 976     1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 977     1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 978     1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 979     1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 980     1     *     S     Q     1    -9     1 11/08/1994 11/17/1997
    ## 981     1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 982     1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 983     1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 984     1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 985     1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 986     1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 987     2     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 988     1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 989     1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 990     1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 991     1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 992     1     M     M     M     2     2     2 11/10/1994 11/14/1997
    ## 993     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 994     1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 995     1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 996     1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 997     1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 998     1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 999     1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1000    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 1001    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1002    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1003    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 1004    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1005    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1006    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1007    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 1008    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1009    1     M     M     M     4     6     7 11/18/1994 11/26/1997
    ## 1010    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1011    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1012    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1013    1     *     *     Q     1     1     1 11/16/1994 11/27/1997
    ## 1014    2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1015    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1016    2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1017    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1018    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1019    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1020    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 1021    1     Q     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1022    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1023    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 1024    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1025    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1026    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1027    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1028    2     M     M     M     3     2     3 11/02/1994 11/26/1997
    ## 1029    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1030    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1031    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1032    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1033    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1034    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1035    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1036    1     M     M    MQ     6     4     5 11/09/1994 11/17/1997
    ## 1037    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 1038    1     M     M     M     2     2     2 11/23/1994 11/14/1997
    ## 1039    2     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1040    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 1041    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1042    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1043    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1044    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 1045    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1046    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1047    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 1048    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 1049    1     M     M     M     2     2     2 11/04/1994 11/13/1997
    ## 1050    0     *    DC    DS     1    -1    -1 11/18/1994 11/12/1997
    ## 1051    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1052    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1053    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1054    1     M     *     *     2     1     1 11/17/1994 11/11/1997
    ## 1055    2     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1056    1    ML    ML    ML     2     2     2 11/18/1994 11/12/1997
    ## 1057    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1058    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1059    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1060    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 1061    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1062    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1063    2     M     M     M     5     4     5 11/09/1994 12/17/1997
    ## 1064    0     *    DS    DS     1    -1    -1 11/09/1994 12/17/1997
    ## 1065    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 1066    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1067    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 1068    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1069    2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1070    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1071    2     M     B     B     2    -9     1 11/08/1994 11/14/1997
    ## 1072    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1073    1     *     Q     *     1     1     1 11/17/1994 11/26/1997
    ## 1074    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1075    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 1076    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1077    2     *    ML     M     1     2     2 11/22/1994 11/17/1997
    ## 1078    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1079    1     Q     R     Q     1     1     1 11/23/1994 11/18/1997
    ## 1080    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1081    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1082    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1083    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1084    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1085    1     L     L    ML     1     1     2 11/07/1994 11/12/1997
    ## 1086    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 1087    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1088    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1089    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1090    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1091    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1092    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1093    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1094    1     M     *     M     2     1     3 11/17/1994 11/25/1997
    ## 1095    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1096    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1097    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1098    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1099    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1100    1     M     M     M     2     2     2 12/05/1994 11/26/1997
    ## 1101    1     *    ML    ML     1     4     3 11/21/1994 11/28/1997
    ## 1102    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1103    1     *     Q     *     1     1     1 11/04/1994 11/12/1997
    ## 1104    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1105    1     M     M     M     2     2     2 12/01/1994 11/21/1997
    ## 1106    0     *    DS    DS     1    -1    -1 12/02/1994 11/25/1997
    ## 1107    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 1108    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1109    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 1110    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1111    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1112    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1113    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1114    1     M     M     M     2     2     2 11/22/1994 11/17/1997
    ## 1115    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1116    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1117    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1118    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1119    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1120    0     *     *    DS     1     1    -1 11/07/1994 11/11/1997
    ## 1121    1     *     R     R     1     1     1 11/08/1994 11/12/1997
    ## 1122    3     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 1123    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 1124    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 1125    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1126    1     M     M     M     2     2     2 11/18/1994 11/12/1997
    ## 1127    1     L     L     L     1     1     1 11/24/1994 11/17/1997
    ## 1128    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 1129    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1130    2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1131    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 1132    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 1133    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1134    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 1135    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1136    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1137    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 1138    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1139    0     *    DS    DS     1    -1    -1 11/08/1994 11/14/1997
    ## 1140    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 1141    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1142    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1143    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 1144    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 1145    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1146    1     *     *     M     1     1     2 11/11/1994 11/25/1997
    ## 1147    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1148    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 1149    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1150    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1151    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1152    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1153    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 1154    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 1155    1     *     M     M     2     2     2 12/06/1994 11/26/1997
    ## 1156    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1157    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1158    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 1159    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1160    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1161    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1162    1     M     M     M     2     2     3 11/14/1994 12/17/1997
    ## 1163    1     M     M     M     3     3     3 11/14/1994 11/21/1997
    ## 1164    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 1165    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 1166    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1167    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1168    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 1169    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1170    0     L    DC    DT     1    -1    -1 11/15/1994 11/20/1997
    ## 1171    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1172    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1173    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1174    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1175    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 1176    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 1177    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1178    0     *     *    DC     1     1    -1 11/07/1994 11/11/1997
    ## 1179    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1180    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 1181    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1182    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1183    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1184    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1185    2     M     *     *     2     1     1 11/07/1994 11/20/1997
    ## 1186    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1187    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1188    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1189    1     M     M     M     2     2     2 12/02/1994 11/19/1997
    ## 1190    1     *     *     Q     1     1     1 11/07/1994 11/11/1997
    ## 1191    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1192    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1193    1     *     L     *     1     1     1 11/11/1994 11/18/1997
    ## 1194    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1195    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1196    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1197    2     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1198    2     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 1199    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1200    2     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1201    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1202    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 1203    1     M     M     M     3     3     3 12/05/1994 11/25/1997
    ## 1204    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 1205    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1206    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1207    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 1208    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1209    2     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 1210    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1211    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1212    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 1213    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1214    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1215    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1216    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 1217    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1218    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 1219    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1220    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1221    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1222    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 1223    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 1224    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1225    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1226    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1227    1     M     M     M     3     2     2 12/01/1994 11/21/1997
    ## 1228    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1229    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1230    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1231    0     *     *    DC     1     1    -1 11/09/1994 11/21/1997
    ## 1232    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1233    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1234    2     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 1235    0     *     R    DC     1    -1    -1 11/23/1994 11/18/1997
    ## 1236    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1237    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 1238    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 1239    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1240    1     M     M     M     3     3     3 11/16/1994 11/27/1997
    ## 1241    1     *     *     L     1     1     1 11/17/1994 11/12/1997
    ## 1242    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1243    1     *     *     *     2     1     1 12/01/1994 11/24/1997
    ## 1244    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1245    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1246    1     M     M     M     3     3     3 11/17/1994 11/25/1997
    ## 1247    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 1248    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1249    0     *     *    DS     1     1    -1 11/08/1994 11/17/1997
    ## 1250    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1251    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 1252    1     *     L     L     1     1     1 11/11/1994 11/24/1997
    ## 1253    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1254    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1255    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1256    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1257    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1258    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1259    1     M     M     M     4     3     3 11/23/1994 11/24/1997
    ## 1260    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1261    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1262    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 1263    1     M     M     M     7     6     6 11/04/1994 11/12/1997
    ## 1264    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1265    1     Q     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1266    1     M     M     M     2     2     2 11/23/1994 11/13/1997
    ## 1267    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 1268    1     M     M     M     2     4     4 12/05/1994 11/25/1997
    ## 1269    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 1270    1     *     *     R     1     1     1 11/23/1994 11/24/1997
    ## 1271    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1272    3     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 1273    1     M     M     M     3     4     4 11/22/1994 11/17/1997
    ## 1274    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 1275    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1276    1     M     *     M     2     1     2 11/18/1994 11/27/1997
    ## 1277    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 1278    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1279    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1280    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1281    1     M     M     M     4     4     4 11/08/1994 11/13/1997
    ## 1282    3     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1283    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1284    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1285    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1286    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1287    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 1288    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1289    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1290    1     *     *     M     1     1     2 11/09/1994 12/17/1997
    ## 1291    0     *     *    DS     1     1    -1 11/09/1994 12/17/1997
    ## 1292    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1293    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1294    0     *     *    DC     1     1    -1 11/09/1994 11/21/1997
    ## 1295    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1296    0     *     *    DS     1     1    -1 11/14/1994 11/26/1997
    ## 1297    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 1298    0     *    DS    DS     1    -1    -1 11/16/1994 11/27/1997
    ## 1299    1     Q     *     *     1     1     1 12/02/1994 11/19/1997
    ## 1300    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1301    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1302    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1303    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 1304    1     M     M     M     7     7     6 12/02/1994 11/25/1997
    ## 1305    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 1306    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1307    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 1308    1     M     M     M     2     2     2 12/01/1994 11/20/1997
    ## 1309    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1310    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1311    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1312    1     L     *     L     1    -9     1 11/09/1994 11/21/1997
    ## 1313    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1314    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1315    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 1316    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1317    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1318    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1319    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1320    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 1321    1     M     M     M     2     2     2 11/29/1994 11/17/1997
    ## 1322    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1323    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 1324    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1325    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1326    1     *     *     *     2     1     1 11/11/1994 11/17/1997
    ## 1327    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1328    1    ML    ML     M     2     2     2 11/02/1994 11/11/1997
    ## 1329    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 1330    0     *    DS    DC     1    -1    -1 11/17/1994 11/25/1997
    ## 1331    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 1332    1     M     M     M     2     2     2 11/08/1994 11/14/1997
    ## 1333    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1334    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 1335    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1336    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1337    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1338    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1339    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1340    1     M     *     *     3     1     1 11/07/1994 11/11/1997
    ## 1341    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 1342    1     *     Q     *     1     1     1 11/18/1994 11/26/1997
    ## 1343    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1344    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1345    0     *    DS    DC     1    -1    -1 11/24/1994 11/19/1997
    ## 1346    2     *     R     *     1    -1     1 12/01/1994 11/20/1997
    ## 1347    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1348    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1349    0     *    DS    DS     1    -1    -1 11/08/1994 11/17/1997
    ## 1350    1     L     L     L     1     1     1 11/08/1994 11/17/1997
    ## 1351    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1352    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1353    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 1354    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1355    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1356    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1357    0     *     *    DS     1     1    -1 11/17/1994 11/12/1997
    ## 1358    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 1359    1     *     *    ML     1     1     4 11/24/1994 11/14/1997
    ## 1360    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 1361    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1362    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1363    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1364    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1365    1     M     M     *     2     2     1 12/02/1994 11/24/1997
    ## 1366    0     *    DS    DS     1    -1    -1 11/09/1994 12/17/1997
    ## 1367    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1368    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1369    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1370    1     M     *     *     2     1     1 11/09/1994 11/24/1997
    ## 1371    2     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1372    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1373    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1374    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1375    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1376    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1377    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1378    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1379    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1380    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1381    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 1382    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1383    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 1384    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 1385    1     M    ML    MR     5     4     3 11/07/1994 11/12/1997
    ## 1386    2     M     *     *     2     1     1 11/10/1994 11/14/1997
    ## 1387    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1388    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1389    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1390    1     M     M     M     2     2     2 11/16/1994 11/21/1997
    ## 1391    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1392    0     *    DC    DC     1    -1    -1 11/24/1994 11/19/1997
    ## 1393    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1394    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1395    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 1396    1     Q     *     *     1     1     1 11/04/1994 11/12/1997
    ## 1397    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1398    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1399    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1400    1     M     M     M     2     2     2 11/24/1994 11/17/1997
    ## 1401    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 1402    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1403    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 1404    0    ML    DC    DC     4    -1    -1 11/21/1994 11/20/1997
    ## 1405    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 1406    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 1407    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1408    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1409    2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1410    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1411    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1412    1     *     *     L     1     1     1 12/02/1994 11/21/1997
    ## 1413    1     M     *     *     2     1     1 11/02/1994 11/26/1997
    ## 1414    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1415    3     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1416    0     L     L    DT     1     1    -1 11/04/1994 11/12/1997
    ## 1417    1     M     M     M     3     3     3 11/04/1994 11/12/1997
    ## 1418    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 1419    2     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 1420    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1421    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 1422    1     *     M    MR     3     2     2 12/02/1994 11/24/1997
    ## 1423    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1424    0     *   DSQ    DS     1    -1    -1 11/07/1994 11/11/1997
    ## 1425    1     M     M     M     3     4     3 11/21/1994 11/20/1997
    ## 1426    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1427    2     *     *     R     1     1     1 11/30/1994 11/21/1997
    ## 1428    1     M     M     M     2     2     2 12/01/1994 11/21/1997
    ## 1429    1     M     M     M     6     5     5 11/02/1994 11/11/1997
    ## 1430    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1431    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1432    2     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1433    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1434    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1435    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 1436    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1437    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1438    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1439    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1440    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 1441    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1442    2     *     R     *     1    -1     1 12/02/1994 11/24/1997
    ## 1443    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1444    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1445    2     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 1446    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1447    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1448    2     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 1449    2     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 1450    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 1451    2     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1452    1     M     M     M     3     3     2 11/04/1994 11/13/1997
    ## 1453    2     M     M     M     4     3     5 11/07/1994 11/20/1997
    ## 1454    1     M     M     M     2     2     2 11/14/1994 11/26/1997
    ## 1455    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1456    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1457    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1458    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1459    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 1460    1     Q     R     *     2     1     1 11/23/1994 11/14/1997
    ## 1461    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 1462    1     M    MR     M     3     2     2 12/05/1994 11/25/1997
    ## 1463    1     M     M    MR     3     3     2 12/05/1994 11/25/1997
    ## 1464    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 1465    3     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1466    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1467    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 1468    1     M     M     M     2     2     2 12/05/1994 11/26/1997
    ## 1469    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1470    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1471    0     *    DC    DC     1    -1    -1 11/18/1994 11/26/1997
    ## 1472    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1473    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1474    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 1475    0     L    DC    DC     1    -1    -1 11/09/1994 11/21/1997
    ## 1476    1     M     M     M     2     3     3 11/14/1994 11/26/1997
    ## 1477    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1478    0     *    DS    DC     1    -1    -1 11/23/1994 11/14/1997
    ## 1479    1     M     M     M     4     4     4 11/29/1994 11/18/1997
    ## 1480    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1481    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1482    1     *     *     L     1     1     1 11/07/1994 11/11/1997
    ## 1483    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1484    1     M     M     M     3     3     3 11/09/1994 11/14/1997
    ## 1485    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1486    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1487    1     *     *     M     1     1     3 11/30/1994 11/21/1997
    ## 1488    3     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 1489    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1490    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 1491    2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1492    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1493    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1494    0     *     *    DS     1     1    -1 11/08/1994 11/17/1997
    ## 1495    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1496    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1497    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1498    2     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1499    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1500    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1501    2     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1502    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1503    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1504    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1505    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 1506    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1507    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1508    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1509    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1510    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1511    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1512    1    ML    ML     M     7     4     4 11/02/1994 11/11/1997
    ## 1513    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1514    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1515    1     M     M     M     3     2     3 12/01/1994 11/20/1997
    ## 1516    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1517    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1518    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1519    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1520    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1521    0     M    DS    DN     3    -1    -1 11/21/1994 11/27/1997
    ## 1522    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1523    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1524    1     *     L     L     1     1     1 11/11/1994 11/25/1997
    ## 1525    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1526    1     *     *     M     1     1     2 11/11/1994 11/25/1997
    ## 1527    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1528    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1529    0     M    DC    DC     2    -1    -1 11/16/1994 11/27/1997
    ## 1530    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1531    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1532    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 1533    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1534    1     M     M     M     3     2     2 11/08/1994 11/13/1997
    ## 1535    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1536    0     *     *    DS     1     1    -1 11/23/1994 11/18/1997
    ## 1537    1     L     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1538    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1539    0     *     *    DS     1     1    -1 11/02/1994 11/11/1997
    ## 1540    1     *     M     M     2     2     2 11/07/1994 11/12/1997
    ## 1541    2     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 1542    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1543    1     *     *     M     1     1     3 11/07/1994 11/13/1997
    ## 1544    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 1545    0     M    DS    DT     2    -1    -1 11/07/1994 11/19/1997
    ## 1546    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1547    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 1548    1    ML     M     M     6     5     4 11/29/1994 11/18/1997
    ## 1549    1     M     M     M     3     3     3 11/29/1994 11/18/1997
    ## 1550    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 1551    1     M     M     M     2     2     2 12/01/1994 11/18/1997
    ## 1552    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1553    2     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1554    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1555    0     M     M    DC     3     2    -1 12/02/1994 11/25/1997
    ## 1556    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1557    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1558    0     *    DS    DS     1    -1    -1 11/15/1994 11/20/1997
    ## 1559    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1560    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1561    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1562    0     M    DC    DC     2    -1    -1 11/09/1994 12/17/1997
    ## 1563    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1564    1     M     M     M     2     2     2 11/17/1994 11/12/1997
    ## 1565    1     M     M     M     4     3     4 11/18/1994 11/12/1997
    ## 1566    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1567    1     *     M     M     2     2     2 11/25/1994 11/25/1997
    ## 1568    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 1569    1     M     M     M     2     2     2 11/08/1994 11/13/1997
    ## 1570    0     *     *    DS     1     1    -1 11/08/1994 11/13/1997
    ## 1571    0     M     R    DS     2     1    -1 11/16/1994 11/19/1997
    ## 1572    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1573    1     M     M     M     2     2     2 11/07/1994 11/12/1997
    ## 1574    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1575    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1576    1     *     M     M     2     2     2 11/16/1994 11/21/1997
    ## 1577    1     *     M     M     1     2     2 11/16/1994 11/21/1997
    ## 1578    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1579    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1580    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 1581    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1582    1     M     M     M     3     3     3 11/11/1994 11/25/1997
    ## 1583    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1584    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1585    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1586    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1587    3     Q     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1588    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 1589    1     M     M     M     3     6     6 12/05/1994 11/25/1997
    ## 1590    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1591    1     *     *     M     3     1     2 11/09/1994 11/14/1997
    ## 1592    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1593    1     M     *     *     2     1     1 11/25/1994 11/25/1997
    ## 1594    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 1595    1     M     M     M     5     2     5 11/07/1994 11/12/1997
    ## 1596    1     M     M     *     2    -9     1 11/07/1994 11/12/1997
    ## 1597    1    QQ     *     *     1     1     1 11/09/1994 11/13/1997
    ## 1598    1     *     *     *     3     1     1 11/18/1994 11/27/1997
    ## 1599    1     L     L     *     1     1     1 11/18/1994 11/26/1997
    ## 1600    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1601    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 1602    1    ML     M     M     3     3     3 12/01/1994 11/21/1997
    ## 1603    1     *     *     M     1     1     2 12/02/1994 11/24/1997
    ## 1604    1    ML    ML     M     5     4     4 11/07/1994 11/13/1997
    ## 1605    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 1606    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1607    2     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1608    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1609    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 1610    1     M     M     M     2     2     2 11/07/1994 11/19/1997
    ## 1611    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1612    1    ML    ML    ML     3     3     4 11/16/1994 11/27/1997
    ## 1613    1     M     M     M     2     2     2 11/18/1994 11/12/1997
    ## 1614    1     M     M     *     3     3     1 11/22/1994 11/13/1997
    ## 1615    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1616    1    ML     M     M     3     3     3 12/02/1994 11/19/1997
    ## 1617    2     *    MR     R     1     2     1 12/05/1994 11/25/1997
    ## 1618    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1619    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1620    1     M     M     M     3     3     3 11/30/1994 11/21/1997
    ## 1621    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1622    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 1623    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1624    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 1625    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 1626    1     M    MR     M     3     5     2 11/21/1994 11/28/1997
    ## 1627    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1628    2     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 1629    1     L     *     L     1     1     1 12/01/1994 11/21/1997
    ## 1630    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 1631    1     *     M     *     1     2     1 11/07/1994 11/13/1997
    ## 1632    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 1633    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 1634    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 1635    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 1636    2     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1637    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1638    1     Q     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1639    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 1640    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 1641    1     M    MR     M     3     2     2 12/05/1994 11/25/1997
    ## 1642    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1643    1     M     M     M     2     3     3 11/07/1994 11/11/1997
    ## 1644    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 1645    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1646    1     M     *     *     1     1     1 11/29/1994 11/25/1997
    ## 1647    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1648    2     *     M     M     1     2     2 11/09/1994 11/13/1997
    ## 1649    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1650    2     *     *     R     1     1     1 11/15/1994 11/21/1997
    ## 1651    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1652    0     *    DC    DC     1    -1    -1 11/17/1994 11/25/1997
    ## 1653    1     M     M     M     4     4     4 11/23/1994 11/18/1997
    ## 1654    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1655    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1656    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1657    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1658    1     *     L     *     1     1     1 11/07/1994 11/20/1997
    ## 1659    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 1660    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1661    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1662    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1663    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1664    1     *     *     M     1     1     2 11/24/1994 11/17/1997
    ## 1665    1     M    MR     M     4     4     4 11/29/1994 11/17/1997
    ## 1666    1     *     M     M     1     3     3 12/05/1994 11/25/1997
    ## 1667    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1668    1     M     M     M     2     2     2 11/09/1994 11/17/1997
    ## 1669    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1670    1     *     M     M     2     2     2 12/02/1994 11/25/1997
    ## 1671    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1672    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1673    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1674    1     *     M     M     1     3     3 11/07/1994 11/12/1997
    ## 1675    1     M     *     *     2     1     1 11/07/1994 11/12/1997
    ## 1676    1     M     M     M     5     5     6 11/15/1994 11/20/1997
    ## 1677    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 1678    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1679    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 1680    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1681    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 1682    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1683    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1684    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1685    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1686    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1687    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1688    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 1689    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1690    1     M     M     M     2     2     2 11/08/1994 11/13/1997
    ## 1691    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 1692    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1693    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1694    2     *     M     M     2     2     2 11/23/1994 11/24/1997
    ## 1695    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 1696    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 1697    2     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 1698    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 1699    1     *     *     M     1     1     2 11/14/1994 11/19/1997
    ## 1700    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 1701    2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1702    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1703    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1704    1     *     *     M     1     1     2 11/23/1994 11/18/1997
    ## 1705    0     *    DC    DT     1    -1    -1 11/30/1994 11/20/1997
    ## 1706    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1707    1     M     M     M     3     3     4 11/07/1994 11/13/1997
    ## 1708    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1709    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1710    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1711    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1712    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1713    1     M     M     M     3     3     3 11/15/1994 11/21/1997
    ## 1714    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1715    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 1716    1     M     M     M     2     2     2 11/04/1994 11/13/1997
    ## 1717    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 1718    1     M     M     M     2     2     2 11/14/1994 11/26/1997
    ## 1719    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1720    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 1721    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1722    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 1723    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1724    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1725    1     M     M     M     3     3     3 11/08/1994 11/13/1997
    ## 1726    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1727    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1728    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 1729    1     M     M     M     3     3     3 11/25/1994 11/25/1997
    ## 1730    1    ML    ML    ML     4     4     4 11/10/1994 11/14/1997
    ## 1731    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1732    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1733    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1734    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1735    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1736    0    MQ    DS    DS     2    -1    -1 11/09/1994 11/17/1997
    ## 1737    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1738    1     M     M     M     4     2     4 11/17/1994 11/26/1997
    ## 1739    1     M     M     M     2     2     6 11/21/1994 11/27/1997
    ## 1740    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 1741    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1742    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1743    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 1744    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 1745    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1746    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1747    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1748    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1749    1     M     M     M     4     3     5 11/16/1994 11/18/1997
    ## 1750    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 1751    1     M     M     M     4     3     3 12/02/1994 11/25/1997
    ## 1752    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 1753    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1754    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 1755    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1756    1     M     M     M     2     2     2 11/16/1994 11/21/1997
    ## 1757    0     *    DC    DC     1    -1    -1 11/18/1994 11/26/1997
    ## 1758    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1759    1     M     M     M     2     2     2 11/14/1994 12/17/1997
    ## 1760    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1761    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 1762    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1763    0     *    DS    DS     1    -1    -1 11/11/1994 11/25/1997
    ## 1764    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1765    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1766    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 1767    0     *     R    DC     1    -1    -1 12/05/1994 11/25/1997
    ## 1768    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1769    0     *    DC    DC     1    -1    -1 11/08/1994 11/14/1997
    ## 1770    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1771    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1772    1     M     *     *     2     1     1 12/01/1994 11/21/1997
    ## 1773    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 1774    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1775    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1776    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1777    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1778    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1779    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1780    1     M     M     M     3     2     2 11/23/1994 11/18/1997
    ## 1781    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1782    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1783    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 1784    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1785    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1786    1     M     N     M     3    -9     3 11/14/1994 11/26/1997
    ## 1787    0     *    DS    DC     1    -1    -1 11/15/1994 11/26/1997
    ## 1788    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1789    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1790    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1791    2     M     M     M     3     2     2 11/29/1994 11/17/1997
    ## 1792    1     M     M    ML     2     3     3 12/05/1994 11/25/1997
    ## 1793    1     M     M     M     2     4     4 12/05/1994 11/25/1997
    ## 1794    1     M     M     M     3     3     3 11/07/1994 11/11/1997
    ## 1795    2     M     M     M     2     2     3 11/16/1994 11/20/1997
    ## 1796    1     *     *     M     1     1     2 11/22/1994 11/20/1997
    ## 1797    1     *    MR     M     1     2     2 11/23/1994 11/24/1997
    ## 1798    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 1799    1     M     M     M     2     2     2 11/30/1994 11/21/1997
    ## 1800    1     M     M     *     2     2     1 11/07/1994 11/12/1997
    ## 1801    1     L     L     L     1     1     1 11/10/1994 11/14/1997
    ## 1802    1     M    ML    ML     2     2     3 11/10/1994 11/14/1997
    ## 1803    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1804    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 1805    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1806    0     *    DN    DN     1    -1    -1 11/18/1994 11/26/1997
    ## 1807    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1808    0     *     L    DC     1     1    -1 11/21/1994 11/28/1997
    ## 1809    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1810    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1811    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1812    1     M     *     *     2     1     1 11/30/1994 11/20/1997
    ## 1813    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1814    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1815    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1816    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1817    1     M     *     L     2     1     1 11/16/1994 11/24/1997
    ## 1818    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 1819    1     M     *     M     3     1     3 11/15/1994 11/26/1997
    ## 1820    0     *    DC    DN     1    -1    -1 11/16/1994 11/27/1997
    ## 1821    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1822    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1823    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1824    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1825    1     M     M     M     2     2     3 11/29/1994 11/18/1997
    ## 1826    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 1827    1     *     R     *     1     1     1 11/07/1994 11/11/1997
    ## 1828    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1829    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1830    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 1831    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1832    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1833    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1834    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1835    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1836    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 1837    1     M    MR     M     4     2     2 11/02/1994 11/11/1997
    ## 1838    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 1839    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1840    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 1841    1     M     M     M     4     4     4 12/01/1994 11/21/1997
    ## 1842    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 1843    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1844    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 1845    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1846    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1847    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1848    0     *    DC    DT     1    -1    -1 11/15/1994 11/26/1997
    ## 1849    1     M     M     M     2     2     2 11/16/1994 11/27/1997
    ## 1850    1     M     M     M     2     2     3 11/16/1994 11/27/1997
    ## 1851    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1852    1     M     M     M     4     5     4 12/02/1994 11/24/1997
    ## 1853    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 1854    0    ML    ML    DC     2     2    -1 11/07/1994 11/11/1997
    ## 1855    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 1856    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1857    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1858    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1859    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1860    1     Q     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1861    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1862    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 1863    1    ML     M     M     5     4     5 11/14/1994 11/19/1997
    ## 1864    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 1865    2     *     M     M     2     2     2 11/18/1994 11/27/1997
    ## 1866    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 1867    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 1868    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 1869    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1870    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1871    1     M     M     M     3     2     3 11/14/1994 12/17/1997
    ## 1872    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 1873    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1874    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1875    0     L    DS    DC     1    -1    -1 11/14/1994 11/26/1997
    ## 1876    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 1877    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 1878    1     M     M     M     3     3     3 11/17/1994 11/11/1997
    ## 1879    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 1880    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 1881    1     M     M     M     2     2     3 11/09/1994 11/14/1997
    ## 1882    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1883    1     *     L     *     1     1     1 11/16/1994 11/18/1997
    ## 1884    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1885    1     *     *     *     3     1     1 11/25/1994 11/24/1997
    ## 1886    1     M     *     *     2     1     1 11/30/1994 11/21/1997
    ## 1887    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 1888    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 1889    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1890    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 1891    0     Q     R    DS     1    -1    -1 11/15/1994 11/21/1997
    ## 1892    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 1893    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1894    1     M     M     M     3     3     4 11/23/1994 11/18/1997
    ## 1895    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1896    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1897    1     M     M     M     3     3     3 11/24/1994 11/19/1997
    ## 1898    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 1899    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 1900    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1901    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1902    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1903    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 1904    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 1905    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1906    1     *     *     M     2     1     2 11/09/1994 11/21/1997
    ## 1907    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 1908    0     *    DS    DC     1    -1    -1 11/11/1994 11/25/1997
    ## 1909    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1910    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1911    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1912    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 1913    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 1914    1     L     L     L     1     1     1 12/05/1994 11/25/1997
    ## 1915    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 1916    1     L     L     L     1     1     1 11/16/1994 11/19/1997
    ## 1917    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 1918    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1919    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 1920    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 1921    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1922    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 1923    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1924    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 1925    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1926    1    ML    ML    ML     3     3     3 11/09/1994 11/13/1997
    ## 1927    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 1928    1     M     M     M     2     2     2 11/07/1994 11/13/1997
    ## 1929    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 1930    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1931    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1932    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1933    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1934    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1935    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 1936    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 1937    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 1938    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 1939    1     M     M     M     2     2     2 11/09/1994 11/24/1997
    ## 1940    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 1941    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 1942    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 1943    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1944    1     M     M     M     3     5     3 11/18/1994 11/12/1997
    ## 1945    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1946    1     *     M     M     4     5     5 11/29/1994 11/17/1997
    ## 1947    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 1948    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 1949    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1950    1     M    MR     M     3     3     4 12/05/1994 11/25/1997
    ## 1951    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 1952    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 1953    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1954    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 1955    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1956    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 1957    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1958    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 1959    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1960    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 1961    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 1962    1     *     M     *     1     2     1 11/16/1994 11/21/1997
    ## 1963    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 1964    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 1965    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 1966    1     M     M     M     2     2     2 11/08/1994 11/14/1997
    ## 1967    1    ML     M     M     3     2     3 11/08/1994 11/14/1997
    ## 1968    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 1969    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 1970    1     M     M     M     3     2     3 11/04/1994 11/13/1997
    ## 1971    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 1972    1     M     M     M     2     2     2 11/07/1994 11/20/1997
    ## 1973    2     M     *     M     3     1     2 11/08/1994 11/20/1997
    ## 1974    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 1975    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 1976    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 1977    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 1978    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 1979    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 1980    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 1981    1     M     M     M     2     2     2 11/07/1994 11/11/1997
    ## 1982    2     M     M    MR     2     2     2 11/11/1994 11/17/1997
    ## 1983    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 1984    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 1985    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 1986    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 1987    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 1988    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 1989    1    ML     M     M     3     3     3 11/25/1994 11/25/1997
    ## 1990    2     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 1991    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 1992    1     M     M     M     3     3     3 12/05/1994 11/26/1997
    ## 1993    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 1994    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 1995    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1996    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 1997    2     M     M     M     2     4     2 11/14/1994 11/19/1997
    ## 1998    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 1999    1     Q     *     *     1     1     1 11/14/1994 11/20/1997
    ## 2000    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2001    1     M     M     M     4     4     4 11/21/1994 11/28/1997
    ## 2002    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2003    2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 2004    1     M     M     M     2     2     2 12/01/1994 11/20/1997
    ## 2005    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2006    0     *     *    DS     1     1    -1 11/02/1994 11/26/1997
    ## 2007    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2008    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 2009    0     *    DS    DS     1    -1    -1 11/17/1994 11/26/1997
    ## 2010    1     M     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2011    0     *    DS    DC     1    -1    -1 11/07/1994 11/20/1997
    ## 2012    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 2013    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2014    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 2015    1     M     M     M     2     2     2 11/17/1994 11/12/1997
    ## 2016    1     M     M     M     2     2     2 11/17/1994 11/12/1997
    ## 2017    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2018    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2019    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 2020    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2021    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2022    2     M     M     M     2     2     2 11/16/1994 11/19/1997
    ## 2023    1     M     M     *     2     2     1 11/16/1994 11/20/1997
    ## 2024    1     M     M     M     2     2     2 11/23/1994 11/24/1997
    ## 2025    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 2026    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 2027    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 2028    1     M     M     M     2     2     2 11/07/1994 11/12/1997
    ## 2029    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2030    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2031    2     *     R     *     1    -1     1 11/14/1994 11/19/1997
    ## 2032    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2033    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2034    0     *     *    DS     1     1    -1 11/18/1994 11/27/1997
    ## 2035    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2036    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 2037    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 2038    1     M     M     M     2     2     2 11/07/1994 11/13/1997
    ## 2039    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 2040    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 2041    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2042    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2043    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2044    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2045    2     M     M     M     4     3     3 11/14/1994 11/26/1997
    ## 2046    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2047    1     M     *     *     2     1     1 11/22/1994 11/13/1997
    ## 2048    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2049    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 2050    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 2051    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2052    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2053    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2054    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2055    1     M     M     M     3     3     3 11/16/1994 11/20/1997
    ## 2056    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 2057    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2058    1     *     L     *     1     1     1 11/23/1994 11/19/1997
    ## 2059    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2060    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 2061    1    ML    ML    ML     2     2     3 11/02/1994 11/11/1997
    ## 2062    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2063    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2064    0     *     *    DS     1     1    -1 11/15/1994 11/21/1997
    ## 2065    1     M     M     M     2     2     2 11/18/1994 11/26/1997
    ## 2066    1     *     L     L     1     1     1 12/01/1994 11/20/1997
    ## 2067    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2068    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2069    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2070    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2071    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2072    1     M     M     M     2     2     2 11/14/1994 11/21/1997
    ## 2073    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 2074    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2075    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2076    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 2077    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2078    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2079    0     *    DC    DC     1    -1    -1 11/15/1994 11/26/1997
    ## 2080    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2081    1     M     M     M     2     3     3 11/22/1994 11/13/1997
    ## 2082    1     L     *     M     1     1     2 11/24/1994 11/17/1997
    ## 2083    1     M     M     M     2     4     4 11/29/1994 11/18/1997
    ## 2084    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2085    2     *     R     *     1     1     1 11/16/1994 11/18/1997
    ## 2086    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2087    1     M     *     M     2     1     2 11/16/1994 11/18/1997
    ## 2088    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2089    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2090    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 2091    1     M    MR     M     2     2     2 11/14/1994 11/19/1997
    ## 2092    0     *    DN    DT     1    -1    -1 11/14/1994 11/19/1997
    ## 2093    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2094    1     *     *     M     1     1     2 11/18/1994 11/27/1997
    ## 2095    0     M    DS    DC     2    -1    -1 11/18/1994 11/27/1997
    ## 2096    0     *    DN    DN     1    -1    -1 11/21/1994 11/28/1997
    ## 2097    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2098    1     M     M     M     3     2     2 12/01/1994 11/21/1997
    ## 2099    3     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2100    1     M     M     M     2     2     2 12/02/1994 11/24/1997
    ## 2101    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2102    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2103    1    ML     M     M     2     2     2 11/21/1994 11/27/1997
    ## 2104    3     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 2105    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 2106    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 2107    1     L     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2108    1     M     M     *     3     3     1 11/11/1994 11/24/1997
    ## 2109    1     *     *     M     1     1     2 11/11/1994 11/24/1997
    ## 2110    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2111    1     M     M    ML     2     2     2 11/11/1994 11/25/1997
    ## 2112    2     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2113    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2114    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2115    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2116    1     M     M     M     6     6     6 11/14/1994 11/26/1997
    ## 2117    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 2118    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2119    2     *     *     R     1     1     1 11/16/1994 11/27/1997
    ## 2120    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2121    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2122    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2123    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2124    1     M     M     M     2     2     2 11/22/1994 11/13/1997
    ## 2125    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 2126    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2127    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 2128    1    ML     L     L     2     1     1 11/24/1994 11/17/1997
    ## 2129    0     *    DS    DS     1    -1    -1 11/29/1994 11/17/1997
    ## 2130    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2131    0     *    DS    DC     1    -1    -1 12/01/1994 11/19/1997
    ## 2132    1     *     M     M     1     2     2 12/02/1994 11/24/1997
    ## 2133    1     *     *     L     2     1     1 11/07/1994 11/11/1997
    ## 2134    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 2135    1     M     M    MR     2     2     2 11/08/1994 11/13/1997
    ## 2136    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 2137    1     M     *     M     1     1     2 11/09/1994 11/14/1997
    ## 2138    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 2139    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 2140    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2141    1     L    ML    ML     1     2     2 11/14/1994 11/17/1997
    ## 2142    0     *     *    DS     1     1    -1 11/16/1994 11/18/1997
    ## 2143    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 2144    1     L     L     L     1     1     1 11/25/1994 11/24/1997
    ## 2145    0     *     *    DS     1     1    -1 11/25/1994 11/24/1997
    ## 2146    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 2147    1     M     M     M     4     2     2 12/02/1994 11/25/1997
    ## 2148    0     L    DC    DC     1    -1    -1 12/05/1994 11/26/1997
    ## 2149    1     L     M     M     1     2     2 11/09/1994 11/13/1997
    ## 2150    2     *     M     M     2     2     2 11/15/1994 11/20/1997
    ## 2151    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 2152    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2153    1     *     M     M     6     3     4 11/21/1994 11/28/1997
    ## 2154    1     M     M     M     2     3     2 11/24/1994 11/19/1997
    ## 2155    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 2156    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2157    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2158    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2159    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 2160    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2161    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2162    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2163    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2164    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2165    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2166    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 2167    1    ML     R     *     2     1     1 11/08/1994 11/20/1997
    ## 2168    1     L     M     M     2     2     2 11/11/1994 11/25/1997
    ## 2169    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2170    1     M     M     M     2     2     2 11/22/1994 11/13/1997
    ## 2171    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 2172    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 2173    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2174    1     M     M     M     2     2     2 12/05/1994 11/25/1997
    ## 2175    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2176    1     M     M     M     2     2     2 11/08/1994 11/14/1997
    ## 2177    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2178    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2179    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 2180    1     *     L     L     1     1     1 11/23/1994 11/24/1997
    ## 2181    0     *    DS    DS     1    -1    -1 11/25/1994 11/24/1997
    ## 2182    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2183    1    ML     M     M     2     2     2 12/02/1994 11/25/1997
    ## 2184    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 2185    2     *     R     *     1    -1     1 12/02/1994 11/25/1997
    ## 2186    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 2187    3     *     M     M     2     2     2 11/16/1994 11/21/1997
    ## 2188    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 2189    0     *    DS    DS     1    -1    -1 11/22/1994 11/17/1997
    ## 2190    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2191    0     *    DC    DN     1    -1    -1 11/23/1994 11/18/1997
    ## 2192    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 2193    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 2194    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 2195    1     M     M     M     3     2     2 11/08/1994 11/17/1997
    ## 2196    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2197    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2198    1     M     R     *     2     1     1 11/14/1994 11/21/1997
    ## 2199    1     M     *     *     2     1     1 11/18/1994 11/27/1997
    ## 2200    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2201    2     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 2202    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 2203    1     M     M     M     4     3     4 11/14/1994 11/26/1997
    ## 2204    1     *     *     M     1     1     2 11/16/1994 11/27/1997
    ## 2205    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 2206    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 2207    1     M     M     M     4     5     7 11/09/1994 11/17/1997
    ## 2208    1     M     M     M     2     2     2 11/14/1994 11/17/1997
    ## 2209    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2210    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2211    1     M     M     M     2     2     3 11/16/1994 11/20/1997
    ## 2212    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 2213    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2214    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2215    1     M     M     M     2     2     2 11/25/1994 11/25/1997
    ## 2216    1     M     M     *     2     2     1 11/30/1994 11/21/1997
    ## 2217    0     *    DS    DS     1    -1    -1 12/01/1994 11/21/1997
    ## 2218    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 2219    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2220    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2221    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2222    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2223    1     M     M     M     6    10    10 11/21/1994 11/28/1997
    ## 2224    1    ML    ML     M     2     4     3 11/21/1994 11/28/1997
    ## 2225    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2226    0     L     L    DS     1     1    -1 11/02/1994 11/26/1997
    ## 2227    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2228    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2229    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2230    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2231    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 2232    1     M     M     M     2     2     2 11/09/1994 12/17/1997
    ## 2233    0     *    DS    DS     1    -1    -1 11/14/1994 11/21/1997
    ## 2234    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 2235    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2236    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 2237    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2238    1     *     *     M     1     1     2 11/11/1994 11/25/1997
    ## 2239    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2240    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2241    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2242    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2243    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2244    1     M     M     M     3     4     4 11/22/1994 11/13/1997
    ## 2245    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2246    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2247    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 2248    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2249    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 2250    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 2251    0     L    DC    DT     1    -1    -1 11/25/1994 11/25/1997
    ## 2252    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 2253    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 2254    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 2255    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 2256    1    ML     R     *     2     1     1 12/02/1994 11/25/1997
    ## 2257    1     *     *     R     1     1     1 12/05/1994 11/26/1997
    ## 2258    1     M     M     M     6     6     6 12/05/1994 11/26/1997
    ## 2259    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 2260    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 2261    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 2262    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 2263    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 2264    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2265    1     *     M     M     1     2     2 11/22/1994 11/17/1997
    ## 2266    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 2267    1    ML    ML    ML     3     3     3 11/24/1994 11/19/1997
    ## 2268    2     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 2269    1     M     M     M     2     2     2 11/30/1994 11/20/1997
    ## 2270    1     M     M     M     3     2     3 12/01/1994 11/20/1997
    ## 2271    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2272    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2273    1     M     M     M     2     2     2 11/07/1994 11/14/1997
    ## 2274    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 2275    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 2276    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2277    0     M     *    DS     2     1    -1 11/09/1994 11/17/1997
    ## 2278    0     *    DC    DN     1    -1    -1 11/14/1994 12/17/1997
    ## 2279    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 2280    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2281    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2282    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2283    1     M     M     M     2     2     2 11/04/1994 11/13/1997
    ## 2284    1     M     *     *     2     1     1 11/09/1994 11/24/1997
    ## 2285    0     M     R    DS     2     1    -1 11/11/1994 11/25/1997
    ## 2286    2     M     *     *     2     1     1 11/14/1994 11/26/1997
    ## 2287    1     *     *     M     1     1     2 11/14/1994 11/26/1997
    ## 2288    0     L    DC    DC     1    -1    -1 11/15/1994 11/27/1997
    ## 2289    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2290    1     L     L     L     1     1     1 11/22/1994 11/13/1997
    ## 2291    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2292    1     *     M     M     2     2     2 11/23/1994 11/14/1997
    ## 2293    1     M     *     *     2     1     1 11/24/1994 11/14/1997
    ## 2294    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 2295    1     *    MR     *     1     4     1 12/05/1994 11/25/1997
    ## 2296    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2297    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2298    1     *     M    ML     1     2     3 11/08/1994 11/12/1997
    ## 2299    0     *     *    DS     1     1    -1 11/08/1994 11/14/1997
    ## 2300    3     M     M     M     3     2     2 11/08/1994 11/14/1997
    ## 2301    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2302    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2303    1    ML     L     M     3     1     3 11/16/1994 11/20/1997
    ## 2304    1     M     *     M     2     1     2 11/16/1994 11/20/1997
    ## 2305    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 2306    1     *     M     M     3     2     3 11/25/1994 11/24/1997
    ## 2307    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 2308    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2309    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 2310    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 2311    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 2312    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2313    1    ML     M     M     2     2     2 11/10/1994 11/14/1997
    ## 2314    1     M     *     *     2     1     1 11/14/1994 11/19/1997
    ## 2315    1     M     M     M     2     4     2 11/14/1994 11/20/1997
    ## 2316    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2317    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2318    0     *     *    DS     1     1    -1 11/15/1994 11/21/1997
    ## 2319    1     M     M     M     3     3     3 11/16/1994 11/21/1997
    ## 2320    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 2321    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2322    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 2323    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 2324    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2325    1     M     M     M     3     3     3 12/02/1994 11/21/1997
    ## 2326    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2327    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2328    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 2329    1     *     *     M     1     1     2 11/14/1994 12/17/1997
    ## 2330    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 2331    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 2332    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 2333    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 2334    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 2335    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2336    1    ML     M     M     4     4     4 11/04/1994 11/13/1997
    ## 2337    2     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 2338    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 2339    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2340    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2341    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 2342    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2343    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2344    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2345    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2346    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2347    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 2348    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2349    1     M    MR     M     2     2     2 11/07/1994 11/11/1997
    ## 2350    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2351    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2352    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 2353    1     M     M     M     6     5     6 11/08/1994 11/13/1997
    ## 2354    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2355    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2356    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 2357    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 2358    1     *     M     M     1     3     2 11/09/1994 11/13/1997
    ## 2359    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2360    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2361    1    ML    ML    ML     2     2     2 11/14/1994 11/19/1997
    ## 2362    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2363    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2364    1     *     *     *    10     1     1 11/17/1994 11/25/1997
    ## 2365    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 2366    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 2367    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 2368    1     *     *     R     1     1     1 11/23/1994 11/18/1997
    ## 2369    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2370    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2371    1     M     M     M     2     2     2 11/24/1994 11/19/1997
    ## 2372    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2373    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 2374    1     M     M     M     2     3     3 11/08/1994 11/14/1997
    ## 2375    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 2376    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2377    1     M     M     M     3     2     2 11/09/1994 11/17/1997
    ## 2378    1     M     M     M     2     2     3 11/09/1994 12/17/1997
    ## 2379    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 2380    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2381    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2382    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2383    1    ML    ML    ML     3     3     2 11/21/1994 11/27/1997
    ## 2384    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 2385    2     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 2386    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 2387    1     L     L     L     1     1     1 11/11/1994 11/24/1997
    ## 2388    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2389    2     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2390    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 2391    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2392    1     M     *     *     2     1     1 11/17/1994 11/12/1997
    ## 2393    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2394    0     L    DS    DC     2    -1    -1 11/18/1994 11/12/1997
    ## 2395    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2396    1     M     M     M     2     2     2 11/29/1994 11/17/1997
    ## 2397    0     *    DS    DC     1    -1    -1 11/29/1994 11/17/1997
    ## 2398    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 2399    1     M     M     M     2     2     2 12/01/1994 11/19/1997
    ## 2400    0     *     *    DS     1     1    -1 12/02/1994 11/24/1997
    ## 2401    1     M     M     M     2     2     2 11/22/1994 11/13/1997
    ## 2402    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2403    2     *     R     *     1    -1     1 11/08/1994 11/12/1997
    ## 2404    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 2405    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2406    1     M     M     M     4     6     9 11/09/1994 11/17/1997
    ## 2407    1    ML     M     M     4     5     4 11/09/1994 11/17/1997
    ## 2408    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2409    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2410    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2411    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2412    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2413    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2414    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 2415    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 2416    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 2417    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 2418    1     M     M     M     2     2     2 11/25/1994 11/25/1997
    ## 2419    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 2420    1     M     M     M     4     3     3 11/30/1994 11/21/1997
    ## 2421    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 2422    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 2423    1    ML    ML    ML     4     4     4 12/05/1994 11/26/1997
    ## 2424    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 2425    1     M     M     M     4     4     6 11/02/1994 11/11/1997
    ## 2426    1     L    ML     M     1     7     8 11/09/1994 11/13/1997
    ## 2427    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2428    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 2429    0    ML    DN    DN     5    -1    -1 11/17/1994 11/25/1997
    ## 2430    1     M     *     *     2     1     1 11/18/1994 11/26/1997
    ## 2431    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2432    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 2433    0     *     *    DS     1     1    -1 11/22/1994 11/17/1997
    ## 2434    1     M     M     M     2     3     3 11/22/1994 11/17/1997
    ## 2435    1     M    MR     M     3     2     2 11/23/1994 11/18/1997
    ## 2436    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2437    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 2438    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2439    1     M     M     M     2     3     3 11/02/1994 11/26/1997
    ## 2440    1     M     M     M     3     3     3 11/07/1994 11/13/1997
    ## 2441    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2442    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 2443    0     *    DS    DS     1    -1    -1 11/09/1994 11/17/1997
    ## 2444    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2445    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 2446    0     *     *    DS     1     1    -1 11/15/1994 11/21/1997
    ## 2447    1     M     M     M     2     2     2 11/16/1994 11/24/1997
    ## 2448    1     M     *     L     4     1     1 11/18/1994 11/27/1997
    ## 2449    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2450    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2451    1     M     R     *     3     1     1 11/21/1994 11/27/1997
    ## 2452    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2453    1     M     *     *     2     1     1 11/08/1994 11/20/1997
    ## 2454    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 2455    1    ML    MR     M     8     6     4 11/09/1994 11/21/1997
    ## 2456    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 2457    1     L     L    ML     1     1     2 11/14/1994 11/26/1997
    ## 2458    1     *     M     M     3     3     3 11/16/1994 11/27/1997
    ## 2459    0     *     R    DC     1     1    -1 11/16/1994 11/27/1997
    ## 2460    1     *     M     M     3     3     3 11/17/1994 11/12/1997
    ## 2461    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2462    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 2463    1     *     M     M     2     2     2 11/29/1994 11/17/1997
    ## 2464    1     L     L    ML     1     1     2 12/01/1994 11/18/1997
    ## 2465    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2466    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 2467    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2468    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 2469    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2470    1    ML     M     M     3     2     2 11/09/1994 11/17/1997
    ## 2471    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2472    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2473    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2474    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2475    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2476    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 2477    1    ML    ML    ML     2     3     4 12/06/1994 11/26/1997
    ## 2478    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 2479    0     *     R    DS     1     1    -1 11/23/1994 11/18/1997
    ## 2480    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 2481    0     *    DS    DC     1    -1    -1 11/09/1994 11/13/1997
    ## 2482    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2483    1     M     M     M     2     2     2 11/10/1994 11/14/1997
    ## 2484    1     M     M     M     2     2     2 11/11/1994 11/18/1997
    ## 2485    1     M     M     *     2     2     1 11/14/1994 11/19/1997
    ## 2486    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2487    1     M     M     M     3     3     3 11/15/1994 11/21/1997
    ## 2488    0     *    DC    DC     1    -1    -1 11/15/1994 11/21/1997
    ## 2489    1     M     M     M     2     2     2 11/16/1994 11/21/1997
    ## 2490    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2491    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2492    1     M     M     M     3     3     3 11/18/1994 11/27/1997
    ## 2493    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 2494    1     M     M     R     2     2     1 11/22/1994 11/17/1997
    ## 2495    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 2496    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 2497    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 2498    1    ML     M     M     3     2     2 12/01/1994 11/21/1997
    ## 2499    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2500    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2501    2     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2502    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 2503    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 2504    1     *     *     M     1     1     2 11/07/1994 11/14/1997
    ## 2505    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2506    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 2507    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 2508    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2509    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 2510    3     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2511    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2512    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 2513    1     *     L     *     1     1     1 11/21/1994 11/27/1997
    ## 2514    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 2515    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2516    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2517    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 2518    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 2519    1     M     M     M     2     3     2 11/09/1994 11/24/1997
    ## 2520    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2521    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2522    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2523    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2524    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 2525    1     M     M     M     3     2     2 11/16/1994 11/27/1997
    ## 2526    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 2527    1     *     R     *     1     1     1 11/17/1994 11/12/1997
    ## 2528    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2529    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2530    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2531    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2532    0     *     R    DC     1    -1    -1 11/23/1994 11/14/1997
    ## 2533    1     M     M     M     2     3     2 11/24/1994 11/17/1997
    ## 2534    1     M     M     M     4     3     3 11/24/1994 11/17/1997
    ## 2535    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 2536    0     *     L    DN     1     1    -1 12/05/1994 11/25/1997
    ## 2537    0     *    DS    DC     1    -1    -1 11/07/1994 11/11/1997
    ## 2538    1     *     *     R     1     1     1 11/08/1994 11/12/1997
    ## 2539    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 2540    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2541    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2542    1    ML    ML    ML     3     2     6 11/14/1994 11/17/1997
    ## 2543    1     M     *     M     3     1     5 11/14/1994 11/17/1997
    ## 2544    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2545    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 2546    1     M     M     M     2     2     2 11/21/1994 11/20/1997
    ## 2547    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2548    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 2549    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 2550    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2551    0     *    DC    DC     1    -1    -1 12/02/1994 11/24/1997
    ## 2552    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 2553    1     M     *     M     2     1     2 12/02/1994 11/25/1997
    ## 2554    1     L    ML     R     2     2     1 12/02/1994 11/25/1997
    ## 2555    2     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 2556    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2557    1     M     M     M     3     2     2 11/02/1994 11/11/1997
    ## 2558    1     *    ML     M     1     2     2 11/07/1994 11/12/1997
    ## 2559    1     M     M     L     2     2     1 11/09/1994 11/13/1997
    ## 2560    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2561    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 2562    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2563    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2564    0     M    DS    DC     2    -1    -1 11/18/1994 11/27/1997
    ## 2565    2     M     M     R     2     2     1 11/17/1994 11/25/1997
    ## 2566    1     M     M     M     2     2     2 11/18/1994 11/26/1997
    ## 2567    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 2568    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2569    1     M     *     *     2     1     1 12/01/1994 11/20/1997
    ## 2570    1    ML    ML     M     2     2     2 12/01/1994 11/21/1997
    ## 2571    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2572    1     M     M     M     2     2     2 12/02/1994 11/21/1997
    ## 2573    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 2574    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2575    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2576    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 2577    1     M     M     M     2     2     2 11/09/1994 12/17/1997
    ## 2578    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2579    1     M     *     M     2     1     2 11/14/1994 12/17/1997
    ## 2580    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 2581    0     *    DS    DC     1    -1    -1 11/14/1994 11/21/1997
    ## 2582    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2583    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2584    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2585    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2586    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 2587    1     M     M     M     2     2     2 11/21/1994 11/27/1997
    ## 2588    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 2589    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2590    1     M     M     M     4     3     4 11/04/1994 11/13/1997
    ## 2591    0     *    DC    DC     1    -1    -1 11/04/1994 11/13/1997
    ## 2592    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 2593    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2594    1     M     M     M     2     2     2 11/16/1994 11/27/1997
    ## 2595    1     *     R     *     1     1     1 11/17/1994 11/12/1997
    ## 2596    2     L     R     *     1    -1     1 11/18/1994 11/12/1997
    ## 2597    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2598    2     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2599    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 2600    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 2601    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 2602    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2603    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2604    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2605    1     M     M     M     6     4     4 12/01/1994 11/19/1997
    ## 2606    1     *     *     *     3     1     1 12/02/1994 11/24/1997
    ## 2607    0     L    DC    DN     1    -1    -1 12/02/1994 11/24/1997
    ## 2608    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2609    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 2610    1     M     M     M     3     2     4 11/09/1994 11/17/1997
    ## 2611    2     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2612    1     *     L     L     1     1     1 11/14/1994 11/17/1997
    ## 2613    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2614    1     M     M     M     2     3     3 11/16/1994 11/18/1997
    ## 2615    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2616    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2617    1     M     M     M     2     2     2 11/16/1994 11/18/1997
    ## 2618    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2619    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 2620    1     M     *     *     2     1     1 11/25/1994 11/25/1997
    ## 2621    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 2622    2     M     M     R    10     2     1 11/17/1994 11/25/1997
    ## 2623    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 2624    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 2625    1    ML     M     M     3     3     3 11/07/1994 11/12/1997
    ## 2626    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2627    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2628    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2629    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2630    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2631    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2632    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 2633    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 2634    1     M    MR     M     2     4     4 11/21/1994 11/28/1997
    ## 2635    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 2636    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 2637    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 2638    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2639    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 2640    1     M     *     M     2     1     2 12/01/1994 11/21/1997
    ## 2641    1     M     M     M     3     2     2 12/01/1994 11/21/1997
    ## 2642    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2643    1     M     M     M     2     2     2 12/02/1994 11/21/1997
    ## 2644    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2645    1     M     *     M     2     1     2 11/02/1994 11/26/1997
    ## 2646    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2647    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2648    1     *     *     M     1     1     2 11/08/1994 11/14/1997
    ## 2649    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2650    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 2651    0     *    DN    DN     1    -1    -1 11/14/1994 11/21/1997
    ## 2652    0     *    DS    DC     1    -1    -1 11/15/1994 11/21/1997
    ## 2653    1     M     *     *     2     1     1 11/15/1994 11/21/1997
    ## 2654    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2655    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2656    1     M     M     M     2     2     2 11/16/1994 11/24/1997
    ## 2657    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 2658    1     M     S     M     4    -9     3 11/18/1994 11/27/1997
    ## 2659    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 2660    1     M     M     M     2     2     2 11/07/1994 11/19/1997
    ## 2661    2     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 2662    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 2663    0     M    DC    DC     2    -1    -1 11/09/1994 11/21/1997
    ## 2664    1     M     M     M     2     2     3 11/11/1994 11/24/1997
    ## 2665    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 2666    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2667    1     M     M     M     2     2     2 11/15/1994 11/26/1997
    ## 2668    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2669    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2670    1     *     R     *     1     1     1 11/23/1994 11/14/1997
    ## 2671    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2672    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2673    2     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 2674    1     M     *     *     2     1     1 11/29/1994 11/17/1997
    ## 2675    1     M     M     M     3     3     2 12/01/1994 11/18/1997
    ## 2676    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2677    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 2678    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 2679    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2680    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2681    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2682    2     *     R     *     1    -1     1 11/08/1994 11/12/1997
    ## 2683    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2684    1     *     M     M    13    10    18 11/14/1994 11/17/1997
    ## 2685    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2686    1     M     M     M     2     2     2 11/16/1994 11/20/1997
    ## 2687    1     *     M     M     1     2     2 11/21/1994 11/20/1997
    ## 2688    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 2689    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 2690    1     *     *     M     1     1     2 11/23/1994 11/24/1997
    ## 2691    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2692    1     M     M     M     2     2     2 11/30/1994 11/21/1997
    ## 2693    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 2694    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 2695    1     M     M     M     4     4     4 12/02/1994 11/25/1997
    ## 2696    1     *     *     *     2     1     1 12/02/1994 11/25/1997
    ## 2697    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 2698    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 2699    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 2700    1     L     *     *     1     1     1 11/09/1994 11/13/1997
    ## 2701    2     *     C     R     1    -9     1 11/09/1994 11/13/1997
    ## 2702    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 2703    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 2704    1     M     M     M     9     6     8 11/14/1994 11/20/1997
    ## 2705    1     M     M     *     7     5     1 11/17/1994 11/25/1997
    ## 2706    1     M     M     M     3     3     2 11/18/1994 11/27/1997
    ## 2707    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2708    2     *     M     M     2     2     2 11/22/1994 11/17/1997
    ## 2709    2     *     M     M     2     2     2 11/23/1994 11/18/1997
    ## 2710    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2711    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2712    2     *    ML     L     1     2     1 11/24/1994 11/19/1997
    ## 2713    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 2714    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 2715    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 2716    1     M     *     M     2     1     2 11/07/1994 11/13/1997
    ## 2717    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 2718    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2719    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 2720    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 2721    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2722    1     M     M     M     6     6     3 11/18/1994 11/27/1997
    ## 2723    1     *     R     *     1     1     1 11/04/1994 11/13/1997
    ## 2724    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 2725    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 2726    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2727    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 2728    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2729    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2730    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2731    1     M     M     M     3     2     2 11/16/1994 11/27/1997
    ## 2732    1     *     M     M     3     3     3 11/16/1994 11/27/1997
    ## 2733    0     L    DC    DN     1    -1    -1 11/18/1994 11/12/1997
    ## 2734    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2735    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2736    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2737    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 2738    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 2739    1     M     M     M     6     5     6 11/29/1994 11/17/1997
    ## 2740    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 2741    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 2742    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2743    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 2744    0     *    DS    DS     1    -1    -1 12/01/1994 11/19/1997
    ## 2745    1     L     *     L     1     1     1 11/24/1994 11/14/1997
    ## 2746    2     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2747    1    ML     M    MR     4     3     2 11/07/1994 11/11/1997
    ## 2748    1     M     M     M     2     2     2 11/08/1994 11/12/1997
    ## 2749    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2750    1     *     M     *     1     2     1 11/14/1994 11/17/1997
    ## 2751    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2752    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2753    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2754    1     *    ML    ML     1     2     2 11/16/1994 11/19/1997
    ## 2755    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 2756    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 2757    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 2758    1     M     M     *     2     2     1 11/23/1994 11/24/1997
    ## 2759    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2760    1     M     M     M     4     4     4 11/25/1994 11/24/1997
    ## 2761    0     *    DC    DC     1    -1    -1 11/25/1994 11/25/1997
    ## 2762    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 2763    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 2764    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2765    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2766    1     M    MR     M     3     4     4 11/02/1994 11/11/1997
    ## 2767    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2768    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2769    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2770    1     M     M     M     2     3     2 11/14/1994 11/19/1997
    ## 2771    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 2772    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 2773    1     M     M     M     2     3     2 11/15/1994 11/20/1997
    ## 2774    0     *    DC    DC     1    -1    -1 11/15/1994 11/20/1997
    ## 2775    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 2776    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2777    1     M     M     M     2     2     2 11/16/1994 11/21/1997
    ## 2778    2     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2779    1     M     M     *     3     2     1 11/17/1994 11/25/1997
    ## 2780    1     L    ML     *     2     2     1 11/17/1994 11/25/1997
    ## 2781    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2782    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 2783    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 2784    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 2785    1     *     M     M     1     2     2 12/01/1994 11/20/1997
    ## 2786    1     M     M     M     3     4     3 12/02/1994 11/21/1997
    ## 2787    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2788    1     *     *     M     1     1     2 11/07/1994 11/14/1997
    ## 2789    1     *     M    ML     1     2     2 11/08/1994 11/14/1997
    ## 2790    1     M     M     M     2     2     2 11/08/1994 11/14/1997
    ## 2791    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2792    2     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2793    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2794    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2795    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2796    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2797    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2798    1     M    ML    ML     2     2     2 11/04/1994 11/13/1997
    ## 2799    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 2800    1     M     M     M     2     2     2 11/09/1994 11/21/1997
    ## 2801    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 2802    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 2803    1    ML    MR     M     7     4     6 11/11/1994 11/25/1997
    ## 2804    1     M     M     *     3     2     1 11/11/1994 11/25/1997
    ## 2805    2     L     R     *     1    -1     1 11/14/1994 11/26/1997
    ## 2806    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2807    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2808    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 2809    1     L    ML     L     2     2     1 11/18/1994 11/12/1997
    ## 2810    1     M     M     M     2     2     2 11/18/1994 11/12/1997
    ## 2811    1     M     M     M     6     3     4 11/22/1994 11/13/1997
    ## 2812    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2813    1     M     *     *     2     1     1 11/23/1994 11/13/1997
    ## 2814    0     *    DC    DN     1    -1    -1 11/23/1994 11/14/1997
    ## 2815    1     M     M     M     2     2     2 11/24/1994 11/17/1997
    ## 2816    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 2817    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 2818    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 2819    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 2820    1     M     M     *     5     4     1 11/07/1994 11/11/1997
    ## 2821    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 2822    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2823    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2824    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2825    0     *    DS    DS     1    -1    -1 11/14/1994 11/17/1997
    ## 2826    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2827    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2828    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2829    2     M     M     R     3     3     1 11/16/1994 11/20/1997
    ## 2830    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 2831    1     M     *     M     2     1     2 11/23/1994 11/24/1997
    ## 2832    0     *     *    DN     1     1    -1 11/25/1994 11/24/1997
    ## 2833    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2834    1     M     M     M     3     3     3 11/25/1994 11/24/1997
    ## 2835    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 2836    2     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 2837    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 2838    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 2839    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 2840    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2841    1     M     M     M     2     2     2 12/01/1994 11/21/1997
    ## 2842    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 2843    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 2844    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 2845    1     M     M     M     2     3     2 11/07/1994 11/12/1997
    ## 2846    2     *     M     M     2     2     2 11/10/1994 11/14/1997
    ## 2847    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 2848    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2849    1     *     *     M     1     1     2 11/11/1994 11/18/1997
    ## 2850    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2851    1     *     M     M     1     2     3 11/11/1994 11/18/1997
    ## 2852    1     *     M     M     1     2     2 11/14/1994 11/19/1997
    ## 2853    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 2854    0     *     *    DS     1     1    -1 11/14/1994 11/20/1997
    ## 2855    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 2856    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2857    2     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 2858    1     M     M     M     2     3     2 11/18/1994 11/27/1997
    ## 2859    1     M     *     *     2     1     1 12/01/1994 11/20/1997
    ## 2860    2     *     *     R     1     1     1 12/01/1994 11/20/1997
    ## 2861    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 2862    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2863    1     M     *     *     3     1     1 12/01/1994 11/21/1997
    ## 2864    1     M     M     M     2     2     2 12/02/1994 11/21/1997
    ## 2865    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2866    1     M     *     *     2     1     1 11/02/1994 11/26/1997
    ## 2867    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 2868    2    ML     *     R     2     1     1 11/07/1994 11/14/1997
    ## 2869    0     *     *    DS     1     1    -1 11/07/1994 11/14/1997
    ## 2870    1     M     *     M     3     1     2 11/08/1994 11/17/1997
    ## 2871    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 2872    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2873    1     L     *     *     1     1     1 11/14/1994 11/21/1997
    ## 2874    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 2875    2    ML     M     R     4     4     1 11/15/1994 11/21/1997
    ## 2876    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 2877    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 2878    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2879    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 2880    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2881    0     M     M    DS     2     2    -1 11/04/1994 11/13/1997
    ## 2882    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 2883    0     *    DS    DS     1    -1    -1 11/04/1994 11/13/1997
    ## 2884    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 2885    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 2886    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 2887    1     *     *     M     1     1     3 11/11/1994 11/25/1997
    ## 2888    0    ML    DS    DC     2    -1    -1 11/11/1994 11/25/1997
    ## 2889    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2890    1     *     M     M     1     2     2 11/16/1994 11/27/1997
    ## 2891    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 2892    0     *     *    DS     1     1    -1 11/18/1994 11/12/1997
    ## 2893    0     *    DS    DC     1    -1    -1 11/18/1994 11/12/1997
    ## 2894    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 2895    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2896    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2897    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 2898    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 2899    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 2900    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 2901    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 2902    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2903    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2904    1     M     M     M     6     7     3 12/01/1994 11/19/1997
    ## 2905    0     L    DS    DC     1    -1    -1 11/22/1994 11/13/1997
    ## 2906    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2907    1     *     M     M     1     4     5 11/11/1994 11/17/1997
    ## 2908    2     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 2909    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 2910    1     M     M     M     5     7     8 11/14/1994 11/17/1997
    ## 2911    1     M     *     *     2     1     1 11/14/1994 11/17/1997
    ## 2912    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2913    1     *     M     M     1     2     2 11/14/1994 11/17/1997
    ## 2914    0     *     R    DC     1    -1    -1 11/16/1994 11/19/1997
    ## 2915    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2916    2     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2917    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 2918    0     *     R    DN     1    -1    -1 11/16/1994 11/20/1997
    ## 2919    1     *     M     M     1     2     2 11/23/1994 11/19/1997
    ## 2920    1     M     M     M     2     2     2 11/23/1994 11/24/1997
    ## 2921    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 2922    1     M     M     M     3     3     2 11/25/1994 11/25/1997
    ## 2923    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 2924    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 2925    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 2926    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 2927    1     M     M     M     4     4     4 11/02/1994 11/11/1997
    ## 2928    1     M     M     M     4     4     4 11/07/1994 11/12/1997
    ## 2929    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 2930    1     M     M     M     2     2     2 11/07/1994 11/12/1997
    ## 2931    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 2932    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 2933    2     *     *     R     1     1     1 11/14/1994 11/19/1997
    ## 2934    1     M     M     M     2     2     2 11/15/1994 11/20/1997
    ## 2935    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 2936    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 2937    0     *     *    DC     1     1    -1 11/17/1994 11/25/1997
    ## 2938    1    ML    ML     M     4     3     5 11/18/1994 11/26/1997
    ## 2939    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 2940    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 2941    1     *     *     M     1     1     2 11/23/1994 11/18/1997
    ## 2942    2     *     M     *     2     2     1 11/23/1994 11/18/1997
    ## 2943    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2944    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 2945    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 2946    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 2947    1     M     *     *     4     1     1 11/02/1994 11/26/1997
    ## 2948    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 2949    1     M     M     M     2     2     2 11/08/1994 11/14/1997
    ## 2950    1     M     M     M     2     2     2 11/09/1994 11/17/1997
    ## 2951    1     M     M     M     3     2     2 11/09/1994 11/17/1997
    ## 2952    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 2953    1     M     M     M     2     2     2 11/14/1994 12/17/1997
    ## 2954    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 2955    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2956    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2957    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2958    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2959    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 2960    1     M     M     *     4     2     1 11/18/1994 11/27/1997
    ## 2961    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 2962    1     L     *     L     1     1     1 11/04/1994 11/12/1997
    ## 2963    1     *     M     M     1     2     2 11/04/1994 11/12/1997
    ## 2964    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 2965    1     M     M     M     3     3     3 11/04/1994 11/13/1997
    ## 2966    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 2967    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 2968    1     *     *     M     1     1     2 11/11/1994 11/25/1997
    ## 2969    1     *     *     M     1     1     2 11/14/1994 11/26/1997
    ## 2970    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 2971    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 2972    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 2973    1     M     *     M     2     1     2 11/17/1994 11/12/1997
    ## 2974    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 2975    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 2976    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 2977    1     M     M     M     2     2     2 11/24/1994 11/17/1997
    ## 2978    1     M     M     M     3     3     3 11/24/1994 11/17/1997
    ## 2979    1     M     M     M     4     4     4 11/24/1994 11/17/1997
    ## 2980    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 2981    1     M     M     M     2     2     2 12/01/1994 11/18/1997
    ## 2982    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2983    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2984    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 2985    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 2986    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 2987    1     L     L     L     1     1     1 11/08/1994 11/14/1997
    ## 2988    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 2989    1     M     M     M     2     2     2 11/09/1994 11/17/1997
    ## 2990    1     M     *     *     2     1     1 11/11/1994 11/17/1997
    ## 2991    1     M     *     M     2     1     2 11/14/1994 11/17/1997
    ## 2992    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2993    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2994    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2995    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 2996    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 2997    1    ML    ML     M     5     5     5 11/16/1994 11/18/1997
    ## 2998    1     M     *     *     2     1     1 11/16/1994 11/18/1997
    ## 2999    1     *     *     M     1     1     2 11/16/1994 11/18/1997
    ## 3000    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3001    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3002    0     *    DS    DS     1    -1    -1 11/16/1994 11/19/1997
    ## 3003    0     *    DC    DN     1    -1    -1 11/16/1994 11/20/1997
    ## 3004    1     M     M     M     2     2     2 11/16/1994 11/20/1997
    ## 3005    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 3006    1     M     M     R     2     2     1 11/23/1994 11/19/1997
    ## 3007    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 3008    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3009    1    ML    ML    ML     4     5     7 11/29/1994 11/25/1997
    ## 3010    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 3011    2     *     R     *     1    -1     1 11/30/1994 11/21/1997
    ## 3012    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 3013    1     M     M     M     3     3     3 12/02/1994 11/24/1997
    ## 3014    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3015    1     M     *     *     2     1     1 12/02/1994 11/25/1997
    ## 3016    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 3017    2     *     M     R     1     2     1 11/02/1994 11/11/1997
    ## 3018    1     *     *     M     1     1     3 11/18/1994 11/27/1997
    ## 3019    1    ML    MR     M     2     4     3 11/09/1994 11/13/1997
    ## 3020    1     *     M     M     1     2     2 11/10/1994 11/14/1997
    ## 3021    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3022    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3023    1     *     M     *     1     2     1 11/14/1994 11/19/1997
    ## 3024    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3025    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3026    1     *     M     M     1     2     2 11/16/1994 11/21/1997
    ## 3027    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3028    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3029    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3030    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3031    1     M     M     M     2     2     2 11/17/1994 11/25/1997
    ## 3032    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 3033    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 3034    0     *    DT    DN     1    -1    -1 11/21/1994 11/28/1997
    ## 3035    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3036    0     *    DC    DC     1    -1    -1 11/22/1994 11/17/1997
    ## 3037    1    ML     M     M     3     4     4 11/23/1994 11/18/1997
    ## 3038    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 3039    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 3040    1     *     L     *     1     1     1 12/01/1994 11/20/1997
    ## 3041    0     L    DS    DT     1    -1    -1 12/01/1994 11/20/1997
    ## 3042    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 3043    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3044    1     *     *     L     1     1     1 11/07/1994 11/14/1997
    ## 3045    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3046    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3047    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3048    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3049    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 3050    1     M     M     M     2     2     2 11/14/1994 12/17/1997
    ## 3051    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 3052    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3053    1     M     M     M     2     4     4 11/16/1994 11/24/1997
    ## 3054    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 3055    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3056    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3057    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3058    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3059    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3060    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 3061    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 3062    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 3063    1     M     *     *     2     1     1 11/08/1994 11/20/1997
    ## 3064    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3065    1     M     M     M     2     2     2 11/11/1994 11/25/1997
    ## 3066    0     L     R    DC     1    -1    -1 11/14/1994 11/26/1997
    ## 3067    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 3068    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 3069    1     M     M     M     3     3     4 11/17/1994 11/12/1997
    ## 3070    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3071    1     *     *     M     1     1     2 11/24/1994 11/14/1997
    ## 3072    1     M     M     M     2     2     2 11/24/1994 11/17/1997
    ## 3073    1     M     M     M     4     4     3 11/24/1994 11/17/1997
    ## 3074    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3075    1     M     M     M     2     2     2 11/24/1994 11/17/1997
    ## 3076    1     M     M     M     3     2     3 11/24/1994 11/17/1997
    ## 3077    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 3078    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 3079    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 3080    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3081    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 3082    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3083    0     *    DN    DN     1    -1    -1 12/05/1994 11/25/1997
    ## 3084    1     *     *     L     1     1     1 11/07/1994 11/11/1997
    ## 3085    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 3086    3     *     *     R     1     1     1 11/08/1994 11/12/1997
    ## 3087    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 3088    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 3089    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 3090    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 3091    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3092    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 3093    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 3094    1     L     L     L     1     1     1 11/09/1994 11/14/1997
    ## 3095    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3096    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 3097    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 3098    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 3099    1     L     *     L     1     1     1 11/25/1994 11/24/1997
    ## 3100    1     M     *     R     2     1     1 11/30/1994 11/21/1997
    ## 3101    2     M     *     R     2     1     1 11/17/1994 11/25/1997
    ## 3102    1     *     M     M     1     2     2 12/01/1994 11/21/1997
    ## 3103    1     *     M     M     1     2     3 12/01/1994 11/21/1997
    ## 3104    1     *     *     M     1     1     2 12/02/1994 11/24/1997
    ## 3105    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3106    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3107    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3108    1     M     M     M     2     2     2 12/06/1994 11/26/1997
    ## 3109    1     L    MR     M     3     4     5 11/25/1994 11/25/1997
    ## 3110    1     M     M     M     2     2     2 11/02/1994 11/11/1997
    ## 3111    2    ML     *     *     3     1     1 11/09/1994 11/13/1997
    ## 3112    1     M    MR     M     4     4     4 11/09/1994 11/13/1997
    ## 3113    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3114    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3115    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3116    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3117    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3118    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3119    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 3120    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 3121    0     *    DS    DC     1    -1    -1 11/15/1994 11/20/1997
    ## 3122    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3123    1     M    ML     R     3     2     1 11/18/1994 11/26/1997
    ## 3124    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3125    1     M     M     M     3     3     3 11/22/1994 11/17/1997
    ## 3126    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3127    0     L    DC    DN     1    -1    -1 11/23/1994 11/18/1997
    ## 3128    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3129    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3130    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 3131    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 3132    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 3133    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 3134    2     *     M     R     1     5     1 12/02/1994 11/24/1997
    ## 3135    1     *     M     M     5     5     4 12/02/1994 11/24/1997
    ## 3136    1     M     M     M     2     2     2 11/02/1994 11/26/1997
    ## 3137    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3138    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3139    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3140    1     M     M     M     2     3     3 11/07/1994 11/13/1997
    ## 3141    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3142    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3143    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3144    0     *     *    DS     1     1    -1 11/08/1994 11/17/1997
    ## 3145    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3146    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3147    1     M     M     M     2     2     2 11/09/1994 11/17/1997
    ## 3148    1     M     M     M     2     2     2 11/09/1994 11/17/1997
    ## 3149    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 3150    2     L     R     *     1    -1     1 11/21/1994 11/27/1997
    ## 3151    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 3152    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3153    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3154    2     L     L     R     1     1     1 11/16/1994 11/24/1997
    ## 3155    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 3156    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3157    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3158    1     *     M     M     1     2     2 11/18/1994 11/27/1997
    ## 3159    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3160    0     *    DT    DN     1    -1    -1 11/21/1994 11/27/1997
    ## 3161    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3162    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 3163    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 3164    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 3165    2     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3166    2     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3167    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3168    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3169    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 3170    1     M     M     M     2     2     2 11/11/1994 11/24/1997
    ## 3171    1     *     M     M     2     2     2 11/11/1994 11/25/1997
    ## 3172    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 3173    2     *     *     R     1     1     1 11/15/1994 11/26/1997
    ## 3174    1     M     M     M     3     2     2 11/16/1994 11/27/1997
    ## 3175    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3176    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3177    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3178    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3179    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3180    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 3181    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 3182    1     *     *     L     1     1     1 11/29/1994 11/17/1997
    ## 3183    1     M     M     M     3     3     3 11/29/1994 11/17/1997
    ## 3184    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 3185    1    ML     *     *     2     1     1 11/29/1994 11/18/1997
    ## 3186    1     M     M     M     2     2     2 11/29/1994 11/18/1997
    ## 3187    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3188    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3189    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3190    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 3191    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 3192    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 3193    1     *     *     *     1     1     1 12/02/1994 11/19/1997
    ## 3194    1     M     M     M     2     2     2 12/02/1994 11/24/1997
    ## 3195    1     *     R     *     1     1     1 12/05/1994 11/25/1997
    ## 3196    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 3197    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 3198    0     *    DC    DC     1    -1    -1 11/08/1994 11/14/1997
    ## 3199    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3200    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 3201    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 3202    1    ML    ML     M     2     2     2 11/14/1994 11/17/1997
    ## 3203    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3204    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 3205    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 3206    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 3207    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 3208    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 3209    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 3210    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 3211    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3212    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 3213    1     *     *     R     1     1     1 11/25/1994 11/25/1997
    ## 3214    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 3215    1     M     M     R     2     2     1 11/30/1994 11/21/1997
    ## 3216    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 3217    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 3218    0     L    DC    DN     1    -1    -1 12/01/1994 11/24/1997
    ## 3219    1     *     M     M     1     2     2 12/02/1994 11/24/1997
    ## 3220    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3221    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3222    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3223    1     *     M     M     1     2     2 11/02/1994 11/11/1997
    ## 3224    1     *     M    MR     1     4     2 11/02/1994 11/11/1997
    ## 3225    1     M     M     M     2     2     2 11/07/1994 11/12/1997
    ## 3226    1     *     M     M     1     2     2 11/07/1994 11/12/1997
    ## 3227    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3228    1     *     M     M     1     2     2 11/11/1994 11/18/1997
    ## 3229    0     *    DS    DS     1    -1    -1 11/11/1994 11/18/1997
    ## 3230    2     *     M     M     2     2     2 11/11/1994 11/18/1997
    ## 3231    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3232    2     *     M     M     1     2     2 11/14/1994 11/19/1997
    ## 3233    1    ML     L    LR     6     1     1 11/14/1994 11/20/1997
    ## 3234    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3235    3     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3236    2     M     *     *     2     1     1 11/15/1994 11/21/1997
    ## 3237    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3238    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3239    1     *     S     *     1    -1     1 11/16/1994 11/21/1997
    ## 3240    1     M     M     M     3     3     3 11/18/1994 11/26/1997
    ## 3241    0     *     R    DC     1    -1    -1 11/18/1994 11/26/1997
    ## 3242    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 3243    1     *     M     M     4     4     3 11/18/1994 11/27/1997
    ## 3244    1     *     M     M     2     2     2 11/18/1994 11/27/1997
    ## 3245    2     M     *     M     2     1     2 11/23/1994 11/18/1997
    ## 3246    1     M     M     M     4     3     4 11/24/1994 11/19/1997
    ## 3247    1     M     *     *     2     1     1 12/01/1994 11/20/1997
    ## 3248    1     M     M     M     2     2     3 12/01/1994 11/20/1997
    ## 3249    1     M     M     M     2     2     2 12/01/1994 11/20/1997
    ## 3250    1     M     M     M     2     2     3 12/01/1994 11/20/1997
    ## 3251    1     M     *     *     3     1     1 12/01/1994 11/21/1997
    ## 3252    1     *     *     M    42     1     4 12/02/1994 11/24/1997
    ## 3253    1     M     M     M     2     2     2 11/02/1994 11/26/1997
    ## 3254    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 3255    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 3256    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3257    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3258    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3259    1     M     M     M     2     2     2 11/14/1994 12/17/1997
    ## 3260    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3261    1     L     *     M     2     1     2 11/15/1994 11/21/1997
    ## 3262    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3263    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3264    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3265    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3266    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3267    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3268    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3269    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3270    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3271    1     M     M    MR     3     4     4 11/04/1994 11/12/1997
    ## 3272    1     *     L     R     1     1     1 11/04/1994 11/12/1997
    ## 3273    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 3274    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 3275    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3276    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3277    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3278    1     *     *     L     1     1     1 11/09/1994 11/21/1997
    ## 3279    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3280    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3281    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 3282    1     L     *     M     1     1     3 11/14/1994 11/26/1997
    ## 3283    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 3284    2     *     M     M     3     2     2 11/16/1994 11/27/1997
    ## 3285    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 3286    2     *     R     *     1    -1     1 11/16/1994 11/27/1997
    ## 3287    1     M     M     M     2     2     2 11/17/1994 11/12/1997
    ## 3288    0     *     *    DS     1     1    -1 11/18/1994 11/12/1997
    ## 3289    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3290    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3291    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 3292    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 3293    1     M     M     M     4     4     4 11/24/1994 11/14/1997
    ## 3294    1     *     M     M     5     4     3 11/24/1994 11/17/1997
    ## 3295    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3296    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 3297    1     L     M     M     4     2     3 11/29/1994 11/18/1997
    ## 3298    1    ML     M     M     2     2     2 12/01/1994 11/18/1997
    ## 3299    1     M     M    ML     2     2     2 12/01/1994 11/18/1997
    ## 3300    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 3301    1     M     M     M     2     2     2 12/02/1994 11/19/1997
    ## 3302    1     M     M     M     2     2     2 12/02/1994 11/24/1997
    ## 3303    1     *     M     M     1     2     2 12/02/1994 11/24/1997
    ## 3304    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3305    2     *     R     *     1     1     1 11/07/1994 11/11/1997
    ## 3306    1     *     L     *     1     1     1 11/07/1994 11/11/1997
    ## 3307    1     M     M     M     3     3     3 11/07/1994 11/11/1997
    ## 3308    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3309    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3310    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3311    0     *    DN    DN     1    -1    -1 11/14/1994 11/17/1997
    ## 3312    1     M     M     *     2     2     1 11/14/1994 11/17/1997
    ## 3313    1     L     C     L     1    -9     1 11/16/1994 11/19/1997
    ## 3314    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3315    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 3316    2     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 3317    2     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3318    1     *     M     M     1     2     3 11/25/1994 11/25/1997
    ## 3319    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 3320    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 3321    0     M     M    DS     2     2    -1 12/01/1994 11/24/1997
    ## 3322    0     M    DS    DC     2    -1    -1 12/01/1994 11/24/1997
    ## 3323    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3324    1     *     *     R     1     1     1 12/02/1994 11/25/1997
    ## 3325    1     L     L     L     1     1     1 12/06/1994 11/26/1997
    ## 3326    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3327    1    ML     M     M     3     5     5 11/09/1994 11/13/1997
    ## 3328    1     L    MR     M     1     2     2 11/09/1994 11/13/1997
    ## 3329    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3330    1     M     M     M     3     4     4 11/11/1994 11/18/1997
    ## 3331    1     L     M     M     1     2     2 11/14/1994 11/19/1997
    ## 3332    1     *     *     M     1     1     2 11/14/1994 11/19/1997
    ## 3333    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3334    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 3335    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3336    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3337    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3338    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3339    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3340    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3341    2     *     *     R     1     1     1 11/17/1994 11/25/1997
    ## 3342    1     M     M     M     6     5     5 11/18/1994 11/27/1997
    ## 3343    1     *     *     L     1     1     1 11/22/1994 11/17/1997
    ## 3344    1     *     M     M     1     2     2 11/22/1994 11/17/1997
    ## 3345    2     L     *     R     2     1     1 11/22/1994 11/17/1997
    ## 3346    1     M     M     M     3     3     3 11/23/1994 11/18/1997
    ## 3347    1     M     M     M     2     2     2 11/23/1994 11/18/1997
    ## 3348    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3349    0     *    DC    DC     1    -1    -1 11/23/1994 11/18/1997
    ## 3350    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3351    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 3352    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 3353    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 3354    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 3355    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3356    1     *     M     M     6     2     7 12/02/1994 11/24/1997
    ## 3357    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3358    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3359    1     M     M     M     3    11     3 12/02/1994 11/24/1997
    ## 3360    1     *     M     M    10     9     9 12/02/1994 11/24/1997
    ## 3361    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3362    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3363    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3364    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 3365    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 3366    1     M     M     M     2     2     2 11/07/1994 11/13/1997
    ## 3367    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3368    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3369    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3370    1     M     M     M     2     2     2 11/07/1994 11/14/1997
    ## 3371    1    ML    ML    ML     2     2     2 11/07/1994 11/14/1997
    ## 3372    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3373    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3374    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3375    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3376    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3377    0     *    DS    DC     1    -1    -1 11/09/1994 11/17/1997
    ## 3378    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3379    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 3380    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 3381    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 3382    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 3383    0     *     L    DC     1     1    -1 11/14/1994 11/21/1997
    ## 3384    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3385    1     M     *     *     2     1     1 11/15/1994 11/21/1997
    ## 3386    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3387    1     M     *     *     2     1     1 11/18/1994 11/27/1997
    ## 3388    2     *     *     R     1     1     1 11/18/1994 11/27/1997
    ## 3389    0     M    DC    DN     2    -1    -1 11/18/1994 11/27/1997
    ## 3390    1     M     *     *     2     1     1 11/18/1994 11/27/1997
    ## 3391    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3392    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3393    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3394    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 3395    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 3396    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 3397    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3398    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3399    1     M     *     *     2     1     1 11/09/1994 11/24/1997
    ## 3400    0     *    DS    DN     1    -1    -1 11/14/1994 11/26/1997
    ## 3401    1     L     *    ML     1     1     2 11/14/1994 11/26/1997
    ## 3402    1     *     L    ML     1     1     2 11/14/1994 11/26/1997
    ## 3403    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 3404    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 3405    0     *     R    DS     1    -1    -1 11/16/1994 11/27/1997
    ## 3406    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3407    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3408    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3409    0     *    DS    DC     1    -1    -1 11/18/1994 11/12/1997
    ## 3410    0     M     *    DS     2     1    -1 11/18/1994 11/12/1997
    ## 3411    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3412    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 3413    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 3414    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3415    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 3416    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3417    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 3418    1     M     M     M     3     3     3 12/01/1994 11/18/1997
    ## 3419    1     *     M     M     1     2     2 12/01/1994 11/18/1997
    ## 3420    0     *     *    DS     1     1    -1 12/02/1994 11/24/1997
    ## 3421    0     *    DS    DS     1    -1    -1 12/02/1994 11/24/1997
    ## 3422    2     *     N     R     1    -9     1 12/05/1994 11/25/1997
    ## 3423    1     *     *     R     1     1     1 11/07/1994 11/11/1997
    ## 3424    1     M     M     M     3     3     3 11/08/1994 11/12/1997
    ## 3425    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 3426    1    ML     *    ML     2     1     3 11/08/1994 11/13/1997
    ## 3427    1     M     M     M     3     3     4 11/08/1994 11/14/1997
    ## 3428    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3429    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3430    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3431    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 3432    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 3433    1     M     *     R     2     1     1 11/23/1994 11/24/1997
    ## 3434    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 3435    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3436    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3437    1     *     M     M     1     2     2 11/25/1994 11/24/1997
    ## 3438    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3439    1     *     M     M     1     2     2 11/25/1994 11/24/1997
    ## 3440    1     *     M     M     4     4     4 11/25/1994 11/25/1997
    ## 3441    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 3442    2     *     *     R     1     1     1 11/30/1994 11/21/1997
    ## 3443    2     *     *     R     1     1     1 11/30/1994 11/21/1997
    ## 3444    1     M     M     M     2     2     2 12/01/1994 11/24/1997
    ## 3445    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3446    1     *     M     M     1     2     2 12/05/1994 11/26/1997
    ## 3447    2     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 3448    1     M     M     M     2     2     2 11/02/1994 11/11/1997
    ## 3449    1     M     M     M     2     2     2 11/07/1994 11/12/1997
    ## 3450    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 3451    1    ML     M     M     2     2     2 11/09/1994 11/13/1997
    ## 3452    1     *     R     *     1     1     1 11/09/1994 11/13/1997
    ## 3453    1     M     M     M     2     2     2 11/10/1994 11/14/1997
    ## 3454    2     *     R     *     1    -1     1 11/10/1994 11/14/1997
    ## 3455    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3456    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3457    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3458    1     M     M     M     3     3     3 11/11/1994 11/18/1997
    ## 3459    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3460    1     M     M     M     4     3     4 11/14/1994 11/20/1997
    ## 3461    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3462    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3463    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3464    1     M     M     M     3     3     3 11/16/1994 11/21/1997
    ## 3465    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3466    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3467    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3468    1     M     *     *     2     1     1 11/17/1994 11/25/1997
    ## 3469    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3470    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3471    1     M     M     M     5     2     3 11/18/1994 11/27/1997
    ## 3472    1     M     R     M     4     1     3 11/21/1994 11/28/1997
    ## 3473    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3474    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3475    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3476    0     *    DC    DC     1    -1    -1 11/23/1994 11/18/1997
    ## 3477    1     M     M     M     2     2     2 11/23/1994 11/18/1997
    ## 3478    1     M     M     M     4     3     3 11/24/1994 11/19/1997
    ## 3479    1     M     *     *     2     1     1 11/30/1994 11/20/1997
    ## 3480    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 3481    1     M     M     M     2     2     2 12/01/1994 11/20/1997
    ## 3482    1     M     M     M     5     3     3 12/01/1994 11/21/1997
    ## 3483    1    ML     M     M     2    21     2 12/02/1994 11/24/1997
    ## 3484    1     M     M     M     2     2     2 11/07/1994 11/13/1997
    ## 3485    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 3486    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3487    1     M     M     M     3     2     4 11/08/1994 11/14/1997
    ## 3488    1     M     M     M     3     2     3 11/08/1994 11/14/1997
    ## 3489    1     M     R     *     2     1     1 11/08/1994 11/17/1997
    ## 3490    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3491    2     *     R     *     1    -1     1 11/09/1994 11/17/1997
    ## 3492    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 3493    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 3494    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 3495    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 3496    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3497    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3498    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3499    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 3500    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3501    1     M     M     M     2     2     2 11/17/1994 11/26/1997
    ## 3502    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 3503    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3504    2     *     R     *     1    -1     1 11/21/1994 11/27/1997
    ## 3505    1     L    LR     *     1     1     1 11/04/1994 11/12/1997
    ## 3506    1     *     M     M     1     2     2 11/04/1994 11/13/1997
    ## 3507    1     M     M     M     2     2     3 11/04/1994 11/13/1997
    ## 3508    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 3509    2     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 3510    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 3511    1    ML     M    ML     3     2     2 11/07/1994 11/20/1997
    ## 3512    1     *     M     M     2     2     2 11/07/1994 11/20/1997
    ## 3513    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 3514    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3515    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3516    1    ML     M    ML     2     2     2 11/08/1994 11/20/1997
    ## 3517    1     M     *     *     2     1     1 11/08/1994 11/20/1997
    ## 3518    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3519    0     *     *    DC     1     1    -1 11/09/1994 11/21/1997
    ## 3520    0     *    DS    DC     1    -1    -1 11/11/1994 11/25/1997
    ## 3521    1     M     M     M     2     2     2 11/11/1994 11/25/1997
    ## 3522    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 3523    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 3524    1     M     M     M     2     2     2 11/14/1994 11/26/1997
    ## 3525    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 3526    2     *     R     *     1    -1     1 11/16/1994 11/27/1997
    ## 3527    2     *     M     M     2     3     3 11/16/1994 11/27/1997
    ## 3528    0     *    DS    DC     1    -1    -1 11/16/1994 11/27/1997
    ## 3529    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 3530    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3531    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3532    0     *     R    DC     1    -1    -1 11/18/1994 11/12/1997
    ## 3533    1     M     *     *     2     1     1 11/18/1994 11/12/1997
    ## 3534    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3535    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 3536    1     M     M     M     3     3     2 11/23/1994 11/13/1997
    ## 3537    0     *    DS    DC     1    -1    -1 11/23/1994 11/14/1997
    ## 3538    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3539    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3540    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3541    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 3542    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 3543    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3544    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3545    1     L     L     L     1     1     1 12/01/1994 11/18/1997
    ## 3546    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3547    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 3548    2    ML     R     *     3    -1     1 12/05/1994 11/25/1997
    ## 3549    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 3550    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3551    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3552    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 3553    1     L     L     L     1     1     1 11/16/1994 11/18/1997
    ## 3554    0     *     *    DS     1     1    -1 11/16/1994 11/18/1997
    ## 3555    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3556    2     *     S     R     1    -9     1 11/16/1994 11/20/1997
    ## 3557    1     M     *     *     2     1     1 11/16/1994 11/20/1997
    ## 3558    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 3559    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 3560    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3561    1    ML     M     M     3     3     3 11/29/1994 11/25/1997
    ## 3562    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 3563    1     L     L     *     1     1     1 12/01/1994 11/24/1997
    ## 3564    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 3565    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3566    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3567    1     M     M     M     2     2     2 12/02/1994 11/25/1997
    ## 3568    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 3569    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 3570    1     M    MR     *     2     2     1 11/23/1994 11/18/1997
    ## 3571    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3572    1     *     M    MR     1     3     2 11/02/1994 11/11/1997
    ## 3573    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3574    1     M     M     M     2     2     2 11/02/1994 11/11/1997
    ## 3575    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3576    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3577    1     *     M     M     1     2     2 11/07/1994 11/12/1997
    ## 3578    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3579    1     M     M     M     2     2     4 11/09/1994 11/13/1997
    ## 3580    1     M     *     M     4     1     4 11/09/1994 11/13/1997
    ## 3581    2     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3582    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3583    1     *     M     M     1     2     2 11/14/1994 11/19/1997
    ## 3584    1     M     M     M     3     3     3 11/14/1994 11/19/1997
    ## 3585    2     *     S     R     1    -9     1 11/14/1994 11/20/1997
    ## 3586    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 3587    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3588    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3589    2     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3590    2     *     R     *     2     1     1 11/16/1994 11/21/1997
    ## 3591    2     *     M     R     1     2     1 11/17/1994 11/25/1997
    ## 3592    1     M     M     M     3     3     2 11/17/1994 11/25/1997
    ## 3593    0     *     *    DS     1     1    -1 11/17/1994 11/25/1997
    ## 3594    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 3595    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3596    2     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3597    1     *     R     *     1     1     1 11/23/1994 11/18/1997
    ## 3598    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3599    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3600    1    ML    MR     M     2     2     2 11/23/1994 11/18/1997
    ## 3601    1     *     M     M     2     2     2 11/24/1994 11/19/1997
    ## 3602    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 3603    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 3604    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 3605    1     M     M     *     2     2     1 12/01/1994 11/20/1997
    ## 3606    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 3607    1     M     M     M     2     2     2 12/01/1994 11/20/1997
    ## 3608    1    ML     L     M     3     1     4 12/01/1994 11/20/1997
    ## 3609    1    ML     R     *     2     1     1 12/01/1994 11/20/1997
    ## 3610    1     M     M    MR     2     2     2 12/01/1994 11/21/1997
    ## 3611    1     M     M     M     4     3     3 12/01/1994 11/21/1997
    ## 3612    1     M     M     M     4     2     3 12/01/1994 11/21/1997
    ## 3613    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 3614    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 3615    1    ML     *     *     3     1     1 12/02/1994 11/21/1997
    ## 3616    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3617    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 3618    1     M     M     M     3     4     4 11/07/1994 11/13/1997
    ## 3619    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 3620    1     M     M     M     3     2     2 11/08/1994 11/14/1997
    ## 3621    0     *     *    DS     1     1    -1 11/07/1994 11/14/1997
    ## 3622    1     M     M     M     2     2     2 11/08/1994 11/17/1997
    ## 3623    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3624    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3625    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 3626    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 3627    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3628    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3629    0     L     L    DN     1     1    -1 11/17/1994 11/26/1997
    ## 3630    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3631    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3632    1     M     *     M     2     1     2 11/18/1994 11/27/1997
    ## 3633    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 3634    1     *     N     *     1    -9     1 11/04/1994 11/12/1997
    ## 3635    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 3636    1     M     M     M     2     2     2 11/04/1994 11/12/1997
    ## 3637    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 3638    2     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 3639    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3640    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3641    1     M     M     M     2     2     2 11/08/1994 11/20/1997
    ## 3642    1     M     M     M     3     2     2 11/09/1994 11/21/1997
    ## 3643    1     M     R     *     3     1     1 11/09/1994 11/21/1997
    ## 3644    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3645    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 3646    1     M     M     M     3     3     3 11/09/1994 11/24/1997
    ## 3647    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 3648    1     M     M     M     3     3     3 11/11/1994 11/25/1997
    ## 3649    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 3650    1     M     *     *     2     1     1 11/14/1994 11/26/1997
    ## 3651    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 3652    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 3653    1     *     M     M     1     2     2 11/15/1994 11/27/1997
    ## 3654    1     M     L     *     2     1     1 11/16/1994 11/27/1997
    ## 3655    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 3656    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3657    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3658    1     M     M     M     2     2     2 11/22/1994 11/13/1997
    ## 3659    2     *     R     R     1     1     1 11/22/1994 11/13/1997
    ## 3660    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3661    1     M     M     M     3     3     4 11/22/1994 11/13/1997
    ## 3662    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3663    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3664    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3665    1     M     M     M     2     2     2 11/24/1994 11/14/1997
    ## 3666    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3667    1     M     *     *     2     1     1 11/24/1994 11/17/1997
    ## 3668    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 3669    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 3670    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 3671    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3672    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3673    1     *     *     M     1     1     2 12/01/1994 11/18/1997
    ## 3674    1     M     M     M     2     2     2 12/01/1994 11/19/1997
    ## 3675    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 3676    2     *     R     *     1    -1     1 12/05/1994 11/25/1997
    ## 3677    1     M     N     M     4    -9     4 12/05/1994 11/25/1997
    ## 3678    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 3679    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 3680    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 3681    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3682    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3683    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3684    1     *     M     R     1     2     1 11/16/1994 11/18/1997
    ## 3685    1     L     L     L     1     1     1 11/16/1994 11/18/1997
    ## 3686    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 3687    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 3688    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 3689    1     M     M     M     2     2     2 11/16/1994 11/19/1997
    ## 3690    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3691    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 3692    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 3693    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 3694    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 3695    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 3696    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3697    1     *     *     M     1     1     2 11/25/1994 11/24/1997
    ## 3698    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3699    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 3700    1     M     M     M     2     2     2 11/29/1994 11/25/1997
    ## 3701    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 3702    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3703    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 3704    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 3705    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3706    1     M     M     M     5     4     4 11/02/1994 11/11/1997
    ## 3707    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3708    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 3709    1     L     L     L     1     1     1 11/10/1994 11/14/1997
    ## 3710    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3711    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3712    3     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3713    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3714    2     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3715    0     *    DS    DC     1    -1    -1 11/11/1994 11/18/1997
    ## 3716    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3717    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3718    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3719    1     M     M     M     2     2     2 11/14/1994 11/20/1997
    ## 3720    1     M     *     *     2     1     1 11/14/1994 11/20/1997
    ## 3721    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3722    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3723    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3724    1    ML     M     M     2     2     3 11/16/1994 11/21/1997
    ## 3725    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3726    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3727    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3728    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3729    1     M     *     M     2     1     2 11/18/1994 11/26/1997
    ## 3730    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 3731    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 3732    1     *     M     M     2     2     2 11/22/1994 11/17/1997
    ## 3733    2     M     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3734    1     M     M     M     2     2     3 11/23/1994 11/18/1997
    ## 3735    1     M     M     M     4     4     5 11/24/1994 11/19/1997
    ## 3736    0     *    DS    DS     1    -1    -1 11/30/1994 11/20/1997
    ## 3737    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 3738    1    ML    ML     R     2     2     1 12/01/1994 11/20/1997
    ## 3739    1     *     *     M     1     1     2 12/02/1994 11/21/1997
    ## 3740    1     *     *     M     4     1     5 12/02/1994 11/24/1997
    ## 3741    0     L    DN    DN     1    -1    -1 11/07/1994 11/13/1997
    ## 3742    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3743    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3744    1     M     M     M     2     2     2 11/07/1994 11/13/1997
    ## 3745    1     *     *     M     1     1     2 11/08/1994 11/17/1997
    ## 3746    0     M    DN    DC     2    -1    -1 11/07/1994 11/14/1997
    ## 3747    2     *     R     *     1    -1     1 11/09/1994 12/17/1997
    ## 3748    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3749    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3750    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3751    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3752    1     M     M     M     3     4     3 11/16/1994 11/24/1997
    ## 3753    1     *     L     L     1     1     1 11/17/1994 11/26/1997
    ## 3754    2     M     R     *     2    -1     1 11/18/1994 11/27/1997
    ## 3755    1     *     R     *     1     1     1 11/18/1994 11/27/1997
    ## 3756    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3757    2     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3758    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 3759    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 3760    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 3761    2     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 3762    1     M     M     M     2     2     2 11/07/1994 11/20/1997
    ## 3763    1    ML    ML    ML     2     2     2 11/09/1994 11/21/1997
    ## 3764    1     *     *     L     1     1     1 11/09/1994 11/21/1997
    ## 3765    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 3766    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 3767    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 3768    2     M     R     R     2     1     1 11/11/1994 11/24/1997
    ## 3769    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 3770    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 3771    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 3772    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 3773    0     *    DS    DS     1    -1    -1 11/15/1994 11/26/1997
    ## 3774    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 3775    1     *     *     *     1     1     1 11/15/1994 11/27/1997
    ## 3776    2    ML    ML     R     2     2     1 11/16/1994 11/27/1997
    ## 3777    1     *     R     *     1     1     1 11/17/1994 11/12/1997
    ## 3778    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3779    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3780    1     M     *     M     2     1     2 11/22/1994 11/13/1997
    ## 3781    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3782    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3783    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 3784    1    ML     M     M     2     2     2 11/24/1994 11/17/1997
    ## 3785    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3786    2     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3787    1     M     M     M     2     2     2 12/01/1994 11/18/1997
    ## 3788    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3789    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3790    1     M     M     M     2     2     2 12/01/1994 11/18/1997
    ## 3791    1     M     M     M     3     2     2 12/02/1994 11/19/1997
    ## 3792    2     *     R     R     2     1     1 12/02/1994 11/24/1997
    ## 3793    2     L     R     *     1    -1     1 12/05/1994 11/25/1997
    ## 3794    1     *     *     *     1     1     1 12/05/1994 11/25/1997
    ## 3795    1     M     M     M     3     3     4 12/05/1994 11/25/1997
    ## 3796    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 3797    1     *     M     *     1     2     1 11/07/1994 11/11/1997
    ## 3798    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 3799    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 3800    1     M     *     *     2     1     1 11/08/1994 11/14/1997
    ## 3801    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 3802    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3803    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3804    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 3805    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3806    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3807    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3808    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3809    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3810    1     M     M     M     2     2     2 11/16/1994 11/18/1997
    ## 3811    1     *    ML    ML     1     2     2 11/16/1994 11/18/1997
    ## 3812    1     M     M     M     3     3     2 11/16/1994 11/19/1997
    ## 3813    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 3814    2     *     R     *     1    -1     1 11/16/1994 11/20/1997
    ## 3815    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 3816    1    ML    ML    ML     3     2     3 11/23/1994 11/24/1997
    ## 3817    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3818    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3819    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 3820    1     L     *     L     1     1     1 11/29/1994 11/25/1997
    ## 3821    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 3822    1     M     M     M     2     2     2 12/01/1994 11/21/1997
    ## 3823    2     *     *     R     1     1     1 12/02/1994 11/25/1997
    ## 3824    2     M     M     M     2     2     2 11/02/1994 11/11/1997
    ## 3825    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3826    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3827    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 3828    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 3829    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3830    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3831    2     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3832    1     M     M     M     2     3     3 11/11/1994 11/18/1997
    ## 3833    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3834    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3835    1     M     M     M     2     2     2 11/16/1994 11/21/1997
    ## 3836    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3837    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3838    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3839    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3840    2     *     R     *     1    -1     1 11/18/1994 11/27/1997
    ## 3841    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3842    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3843    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3844    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3845    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3846    2     M     M     M     2     2     2 11/23/1994 11/18/1997
    ## 3847    2     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3848    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 3849    1     M     *     *     2     1     1 11/30/1994 11/20/1997
    ## 3850    1     M     M     M     2     2     2 12/01/1994 11/20/1997
    ## 3851    1     M     M     M     3     3     2 12/01/1994 11/20/1997
    ## 3852    1     M     *     *     2     1     1 12/01/1994 11/21/1997
    ## 3853    1     M     R     *     2     1     1 12/01/1994 11/21/1997
    ## 3854    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3855    1     L     L    ML     1     1     2 12/02/1994 11/21/1997
    ## 3856    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 3857    1     M     M     M     4     4     4 12/02/1994 11/21/1997
    ## 3858    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3859    1     L     L     L     1     1     1 11/02/1994 11/26/1997
    ## 3860    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3861    1     M     M     M     2     2     2 11/02/1994 11/26/1997
    ## 3862    1     M     *     M     2     1     2 11/02/1994 11/26/1997
    ## 3863    1     M     M     M     3     3     3 11/02/1994 11/26/1997
    ## 3864    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 3865    0     *    DN    DT     1    -1    -1 11/02/1994 11/26/1997
    ## 3866    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 3867    1    ML     L     L     2     1     1 11/07/1994 11/13/1997
    ## 3868    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3869    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3870    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 3871    0     *    DC    DC     1    -1    -1 11/09/1994 11/17/1997
    ## 3872    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 3873    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 3874    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 3875    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 3876    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 3877    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3878    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 3879    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 3880    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 3881    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 3882    0     M     R    DC     2    -1    -1 11/18/1994 11/27/1997
    ## 3883    2     L     L     R     1     1     1 11/15/1994 11/27/1997
    ## 3884    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 3885    1     M     M     M     4     3     3 11/07/1994 11/19/1997
    ## 3886    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 3887    2     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 3888    1     M     M     M     2     2     2 11/07/1994 11/20/1997
    ## 3889    1     L    ML    ML     2     4     4 11/07/1994 11/20/1997
    ## 3890    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3891    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 3892    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 3893    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 3894    1     M     M     M     2     2     2 11/11/1994 11/25/1997
    ## 3895    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 3896    0     *    DC    DT     1    -1    -1 11/16/1994 11/27/1997
    ## 3897    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 3898    1     *     *     *     1     1     1 11/17/1994 11/12/1997
    ## 3899    0     *     *    DS     1     1    -1 11/17/1994 11/12/1997
    ## 3900    0     *    DC    DN     1    -1    -1 11/18/1994 11/12/1997
    ## 3901    1     *     *     *     1     1     1 11/18/1994 11/12/1997
    ## 3902    1     M     M     M     2     2     2 11/22/1994 11/13/1997
    ## 3903    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 3904    2     M     M     R     2     2     1 11/23/1994 11/13/1997
    ## 3905    1     *     *     *     3     1     1 11/23/1994 11/13/1997
    ## 3906    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3907    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3908    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 3909    1     M     M     M     2     2     2 11/24/1994 11/17/1997
    ## 3910    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 3911    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 3912    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3913    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 3914    1     L     *     L     1     1     1 12/01/1994 11/19/1997
    ## 3915    1     M     M     M     2     2     2 12/01/1994 11/19/1997
    ## 3916    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 3917    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3918    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3919    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 3920    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 3921    1     M     M     M     2     3     2 11/08/1994 11/12/1997
    ## 3922    1     *     M     M     2     2     4 11/08/1994 11/12/1997
    ## 3923    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 3924    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 3925    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 3926    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3927    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 3928    1     M     M     M     3     3     3 11/14/1994 11/17/1997
    ## 3929    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 3930    1     M     M     M     3     3     3 11/16/1994 11/20/1997
    ## 3931    1     *     *     *     1     1     1 11/16/1994 11/20/1997
    ## 3932    0     *    DC    DN     1    -1    -1 11/22/1994 11/20/1997
    ## 3933    1     M     M     M     3     5     5 11/23/1994 11/19/1997
    ## 3934    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 3935    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 3936    0     L    DC    DN     1    -1    -1 11/23/1994 11/24/1997
    ## 3937    0     *    DS    DC     1    -1    -1 11/25/1994 11/24/1997
    ## 3938    1     M     *    ML     2     1     3 11/23/1994 11/24/1997
    ## 3939    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3940    1     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 3941    1     M     M     M     3     3     4 11/29/1994 11/25/1997
    ## 3942    1     M     M     M     6     4     2 11/29/1994 11/25/1997
    ## 3943    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 3944    2     *     *     R     1     1     1 12/01/1994 11/21/1997
    ## 3945    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3946    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 3947    1     M     M     M     2     2     2 12/02/1994 11/25/1997
    ## 3948    2     *     R     *     1    -1     1 12/02/1994 11/25/1997
    ## 3949    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 3950    1     *     *     *     1     1     1 12/05/1994 11/26/1997
    ## 3951    1    ML     M     M     5     4     3 12/06/1994 11/26/1997
    ## 3952    1    ML     R     M     3     1     2 11/23/1994 11/24/1997
    ## 3953    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3954    1     *     M     M     1     2     2 11/02/1994 11/11/1997
    ## 3955    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 3956    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 3957    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 3958    2    ML     S     R     5    -9     1 11/09/1994 11/13/1997
    ## 3959    1    ML     *     M     3     1     2 11/09/1994 11/13/1997
    ## 3960    2     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 3961    1     L     M    ML     1     2     2 11/09/1994 11/13/1997
    ## 3962    2     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3963    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 3964    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3965    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3966    1     *     L     L     1     1     1 11/11/1994 11/18/1997
    ## 3967    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 3968    2    ML     C     R     2    -9     1 11/14/1994 11/19/1997
    ## 3969    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3970    2     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 3971    1     *     M     M     1     2     2 11/14/1994 11/19/1997
    ## 3972    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 3973    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 3974    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3975    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 3976    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3977    0     *    DS    DC     1    -1    -1 11/15/1994 11/21/1997
    ## 3978    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 3979    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3980    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3981    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3982    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3983    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3984    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3985    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 3986    2     *     *     R     1     1     1 11/17/1994 11/26/1997
    ## 3987    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3988    0     *     *    DS     1     1    -1 11/18/1994 11/26/1997
    ## 3989    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 3990    2     *     L     R     1     1     1 11/18/1994 11/26/1997
    ## 3991    1     M    ML     M     2     2     2 11/18/1994 11/27/1997
    ## 3992    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 3993    1     M     M     M     3     3     3 11/21/1994 11/28/1997
    ## 3994    1     M     *     *     1     1     1 11/17/1994 11/25/1997
    ## 3995    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 3996    0     L    DC    DC     1    -1    -1 11/22/1994 11/17/1997
    ## 3997    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 3998    1     M     M     M     4     3     5 11/23/1994 11/18/1997
    ## 3999    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 4000    2     *     *     R     1     1     1 11/23/1994 11/18/1997
    ## 4001    1     M     M     M     4     4     3 11/23/1994 11/18/1997
    ## 4002    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 4003    1     *     *     *     1     1     1 12/01/1994 11/20/1997
    ## 4004    1    ML     *     *     1     1     1 12/01/1994 11/20/1997
    ## 4005    1     M     *     *     2     1     1 12/01/1994 11/21/1997
    ## 4006    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 4007    1     M     M     M     3     3     3 12/02/1994 11/21/1997
    ## 4008    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 4009    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 4010    1     M     M     *     2     2     1 11/07/1994 11/13/1997
    ## 4011    1     M     M     M     3     3     3 11/07/1994 11/13/1997
    ## 4012    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 4013    1     M     M     M     2     3     6 11/07/1994 11/14/1997
    ## 4014    1     M     *     *     2     1     1 11/08/1994 11/14/1997
    ## 4015    1     M     M     M     2     2     2 11/09/1994 11/17/1997
    ## 4016    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 4017    1     M     M     M     2     2     2 11/09/1994 12/17/1997
    ## 4018    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 4019    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 4020    1     *     R     M     1     1     2 11/18/1994 11/27/1997
    ## 4021    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 4022    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 4023    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4024    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4025    0     L    DC    DT     1    -1    -1 11/15/1994 11/21/1997
    ## 4026    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4027    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4028    1     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 4029    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 4030    0     *    DS    DC     1    -1    -1 11/18/1994 11/27/1997
    ## 4031    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 4032    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 4033    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 4034    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 4035    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 4036    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 4037    1     M     *     *     2     1     1 11/07/1994 11/19/1997
    ## 4038    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 4039    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4040    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4041    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4042    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4043    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4044    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4045    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 4046    1     *     *     *     1     1     1 11/08/1994 11/20/1997
    ## 4047    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 4048    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 4049    1     *     *     *     1     1     1 11/11/1994 11/24/1997
    ## 4050    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 4051    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 4052    1     M     *     *     3     1     1 11/14/1994 11/26/1997
    ## 4053    1     *     *     *     2     1     1 11/14/1994 11/26/1997
    ## 4054    1     L     *     M     1     1     2 11/14/1994 11/26/1997
    ## 4055    1     M     M     M     2     3     3 11/14/1994 11/26/1997
    ## 4056    1     *     *     *     1     1     1 11/15/1994 11/26/1997
    ## 4057    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 4058    1     M     M     M     2     2     2 11/17/1994 11/12/1997
    ## 4059    1     M     *     *     2     1     1 11/18/1994 11/12/1997
    ## 4060    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 4061    1     M     M     M     2     2     2 11/22/1994 11/13/1997
    ## 4062    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 4063    1     *     *     *     1     1     1 11/23/1994 11/13/1997
    ## 4064    1     M     M     M     2     2     2 11/23/1994 11/13/1997
    ## 4065    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 4066    1     *     *     *     1     1     1 11/23/1994 11/14/1997
    ## 4067    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 4068    1     *     R     *     1     1     1 11/24/1994 11/14/1997
    ## 4069    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 4070    1     *     *     *     1     1     1 11/24/1994 11/14/1997
    ## 4071    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 4072    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 4073    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 4074    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 4075    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 4076    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 4077    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 4078    1     M     M     M     2     2     2 12/01/1994 11/18/1997
    ## 4079    1    ML     *     L     2     1     1 12/01/1994 11/18/1997
    ## 4080    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 4081    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 4082    0     *    DC    DT     1    -1    -1 12/05/1994 11/25/1997
    ## 4083    1     *     R     M     1     1     2 11/25/1994 11/25/1997
    ## 4084    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 4085    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 4086    1     *     *     *     1     1     1 11/09/1994 11/14/1997
    ## 4087    1     L     *     *     1     1     1 11/11/1994 11/17/1997
    ## 4088    1     M     M     M     2     2     2 11/14/1994 11/17/1997
    ## 4089    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 4090    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 4091    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 4092    1     *     *     M     1     1     2 11/16/1994 11/18/1997
    ## 4093    0     *     *    DS     1     1    -1 11/16/1994 11/18/1997
    ## 4094    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 4095    1     M     M     M     2     2     2 11/16/1994 11/19/1997
    ## 4096    2     *     L     R     1     1     1 11/16/1994 11/19/1997
    ## 4097    0     L    DN    DT     1    -1    -1 11/16/1994 11/19/1997
    ## 4098    1     M     M     *     2     2     1 11/16/1994 11/20/1997
    ## 4099    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 4100    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 4101    2     *     R     *     1    -1     1 11/21/1994 11/20/1997
    ## 4102    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 4103    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 4104    2     *     R     *     1    -1     1 11/22/1994 11/20/1997
    ## 4105    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 4106    1     L    ML    ML     1     2     3 11/23/1994 11/24/1997
    ## 4107    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 4108    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 4109    1     M     L     *     2     1     1 11/25/1994 11/25/1997
    ## 4110    1     M     M     M     4     4     5 11/25/1994 11/25/1997
    ## 4111    1     *     M     M     2     2     2 11/25/1994 11/25/1997
    ## 4112    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 4113    1     *     *     *     1     1     1 11/25/1994 11/25/1997
    ## 4114    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 4115    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 4116    1     M     *     M     3     1     2 11/29/1994 11/25/1997
    ## 4117    1     *     *     *     1     1     1 11/30/1994 11/21/1997
    ## 4118    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 4119    1     *     M     M     2     2     2 12/01/1994 11/24/1997
    ## 4120    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 4121    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 4122    1     *     *     *     1     1     1 12/06/1994 11/26/1997
    ## 4123    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 4124    1     M     M     M     2     2     2 11/02/1994 11/11/1997
    ## 4125    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 4126    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 4127    2     *     *     R     1     1     1 11/09/1994 11/13/1997
    ## 4128    2     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 4129    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 4130    1     *     *     *     1     1     1 11/09/1994 11/13/1997
    ## 4131    1     M     M     M     3     4     4 11/10/1994 11/14/1997
    ## 4132    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 4133    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4134    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4135    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4136    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4137    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4138    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4139    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4140    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4141    1     M     *     *     2     1     1 11/14/1994 11/19/1997
    ## 4142    1     M     M     M     2     2     2 11/14/1994 11/19/1997
    ## 4143    0     *     *    DS     1     1    -1 11/14/1994 11/19/1997
    ## 4144    1     *     *     *     1     1     1 11/14/1994 11/20/1997
    ## 4145    0     *    DC    DT     1    -1    -1 11/14/1994 11/20/1997
    ## 4146    1     M     M     M     2     2     2 11/15/1994 11/20/1997
    ## 4147    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 4148    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 4149    1     *     *     *     1     1     1 11/15/1994 11/20/1997
    ## 4150    1     M     M     M     2     2     2 11/15/1994 11/21/1997
    ## 4151    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4152    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4153    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 4154    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 4155    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 4156    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 4157    1     M     M     M     2     2     2 11/16/1994 11/21/1997
    ## 4158    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 4159    1     M     M     M     3     3     3 11/18/1994 11/26/1997
    ## 4160    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 4161    1     *     *     *     1     1     1 11/18/1994 11/26/1997
    ## 4162    1     M     M     M     2     2     2 11/18/1994 11/27/1997
    ## 4163    1     *    ML    ML     1     2     2 11/18/1994 11/27/1997
    ## 4164    1     M    ML     M     2     2     3 11/21/1994 11/28/1997
    ## 4165    1     *     *     *     1     1     1 11/22/1994 11/17/1997
    ## 4166    1     L     L     *     1     1     1 11/22/1994 11/17/1997
    ## 4167    1     *     R     *     1     1     1 11/22/1994 11/17/1997
    ## 4168    1    ML     M     M     3     4     5 11/22/1994 11/17/1997
    ## 4169    1    ML    ML    ML     2     2     2 11/23/1994 11/18/1997
    ## 4170    1     *     *     L     1     1     1 11/23/1994 11/18/1997
    ## 4171    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 4172    1     M     M     M     4     2     2 11/23/1994 11/18/1997
    ## 4173    1     M     *     *     2     1     1 11/23/1994 11/18/1997
    ## 4174    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 4175    1     *     *     *     1     1     1 11/24/1994 11/19/1997
    ## 4176    1     *     *     *     1     1     1 11/30/1994 11/20/1997
    ## 4177    1     M     *     *     2     1     1 11/30/1994 11/20/1997
    ## 4178    1     M     M     M     2     2     2 12/01/1994 11/20/1997
    ## 4179    1    ML     M     M     3     2     2 12/01/1994 11/21/1997
    ## 4180    1     M     R     *     2     1     1 12/01/1994 11/21/1997
    ## 4181    1     *     *     *     1     1     1 12/02/1994 11/21/1997
    ## 4182    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 4183    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 4184    1     M     *     *     2     1     1 12/02/1994 11/24/1997
    ## 4185    1     M     *     *     2     1     1 11/02/1994 11/26/1997
    ## 4186    1     M     M     M     2     2     2 11/02/1994 11/26/1997
    ## 4187    1     *     *     M     1     1     2 11/07/1994 11/13/1997
    ## 4188    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 4189    2     M     R     *     3    -1     1 11/07/1994 11/14/1997
    ## 4190    1     *     *     *     1     1     1 11/07/1994 11/14/1997
    ## 4191    1     *     M     M     1     4     5 11/08/1994 11/14/1997
    ## 4192    1     *     M     M     1     2     3 11/08/1994 11/14/1997
    ## 4193    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 4194    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 4195    1     *     *     *     1     1     1 11/08/1994 11/17/1997
    ## 4196    1     *     *     *     1     1     1 11/09/1994 12/17/1997
    ## 4197    1     *     *     *     1     1     1 11/14/1994 12/17/1997
    ## 4198    1     *     *     *     1     1     1 11/14/1994 11/21/1997
    ## 4199    0     *    DS    DC     1    -1    -1 11/14/1994 11/21/1997
    ## 4200    2     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4201    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4202    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4203    1     *     *     *     1     1     1 11/15/1994 11/21/1997
    ## 4204    1     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 4205    2     *     *     *     1     1     1 11/16/1994 11/24/1997
    ## 4206    2     *     *     *     1     1     1 11/17/1994 11/26/1997
    ## 4207    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 4208    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 4209    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 4210    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 4211    1     *     *     *     1     1     1 11/21/1994 11/27/1997
    ## 4212    1     M     M     M     2     2     2 11/21/1994 11/27/1997
    ## 4213    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 4214    1     *     M     M     1     2     2 11/04/1994 11/12/1997
    ## 4215    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 4216    2     *     R     *     1    -1     1 11/04/1994 11/12/1997
    ## 4217    1     *     *     *     1     1     1 11/04/1994 11/12/1997
    ## 4218    1     *     *     *     1     1     1 11/04/1994 11/13/1997
    ## 4219    1     *     *     *     1     1     1 11/07/1994 11/19/1997
    ## 4220    1     M     M     M     3     3     3 11/07/1994 11/19/1997
    ## 4221    1     M     M     M     2     2     3 11/07/1994 11/19/1997
    ## 4222    1     M     M     M     2     2     2 11/07/1994 11/19/1997
    ## 4223    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4224    1     *     *     *     1     1     1 11/07/1994 11/20/1997
    ## 4225    1     *     *     *     1     1     1 11/14/1994 11/26/1997
    ## 4226    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 4227    1     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 4228    2     *     *     *     1     1     1 11/09/1994 11/21/1997
    ## 4229    1     *     *     *     1     1     1 11/09/1994 11/24/1997
    ## 4230    1     *     *     *     1     1     1 11/11/1994 11/25/1997
    ## 4231    2     M     *     M     2     1     2 11/15/1994 11/26/1997
    ## 4232    1     M     M     M     2     2     2 11/15/1994 11/26/1997
    ## 4233    1    ML    ML     M     3     3     2 11/15/1994 11/27/1997
    ## 4234    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 4235    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 4236    1     *     *     *     1     1     1 11/16/1994 11/27/1997
    ## 4237    1     *     *     *     1     1     1 11/17/1994 11/11/1997
    ## 4238    1     M     *     *     2     1     1 11/18/1994 11/12/1997
    ## 4239    1     *     *     M     1     1     2 11/18/1994 11/12/1997
    ## 4240    1     L     L     L     1     1     1 11/18/1994 11/12/1997
    ## 4241    1     *     *     *     1     1     1 11/22/1994 11/13/1997
    ## 4242    1     M     R     *     2     1     1 11/24/1994 11/14/1997
    ## 4243    1    ML    MR     M     2     2     2 11/24/1994 11/17/1997
    ## 4244    1     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 4245    2     *     *     *     1     1     1 11/24/1994 11/17/1997
    ## 4246    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 4247    1     *     *     *     1     1     1 11/29/1994 11/17/1997
    ## 4248    1     M     M     M     2     2     2 11/29/1994 11/17/1997
    ## 4249    1     *     *     *     1     1     1 11/29/1994 11/18/1997
    ## 4250    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 4251    0     *    DC    DT     1    -1    -1 12/01/1994 11/18/1997
    ## 4252    1     *     M     M     3     3     3 12/01/1994 11/18/1997
    ## 4253    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 4254    1     *     *     *     1     1     1 12/01/1994 11/18/1997
    ## 4255    1     *     *     *     1     1     1 12/01/1994 11/19/1997
    ## 4256    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 4257    1     *     *     *     1     1     1 11/07/1994 11/11/1997
    ## 4258    2     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 4259    1     *     *     *     1     1     1 11/08/1994 11/12/1997
    ## 4260    1     *     *     *     1     1     1 11/08/1994 11/13/1997
    ## 4261    1     M     M     M     3     3     2 11/08/1994 11/13/1997
    ## 4262    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 4263    1     *     *     *     1     1     1 11/08/1994 11/14/1997
    ## 4264    1     *     *     *     1     1     1 11/09/1994 11/17/1997
    ## 4265    1     *     *     *     1     1     1 11/11/1994 11/17/1997
    ## 4266    1     *     M     M     2     2     2 11/11/1994 11/17/1997
    ## 4267    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 4268    1     *     *     *     1     1     1 11/14/1994 11/17/1997
    ## 4269    1     L     L     L     1     1     1 11/16/1994 11/18/1997
    ## 4270    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 4271    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 4272    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 4273    1    ML     M    ML     2     2     2 11/16/1994 11/18/1997
    ## 4274    1     *     *     *     1     1     1 11/16/1994 11/18/1997
    ## 4275    1     M     M     M     2     2     2 11/16/1994 11/18/1997
    ## 4276    1     *     *     *     1     1     1 11/16/1994 11/19/1997
    ## 4277    0     *     R    DS     1    -1    -1 11/16/1994 11/20/1997
    ## 4278    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 4279    1     *     *     *     1     1     1 11/21/1994 11/20/1997
    ## 4280    1     *     *     *     1     1     1 11/22/1994 11/20/1997
    ## 4281    1     *     *     *     1     1     1 11/23/1994 11/19/1997
    ## 4282    1     L     M     M     1     2     3 11/23/1994 11/24/1997
    ## 4283    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 4284    1     *     *     *     1     1     1 11/23/1994 11/24/1997
    ## 4285    2     M     R     *     2    -1     1 11/23/1994 11/24/1997
    ## 4286    2    ML     C     R     2    -9     1 11/23/1994 11/24/1997
    ## 4287    1     M     M     M     3     2     3 11/25/1994 11/24/1997
    ## 4288    2     *     *     *     1     1     1 11/25/1994 11/24/1997
    ## 4289    1     *     *     *     1     1     1 11/29/1994 11/25/1997
    ## 4290    1     *     M     M     1     2     2 11/29/1994 11/25/1997
    ## 4291    0     *    DS    DC     1    -1    -1 12/01/1994 11/21/1997
    ## 4292    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 4293    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 4294    1     *     M    ML     4     3     2 12/01/1994 11/24/1997
    ## 4295    1     *     *     *     1     1     1 12/01/1994 11/24/1997
    ## 4296    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 4297    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 4298    1     *     *     *     1     1     1 12/02/1994 11/25/1997
    ## 4299    1     L     R     *     1     1     1 11/21/1994 11/20/1997
    ## 4300    0     L     R    DC     2    -1    -1 11/23/1994 11/18/1997
    ## 4301    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 4302    1     M     M     M     3     3     3 11/07/1994 11/12/1997
    ## 4303    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 4304    1     *     *     *     1     1     1 11/07/1994 11/12/1997
    ## 4305    1     M     M     M     2     2     2 11/07/1994 11/12/1997
    ## 4306    1     L     *     *     1     1     1 11/09/1994 11/13/1997
    ## 4307    1     *     *     L     1     1     1 11/09/1994 11/13/1997
    ## 4308    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 4309    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 4310    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 4311    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 4312    1     *     *     *     1     1     1 11/10/1994 11/14/1997
    ## 4313    1     L     L    ML     1     1     2 11/10/1994 11/14/1997
    ## 4314    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4315    0     *    DS    DN     1    -1    -1 11/11/1994 11/18/1997
    ## 4316    1     *     *     *     1     1     1 11/11/1994 11/18/1997
    ## 4317    1     M     M     M     2     2     2 11/14/1994 11/19/1997
    ## 4318    1     M     M     M     2     2     2 11/14/1994 11/19/1997
    ## 4319    1     *     L     L     1     1     1 11/14/1994 11/19/1997
    ## 4320    1     *     *     *     1     1     1 11/14/1994 11/19/1997
    ## 4321    0     M    DS    DT     3    -1    -1 11/14/1994 11/20/1997
    ## 4322    0     *     *    DS     1     1    -1 11/15/1994 11/21/1997
    ## 4323    1     *     *     *     1     1     1 11/02/1994 11/11/1997
    ## 4324    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 4325    1     *     *     *     1     1     1 11/16/1994 11/21/1997
    ## 4326    1     *     *     *     1     1     1 11/17/1994 11/25/1997
    ## 4327    1     *     *     *     1     1     1 11/18/1994 11/27/1997
    ## 4328    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 4329    1     *     *     *     1     1     1 11/21/1994 11/28/1997
    ## 4330    1     M     M     M     2     2     2 11/21/1994 11/28/1997
    ## 4331    2     *     M     *     1     2     1 11/22/1994 11/17/1997
    ## 4332    2     *     *     R     1     1     1 11/23/1994 11/18/1997
    ## 4333    1     *     *     *     1     1     1 11/23/1994 11/18/1997
    ## 4334    2     *     R     *     1    -1     1 12/01/1994 11/20/1997
    ## 4335    1    ML     M     M     4     2     3 12/01/1994 11/20/1997
    ## 4336    1     *     *     *     1     1     1 12/01/1994 11/21/1997
    ## 4337    1     *     L    ML     1     1     2 12/01/1994 11/21/1997
    ## 4338    1     *     *     L     1     1     1 12/02/1994 11/21/1997
    ## 4339    1     *     M     M     6     5     4 12/02/1994 11/24/1997
    ## 4340    1     *     *     *     1     1     1 12/02/1994 11/24/1997
    ## 4341    0     *     M    DS     1     9    -1 12/02/1994 11/24/1997
    ## 4342    1     L     M     L     2     2     1 11/02/1994 11/26/1997
    ## 4343    1     *     *     *     1     1     1 11/02/1994 11/26/1997
    ## 4344    2     *     R     *     1    -1     1 11/07/1994 11/13/1997
    ## 4345    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ## 4346    1     M     M     M     2     3     3 11/07/1994 11/13/1997
    ## 4347    1     *     *     *     1     1     1 11/07/1994 11/13/1997
    ##           date3          rgr1
    ## 1    12/22/1998 -5.890228e-02
    ## 2    12/02/1998  7.931538e-03
    ## 3    12/04/1998 -8.438819e-04
    ## 4    12/25/1998  1.184211e-02
    ## 5    11/23/1998  3.333333e-03
    ## 6    12/27/1998  1.127098e-02
    ## 7    11/24/1998 -1.956469e-03
    ## 8    12/11/1998  1.372549e-02
    ## 9    12/08/1998  1.698671e-02
    ## 10   11/23/1998  1.076076e-02
    ## 11   12/02/1998  1.164262e-02
    ## 12   12/01/1998  1.135763e-02
    ## 13   12/23/1998  1.640212e-02
    ## 14   12/21/1998  1.231527e-02
    ## 15   12/23/1998  2.683264e-02
    ## 16   12/17/1998  5.317660e-03
    ## 17   12/09/1998  8.115942e-03
    ## 18   12/02/1998  1.681416e-02
    ## 19   12/23/1998  1.204456e-03
    ## 20   11/30/1998  3.263826e-02
    ## 21   12/17/1998  1.689708e-02
    ## 22   12/28/1998  3.555968e-02
    ## 23   12/22/1998  3.132832e-04
    ## 24   12/21/1998  3.289474e-02
    ## 25   12/22/1998  5.703422e-03
    ## 26   12/28/1998  1.148325e-02
    ## 27   12/10/1998  2.209414e-02
    ## 28   12/19/1998  2.909796e-03
    ## 29   12/10/1998 -3.336570e-01
    ## 30   12/02/1998  1.480750e-02
    ## 31   12/23/1998 -3.363095e-01
    ## 32   12/11/1998  1.360544e-02
    ## 33   12/03/1998  2.149437e-02
    ## 34   12/04/1998  6.865774e-03
    ## 35   12/09/1998  1.727116e-03
    ## 36   11/04/1998  2.072539e-03
    ## 37   11/04/1998  7.017544e-04
    ## 38   12/03/1998  1.026185e-02
    ## 39   11/23/1998  1.113506e-02
    ## 40   12/16/1998  1.552907e-02
    ## 41   12/02/1998  1.168310e-02
    ## 42   11/26/1998  1.721612e-02
    ## 43   12/01/1998  8.820287e-03
    ## 44   12/14/1998 -3.366556e-01
    ## 45   11/24/1998  2.254791e-02
    ## 46   12/27/1998  6.026365e-03
    ## 47   12/17/1998  6.102212e-03
    ## 48   12/08/1998  1.760429e-02
    ## 49   12/03/1998  7.724990e-04
    ## 50   12/09/1998  4.640371e-03
    ## 51   12/03/1998  1.240310e-02
    ## 52   12/02/1998  8.158508e-03
    ## 53   12/21/1998  2.255932e-02
    ## 54   12/17/1998  2.722676e-03
    ## 55   12/16/1998  1.066351e-02
    ## 56   12/15/1998  1.818901e-02
    ## 57   12/23/1998  1.430274e-02
    ## 58   12/16/1998  2.784407e-03
    ## 59   12/22/1998  2.008032e-03
    ## 60   12/08/1998  1.666667e-02
    ## 61   12/23/1998 -7.742461e-03
    ## 62   12/28/1998  3.671971e-03
    ## 63   12/04/1998  1.348039e-02
    ## 64   12/01/1998  0.000000e+00
    ## 65   12/14/1998  9.488449e-03
    ## 66   12/23/1998  1.526403e-02
    ## 67   12/09/1998  7.956449e-03
    ## 68   12/16/1998  3.773585e-03
    ## 69   12/04/1998  8.396306e-04
    ## 70   11/27/1998  4.219409e-03
    ## 71   11/23/1998  1.953291e-02
    ## 72   12/15/1998  1.156812e-02
    ## 73   12/23/1998  4.284490e-03
    ## 74   12/15/1998  4.731183e-03
    ## 75   11/30/1998  9.067358e-03
    ## 76   12/25/1998 -3.372498e-01
    ## 77   12/04/1998  1.614311e-02
    ## 78   12/08/1998 -2.978537e-02
    ## 79   11/30/1998  2.631579e-03
    ## 80   12/17/1998  2.239789e-02
    ## 81   11/30/1998  1.319261e-03
    ## 82   12/22/1998  3.544190e-02
    ## 83   12/10/1998  0.000000e+00
    ## 84   12/15/1998  1.821494e-03
    ## 85   12/10/1998  2.602740e-02
    ## 86   12/17/1998  2.979516e-02
    ## 87   12/16/1998 -4.675082e-04
    ## 88   12/15/1998  4.688233e-04
    ## 89   12/10/1998  1.418440e-02
    ## 90   11/27/1998  3.338102e-03
    ## 91   12/23/1998  1.245211e-02
    ## 92   12/28/1998  1.348748e-02
    ## 93   11/30/1998 -3.376749e-01
    ## 94   12/02/1998 -5.217391e-02
    ## 95   12/09/1998  6.782946e-03
    ## 96   12/02/1998  7.331378e-03
    ## 97   12/04/1998  5.899705e-03
    ## 98   12/02/1998  5.988024e-03
    ## 99   12/11/1998 -3.992016e-03
    ## 100  12/03/1998  1.002506e-03
    ## 101  12/17/1998  2.997967e-02
    ## 102  12/14/1998  6.106870e-03
    ## 103  12/17/1998  5.096840e-04
    ## 104  12/15/1998  3.058104e-03
    ## 105  12/03/1998  1.939765e-02
    ## 106  12/23/1998  2.572016e-03
    ## 107  12/15/1998  1.496388e-02
    ## 108  12/15/1998  8.812856e-03
    ## 109  12/15/1998  1.719078e-01
    ## 110  12/14/1998 -3.381028e-01
    ## 111  12/16/1998  3.301384e-02
    ## 112  12/01/1998  1.177100e-02
    ## 113  11/30/1998  2.090032e-02
    ## 114  12/22/1998  2.947481e-02
    ## 115  12/09/1998  5.420054e-03
    ## 116  12/15/1998  1.192412e-02
    ## 117  12/15/1998 -3.338753e-01
    ## 118  12/15/1998  5.428882e-04
    ## 119  12/10/1998  3.114754e-02
    ## 120  12/23/1998  5.464481e-03
    ## 121  12/22/1998  2.576754e-02
    ## 122  12/17/1998  1.550388e-02
    ## 123  11/24/1998  2.666667e-02
    ## 124  12/16/1998  5.583473e-03
    ## 125  12/14/1998  1.454139e-02
    ## 126  11/24/1998  5.630631e-03
    ## 127  12/14/1998  1.689189e-03
    ## 128  12/19/1998  2.829655e-02
    ## 129  12/04/1998  2.849003e-03
    ## 130  12/02/1998 -6.872852e-03
    ## 131  12/31/1998  2.873563e-03
    ## 132  11/23/1998  1.474926e-02
    ## 133  11/26/1998  3.380783e-02
    ## 134  12/10/1998  2.230259e-02
    ## 135  12/17/1998  3.925121e-02
    ## 136  12/17/1998  1.654412e-02
    ## 137  12/23/1998  6.740196e-03
    ## 138  12/10/1998  1.170672e-02
    ## 139  12/23/1998  4.390847e-02
    ## 140  12/09/1998  1.117318e-02
    ## 141  12/09/1998  0.000000e+00
    ## 142  12/11/1998  1.274697e-03
    ## 143  12/16/1998  0.000000e+00
    ## 144  12/28/1998  2.559181e-03
    ## 145  12/14/1998  7.051282e-03
    ## 146  12/14/1998  1.307190e-03
    ## 147  12/10/1998  8.563900e-03
    ## 148  12/10/1998  9.881423e-03
    ## 149  12/23/1998  2.000000e-03
    ## 150  12/11/1998  1.355014e-03
    ## 151  12/03/1998 -3.394433e-01
    ## 152  12/22/1998  1.502732e-02
    ## 153  12/23/1998  1.366120e-03
    ## 154  11/25/1998  2.053388e-03
    ## 155  12/19/1998  6.844627e-04
    ## 156  12/22/1998  0.000000e+00
    ## 157  11/23/1998  3.436426e-03
    ## 158  12/23/1998  1.383126e-02
    ## 159  12/10/1998 -6.930007e-04
    ## 160  12/29/1998  1.391788e-03
    ## 161  11/24/1998  1.064718e-01
    ## 162  11/24/1998  1.397624e-03
    ## 163  12/18/1998  2.666667e-02
    ## 164  12/04/1998  4.219409e-03
    ## 165  12/15/1998  9.971510e-03
    ## 166  12/03/1998  1.727862e-02
    ## 167  12/28/1998  3.615329e-03
    ## 168  11/30/1998  1.098901e-02
    ## 169  12/03/1998  1.471670e-03
    ## 170  12/21/1998 -3.340774e-01
    ## 171  12/08/1998  2.232143e-03
    ## 172  12/23/1998  5.992509e-03
    ## 173  12/04/1998  7.610350e-03
    ## 174  12/10/1998  3.203661e-02
    ## 175  12/02/1998  0.000000e+00
    ## 176  12/04/1998  6.880734e-03
    ## 177  12/23/1998  4.618938e-03
    ## 178  12/09/1998  0.000000e+00
    ## 179  12/15/1998  3.866976e-03
    ## 180  12/04/1998  1.624130e-02
    ## 181  12/03/1998  1.550388e-02
    ## 182  12/01/1998  6.201550e-03
    ## 183  12/23/1998 -3.341085e-01
    ## 184  12/01/1998  5.490196e-03
    ## 185  12/11/1998  2.352941e-03
    ## 186  12/02/1998  1.176471e-02
    ## 187  12/08/1998 -1.572327e-03
    ## 188  12/04/1998  1.260835e-02
    ## 189  12/08/1998  7.917656e-04
    ## 190  12/02/1998  4.750594e-03
    ## 191  12/11/1998  0.000000e+00
    ## 192  12/02/1998  8.730159e-03
    ## 193  12/11/1998  5.582137e-03
    ## 194  12/17/1998  3.197442e-03
    ## 195  12/02/1998  2.409639e-03
    ## 196  12/02/1998  4.016064e-03
    ## 197  12/23/1998  1.285141e-02
    ## 198  12/04/1998  0.000000e+00
    ## 199  12/21/1998  4.854369e-03
    ## 200  12/28/1998  6.472492e-03
    ## 201  11/24/1998  1.228501e-02
    ## 202  12/21/1998  1.893004e-02
    ## 203  11/24/1998  0.000000e+00
    ## 204  12/29/1998  2.500000e-03
    ## 205  12/17/1998  3.341688e-03
    ## 206  11/24/1998  0.000000e+00
    ## 207  12/02/1998  2.102178e-01
    ## 208  11/25/1998 -3.358522e-03
    ## 209  12/23/1998  2.518892e-03
    ## 210  12/15/1998  4.230118e-03
    ## 211  12/28/1998  0.000000e+00
    ## 212  12/22/1998  1.196581e-02
    ## 213  12/21/1998  1.718213e-03
    ## 214  12/22/1998 -1.899827e-02
    ## 215  12/28/1998  0.000000e+00
    ## 216  12/04/1998 -9.573542e-03
    ## 217  12/27/1998  7.915567e-03
    ## 218  12/18/1998  3.536693e-03
    ## 219  12/21/1998  2.666667e-02
    ## 220  11/26/1998 -8.912656e-03
    ## 221  12/10/1998  8.064516e-03
    ## 222  12/21/1998  0.000000e+00
    ## 223  12/08/1998 -3.613369e-03
    ## 224  12/17/1998  9.057971e-04
    ## 225  12/22/1998  5.479452e-03
    ## 226  12/17/1998  2.930403e-02
    ## 227  12/17/1998  0.000000e+00
    ## 228  11/26/1998  3.703704e-02
    ## 229  12/01/1998  1.851852e-03
    ## 230  12/17/1998  1.851852e-03
    ## 231  12/29/1998  2.777778e-03
    ## 232  12/04/1998  0.000000e+00
    ## 233  12/04/1998  1.851852e-02
    ## 234  12/18/1998  3.240741e-02
    ## 235  12/28/1998  0.000000e+00
    ## 236  12/11/1998  0.000000e+00
    ## 237  12/01/1998  2.414113e-02
    ## 238  12/11/1998  1.680672e-02
    ## 239  12/15/1998  0.000000e+00
    ## 240  12/18/1998  1.680672e-02
    ## 241  12/10/1998  0.000000e+00
    ## 242  12/01/1998  1.690141e-02
    ## 243  12/10/1998  0.000000e+00
    ## 244  12/28/1998  9.389671e-04
    ## 245  12/23/1998  4.694836e-03
    ## 246  12/29/1998  3.021719e-02
    ## 247  11/24/1998  4.721435e-03
    ## 248  11/23/1998  9.469697e-03
    ## 249  12/08/1998  2.556818e-02
    ## 250  11/26/1998  3.787879e-03
    ## 251  11/30/1998  2.849003e-03
    ## 252  12/01/1998  2.285714e-02
    ## 253  12/01/1998  1.142857e-02
    ## 254  12/10/1998  1.915709e-03
    ## 255  12/22/1998 -3.342912e-01
    ## 256  12/04/1998  2.601156e-02
    ## 257  11/25/1998  1.642512e-02
    ## 258  12/01/1998  0.000000e+00
    ## 259  12/15/1998  4.830918e-03
    ## 260  11/24/1998  0.000000e+00
    ## 261  12/01/1998 -9.689922e-03
    ## 262  11/04/1998  2.713178e-02
    ## 263  12/23/1998  2.519380e-02
    ## 264  11/23/1998  9.746589e-04
    ## 265  12/01/1998  1.955034e-03
    ## 266  12/23/1998  4.496579e-02
    ## 267  12/27/1998 -3.421569e-01
    ## 268  12/01/1998  3.638151e-02
    ## 269  12/23/1998 -3.343195e-01
    ## 270  12/02/1998  5.917160e-03
    ## 271  12/08/1998  2.465483e-02
    ## 272  12/11/1998  4.930966e-03
    ## 273  12/10/1998  2.967359e-03
    ## 274  12/02/1998 -1.984127e-03
    ## 275  12/10/1998  1.990050e-02
    ## 276  12/10/1998  1.996008e-03
    ## 277  12/10/1998  7.984032e-03
    ## 278  12/28/1998  0.000000e+00
    ## 279  11/24/1998  0.000000e+00
    ## 280  12/25/1998 -3.424242e-01
    ## 281  12/03/1998  6.079027e-03
    ## 282  12/01/1998  3.039514e-03
    ## 283  12/10/1998 -1.016260e-02
    ## 284  12/17/1998 -3.343527e-01
    ## 285  12/25/1998 -4.089980e-03
    ## 286  12/10/1998 -3.343590e-01
    ## 287  12/09/1998  0.000000e+00
    ## 288  12/02/1998  1.440329e-02
    ## 289  12/27/1998  1.131687e-02
    ## 290  12/14/1998  1.028807e-03
    ## 291  12/10/1998  2.063983e-03
    ## 292  12/22/1998  3.105590e-02
    ## 293  12/27/1998  1.453790e-02
    ## 294  12/18/1998  1.142264e-02
    ## 295  11/24/1998  2.604167e-02
    ## 296  12/29/1998  1.041667e-03
    ## 297  12/27/1998  3.125000e-03
    ## 298  12/11/1998 -5.208333e-03
    ## 299  12/09/1998  0.000000e+00
    ## 300  12/23/1998  0.000000e+00
    ## 301  11/30/1998  0.000000e+00
    ## 302  12/23/1998  3.154574e-03
    ## 303  11/23/1998  7.383966e-03
    ## 304  12/01/1998  2.004219e-02
    ## 305  12/01/1998  4.232804e-03
    ## 306  11/30/1998  0.000000e+00
    ## 307  12/04/1998  4.259851e-03
    ## 308  12/23/1998  3.194888e-03
    ## 309  11/24/1998  1.388889e-02
    ## 310  12/14/1998  2.136752e-02
    ## 311  12/01/1998 -3.215434e-03
    ## 312  12/01/1998  4.301075e-03
    ## 313  12/04/1998 -3.344086e-01
    ## 314  12/08/1998  0.000000e+00
    ## 315  11/26/1998  1.397849e-02
    ## 316  12/21/1998  0.000000e+00
    ## 317  12/21/1998  0.000000e+00
    ## 318  12/22/1998 -5.411255e-03
    ## 319  12/15/1998 -9.836066e-03
    ## 320  12/17/1998  0.000000e+00
    ## 321  12/10/1998  3.311258e-03
    ## 322  11/24/1998  1.661130e-02
    ## 323  12/02/1998  1.328904e-02
    ## 324  12/23/1998  3.333333e-02
    ## 325  12/14/1998  2.229654e-03
    ## 326  12/01/1998  5.574136e-03
    ## 327  12/23/1998  2.013423e-02
    ## 328  12/19/1998  3.367003e-03
    ## 329  12/22/1998  3.142536e-02
    ## 330  12/17/1998  0.000000e+00
    ## 331  12/23/1998  1.468927e-02
    ## 332  12/11/1998  2.054795e-02
    ## 333  12/10/1998  4.581901e-03
    ## 334  11/30/1998  4.581901e-03
    ## 335  11/04/1998  1.149425e-03
    ## 336  12/31/1998  3.703704e-02
    ## 337  11/30/1998  2.893519e-02
    ## 338  12/15/1998  1.161440e-02
    ## 339  12/28/1998  1.754386e-02
    ## 340  12/01/1998  1.177856e-02
    ## 341  12/10/1998  4.728132e-03
    ## 342  12/17/1998  9.456265e-03
    ## 343  12/28/1998  3.546099e-03
    ## 344  12/01/1998  4.744958e-03
    ## 345  11/24/1998  1.340451e-01
    ## 346  12/19/1998  1.067616e-02
    ## 347  12/23/1998  3.571429e-03
    ## 348  12/03/1998  0.000000e+00
    ## 349  12/01/1998 -3.345238e-01
    ## 350  12/23/1998 -3.345281e-01
    ## 351  12/14/1998  2.389486e-03
    ## 352  12/15/1998  1.314217e-02
    ## 353  12/03/1998  2.038369e-02
    ## 354  11/26/1998 -3.345324e-01
    ## 355  12/02/1998 -3.345324e-01
    ## 356  11/27/1998  4.452467e-02
    ## 357  12/22/1998  6.137184e-02
    ## 358  12/15/1998  0.000000e+00
    ## 359  12/04/1998 -5.655836e-02
    ## 360  12/04/1998  3.743961e-02
    ## 361  12/23/1998  9.696970e-03
    ## 362  12/10/1998  4.136253e-02
    ## 363  12/23/1998  1.098901e-02
    ## 364  12/11/1998  3.799020e-02
    ## 365  12/02/1998  2.450980e-03
    ## 366  12/15/1998  1.715686e-02
    ## 367  12/25/1998  7.777778e-02
    ## 368  12/04/1998  6.172840e-03
    ## 369  12/22/1998  3.703704e-03
    ## 370  12/21/1998  2.469136e-03
    ## 371  11/27/1998  3.995006e-02
    ## 372  12/17/1998  4.993758e-03
    ## 373  12/22/1998  4.868914e-02
    ## 374  12/23/1998  0.000000e+00
    ## 375  12/28/1998  7.518797e-03
    ## 376  12/29/1998  0.000000e+00
    ## 377  11/24/1998  1.886792e-02
    ## 378  11/24/1998 -3.345912e-01
    ## 379  12/11/1998  6.289308e-03
    ## 380  11/27/1998 -1.136364e-02
    ## 381  12/23/1998  1.145038e-02
    ## 382  12/27/1998  1.017812e-02
    ## 383  12/04/1998  1.017812e-02
    ## 384  12/01/1998  3.180662e-02
    ## 385  11/27/1998  2.554278e-03
    ## 386  12/28/1998  1.021711e-02
    ## 387  12/17/1998  7.692308e-03
    ## 388  12/28/1998  0.000000e+00
    ## 389  11/26/1998 -2.574003e-03
    ## 390  12/01/1998  1.291990e-03
    ## 391  11/30/1998  3.875969e-03
    ## 392  12/22/1998  6.485084e-03
    ## 393  12/09/1998  0.000000e+00
    ## 394  12/11/1998 -2.594034e-03
    ## 395  12/23/1998  3.242542e-02
    ## 396  12/17/1998  2.083333e-02
    ## 397  12/04/1998  7.812500e-03
    ## 398  12/27/1998  3.921569e-03
    ## 399  12/28/1998  0.000000e+00
    ## 400  12/10/1998  9.186352e-03
    ## 401  12/28/1998  1.574803e-02
    ## 402  12/23/1998  5.291005e-03
    ## 403  12/01/1998  1.984127e-02
    ## 404  12/08/1998  2.645503e-03
    ## 405  12/23/1998 -3.346614e-01
    ## 406  12/15/1998  1.328021e-03
    ## 407  11/27/1998  1.200000e-02
    ## 408  12/22/1998  8.000000e-03
    ## 409  12/19/1998  2.666667e-03
    ## 410  11/30/1998  0.000000e+00
    ## 411  11/30/1998  4.000000e-03
    ## 412  12/01/1998  6.693440e-03
    ## 413  12/27/1998  6.747638e-03
    ## 414  12/14/1998  2.574526e-02
    ## 415  12/17/1998  0.000000e+00
    ## 416  12/22/1998  1.360544e-03
    ## 417  12/22/1998  4.644809e-02
    ## 418  12/14/1998 -3.347051e-01
    ## 419  12/28/1998  0.000000e+00
    ## 420  11/23/1998  2.351314e-02
    ## 421  12/10/1998  5.394191e-02
    ## 422  11/30/1998  1.798064e-02
    ## 423  12/11/1998 -5.555556e-03
    ## 424  12/18/1998  2.777778e-03
    ## 425  12/22/1998  1.813110e-02
    ## 426  12/10/1998  5.578801e-03
    ## 427  12/29/1998  4.201681e-03
    ## 428  12/22/1998  4.201681e-03
    ## 429  12/22/1998 -3.347398e-01
    ## 430  12/21/1998  2.531646e-02
    ## 431  11/27/1998  3.785311e-01
    ## 432  12/04/1998  0.000000e+00
    ## 433  12/10/1998 -3.347518e-01
    ## 434  12/25/1998  1.424501e-03
    ## 435  12/31/1998  1.566952e-02
    ## 436  12/27/1998  0.000000e+00
    ## 437  12/15/1998  1.001431e-02
    ## 438  12/01/1998  1.293103e-02
    ## 439  12/18/1998  3.879310e-02
    ## 440  12/28/1998  3.304598e-02
    ## 441  12/01/1998  0.000000e+00
    ## 442  12/15/1998 -2.308802e-02
    ## 443  12/22/1998  1.159420e-02
    ## 444  12/04/1998  1.884058e-02
    ## 445  11/26/1998  4.347826e-03
    ## 446  12/04/1998 -3.347889e-01
    ## 447  11/26/1998  0.000000e+00
    ## 448  12/23/1998  0.000000e+00
    ## 449  12/17/1998  2.046784e-02
    ## 450  11/30/1998  1.023392e-02
    ## 451  11/24/1998  4.405286e-03
    ## 452  12/03/1998  7.342144e-03
    ## 453  12/02/1998  2.055800e-02
    ## 454  11/23/1998  4.572271e-02
    ## 455  12/23/1998  0.000000e+00
    ## 456  12/03/1998  1.185185e-02
    ## 457  12/27/1998 -3.348148e-01
    ## 458  12/15/1998  1.777778e-02
    ## 459  11/24/1998  1.345291e-02
    ## 460  11/25/1998  5.979073e-03
    ## 461  12/11/1998  1.345291e-02
    ## 462  11/26/1998  6.006006e-03
    ## 463  12/08/1998  3.016591e-03
    ## 464  12/22/1998  2.111614e-02
    ## 465  12/22/1998  2.413273e-02
    ## 466  12/04/1998  5.882353e-02
    ## 467  12/11/1998  2.272727e-02
    ## 468  12/23/1998  1.515152e-02
    ## 469  12/01/1998  1.212121e-02
    ## 470  12/01/1998 -1.060606e-02
    ## 471  12/04/1998  1.515152e-03
    ## 472  11/30/1998  1.363636e-02
    ## 473  12/01/1998  1.522070e-03
    ## 474  12/04/1998  0.000000e+00
    ## 475  12/14/1998  4.566210e-03
    ## 476  12/01/1998  7.645260e-03
    ## 477  11/27/1998 -3.348765e-01
    ## 478  12/22/1998  6.635802e-02
    ## 479  11/30/1998  1.543210e-03
    ## 480  12/03/1998  1.080247e-02
    ## 481  12/14/1998  1.395349e-02
    ## 482  12/19/1998  9.302326e-03
    ## 483  11/26/1998  6.046512e-02
    ## 484  12/16/1998  3.100775e-03
    ## 485  12/11/1998 -3.348910e-01
    ## 486  12/08/1998  1.557632e-02
    ## 487  11/27/1998  5.477308e-02
    ## 488  12/21/1998  6.259781e-03
    ## 489  12/15/1998  3.129890e-03
    ## 490  11/24/1998  1.572327e-02
    ## 491  11/25/1998  7.861635e-03
    ## 492  12/11/1998  1.415094e-02
    ## 493  12/01/1998  2.685624e-02
    ## 494  12/10/1998  5.845182e-02
    ## 495  12/22/1998  2.539683e-02
    ## 496  11/04/1998 -3.349206e-01
    ## 497  12/15/1998  6.379585e-03
    ## 498  12/01/1998  1.116427e-02
    ## 499  12/14/1998  4.784689e-03
    ## 500  12/03/1998  0.000000e+00
    ## 501  11/04/1998  1.602564e-03
    ## 502  12/10/1998  1.602564e-02
    ## 503  12/23/1998  1.442308e-02
    ## 504  12/14/1998  2.093398e-02
    ## 505  12/11/1998 -3.478964e-01
    ## 506  12/04/1998  0.000000e+00
    ## 507  11/26/1998  3.252033e-02
    ## 508  12/22/1998  2.439024e-02
    ## 509  11/23/1998  1.951220e-02
    ## 510  12/22/1998  2.764228e-02
    ## 511  11/27/1998  2.777778e-02
    ## 512  12/23/1998  1.970443e-02
    ## 513  11/23/1998  6.633499e-03
    ## 514  12/01/1998  6.633499e-03
    ## 515  12/17/1998  2.653400e-02
    ## 516  12/29/1998  3.316750e-03
    ## 517  12/01/1998  0.000000e+00
    ## 518  12/22/1998  1.833333e-02
    ## 519  12/10/1998 -3.350000e-01
    ## 520  11/30/1998  7.166667e-02
    ## 521  12/27/1998  6.700168e-03
    ## 522  12/09/1998  1.675042e-03
    ## 523  12/10/1998  0.000000e+00
    ## 524  12/04/1998  0.000000e+00
    ## 525  12/23/1998  5.025126e-03
    ## 526  12/04/1998  5.050505e-03
    ## 527  12/10/1998 -3.350168e-01
    ## 528  12/15/1998  3.367003e-03
    ## 529  12/23/1998 -3.350254e-01
    ## 530  12/10/1998  2.891156e-02
    ## 531  12/23/1998  3.231293e-02
    ## 532  11/24/1998  3.911565e-02
    ## 533  12/10/1998 -3.350427e-01
    ## 534  12/15/1998  0.000000e+00
    ## 535  11/30/1998  2.393162e-02
    ## 536  11/04/1998  5.128205e-03
    ## 537  12/16/1998  0.000000e+00
    ## 538  11/26/1998  3.436426e-03
    ## 539  12/15/1998  1.030928e-02
    ## 540  12/18/1998  1.202749e-02
    ## 541  12/22/1998  2.920962e-02
    ## 542  11/27/1998  1.727116e-03
    ## 543  12/11/1998  2.763385e-02
    ## 544  12/14/1998  1.208981e-02
    ## 545  11/30/1998  6.908463e-03
    ## 546  12/10/1998  1.036269e-02
    ## 547  12/14/1998 -5.181347e-03
    ## 548  12/02/1998  8.680556e-03
    ## 549  12/22/1998  8.680556e-03
    ## 550  11/24/1998  1.388889e-02
    ## 551  12/28/1998  1.041667e-02
    ## 552  12/04/1998 -1.745201e-03
    ## 553  12/10/1998  6.980803e-03
    ## 554  11/23/1998  1.047120e-02
    ## 555  12/10/1998  1.919721e-02
    ## 556  12/22/1998  1.396161e-02
    ## 557  12/28/1998  0.000000e+00
    ## 558  12/01/1998  7.017544e-03
    ## 559  12/16/1998 -4.912281e-02
    ## 560  12/18/1998  3.508772e-03
    ## 561  12/23/1998  1.228070e-02
    ## 562  11/24/1998  1.763668e-02
    ## 563  12/02/1998  1.058201e-02
    ## 564  12/16/1998  0.000000e+00
    ## 565  12/01/1998  5.319149e-03
    ## 566  12/10/1998  3.546099e-03
    ## 567  12/04/1998  6.560284e-02
    ## 568  12/18/1998  5.319149e-03
    ## 569  12/23/1998  2.852050e-02
    ## 570  12/31/1998  5.347594e-03
    ## 571  12/01/1998 -3.351159e-01
    ## 572  12/14/1998 -3.351159e-01
    ## 573  12/18/1998  1.426025e-02
    ## 574  12/09/1998  3.584229e-03
    ## 575  12/11/1998  1.792115e-03
    ## 576  12/04/1998  0.000000e+00
    ## 577  12/03/1998 -3.351351e-01
    ## 578  12/17/1998  1.081081e-02
    ## 579  12/23/1998 -3.351351e-01
    ## 580  12/01/1998  1.630435e-02
    ## 581  12/27/1998  3.642987e-03
    ## 582  12/15/1998  7.285974e-03
    ## 583  11/26/1998  1.465201e-02
    ## 584  12/19/1998  0.000000e+00
    ## 585  12/02/1998  0.000000e+00
    ## 586  11/24/1998  1.289134e-02
    ## 587  12/23/1998  5.340700e-02
    ## 588  12/25/1998  5.524862e-03
    ## 589  12/02/1998  3.683241e-03
    ## 590  12/10/1998  7.366483e-03
    ## 591  11/24/1998  2.209945e-02
    ## 592  12/04/1998  3.867403e-02
    ## 593  11/26/1998  0.000000e+00
    ## 594  12/18/1998  3.683241e-03
    ## 595  12/23/1998  0.000000e+00
    ## 596  11/24/1998  2.685185e-01
    ## 597  12/03/1998  0.000000e+00
    ## 598  12/10/1998  5.555556e-03
    ## 599  12/22/1998  1.296296e-02
    ## 600  12/15/1998  3.703704e-03
    ## 601  12/15/1998  1.851852e-03
    ## 602  12/23/1998  5.555556e-03
    ## 603  12/10/1998  8.193669e-02
    ## 604  12/10/1998  9.310987e-03
    ## 605  12/08/1998  7.448790e-03
    ## 606  12/02/1998  1.489758e-02
    ## 607  12/28/1998  0.000000e+00
    ## 608  12/22/1998  5.617978e-03
    ## 609  12/04/1998  3.745318e-03
    ## 610  12/15/1998  3.745318e-03
    ## 611  12/16/1998 -3.745318e-03
    ## 612  12/09/1998  9.416196e-03
    ## 613  12/01/1998  1.129944e-02
    ## 614  12/01/1998  7.532957e-03
    ## 615  12/04/1998  7.575758e-03
    ## 616  12/22/1998  3.598485e-02
    ## 617  12/03/1998  1.904762e-02
    ## 618  12/23/1998  4.571429e-02
    ## 619  12/15/1998  9.523810e-03
    ## 620  12/17/1998  6.285714e-01
    ## 621  12/22/1998  2.476190e-02
    ## 622  11/24/1998 -9.578544e-02
    ## 623  12/18/1998  1.915709e-02
    ## 624  12/21/1998  2.681992e-02
    ## 625  12/01/1998  2.890173e-02
    ## 626  12/17/1998  0.000000e+00
    ## 627  12/23/1998  1.734104e-02
    ## 628  12/01/1998  1.734104e-02
    ## 629  12/15/1998  2.312139e-02
    ## 630  11/30/1998 -5.780347e-03
    ## 631  12/02/1998  2.312139e-02
    ## 632  12/02/1998  1.937984e-03
    ## 633  12/10/1998  5.813953e-03
    ## 634  12/10/1998  9.689922e-03
    ## 635  12/01/1998  1.162791e-02
    ## 636  11/23/1998  1.871345e-01
    ## 637  12/03/1998  0.000000e+00
    ## 638  12/11/1998  3.898635e-03
    ## 639  12/29/1998  1.960784e-02
    ## 640  12/29/1998  1.176471e-02
    ## 641  12/04/1998  1.764706e-02
    ## 642  12/14/1998  7.843137e-03
    ## 643  12/02/1998  7.889546e-03
    ## 644  11/30/1998 -1.380671e-02
    ## 645  12/14/1998  3.747535e-02
    ## 646  12/17/1998  9.861933e-03
    ## 647  11/26/1998  5.952381e-02
    ## 648  11/24/1998  1.190476e-02
    ## 649  12/03/1998  3.968254e-03
    ## 650  12/11/1998  1.587302e-02
    ## 651  12/14/1998  9.920635e-03
    ## 652  12/01/1998  1.996008e-03
    ## 653  12/02/1998  5.988024e-03
    ## 654  12/03/1998  1.996008e-02
    ## 655  12/11/1998  1.397206e-02
    ## 656  12/15/1998  1.796407e-02
    ## 657  12/01/1998  2.008032e-03
    ## 658  12/28/1998  0.000000e+00
    ## 659  12/17/1998  1.212121e-02
    ## 660  12/08/1998  4.674797e-02
    ## 661  11/24/1998  4.268293e-02
    ## 662  12/02/1998  6.097561e-03
    ## 663  12/04/1998  6.097561e-03
    ## 664  11/26/1998  6.097561e-03
    ## 665  11/30/1998  2.032520e-03
    ## 666  12/01/1998 -2.044990e-03
    ## 667  12/22/1998  0.000000e+00
    ## 668  12/22/1998  3.497942e-02
    ## 669  12/09/1998  1.646091e-02
    ## 670  12/17/1998  0.000000e+00
    ## 671  12/17/1998  1.851852e-02
    ## 672  12/22/1998  2.057613e-03
    ## 673  12/27/1998 -3.519669e-01
    ## 674  12/15/1998  2.484472e-02
    ## 675  12/15/1998 -3.933747e-02
    ## 676  12/08/1998 -4.166667e-03
    ## 677  12/08/1998  3.333333e-02
    ## 678  12/11/1998  3.125000e-02
    ## 679  12/22/1998 -3.354167e-01
    ## 680  11/24/1998  0.000000e+00
    ## 681  12/01/1998  1.041667e-02
    ## 682  11/26/1998  1.041667e-02
    ## 683  11/30/1998  4.166667e-03
    ## 684  12/23/1998  6.250000e-03
    ## 685  12/02/1998  6.289308e-03
    ## 686  12/10/1998  2.935010e-02
    ## 687  12/15/1998  1.467505e-02
    ## 688  12/22/1998  1.898734e-02
    ## 689  12/23/1998  6.329114e-03
    ## 690  12/18/1998  4.219409e-03
    ## 691  12/23/1998  6.581741e-02
    ## 692  12/31/1998  4.273504e-03
    ## 693  12/10/1998  8.547009e-03
    ## 694  12/11/1998  2.136752e-03
    ## 695  12/21/1998  0.000000e+00
    ## 696  12/23/1998  1.923077e-02
    ## 697  12/17/1998  6.451613e-03
    ## 698  11/30/1998 -6.451613e-03
    ## 699  11/04/1998  0.000000e+00
    ## 700  12/14/1998  4.301075e-03
    ## 701  12/22/1998  1.948052e-02
    ## 702  11/30/1998  1.298701e-02
    ## 703  12/04/1998  6.493506e-03
    ## 704  12/10/1998  2.164502e-03
    ## 705  12/23/1998  1.731602e-02
    ## 706  12/09/1998  0.000000e+00
    ## 707  12/09/1998  0.000000e+00
    ## 708  12/15/1998  8.714597e-03
    ## 709  11/26/1998  8.714597e-03
    ## 710  12/18/1998  4.357298e-03
    ## 711  12/04/1998  1.096491e-02
    ## 712  12/04/1998  6.578947e-03
    ## 713  12/10/1998  8.771930e-03
    ## 714  12/31/1998  0.000000e+00
    ## 715  11/24/1998 -2.192982e-02
    ## 716  12/14/1998  3.070175e-02
    ## 717  12/22/1998 -2.280702e-01
    ## 718  12/22/1998  0.000000e+00
    ## 719  12/09/1998  4.856512e-02
    ## 720  12/10/1998  8.830022e-03
    ## 721  12/03/1998  2.207506e-03
    ## 722  12/28/1998  1.986755e-02
    ## 723  12/22/1998  2.222222e-02
    ## 724  12/22/1998  1.111111e-02
    ## 725  12/23/1998  1.111111e-02
    ## 726  12/25/1998  2.222222e-02
    ## 727  12/04/1998  4.000000e-02
    ## 728  12/16/1998  0.000000e+00
    ## 729  12/18/1998  2.222222e-03
    ## 730  12/18/1998  4.000000e-02
    ## 731  12/17/1998  2.222222e-03
    ## 732  11/24/1998  0.000000e+00
    ## 733  11/30/1998  2.237136e-03
    ## 734  12/03/1998  3.131991e-02
    ## 735  12/23/1998  2.237136e-03
    ## 736  12/21/1998  0.000000e+00
    ## 737  12/22/1998  0.000000e+00
    ## 738  12/01/1998  2.252252e-03
    ## 739  12/03/1998  2.027027e-02
    ## 740  12/10/1998  2.477477e-02
    ## 741  12/22/1998  2.027027e-02
    ## 742  12/17/1998  6.756757e-03
    ## 743  12/22/1998  0.000000e+00
    ## 744  12/15/1998  9.009009e-03
    ## 745  12/15/1998  9.009009e-03
    ## 746  12/15/1998  2.702703e-02
    ## 747  12/23/1998  0.000000e+00
    ## 748  12/14/1998  2.267574e-02
    ## 749  12/11/1998  2.494331e-02
    ## 750  12/17/1998  0.000000e+00
    ## 751  12/17/1998  6.802721e-03
    ## 752  11/27/1998  9.132420e-03
    ## 753  12/10/1998  2.054795e-02
    ## 754  11/30/1998 -2.283105e-03
    ## 755  12/28/1998  3.196347e-02
    ## 756  12/22/1998  1.149425e-02
    ## 757  12/10/1998  1.839080e-02
    ## 758  12/11/1998  1.149425e-02
    ## 759  12/22/1998  1.149425e-02
    ## 760  12/21/1998  9.195402e-03
    ## 761  11/27/1998  0.000000e+00
    ## 762  12/10/1998  2.777778e-02
    ## 763  12/11/1998  2.314815e-03
    ## 764  12/15/1998  6.481481e-02
    ## 765  12/10/1998  6.944444e-03
    ## 766  12/22/1998  5.324074e-02
    ## 767  12/28/1998  1.388889e-02
    ## 768  11/24/1998  6.526807e-02
    ## 769  12/08/1998  0.000000e+00
    ## 770  12/29/1998  1.165501e-02
    ## 771  12/15/1998  2.797203e-02
    ## 772  12/17/1998  0.000000e+00
    ## 773  12/28/1998  1.864802e-02
    ## 774  12/17/1998 -3.356808e-01
    ## 775  12/14/1998  4.460094e-02
    ## 776  12/01/1998  1.182033e-02
    ## 777  11/24/1998  1.418440e-02
    ## 778  12/17/1998 -2.364066e-03
    ## 779  12/03/1998 -2.142857e-02
    ## 780  12/22/1998  9.523810e-03
    ## 781  12/14/1998  2.380952e-03
    ## 782  12/27/1998  1.904762e-02
    ## 783  12/01/1998  2.857143e-02
    ## 784  11/30/1998  4.761905e-03
    ## 785  12/01/1998  0.000000e+00
    ## 786  11/26/1998  0.000000e+00
    ## 787  11/26/1998  9.523810e-03
    ## 788  12/23/1998 -2.380952e-03
    ## 789  11/23/1998  0.000000e+00
    ## 790  12/31/1998  2.398082e-03
    ## 791  12/04/1998  4.076739e-02
    ## 792  12/17/1998  4.796163e-03
    ## 793  12/09/1998  0.000000e+00
    ## 794  11/24/1998  3.381643e-02
    ## 795  12/17/1998 -3.357488e-01
    ## 796  12/22/1998  2.173913e-02
    ## 797  12/22/1998  1.216545e-02
    ## 798  12/23/1998  2.189781e-02
    ## 799  12/11/1998  7.299270e-03
    ## 800  12/17/1998 -2.433090e-03
    ## 801  12/22/1998  4.656863e-02
    ## 802  12/22/1998  4.656863e-02
    ## 803  12/17/1998  3.186275e-02
    ## 804  12/17/1998  9.803922e-03
    ## 805  12/23/1998 -3.357843e-01
    ## 806  12/17/1998  3.703704e-02
    ## 807  12/23/1998  2.469136e-02
    ## 808  12/21/1998 -2.691358e-01
    ## 809  12/14/1998  3.950617e-02
    ## 810  12/25/1998  1.481481e-02
    ## 811  12/09/1998  4.938272e-03
    ## 812  12/04/1998  3.209877e-02
    ## 813  12/04/1998  0.000000e+00
    ## 814  12/17/1998  0.000000e+00
    ## 815  11/04/1998  1.975309e-02
    ## 816  12/14/1998  2.469136e-02
    ## 817  12/18/1998  4.938272e-03
    ## 818  12/28/1998  0.000000e+00
    ## 819  11/27/1998  9.950249e-03
    ## 820  12/03/1998  7.462687e-03
    ## 821  12/11/1998  4.975124e-02
    ## 822  12/22/1998  2.487562e-03
    ## 823  12/04/1998  1.492537e-02
    ## 824  12/11/1998  1.492537e-02
    ## 825  12/02/1998  7.462687e-03
    ## 826  12/04/1998  1.243781e-02
    ## 827  12/02/1998  1.503759e-02
    ## 828  12/22/1998 -1.503759e-02
    ## 829  11/25/1998  5.012531e-03
    ## 830  11/30/1998  0.000000e+00
    ## 831  11/30/1998  5.012531e-03
    ## 832  12/15/1998  4.761905e-02
    ## 833  12/23/1998  1.754386e-02
    ## 834  11/27/1998  7.575758e-03
    ## 835  12/02/1998  2.525253e-03
    ## 836  12/10/1998  4.292929e-02
    ## 837  12/11/1998  5.050505e-03
    ## 838  12/17/1998  4.040404e-02
    ## 839  12/22/1998  7.575758e-03
    ## 840  12/22/1998  1.767677e-02
    ## 841  12/23/1998  1.262626e-02
    ## 842  12/22/1998  1.767677e-02
    ## 843  12/10/1998  0.000000e+00
    ## 844  12/28/1998  5.050505e-03
    ## 845  12/22/1998  0.000000e+00
    ## 846  12/10/1998 -3.358779e-01
    ## 847  12/17/1998  2.544529e-03
    ## 848  11/23/1998  1.526718e-02
    ## 849  12/16/1998  5.089059e-03
    ## 850  12/02/1998  1.025641e-02
    ## 851  12/10/1998  2.564103e-03
    ## 852  12/08/1998  7.692308e-03
    ## 853  12/01/1998  5.128205e-03
    ## 854  12/18/1998  1.025641e-02
    ## 855  12/23/1998  0.000000e+00
    ## 856  11/24/1998 -2.506460e-01
    ## 857  11/24/1998  1.033592e-02
    ## 858  12/02/1998  1.550388e-02
    ## 859  12/17/1998  3.617571e-02
    ## 860  12/14/1998  3.617571e-02
    ## 861  11/24/1998  7.493540e-02
    ## 862  11/24/1998  0.000000e+00
    ## 863  12/15/1998  2.583979e-03
    ## 864  12/17/1998  1.291990e-02
    ## 865  11/23/1998  7.552083e-02
    ## 866  12/11/1998  2.864583e-02
    ## 867  12/23/1998  1.822917e-02
    ## 868  12/09/1998 -1.302083e-02
    ## 869  12/22/1998  1.302083e-02
    ## 870  12/02/1998  3.412073e-02
    ## 871  12/23/1998  2.624672e-03
    ## 872  11/24/1998  3.015873e-01
    ## 873  12/14/1998  1.587302e-02
    ## 874  12/23/1998  1.058201e-02
    ## 875  11/26/1998  4.000000e-02
    ## 876  12/22/1998  4.266667e-02
    ## 877  12/27/1998  2.133333e-02
    ## 878  12/15/1998  5.333333e-03
    ## 879  12/10/1998  1.333333e-02
    ## 880  12/10/1998  1.653333e-01
    ## 881  12/17/1998  2.666667e-03
    ## 882  12/09/1998  2.688172e-03
    ## 883  12/10/1998  1.612903e-02
    ## 884  12/17/1998  0.000000e+00
    ## 885  12/22/1998  2.688172e-03
    ## 886  12/02/1998  2.168022e-02
    ## 887  11/23/1998  2.710027e-03
    ## 888  12/25/1998  3.523035e-02
    ## 889  12/04/1998  1.897019e-02
    ## 890  12/04/1998  2.439024e-02
    ## 891  12/17/1998  1.084011e-02
    ## 892  12/23/1998  1.355014e-02
    ## 893  12/01/1998 -3.333333e-01
    ## 894  12/17/1998  0.000000e+00
    ## 895  12/02/1998  6.557377e-02
    ## 896  12/03/1998  8.196721e-03
    ## 897  12/15/1998  5.464481e-03
    ## 898  12/03/1998  1.366120e-02
    ## 899  12/16/1998  2.732240e-03
    ## 900  12/17/1998  2.754821e-03
    ## 901  12/22/1998  5.234160e-02
    ## 902  11/24/1998  0.000000e+00
    ## 903  12/15/1998  0.000000e+00
    ## 904  12/02/1998  4.132231e-02
    ## 905  12/17/1998  0.000000e+00
    ## 906  12/04/1998 -3.583333e-01
    ## 907  12/21/1998  2.500000e-02
    ## 908  12/14/1998  8.333333e-03
    ## 909  12/14/1998  5.555556e-03
    ## 910  12/02/1998 -3.361111e-01
    ## 911  12/03/1998  1.388889e-02
    ## 912  12/15/1998  3.055556e-02
    ## 913  12/23/1998  8.333333e-03
    ## 914  12/22/1998  1.400560e-02
    ## 915  12/22/1998  2.521008e-02
    ## 916  12/22/1998  8.403361e-03
    ## 917  12/10/1998  2.801120e-03
    ## 918  12/09/1998  1.120448e-02
    ## 919  11/27/1998  8.474576e-03
    ## 920  11/27/1998  0.000000e+00
    ## 921  12/02/1998  5.367232e-02
    ## 922  12/02/1998  2.824859e-03
    ## 923  12/15/1998  1.694915e-02
    ## 924  11/24/1998  4.802260e-02
    ## 925  12/22/1998  0.000000e+00
    ## 926  12/11/1998  0.000000e+00
    ## 927  12/01/1998  0.000000e+00
    ## 928  12/01/1998 -2.564103e-02
    ## 929  12/08/1998  2.849003e-03
    ## 930  12/22/1998  1.709402e-02
    ## 931  12/15/1998  7.977208e-02
    ## 932  12/08/1998 -2.298851e-02
    ## 933  12/10/1998  9.482759e-02
    ## 934  12/23/1998  0.000000e+00
    ## 935  11/24/1998  4.310345e-02
    ## 936  12/02/1998  2.873563e-02
    ## 937  12/15/1998  1.149425e-02
    ## 938  12/19/1998  3.160920e-02
    ## 939  11/23/1998  8.695652e-03
    ## 940  12/04/1998  8.695652e-03
    ## 941  12/03/1998  2.898551e-03
    ## 942  12/10/1998  2.898551e-03
    ## 943  12/14/1998  2.028986e-02
    ## 944  12/04/1998  0.000000e+00
    ## 945  12/01/1998  2.028986e-02
    ## 946  12/15/1998  5.797101e-03
    ## 947  12/09/1998  4.057971e-02
    ## 948  12/11/1998  5.797101e-03
    ## 949  11/30/1998  2.028986e-02
    ## 950  12/03/1998  8.695652e-03
    ## 951  11/27/1998  4.093567e-02
    ## 952  11/24/1998  2.631579e-02
    ## 953  12/08/1998  0.000000e+00
    ## 954  12/17/1998  8.849558e-03
    ## 955  12/01/1998 -4.719764e-02
    ## 956  12/10/1998  2.064897e-02
    ## 957  11/30/1998  0.000000e+00
    ## 958  11/24/1998  2.976190e-02
    ## 959  12/17/1998  1.190476e-02
    ## 960  12/02/1998 -2.380952e-02
    ## 961  12/14/1998 -5.952381e-03
    ## 962  12/01/1998  0.000000e+00
    ## 963  12/18/1998  1.190476e-02
    ## 964  11/27/1998  3.003003e-03
    ## 965  12/11/1998  2.702703e-02
    ## 966  12/01/1998  0.000000e+00
    ## 967  12/02/1998  3.003003e-03
    ## 968  12/04/1998  6.006006e-03
    ## 969  12/15/1998  2.402402e-02
    ## 970  11/25/1998  1.801802e-02
    ## 971  11/25/1998  9.009009e-03
    ## 972  12/02/1998  1.801802e-02
    ## 973  12/04/1998  3.003003e-03
    ## 974  12/04/1998  3.003003e-03
    ## 975  12/17/1998  0.000000e+00
    ## 976  12/17/1998  0.000000e+00
    ## 977  12/02/1998  3.003003e-03
    ## 978  12/22/1998  6.006006e-03
    ## 979  11/24/1998  3.333333e-02
    ## 980  12/27/1998 -3.606061e-01
    ## 981  12/15/1998  1.818182e-02
    ## 982  12/01/1998 -6.060606e-03
    ## 983  12/04/1998  2.424242e-02
    ## 984  12/08/1998  6.363636e-02
    ## 985  12/10/1998 -3.333333e-02
    ## 986  12/15/1998  1.515152e-02
    ## 987  12/02/1998 -1.000000e-01
    ## 988  12/10/1998  2.727273e-02
    ## 989  12/17/1998  3.030303e-03
    ## 990  12/17/1998  9.090909e-03
    ## 991  12/18/1998  1.515152e-02
    ## 992  11/27/1998  3.058104e-03
    ## 993  12/02/1998  0.000000e+00
    ## 994  12/10/1998  3.363914e-02
    ## 995  12/04/1998  9.174312e-03
    ## 996  12/09/1998  3.363914e-02
    ## 997  11/30/1998 -3.058104e-03
    ## 998  12/02/1998  0.000000e+00
    ## 999  12/09/1998  3.058104e-03
    ## 1000 12/10/1998  2.446483e-02
    ## 1001 12/22/1998  4.587156e-02
    ## 1002 12/10/1998  9.174312e-02
    ## 1003 12/16/1998  1.834862e-02
    ## 1004 12/17/1998  4.281346e-02
    ## 1005 12/18/1998  2.140673e-02
    ## 1006 11/24/1998  9.259259e-03
    ## 1007 12/02/1998  1.851852e-02
    ## 1008 12/02/1998  5.246914e-02
    ## 1009 12/10/1998  2.160494e-02
    ## 1010 11/23/1998  1.851852e-02
    ## 1011 12/04/1998  9.259259e-03
    ## 1012 12/04/1998  4.629630e-02
    ## 1013 12/14/1998  0.000000e+00
    ## 1014 12/22/1998  4.049844e-02
    ## 1015 12/22/1998  9.345794e-03
    ## 1016 12/23/1998  3.426791e-02
    ## 1017 12/27/1998  9.345794e-03
    ## 1018 12/04/1998  1.869159e-02
    ## 1019 12/11/1998  3.115265e-03
    ## 1020 11/04/1998  3.115265e-02
    ## 1021 12/14/1998  1.557632e-02
    ## 1022 12/22/1998  1.557632e-02
    ## 1023 12/22/1998  9.345794e-03
    ## 1024 12/04/1998  9.433962e-03
    ## 1025 12/22/1998  0.000000e+00
    ## 1026 12/22/1998  3.144654e-02
    ## 1027 12/29/1998  1.037736e-01
    ## 1028 11/23/1998 -6.289308e-03
    ## 1029 12/02/1998  3.144654e-02
    ## 1030 12/01/1998  0.000000e+00
    ## 1031 12/04/1998  0.000000e+00
    ## 1032 12/09/1998  0.000000e+00
    ## 1033 12/04/1998  1.886792e-02
    ## 1034 11/24/1998  2.222222e-02
    ## 1035 11/27/1998  0.000000e+00
    ## 1036 12/31/1998  1.587302e-02
    ## 1037 12/10/1998  2.222222e-02
    ## 1038 12/10/1998  3.174603e-03
    ## 1039 12/16/1998  1.587302e-02
    ## 1040 11/04/1998  3.174603e-02
    ## 1041 12/17/1998  6.349206e-03
    ## 1042 12/22/1998  1.587302e-02
    ## 1043 12/23/1998  0.000000e+00
    ## 1044 12/10/1998  2.243590e-02
    ## 1045 12/23/1998  3.205128e-03
    ## 1046 12/04/1998  9.615385e-03
    ## 1047 12/15/1998  3.525641e-02
    ## 1048 11/24/1998  2.564103e-02
    ## 1049 11/24/1998  2.884615e-02
    ## 1050 12/02/1998 -3.365385e-01
    ## 1051 12/04/1998  1.618123e-02
    ## 1052 12/15/1998  1.294498e-02
    ## 1053 12/15/1998  0.000000e+00
    ## 1054 12/01/1998  1.618123e-02
    ## 1055 12/01/1998  0.000000e+00
    ## 1056 12/02/1998  9.708738e-03
    ## 1057 12/04/1998  2.265372e-02
    ## 1058 11/30/1998  3.236246e-02
    ## 1059 12/17/1998  1.941748e-02
    ## 1060 12/23/1998  1.294498e-02
    ## 1061 12/23/1998  5.825243e-02
    ## 1062 12/22/1998  2.614379e-02
    ## 1063 12/01/1998  3.267974e-03
    ## 1064 12/01/1998 -3.366013e-01
    ## 1065 12/10/1998  1.307190e-02
    ## 1066 12/09/1998  7.189542e-02
    ## 1067 12/17/1998  1.633987e-02
    ## 1068 12/04/1998  3.300330e-02
    ## 1069 12/17/1998  4.950495e-02
    ## 1070 12/22/1998  2.640264e-02
    ## 1071 12/27/1998 -3.630363e-01
    ## 1072 12/31/1998  3.300330e-03
    ## 1073 12/10/1998  6.600660e-03
    ## 1074 12/01/1998  1.320132e-02
    ## 1075 12/10/1998  3.300330e-03
    ## 1076 12/03/1998  2.333333e-02
    ## 1077 12/17/1998  6.666667e-03
    ## 1078 12/17/1998  3.666667e-02
    ## 1079 12/23/1998  3.666667e-02
    ## 1080 11/30/1998  3.333333e-03
    ## 1081 12/02/1998  1.333333e-02
    ## 1082 12/08/1998  5.666667e-02
    ## 1083 12/11/1998  2.000000e-02
    ## 1084 12/17/1998  3.333333e-03
    ## 1085 11/24/1998  0.000000e+00
    ## 1086 11/26/1998 -1.010101e-02
    ## 1087 12/09/1998  2.020202e-02
    ## 1088 12/04/1998  2.020202e-02
    ## 1089 12/09/1998  2.020202e-02
    ## 1090 12/15/1998  1.683502e-02
    ## 1091 12/15/1998  2.356902e-02
    ## 1092 12/09/1998  3.367003e-03
    ## 1093 12/22/1998  2.020202e-02
    ## 1094 12/08/1998  3.367003e-03
    ## 1095 12/22/1998  3.061224e-02
    ## 1096 12/22/1998  3.741497e-02
    ## 1097 12/22/1998  1.020408e-02
    ## 1098 12/04/1998  1.360544e-02
    ## 1099 12/21/1998  5.782313e-02
    ## 1100 12/28/1998  6.802721e-03
    ## 1101 12/11/1998  1.030928e-02
    ## 1102 12/17/1998  2.749141e-02
    ## 1103 11/24/1998  1.030928e-02
    ## 1104 12/22/1998  3.092784e-02
    ## 1105 12/21/1998  1.030928e-02
    ## 1106 12/23/1998 -3.367698e-01
    ## 1107 12/25/1998  1.388889e-02
    ## 1108 12/02/1998  1.041667e-02
    ## 1109 12/17/1998  1.736111e-02
    ## 1110 12/15/1998  7.291667e-02
    ## 1111 11/23/1998  1.403509e-02
    ## 1112 11/27/1998  3.859649e-02
    ## 1113 12/04/1998  2.456140e-02
    ## 1114 12/22/1998  0.000000e+00
    ## 1115 12/27/1998  3.508772e-03
    ## 1116 12/01/1998  4.561404e-02
    ## 1117 12/09/1998  7.017544e-03
    ## 1118 12/11/1998  4.561404e-02
    ## 1119 11/30/1998  7.017544e-03
    ## 1120 11/30/1998  0.000000e+00
    ## 1121 12/02/1998 -2.526316e-01
    ## 1122 12/02/1998  3.508772e-02
    ## 1123 12/28/1998  7.017544e-03
    ## 1124 12/14/1998  6.737589e-02
    ## 1125 12/15/1998  3.546099e-03
    ## 1126 12/02/1998  1.773050e-02
    ## 1127 12/14/1998  3.546099e-03
    ## 1128 12/18/1998  1.063830e-02
    ## 1129 12/17/1998  2.508961e-02
    ## 1130 12/22/1998  2.508961e-02
    ## 1131 12/23/1998  7.168459e-03
    ## 1132 12/23/1998  2.508961e-02
    ## 1133 12/31/1998  1.433692e-02
    ## 1134 11/24/1998  1.075269e-02
    ## 1135 11/30/1998  3.584229e-03
    ## 1136 12/02/1998  7.168459e-03
    ## 1137 12/11/1998  2.508961e-02
    ## 1138 12/14/1998  3.225806e-02
    ## 1139 12/04/1998 -3.369176e-01
    ## 1140 12/18/1998  7.168459e-03
    ## 1141 12/17/1998  2.536232e-02
    ## 1142 12/04/1998  1.086957e-02
    ## 1143 12/10/1998  7.246377e-03
    ## 1144 12/15/1998  1.086957e-02
    ## 1145 12/03/1998  9.420290e-02
    ## 1146 12/04/1998  3.260870e-02
    ## 1147 12/04/1998  2.898551e-02
    ## 1148 12/10/1998  2.536232e-02
    ## 1149 12/16/1998  5.434783e-02
    ## 1150 11/26/1998  7.246377e-03
    ## 1151 12/17/1998  2.173913e-02
    ## 1152 12/17/1998  0.000000e+00
    ## 1153 12/22/1998  1.086957e-02
    ## 1154 12/28/1998  0.000000e+00
    ## 1155 12/28/1998  3.623188e-02
    ## 1156 11/24/1998  2.197802e-02
    ## 1157 11/24/1998  1.465201e-02
    ## 1158 11/26/1998  1.208791e-01
    ## 1159 12/04/1998  7.326007e-03
    ## 1160 11/23/1998  1.098901e-02
    ## 1161 12/31/1998  6.593407e-02
    ## 1162 12/02/1998  1.098901e-02
    ## 1163 12/09/1998  3.663004e-03
    ## 1164 12/15/1998  3.663004e-03
    ## 1165 11/24/1998  1.831502e-02
    ## 1166 12/04/1998  3.296703e-02
    ## 1167 12/16/1998  2.197802e-02
    ## 1168 12/15/1998  1.831502e-02
    ## 1169 12/01/1998  3.703704e-03
    ## 1170 12/02/1998 -3.370370e-01
    ## 1171 12/10/1998  6.296296e-02
    ## 1172 12/17/1998  1.481481e-02
    ## 1173 12/31/1998  1.481481e-02
    ## 1174 12/01/1998  0.000000e+00
    ## 1175 11/25/1998  1.111111e-02
    ## 1176 12/01/1998  7.407407e-03
    ## 1177 12/19/1998  5.185185e-02
    ## 1178 11/26/1998  7.407407e-03
    ## 1179 12/17/1998  3.703704e-02
    ## 1180 11/27/1998 -3.745318e-03
    ## 1181 12/03/1998  0.000000e+00
    ## 1182 12/17/1998  1.872659e-02
    ## 1183 12/23/1998  7.490637e-03
    ## 1184 12/17/1998  2.996255e-02
    ## 1185 11/25/1998  7.490637e-03
    ## 1186 12/08/1998  5.617978e-02
    ## 1187 12/15/1998 -3.745318e-03
    ## 1188 12/11/1998  3.745318e-02
    ## 1189 12/17/1998  1.872659e-02
    ## 1190 11/30/1998  7.490637e-03
    ## 1191 12/17/1998  1.123596e-02
    ## 1192 12/22/1998  1.123596e-02
    ## 1193 11/27/1998  3.787879e-03
    ## 1194 12/27/1998  2.651515e-02
    ## 1195 12/31/1998  1.136364e-02
    ## 1196 12/04/1998  0.000000e+00
    ## 1197 12/08/1998  6.060606e-02
    ## 1198 12/11/1998  0.000000e+00
    ## 1199 12/01/1998  3.787879e-03
    ## 1200 12/01/1998  0.000000e+00
    ## 1201 12/09/1998  4.545455e-02
    ## 1202 12/15/1998 -7.575758e-03
    ## 1203 12/22/1998  0.000000e+00
    ## 1204 12/15/1998  7.575758e-03
    ## 1205 12/22/1998  7.575758e-03
    ## 1206 12/29/1998  1.915709e-02
    ## 1207 12/25/1998  3.831418e-02
    ## 1208 12/01/1998  1.915709e-02
    ## 1209 12/10/1998  0.000000e+00
    ## 1210 12/01/1998  1.149425e-02
    ## 1211 12/17/1998  1.532567e-02
    ## 1212 12/23/1998  0.000000e+00
    ## 1213 12/21/1998  4.597701e-02
    ## 1214 12/23/1998  0.000000e+00
    ## 1215 12/02/1998  3.488372e-02
    ## 1216 12/25/1998  1.937984e-02
    ## 1217 12/14/1998  2.713178e-02
    ## 1218 12/01/1998 -3.875969e-03
    ## 1219 12/04/1998  1.937984e-02
    ## 1220 12/16/1998  2.713178e-02
    ## 1221 11/30/1998  1.550388e-02
    ## 1222 12/03/1998  3.875969e-03
    ## 1223 12/16/1998  3.100775e-02
    ## 1224 12/17/1998  0.000000e+00
    ## 1225 12/17/1998  3.875969e-03
    ## 1226 12/17/1998  3.875969e-03
    ## 1227 12/21/1998  3.875969e-03
    ## 1228 12/21/1998  2.713178e-02
    ## 1229 11/23/1998  6.666667e-02
    ## 1230 12/27/1998  1.176471e-02
    ## 1231 12/01/1998  3.137255e-02
    ## 1232 12/17/1998  1.568627e-02
    ## 1233 12/22/1998  1.176471e-02
    ## 1234 12/10/1998  7.936508e-03
    ## 1235 12/22/1998 -3.333333e-01
    ## 1236 12/22/1998  3.968254e-02
    ## 1237 12/14/1998  2.380952e-02
    ## 1238 11/24/1998  1.230159e-01
    ## 1239 12/01/1998 -8.730159e-02
    ## 1240 12/14/1998  1.190476e-02
    ## 1241 12/01/1998  8.730159e-02
    ## 1242 12/11/1998  2.380952e-02
    ## 1243 12/22/1998  1.190476e-02
    ## 1244 11/24/1998 -2.811245e-02
    ## 1245 12/04/1998  2.811245e-02
    ## 1246 12/08/1998  5.622490e-02
    ## 1247 12/08/1998  0.000000e+00
    ## 1248 12/29/1998  8.032129e-03
    ## 1249 12/27/1998  0.000000e+00
    ## 1250 12/02/1998  4.016064e-03
    ## 1251 11/24/1998  4.016064e-02
    ## 1252 12/03/1998  4.016064e-03
    ## 1253 12/15/1998 -8.032129e-03
    ## 1254 12/10/1998  4.819277e-02
    ## 1255 12/10/1998  2.811245e-02
    ## 1256 12/17/1998  8.032129e-03
    ## 1257 12/18/1998  2.008032e-02
    ## 1258 12/18/1998  1.204819e-02
    ## 1259 12/18/1998  4.016064e-03
    ## 1260 12/23/1998  5.220884e-02
    ## 1261 11/27/1998  4.065041e-03
    ## 1262 12/23/1998  1.219512e-02
    ## 1263 11/24/1998  5.691057e-02
    ## 1264 12/08/1998  9.349593e-02
    ## 1265 12/01/1998  2.032520e-02
    ## 1266 12/09/1998  4.065041e-03
    ## 1267 12/15/1998  2.439024e-02
    ## 1268 12/22/1998  4.065041e-03
    ## 1269 12/03/1998  0.000000e+00
    ## 1270 12/18/1998  4.471545e-02
    ## 1271 12/02/1998  2.057613e-02
    ## 1272 12/08/1998  8.230453e-03
    ## 1273 12/22/1998  4.115226e-03
    ## 1274 12/14/1998  1.646091e-02
    ## 1275 12/11/1998  0.000000e+00
    ## 1276 12/11/1998  0.000000e+00
    ## 1277 12/15/1998  3.703704e-02
    ## 1278 12/03/1998  5.349794e-02
    ## 1279 12/11/1998  4.115226e-02
    ## 1280 12/16/1998  1.646091e-02
    ## 1281 12/03/1998  4.115226e-03
    ## 1282 12/10/1998  1.111111e-01
    ## 1283 12/10/1998  0.000000e+00
    ## 1284 12/14/1998  6.584362e-02
    ## 1285 12/22/1998  1.234568e-02
    ## 1286 12/23/1998  8.230453e-03
    ## 1287 12/28/1998  4.115226e-03
    ## 1288 12/22/1998  0.000000e+00
    ## 1289 12/01/1998  8.333333e-03
    ## 1290 12/01/1998  8.333333e-03
    ## 1291 12/01/1998  0.000000e+00
    ## 1292 12/09/1998  4.166667e-03
    ## 1293 11/30/1998  0.000000e+00
    ## 1294 12/01/1998  8.333333e-03
    ## 1295 12/04/1998  5.833333e-02
    ## 1296 12/08/1998  0.000000e+00
    ## 1297 12/11/1998  1.250000e-02
    ## 1298 12/15/1998 -3.375000e-01
    ## 1299 12/17/1998  2.500000e-02
    ## 1300 12/10/1998  3.333333e-02
    ## 1301 12/17/1998  1.666667e-02
    ## 1302 12/18/1998  8.333333e-03
    ## 1303 12/18/1998  4.166667e-03
    ## 1304 12/23/1998  3.333333e-02
    ## 1305 12/28/1998  2.083333e-02
    ## 1306 12/04/1998  1.687764e-02
    ## 1307 12/02/1998  1.687764e-02
    ## 1308 12/21/1998 -2.953586e-02
    ## 1309 12/29/1998  1.687764e-02
    ## 1310 11/23/1998  8.438819e-03
    ## 1311 12/04/1998  1.265823e-02
    ## 1312 12/01/1998 -3.713080e-01
    ## 1313 12/03/1998  2.531646e-02
    ## 1314 12/10/1998  0.000000e+00
    ## 1315 12/11/1998 -1.265823e-02
    ## 1316 12/14/1998  4.219409e-03
    ## 1317 12/02/1998  8.438819e-03
    ## 1318 12/11/1998  0.000000e+00
    ## 1319 12/11/1998  0.000000e+00
    ## 1320 12/15/1998  2.109705e-02
    ## 1321 12/15/1998  8.438819e-03
    ## 1322 12/22/1998  4.641350e-02
    ## 1323 12/02/1998  0.000000e+00
    ## 1324 12/04/1998  1.687764e-02
    ## 1325 12/04/1998  4.219409e-03
    ## 1326 12/10/1998  2.109705e-01
    ## 1327 12/18/1998  1.265823e-02
    ## 1328 11/23/1998  0.000000e+00
    ## 1329 12/02/1998  8.547009e-03
    ## 1330 12/08/1998 -3.376068e-01
    ## 1331 12/10/1998  2.991453e-02
    ## 1332 12/27/1998  0.000000e+00
    ## 1333 12/31/1998  0.000000e+00
    ## 1334 11/25/1998  4.273504e-03
    ## 1335 12/14/1998  4.273504e-03
    ## 1336 12/01/1998 -4.273504e-03
    ## 1337 12/08/1998  0.000000e+00
    ## 1338 12/11/1998  1.282051e-02
    ## 1339 12/19/1998  8.547009e-03
    ## 1340 11/30/1998 -4.273504e-02
    ## 1341 12/23/1998  4.273504e-03
    ## 1342 12/10/1998 -4.329004e-03
    ## 1343 12/23/1998 -2.597403e-02
    ## 1344 12/23/1998  5.194805e-02
    ## 1345 12/23/1998 -3.376623e-01
    ## 1346 12/21/1998 -3.333333e-01
    ## 1347 12/22/1998  0.000000e+00
    ## 1348 12/27/1998  2.597403e-02
    ## 1349 12/27/1998 -3.376623e-01
    ## 1350 12/27/1998 -8.658009e-03
    ## 1351 12/02/1998  4.329004e-03
    ## 1352 12/04/1998  8.658009e-03
    ## 1353 12/10/1998 -3.463203e-02
    ## 1354 12/03/1998  0.000000e+00
    ## 1355 12/14/1998  5.627706e-02
    ## 1356 12/14/1998  3.030303e-02
    ## 1357 12/01/1998 -4.329004e-03
    ## 1358 12/10/1998  1.731602e-02
    ## 1359 12/11/1998 -5.627706e-02
    ## 1360 12/15/1998  1.731602e-02
    ## 1361 12/15/1998  4.329004e-02
    ## 1362 12/17/1998  5.627706e-02
    ## 1363 12/17/1998  1.731602e-02
    ## 1364 12/17/1998  2.192982e-02
    ## 1365 12/29/1998 -8.771930e-03
    ## 1366 12/01/1998 -3.377193e-01
    ## 1367 12/04/1998  1.315789e-02
    ## 1368 12/11/1998  8.771930e-03
    ## 1369 11/30/1998  4.385965e-03
    ## 1370 12/02/1998  0.000000e+00
    ## 1371 12/04/1998  5.263158e-02
    ## 1372 12/10/1998 -8.771930e-03
    ## 1373 12/15/1998  0.000000e+00
    ## 1374 12/01/1998 -4.385965e-03
    ## 1375 12/01/1998  0.000000e+00
    ## 1376 12/04/1998  8.771930e-03
    ## 1377 12/04/1998  3.070175e-02
    ## 1378 12/16/1998  4.385965e-02
    ## 1379 12/16/1998  2.631579e-02
    ## 1380 12/19/1998  4.385965e-03
    ## 1381 11/04/1998  3.508772e-02
    ## 1382 12/04/1998  5.263158e-02
    ## 1383 12/16/1998  1.754386e-02
    ## 1384 12/18/1998  8.771930e-03
    ## 1385 11/24/1998  1.777778e-02
    ## 1386 11/27/1998  5.333333e-02
    ## 1387 12/01/1998  0.000000e+00
    ## 1388 12/04/1998  1.777778e-02
    ## 1389 12/03/1998  4.444444e-03
    ## 1390 12/04/1998  4.444444e-03
    ## 1391 12/10/1998  0.000000e+00
    ## 1392 12/23/1998 -3.377778e-01
    ## 1393 12/01/1998  0.000000e+00
    ## 1394 12/09/1998  3.111111e-02
    ## 1395 12/15/1998  8.888889e-03
    ## 1396 11/24/1998 -4.000000e-02
    ## 1397 11/30/1998  8.888889e-03
    ## 1398 12/14/1998  1.333333e-02
    ## 1399 12/15/1998  4.444444e-03
    ## 1400 12/14/1998  5.333333e-02
    ## 1401 12/15/1998  4.444444e-02
    ## 1402 12/22/1998  5.333333e-02
    ## 1403 12/15/1998  2.666667e-02
    ## 1404 12/17/1998 -3.377778e-01
    ## 1405 12/23/1998  2.666667e-02
    ## 1406 12/28/1998  2.222222e-02
    ## 1407 11/23/1998  6.306306e-02
    ## 1408 11/27/1998  4.504505e-03
    ## 1409 12/23/1998  2.702703e-02
    ## 1410 12/17/1998 -9.009009e-03
    ## 1411 12/22/1998  4.504505e-03
    ## 1412 12/23/1998  2.252252e-02
    ## 1413 11/23/1998 -9.009009e-03
    ## 1414 11/23/1998  6.306306e-02
    ## 1415 12/09/1998  9.009009e-03
    ## 1416 11/24/1998 -1.621622e-01
    ## 1417 11/24/1998  0.000000e+00
    ## 1418 11/24/1998  9.009009e-03
    ## 1419 11/24/1998  9.009009e-03
    ## 1420 12/01/1998  2.702703e-02
    ## 1421 12/14/1998 -1.801802e-02
    ## 1422 12/19/1998 -2.522523e-01
    ## 1423 12/19/1998  4.954955e-02
    ## 1424 11/30/1998 -3.378378e-01
    ## 1425 12/17/1998  2.702703e-02
    ## 1426 12/17/1998  2.252252e-02
    ## 1427 12/17/1998 -1.801802e-02
    ## 1428 12/21/1998  0.000000e+00
    ## 1429 11/23/1998  0.000000e+00
    ## 1430 11/23/1998  1.369863e-02
    ## 1431 12/04/1998  9.132420e-03
    ## 1432 12/02/1998  4.566210e-03
    ## 1433 12/09/1998  0.000000e+00
    ## 1434 12/11/1998  1.826484e-02
    ## 1435 11/25/1998  1.369863e-02
    ## 1436 12/03/1998  5.479452e-02
    ## 1437 12/03/1998 -2.557078e-01
    ## 1438 12/04/1998  5.022831e-02
    ## 1439 12/16/1998  0.000000e+00
    ## 1440 11/25/1998  1.369863e-02
    ## 1441 12/14/1998  4.566210e-02
    ## 1442 12/23/1998 -3.333333e-01
    ## 1443 12/02/1998  2.777778e-02
    ## 1444 12/04/1998 -4.629630e-03
    ## 1445 12/10/1998  3.240741e-02
    ## 1446 12/17/1998  0.000000e+00
    ## 1447 12/23/1998  9.722222e-02
    ## 1448 12/21/1998  4.629630e-03
    ## 1449 12/14/1998  4.629630e-03
    ## 1450 12/14/1998  4.629630e-02
    ## 1451 12/15/1998  0.000000e+00
    ## 1452 11/24/1998  9.259259e-03
    ## 1453 11/25/1998  4.166667e-02
    ## 1454 12/08/1998  4.629630e-03
    ## 1455 12/14/1998 -3.703704e-02
    ## 1456 12/04/1998  1.851852e-02
    ## 1457 12/09/1998  0.000000e+00
    ## 1458 12/09/1998  9.259259e-03
    ## 1459 12/10/1998 -9.259259e-03
    ## 1460 12/10/1998 -2.314815e-01
    ## 1461 12/15/1998  4.629630e-03
    ## 1462 12/22/1998 -2.592593e-01
    ## 1463 12/22/1998  0.000000e+00
    ## 1464 12/02/1998  3.240741e-02
    ## 1465 12/15/1998  0.000000e+00
    ## 1466 12/18/1998  4.629630e-03
    ## 1467 12/28/1998  1.388889e-02
    ## 1468 12/28/1998 -1.851852e-02
    ## 1469 11/24/1998 -3.286385e-02
    ## 1470 12/02/1998  1.877934e-02
    ## 1471 12/10/1998 -3.380282e-01
    ## 1472 12/17/1998  4.694836e-03
    ## 1473 12/09/1998  1.877934e-02
    ## 1474 12/10/1998  4.694836e-03
    ## 1475 12/01/1998 -3.380282e-01
    ## 1476 12/08/1998  1.408451e-02
    ## 1477 12/04/1998 -9.389671e-03
    ## 1478 12/10/1998 -3.380282e-01
    ## 1479 12/15/1998  9.389671e-03
    ## 1480 12/16/1998  4.694836e-03
    ## 1481 12/19/1998  4.694836e-02
    ## 1482 11/30/1998  0.000000e+00
    ## 1483 11/30/1998  0.000000e+00
    ## 1484 11/04/1998  4.694836e-03
    ## 1485 12/17/1998  9.389671e-03
    ## 1486 12/17/1998  2.816901e-02
    ## 1487 12/17/1998  0.000000e+00
    ## 1488 11/27/1998  8.571429e-02
    ## 1489 12/02/1998  3.333333e-02
    ## 1490 12/08/1998 -9.523810e-03
    ## 1491 12/17/1998  9.523810e-03
    ## 1492 12/22/1998  5.238095e-02
    ## 1493 12/27/1998  0.000000e+00
    ## 1494 12/27/1998  0.000000e+00
    ## 1495 12/31/1998  2.380952e-02
    ## 1496 12/01/1998  0.000000e+00
    ## 1497 12/02/1998  0.000000e+00
    ## 1498 12/15/1998  4.761905e-03
    ## 1499 12/01/1998  4.285714e-02
    ## 1500 12/04/1998  0.000000e+00
    ## 1501 12/04/1998 -2.857143e-01
    ## 1502 12/14/1998  3.333333e-02
    ## 1503 12/15/1998  0.000000e+00
    ## 1504 12/02/1998  0.000000e+00
    ## 1505 12/09/1998  0.000000e+00
    ## 1506 12/16/1998  3.333333e-02
    ## 1507 11/30/1998  0.000000e+00
    ## 1508 12/04/1998  3.333333e-02
    ## 1509 12/10/1998  0.000000e+00
    ## 1510 12/15/1998  1.904762e-02
    ## 1511 12/15/1998  1.428571e-02
    ## 1512 11/23/1998  0.000000e+00
    ## 1513 12/01/1998 -9.661836e-03
    ## 1514 12/17/1998 -4.347826e-02
    ## 1515 12/21/1998  9.661836e-03
    ## 1516 12/22/1998  0.000000e+00
    ## 1517 12/01/1998 -4.347826e-02
    ## 1518 12/02/1998  3.381643e-02
    ## 1519 12/04/1998  1.932367e-02
    ## 1520 12/09/1998  1.449275e-02
    ## 1521 12/15/1998 -3.381643e-01
    ## 1522 11/30/1998 -4.830918e-03
    ## 1523 12/01/1998 -3.864734e-02
    ## 1524 12/04/1998  0.000000e+00
    ## 1525 12/04/1998  1.932367e-02
    ## 1526 12/04/1998  3.381643e-02
    ## 1527 12/08/1998  1.932367e-02
    ## 1528 12/10/1998 -5.314010e-02
    ## 1529 12/14/1998 -3.381643e-01
    ## 1530 12/01/1998 -4.830918e-03
    ## 1531 12/11/1998  1.449275e-02
    ## 1532 12/14/1998  1.111111e-01
    ## 1533 12/16/1998  0.000000e+00
    ## 1534 12/03/1998  9.661836e-03
    ## 1535 12/14/1998  4.347826e-02
    ## 1536 12/23/1998  1.449275e-02
    ## 1537 11/23/1998 -1.911765e-01
    ## 1538 11/23/1998  4.901961e-03
    ## 1539 11/23/1998  2.941176e-02
    ## 1540 11/24/1998 -2.450980e-02
    ## 1541 12/21/1998  3.431373e-02
    ## 1542 12/22/1998  0.000000e+00
    ## 1543 12/14/1998  4.901961e-03
    ## 1544 12/25/1998  1.960784e-02
    ## 1545 11/24/1998 -3.382353e-01
    ## 1546 12/15/1998  0.000000e+00
    ## 1547 12/01/1998 -4.901961e-03
    ## 1548 12/15/1998 -2.941176e-02
    ## 1549 12/15/1998  3.921569e-02
    ## 1550 12/16/1998  9.803922e-03
    ## 1551 12/16/1998  0.000000e+00
    ## 1552 12/22/1998  3.431373e-02
    ## 1553 12/17/1998  4.901961e-03
    ## 1554 12/21/1998  2.450980e-02
    ## 1555 12/23/1998  0.000000e+00
    ## 1556 11/27/1998  1.990050e-02
    ## 1557 12/01/1998  2.985075e-02
    ## 1558 12/02/1998 -3.383085e-01
    ## 1559 12/22/1998  0.000000e+00
    ## 1560 12/27/1998  3.482587e-02
    ## 1561 12/31/1998  1.492537e-02
    ## 1562 12/01/1998 -3.383085e-01
    ## 1563 12/10/1998  4.477612e-02
    ## 1564 12/01/1998 -2.487562e-02
    ## 1565 12/04/1998  2.487562e-02
    ## 1566 12/08/1998  0.000000e+00
    ## 1567 12/23/1998  0.000000e+00
    ## 1568 12/02/1998  0.000000e+00
    ## 1569 12/03/1998  1.492537e-02
    ## 1570 12/03/1998  4.975124e-03
    ## 1571 12/15/1998 -2.189055e-01
    ## 1572 12/23/1998 -3.980100e-02
    ## 1573 11/24/1998  1.515152e-02
    ## 1574 11/27/1998  5.050505e-03
    ## 1575 12/04/1998  1.010101e-02
    ## 1576 12/03/1998  5.050505e-03
    ## 1577 12/04/1998 -2.525253e-02
    ## 1578 12/04/1998  5.050505e-03
    ## 1579 12/23/1998  1.010101e-02
    ## 1580 11/24/1998  3.030303e-02
    ## 1581 11/30/1998  2.020202e-02
    ## 1582 12/04/1998 -1.010101e-02
    ## 1583 12/08/1998  3.030303e-02
    ## 1584 12/14/1998  0.000000e+00
    ## 1585 12/02/1998  2.020202e-02
    ## 1586 12/02/1998  4.040404e-02
    ## 1587 12/08/1998  0.000000e+00
    ## 1588 12/15/1998  2.020202e-02
    ## 1589 12/22/1998  3.535354e-02
    ## 1590 12/10/1998  1.010101e-02
    ## 1591 11/04/1998  0.000000e+00
    ## 1592 12/15/1998  2.020202e-02
    ## 1593 12/23/1998  0.000000e+00
    ## 1594 12/28/1998  0.000000e+00
    ## 1595 11/24/1998  5.641026e-02
    ## 1596 11/24/1998 -3.794872e-01
    ## 1597 11/26/1998  5.128205e-02
    ## 1598 12/10/1998  4.000000e-01
    ## 1599 12/10/1998  1.025641e-02
    ## 1600 12/17/1998  1.025641e-02
    ## 1601 12/21/1998  0.000000e+00
    ## 1602 12/22/1998 -1.025641e-02
    ## 1603 12/29/1998  0.000000e+00
    ## 1604 12/14/1998 -4.615385e-02
    ## 1605 12/25/1998  7.692308e-02
    ## 1606 12/27/1998  1.538462e-02
    ## 1607 12/31/1998  2.051282e-02
    ## 1608 12/09/1998  0.000000e+00
    ## 1609 12/15/1998  1.538462e-02
    ## 1610 11/24/1998  2.564103e-02
    ## 1611 12/01/1998  1.025641e-02
    ## 1612 12/14/1998  0.000000e+00
    ## 1613 12/04/1998  5.128205e-03
    ## 1614 12/04/1998 -1.538462e-02
    ## 1615 12/11/1998  0.000000e+00
    ## 1616 12/17/1998  2.564103e-02
    ## 1617 12/22/1998 -2.410256e-01
    ## 1618 11/26/1998  1.538462e-02
    ## 1619 12/22/1998  1.538462e-02
    ## 1620 12/17/1998  2.564103e-02
    ## 1621 12/23/1998  2.051282e-02
    ## 1622 11/27/1998  9.895833e-02
    ## 1623 11/27/1998  1.562500e-02
    ## 1624 12/02/1998  0.000000e+00
    ## 1625 12/08/1998 -1.041667e-02
    ## 1626 12/11/1998 -2.395833e-01
    ## 1627 12/17/1998  0.000000e+00
    ## 1628 12/21/1998  3.125000e-02
    ## 1629 12/22/1998  3.125000e-02
    ## 1630 12/14/1998  3.125000e-02
    ## 1631 12/14/1998  5.208333e-03
    ## 1632 12/04/1998 -4.166667e-02
    ## 1633 12/11/1998 -5.208333e-02
    ## 1634 12/15/1998  2.083333e-02
    ## 1635 11/24/1998  3.125000e-02
    ## 1636 12/02/1998  5.208333e-03
    ## 1637 12/04/1998 -1.562500e-02
    ## 1638 12/01/1998  1.562500e-02
    ## 1639 12/10/1998  1.041667e-01
    ## 1640 12/15/1998  0.000000e+00
    ## 1641 12/22/1998 -1.354167e-01
    ## 1642 11/26/1998  1.562500e-02
    ## 1643 11/30/1998  2.083333e-02
    ## 1644 12/16/1998  5.208333e-03
    ## 1645 12/17/1998  5.208333e-03
    ## 1646 12/28/1998  0.000000e+00
    ## 1647 12/23/1998  2.604167e-02
    ## 1648 11/26/1998  4.761905e-02
    ## 1649 12/04/1998 -5.291005e-03
    ## 1650 12/02/1998  5.291005e-03
    ## 1651 12/04/1998  5.291005e-03
    ## 1652 12/08/1998 -3.386243e-01
    ## 1653 12/23/1998  5.291005e-03
    ## 1654 12/22/1998  1.587302e-02
    ## 1655 12/27/1998  9.523810e-02
    ## 1656 12/31/1998  5.291005e-03
    ## 1657 12/01/1998  1.058201e-02
    ## 1658 11/25/1998  2.645503e-02
    ## 1659 11/25/1998  6.349206e-02
    ## 1660 12/01/1998  1.058201e-02
    ## 1661 12/02/1998  1.058201e-02
    ## 1662 12/14/1998  5.291005e-03
    ## 1663 12/15/1998 -5.291005e-03
    ## 1664 12/14/1998  0.000000e+00
    ## 1665 12/15/1998 -1.428571e-01
    ## 1666 12/22/1998  5.291005e-03
    ## 1667 12/04/1998  3.174603e-02
    ## 1668 12/04/1998  4.761905e-02
    ## 1669 12/21/1998  3.174603e-02
    ## 1670 12/23/1998 -5.291005e-03
    ## 1671 11/23/1998  2.688172e-02
    ## 1672 11/23/1998  5.376344e-03
    ## 1673 11/24/1998  2.688172e-02
    ## 1674 11/24/1998  5.376344e-03
    ## 1675 11/24/1998  0.000000e+00
    ## 1676 12/02/1998  0.000000e+00
    ## 1677 12/08/1998  1.612903e-02
    ## 1678 12/23/1998  3.225806e-02
    ## 1679 12/23/1998  1.612903e-02
    ## 1680 12/09/1998  5.376344e-03
    ## 1681 12/10/1998  2.688172e-02
    ## 1682 12/15/1998  4.838710e-02
    ## 1683 11/30/1998  5.376344e-02
    ## 1684 12/03/1998  3.225806e-02
    ## 1685 12/04/1998  4.838710e-02
    ## 1686 12/08/1998  1.612903e-02
    ## 1687 12/10/1998 -5.376344e-03
    ## 1688 12/11/1998 -3.225806e-02
    ## 1689 12/11/1998  3.763441e-02
    ## 1690 12/03/1998  3.225806e-02
    ## 1691 11/04/1998  2.688172e-02
    ## 1692 12/10/1998  0.000000e+00
    ## 1693 12/17/1998  2.688172e-02
    ## 1694 12/18/1998  2.688172e-02
    ## 1695 12/23/1998  1.075269e-02
    ## 1696 12/23/1998  2.150538e-02
    ## 1697 12/28/1998  0.000000e+00
    ## 1698 11/26/1998  3.278689e-02
    ## 1699 12/01/1998  1.639344e-02
    ## 1700 12/10/1998 -1.092896e-02
    ## 1701 12/17/1998  3.278689e-02
    ## 1702 12/22/1998  8.196721e-02
    ## 1703 12/22/1998  0.000000e+00
    ## 1704 12/23/1998  2.732240e-02
    ## 1705 12/17/1998 -3.387978e-01
    ## 1706 12/22/1998  0.000000e+00
    ## 1707 12/14/1998 -1.092896e-02
    ## 1708 12/27/1998 -1.092896e-02
    ## 1709 12/09/1998  1.092896e-02
    ## 1710 12/09/1998  5.464481e-03
    ## 1711 12/09/1998  5.464481e-03
    ## 1712 12/09/1998  0.000000e+00
    ## 1713 12/04/1998  1.639344e-02
    ## 1714 12/09/1998  8.743169e-02
    ## 1715 11/24/1998  1.092896e-02
    ## 1716 11/24/1998  6.010929e-02
    ## 1717 11/25/1998  7.103825e-02
    ## 1718 12/08/1998  9.836066e-02
    ## 1719 12/10/1998  2.185792e-02
    ## 1720 12/11/1998 -1.092896e-02
    ## 1721 12/14/1998  5.464481e-03
    ## 1722 12/01/1998 -1.092896e-02
    ## 1723 12/02/1998  1.092896e-02
    ## 1724 12/04/1998  0.000000e+00
    ## 1725 12/03/1998  3.278689e-02
    ## 1726 12/04/1998  1.639344e-02
    ## 1727 12/10/1998  1.256831e-01
    ## 1728 12/17/1998  0.000000e+00
    ## 1729 12/23/1998  5.464481e-03
    ## 1730 11/27/1998 -1.111111e-02
    ## 1731 12/22/1998  5.555556e-03
    ## 1732 12/23/1998  1.111111e-02
    ## 1733 12/29/1998  3.333333e-02
    ## 1734 11/23/1998  7.777778e-02
    ## 1735 11/23/1998 -5.555556e-02
    ## 1736 12/31/1998 -3.388889e-01
    ## 1737 12/01/1998  3.888889e-02
    ## 1738 12/10/1998  0.000000e+00
    ## 1739 12/15/1998  1.111111e-02
    ## 1740 11/24/1998  0.000000e+00
    ## 1741 12/01/1998  2.777778e-02
    ## 1742 12/04/1998  2.777778e-02
    ## 1743 12/15/1998  3.888889e-02
    ## 1744 12/17/1998  0.000000e+00
    ## 1745 12/19/1998  0.000000e+00
    ## 1746 12/19/1998  3.333333e-02
    ## 1747 12/19/1998  0.000000e+00
    ## 1748 12/10/1998  7.222222e-02
    ## 1749 12/15/1998  3.333333e-02
    ## 1750 12/23/1998  5.555556e-03
    ## 1751 12/23/1998  2.222222e-02
    ## 1752 12/28/1998  1.666667e-02
    ## 1753 11/24/1998  5.084746e-02
    ## 1754 11/27/1998  2.033898e-01
    ## 1755 12/01/1998  5.084746e-02
    ## 1756 12/04/1998  5.649718e-03
    ## 1757 12/10/1998 -3.389831e-01
    ## 1758 12/31/1998  2.824859e-02
    ## 1759 12/02/1998  1.694915e-02
    ## 1760 12/09/1998  1.129944e-02
    ## 1761 12/01/1998  6.214689e-02
    ## 1762 12/04/1998 -1.694915e-02
    ## 1763 12/04/1998 -3.389831e-01
    ## 1764 12/14/1998  0.000000e+00
    ## 1765 12/02/1998 -2.824859e-02
    ## 1766 12/17/1998  4.519774e-02
    ## 1767 12/22/1998 -3.333333e-01
    ## 1768 12/04/1998  5.649718e-03
    ## 1769 12/04/1998 -3.389831e-01
    ## 1770 12/14/1998  0.000000e+00
    ## 1771 12/17/1998 -5.649718e-02
    ## 1772 12/21/1998  1.186441e-01
    ## 1773 12/22/1998  8.474576e-02
    ## 1774 11/23/1998  1.724138e-02
    ## 1775 11/24/1998  4.597701e-02
    ## 1776 11/27/1998  4.597701e-02
    ## 1777 11/27/1998  2.298851e-02
    ## 1778 12/01/1998  2.873563e-02
    ## 1779 12/17/1998  0.000000e+00
    ## 1780 12/22/1998  3.448276e-02
    ## 1781 12/01/1998  1.724138e-02
    ## 1782 12/09/1998  5.747126e-03
    ## 1783 11/24/1998  5.747126e-03
    ## 1784 12/02/1998  0.000000e+00
    ## 1785 12/04/1998  3.448276e-02
    ## 1786 12/08/1998 -3.850575e-01
    ## 1787 12/10/1998 -3.390805e-01
    ## 1788 12/04/1998  1.724138e-02
    ## 1789 12/04/1998  1.149425e-02
    ## 1790 12/11/1998  0.000000e+00
    ## 1791 12/15/1998  1.091954e-01
    ## 1792 12/22/1998  6.896552e-02
    ## 1793 12/22/1998 -5.747126e-03
    ## 1794 11/26/1998  1.149425e-02
    ## 1795 12/16/1998  1.149425e-02
    ## 1796 12/17/1998  1.724138e-02
    ## 1797 12/18/1998 -2.701149e-01
    ## 1798 12/28/1998  1.149425e-02
    ## 1799 12/17/1998 -2.298851e-02
    ## 1800 11/24/1998  1.228070e-01
    ## 1801 11/27/1998  5.847953e-03
    ## 1802 11/27/1998  2.339181e-02
    ## 1803 12/01/1998  0.000000e+00
    ## 1804 12/02/1998  1.169591e-02
    ## 1805 12/02/1998  1.754386e-02
    ## 1806 12/10/1998 -3.391813e-01
    ## 1807 12/10/1998  1.169591e-02
    ## 1808 12/11/1998  2.923977e-02
    ## 1809 12/22/1998  0.000000e+00
    ## 1810 12/22/1998  1.754386e-02
    ## 1811 12/22/1998  5.263158e-02
    ## 1812 12/17/1998  0.000000e+00
    ## 1813 12/01/1998  2.339181e-02
    ## 1814 12/02/1998  1.754386e-02
    ## 1815 12/02/1998 -8.771930e-02
    ## 1816 12/04/1998  5.847953e-03
    ## 1817 12/09/1998  0.000000e+00
    ## 1818 12/08/1998  3.508772e-02
    ## 1819 12/10/1998  3.508772e-02
    ## 1820 12/14/1998 -3.391813e-01
    ## 1821 12/01/1998  5.847953e-03
    ## 1822 12/01/1998  6.432749e-02
    ## 1823 12/02/1998  0.000000e+00
    ## 1824 12/04/1998  1.169591e-02
    ## 1825 12/15/1998  2.923977e-02
    ## 1826 12/15/1998  1.754386e-02
    ## 1827 11/30/1998  0.000000e+00
    ## 1828 12/04/1998  2.339181e-02
    ## 1829 12/10/1998  4.678363e-02
    ## 1830 12/18/1998  0.000000e+00
    ## 1831 12/22/1998  1.169591e-02
    ## 1832 12/17/1998  4.678363e-02
    ## 1833 12/17/1998  1.169591e-02
    ## 1834 12/17/1998  5.847953e-03
    ## 1835 12/17/1998  0.000000e+00
    ## 1836 12/28/1998  2.923977e-02
    ## 1837 11/23/1998 -1.785714e-01
    ## 1838 11/27/1998  6.547619e-02
    ## 1839 12/02/1998  1.190476e-02
    ## 1840 12/22/1998  0.000000e+00
    ## 1841 12/22/1998  0.000000e+00
    ## 1842 12/31/1998  0.000000e+00
    ## 1843 12/04/1998  2.976190e-02
    ## 1844 12/09/1998  1.190476e-02
    ## 1845 12/15/1998  2.380952e-02
    ## 1846 12/02/1998  1.190476e-02
    ## 1847 12/03/1998  0.000000e+00
    ## 1848 12/10/1998 -3.392857e-01
    ## 1849 12/14/1998  5.952381e-03
    ## 1850 12/14/1998 -2.976190e-02
    ## 1851 12/04/1998  2.380952e-02
    ## 1852 12/19/1998 -8.928571e-02
    ## 1853 11/26/1998  5.952381e-02
    ## 1854 11/26/1998  1.190476e-02
    ## 1855 12/03/1998  2.380952e-02
    ## 1856 12/10/1998  0.000000e+00
    ## 1857 12/14/1998  8.333333e-02
    ## 1858 12/18/1998  5.952381e-03
    ## 1859 12/17/1998  2.976190e-02
    ## 1860 12/17/1998  0.000000e+00
    ## 1861 12/21/1998  4.166667e-02
    ## 1862 11/26/1998  4.242424e-02
    ## 1863 12/01/1998 -1.878788e-01
    ## 1864 12/08/1998 -6.060606e-03
    ## 1865 12/10/1998  1.212121e-02
    ## 1866 12/11/1998  1.212121e-02
    ## 1867 12/11/1998  6.060606e-02
    ## 1868 12/21/1998  0.000000e+00
    ## 1869 12/01/1998  3.030303e-02
    ## 1870 12/02/1998  1.818182e-02
    ## 1871 12/02/1998  1.818182e-02
    ## 1872 12/02/1998  0.000000e+00
    ## 1873 12/03/1998  4.242424e-02
    ## 1874 12/04/1998  4.848485e-02
    ## 1875 12/08/1998 -3.393939e-01
    ## 1876 12/01/1998 -1.818182e-02
    ## 1877 12/01/1998  6.060606e-03
    ## 1878 12/01/1998  0.000000e+00
    ## 1879 12/14/1998  1.818182e-02
    ## 1880 12/02/1998  0.000000e+00
    ## 1881 11/04/1998  6.060606e-03
    ## 1882 12/10/1998  9.090909e-02
    ## 1883 12/15/1998  1.818182e-02
    ## 1884 12/15/1998  0.000000e+00
    ## 1885 12/22/1998  0.000000e+00
    ## 1886 12/17/1998  1.212121e-02
    ## 1887 12/17/1998  1.818182e-02
    ## 1888 11/23/1998  1.851852e-02
    ## 1889 12/01/1998 -6.172840e-03
    ## 1890 12/02/1998  0.000000e+00
    ## 1891 12/02/1998 -3.333333e-01
    ## 1892 12/11/1998  9.876543e-02
    ## 1893 12/22/1998  4.320988e-02
    ## 1894 12/23/1998 -6.172840e-03
    ## 1895 12/23/1998  6.172840e-03
    ## 1896 12/23/1998  0.000000e+00
    ## 1897 12/23/1998  2.469136e-02
    ## 1898 12/23/1998  6.172840e-03
    ## 1899 12/17/1998  6.172840e-03
    ## 1900 12/29/1998  0.000000e+00
    ## 1901 12/29/1998  0.000000e+00
    ## 1902 12/01/1998  2.469136e-02
    ## 1903 12/01/1998  1.851852e-02
    ## 1904 12/09/1998  6.790123e-02
    ## 1905 12/11/1998  6.172840e-03
    ## 1906 12/01/1998  1.234568e-02
    ## 1907 12/03/1998  8.641975e-02
    ## 1908 12/04/1998 -3.395062e-01
    ## 1909 12/10/1998  3.703704e-02
    ## 1910 12/10/1998  0.000000e+00
    ## 1911 12/15/1998  1.234568e-02
    ## 1912 12/02/1998  4.938272e-02
    ## 1913 12/22/1998  6.172840e-03
    ## 1914 12/22/1998  6.172840e-02
    ## 1915 11/04/1998  5.555556e-02
    ## 1916 12/15/1998  6.172840e-03
    ## 1917 12/21/1998  6.172840e-03
    ## 1918 12/23/1998  0.000000e+00
    ## 1919 12/28/1998  0.000000e+00
    ## 1920 11/27/1998  8.176101e-02
    ## 1921 12/01/1998  1.257862e-02
    ## 1922 12/01/1998  6.289308e-03
    ## 1923 12/04/1998  4.402516e-02
    ## 1924 12/02/1998  3.773585e-02
    ## 1925 12/22/1998  1.257862e-02
    ## 1926 11/26/1998 -1.257862e-02
    ## 1927 11/23/1998  0.000000e+00
    ## 1928 12/14/1998  6.289308e-03
    ## 1929 12/27/1998  3.773585e-02
    ## 1930 12/27/1998  0.000000e+00
    ## 1931 12/02/1998  3.144654e-02
    ## 1932 12/02/1998  1.886792e-02
    ## 1933 12/04/1998  2.515723e-02
    ## 1934 12/11/1998  1.257862e-02
    ## 1935 12/15/1998  1.886792e-02
    ## 1936 11/24/1998 -4.402516e-02
    ## 1937 11/24/1998  6.289308e-03
    ## 1938 11/24/1998  0.000000e+00
    ## 1939 12/02/1998  1.257862e-02
    ## 1940 12/04/1998  1.257862e-02
    ## 1941 12/10/1998  1.886792e-02
    ## 1942 12/11/1998  6.289308e-03
    ## 1943 12/01/1998  0.000000e+00
    ## 1944 12/04/1998  1.257862e-02
    ## 1945 12/08/1998  2.515723e-02
    ## 1946 12/15/1998  1.257862e-02
    ## 1947 12/17/1998 -6.289308e-03
    ## 1948 12/17/1998  6.289308e-03
    ## 1949 12/19/1998  3.144654e-02
    ## 1950 12/22/1998 -8.805031e-02
    ## 1951 12/02/1998  0.000000e+00
    ## 1952 12/04/1998  3.773585e-02
    ## 1953 12/10/1998  4.402516e-02
    ## 1954 12/16/1998  0.000000e+00
    ## 1955 12/18/1998  6.289308e-03
    ## 1956 12/18/1998  6.289308e-03
    ## 1957 12/23/1998  2.515723e-02
    ## 1958 12/23/1998  5.031447e-02
    ## 1959 11/24/1998  1.282051e-02
    ## 1960 12/02/1998  6.410256e-03
    ## 1961 12/03/1998  3.846154e-02
    ## 1962 12/04/1998 -1.923077e-02
    ## 1963 12/08/1998  1.282051e-02
    ## 1964 12/11/1998  0.000000e+00
    ## 1965 12/22/1998  0.000000e+00
    ## 1966 12/27/1998  5.769231e-02
    ## 1967 12/27/1998 -2.564103e-02
    ## 1968 12/02/1998  1.282051e-02
    ## 1969 12/15/1998  1.282051e-02
    ## 1970 11/24/1998  3.205128e-02
    ## 1971 11/24/1998  1.923077e-02
    ## 1972 11/25/1998  5.128205e-02
    ## 1973 11/30/1998 -3.205128e-02
    ## 1974 11/30/1998  6.410256e-03
    ## 1975 12/14/1998  1.923077e-02
    ## 1976 12/01/1998  0.000000e+00
    ## 1977 12/04/1998  0.000000e+00
    ## 1978 12/11/1998  5.769231e-02
    ## 1979 12/15/1998  0.000000e+00
    ## 1980 12/19/1998  6.410256e-03
    ## 1981 11/30/1998  1.923077e-02
    ## 1982 12/10/1998  6.410256e-03
    ## 1983 12/10/1998  0.000000e+00
    ## 1984 12/14/1998  4.487179e-02
    ## 1985 12/15/1998  2.564103e-02
    ## 1986 12/17/1998  0.000000e+00
    ## 1987 12/18/1998  1.923077e-02
    ## 1988 12/22/1998  2.564103e-02
    ## 1989 12/23/1998  1.923077e-02
    ## 1990 12/28/1998  6.410256e-03
    ## 1991 12/22/1998  1.282051e-02
    ## 1992 12/28/1998  1.923077e-02
    ## 1993 11/24/1998  3.921569e-02
    ## 1994 11/27/1998  1.307190e-02
    ## 1995 11/27/1998  8.496732e-02
    ## 1996 11/27/1998  2.614379e-02
    ## 1997 12/01/1998  6.535948e-03
    ## 1998 12/04/1998  1.960784e-02
    ## 1999 12/04/1998  0.000000e+00
    ## 2000 12/10/1998  7.189542e-02
    ## 2001 12/11/1998  9.803922e-02
    ## 2002 12/22/1998  1.960784e-02
    ## 2003 12/23/1998  2.614379e-02
    ## 2004 12/21/1998  6.535948e-03
    ## 2005 12/22/1998  6.535948e-02
    ## 2006 11/23/1998  6.535948e-03
    ## 2007 12/14/1998  3.921569e-02
    ## 2008 12/02/1998  1.307190e-02
    ## 2009 12/10/1998 -3.398693e-01
    ## 2010 12/11/1998  3.921569e-02
    ## 2011 11/25/1998 -3.398693e-01
    ## 2012 12/02/1998  6.535948e-03
    ## 2013 12/04/1998  1.307190e-01
    ## 2014 12/01/1998 -6.535948e-03
    ## 2015 12/01/1998  0.000000e+00
    ## 2016 12/01/1998  1.307190e-02
    ## 2017 12/02/1998  3.267974e-02
    ## 2018 12/10/1998  6.535948e-03
    ## 2019 12/11/1998  1.307190e-02
    ## 2020 12/14/1998  0.000000e+00
    ## 2021 12/15/1998  1.960784e-02
    ## 2022 12/15/1998  1.307190e-02
    ## 2023 12/16/1998  1.960784e-02
    ## 2024 12/18/1998  6.535948e-03
    ## 2025 12/17/1998  7.189542e-02
    ## 2026 12/23/1998  6.535948e-03
    ## 2027 12/28/1998  6.535948e-03
    ## 2028 11/24/1998  6.666667e-03
    ## 2029 11/27/1998  1.200000e-01
    ## 2030 11/27/1998  2.333333e-01
    ## 2031 12/01/1998 -3.333333e-01
    ## 2032 12/03/1998  1.333333e-02
    ## 2033 12/04/1998  9.333333e-02
    ## 2034 12/10/1998  0.000000e+00
    ## 2035 12/22/1998  6.000000e-02
    ## 2036 12/23/1998  1.333333e-02
    ## 2037 12/23/1998  1.333333e-02
    ## 2038 12/14/1998 -3.333333e-02
    ## 2039 12/02/1998  0.000000e+00
    ## 2040 12/04/1998  6.666667e-03
    ## 2041 12/09/1998  6.666667e-03
    ## 2042 12/09/1998  4.666667e-02
    ## 2043 12/08/1998  2.000000e-02
    ## 2044 11/24/1998  1.133333e-01
    ## 2045 12/08/1998 -1.333333e-02
    ## 2046 12/14/1998  6.666667e-03
    ## 2047 12/04/1998  3.333333e-02
    ## 2048 12/10/1998  0.000000e+00
    ## 2049 12/14/1998  6.666667e-02
    ## 2050 12/15/1998  1.333333e-02
    ## 2051 12/19/1998  6.666667e-02
    ## 2052 11/26/1998  3.333333e-02
    ## 2053 12/10/1998  1.333333e-02
    ## 2054 12/15/1998  6.000000e-02
    ## 2055 12/16/1998  3.333333e-02
    ## 2056 12/16/1998  1.333333e-02
    ## 2057 12/17/1998  2.666667e-02
    ## 2058 12/18/1998  1.333333e-02
    ## 2059 12/21/1998  8.666667e-02
    ## 2060 11/23/1998  4.081633e-02
    ## 2061 11/23/1998 -1.768707e-01
    ## 2062 11/27/1998  1.360544e-02
    ## 2063 12/02/1998  3.401361e-02
    ## 2064 12/02/1998  6.802721e-03
    ## 2065 12/10/1998 -1.360544e-02
    ## 2066 12/21/1998  0.000000e+00
    ## 2067 12/22/1998  8.843537e-02
    ## 2068 12/22/1998  4.081633e-02
    ## 2069 12/23/1998  1.360544e-02
    ## 2070 12/31/1998  6.802721e-03
    ## 2071 12/01/1998  6.802721e-03
    ## 2072 12/09/1998  6.802721e-03
    ## 2073 12/04/1998  2.040816e-02
    ## 2074 11/24/1998  6.802721e-03
    ## 2075 11/24/1998  1.292517e-01
    ## 2076 12/01/1998  4.761905e-02
    ## 2077 12/04/1998  4.081633e-02
    ## 2078 12/10/1998  2.721088e-02
    ## 2079 12/10/1998 -3.401361e-01
    ## 2080 12/02/1998  2.040816e-02
    ## 2081 12/04/1998  3.401361e-02
    ## 2082 12/14/1998 -2.040816e-02
    ## 2083 12/15/1998  2.721088e-02
    ## 2084 12/14/1998  0.000000e+00
    ## 2085 12/15/1998 -1.428571e-01
    ## 2086 12/15/1998  0.000000e+00
    ## 2087 12/15/1998  3.401361e-02
    ## 2088 12/17/1998  0.000000e+00
    ## 2089 12/17/1998  2.040816e-02
    ## 2090 12/17/1998  2.040816e-02
    ## 2091 12/01/1998  4.861111e-02
    ## 2092 12/01/1998 -3.402778e-01
    ## 2093 12/04/1998  2.777778e-02
    ## 2094 12/10/1998  6.944444e-03
    ## 2095 12/10/1998 -3.402778e-01
    ## 2096 12/11/1998 -3.402778e-01
    ## 2097 12/22/1998  0.000000e+00
    ## 2098 12/22/1998  0.000000e+00
    ## 2099 12/23/1998  4.861111e-02
    ## 2100 12/29/1998  6.944444e-03
    ## 2101 12/11/1998  1.388889e-02
    ## 2102 12/15/1998  6.944444e-03
    ## 2103 12/15/1998  4.166667e-02
    ## 2104 11/24/1998  3.472222e-02
    ## 2105 11/30/1998  4.861111e-02
    ## 2106 12/01/1998  3.472222e-02
    ## 2107 12/08/1998  6.944444e-03
    ## 2108 12/03/1998  4.166667e-02
    ## 2109 12/03/1998  0.000000e+00
    ## 2110 12/04/1998  1.458333e-01
    ## 2111 12/04/1998  2.777778e-02
    ## 2112 12/04/1998  0.000000e+00
    ## 2113 12/04/1998  0.000000e+00
    ## 2114 12/08/1998  2.777778e-02
    ## 2115 12/08/1998  8.333333e-02
    ## 2116 12/08/1998  2.777778e-02
    ## 2117 12/11/1998  1.388889e-02
    ## 2118 12/14/1998  6.944444e-03
    ## 2119 12/14/1998  1.388889e-02
    ## 2120 12/14/1998  0.000000e+00
    ## 2121 12/15/1998  0.000000e+00
    ## 2122 12/15/1998  2.083333e-02
    ## 2123 12/04/1998  2.777778e-02
    ## 2124 12/04/1998  4.166667e-02
    ## 2125 12/04/1998  1.388889e-02
    ## 2126 12/10/1998  2.777778e-02
    ## 2127 12/11/1998  4.166667e-02
    ## 2128 12/14/1998  0.000000e+00
    ## 2129 12/15/1998 -3.402778e-01
    ## 2130 12/16/1998  0.000000e+00
    ## 2131 12/17/1998 -3.402778e-01
    ## 2132 12/19/1998 -4.166667e-02
    ## 2133 11/30/1998  2.777778e-02
    ## 2134 12/03/1998  2.777778e-02
    ## 2135 12/03/1998  0.000000e+00
    ## 2136 11/04/1998  1.388889e-02
    ## 2137 11/04/1998  6.944444e-02
    ## 2138 12/10/1998  6.944444e-02
    ## 2139 12/10/1998  8.333333e-02
    ## 2140 12/14/1998  1.388889e-02
    ## 2141 12/14/1998  2.777778e-02
    ## 2142 12/15/1998  0.000000e+00
    ## 2143 12/18/1998  6.944444e-03
    ## 2144 12/22/1998  0.000000e+00
    ## 2145 12/22/1998  6.944444e-03
    ## 2146 12/28/1998  6.944444e-03
    ## 2147 12/23/1998  0.000000e+00
    ## 2148 12/28/1998 -3.402778e-01
    ## 2149 11/26/1998  4.255319e-02
    ## 2150 12/02/1998  1.418440e-02
    ## 2151 12/02/1998  2.836879e-02
    ## 2152 12/02/1998  7.092199e-03
    ## 2153 12/11/1998  9.929078e-02
    ## 2154 12/23/1998  6.382979e-02
    ## 2155 12/17/1998  4.964539e-02
    ## 2156 12/29/1998  3.546099e-02
    ## 2157 12/14/1998  7.092199e-02
    ## 2158 12/27/1998  7.092199e-03
    ## 2159 12/27/1998  1.418440e-02
    ## 2160 12/31/1998  1.418440e-02
    ## 2161 12/01/1998  7.092199e-03
    ## 2162 12/04/1998  7.092199e-03
    ## 2163 12/15/1998  2.836879e-02
    ## 2164 11/24/1998  2.836879e-02
    ## 2165 11/24/1998  7.092199e-03
    ## 2166 11/25/1998  2.127660e-02
    ## 2167 11/30/1998 -1.985816e-01
    ## 2168 12/04/1998  1.418440e-02
    ## 2169 12/15/1998 -1.276596e-01
    ## 2170 12/04/1998  0.000000e+00
    ## 2171 12/14/1998  0.000000e+00
    ## 2172 12/17/1998  0.000000e+00
    ## 2173 12/19/1998  0.000000e+00
    ## 2174 12/22/1998  7.801418e-02
    ## 2175 12/04/1998  3.546099e-02
    ## 2176 12/04/1998  3.546099e-02
    ## 2177 12/14/1998  1.418440e-02
    ## 2178 12/15/1998  1.063830e-01
    ## 2179 12/16/1998  2.127660e-02
    ## 2180 12/18/1998  2.836879e-02
    ## 2181 12/22/1998 -3.404255e-01
    ## 2182 12/22/1998  3.546099e-02
    ## 2183 12/23/1998 -1.418440e-02
    ## 2184 12/23/1998  2.127660e-02
    ## 2185 12/23/1998 -3.333333e-01
    ## 2186 12/28/1998  0.000000e+00
    ## 2187 12/03/1998  1.449275e-02
    ## 2188 12/10/1998  0.000000e+00
    ## 2189 12/17/1998 -3.405797e-01
    ## 2190 12/22/1998  6.521739e-02
    ## 2191 12/23/1998 -3.405797e-01
    ## 2192 12/23/1998  0.000000e+00
    ## 2193 12/17/1998  2.898551e-02
    ## 2194 12/21/1998  2.898551e-02
    ## 2195 12/27/1998  1.014493e-01
    ## 2196 12/31/1998  7.246377e-03
    ## 2197 12/31/1998  0.000000e+00
    ## 2198 12/09/1998 -2.463768e-01
    ## 2199 12/11/1998  1.449275e-02
    ## 2200 11/24/1998  1.449275e-02
    ## 2201 11/25/1998  5.797101e-02
    ## 2202 12/02/1998  9.420290e-02
    ## 2203 12/08/1998  0.000000e+00
    ## 2204 12/14/1998  2.173913e-02
    ## 2205 12/17/1998  7.246377e-03
    ## 2206 12/02/1998  1.449275e-02
    ## 2207 12/04/1998 -1.449275e-02
    ## 2208 12/10/1998  7.246377e-02
    ## 2209 12/14/1998  2.898551e-02
    ## 2210 12/15/1998  2.173913e-02
    ## 2211 12/16/1998  1.449275e-02
    ## 2212 12/18/1998  0.000000e+00
    ## 2213 12/22/1998  0.000000e+00
    ## 2214 12/22/1998  1.449275e-02
    ## 2215 12/23/1998  0.000000e+00
    ## 2216 12/17/1998  0.000000e+00
    ## 2217 12/21/1998 -3.405797e-01
    ## 2218 11/24/1998  2.222222e-02
    ## 2219 11/27/1998  7.407407e-03
    ## 2220 12/02/1998  1.481481e-02
    ## 2221 12/02/1998  5.185185e-02
    ## 2222 12/02/1998  2.222222e-02
    ## 2223 12/11/1998  8.148148e-02
    ## 2224 12/11/1998  2.962963e-02
    ## 2225 12/22/1998  2.222222e-02
    ## 2226 11/23/1998  7.407407e-03
    ## 2227 12/14/1998  7.407407e-03
    ## 2228 12/14/1998  2.222222e-02
    ## 2229 12/27/1998  5.185185e-02
    ## 2230 12/27/1998  2.962963e-02
    ## 2231 12/27/1998 -2.222222e-02
    ## 2232 12/01/1998 -2.962963e-02
    ## 2233 12/09/1998 -3.407407e-01
    ## 2234 12/09/1998  2.962963e-02
    ## 2235 11/24/1998  2.222222e-02
    ## 2236 12/03/1998  0.000000e+00
    ## 2237 12/04/1998  1.481481e-02
    ## 2238 12/04/1998  0.000000e+00
    ## 2239 12/14/1998  7.407407e-03
    ## 2240 12/14/1998  7.407407e-03
    ## 2241 12/15/1998 -7.407407e-03
    ## 2242 12/02/1998  0.000000e+00
    ## 2243 12/04/1998 -2.962963e-02
    ## 2244 12/04/1998  0.000000e+00
    ## 2245 12/09/1998  2.222222e-02
    ## 2246 12/16/1998  1.481481e-02
    ## 2247 11/04/1998  2.962963e-02
    ## 2248 12/04/1998  7.407407e-03
    ## 2249 12/10/1998  0.000000e+00
    ## 2250 12/23/1998  0.000000e+00
    ## 2251 12/23/1998 -3.407407e-01
    ## 2252 12/23/1998  3.703704e-02
    ## 2253 12/23/1998  0.000000e+00
    ## 2254 12/22/1998  2.222222e-02
    ## 2255 12/22/1998  2.962963e-02
    ## 2256 12/23/1998 -2.296296e-01
    ## 2257 12/28/1998  2.222222e-02
    ## 2258 12/28/1998 -7.407407e-03
    ## 2259 11/24/1998  7.575758e-02
    ## 2260 12/01/1998  7.575758e-03
    ## 2261 12/01/1998  4.545455e-02
    ## 2262 12/04/1998  6.818182e-02
    ## 2263 12/10/1998  0.000000e+00
    ## 2264 11/27/1998 -1.515152e-02
    ## 2265 12/17/1998  1.515152e-02
    ## 2266 12/22/1998  4.545455e-02
    ## 2267 12/23/1998  1.515152e-02
    ## 2268 12/23/1998  1.515152e-02
    ## 2269 12/17/1998  7.575758e-03
    ## 2270 12/21/1998  7.575758e-03
    ## 2271 12/22/1998  1.515152e-02
    ## 2272 12/29/1998  5.303030e-02
    ## 2273 12/25/1998  9.090909e-02
    ## 2274 12/25/1998  1.515152e-02
    ## 2275 12/27/1998  8.333333e-02
    ## 2276 12/31/1998  0.000000e+00
    ## 2277 12/31/1998  0.000000e+00
    ## 2278 12/02/1998 -3.409091e-01
    ## 2279 12/09/1998  7.575758e-03
    ## 2280 12/04/1998  4.545455e-02
    ## 2281 12/04/1998  1.515152e-02
    ## 2282 12/15/1998  2.272727e-02
    ## 2283 11/24/1998  0.000000e+00
    ## 2284 12/02/1998  1.136364e-01
    ## 2285 12/04/1998 -2.272727e-01
    ## 2286 12/08/1998 -1.742424e-01
    ## 2287 12/08/1998  1.515152e-02
    ## 2288 12/11/1998 -3.409091e-01
    ## 2289 12/02/1998  0.000000e+00
    ## 2290 12/04/1998  0.000000e+00
    ## 2291 12/10/1998  7.575758e-03
    ## 2292 12/10/1998  7.575758e-03
    ## 2293 12/11/1998  6.060606e-02
    ## 2294 12/17/1998  2.272727e-02
    ## 2295 12/22/1998 -2.121212e-01
    ## 2296 11/26/1998  1.515152e-02
    ## 2297 11/26/1998  0.000000e+00
    ## 2298 12/02/1998  7.575758e-03
    ## 2299 12/04/1998  2.272727e-02
    ## 2300 12/04/1998  3.787879e-02
    ## 2301 12/04/1998  3.030303e-02
    ## 2302 12/04/1998  0.000000e+00
    ## 2303 12/16/1998  7.575758e-03
    ## 2304 12/16/1998 -2.272727e-02
    ## 2305 12/17/1998  2.272727e-02
    ## 2306 12/22/1998  0.000000e+00
    ## 2307 12/17/1998  0.000000e+00
    ## 2308 12/21/1998  4.545455e-02
    ## 2309 12/28/1998  0.000000e+00
    ## 2310 12/28/1998  7.575758e-03
    ## 2311 12/28/1998  0.000000e+00
    ## 2312 11/27/1998  1.085271e-01
    ## 2313 11/27/1998  3.100775e-02
    ## 2314 12/01/1998 -7.751938e-03
    ## 2315 12/04/1998 -2.325581e-02
    ## 2316 12/02/1998  2.325581e-02
    ## 2317 12/02/1998  7.751938e-03
    ## 2318 12/02/1998  0.000000e+00
    ## 2319 12/03/1998  0.000000e+00
    ## 2320 12/17/1998  7.751938e-03
    ## 2321 12/22/1998  8.527132e-02
    ## 2322 12/21/1998  2.325581e-02
    ## 2323 12/21/1998  7.751938e-03
    ## 2324 12/23/1998  6.201550e-02
    ## 2325 12/23/1998  3.100775e-02
    ## 2326 12/14/1998  4.651163e-02
    ## 2327 12/01/1998  3.875969e-02
    ## 2328 12/02/1998  2.325581e-02
    ## 2329 12/02/1998  1.550388e-02
    ## 2330 12/10/1998  0.000000e+00
    ## 2331 12/10/1998  7.751938e-03
    ## 2332 12/10/1998  7.751938e-02
    ## 2333 12/15/1998  0.000000e+00
    ## 2334 12/15/1998  6.976744e-02
    ## 2335 11/24/1998  7.751938e-03
    ## 2336 11/24/1998  3.100775e-02
    ## 2337 11/24/1998  3.875969e-02
    ## 2338 11/30/1998  2.325581e-02
    ## 2339 12/14/1998 -2.325581e-02
    ## 2340 12/15/1998  3.100775e-02
    ## 2341 12/01/1998 -2.325581e-02
    ## 2342 12/02/1998  6.201550e-02
    ## 2343 12/02/1998  7.751938e-03
    ## 2344 12/09/1998  7.751938e-03
    ## 2345 12/09/1998  2.325581e-02
    ## 2346 12/10/1998  0.000000e+00
    ## 2347 12/11/1998  0.000000e+00
    ## 2348 12/19/1998  0.000000e+00
    ## 2349 11/26/1998  7.751938e-03
    ## 2350 11/26/1998  1.550388e-02
    ## 2351 11/30/1998  3.100775e-02
    ## 2352 12/02/1998  0.000000e+00
    ## 2353 12/03/1998  7.751938e-03
    ## 2354 12/15/1998  4.651163e-02
    ## 2355 12/17/1998  7.751938e-03
    ## 2356 12/18/1998  2.325581e-02
    ## 2357 12/17/1998  0.000000e+00
    ## 2358 11/26/1998  3.174603e-02
    ## 2359 11/27/1998  3.174603e-02
    ## 2360 11/27/1998  3.968254e-02
    ## 2361 12/01/1998 -7.936508e-03
    ## 2362 12/04/1998  7.936508e-03
    ## 2363 12/10/1998  0.000000e+00
    ## 2364 12/08/1998 -1.587302e-02
    ## 2365 12/11/1998  7.936508e-02
    ## 2366 12/11/1998  8.730159e-02
    ## 2367 12/22/1998  0.000000e+00
    ## 2368 12/22/1998  1.587302e-02
    ## 2369 12/23/1998  3.174603e-02
    ## 2370 12/23/1998  0.000000e+00
    ## 2371 12/23/1998  2.380952e-02
    ## 2372 12/23/1998  6.349206e-02
    ## 2373 12/25/1998  1.190476e-01
    ## 2374 12/27/1998  3.174603e-02
    ## 2375 12/27/1998  3.968254e-02
    ## 2376 12/31/1998  4.761905e-02
    ## 2377 12/31/1998  1.587302e-02
    ## 2378 12/01/1998  5.555556e-02
    ## 2379 12/09/1998  2.380952e-02
    ## 2380 12/04/1998  4.761905e-02
    ## 2381 12/09/1998  1.587302e-02
    ## 2382 12/15/1998  6.349206e-02
    ## 2383 12/15/1998  2.380952e-02
    ## 2384 11/24/1998  2.380952e-02
    ## 2385 11/25/1998  1.587302e-02
    ## 2386 12/02/1998  0.000000e+00
    ## 2387 12/03/1998  7.936508e-03
    ## 2388 12/08/1998  4.761905e-02
    ## 2389 12/10/1998  1.587302e-02
    ## 2390 12/11/1998  1.587302e-02
    ## 2391 12/15/1998  3.968254e-02
    ## 2392 12/01/1998  0.000000e+00
    ## 2393 12/02/1998  1.587302e-02
    ## 2394 12/04/1998 -3.412698e-01
    ## 2395 12/04/1998  6.349206e-02
    ## 2396 12/15/1998  1.587302e-02
    ## 2397 12/15/1998 -3.412698e-01
    ## 2398 12/15/1998  2.380952e-02
    ## 2399 12/17/1998 -7.936508e-03
    ## 2400 12/19/1998  0.000000e+00
    ## 2401 12/08/1998  0.000000e+00
    ## 2402 11/26/1998  7.936508e-03
    ## 2403 12/02/1998 -3.333333e-01
    ## 2404 11/04/1998  2.380952e-02
    ## 2405 12/04/1998  1.587302e-02
    ## 2406 12/04/1998 -1.587302e-02
    ## 2407 12/04/1998  7.936508e-03
    ## 2408 12/04/1998  7.936508e-03
    ## 2409 12/04/1998 -6.349206e-02
    ## 2410 12/10/1998  7.936508e-03
    ## 2411 12/14/1998  3.968254e-02
    ## 2412 12/15/1998  3.174603e-02
    ## 2413 12/17/1998  6.349206e-02
    ## 2414 12/17/1998  0.000000e+00
    ## 2415 12/18/1998  4.761905e-02
    ## 2416 12/18/1998  4.761905e-02
    ## 2417 12/18/1998  0.000000e+00
    ## 2418 12/23/1998  0.000000e+00
    ## 2419 12/28/1998  0.000000e+00
    ## 2420 12/17/1998 -5.555556e-02
    ## 2421 12/17/1998  7.936508e-03
    ## 2422 12/22/1998  7.936508e-03
    ## 2423 12/28/1998 -7.936508e-03
    ## 2424 11/23/1998  6.504065e-02
    ## 2425 11/23/1998  2.439024e-02
    ## 2426 11/26/1998 -3.252033e-02
    ## 2427 11/27/1998  2.439024e-02
    ## 2428 12/02/1998 -8.130081e-03
    ## 2429 12/08/1998 -3.414634e-01
    ## 2430 12/10/1998  2.439024e-02
    ## 2431 12/10/1998  3.252033e-02
    ## 2432 12/11/1998  3.252033e-02
    ## 2433 12/17/1998  0.000000e+00
    ## 2434 12/22/1998  3.252033e-02
    ## 2435 12/22/1998 -1.300813e-01
    ## 2436 12/23/1998  0.000000e+00
    ## 2437 12/23/1998  1.626016e-02
    ## 2438 12/23/1998  8.130081e-03
    ## 2439 11/23/1998  0.000000e+00
    ## 2440 12/14/1998  4.878049e-02
    ## 2441 12/14/1998  5.691057e-02
    ## 2442 12/25/1998  1.382114e-01
    ## 2443 12/31/1998 -3.414634e-01
    ## 2444 12/01/1998  4.878049e-02
    ## 2445 12/02/1998  1.626016e-02
    ## 2446 12/04/1998  8.130081e-03
    ## 2447 12/09/1998  4.065041e-02
    ## 2448 12/11/1998  3.252033e-02
    ## 2449 12/15/1998  1.626016e-02
    ## 2450 12/15/1998  3.252033e-02
    ## 2451 12/15/1998 -6.504065e-02
    ## 2452 11/24/1998  0.000000e+00
    ## 2453 11/30/1998  3.252033e-02
    ## 2454 11/30/1998  1.626016e-02
    ## 2455 12/01/1998  3.252033e-02
    ## 2456 12/03/1998  5.691057e-02
    ## 2457 12/08/1998  3.252033e-02
    ## 2458 12/15/1998 -8.130081e-03
    ## 2459 12/15/1998 -2.439024e-01
    ## 2460 12/01/1998  4.065041e-02
    ## 2461 12/02/1998  1.382114e-01
    ## 2462 12/08/1998  8.130081e-03
    ## 2463 12/15/1998  2.439024e-02
    ## 2464 12/16/1998  0.000000e+00
    ## 2465 12/19/1998  0.000000e+00
    ## 2466 12/22/1998  8.130081e-03
    ## 2467 11/30/1998  4.065041e-02
    ## 2468 12/03/1998  3.252033e-02
    ## 2469 12/04/1998  0.000000e+00
    ## 2470 12/04/1998  8.130081e-03
    ## 2471 12/10/1998  8.130081e-03
    ## 2472 12/14/1998  0.000000e+00
    ## 2473 12/14/1998  4.878049e-02
    ## 2474 12/15/1998  0.000000e+00
    ## 2475 12/21/1998  1.626016e-02
    ## 2476 12/28/1998  8.130081e-02
    ## 2477 12/28/1998  1.626016e-02
    ## 2478 12/28/1998  0.000000e+00
    ## 2479 12/22/1998  3.252033e-02
    ## 2480 11/26/1998  4.166667e-02
    ## 2481 11/26/1998 -3.416667e-01
    ## 2482 11/27/1998  1.416667e-01
    ## 2483 11/27/1998  2.500000e-02
    ## 2484 11/27/1998  1.666667e-02
    ## 2485 12/01/1998 -8.333333e-03
    ## 2486 12/02/1998  1.666667e-02
    ## 2487 12/02/1998  8.333333e-03
    ## 2488 12/02/1998 -3.416667e-01
    ## 2489 12/03/1998 -8.333333e-03
    ## 2490 12/04/1998  2.500000e-02
    ## 2491 12/04/1998  0.000000e+00
    ## 2492 12/10/1998 -4.166667e-02
    ## 2493 12/10/1998  4.166667e-02
    ## 2494 12/22/1998  0.000000e+00
    ## 2495 12/23/1998  6.666667e-02
    ## 2496 12/17/1998  4.166667e-02
    ## 2497 12/17/1998  8.333333e-03
    ## 2498 12/22/1998  0.000000e+00
    ## 2499 12/22/1998  1.083333e-01
    ## 2500 12/23/1998  4.166667e-02
    ## 2501 12/29/1998  3.333333e-02
    ## 2502 11/23/1998  6.666667e-02
    ## 2503 12/25/1998  5.000000e-02
    ## 2504 12/25/1998  2.500000e-02
    ## 2505 12/01/1998  5.000000e-02
    ## 2506 12/09/1998  2.500000e-02
    ## 2507 12/09/1998  2.500000e-02
    ## 2508 12/04/1998  2.500000e-02
    ## 2509 12/04/1998  8.333333e-03
    ## 2510 12/09/1998  4.166667e-02
    ## 2511 12/09/1998  0.000000e+00
    ## 2512 12/11/1998  8.333333e-03
    ## 2513 12/15/1998  4.166667e-02
    ## 2514 12/15/1998  4.166667e-02
    ## 2515 11/24/1998  7.500000e-02
    ## 2516 11/24/1998  0.000000e+00
    ## 2517 11/30/1998  0.000000e+00
    ## 2518 11/30/1998  8.333333e-03
    ## 2519 12/02/1998  0.000000e+00
    ## 2520 12/10/1998  3.333333e-02
    ## 2521 12/10/1998  0.000000e+00
    ## 2522 12/10/1998  4.166667e-02
    ## 2523 12/10/1998  2.500000e-02
    ## 2524 12/11/1998  4.166667e-02
    ## 2525 12/14/1998  5.000000e-02
    ## 2526 12/01/1998  6.666667e-02
    ## 2527 12/01/1998 -2.333333e-01
    ## 2528 12/02/1998  0.000000e+00
    ## 2529 12/04/1998  2.083333e-01
    ## 2530 12/09/1998  0.000000e+00
    ## 2531 12/09/1998  8.333333e-03
    ## 2532 12/10/1998 -3.333333e-01
    ## 2533 12/14/1998  3.333333e-02
    ## 2534 12/14/1998 -6.666667e-02
    ## 2535 12/15/1998  5.833333e-02
    ## 2536 12/22/1998  3.333333e-02
    ## 2537 11/30/1998 -3.416667e-01
    ## 2538 12/02/1998  8.333333e-03
    ## 2539 12/03/1998  5.000000e-02
    ## 2540 12/10/1998  1.666667e-02
    ## 2541 12/14/1998  5.000000e-02
    ## 2542 12/14/1998  0.000000e+00
    ## 2543 12/14/1998  4.166667e-02
    ## 2544 12/15/1998  5.000000e-02
    ## 2545 12/16/1998  5.833333e-02
    ## 2546 12/17/1998  0.000000e+00
    ## 2547 12/17/1998  3.333333e-02
    ## 2548 12/17/1998  0.000000e+00
    ## 2549 12/18/1998  5.000000e-02
    ## 2550 12/22/1998  8.333333e-02
    ## 2551 12/23/1998 -3.416667e-01
    ## 2552 12/23/1998  1.666667e-02
    ## 2553 12/23/1998  0.000000e+00
    ## 2554 12/23/1998  0.000000e+00
    ## 2555 12/28/1998  0.000000e+00
    ## 2556 12/22/1998  2.500000e-02
    ## 2557 11/23/1998  4.273504e-02
    ## 2558 11/24/1998 -5.128205e-02
    ## 2559 11/26/1998 -9.401709e-02
    ## 2560 11/27/1998  1.025641e-01
    ## 2561 12/04/1998  1.709402e-02
    ## 2562 12/03/1998  3.418803e-02
    ## 2563 12/04/1998  2.564103e-02
    ## 2564 12/10/1998 -3.418803e-01
    ## 2565 12/08/1998 -8.547009e-03
    ## 2566 12/10/1998  1.709402e-02
    ## 2567 12/17/1998  8.547009e-03
    ## 2568 12/22/1998  0.000000e+00
    ## 2569 12/21/1998  2.564103e-02
    ## 2570 12/22/1998 -1.709402e-02
    ## 2571 12/23/1998  3.418803e-02
    ## 2572 12/23/1998  3.418803e-02
    ## 2573 11/23/1998  4.273504e-02
    ## 2574 12/14/1998  3.418803e-02
    ## 2575 12/14/1998  5.128205e-02
    ## 2576 12/27/1998  3.418803e-02
    ## 2577 12/01/1998  2.564103e-02
    ## 2578 12/01/1998  3.418803e-02
    ## 2579 12/02/1998  1.709402e-02
    ## 2580 12/02/1998  2.564103e-02
    ## 2581 12/09/1998 -3.418803e-01
    ## 2582 12/09/1998  1.709402e-02
    ## 2583 12/11/1998  0.000000e+00
    ## 2584 12/11/1998  3.418803e-02
    ## 2585 12/15/1998  1.709402e-02
    ## 2586 12/15/1998  1.025641e-01
    ## 2587 12/15/1998  5.128205e-02
    ## 2588 12/15/1998  0.000000e+00
    ## 2589 11/24/1998  3.418803e-02
    ## 2590 11/24/1998  3.418803e-02
    ## 2591 11/24/1998 -3.418803e-01
    ## 2592 11/25/1998  5.982906e-02
    ## 2593 12/08/1998  3.418803e-02
    ## 2594 12/15/1998  8.547009e-03
    ## 2595 12/01/1998 -5.128205e-02
    ## 2596 12/02/1998 -3.333333e-01
    ## 2597 12/04/1998  8.547009e-02
    ## 2598 12/10/1998  3.418803e-02
    ## 2599 12/14/1998  3.418803e-02
    ## 2600 12/15/1998 -8.547009e-03
    ## 2601 12/15/1998  0.000000e+00
    ## 2602 12/16/1998  0.000000e+00
    ## 2603 12/16/1998  1.709402e-02
    ## 2604 12/16/1998  5.128205e-02
    ## 2605 12/17/1998  8.547009e-03
    ## 2606 12/19/1998  0.000000e+00
    ## 2607 12/19/1998 -3.418803e-01
    ## 2608 11/26/1998 -5.128205e-02
    ## 2609 12/02/1998  0.000000e+00
    ## 2610 12/04/1998  0.000000e+00
    ## 2611 12/04/1998 -1.709402e-02
    ## 2612 12/10/1998  0.000000e+00
    ## 2613 12/10/1998  8.547009e-03
    ## 2614 12/15/1998  1.709402e-02
    ## 2615 12/15/1998  2.564103e-02
    ## 2616 12/15/1998  5.128205e-02
    ## 2617 12/15/1998  0.000000e+00
    ## 2618 12/17/1998  8.547009e-03
    ## 2619 12/18/1998  3.418803e-02
    ## 2620 12/23/1998  0.000000e+00
    ## 2621 12/28/1998  0.000000e+00
    ## 2622 12/08/1998 -3.418803e-02
    ## 2623 11/23/1998  6.140351e-02
    ## 2624 11/23/1998  1.929825e-01
    ## 2625 11/24/1998  3.508772e-02
    ## 2626 11/27/1998  4.385965e-02
    ## 2627 11/27/1998  8.771930e-03
    ## 2628 11/27/1998  0.000000e+00
    ## 2629 11/27/1998  4.385965e-02
    ## 2630 12/04/1998  2.631579e-02
    ## 2631 12/04/1998  2.631579e-02
    ## 2632 12/10/1998  6.140351e-02
    ## 2633 12/02/1998  2.631579e-02
    ## 2634 12/11/1998 -4.385965e-02
    ## 2635 12/11/1998  8.771930e-02
    ## 2636 12/11/1998  5.263158e-02
    ## 2637 12/11/1998  3.508772e-02
    ## 2638 12/23/1998  8.771930e-03
    ## 2639 12/17/1998  4.385965e-02
    ## 2640 12/22/1998  8.771930e-03
    ## 2641 12/22/1998  0.000000e+00
    ## 2642 12/23/1998  5.263158e-02
    ## 2643 12/23/1998  4.385965e-02
    ## 2644 12/29/1998  0.000000e+00
    ## 2645 11/23/1998  2.631579e-02
    ## 2646 12/14/1998  9.649123e-02
    ## 2647 12/14/1998  4.385965e-02
    ## 2648 12/27/1998  8.771930e-03
    ## 2649 12/01/1998  1.754386e-02
    ## 2650 12/02/1998  2.631579e-02
    ## 2651 12/09/1998 -3.421053e-01
    ## 2652 12/04/1998 -3.421053e-01
    ## 2653 12/04/1998  3.508772e-02
    ## 2654 12/04/1998  1.754386e-02
    ## 2655 12/09/1998  0.000000e+00
    ## 2656 12/09/1998  4.385965e-02
    ## 2657 12/10/1998  0.000000e+00
    ## 2658 12/11/1998 -4.122807e-01
    ## 2659 12/15/1998  2.631579e-02
    ## 2660 11/24/1998  1.754386e-02
    ## 2661 11/25/1998  9.649123e-02
    ## 2662 12/01/1998  1.754386e-02
    ## 2663 12/01/1998 -3.421053e-01
    ## 2664 12/03/1998  3.508772e-02
    ## 2665 12/03/1998  0.000000e+00
    ## 2666 12/10/1998  4.385965e-02
    ## 2667 12/10/1998 -8.771930e-02
    ## 2668 12/14/1998  8.771930e-03
    ## 2669 12/02/1998  2.631579e-02
    ## 2670 12/10/1998 -7.894737e-02
    ## 2671 12/10/1998  3.508772e-02
    ## 2672 12/10/1998  7.894737e-02
    ## 2673 12/14/1998  1.754386e-02
    ## 2674 12/15/1998  8.771930e-03
    ## 2675 12/16/1998  3.508772e-02
    ## 2676 12/19/1998  1.754386e-02
    ## 2677 12/22/1998  5.263158e-02
    ## 2678 12/22/1998  2.105263e-01
    ## 2679 11/26/1998  3.508772e-02
    ## 2680 11/26/1998  5.263158e-02
    ## 2681 11/26/1998  0.000000e+00
    ## 2682 12/02/1998 -3.333333e-01
    ## 2683 12/04/1998  8.771930e-03
    ## 2684 12/10/1998  0.000000e+00
    ## 2685 12/15/1998  5.263158e-02
    ## 2686 12/16/1998  8.771930e-03
    ## 2687 12/17/1998  2.631579e-02
    ## 2688 12/17/1998 -8.771930e-03
    ## 2689 12/17/1998  6.140351e-02
    ## 2690 12/18/1998  1.754386e-02
    ## 2691 12/22/1998  2.631579e-02
    ## 2692 12/17/1998  8.771930e-03
    ## 2693 12/22/1998 -5.263158e-02
    ## 2694 12/23/1998  3.508772e-02
    ## 2695 12/23/1998  2.631579e-02
    ## 2696 12/23/1998  1.228070e-01
    ## 2697 12/28/1998  2.631579e-02
    ## 2698 11/24/1998  7.207207e-02
    ## 2699 11/26/1998  5.405405e-02
    ## 2700 11/26/1998  8.108108e-02
    ## 2701 11/26/1998 -4.144144e-01
    ## 2702 11/26/1998  2.702703e-02
    ## 2703 12/01/1998  3.603604e-02
    ## 2704 12/04/1998  1.801802e-02
    ## 2705 12/08/1998 -3.603604e-02
    ## 2706 12/10/1998  2.702703e-02
    ## 2707 12/10/1998  7.207207e-02
    ## 2708 12/17/1998  9.009009e-03
    ## 2709 12/22/1998  0.000000e+00
    ## 2710 12/23/1998  9.009009e-03
    ## 2711 12/23/1998  8.108108e-02
    ## 2712 12/23/1998  0.000000e+00
    ## 2713 12/21/1998  0.000000e+00
    ## 2714 12/23/1998  9.009009e-03
    ## 2715 11/23/1998  4.504505e-02
    ## 2716 12/14/1998  2.702703e-02
    ## 2717 12/27/1998  1.801802e-02
    ## 2718 12/31/1998  2.702703e-02
    ## 2719 12/02/1998  2.702703e-02
    ## 2720 12/10/1998  1.801802e-02
    ## 2721 12/11/1998  8.108108e-02
    ## 2722 12/15/1998  9.009009e-03
    ## 2723 11/24/1998  4.504505e-02
    ## 2724 11/25/1998  9.009009e-03
    ## 2725 12/03/1998  4.504505e-02
    ## 2726 12/04/1998  1.891892e-01
    ## 2727 12/04/1998  7.207207e-02
    ## 2728 12/10/1998  1.801802e-02
    ## 2729 12/10/1998  1.801802e-02
    ## 2730 12/14/1998  9.009009e-03
    ## 2731 12/15/1998 -1.801802e-02
    ## 2732 12/15/1998 -9.009009e-03
    ## 2733 12/02/1998 -3.423423e-01
    ## 2734 12/02/1998  9.009009e-03
    ## 2735 12/10/1998 -9.009009e-03
    ## 2736 12/10/1998  2.702703e-02
    ## 2737 12/11/1998  2.702703e-02
    ## 2738 12/11/1998  0.000000e+00
    ## 2739 12/15/1998  2.702703e-02
    ## 2740 12/15/1998  9.009009e-03
    ## 2741 12/15/1998  3.603604e-02
    ## 2742 12/16/1998  2.702703e-02
    ## 2743 12/17/1998  9.009009e-03
    ## 2744 12/17/1998 -3.423423e-01
    ## 2745 12/11/1998  0.000000e+00
    ## 2746 11/26/1998  9.009009e-03
    ## 2747 11/30/1998  0.000000e+00
    ## 2748 12/02/1998  9.009009e-03
    ## 2749 12/04/1998  5.405405e-02
    ## 2750 12/14/1998  0.000000e+00
    ## 2751 12/15/1998  9.009009e-03
    ## 2752 12/15/1998  5.405405e-02
    ## 2753 12/15/1998  9.009009e-03
    ## 2754 12/15/1998  1.801802e-02
    ## 2755 12/16/1998  9.009009e-03
    ## 2756 12/17/1998  9.009009e-03
    ## 2757 12/18/1998  1.801802e-02
    ## 2758 12/18/1998  1.801802e-02
    ## 2759 12/22/1998  1.801802e-02
    ## 2760 12/22/1998  9.009009e-03
    ## 2761 12/23/1998 -3.423423e-01
    ## 2762 12/22/1998  3.603604e-02
    ## 2763 12/22/1998  6.306306e-02
    ## 2764 12/23/1998  3.603604e-02
    ## 2765 12/23/1998  6.306306e-02
    ## 2766 11/23/1998  9.259259e-03
    ## 2767 11/27/1998  9.259259e-03
    ## 2768 11/27/1998  0.000000e+00
    ## 2769 11/27/1998  1.851852e-02
    ## 2770 12/01/1998  0.000000e+00
    ## 2771 12/04/1998  1.851852e-02
    ## 2772 12/04/1998  9.259259e-03
    ## 2773 12/02/1998  1.851852e-02
    ## 2774 12/02/1998 -3.425926e-01
    ## 2775 12/02/1998  0.000000e+00
    ## 2776 12/02/1998  1.851852e-02
    ## 2777 12/03/1998  9.259259e-03
    ## 2778 12/04/1998  9.259259e-03
    ## 2779 12/08/1998 -3.703704e-02
    ## 2780 12/08/1998 -1.851852e-02
    ## 2781 12/10/1998  0.000000e+00
    ## 2782 12/17/1998  0.000000e+00
    ## 2783 12/17/1998  0.000000e+00
    ## 2784 12/21/1998  3.703704e-02
    ## 2785 12/21/1998  9.259259e-02
    ## 2786 12/23/1998  5.555556e-02
    ## 2787 12/14/1998  3.703704e-02
    ## 2788 12/25/1998 -1.851852e-02
    ## 2789 12/27/1998  3.703704e-02
    ## 2790 12/27/1998  5.555556e-02
    ## 2791 12/01/1998  9.259259e-03
    ## 2792 12/04/1998  1.851852e-02
    ## 2793 12/04/1998  4.629630e-02
    ## 2794 12/04/1998  2.777778e-02
    ## 2795 12/15/1998  9.259259e-03
    ## 2796 11/24/1998  1.851852e-02
    ## 2797 11/24/1998  0.000000e+00
    ## 2798 11/24/1998  0.000000e+00
    ## 2799 11/30/1998 -9.259259e-03
    ## 2800 12/01/1998  9.259259e-03
    ## 2801 12/02/1998  2.777778e-02
    ## 2802 12/03/1998  0.000000e+00
    ## 2803 12/04/1998 -7.407407e-02
    ## 2804 12/04/1998 -9.259259e-03
    ## 2805 12/08/1998 -3.333333e-01
    ## 2806 12/10/1998  2.777778e-02
    ## 2807 12/10/1998  3.703704e-02
    ## 2808 12/14/1998  2.777778e-02
    ## 2809 12/02/1998 -1.018519e-01
    ## 2810 12/04/1998 -8.333333e-02
    ## 2811 12/04/1998  0.000000e+00
    ## 2812 12/09/1998 -8.333333e-02
    ## 2813 12/09/1998  0.000000e+00
    ## 2814 12/10/1998 -3.425926e-01
    ## 2815 12/14/1998  1.203704e-01
    ## 2816 12/17/1998  3.703704e-02
    ## 2817 12/17/1998  3.703704e-02
    ## 2818 12/17/1998  8.333333e-02
    ## 2819 12/17/1998  9.259259e-03
    ## 2820 11/30/1998  9.259259e-03
    ## 2821 12/02/1998  1.851852e-02
    ## 2822 12/04/1998  7.407407e-02
    ## 2823 12/04/1998  1.851852e-02
    ## 2824 12/10/1998  1.851852e-02
    ## 2825 12/14/1998 -3.425926e-01
    ## 2826 12/15/1998  1.851852e-02
    ## 2827 12/15/1998  5.555556e-02
    ## 2828 12/15/1998  6.481481e-02
    ## 2829 12/16/1998  0.000000e+00
    ## 2830 12/18/1998  4.629630e-02
    ## 2831 12/18/1998  9.259259e-03
    ## 2832 12/22/1998  0.000000e+00
    ## 2833 12/22/1998  5.555556e-02
    ## 2834 12/22/1998  9.259259e-03
    ## 2835 12/22/1998  9.259259e-03
    ## 2836 12/28/1998  2.777778e-02
    ## 2837 12/28/1998  0.000000e+00
    ## 2838 12/28/1998  5.555556e-02
    ## 2839 12/17/1998  6.481481e-02
    ## 2840 12/21/1998  9.259259e-03
    ## 2841 12/21/1998  2.777778e-02
    ## 2842 12/23/1998  1.111111e-01
    ## 2843 12/28/1998  9.259259e-03
    ## 2844 12/28/1998  1.851852e-02
    ## 2845 11/24/1998  1.142857e-01
    ## 2846 11/27/1998  3.809524e-02
    ## 2847 11/27/1998  1.333333e-01
    ## 2848 11/27/1998  1.333333e-01
    ## 2849 11/27/1998  3.809524e-02
    ## 2850 11/27/1998  3.809524e-02
    ## 2851 11/27/1998 -9.523810e-03
    ## 2852 12/01/1998 -2.857143e-02
    ## 2853 12/04/1998  9.523810e-03
    ## 2854 12/04/1998 -9.523810e-03
    ## 2855 12/02/1998  9.523810e-03
    ## 2856 12/03/1998  0.000000e+00
    ## 2857 12/10/1998  5.714286e-02
    ## 2858 12/10/1998  9.523810e-03
    ## 2859 12/21/1998  4.761905e-02
    ## 2860 12/21/1998  2.857143e-02
    ## 2861 12/21/1998  0.000000e+00
    ## 2862 12/22/1998  7.619048e-02
    ## 2863 12/22/1998  8.571429e-02
    ## 2864 12/23/1998  9.523810e-03
    ## 2865 12/29/1998  4.761905e-02
    ## 2866 11/23/1998  3.809524e-02
    ## 2867 12/14/1998  1.904762e-02
    ## 2868 12/25/1998  0.000000e+00
    ## 2869 12/25/1998  1.333333e-01
    ## 2870 12/27/1998  3.809524e-02
    ## 2871 12/27/1998  0.000000e+00
    ## 2872 12/01/1998  1.904762e-02
    ## 2873 12/09/1998  9.523810e-03
    ## 2874 12/04/1998  4.761905e-02
    ## 2875 12/04/1998 -4.761905e-02
    ## 2876 12/15/1998  6.666667e-02
    ## 2877 12/15/1998  5.714286e-02
    ## 2878 11/24/1998  5.714286e-02
    ## 2879 11/25/1998  9.523810e-03
    ## 2880 11/24/1998  9.523810e-03
    ## 2881 11/24/1998 -1.904762e-02
    ## 2882 11/24/1998  3.809524e-02
    ## 2883 11/24/1998 -3.428571e-01
    ## 2884 11/24/1998  3.809524e-02
    ## 2885 12/02/1998  3.809524e-02
    ## 2886 12/02/1998  0.000000e+00
    ## 2887 12/04/1998  2.857143e-02
    ## 2888 12/04/1998 -3.428571e-01
    ## 2889 12/08/1998  7.619048e-02
    ## 2890 12/15/1998  3.809524e-02
    ## 2891 12/02/1998 -1.904762e-02
    ## 2892 12/02/1998  0.000000e+00
    ## 2893 12/02/1998 -3.428571e-01
    ## 2894 12/04/1998  9.523810e-03
    ## 2895 12/09/1998  2.857143e-02
    ## 2896 12/09/1998  9.523810e-03
    ## 2897 12/09/1998  5.714286e-02
    ## 2898 12/11/1998  1.904762e-02
    ## 2899 12/11/1998  5.714286e-02
    ## 2900 12/15/1998  4.761905e-02
    ## 2901 12/15/1998  9.523810e-03
    ## 2902 12/16/1998  3.809524e-02
    ## 2903 12/16/1998  3.809524e-02
    ## 2904 12/17/1998  0.000000e+00
    ## 2905 12/08/1998 -3.428571e-01
    ## 2906 12/04/1998  2.857143e-02
    ## 2907 12/10/1998  9.523810e-02
    ## 2908 12/10/1998  9.523810e-03
    ## 2909 12/10/1998  1.428571e-01
    ## 2910 12/10/1998 -9.523810e-03
    ## 2911 12/10/1998  2.857143e-02
    ## 2912 12/14/1998  9.523810e-03
    ## 2913 12/14/1998  9.523810e-03
    ## 2914 12/15/1998 -3.333333e-01
    ## 2915 12/15/1998  1.142857e-01
    ## 2916 12/15/1998  9.523810e-03
    ## 2917 12/15/1998  5.714286e-02
    ## 2918 12/16/1998 -3.333333e-01
    ## 2919 12/18/1998  8.571429e-02
    ## 2920 12/18/1998  3.809524e-02
    ## 2921 12/23/1998  2.857143e-02
    ## 2922 12/23/1998  0.000000e+00
    ## 2923 12/28/1998  2.857143e-02
    ## 2924 12/17/1998  0.000000e+00
    ## 2925 12/21/1998  5.714286e-02
    ## 2926 12/28/1998  1.904762e-02
    ## 2927 11/23/1998  1.960784e-02
    ## 2928 11/24/1998  2.941176e-02
    ## 2929 11/24/1998  0.000000e+00
    ## 2930 11/24/1998  3.921569e-02
    ## 2931 11/26/1998  1.960784e-02
    ## 2932 11/27/1998  4.901961e-02
    ## 2933 12/01/1998 -2.941176e-02
    ## 2934 12/02/1998  2.941176e-02
    ## 2935 12/02/1998  1.960784e-02
    ## 2936 12/03/1998  7.843137e-02
    ## 2937 12/08/1998  4.901961e-02
    ## 2938 12/10/1998  1.960784e-02
    ## 2939 12/17/1998  0.000000e+00
    ## 2940 12/17/1998  0.000000e+00
    ## 2941 12/22/1998  3.921569e-02
    ## 2942 12/22/1998  1.960784e-02
    ## 2943 12/23/1998  3.921569e-02
    ## 2944 12/23/1998  4.901961e-02
    ## 2945 12/21/1998  9.803922e-03
    ## 2946 12/29/1998  1.372549e-01
    ## 2947 11/23/1998 -1.960784e-02
    ## 2948 12/25/1998  1.078431e-01
    ## 2949 12/27/1998  1.960784e-02
    ## 2950 12/31/1998  2.941176e-02
    ## 2951 12/31/1998  2.941176e-02
    ## 2952 12/01/1998  2.941176e-02
    ## 2953 12/02/1998  5.882353e-02
    ## 2954 12/04/1998  4.901961e-02
    ## 2955 12/09/1998  0.000000e+00
    ## 2956 12/09/1998  2.941176e-02
    ## 2957 12/09/1998  2.941176e-02
    ## 2958 12/09/1998  3.921569e-02
    ## 2959 12/09/1998  2.941176e-02
    ## 2960 12/15/1998 -9.803922e-03
    ## 2961 12/15/1998  3.921569e-02
    ## 2962 11/24/1998  2.941176e-02
    ## 2963 11/24/1998  3.921569e-02
    ## 2964 11/24/1998  5.882353e-02
    ## 2965 11/24/1998  3.921569e-02
    ## 2966 11/24/1998  4.901961e-02
    ## 2967 11/24/1998  1.960784e-02
    ## 2968 12/04/1998  1.078431e-01
    ## 2969 12/08/1998  7.843137e-02
    ## 2970 12/08/1998  4.901961e-02
    ## 2971 12/10/1998  9.803922e-03
    ## 2972 12/01/1998  0.000000e+00
    ## 2973 12/01/1998  3.921569e-02
    ## 2974 12/10/1998  1.960784e-02
    ## 2975 12/14/1998  9.803922e-03
    ## 2976 12/14/1998  9.803922e-02
    ## 2977 12/14/1998 -9.803922e-03
    ## 2978 12/14/1998  1.372549e-01
    ## 2979 12/14/1998  9.803922e-03
    ## 2980 12/16/1998  0.000000e+00
    ## 2981 12/16/1998  1.960784e-02
    ## 2982 11/26/1998  7.843137e-02
    ## 2983 11/26/1998  7.843137e-02
    ## 2984 11/30/1998  4.901961e-02
    ## 2985 12/03/1998  2.941176e-02
    ## 2986 12/04/1998  9.803922e-03
    ## 2987 12/04/1998  9.803922e-03
    ## 2988 12/04/1998  0.000000e+00
    ## 2989 12/04/1998  0.000000e+00
    ## 2990 12/10/1998  4.901961e-02
    ## 2991 12/10/1998  0.000000e+00
    ## 2992 12/14/1998  9.803922e-03
    ## 2993 12/14/1998  4.901961e-02
    ## 2994 12/14/1998  7.843137e-02
    ## 2995 12/14/1998  9.803922e-03
    ## 2996 12/15/1998  9.803922e-02
    ## 2997 12/15/1998  4.901961e-02
    ## 2998 12/15/1998  9.803922e-02
    ## 2999 12/15/1998  2.941176e-02
    ## 3000 12/15/1998  4.901961e-02
    ## 3001 12/15/1998  3.921569e-02
    ## 3002 12/15/1998 -3.431373e-01
    ## 3003 12/16/1998 -3.431373e-01
    ## 3004 12/16/1998  0.000000e+00
    ## 3005 12/17/1998  9.803922e-03
    ## 3006 12/18/1998  9.803922e-03
    ## 3007 12/18/1998  0.000000e+00
    ## 3008 12/22/1998  9.803922e-03
    ## 3009 12/28/1998  4.901961e-02
    ## 3010 12/28/1998  5.882353e-02
    ## 3011 12/17/1998 -3.333333e-01
    ## 3012 12/22/1998  2.941176e-02
    ## 3013 12/23/1998  2.941176e-02
    ## 3014 12/23/1998  4.901961e-02
    ## 3015 12/23/1998  6.862745e-02
    ## 3016 12/17/1998  0.000000e+00
    ## 3017 11/23/1998  5.050505e-02
    ## 3018 12/10/1998  1.010101e-02
    ## 3019 11/26/1998 -1.010101e-02
    ## 3020 11/27/1998  3.030303e-02
    ## 3021 11/27/1998  1.010101e-02
    ## 3022 11/27/1998  4.040404e-02
    ## 3023 12/01/1998  1.010101e-02
    ## 3024 12/01/1998  1.010101e-02
    ## 3025 12/01/1998 -1.010101e-02
    ## 3026 12/03/1998  4.040404e-02
    ## 3027 12/03/1998  1.010101e-02
    ## 3028 12/04/1998  2.020202e-02
    ## 3029 12/04/1998  0.000000e+00
    ## 3030 12/04/1998  3.030303e-02
    ## 3031 12/08/1998  0.000000e+00
    ## 3032 12/10/1998 -1.010101e-02
    ## 3033 12/10/1998  3.030303e-02
    ## 3034 12/11/1998 -3.434343e-01
    ## 3035 12/17/1998  4.040404e-02
    ## 3036 12/17/1998 -3.434343e-01
    ## 3037 12/22/1998  3.030303e-02
    ## 3038 12/23/1998  5.050505e-02
    ## 3039 12/21/1998  0.000000e+00
    ## 3040 12/21/1998  0.000000e+00
    ## 3041 12/21/1998 -3.434343e-01
    ## 3042 12/23/1998  3.030303e-02
    ## 3043 11/23/1998  0.000000e+00
    ## 3044 12/25/1998  5.050505e-02
    ## 3045 12/27/1998  2.020202e-02
    ## 3046 12/27/1998  2.020202e-02
    ## 3047 12/27/1998  1.111111e-01
    ## 3048 12/27/1998  2.020202e-02
    ## 3049 12/01/1998  3.030303e-02
    ## 3050 12/02/1998  3.030303e-02
    ## 3051 12/09/1998  4.040404e-02
    ## 3052 12/09/1998  2.020202e-02
    ## 3053 12/09/1998 -1.010101e-02
    ## 3054 12/11/1998  0.000000e+00
    ## 3055 12/11/1998  2.020202e-02
    ## 3056 12/15/1998  1.010101e-02
    ## 3057 12/15/1998  1.010101e-02
    ## 3058 12/15/1998  4.040404e-02
    ## 3059 12/15/1998  1.010101e-01
    ## 3060 11/24/1998  0.000000e+00
    ## 3061 11/24/1998  2.020202e-02
    ## 3062 11/24/1998  0.000000e+00
    ## 3063 11/30/1998  3.030303e-02
    ## 3064 12/01/1998  3.030303e-02
    ## 3065 12/04/1998 -1.010101e-02
    ## 3066 12/08/1998 -3.333333e-01
    ## 3067 12/15/1998  4.040404e-02
    ## 3068 12/01/1998  0.000000e+00
    ## 3069 12/01/1998  1.010101e-02
    ## 3070 12/04/1998  3.030303e-02
    ## 3071 12/11/1998  1.010101e-02
    ## 3072 12/14/1998  6.060606e-02
    ## 3073 12/14/1998 -1.010101e-02
    ## 3074 12/14/1998  2.020202e-02
    ## 3075 12/14/1998  1.010101e-02
    ## 3076 12/14/1998 -1.010101e-02
    ## 3077 12/15/1998  6.060606e-02
    ## 3078 12/15/1998 -1.010101e-02
    ## 3079 12/15/1998 -3.030303e-02
    ## 3080 12/16/1998  2.020202e-02
    ## 3081 12/17/1998  0.000000e+00
    ## 3082 12/19/1998  2.020202e-02
    ## 3083 12/22/1998 -3.434343e-01
    ## 3084 11/26/1998  5.050505e-02
    ## 3085 12/02/1998  3.030303e-02
    ## 3086 12/02/1998  0.000000e+00
    ## 3087 12/02/1998  4.040404e-02
    ## 3088 12/02/1998  0.000000e+00
    ## 3089 12/03/1998  3.030303e-02
    ## 3090 12/03/1998  3.030303e-02
    ## 3091 12/04/1998  1.010101e-02
    ## 3092 11/04/1998  4.040404e-02
    ## 3093 11/04/1998  2.020202e-02
    ## 3094 11/04/1998 -1.010101e-02
    ## 3095 12/15/1998  0.000000e+00
    ## 3096 12/17/1998  0.000000e+00
    ## 3097 12/17/1998  0.000000e+00
    ## 3098 12/18/1998  5.050505e-02
    ## 3099 12/22/1998  2.020202e-02
    ## 3100 12/17/1998  1.010101e-02
    ## 3101 12/08/1998  0.000000e+00
    ## 3102 12/21/1998  3.030303e-02
    ## 3103 12/21/1998  0.000000e+00
    ## 3104 12/23/1998  0.000000e+00
    ## 3105 12/23/1998  4.040404e-02
    ## 3106 12/23/1998  0.000000e+00
    ## 3107 12/23/1998  6.060606e-02
    ## 3108 12/28/1998  1.010101e-02
    ## 3109 12/23/1998 -1.010101e-02
    ## 3110 11/23/1998  2.083333e-02
    ## 3111 11/26/1998 -1.875000e-01
    ## 3112 11/26/1998 -2.083333e-02
    ## 3113 11/27/1998  0.000000e+00
    ## 3114 11/27/1998  1.250000e-01
    ## 3115 11/27/1998  1.562500e-01
    ## 3116 11/27/1998  1.354167e-01
    ## 3117 11/27/1998  3.125000e-02
    ## 3118 11/27/1998  1.041667e-01
    ## 3119 12/04/1998  2.083333e-02
    ## 3120 12/04/1998  1.041667e-02
    ## 3121 12/02/1998 -3.437500e-01
    ## 3122 12/08/1998  0.000000e+00
    ## 3123 12/10/1998  1.041667e-02
    ## 3124 12/10/1998  0.000000e+00
    ## 3125 12/17/1998  0.000000e+00
    ## 3126 12/17/1998  1.041667e-02
    ## 3127 12/22/1998 -3.437500e-01
    ## 3128 12/22/1998  1.041667e-02
    ## 3129 12/23/1998  2.083333e-02
    ## 3130 12/17/1998  1.041667e-02
    ## 3131 12/17/1998  0.000000e+00
    ## 3132 12/22/1998  0.000000e+00
    ## 3133 12/23/1998  6.250000e-02
    ## 3134 12/29/1998 -6.250000e-02
    ## 3135 12/29/1998  0.000000e+00
    ## 3136 11/23/1998  1.041667e-02
    ## 3137 11/23/1998  4.166667e-02
    ## 3138 11/23/1998  1.770833e-01
    ## 3139 11/23/1998  4.166667e-02
    ## 3140 12/14/1998  4.166667e-02
    ## 3141 12/25/1998  3.125000e-02
    ## 3142 12/27/1998  2.395833e-01
    ## 3143 12/27/1998  0.000000e+00
    ## 3144 12/27/1998  1.041667e-02
    ## 3145 12/27/1998  2.083333e-02
    ## 3146 12/27/1998  3.125000e-02
    ## 3147 12/31/1998  2.083333e-02
    ## 3148 12/31/1998  3.125000e-02
    ## 3149 12/01/1998  4.166667e-02
    ## 3150 12/15/1998 -3.333333e-01
    ## 3151 12/04/1998  2.083333e-02
    ## 3152 12/04/1998  3.125000e-02
    ## 3153 12/04/1998  0.000000e+00
    ## 3154 12/09/1998  1.041667e-02
    ## 3155 12/11/1998  1.041667e-02
    ## 3156 12/11/1998  0.000000e+00
    ## 3157 12/15/1998  4.166667e-02
    ## 3158 12/15/1998 -2.083333e-02
    ## 3159 12/15/1998  0.000000e+00
    ## 3160 12/15/1998 -3.437500e-01
    ## 3161 12/15/1998  2.083333e-02
    ## 3162 11/24/1998  7.291667e-02
    ## 3163 11/24/1998 -1.041667e-02
    ## 3164 11/24/1998  0.000000e+00
    ## 3165 11/30/1998  4.166667e-02
    ## 3166 11/30/1998  4.166667e-02
    ## 3167 11/30/1998  1.041667e-02
    ## 3168 12/01/1998  0.000000e+00
    ## 3169 12/02/1998  2.083333e-02
    ## 3170 12/03/1998  4.166667e-02
    ## 3171 12/04/1998  7.291667e-02
    ## 3172 12/08/1998  0.000000e+00
    ## 3173 12/10/1998  1.041667e-02
    ## 3174 12/14/1998  4.166667e-02
    ## 3175 12/01/1998  0.000000e+00
    ## 3176 12/02/1998  0.000000e+00
    ## 3177 12/02/1998  5.208333e-02
    ## 3178 12/02/1998  0.000000e+00
    ## 3179 12/02/1998  0.000000e+00
    ## 3180 12/09/1998  0.000000e+00
    ## 3181 12/11/1998  2.083333e-02
    ## 3182 12/15/1998 -1.041667e-02
    ## 3183 12/15/1998  0.000000e+00
    ## 3184 12/15/1998  0.000000e+00
    ## 3185 12/15/1998 -1.562500e-01
    ## 3186 12/15/1998  4.166667e-02
    ## 3187 12/16/1998  4.166667e-02
    ## 3188 12/16/1998  3.125000e-02
    ## 3189 12/16/1998  2.083333e-02
    ## 3190 12/17/1998  2.083333e-02
    ## 3191 12/17/1998  0.000000e+00
    ## 3192 12/17/1998  1.041667e-02
    ## 3193 12/17/1998  1.041667e-02
    ## 3194 12/19/1998  1.041667e-02
    ## 3195 12/22/1998 -2.187500e-01
    ## 3196 11/26/1998  4.166667e-02
    ## 3197 12/02/1998  2.083333e-02
    ## 3198 12/04/1998 -3.437500e-01
    ## 3199 12/04/1998  3.125000e-02
    ## 3200 11/04/1998  5.208333e-02
    ## 3201 11/04/1998  0.000000e+00
    ## 3202 12/14/1998  1.041667e-02
    ## 3203 12/14/1998  8.333333e-02
    ## 3204 12/15/1998  1.041667e-02
    ## 3205 12/15/1998  3.125000e-02
    ## 3206 12/16/1998  2.083333e-02
    ## 3207 12/17/1998  0.000000e+00
    ## 3208 12/18/1998  2.083333e-02
    ## 3209 12/18/1998  1.041667e-02
    ## 3210 12/18/1998  0.000000e+00
    ## 3211 12/22/1998  4.166667e-02
    ## 3212 12/23/1998 -6.250000e-02
    ## 3213 12/23/1998  0.000000e+00
    ## 3214 12/28/1998  0.000000e+00
    ## 3215 12/17/1998  0.000000e+00
    ## 3216 12/21/1998  0.000000e+00
    ## 3217 12/21/1998  1.041667e-02
    ## 3218 12/22/1998 -3.437500e-01
    ## 3219 12/23/1998  6.250000e-02
    ## 3220 12/23/1998  4.166667e-02
    ## 3221 12/23/1998  1.041667e-02
    ## 3222 12/23/1998  6.250000e-02
    ## 3223 11/23/1998  5.376344e-02
    ## 3224 11/23/1998  9.677419e-02
    ## 3225 11/24/1998 -2.150538e-02
    ## 3226 11/24/1998  2.150538e-02
    ## 3227 11/27/1998  1.290323e-01
    ## 3228 11/27/1998 -2.150538e-02
    ## 3229 11/27/1998 -3.440860e-01
    ## 3230 11/27/1998  4.301075e-02
    ## 3231 12/01/1998 -1.075269e-02
    ## 3232 12/01/1998  3.225806e-02
    ## 3233 12/04/1998 -1.720430e-01
    ## 3234 12/02/1998  0.000000e+00
    ## 3235 12/02/1998  5.376344e-02
    ## 3236 12/02/1998  2.258065e-01
    ## 3237 12/03/1998  4.301075e-02
    ## 3238 12/04/1998  2.150538e-02
    ## 3239 12/04/1998 -4.301075e-01
    ## 3240 12/10/1998  1.075269e-02
    ## 3241 12/10/1998 -3.333333e-01
    ## 3242 12/10/1998  0.000000e+00
    ## 3243 12/10/1998  0.000000e+00
    ## 3244 12/10/1998  0.000000e+00
    ## 3245 12/23/1998  7.526882e-02
    ## 3246 12/23/1998  0.000000e+00
    ## 3247 12/21/1998  1.075269e-02
    ## 3248 12/21/1998 -2.150538e-02
    ## 3249 12/21/1998 -1.075269e-02
    ## 3250 12/21/1998  0.000000e+00
    ## 3251 12/22/1998  9.677419e-02
    ## 3252 12/29/1998 -8.602151e-02
    ## 3253 11/23/1998  8.602151e-02
    ## 3254 12/14/1998  1.182796e-01
    ## 3255 12/14/1998  4.301075e-02
    ## 3256 12/25/1998  2.150538e-02
    ## 3257 12/27/1998  1.075269e-02
    ## 3258 12/31/1998  1.075269e-02
    ## 3259 12/02/1998  3.225806e-02
    ## 3260 12/04/1998  4.301075e-02
    ## 3261 12/04/1998  0.000000e+00
    ## 3262 12/04/1998  2.150538e-02
    ## 3263 12/09/1998  0.000000e+00
    ## 3264 12/09/1998  6.451613e-02
    ## 3265 12/11/1998  0.000000e+00
    ## 3266 12/15/1998  0.000000e+00
    ## 3267 12/15/1998  1.075269e-02
    ## 3268 12/15/1998  8.602151e-02
    ## 3269 12/15/1998  5.376344e-02
    ## 3270 12/15/1998  2.150538e-02
    ## 3271 11/24/1998  2.150538e-02
    ## 3272 11/24/1998  0.000000e+00
    ## 3273 11/24/1998  1.075269e-02
    ## 3274 11/24/1998  0.000000e+00
    ## 3275 11/30/1998  6.451613e-02
    ## 3276 11/30/1998  0.000000e+00
    ## 3277 12/01/1998  7.526882e-02
    ## 3278 12/01/1998  5.376344e-02
    ## 3279 12/01/1998  8.602151e-02
    ## 3280 12/01/1998  1.075269e-02
    ## 3281 12/08/1998  3.225806e-02
    ## 3282 12/08/1998  3.225806e-02
    ## 3283 12/10/1998  0.000000e+00
    ## 3284 12/14/1998 -1.075269e-02
    ## 3285 12/15/1998  1.075269e-02
    ## 3286 12/15/1998 -3.333333e-01
    ## 3287 12/01/1998  0.000000e+00
    ## 3288 12/04/1998  1.075269e-02
    ## 3289 12/08/1998  6.451613e-02
    ## 3290 12/08/1998  0.000000e+00
    ## 3291 12/09/1998  0.000000e+00
    ## 3292 12/09/1998  3.225806e-02
    ## 3293 12/11/1998  0.000000e+00
    ## 3294 12/14/1998 -1.075269e-02
    ## 3295 12/14/1998  1.075269e-02
    ## 3296 12/15/1998 -2.150538e-02
    ## 3297 12/15/1998  1.075269e-02
    ## 3298 12/16/1998  0.000000e+00
    ## 3299 12/16/1998  2.150538e-02
    ## 3300 12/17/1998  0.000000e+00
    ## 3301 12/17/1998  1.075269e-02
    ## 3302 12/19/1998  2.150538e-02
    ## 3303 12/19/1998  1.075269e-02
    ## 3304 12/19/1998  2.150538e-02
    ## 3305 11/26/1998 -6.451613e-02
    ## 3306 11/30/1998  0.000000e+00
    ## 3307 11/30/1998  3.225806e-02
    ## 3308 12/04/1998  0.000000e+00
    ## 3309 12/04/1998  0.000000e+00
    ## 3310 12/04/1998  4.301075e-02
    ## 3311 12/10/1998 -3.440860e-01
    ## 3312 12/10/1998  2.150538e-02
    ## 3313 12/15/1998 -4.301075e-01
    ## 3314 12/15/1998  5.376344e-02
    ## 3315 12/17/1998  2.150538e-02
    ## 3316 12/18/1998  1.075269e-02
    ## 3317 12/22/1998  0.000000e+00
    ## 3318 12/23/1998  1.075269e-02
    ## 3319 12/23/1998  0.000000e+00
    ## 3320 12/21/1998  1.075269e-01
    ## 3321 12/22/1998  1.075269e-02
    ## 3322 12/22/1998 -3.440860e-01
    ## 3323 12/23/1998  4.301075e-02
    ## 3324 12/23/1998  5.376344e-02
    ## 3325 12/28/1998  1.075269e-02
    ## 3326 11/23/1998  3.333333e-02
    ## 3327 11/26/1998  4.444444e-02
    ## 3328 11/26/1998 -1.777778e-01
    ## 3329 11/27/1998  2.222222e-02
    ## 3330 11/27/1998 -2.222222e-02
    ## 3331 12/01/1998  2.222222e-02
    ## 3332 12/01/1998  1.111111e-02
    ## 3333 12/01/1998  1.111111e-02
    ## 3334 12/04/1998 -1.111111e-02
    ## 3335 12/02/1998  2.222222e-02
    ## 3336 12/02/1998 -1.111111e-02
    ## 3337 12/02/1998  2.222222e-02
    ## 3338 12/04/1998  5.555556e-02
    ## 3339 12/04/1998  8.888889e-02
    ## 3340 12/04/1998  1.111111e-02
    ## 3341 12/08/1998  0.000000e+00
    ## 3342 12/10/1998  1.111111e-02
    ## 3343 12/17/1998  1.111111e-02
    ## 3344 12/17/1998  0.000000e+00
    ## 3345 12/17/1998 -1.111111e-02
    ## 3346 12/22/1998  3.333333e-02
    ## 3347 12/22/1998  1.111111e-02
    ## 3348 12/22/1998  7.777778e-02
    ## 3349 12/23/1998 -3.444444e-01
    ## 3350 12/23/1998  3.333333e-02
    ## 3351 12/21/1998  2.222222e-02
    ## 3352 12/21/1998  1.111111e-02
    ## 3353 12/23/1998  3.333333e-02
    ## 3354 12/23/1998  3.333333e-02
    ## 3355 12/29/1998  5.555556e-02
    ## 3356 12/29/1998 -5.555556e-02
    ## 3357 12/29/1998  6.666667e-02
    ## 3358 12/29/1998  5.555556e-02
    ## 3359 12/29/1998  2.222222e-02
    ## 3360 12/29/1998 -3.333333e-02
    ## 3361 11/23/1998  6.666667e-02
    ## 3362 11/23/1998  4.444444e-02
    ## 3363 11/23/1998  4.444444e-02
    ## 3364 12/14/1998  8.888889e-02
    ## 3365 12/14/1998  2.222222e-02
    ## 3366 12/14/1998  3.333333e-02
    ## 3367 12/25/1998  1.000000e-01
    ## 3368 12/25/1998  6.666667e-02
    ## 3369 12/25/1998  8.888889e-02
    ## 3370 12/25/1998  4.444444e-02
    ## 3371 12/25/1998  7.777778e-02
    ## 3372 12/25/1998  1.000000e-01
    ## 3373 12/27/1998  3.333333e-02
    ## 3374 12/27/1998  5.555556e-02
    ## 3375 12/27/1998  6.666667e-02
    ## 3376 12/31/1998  1.111111e-02
    ## 3377 12/31/1998 -3.444444e-01
    ## 3378 12/31/1998  1.000000e-01
    ## 3379 12/01/1998  0.000000e+00
    ## 3380 12/01/1998  3.333333e-02
    ## 3381 12/02/1998  1.111111e-02
    ## 3382 12/02/1998  7.777778e-02
    ## 3383 12/09/1998  0.000000e+00
    ## 3384 12/04/1998  1.444444e-01
    ## 3385 12/04/1998  4.444444e-02
    ## 3386 12/04/1998  3.333333e-02
    ## 3387 12/11/1998  3.333333e-02
    ## 3388 12/15/1998  2.222222e-02
    ## 3389 12/15/1998 -3.444444e-01
    ## 3390 12/15/1998  1.111111e-02
    ## 3391 12/15/1998  5.555556e-02
    ## 3392 12/15/1998  1.222222e-01
    ## 3393 12/27/1998  0.000000e+00
    ## 3394 11/24/1998  2.222222e-02
    ## 3395 11/24/1998  4.444444e-02
    ## 3396 11/24/1998  2.222222e-02
    ## 3397 11/30/1998  3.333333e-02
    ## 3398 12/01/1998  0.000000e+00
    ## 3399 12/02/1998  1.111111e-02
    ## 3400 12/08/1998 -3.444444e-01
    ## 3401 12/08/1998  3.333333e-02
    ## 3402 12/08/1998  2.000000e-01
    ## 3403 12/08/1998  8.888889e-02
    ## 3404 12/15/1998  1.111111e-02
    ## 3405 12/15/1998 -3.333333e-01
    ## 3406 12/01/1998  2.222222e-02
    ## 3407 12/01/1998 -1.111111e-02
    ## 3408 12/01/1998  7.777778e-02
    ## 3409 12/02/1998 -3.444444e-01
    ## 3410 12/04/1998  0.000000e+00
    ## 3411 12/04/1998  3.333333e-02
    ## 3412 12/09/1998  0.000000e+00
    ## 3413 12/09/1998  0.000000e+00
    ## 3414 12/10/1998  0.000000e+00
    ## 3415 12/11/1998  1.111111e-02
    ## 3416 12/14/1998  1.777778e-01
    ## 3417 12/15/1998  0.000000e+00
    ## 3418 12/16/1998  0.000000e+00
    ## 3419 12/16/1998  0.000000e+00
    ## 3420 12/19/1998  1.111111e-02
    ## 3421 12/19/1998 -3.444444e-01
    ## 3422 12/22/1998 -4.333333e-01
    ## 3423 11/30/1998  0.000000e+00
    ## 3424 12/02/1998  3.333333e-02
    ## 3425 12/03/1998  1.111111e-02
    ## 3426 12/03/1998  0.000000e+00
    ## 3427 12/04/1998  2.222222e-02
    ## 3428 12/10/1998  7.777778e-02
    ## 3429 12/10/1998  4.444444e-02
    ## 3430 12/15/1998  1.111111e-02
    ## 3431 12/17/1998  1.111111e-02
    ## 3432 12/17/1998  5.555556e-02
    ## 3433 12/18/1998  2.222222e-02
    ## 3434 12/18/1998  1.111111e-02
    ## 3435 12/22/1998  1.000000e-01
    ## 3436 12/22/1998  2.222222e-02
    ## 3437 12/22/1998  1.111111e-02
    ## 3438 12/22/1998  2.222222e-02
    ## 3439 12/22/1998  1.111111e-02
    ## 3440 12/23/1998  1.111111e-02
    ## 3441 12/23/1998  4.444444e-02
    ## 3442 12/17/1998  2.222222e-02
    ## 3443 12/17/1998  1.111111e-02
    ## 3444 12/22/1998  6.666667e-02
    ## 3445 12/23/1998  4.444444e-02
    ## 3446 12/28/1998  6.666667e-02
    ## 3447 12/17/1998  1.222222e-01
    ## 3448 11/23/1998  1.149425e-02
    ## 3449 11/24/1998 -1.149425e-01
    ## 3450 11/26/1998  1.609195e-01
    ## 3451 11/26/1998 -2.298851e-02
    ## 3452 11/26/1998  2.068966e-01
    ## 3453 11/27/1998  1.034483e-01
    ## 3454 11/27/1998 -3.333333e-01
    ## 3455 11/27/1998  1.034483e-01
    ## 3456 11/27/1998  0.000000e+00
    ## 3457 11/27/1998  1.034483e-01
    ## 3458 11/27/1998  0.000000e+00
    ## 3459 11/27/1998  9.195402e-02
    ## 3460 12/04/1998  5.747126e-02
    ## 3461 12/02/1998  0.000000e+00
    ## 3462 12/02/1998 -1.149425e-02
    ## 3463 12/02/1998 -1.149425e-02
    ## 3464 12/03/1998 -2.298851e-02
    ## 3465 12/04/1998  0.000000e+00
    ## 3466 12/04/1998  5.747126e-02
    ## 3467 12/04/1998 -2.298851e-02
    ## 3468 12/08/1998  0.000000e+00
    ## 3469 12/08/1998 -2.298851e-02
    ## 3470 12/10/1998  1.149425e-01
    ## 3471 12/10/1998  5.747126e-02
    ## 3472 12/11/1998 -2.068966e-01
    ## 3473 12/17/1998 -2.298851e-02
    ## 3474 12/17/1998  1.149425e-02
    ## 3475 12/22/1998  3.448276e-02
    ## 3476 12/23/1998 -3.448276e-01
    ## 3477 12/23/1998  0.000000e+00
    ## 3478 12/23/1998  1.149425e-02
    ## 3479 12/17/1998  1.149425e-02
    ## 3480 12/17/1998  8.045977e-02
    ## 3481 12/21/1998 -1.149425e-02
    ## 3482 12/22/1998 -1.149425e-02
    ## 3483 12/29/1998  2.298851e-02
    ## 3484 12/14/1998  1.149425e-02
    ## 3485 12/14/1998  4.597701e-02
    ## 3486 12/25/1998  1.149425e-02
    ## 3487 12/27/1998  9.195402e-02
    ## 3488 12/27/1998 -2.298851e-02
    ## 3489 12/27/1998 -1.379310e-01
    ## 3490 12/27/1998  2.298851e-02
    ## 3491 12/31/1998 -3.333333e-01
    ## 3492 12/09/1998  2.298851e-02
    ## 3493 12/09/1998  1.149425e-02
    ## 3494 12/09/1998  3.448276e-02
    ## 3495 12/04/1998  1.149425e-02
    ## 3496 12/04/1998  2.298851e-02
    ## 3497 12/04/1998  2.298851e-02
    ## 3498 12/04/1998  1.034483e-01
    ## 3499 12/04/1998  1.149425e-02
    ## 3500 12/09/1998  2.298851e-02
    ## 3501 12/10/1998  1.149425e-02
    ## 3502 12/10/1998  0.000000e+00
    ## 3503 12/15/1998 -1.034483e-01
    ## 3504 12/15/1998 -3.333333e-01
    ## 3505 11/24/1998 -1.724138e-01
    ## 3506 11/24/1998  6.896552e-02
    ## 3507 11/24/1998  0.000000e+00
    ## 3508 11/25/1998  3.448276e-02
    ## 3509 11/25/1998  1.149425e-02
    ## 3510 11/25/1998  2.298851e-02
    ## 3511 11/25/1998  1.149425e-02
    ## 3512 11/25/1998 -1.149425e-02
    ## 3513 11/25/1998 -2.298851e-02
    ## 3514 11/30/1998  3.448276e-02
    ## 3515 11/30/1998  1.149425e-02
    ## 3516 11/30/1998 -1.149425e-02
    ## 3517 11/30/1998  2.298851e-02
    ## 3518 12/01/1998 -8.045977e-02
    ## 3519 12/01/1998 -1.034483e-01
    ## 3520 12/04/1998 -3.448276e-01
    ## 3521 12/04/1998  6.896552e-02
    ## 3522 12/04/1998  4.597701e-02
    ## 3523 12/08/1998  1.034483e-01
    ## 3524 12/08/1998  1.149425e-01
    ## 3525 12/10/1998  2.298851e-02
    ## 3526 12/14/1998 -3.333333e-01
    ## 3527 12/14/1998  0.000000e+00
    ## 3528 12/15/1998 -3.448276e-01
    ## 3529 12/01/1998  0.000000e+00
    ## 3530 12/01/1998  8.045977e-02
    ## 3531 12/01/1998  1.379310e-01
    ## 3532 12/02/1998 -3.333333e-01
    ## 3533 12/04/1998  1.839080e-01
    ## 3534 12/04/1998  0.000000e+00
    ## 3535 12/09/1998  2.298851e-02
    ## 3536 12/09/1998  2.298851e-02
    ## 3537 12/10/1998 -3.448276e-01
    ## 3538 12/10/1998  3.448276e-02
    ## 3539 12/14/1998  4.597701e-02
    ## 3540 12/14/1998  1.149425e-02
    ## 3541 12/15/1998  2.298851e-02
    ## 3542 12/15/1998  1.149425e-02
    ## 3543 12/16/1998  0.000000e+00
    ## 3544 12/16/1998  0.000000e+00
    ## 3545 12/16/1998 -1.149425e-02
    ## 3546 12/19/1998  0.000000e+00
    ## 3547 12/22/1998  4.597701e-02
    ## 3548 12/22/1998 -3.333333e-01
    ## 3549 12/02/1998  1.149425e-02
    ## 3550 12/04/1998  4.597701e-02
    ## 3551 12/10/1998  5.747126e-02
    ## 3552 12/15/1998  2.298851e-02
    ## 3553 12/15/1998  0.000000e+00
    ## 3554 12/15/1998  1.149425e-02
    ## 3555 12/15/1998  5.747126e-02
    ## 3556 12/16/1998 -4.367816e-01
    ## 3557 12/16/1998 -3.448276e-02
    ## 3558 12/17/1998  2.298851e-02
    ## 3559 12/18/1998  1.149425e-02
    ## 3560 12/22/1998  3.448276e-02
    ## 3561 12/28/1998  5.747126e-02
    ## 3562 12/17/1998  1.149425e-02
    ## 3563 12/22/1998  0.000000e+00
    ## 3564 12/22/1998  0.000000e+00
    ## 3565 12/23/1998  5.747126e-02
    ## 3566 12/23/1998  8.045977e-02
    ## 3567 12/23/1998 -2.298851e-02
    ## 3568 12/23/1998  3.448276e-02
    ## 3569 12/28/1998  0.000000e+00
    ## 3570 12/23/1998 -1.149425e-01
    ## 3571 11/23/1998  1.190476e-02
    ## 3572 11/23/1998  3.571429e-02
    ## 3573 11/23/1998 -9.523810e-02
    ## 3574 11/23/1998  3.571429e-02
    ## 3575 11/23/1998  7.142857e-02
    ## 3576 11/23/1998  3.571429e-02
    ## 3577 11/24/1998 -8.333333e-02
    ## 3578 12/08/1998  0.000000e+00
    ## 3579 11/26/1998  9.523810e-02
    ## 3580 11/26/1998  9.523810e-02
    ## 3581 11/27/1998  5.952381e-02
    ## 3582 11/27/1998  5.952381e-02
    ## 3583 12/01/1998  1.190476e-02
    ## 3584 12/01/1998  7.142857e-02
    ## 3585 12/04/1998 -4.404762e-01
    ## 3586 12/04/1998  1.190476e-01
    ## 3587 12/02/1998  3.571429e-02
    ## 3588 12/02/1998 -1.190476e-02
    ## 3589 12/03/1998  8.333333e-02
    ## 3590 12/03/1998 -2.380952e-02
    ## 3591 12/08/1998  1.190476e-02
    ## 3592 12/08/1998  1.190476e-02
    ## 3593 12/08/1998 -2.380952e-02
    ## 3594 12/10/1998  5.952381e-02
    ## 3595 12/17/1998  1.190476e-02
    ## 3596 12/17/1998  1.190476e-02
    ## 3597 12/23/1998 -1.666667e-01
    ## 3598 12/23/1998  1.190476e-02
    ## 3599 12/23/1998  0.000000e+00
    ## 3600 12/23/1998 -1.428571e-01
    ## 3601 12/23/1998  9.523810e-02
    ## 3602 12/17/1998  4.761905e-02
    ## 3603 12/17/1998  0.000000e+00
    ## 3604 12/21/1998  0.000000e+00
    ## 3605 12/21/1998  0.000000e+00
    ## 3606 12/21/1998  2.380952e-02
    ## 3607 12/21/1998 -1.190476e-02
    ## 3608 12/21/1998  0.000000e+00
    ## 3609 12/21/1998 -1.666667e-01
    ## 3610 12/22/1998  3.571429e-02
    ## 3611 12/22/1998 -2.380952e-02
    ## 3612 12/22/1998  1.190476e-02
    ## 3613 12/22/1998  4.761905e-02
    ## 3614 12/23/1998  7.142857e-02
    ## 3615 12/23/1998  0.000000e+00
    ## 3616 11/23/1998  4.761905e-02
    ## 3617 12/14/1998  7.142857e-02
    ## 3618 12/14/1998  1.190476e-02
    ## 3619 12/25/1998  2.380952e-02
    ## 3620 12/27/1998  5.952381e-02
    ## 3621 12/25/1998  3.571429e-02
    ## 3622 12/27/1998  2.380952e-02
    ## 3623 12/27/1998  1.190476e-02
    ## 3624 12/31/1998  3.571429e-02
    ## 3625 12/02/1998  4.761905e-02
    ## 3626 12/02/1998  0.000000e+00
    ## 3627 12/09/1998  8.333333e-02
    ## 3628 12/09/1998  0.000000e+00
    ## 3629 12/10/1998  1.190476e-02
    ## 3630 12/11/1998  1.190476e-02
    ## 3631 12/11/1998  1.190476e-02
    ## 3632 12/15/1998  1.190476e-02
    ## 3633 11/24/1998  0.000000e+00
    ## 3634 11/24/1998 -4.404762e-01
    ## 3635 11/24/1998  0.000000e+00
    ## 3636 11/24/1998  3.690476e-01
    ## 3637 11/24/1998  4.761905e-02
    ## 3638 11/25/1998  0.000000e+00
    ## 3639 11/30/1998 -1.071429e-01
    ## 3640 11/30/1998  1.190476e-02
    ## 3641 11/30/1998  8.333333e-02
    ## 3642 12/01/1998 -1.190476e-02
    ## 3643 12/01/1998 -1.785714e-01
    ## 3644 12/01/1998  2.380952e-02
    ## 3645 12/02/1998  2.380952e-02
    ## 3646 12/02/1998  4.761905e-02
    ## 3647 12/03/1998 -9.523810e-02
    ## 3648 12/04/1998  3.571429e-02
    ## 3649 12/04/1998  1.190476e-02
    ## 3650 12/08/1998  1.190476e-02
    ## 3651 12/08/1998  8.333333e-02
    ## 3652 12/10/1998  3.571429e-02
    ## 3653 12/11/1998  0.000000e+00
    ## 3654 12/15/1998 -2.380952e-02
    ## 3655 12/15/1998  0.000000e+00
    ## 3656 12/01/1998 -3.571429e-02
    ## 3657 12/04/1998  1.428571e-01
    ## 3658 12/04/1998  4.761905e-02
    ## 3659 12/04/1998 -1.309524e-01
    ## 3660 12/04/1998  0.000000e+00
    ## 3661 12/08/1998  4.761905e-02
    ## 3662 12/10/1998  0.000000e+00
    ## 3663 12/10/1998  4.761905e-02
    ## 3664 12/10/1998  1.190476e-02
    ## 3665 12/11/1998  1.190476e-02
    ## 3666 12/14/1998  8.333333e-02
    ## 3667 12/14/1998  5.952381e-02
    ## 3668 12/15/1998  7.142857e-02
    ## 3669 12/15/1998  0.000000e+00
    ## 3670 12/15/1998  1.190476e-01
    ## 3671 12/16/1998  1.190476e-02
    ## 3672 12/16/1998  0.000000e+00
    ## 3673 12/16/1998  5.952381e-02
    ## 3674 12/17/1998 -1.190476e-02
    ## 3675 12/22/1998  5.952381e-02
    ## 3676 12/22/1998 -3.333333e-01
    ## 3677 12/22/1998 -4.404762e-01
    ## 3678 11/26/1998  1.190476e-02
    ## 3679 12/10/1998  4.761905e-02
    ## 3680 12/10/1998  7.142857e-02
    ## 3681 12/10/1998  0.000000e+00
    ## 3682 12/14/1998  1.190476e-02
    ## 3683 12/14/1998  9.523810e-02
    ## 3684 12/15/1998 -2.380952e-02
    ## 3685 12/15/1998  0.000000e+00
    ## 3686 12/15/1998  8.333333e-02
    ## 3687 12/15/1998  1.190476e-02
    ## 3688 12/15/1998  1.190476e-02
    ## 3689 12/15/1998  5.952381e-02
    ## 3690 12/15/1998  9.523810e-02
    ## 3691 12/16/1998  5.952381e-02
    ## 3692 12/16/1998  0.000000e+00
    ## 3693 12/17/1998  4.761905e-02
    ## 3694 12/17/1998  8.333333e-02
    ## 3695 12/18/1998  1.190476e-02
    ## 3696 12/22/1998  7.142857e-02
    ## 3697 12/22/1998  7.142857e-02
    ## 3698 12/22/1998  1.190476e-02
    ## 3699 12/23/1998  0.000000e+00
    ## 3700 12/28/1998  1.190476e-02
    ## 3701 12/22/1998  1.190476e-02
    ## 3702 12/23/1998  2.380952e-02
    ## 3703 12/28/1998  8.333333e-02
    ## 3704 12/28/1998  4.761905e-02
    ## 3705 11/23/1998  4.938272e-02
    ## 3706 11/23/1998  7.407407e-02
    ## 3707 11/23/1998 -1.234568e-02
    ## 3708 11/26/1998  1.481481e-01
    ## 3709 11/27/1998  1.234568e-02
    ## 3710 11/27/1998  4.938272e-02
    ## 3711 11/27/1998  2.469136e-02
    ## 3712 11/27/1998  2.469136e-02
    ## 3713 11/27/1998  2.469136e-02
    ## 3714 11/27/1998 -1.234568e-02
    ## 3715 11/27/1998 -3.456790e-01
    ## 3716 12/01/1998  2.469136e-02
    ## 3717 12/01/1998  4.938272e-02
    ## 3718 12/01/1998  0.000000e+00
    ## 3719 12/04/1998  0.000000e+00
    ## 3720 12/04/1998  0.000000e+00
    ## 3721 12/02/1998  2.469136e-02
    ## 3722 12/02/1998  2.469136e-02
    ## 3723 12/02/1998  1.234568e-02
    ## 3724 12/04/1998  0.000000e+00
    ## 3725 12/04/1998  1.234568e-02
    ## 3726 12/04/1998  2.469136e-02
    ## 3727 12/04/1998  2.469136e-02
    ## 3728 12/08/1998  3.703704e-02
    ## 3729 12/10/1998  1.234568e-02
    ## 3730 12/11/1998  3.580247e-01
    ## 3731 12/11/1998  4.938272e-02
    ## 3732 12/17/1998  0.000000e+00
    ## 3733 12/22/1998  1.234568e-02
    ## 3734 12/23/1998  6.172840e-02
    ## 3735 12/23/1998  0.000000e+00
    ## 3736 12/17/1998 -3.456790e-01
    ## 3737 12/17/1998  1.234568e-02
    ## 3738 12/21/1998  0.000000e+00
    ## 3739 12/23/1998  0.000000e+00
    ## 3740 12/29/1998  0.000000e+00
    ## 3741 12/14/1998 -3.456790e-01
    ## 3742 11/23/1998  9.876543e-02
    ## 3743 11/23/1998  4.938272e-02
    ## 3744 12/14/1998  0.000000e+00
    ## 3745 12/27/1998  1.234568e-02
    ## 3746 12/25/1998 -3.456790e-01
    ## 3747 12/01/1998 -3.333333e-01
    ## 3748 12/04/1998  3.703704e-02
    ## 3749 12/09/1998  2.469136e-02
    ## 3750 12/09/1998  3.703704e-02
    ## 3751 12/09/1998  2.469136e-02
    ## 3752 12/09/1998  1.234568e-02
    ## 3753 12/10/1998  1.234568e-02
    ## 3754 12/11/1998 -3.333333e-01
    ## 3755 12/11/1998 -6.172840e-02
    ## 3756 12/15/1998 -1.234568e-02
    ## 3757 12/15/1998  1.234568e-02
    ## 3758 12/15/1998  6.172840e-02
    ## 3759 11/24/1998  1.234568e-02
    ## 3760 11/24/1998  2.469136e-02
    ## 3761 11/24/1998  1.234568e-02
    ## 3762 11/25/1998  0.000000e+00
    ## 3763 12/01/1998  0.000000e+00
    ## 3764 12/01/1998  0.000000e+00
    ## 3765 12/01/1998  0.000000e+00
    ## 3766 12/02/1998  9.876543e-02
    ## 3767 12/03/1998 -7.407407e-02
    ## 3768 12/03/1998 -3.703704e-02
    ## 3769 12/08/1998  1.851852e-01
    ## 3770 12/10/1998 -1.234568e-02
    ## 3771 12/10/1998 -2.469136e-02
    ## 3772 12/10/1998  4.938272e-02
    ## 3773 12/10/1998 -3.456790e-01
    ## 3774 12/11/1998  0.000000e+00
    ## 3775 12/11/1998  0.000000e+00
    ## 3776 12/14/1998 -1.728395e-01
    ## 3777 12/01/1998 -8.641975e-02
    ## 3778 12/01/1998  2.469136e-02
    ## 3779 12/04/1998  0.000000e+00
    ## 3780 12/04/1998  0.000000e+00
    ## 3781 12/04/1998  4.938272e-02
    ## 3782 12/04/1998  6.172840e-02
    ## 3783 12/11/1998  0.000000e+00
    ## 3784 12/14/1998 -2.469136e-02
    ## 3785 12/14/1998  6.172840e-02
    ## 3786 12/14/1998  4.938272e-02
    ## 3787 12/16/1998  1.234568e-02
    ## 3788 12/16/1998  4.938272e-02
    ## 3789 12/16/1998  6.172840e-02
    ## 3790 12/16/1998  7.407407e-02
    ## 3791 12/17/1998  0.000000e+00
    ## 3792 12/19/1998 -8.641975e-02
    ## 3793 12/22/1998 -3.333333e-01
    ## 3794 12/22/1998  3.703704e-02
    ## 3795 12/22/1998  1.234568e-02
    ## 3796 11/26/1998  1.234568e-02
    ## 3797 11/30/1998  1.234568e-02
    ## 3798 12/03/1998  1.234568e-02
    ## 3799 12/03/1998  2.469136e-02
    ## 3800 12/04/1998  1.111111e-01
    ## 3801 11/04/1998  2.469136e-02
    ## 3802 12/04/1998  0.000000e+00
    ## 3803 12/04/1998  0.000000e+00
    ## 3804 12/10/1998  4.938272e-02
    ## 3805 12/10/1998  4.938272e-02
    ## 3806 12/10/1998  3.703704e-02
    ## 3807 12/14/1998  0.000000e+00
    ## 3808 12/14/1998  0.000000e+00
    ## 3809 12/14/1998  3.703704e-02
    ## 3810 12/15/1998  6.172840e-02
    ## 3811 12/15/1998  0.000000e+00
    ## 3812 12/15/1998  2.469136e-02
    ## 3813 12/15/1998  2.469136e-02
    ## 3814 12/16/1998 -3.333333e-01
    ## 3815 12/18/1998  6.172840e-02
    ## 3816 12/18/1998  0.000000e+00
    ## 3817 12/22/1998  2.469136e-02
    ## 3818 12/22/1998  7.407407e-02
    ## 3819 12/23/1998  1.234568e-02
    ## 3820 12/28/1998  0.000000e+00
    ## 3821 12/21/1998  1.234568e-02
    ## 3822 12/21/1998  1.234568e-02
    ## 3823 12/23/1998  1.234568e-02
    ## 3824 11/23/1998  8.974359e-02
    ## 3825 11/23/1998 -1.282051e-02
    ## 3826 11/23/1998  1.666667e-01
    ## 3827 11/24/1998  1.282051e-02
    ## 3828 11/26/1998  0.000000e+00
    ## 3829 11/27/1998  0.000000e+00
    ## 3830 11/27/1998  7.692308e-02
    ## 3831 11/27/1998  6.410256e-02
    ## 3832 11/27/1998 -1.025641e-01
    ## 3833 11/27/1998  8.974359e-02
    ## 3834 12/02/1998  2.564103e-02
    ## 3835 12/03/1998  1.025641e-01
    ## 3836 12/04/1998  1.282051e-02
    ## 3837 12/04/1998  1.282051e-02
    ## 3838 12/08/1998  3.846154e-02
    ## 3839 12/08/1998  0.000000e+00
    ## 3840 12/10/1998 -3.333333e-01
    ## 3841 12/17/1998 -1.282051e-02
    ## 3842 12/17/1998  0.000000e+00
    ## 3843 12/22/1998  6.410256e-02
    ## 3844 12/22/1998  3.846154e-02
    ## 3845 12/23/1998  2.564103e-02
    ## 3846 12/23/1998  8.974359e-02
    ## 3847 12/23/1998  8.974359e-02
    ## 3848 12/23/1998  7.692308e-02
    ## 3849 12/17/1998  0.000000e+00
    ## 3850 12/21/1998  1.282051e-02
    ## 3851 12/21/1998  1.282051e-02
    ## 3852 12/22/1998  2.564103e-02
    ## 3853 12/22/1998 -1.794872e-01
    ## 3854 11/27/1998  1.025641e-01
    ## 3855 12/23/1998  2.564103e-02
    ## 3856 12/23/1998  2.564103e-02
    ## 3857 12/23/1998  5.128205e-02
    ## 3858 11/23/1998  1.153846e-01
    ## 3859 11/23/1998  1.282051e-02
    ## 3860 11/23/1998  1.153846e-01
    ## 3861 11/23/1998 -1.282051e-02
    ## 3862 11/23/1998  5.128205e-02
    ## 3863 11/23/1998  2.564103e-02
    ## 3864 11/23/1998  1.410256e-01
    ## 3865 11/23/1998 -3.461538e-01
    ## 3866 12/14/1998  7.692308e-02
    ## 3867 12/14/1998  2.564103e-02
    ## 3868 12/27/1998  1.282051e-02
    ## 3869 12/27/1998  1.025641e-01
    ## 3870 12/27/1998  1.025641e-01
    ## 3871 12/31/1998 -3.461538e-01
    ## 3872 12/31/1998  5.128205e-02
    ## 3873 12/02/1998  3.846154e-02
    ## 3874 12/02/1998  1.282051e-02
    ## 3875 12/09/1998  1.025641e-01
    ## 3876 12/09/1998  2.564103e-02
    ## 3877 12/04/1998  1.153846e-01
    ## 3878 12/09/1998  1.282051e-02
    ## 3879 12/10/1998  0.000000e+00
    ## 3880 12/10/1998  0.000000e+00
    ## 3881 12/11/1998  2.564103e-02
    ## 3882 12/15/1998 -3.333333e-01
    ## 3883 12/11/1998 -1.282051e-02
    ## 3884 11/24/1998  2.564103e-02
    ## 3885 11/24/1998  2.564103e-02
    ## 3886 11/24/1998  1.282051e-02
    ## 3887 11/25/1998  0.000000e+00
    ## 3888 11/25/1998  2.564103e-02
    ## 3889 11/25/1998  1.282051e-02
    ## 3890 11/30/1998  1.282051e-02
    ## 3891 11/30/1998  0.000000e+00
    ## 3892 12/03/1998 -5.128205e-02
    ## 3893 12/04/1998  1.025641e-01
    ## 3894 12/04/1998  1.410256e-01
    ## 3895 12/04/1998  1.666667e-01
    ## 3896 12/14/1998 -3.461538e-01
    ## 3897 12/14/1998  1.282051e-02
    ## 3898 12/01/1998  2.564103e-02
    ## 3899 12/01/1998  5.128205e-02
    ## 3900 12/02/1998 -3.461538e-01
    ## 3901 12/04/1998  3.846154e-02
    ## 3902 12/04/1998  1.282051e-02
    ## 3903 12/08/1998  0.000000e+00
    ## 3904 12/09/1998  5.128205e-02
    ## 3905 12/09/1998  3.846154e-02
    ## 3906 12/10/1998  1.025641e-01
    ## 3907 12/10/1998  5.128205e-02
    ## 3908 12/10/1998  2.564103e-02
    ## 3909 12/14/1998  7.692308e-02
    ## 3910 12/14/1998  1.282051e-02
    ## 3911 12/15/1998  0.000000e+00
    ## 3912 12/16/1998  0.000000e+00
    ## 3913 12/16/1998  1.153846e-01
    ## 3914 12/17/1998  0.000000e+00
    ## 3915 12/17/1998  0.000000e+00
    ## 3916 12/17/1998  0.000000e+00
    ## 3917 12/19/1998  0.000000e+00
    ## 3918 12/19/1998  0.000000e+00
    ## 3919 11/26/1998  2.564103e-02
    ## 3920 11/26/1998  1.282051e-02
    ## 3921 12/02/1998  0.000000e+00
    ## 3922 12/02/1998  0.000000e+00
    ## 3923 12/03/1998  2.564103e-02
    ## 3924 12/03/1998  2.564103e-02
    ## 3925 12/04/1998  3.846154e-02
    ## 3926 12/10/1998  7.692308e-02
    ## 3927 12/10/1998  7.692308e-02
    ## 3928 12/14/1998  0.000000e+00
    ## 3929 12/15/1998  3.846154e-02
    ## 3930 12/16/1998  2.564103e-02
    ## 3931 12/16/1998  0.000000e+00
    ## 3932 12/17/1998 -3.461538e-01
    ## 3933 12/18/1998  2.564103e-02
    ## 3934 12/18/1998  2.564103e-02
    ## 3935 12/18/1998  1.282051e-02
    ## 3936 12/18/1998 -3.461538e-01
    ## 3937 12/22/1998 -3.461538e-01
    ## 3938 12/18/1998  7.692308e-02
    ## 3939 12/22/1998  1.282051e-02
    ## 3940 12/22/1998  1.282051e-02
    ## 3941 12/28/1998  0.000000e+00
    ## 3942 12/28/1998  0.000000e+00
    ## 3943 12/28/1998  0.000000e+00
    ## 3944 12/21/1998  5.128205e-02
    ## 3945 12/23/1998  3.846154e-02
    ## 3946 12/23/1998  3.846154e-02
    ## 3947 12/23/1998  3.846154e-02
    ## 3948 12/23/1998 -3.333333e-01
    ## 3949 12/28/1998  1.282051e-02
    ## 3950 12/28/1998  5.128205e-02
    ## 3951 12/28/1998  2.564103e-02
    ## 3952 12/18/1998 -7.692308e-02
    ## 3953 11/23/1998  2.666667e-02
    ## 3954 11/23/1998  1.200000e-01
    ## 3955 11/23/1998  5.333333e-02
    ## 3956 11/24/1998  0.000000e+00
    ## 3957 11/24/1998  1.200000e-01
    ## 3958 11/26/1998 -4.533333e-01
    ## 3959 11/26/1998 -1.333333e-02
    ## 3960 11/26/1998  0.000000e+00
    ## 3961 11/26/1998 -2.666667e-02
    ## 3962 11/27/1998  4.000000e-02
    ## 3963 11/27/1998 -1.333333e-02
    ## 3964 11/27/1998  5.333333e-02
    ## 3965 11/27/1998  1.333333e-02
    ## 3966 11/27/1998  1.333333e-02
    ## 3967 11/27/1998  0.000000e+00
    ## 3968 12/01/1998 -4.533333e-01
    ## 3969 12/01/1998  2.666667e-02
    ## 3970 12/01/1998  6.666667e-02
    ## 3971 12/01/1998 -1.333333e-02
    ## 3972 12/04/1998  1.333333e-02
    ## 3973 12/04/1998  2.666667e-02
    ## 3974 12/02/1998  2.666667e-02
    ## 3975 12/02/1998  2.666667e-02
    ## 3976 12/02/1998  2.666667e-02
    ## 3977 12/02/1998 -3.466667e-01
    ## 3978 12/02/1998  2.666667e-02
    ## 3979 12/03/1998  4.000000e-02
    ## 3980 12/03/1998  1.333333e-02
    ## 3981 12/03/1998  1.333333e-02
    ## 3982 12/04/1998  2.666667e-02
    ## 3983 12/04/1998  5.333333e-02
    ## 3984 12/04/1998  0.000000e+00
    ## 3985 12/04/1998  0.000000e+00
    ## 3986 12/08/1998  9.333333e-02
    ## 3987 12/08/1998  1.333333e-02
    ## 3988 12/10/1998  1.333333e-02
    ## 3989 12/10/1998 -1.333333e-02
    ## 3990 12/10/1998  1.333333e-02
    ## 3991 12/10/1998  1.333333e-02
    ## 3992 12/10/1998  0.000000e+00
    ## 3993 12/11/1998  5.333333e-02
    ## 3994 12/08/1998 -2.666667e-02
    ## 3995 12/22/1998  1.333333e-02
    ## 3996 12/22/1998 -3.466667e-01
    ## 3997 12/22/1998  6.666667e-02
    ## 3998 12/23/1998 -1.333333e-02
    ## 3999 12/23/1998  5.333333e-02
    ## 4000 12/23/1998  1.333333e-02
    ## 4001 12/23/1998  4.000000e-02
    ## 4002 12/17/1998  1.333333e-02
    ## 4003 12/21/1998  2.666667e-02
    ## 4004 12/21/1998  2.666667e-02
    ## 4005 12/22/1998  2.666667e-02
    ## 4006 12/22/1998  2.666667e-02
    ## 4007 12/23/1998  0.000000e+00
    ## 4008 12/23/1998  5.333333e-02
    ## 4009 11/23/1998  2.666667e-02
    ## 4010 12/14/1998  9.333333e-02
    ## 4011 12/14/1998  2.666667e-02
    ## 4012 12/25/1998  2.133333e-01
    ## 4013 12/25/1998  1.333333e-02
    ## 4014 12/27/1998  5.333333e-02
    ## 4015 12/31/1998  6.666667e-02
    ## 4016 12/01/1998  4.000000e-02
    ## 4017 12/01/1998  8.000000e-02
    ## 4018 12/02/1998  6.666667e-02
    ## 4019 12/02/1998  4.000000e-02
    ## 4020 12/11/1998 -1.733333e-01
    ## 4021 12/02/1998  4.000000e-02
    ## 4022 12/09/1998  0.000000e+00
    ## 4023 12/04/1998  8.000000e-02
    ## 4024 12/04/1998  4.000000e-02
    ## 4025 12/04/1998 -3.466667e-01
    ## 4026 12/04/1998  1.333333e-02
    ## 4027 12/04/1998  2.266667e-01
    ## 4028 12/10/1998  0.000000e+00
    ## 4029 12/15/1998  4.000000e-02
    ## 4030 12/15/1998 -3.466667e-01
    ## 4031 12/15/1998  0.000000e+00
    ## 4032 12/15/1998  5.333333e-02
    ## 4033 11/24/1998  0.000000e+00
    ## 4034 11/24/1998  5.333333e-02
    ## 4035 11/24/1998  4.000000e-02
    ## 4036 11/24/1998  1.466667e-01
    ## 4037 11/24/1998  0.000000e+00
    ## 4038 11/24/1998  2.666667e-02
    ## 4039 11/25/1998  2.666667e-02
    ## 4040 11/25/1998 -2.666667e-02
    ## 4041 11/25/1998 -1.333333e-02
    ## 4042 11/25/1998  1.333333e-02
    ## 4043 11/25/1998  5.333333e-02
    ## 4044 11/25/1998  8.000000e-02
    ## 4045 11/30/1998  4.000000e-02
    ## 4046 11/30/1998  1.333333e-02
    ## 4047 12/01/1998  2.666667e-02
    ## 4048 12/01/1998  1.333333e-02
    ## 4049 12/03/1998  1.333333e-01
    ## 4050 12/04/1998  2.133333e-01
    ## 4051 12/04/1998  2.400000e-01
    ## 4052 12/08/1998  2.666667e-02
    ## 4053 12/08/1998  0.000000e+00
    ## 4054 12/08/1998  9.333333e-02
    ## 4055 12/08/1998  2.666667e-02
    ## 4056 12/10/1998  4.000000e-02
    ## 4057 12/01/1998 -1.333333e-02
    ## 4058 12/01/1998  0.000000e+00
    ## 4059 12/02/1998  4.000000e-02
    ## 4060 12/04/1998  1.333333e-02
    ## 4061 12/08/1998  2.666667e-02
    ## 4062 12/08/1998 -2.666667e-02
    ## 4063 12/09/1998  1.333333e-02
    ## 4064 12/09/1998  0.000000e+00
    ## 4065 12/10/1998  1.333333e-02
    ## 4066 12/10/1998  0.000000e+00
    ## 4067 12/11/1998  0.000000e+00
    ## 4068 12/11/1998 -1.466667e-01
    ## 4069 12/11/1998  0.000000e+00
    ## 4070 12/11/1998  2.666667e-02
    ## 4071 12/14/1998  8.000000e-02
    ## 4072 12/14/1998  1.066667e-01
    ## 4073 12/14/1998  1.733333e-01
    ## 4074 12/15/1998  4.000000e-02
    ## 4075 12/15/1998  2.666667e-02
    ## 4076 12/15/1998  4.000000e-02
    ## 4077 12/16/1998  1.333333e-02
    ## 4078 12/16/1998  9.333333e-02
    ## 4079 12/16/1998  2.666667e-02
    ## 4080 12/16/1998  1.333333e-02
    ## 4081 12/17/1998  8.000000e-02
    ## 4082 12/22/1998 -3.466667e-01
    ## 4083 12/23/1998 -1.733333e-01
    ## 4084 11/30/1998  2.666667e-02
    ## 4085 12/04/1998  4.000000e-02
    ## 4086 11/04/1998  1.333333e-02
    ## 4087 12/10/1998  0.000000e+00
    ## 4088 12/10/1998 -4.000000e-02
    ## 4089 12/10/1998  1.333333e-02
    ## 4090 12/14/1998  5.333333e-02
    ## 4091 12/14/1998  6.666667e-02
    ## 4092 12/15/1998  4.000000e-02
    ## 4093 12/15/1998  1.333333e-02
    ## 4094 12/15/1998  1.333333e-02
    ## 4095 12/15/1998  1.333333e-02
    ## 4096 12/15/1998  0.000000e+00
    ## 4097 12/15/1998 -3.466667e-01
    ## 4098 12/16/1998  4.000000e-02
    ## 4099 12/17/1998  1.333333e-02
    ## 4100 12/17/1998  1.333333e-02
    ## 4101 12/17/1998 -3.333333e-01
    ## 4102 12/17/1998  8.000000e-02
    ## 4103 12/17/1998  1.333333e-02
    ## 4104 12/17/1998 -3.333333e-01
    ## 4105 12/18/1998  6.666667e-02
    ## 4106 12/18/1998  0.000000e+00
    ## 4107 12/18/1998  4.000000e-02
    ## 4108 12/18/1998  5.333333e-02
    ## 4109 12/23/1998  0.000000e+00
    ## 4110 12/23/1998  8.000000e-02
    ## 4111 12/23/1998  1.333333e-02
    ## 4112 12/23/1998  0.000000e+00
    ## 4113 12/23/1998  5.333333e-02
    ## 4114 12/28/1998  0.000000e+00
    ## 4115 12/28/1998  2.666667e-02
    ## 4116 12/28/1998  4.000000e-02
    ## 4117 12/17/1998  2.666667e-02
    ## 4118 12/22/1998  0.000000e+00
    ## 4119 12/22/1998  0.000000e+00
    ## 4120 12/23/1998  6.666667e-02
    ## 4121 12/23/1998  1.333333e-02
    ## 4122 12/28/1998  0.000000e+00
    ## 4123 11/23/1998  9.722222e-02
    ## 4124 11/23/1998  1.944444e-01
    ## 4125 11/24/1998  2.777778e-02
    ## 4126 11/26/1998  2.083333e-01
    ## 4127 11/26/1998  1.388889e-02
    ## 4128 11/26/1998  1.111111e-01
    ## 4129 11/26/1998  1.388889e-02
    ## 4130 11/26/1998  4.166667e-02
    ## 4131 11/27/1998  5.555556e-02
    ## 4132 11/27/1998  8.333333e-02
    ## 4133 11/27/1998  1.666667e-01
    ## 4134 11/27/1998  1.388889e-02
    ## 4135 11/27/1998  4.166667e-02
    ## 4136 11/27/1998  5.555556e-02
    ## 4137 11/27/1998  1.388889e-02
    ## 4138 11/27/1998  1.388889e-02
    ## 4139 11/27/1998  1.388889e-02
    ## 4140 11/27/1998  0.000000e+00
    ## 4141 12/01/1998  1.527778e-01
    ## 4142 12/01/1998  0.000000e+00
    ## 4143 12/01/1998  0.000000e+00
    ## 4144 12/04/1998  8.333333e-02
    ## 4145 12/04/1998 -3.472222e-01
    ## 4146 12/02/1998  6.944444e-02
    ## 4147 12/02/1998  5.555556e-02
    ## 4148 12/02/1998  1.250000e-01
    ## 4149 12/02/1998 -1.388889e-02
    ## 4150 12/02/1998  2.777778e-02
    ## 4151 12/02/1998  1.388889e-02
    ## 4152 12/02/1998  1.388889e-02
    ## 4153 12/03/1998  0.000000e+00
    ## 4154 12/04/1998  0.000000e+00
    ## 4155 12/04/1998  1.388889e-02
    ## 4156 12/04/1998  1.388889e-02
    ## 4157 12/04/1998  6.944444e-02
    ## 4158 12/08/1998  0.000000e+00
    ## 4159 12/10/1998  2.777778e-02
    ## 4160 12/10/1998  2.777778e-02
    ## 4161 12/10/1998  0.000000e+00
    ## 4162 12/10/1998  2.777778e-02
    ## 4163 12/10/1998  2.777778e-02
    ## 4164 12/11/1998 -1.388889e-02
    ## 4165 12/17/1998  2.777778e-02
    ## 4166 12/17/1998  2.777778e-02
    ## 4167 12/17/1998  0.000000e+00
    ## 4168 12/22/1998  0.000000e+00
    ## 4169 12/22/1998 -1.388889e-02
    ## 4170 12/22/1998  5.555556e-02
    ## 4171 12/23/1998  2.777778e-02
    ## 4172 12/23/1998  2.083333e-01
    ## 4173 12/23/1998  2.500000e-01
    ## 4174 12/23/1998  1.388889e-02
    ## 4175 12/23/1998  5.555556e-02
    ## 4176 12/17/1998  0.000000e+00
    ## 4177 12/17/1998  1.388889e-02
    ## 4178 12/21/1998  2.777778e-02
    ## 4179 12/22/1998  1.388889e-02
    ## 4180 12/22/1998 -8.333333e-02
    ## 4181 12/23/1998  1.388889e-02
    ## 4182 12/29/1998  0.000000e+00
    ## 4183 12/29/1998  0.000000e+00
    ## 4184 12/29/1998 -4.166667e-02
    ## 4185 11/23/1998  1.388889e-02
    ## 4186 11/23/1998  1.388889e-02
    ## 4187 12/14/1998  0.000000e+00
    ## 4188 12/14/1998  4.166667e-02
    ## 4189 12/25/1998 -3.333333e-01
    ## 4190 12/25/1998  1.388889e-02
    ## 4191 12/27/1998  4.166667e-02
    ## 4192 12/27/1998  0.000000e+00
    ## 4193 12/27/1998  8.333333e-02
    ## 4194 12/27/1998  1.805556e-01
    ## 4195 12/27/1998  6.944444e-02
    ## 4196 12/01/1998  0.000000e+00
    ## 4197 12/02/1998  0.000000e+00
    ## 4198 12/09/1998  4.166667e-02
    ## 4199 12/09/1998 -3.472222e-01
    ## 4200 12/04/1998  1.388889e-02
    ## 4201 12/04/1998 -8.333333e-02
    ## 4202 12/04/1998  0.000000e+00
    ## 4203 12/04/1998  2.777778e-02
    ## 4204 12/09/1998  2.777778e-02
    ## 4205 12/09/1998  4.166667e-02
    ## 4206 12/10/1998  4.166667e-02
    ## 4207 12/11/1998  4.166667e-02
    ## 4208 12/11/1998  0.000000e+00
    ## 4209 12/11/1998  2.777778e-02
    ## 4210 12/15/1998  1.388889e-02
    ## 4211 12/15/1998  1.388889e-02
    ## 4212 12/15/1998  2.777778e-02
    ## 4213 11/24/1998  4.166667e-02
    ## 4214 11/24/1998  4.166667e-02
    ## 4215 11/24/1998  8.333333e-02
    ## 4216 11/24/1998 -3.333333e-01
    ## 4217 11/24/1998  4.166667e-02
    ## 4218 11/24/1998  6.944444e-02
    ## 4219 11/24/1998  0.000000e+00
    ## 4220 11/24/1998  6.944444e-02
    ## 4221 11/24/1998  0.000000e+00
    ## 4222 11/24/1998  6.944444e-02
    ## 4223 11/25/1998 -2.777778e-02
    ## 4224 11/25/1998  4.166667e-02
    ## 4225 12/08/1998  0.000000e+00
    ## 4226 12/01/1998  2.083333e-01
    ## 4227 12/01/1998  6.944444e-02
    ## 4228 12/01/1998 -1.388889e-02
    ## 4229 12/02/1998  4.166667e-02
    ## 4230 12/04/1998  2.777778e-02
    ## 4231 12/10/1998  0.000000e+00
    ## 4232 12/10/1998  0.000000e+00
    ## 4233 12/11/1998  1.388889e-02
    ## 4234 12/14/1998  0.000000e+00
    ## 4235 12/14/1998  1.388889e-02
    ## 4236 12/15/1998  0.000000e+00
    ## 4237 12/01/1998  5.555556e-02
    ## 4238 12/02/1998  2.777778e-02
    ## 4239 12/02/1998  2.777778e-02
    ## 4240 12/04/1998  1.388889e-02
    ## 4241 12/08/1998  8.333333e-02
    ## 4242 12/11/1998 -1.805556e-01
    ## 4243 12/14/1998 -1.111111e-01
    ## 4244 12/14/1998  5.555556e-02
    ## 4245 12/14/1998  0.000000e+00
    ## 4246 12/15/1998  4.166667e-02
    ## 4247 12/15/1998  2.777778e-02
    ## 4248 12/15/1998  4.166667e-02
    ## 4249 12/15/1998  1.388889e-02
    ## 4250 12/16/1998  8.333333e-02
    ## 4251 12/16/1998 -3.472222e-01
    ## 4252 12/16/1998  1.388889e-02
    ## 4253 12/16/1998  0.000000e+00
    ## 4254 12/16/1998  1.250000e-01
    ## 4255 12/17/1998  0.000000e+00
    ## 4256 12/19/1998  1.388889e-02
    ## 4257 11/26/1998  5.555556e-02
    ## 4258 12/02/1998  0.000000e+00
    ## 4259 12/02/1998  0.000000e+00
    ## 4260 12/03/1998 -2.777778e-02
    ## 4261 12/03/1998  1.388889e-02
    ## 4262 12/04/1998  1.388889e-02
    ## 4263 12/04/1998  4.166667e-02
    ## 4264 12/04/1998  2.777778e-02
    ## 4265 12/10/1998  4.166667e-02
    ## 4266 12/10/1998  1.805556e-01
    ## 4267 12/10/1998  6.944444e-02
    ## 4268 12/14/1998  0.000000e+00
    ## 4269 12/15/1998  2.777778e-02
    ## 4270 12/15/1998  6.944444e-02
    ## 4271 12/15/1998  4.166667e-02
    ## 4272 12/15/1998  1.388889e-02
    ## 4273 12/15/1998  0.000000e+00
    ## 4274 12/15/1998  1.388889e-02
    ## 4275 12/15/1998  1.388889e-02
    ## 4276 12/15/1998  5.555556e-02
    ## 4277 12/16/1998 -3.333333e-01
    ## 4278 12/17/1998  2.777778e-02
    ## 4279 12/17/1998  2.777778e-02
    ## 4280 12/17/1998  4.166667e-02
    ## 4281 12/18/1998  0.000000e+00
    ## 4282 12/18/1998  1.388889e-02
    ## 4283 12/18/1998  0.000000e+00
    ## 4284 12/18/1998  2.777778e-02
    ## 4285 12/18/1998 -3.333333e-01
    ## 4286 12/18/1998 -4.583333e-01
    ## 4287 12/22/1998  1.388889e-02
    ## 4288 12/22/1998  0.000000e+00
    ## 4289 12/28/1998  1.388889e-02
    ## 4290 12/28/1998  2.777778e-02
    ## 4291 12/21/1998 -3.472222e-01
    ## 4292 12/21/1998  0.000000e+00
    ## 4293 12/21/1998  1.388889e-02
    ## 4294 12/22/1998  4.166667e-02
    ## 4295 12/22/1998  0.000000e+00
    ## 4296 12/23/1998  0.000000e+00
    ## 4297 12/23/1998  0.000000e+00
    ## 4298 12/23/1998  5.555556e-02
    ## 4299 12/17/1998 -1.944444e-01
    ## 4300 12/22/1998 -3.333333e-01
    ## 4301 11/23/1998  1.304348e-01
    ## 4302 11/24/1998  1.449275e-01
    ## 4303 11/24/1998  1.449275e-02
    ## 4304 11/24/1998  4.347826e-02
    ## 4305 11/24/1998  5.797101e-02
    ## 4306 11/26/1998  7.246377e-02
    ## 4307 11/26/1998  2.898551e-02
    ## 4308 11/27/1998  7.246377e-02
    ## 4309 11/27/1998  5.797101e-02
    ## 4310 11/27/1998  1.304348e-01
    ## 4311 11/27/1998  8.695652e-02
    ## 4312 11/27/1998  1.159420e-01
    ## 4313 11/27/1998  1.449275e-02
    ## 4314 11/27/1998  0.000000e+00
    ## 4315 11/27/1998 -3.478261e-01
    ## 4316 11/27/1998  5.797101e-02
    ## 4317 12/01/1998 -1.449275e-02
    ## 4318 12/01/1998  2.898551e-02
    ## 4319 12/01/1998  0.000000e+00
    ## 4320 12/01/1998 -2.898551e-02
    ## 4321 12/04/1998 -3.478261e-01
    ## 4322 12/02/1998 -1.449275e-02
    ## 4323 11/23/1998  2.173913e-01
    ## 4324 12/03/1998  0.000000e+00
    ## 4325 12/04/1998  5.797101e-02
    ## 4326 12/08/1998  8.695652e-02
    ## 4327 12/10/1998  5.797101e-02
    ## 4328 12/11/1998  4.347826e-02
    ## 4329 12/11/1998  7.246377e-02
    ## 4330 12/11/1998  0.000000e+00
    ## 4331 12/17/1998  4.347826e-02
    ## 4332 12/22/1998  1.449275e-02
    ## 4333 12/23/1998  1.014493e-01
    ## 4334 12/21/1998 -3.333333e-01
    ## 4335 12/21/1998  0.000000e+00
    ## 4336 12/22/1998  2.898551e-02
    ## 4337 12/22/1998 -1.449275e-02
    ## 4338 12/23/1998  1.449275e-02
    ## 4339 12/29/1998  0.000000e+00
    ## 4340 12/29/1998  0.000000e+00
    ## 4341 12/29/1998  2.898551e-02
    ## 4342 11/23/1998  2.898551e-02
    ## 4343 11/23/1998  1.159420e-01
    ## 4344 12/14/1998 -3.333333e-01
    ## 4345 12/14/1998  1.449275e-02
    ## 4346 12/14/1998  1.449275e-02
    ## 4347 12/14/1998  0.000000e+00
    ##  [ reached getOption("max.print") -- omitted 5119 rows ]

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
