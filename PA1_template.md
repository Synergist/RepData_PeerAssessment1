# Reproducible Research: Peer Assessment 1


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

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

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



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
