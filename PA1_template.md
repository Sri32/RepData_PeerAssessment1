Reproducible Research - Peer Assignment 1 
========================================================


```
## Loading required package: splines
## Loaded gam 1.09.1
```

Given below are the steps undertaken for completion of peer assignment 1:

Step 1: Load the data


```r
setwd("C:/Sriraman/B D/5 Reproducible Research/Week 2/repdata-data-activity")
act <- read.csv("activity.csv",header=TRUE)
```

Step 2: Finding out the mean total number of steps taken per day

Histogram of total number of steps taken per day:


```r
aggr <- aggregate(steps~date, sum, data=act)
```

```r
ggplot(aggr,aes(x=date,y=steps))+geom_bar(stat='identity')+ theme(axis.text.x=element_text(angle=-90))
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

Step 3: Calculate and report the mean total number of steps taken per day

Following table shows the mean total number of steps taken per day or all days having valid values.


```r
meanact <- aggregate(steps~date, mean, data=act, na.rm=TRUE)
meanact
```

```
##          date   steps
## 1  2012-10-02  0.4375
## 2  2012-10-03 39.4167
## 3  2012-10-04 42.0694
## 4  2012-10-05 46.1597
## 5  2012-10-06 53.5417
## 6  2012-10-07 38.2465
## 7  2012-10-09 44.4826
## 8  2012-10-10 34.3750
## 9  2012-10-11 35.7778
## 10 2012-10-12 60.3542
## 11 2012-10-13 43.1458
## 12 2012-10-14 52.4236
## 13 2012-10-15 35.2049
## 14 2012-10-16 52.3750
## 15 2012-10-17 46.7083
## 16 2012-10-18 34.9167
## 17 2012-10-19 41.0729
## 18 2012-10-20 36.0938
## 19 2012-10-21 30.6285
## 20 2012-10-22 46.7361
## 21 2012-10-23 30.9653
## 22 2012-10-24 29.0104
## 23 2012-10-25  8.6528
## 24 2012-10-26 23.5347
## 25 2012-10-27 35.1354
## 26 2012-10-28 39.7847
## 27 2012-10-29 17.4236
## 28 2012-10-30 34.0938
## 29 2012-10-31 53.5208
## 30 2012-11-02 36.8056
## 31 2012-11-03 36.7049
## 32 2012-11-05 36.2465
## 33 2012-11-06 28.9375
## 34 2012-11-07 44.7326
## 35 2012-11-08 11.1771
## 36 2012-11-11 43.7778
## 37 2012-11-12 37.3785
## 38 2012-11-13 25.4722
## 39 2012-11-15  0.1424
## 40 2012-11-16 18.8924
## 41 2012-11-17 49.7882
## 42 2012-11-18 52.4653
## 43 2012-11-19 30.6979
## 44 2012-11-20 15.5278
## 45 2012-11-21 44.3993
## 46 2012-11-22 70.9271
## 47 2012-11-23 73.5903
## 48 2012-11-24 50.2708
## 49 2012-11-25 41.0903
## 50 2012-11-26 38.7569
## 51 2012-11-27 47.3819
## 52 2012-11-28 35.3576
## 53 2012-11-29 24.4688
```

Step 4: Time series plot of interval vs average number of steps


```r
tsplot <- aggregate(steps~interval, mean, data=act, na.rm=TRUE)
ggplot( data = tsplot, aes( interval, steps )) + geom_line() 
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 

Observation: 
============
It is observed that there is a spike in the number of steps for the duration of around 550.

Step 5: Number of missing values in the dataset


```r
sum(is.na(act))
```

```
## [1] 2304
```

Step 6: Replace missing values for steps with the mean of the non-missings using the na.gam.replace function. 

The new dataset is act2. The count of missing values in the new data frame is observed to be 0.


```r
act2 <- na.gam.replace(act)
sum(is.na(act2))
```

```
## [1] 0
```

Step 7: Histogram of total number of steps taken per day using the new data frame:


```r
aggrnew <- aggregate(steps~date, sum, data=act2)
```

```r
ggplot(aggrnew,aes(x=date,y=steps))+geom_bar(stat='identity')+ theme(axis.text.x=element_text(angle=-90))
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 

Observation: 
============
It is observed that the number of observations plotted on the histogram has increased. 

Step 8: Adding the Weekday Column.


```r
act3 <- act2
act3$wkdy <- weekdays(as.Date(act3$date))
act4 <- data.table(act3)
act4[, WkDay := ifelse(wkdy %in% c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday"), "Weekday",
                      ifelse(wkdy %in% c("Saturday", "Sunday"), "Weekend",
                              NA))]
```

```
##        steps       date interval   wkdy   WkDay
##     1: 37.38 2012-10-01        0 Monday Weekday
##     2: 37.38 2012-10-01        5 Monday Weekday
##     3: 37.38 2012-10-01       10 Monday Weekday
##     4: 37.38 2012-10-01       15 Monday Weekday
##     5: 37.38 2012-10-01       20 Monday Weekday
##    ---                                         
## 17564: 37.38 2012-11-30     2335 Friday Weekday
## 17565: 37.38 2012-11-30     2340 Friday Weekday
## 17566: 37.38 2012-11-30     2345 Friday Weekday
## 17567: 37.38 2012-11-30     2350 Friday Weekday
## 17568: 37.38 2012-11-30     2355 Friday Weekday
```

Step 9:


```r
ggplot( data = act4, aes( interval, steps )) + geom_line() +  facet_wrap(~ WkDay, nrow=2, ncol=1) 
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11.png) 
