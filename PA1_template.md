---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data
1. We unzip the data with 

```r
unzip("activity.zip")
```
We then read the csv into the data variable with 

```r
data <- read.csv(file = "activity.csv", stringsAsFactors = FALSE)
```
2. We convert the 'date' column to a Date type as follows: 

```r
data$date <- as.Date(data$date, format="%Y-%m-%d")
```


## What is mean total number of steps taken per day?
1. To make a histogram of the total number of steps taken each day, we perform the following:

```r
dataSplitByDay <- split(data, data$date) 
stepsPerDay <- sapply(dataSplitByDay, function(x) sum(x$steps, na.rm=TRUE))
hist(stepsPerDay
    , xlab = "total number steps taken in a day"
    , main = "Histogram of total number steps taken each day")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 
2. Mean and median (over the course of all days) of the total steps per day:

```r
mean(stepsPerDay)
```

```
## [1] 9354
```

```r
median(stepsPerDay)
```

```
## [1] 10395
```
## What is the average daily activity pattern?
1.

```r
dataSplitIntervals <- split(data, data$interval)
avgStepsPerInterval <- sapply(dataSplitIntervals, function(x) mean(x$steps, na.rm=TRUE))
plot(names(dataSplitIntervals), avgStepsPerInterval, type="l")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 
2. The 5-minute interval containing, on average across all the days in the dataset, the maximum number of steps is:

```r
names(which.max(avgStepsPerInterval))
```

```
## [1] "835"
```
## Imputing missing values
1. The number of missing values in the data set is:

```r
nrow(data[!complete.cases(data),])
```

```
## [1] 2304
```
2. We fill in the missing values with the mean for that 5-minute interval, and store it in 'cleanData' data.frame:

```r
cleanData <- data
missingRows <- !complete.cases(cleanData)
cleanData[missingRows, ]$steps <- avgStepsPerInterval[match(as.character(cleanData[missingRows,]$interval), names(avgStepsPerInterval))]
```
4. 

```r
cleanDataSplitByDay <- split(cleanData, data$date) 
cleanStepsPerDay <- sapply(cleanDataSplitByDay, function(x) sum(x$steps, na.rm=TRUE))
hist(cleanStepsPerDay
    , xlab = "total number steps taken in a day"
    , main = "Histogram of total number steps taken each day")
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10-1.png) 

```r
mean(cleanStepsPerDay)
```

```
## [1] 10766
```

```r
median(cleanStepsPerDay)
```

```
## [1] 10766
```
Yes, imputing missing data had the effect of normalizing the estimates of the total daily number of steps. By 'normalizing', I am referring to the fact that the histogram is now much closer to a standard bel-shaped curve, and, notably, the median now equals the mean. 
## Are there differences in activity patterns between weekdays and weekends?

```r
library(lattice)
cleanData$dayType <- as.factor(ifelse(test = weekdays(cleanData$date) %in% c('Saturday', 'Sunday'), yes = 'weekend', no = 'weekday'))
xyplot( steps ~ interval | dayType, data=cleanData, type="l", panel = function(x,y) {panel.average(x,y,horizontal=F,col="blue")}, layout=c(1,2))
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-1.png) 
