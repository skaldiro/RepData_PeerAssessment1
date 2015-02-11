# Peer Assessment 1 - Reproducible Research
Selman Kaldiroglu  
Monday, February 09, 2015  

## Loading and preprocessing the data

1. Show any code that is needed to
2. Load the data (i.e. read.csv())
3. Process/transform the data (if necessary) into a format suitable for your analysis


```r
setwd("C:/Users/skaldiroglu/Documents/R/RepRsrch/repdata-data-activity")
dat <- read.csv("activity.csv",header = TRUE, colClasses = c("numeric","Date","numeric"))
```

## What is mean total number of steps taken per day?

*For this part of the assignment, you can ignore the missing values in the dataset.*

1. Calculate the total number of steps taken per day
2. If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day
3. Calculate and report the mean and median of the total number of steps taken per day


Code:

```r
dat_mean <- subset(dat, !is.na(dat$steps))
data_mean_grp <- aggregate(dat_mean$steps, by = list(dat_mean$date), FUN = sum)
colnames(data_mean_grp) <- c("date","steps")
```

Plot:

```r
hist(data_mean_grp$steps, xlab="Total Steps per day", col=c("red"), main = "Histogram of Steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 


Mean and Median:


```r
mean(data_mean_grp$steps)
```

```
## [1] 10766.19
```


```r
median(data_mean_grp$steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?

1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

Code:

```r
data_by_interval <- aggregate(dat_mean$steps, by = list(dat_mean$interval), FUN = sum)

colnames(data_by_interval) <- c("interval","steps")
```

Plot:

```r
plot(data_by_interval$interval, data_by_interval$steps, type="l", ylab="Total Steps", xlab="Intervals")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png) 

Interval with the Maximum Steps:


```r
subset(data_by_interval, data_by_interval$steps == max(data_by_interval$steps), na.rm = TRUE)
```

```
##     interval steps
## 104      835 10927
```

## Imputing missing values

*Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.*

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

Total Number of Missing Values:


```r
dat_miss <- subset(dat, is.na(dat$steps))
nrow(dat_miss)
```

```
## [1] 2304
```

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
3. Create a new dataset that is equal to the original dataset but with the missing data filled in.
4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

Code:

```r
dat$steps[is.na(dat$steps)] <- with(dat, ave(steps, interval, FUN = function(x) median(x, na.rm=TRUE)))[is.na(dat$steps)]

data_by_date <- aggregate(dat$steps, by = list(dat$date), FUN = sum)

colnames(data_by_date) <- c("date","steps")
```

Plot:

```r
hist(data_by_date$steps, xlab="Total Steps per day", col=c("red"), main = "Histogram of Steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-10-1.png) 

Mean and Median:


```r
mean(data_by_date$steps)
```

```
## [1] 9503.869
```


```r
median(data_by_date$steps)
```

```
## [1] 10395
```

## Are there differences in activity patterns between weekdays and weekends?

*For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.*

1. Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.
2. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

![](PA1_template_files/figure-html/last-1.png) 


