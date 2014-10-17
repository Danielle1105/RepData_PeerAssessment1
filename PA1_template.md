---
title: "Reproducible Research_project 1"
output: html_document
---



```r
library(dplyr)
data<-read.csv("activity.csv")
na_omit<-na.omit(data)
sumdata<-summarise(group_by(na_omit,date),stepsum=sum(na_omit$step,na.rm=TRUE))
hist(sumdata$stepsum)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1-1.png) 

```r
mean<-mean(sumdata$stepsum,na.rm=TRUE)
print(mean)
```

```
## [1] 10766.19
```

```r
median<-median(sumdata$stepsum,na.rm=TRUE)
print(median)
```

```
## [1] 10765
```



```r
mean_interval<- summarise(group_by(na_omit,interval),meanstep=mean(na_omit$step,na.rm=TRUE))
plot(mean_interval)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

```r
max(mean_interval)
```

```
## [1] 2355
```
My strategy is using the mean for that 5-minute interval to subsititute the NA 

```r
sum(is.na(data))
```

```
## [1] 2304
```

```r
newdata<-merge(data,mean_interval,by.x="interval",by.y="interval")
newdata$steps[is.na(newdata$steps)]<-newdata$meanstep[is.na(newdata$steps)]
sumdata2<-summarise(group_by(newdata,date),stepsum=sum(newdata$steps,na.rm=TRUE))
hist(sumdata2$stepsum)
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

```r
mean2<-mean(sumdata2$stepsum,na.rm=TRUE)
print(mean2)
```

```
## [1] 10766.19
```

```r
median2<-median(sumdata2$stepsum,na.rm=TRUE)
print(median2)
```

```
## [1] 10766.19
```
The mean is equal to previous one. The median has changed.

