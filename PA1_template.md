---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---

## Loading and preprocessing the data
  1. Load the data

```r
    fitdata <- read.csv("./activity.csv")
```

  2. Process/transform the data into a format suitable for analysis
  
  Here I will convert the class of the 'date' column from factor to character. This will make it easier to work with.

```r
    fitdata$date <- as.character(fitdata$date)
```


## What is mean total number of steps taken per day?
  1. Calculate the total number of steps taken per day

```r
    as.data.frame.table(tapply(fitdata$steps, fitdata$date, sum, na.rm=TRUE))
```

```
##          Var1  Freq
## 1  2012-10-01     0
## 2  2012-10-02   126
## 3  2012-10-03 11352
## 4  2012-10-04 12116
## 5  2012-10-05 13294
## 6  2012-10-06 15420
## 7  2012-10-07 11015
## 8  2012-10-08     0
## 9  2012-10-09 12811
## 10 2012-10-10  9900
## 11 2012-10-11 10304
## 12 2012-10-12 17382
## 13 2012-10-13 12426
## 14 2012-10-14 15098
## 15 2012-10-15 10139
## 16 2012-10-16 15084
## 17 2012-10-17 13452
## 18 2012-10-18 10056
## 19 2012-10-19 11829
## 20 2012-10-20 10395
## 21 2012-10-21  8821
## 22 2012-10-22 13460
## 23 2012-10-23  8918
## 24 2012-10-24  8355
## 25 2012-10-25  2492
## 26 2012-10-26  6778
## 27 2012-10-27 10119
## 28 2012-10-28 11458
## 29 2012-10-29  5018
## 30 2012-10-30  9819
## 31 2012-10-31 15414
## 32 2012-11-01     0
## 33 2012-11-02 10600
## 34 2012-11-03 10571
## 35 2012-11-04     0
## 36 2012-11-05 10439
## 37 2012-11-06  8334
## 38 2012-11-07 12883
## 39 2012-11-08  3219
## 40 2012-11-09     0
## 41 2012-11-10     0
## 42 2012-11-11 12608
## 43 2012-11-12 10765
## 44 2012-11-13  7336
## 45 2012-11-14     0
## 46 2012-11-15    41
## 47 2012-11-16  5441
## 48 2012-11-17 14339
## 49 2012-11-18 15110
## 50 2012-11-19  8841
## 51 2012-11-20  4472
## 52 2012-11-21 12787
## 53 2012-11-22 20427
## 54 2012-11-23 21194
## 55 2012-11-24 14478
## 56 2012-11-25 11834
## 57 2012-11-26 11162
## 58 2012-11-27 13646
## 59 2012-11-28 10183
## 60 2012-11-29  7047
## 61 2012-11-30     0
```

  
  2. Make a histogram of the total number of steps taken each day

```r
    steps_date <- tapply(fitdata$steps, fitdata$date, sum, na.rm=TRUE)
    steps_date <- as.data.frame.table(steps_date)
    names(steps_date) <- c("Date", "Steps")
    
    library(ggplot2)
    ggp <- ggplot(steps_date, aes(factor(Date), Steps)) +
        geom_bar(stat="identity") +
        theme_bw() + guides(fill=FALSE) +
        theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) +
        labs(y="Number of Steps", title="Total number of steps taken each day")
    
    print(ggp)
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

  3. Calculate and report the mean and median of the total number of steps taken per day

```r
    fitdata2 <- read.csv("./activity.csv", colClasses = c("numeric", "character", "numeric"), na.strings=c("NA", "0"))
    ## Next I want to replace the NA that is taking up the 0 entry in the interval column. I had to change the behavior of the dataframe to treat 0's as NA because if I didn't the median of all dates would = 0, and the instructions say to ignore missing values.
    mean_steps <- as.data.frame.table(tapply(fitdata2$steps, fitdata2$date, mean, na.rm=TRUE))
    median_steps <- as.data.frame.table(tapply(fitdata2$steps, fitdata2$date, median, na.rm=TRUE))
    
    print(mean_steps)
```

```
##          Var1      Freq
## 1  2012-10-01       NaN
## 2  2012-10-02  63.00000
## 3  2012-10-03 140.14815
## 4  2012-10-04 121.16000
## 5  2012-10-05 154.58140
## 6  2012-10-06 145.47170
## 7  2012-10-07 101.99074
## 8  2012-10-08       NaN
## 9  2012-10-09 134.85263
## 10 2012-10-10  95.19231
## 11 2012-10-11 137.38667
## 12 2012-10-12 156.59459
## 13 2012-10-13 119.48077
## 14 2012-10-14 160.61702
## 15 2012-10-15 131.67532
## 16 2012-10-16 157.12500
## 17 2012-10-17 152.86364
## 18 2012-10-18 152.36364
## 19 2012-10-19 127.19355
## 20 2012-10-20 125.24096
## 21 2012-10-21  96.93407
## 22 2012-10-22 154.71264
## 23 2012-10-23 101.34091
## 24 2012-10-24 104.43750
## 25 2012-10-25  56.63636
## 26 2012-10-26  77.02273
## 27 2012-10-27 134.92000
## 28 2012-10-28 110.17308
## 29 2012-10-29  80.93548
## 30 2012-10-30 110.32584
## 31 2012-10-31 179.23256
## 32 2012-11-01       NaN
## 33 2012-11-02 143.24324
## 34 2012-11-03 117.45556
## 35 2012-11-04       NaN
## 36 2012-11-05 141.06757
## 37 2012-11-06 100.40964
## 38 2012-11-07 135.61053
## 39 2012-11-08  61.90385
## 40 2012-11-09       NaN
## 41 2012-11-10       NaN
## 42 2012-11-11 132.71579
## 43 2012-11-12 156.01449
## 44 2012-11-13  90.56790
## 45 2012-11-14       NaN
## 46 2012-11-15  20.50000
## 47 2012-11-16  89.19672
## 48 2012-11-17 183.83333
## 49 2012-11-18 162.47312
## 50 2012-11-19 117.88000
## 51 2012-11-20  95.14894
## 52 2012-11-21 188.04412
## 53 2012-11-22 177.62609
## 54 2012-11-23 252.30952
## 55 2012-11-24 176.56098
## 56 2012-11-25 140.88095
## 57 2012-11-26 128.29885
## 58 2012-11-27 158.67442
## 59 2012-11-28 212.14583
## 60 2012-11-29 110.10938
## 61 2012-11-30       NaN
```

```r
    print(median_steps)
```

```
##          Var1  Freq
## 1  2012-10-01    NA
## 2  2012-10-02  63.0
## 3  2012-10-03  61.0
## 4  2012-10-04  56.5
## 5  2012-10-05  66.0
## 6  2012-10-06  67.0
## 7  2012-10-07  52.5
## 8  2012-10-08    NA
## 9  2012-10-09  48.0
## 10 2012-10-10  56.5
## 11 2012-10-11  35.0
## 12 2012-10-12  46.0
## 13 2012-10-13  45.5
## 14 2012-10-14  60.5
## 15 2012-10-15  54.0
## 16 2012-10-16  64.0
## 17 2012-10-17  61.5
## 18 2012-10-18  52.5
## 19 2012-10-19  74.0
## 20 2012-10-20  49.0
## 21 2012-10-21  48.0
## 22 2012-10-22  52.0
## 23 2012-10-23  56.0
## 24 2012-10-24  51.5
## 25 2012-10-25  35.0
## 26 2012-10-26  36.5
## 27 2012-10-27  72.0
## 28 2012-10-28  61.0
## 29 2012-10-29  54.5
## 30 2012-10-30  40.0
## 31 2012-10-31  83.5
## 32 2012-11-01    NA
## 33 2012-11-02  55.5
## 34 2012-11-03  59.0
## 35 2012-11-04    NA
## 36 2012-11-05  66.0
## 37 2012-11-06  52.0
## 38 2012-11-07  58.0
## 39 2012-11-08  42.5
## 40 2012-11-09    NA
## 41 2012-11-10    NA
## 42 2012-11-11  55.0
## 43 2012-11-12  42.0
## 44 2012-11-13  57.0
## 45 2012-11-14    NA
## 46 2012-11-15  20.5
## 47 2012-11-16  43.0
## 48 2012-11-17  65.5
## 49 2012-11-18  80.0
## 50 2012-11-19  34.0
## 51 2012-11-20  58.0
## 52 2012-11-21  55.0
## 53 2012-11-22  65.0
## 54 2012-11-23 113.0
## 55 2012-11-24  65.5
## 56 2012-11-25  84.0
## 57 2012-11-26  53.0
## 58 2012-11-27  57.0
## 59 2012-11-28  70.0
## 60 2012-11-29  44.5
## 61 2012-11-30    NA
```

## What is the average daily activity pattern?

  1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
  

```r
    ## The 0th interval was replaced with an NA value by the last read.csv call, so here I change those row's intervals back to 0
    fitdata2$interval[is.na(fitdata2$interval)] <- 0
    steps_interval <- tapply(fitdata2$steps, fitdata2$interval, mean, na.rm=TRUE)
    steps_interval <- as.data.frame.table(steps_interval)
    names(steps_interval) <- c("Interval", "Steps")
    
    steps_interval$Interval[is.na(steps_interval$Interval)] <- 0
    steps_interval$Steps[is.na(steps_interval$Steps)] <- 0
    
    with(steps_interval, plot(Interval, Steps, type="l", main="Average Number of Steps Taken Each 5-minute Interval Across All Days", xlab="5-minute interval within day", ylab="Average Number of Steps"))
    with(steps_interval, lines(stats::lowess(steps_interval)))
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
  
  2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?
 

```r
    max_index <- which.max(steps_interval$Steps)

    steps_interval$Interval[max_index]
```

```
## [1] 835
## 288 Levels: 0 5 10 15 20 25 30 35 40 45 50 55 100 105 110 115 120 125 ... 2355
```

## Imputing missing values
  1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
    fitdata3 <- read.csv("./activity.csv")
    sum(is.na(fitdata3))
```

```
## [1] 2304
```

  2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.


    Replace each NA value with the mean for that 5-minute interval

  
  3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
    fitdata_imputed <- fitdata3
    for (i in 1:nrow(fitdata_imputed)) {
      if (is.na(fitdata_imputed$steps[i])) {
        interval_value <- fitdata_imputed$interval[i]
        steps_value <- steps_interval[steps_interval$Interval == interval_value,]
        fitdata_imputed$steps[i] <- steps_value$Steps
      }
    }
```
  
  4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
    ## Histogram
    steps_date_complete <- tapply(fitdata_imputed$steps, fitdata_imputed$date, sum, na.rm=TRUE)
    steps_date_complete <- as.data.frame.table(steps_date_complete)
    names(steps_date_complete) <- c("Date", "Steps")
    
    library(ggplot2)
    ggp <- ggplot(steps_date_complete, aes(factor(Date), Steps)) +
        geom_bar(stat="identity") +
        theme_bw() + guides(fill=FALSE) +
        theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) +
        labs(y="Number of Steps", title="Total number of steps taken each day")
    
    print(ggp)
```

![](PA1_template_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
    ## Mean/Median
    mean_steps <- as.data.frame.table(tapply(fitdata_imputed$steps, fitdata_imputed$date, mean, na.rm=TRUE))
    median_steps <- as.data.frame.table(tapply(fitdata_imputed$steps, fitdata_imputed$date, median, na.rm=TRUE))
    
    print(mean_steps)
```

```
##          Var1        Freq
## 1  2012-10-01 100.8297664
## 2  2012-10-02   0.4375000
## 3  2012-10-03  39.4166667
## 4  2012-10-04  42.0694444
## 5  2012-10-05  46.1597222
## 6  2012-10-06  53.5416667
## 7  2012-10-07  38.2465278
## 8  2012-10-08 100.8297664
## 9  2012-10-09  44.4826389
## 10 2012-10-10  34.3750000
## 11 2012-10-11  35.7777778
## 12 2012-10-12  60.3541667
## 13 2012-10-13  43.1458333
## 14 2012-10-14  52.4236111
## 15 2012-10-15  35.2048611
## 16 2012-10-16  52.3750000
## 17 2012-10-17  46.7083333
## 18 2012-10-18  34.9166667
## 19 2012-10-19  41.0729167
## 20 2012-10-20  36.0937500
## 21 2012-10-21  30.6284722
## 22 2012-10-22  46.7361111
## 23 2012-10-23  30.9652778
## 24 2012-10-24  29.0104167
## 25 2012-10-25   8.6527778
## 26 2012-10-26  23.5347222
## 27 2012-10-27  35.1354167
## 28 2012-10-28  39.7847222
## 29 2012-10-29  17.4236111
## 30 2012-10-30  34.0937500
## 31 2012-10-31  53.5208333
## 32 2012-11-01 100.8297664
## 33 2012-11-02  36.8055556
## 34 2012-11-03  36.7048611
## 35 2012-11-04 100.8297664
## 36 2012-11-05  36.2465278
## 37 2012-11-06  28.9375000
## 38 2012-11-07  44.7326389
## 39 2012-11-08  11.1770833
## 40 2012-11-09 100.8297664
## 41 2012-11-10 100.8297664
## 42 2012-11-11  43.7777778
## 43 2012-11-12  37.3784722
## 44 2012-11-13  25.4722222
## 45 2012-11-14 100.8297664
## 46 2012-11-15   0.1423611
## 47 2012-11-16  18.8923611
## 48 2012-11-17  49.7881944
## 49 2012-11-18  52.4652778
## 50 2012-11-19  30.6979167
## 51 2012-11-20  15.5277778
## 52 2012-11-21  44.3993056
## 53 2012-11-22  70.9270833
## 54 2012-11-23  73.5902778
## 55 2012-11-24  50.2708333
## 56 2012-11-25  41.0902778
## 57 2012-11-26  38.7569444
## 58 2012-11-27  47.3819444
## 59 2012-11-28  35.3576389
## 60 2012-11-29  24.4687500
## 61 2012-11-30 100.8297664
```

```r
    print(median_steps)
```

```
##          Var1     Freq
## 1  2012-10-01 98.51339
## 2  2012-10-02  0.00000
## 3  2012-10-03  0.00000
## 4  2012-10-04  0.00000
## 5  2012-10-05  0.00000
## 6  2012-10-06  0.00000
## 7  2012-10-07  0.00000
## 8  2012-10-08 98.51339
## 9  2012-10-09  0.00000
## 10 2012-10-10  0.00000
## 11 2012-10-11  0.00000
## 12 2012-10-12  0.00000
## 13 2012-10-13  0.00000
## 14 2012-10-14  0.00000
## 15 2012-10-15  0.00000
## 16 2012-10-16  0.00000
## 17 2012-10-17  0.00000
## 18 2012-10-18  0.00000
## 19 2012-10-19  0.00000
## 20 2012-10-20  0.00000
## 21 2012-10-21  0.00000
## 22 2012-10-22  0.00000
## 23 2012-10-23  0.00000
## 24 2012-10-24  0.00000
## 25 2012-10-25  0.00000
## 26 2012-10-26  0.00000
## 27 2012-10-27  0.00000
## 28 2012-10-28  0.00000
## 29 2012-10-29  0.00000
## 30 2012-10-30  0.00000
## 31 2012-10-31  0.00000
## 32 2012-11-01 98.51339
## 33 2012-11-02  0.00000
## 34 2012-11-03  0.00000
## 35 2012-11-04 98.51339
## 36 2012-11-05  0.00000
## 37 2012-11-06  0.00000
## 38 2012-11-07  0.00000
## 39 2012-11-08  0.00000
## 40 2012-11-09 98.51339
## 41 2012-11-10 98.51339
## 42 2012-11-11  0.00000
## 43 2012-11-12  0.00000
## 44 2012-11-13  0.00000
## 45 2012-11-14 98.51339
## 46 2012-11-15  0.00000
## 47 2012-11-16  0.00000
## 48 2012-11-17  0.00000
## 49 2012-11-18  0.00000
## 50 2012-11-19  0.00000
## 51 2012-11-20  0.00000
## 52 2012-11-21  0.00000
## 53 2012-11-22  0.00000
## 54 2012-11-23  0.00000
## 55 2012-11-24  0.00000
## 56 2012-11-25  0.00000
## 57 2012-11-26  0.00000
## 58 2012-11-27  0.00000
## 59 2012-11-28  0.00000
## 60 2012-11-29  0.00000
## 61 2012-11-30 98.51339
```
  
## Are there differences in activity patterns between weekdays and weekends?
  1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.
  

```r
    library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
    fitdata4 <- fitdata_imputed
    fitdata4$weekday <- !(wday(ymd(fitdata4$date), label=TRUE) %in% c("Sat", "Sun"))
    fitdata4$weekday <- as.factor(fitdata4$weekday)
    levels(fitdata4$weekday) <- c("weekend", "weekday")
    
    head(fitdata4)
```

```
##      steps       date interval weekday
## 1 30.33333 2012-10-01        0 weekday
## 2 18.00000 2012-10-01        5 weekday
## 3  7.00000 2012-10-01       10 weekday
## 4  8.00000 2012-10-01       15 weekday
## 5  4.00000 2012-10-01       20 weekday
## 6 27.75000 2012-10-01       25 weekday
```

```r
    table(fitdata4$weekday)
```

```
## 
## weekend weekday 
##    4608   12960
```
  
  
  2. Make a panel plot containing a time series plot (i.e. type="l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
    avgSteps <- aggregate(fitdata4$steps, list(interval = as.numeric(as.character(fitdata4$interval)), weekday = fitdata4$weekday), FUN = "mean")
names(avgSteps)[3] <- "meanOfSteps"
    library(lattice)
    xyplot(avgSteps$meanOfSteps ~ avgSteps$interval | avgSteps$weekday, layout = c(1, 2), type = "l", xlab = "Interval", ylab = "Number of steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png)<!-- -->



