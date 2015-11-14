# Reproducible Research: Peer Assessment 1

Dear Peer,  
  
thank you for taking the time to review my work. At this stage of the course I guess you are as pationate about data as I am. It would be great if we could connect through [linkedin](https://es.linkedin.com/in/andreipopovici).   
  
Best regards,  
Andrei  
  

## (A) Loading and preprocessing the data
### A1. Load the data  
The data file is located [here](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip).  
In the following chunk we will download the zip file, unzip it and read the raw data into myData dataframe.  

```r
sourceURL="https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
#tempZIP is the temporary variable where we will save our data zip file 
tempZIP<-tempfile()
# download the zip file into temp variable
download.file(sourceURL,tempZIP,mode="wb")
#We will save the unziped acttivity.csv file in the temporary variable tempCSV
tempCSV<-unzip(tempZIP)
#In RawData wi will read our raw data
RawData<-read.csv(tempCSV,stringsAsFactors = FALSE)
#Remove the temporary file created before
rm(tempZIP)
rm(tempCSV)
```

### A2. Process and transform the data
Let's look at the data. We expect 17568 observations and 3 varialbles: steps, date, interval.

```r
str(RawData)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```
We observe that their are NA values in steps variable but as indicated in the assigment we will take care of them later. We also see that the date variable is stored as char. In the next chunk we will transform date variable from char to date format.  

```r
#We will use lubridate library
library(lubridate)
#We will store the dataframe with in a new variable ProcessedData and alwways will keep our original data in the RawData variable
ProcessedData <- RawData
ProcessedData$date <- ymd(ProcessedData$date)
#Let's check our result
str(ProcessedData)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : POSIXct, format: "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

***
##(B) What is mean total number of steps taken per day?
First of all we will create a new data frame named StepsXday where we will store the total steps per day done by our subject. We will use dplyr library in order to summarise the data at day level. At this step we will just ignore the NA values.  

```r
# Load the dplyr library
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:lubridate':
## 
##     intersect, setdiff, union
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
# Summarize the totl number of steps per day in the StepsXday variable 
StepsXday<- ProcessedData %>% group_by(date) %>% summarise(total=sum(steps,na.rm=TRUE))
# Calculate the mean of total steps per day
mean(StepsXday$total)
```

```
## [1] 9354.23
```
  
### B1. Make a histogram of the total number of steps taken each day  

```r
# Plotting the histogram of total number of steps per day
hist(StepsXday$total, main="Total steps per day histogram", xlab="Number of total steps during the day")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

```r
# Calculate the mean of total steps per day
mean(StepsXday$total)
```

```
## [1] 9354.23
```

```r
# Calculate the mean of total steps per day
median(StepsXday$total)
```

```
## [1] 10395
```



***
## What is the average daily activity pattern?


***
## Imputing missing values


***
## Are there differences in activity patterns between weekdays and weekends?
