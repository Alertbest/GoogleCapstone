# Google-DAC
This is the repository for my Google Data Analysis Course Capstone

# INTRODUCTION

Cyclistic (a fictional company) was launched in 2016 as a bike sharing company. This company offers the opportunity to unlock a bike from one station and return the bike to another station at anytime. The program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

As a Junior Data Analyst in the Cyclistic Marketing Analytics Team, Lily Moreno(The Director of Marketing and my manager) the responsibility to explore the available data to know how annual members and casual riders use Cyclistic bikes differently.

## BUSINESS TASK

The available data will be used to understand how casual and annual members use cyclistic bike differently with a view to improve the annual membership rate of the company.

## DATA PREPARATION

The dataset that will be used for the project is Cyclistic's historical trip data. The data was made available by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement). The dataset is reliable and original because it was for customers of Cyclistic. This dataset  do not contain any personal informations of either the annual members or casual riders.

The data for the year 2021 was downloaded as zip files and these were extracted into a folder created for the project. The folder contain 12 files for the 12 months in the year of 2021 and the 12 files were checked to ensure that it contain same information. The 12 files were also renamed to reflect the name of the month
The following are the column name:

ride_id: this is a unique ID for each rider

rideable_type: this indicates the type of bike that each rider used. The available bikes are classic bike, electric bike and docked bike

started_at: this is a datetime of when the rider took the bike

ended_at: this is the datetime of when the rider returned the bike

start_station_name: this is the name of the station where the rider returned the bike

start_station_id: this is the id of the start station

end_station_name: this is the name of the end station

end_station_id: this is the id of the end station

start_lat: this is the latitude of the start location

start_lng: this is the longitude of the start location

end_lat: this is the latitude of the end location

end_lng: this is the longitude of the end location

member_casual: this is the membership status of the rider, he can either be an annual member or a casual member

## DATA PROCESSING

The tool that will be used for processing this data is the R studio, this is because Excel can not handle the large size of the dataset. I started this process by installing the packages that will make the analysis easy.

``` r
install.packages("tidyverse")
install.packages("lubridate")
install.packages("dplyr")
install.packages("ggplot2")
```
``` r
library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2)
```

I then added the 12 months file to Rstudio in preparation for analysis. I used read_csv for this task as seen below:

``` R
jan_ride <- read_csv("jan_2021.csv")
feb_ride <- read_csv("feb_2021.csv")
mar_ride <- read_csv("mar_2021.csv")
april_ride <- read_csv("april_2021.csv")
may_ride <- read_csv("may_2021.csv")
june_ride <- read_csv("june_2021.csv")
july_ride <- read_csv("july_2021.csv")
aug_ride <- read_csv("aug_2021.csv")
sept_ride <- read_csv("sept_2021.csv")
oct_ride <- read_csv("oct_2021.csv")
nov_ride <- read_csv("nov_2021.csv")
dec_ride <- read_csv("dec_2021.csv")

```

I combined all the 12 months with rbind so as to make it easy to analyse

``` r
ride_2021 <- rbind(jan_ride, feb_ride, mar_ride, april_ride,may_ride, june_ride, july_ride, aug_ride, sept_ride, oct_ride, nov_ride, dec_ride)
```

I used the glimpse() function to look at the column name and also some row in the dataset. This shows that there are 15 columns and 5595063 rows in the dataset

![glimpse](https://user-images.githubusercontent.com/89348077/163691116-ff95646a-8d9a-4415-97fd-6dd99734287a.JPG)

To make it easier to use the dataset, I change the column name of the rideable_type to ride_type names function. The column is in the second column which is why I put 2 in square bracket.

``` r
names(ride_2021)[2]<- "ride_type"
```

I decided to select only the columns that will be relevant for the analysis with the select() function. 
``` r
trimmed_ride_2021 <- ride_2021 %>%  
   select(ride_id, ride_type, started_at, ended_at, ride_length, start_station_name, start_station_id, end_station_name, end_station_id, member_casual)
```
I checked the column name with colnames(). Note that the new name of the dataset is trimmed_ride_2021
``` r
colnames(trimmed_ride_2021)
```
![image](https://user-images.githubusercontent.com/89348077/163691695-655b7e15-b4c5-41d2-8e6b-1237086fe1d9.png)

I checked the total number of rows in the dataset and this returned a total of 5,595,063
``` r
nrow(trimmed_ride_2021)
```
I checked the ride_type column to know the type of rides that are available and to check whether there is any outliner
``` r
unique(trimmed_ride_2021$ride_type)
```
![image](https://user-images.githubusercontent.com/89348077/164936156-9aec2604-e49c-4622-b37c-c83b145b814a.png)


To gain more insight from the started_at column, the column was converted into an object of class 'POSIXit' and then the day and month are extracted into another column

``` r
trimmed_ride_2021$start_time <- strptime(trimmed_ride_2021$started_at, format = "%m/%d/%Y %H:%M")
trimmed_ride_2021$day <- format(trimmed_ride_2021$start_time,  "%a") # to create a new column with the day of the week from the started_at column
trimmed_ride_2021$month <- format(trimmed_ride_2021$start_time,  "%b")  # to create a new column with the day of the week from the started_at column
```
The ride_length column was converted to numeric so as to make it possible to do calculation on the column
``` r 
is.factor(trimmed_ride_2021$ride_length)
trimmed_ride_2021$ride_length <- as.numeric(as.character(trimmed_ride_2021$ride_length))
is.numeric(trimmed_ride_2021$ride_length)
```

I created another column from the ride_length column which converted the column into minutes, this new column is named ride_length_minute.
``` r
trimmed_ride_2021$ride_length_minute <- floor(trimmed_ride_2021$ride_length/60)
```
I then filter out all rides that are less than 1 minute and rides that are more than 1 day
``` r
cleaned_ride_2021 <- trimmed_ride_2021 %>% filter(ride_length_minute > 1) %>% 
     filter(ride_length_minute < 1440)
```
This made my total rows to be  5390427
``` r
nrow(cleaned_ride_2021)
```
I checked the data for the day with the highest number of ride. I was able to confirm that Cyclistic have the highest number of ride on Saturday
``` r
cleaned_ride_2021 %>% group_by(day) %>% 
  summarise(count=n()) %>% arrange(desc(count)) %>% View()
  
```
![image](https://user-images.githubusercontent.com/89348077/164950879-7fa9370a-19d7-4e0d-877d-eeabb9027797.png)

The summarise() function was used to calcualte the mean, median, max and mean of the data
``` r
cleaned_ride_2021 %>%  # this was used to get the mean, median, max and min
  summarise(mean=mean(ride_length_minute), median=median(ride_length_minute), max=max(ride_length_minute), min=min(ride_length_minute))
```

![image](https://user-images.githubusercontent.com/89348077/164951128-f4b853a6-6313-4040-9bdd-e3b217955f7b.png)

I compared the ride length mean between the member and casual riders
``` r
aggregate(cleaned_ride_2021$ride_length ~ cleaned_ride_2021$member_casual, FUN = mean)
```
![image](https://user-images.githubusercontent.com/89348077/164951386-e24aa013-5f11-4304-8ccf-413380a35190.png)

aggregate(cleaned_ride_2021$ride_length ~ cleaned_ride_2021$member_casual, FUN = median)

![image](https://user-images.githubusercontent.com/89348077/164951405-9cc9974a-cf10-4be7-a74b-d0532ffe60e2.png)


The maximum ride duration which is represented by ride_length for Casual riders is 1439 minutes and member riders have a maximum ride_length of 1439 minutes, the minimum ride length for both casual riders and member rider is 2 minutes
```r
aggregate(cleaned_ride_2021$ride_length_minute ~ cleaned_ride_2021$member_casual, FUN = max)
aggregate(cleaned_ride_2021$ride_length_minute ~ cleaned_ride_2021$member_casual, FUN = min)
```
![image](https://user-images.githubusercontent.com/89348077/164951591-9af9d96b-c37c-4325-986c-30e278ddf8b7.png)


I export the file with write.csv() for further visualization on PowerBi
``` r
write.csv(cleaned_ride_2021,"C:\\Users\\Public\\Documents\\Cyclistic.csv", row.names=FALSE)
```

## Visualization With GGPlot
I compared the numbers of ride for each day and the day with the highest number of ride throughout the year was Saturday
``` r
ggplot(data=cleaned_ride_2021)+
  geom_bar(mapping=aes(reorder(x=day,-ride_length_minute), fill=day))+ labs(x= "Day", title="The Number Of Ride Per Day")
```
![image](https://user-images.githubusercontent.com/89348077/165398958-cf6feae0-3f6d-454e-a6c3-c7678109edab.png)














