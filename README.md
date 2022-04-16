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







