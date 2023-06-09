# Google-Data-Analytics-Cyclistic bike-share analysis case study by using R-
  Hello! I've been working on completing the Google Data Analytics Professional Certificate on Coursera. While it was challenging and stressful at times, it was also a great learning experience!

  I will approach this case study by using RStudio and Tableau.
## Scenario

  I am assigned as a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. 
   From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.


## Stakeholders:

Lily Moreno: The director of marketing and my manager.
Cyclistic executive team: A detail-oriented executive team who will decide whether to approve the recommended marketing program.
Cyclistic marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Cyclistic’s marketing strategy.

## Objective:

  Final goal for the marketing analytics team: Design marketing strategies aimed at converting casual riders into annual members.

  In order to do that, however, the marketing analyst team needs to better understand better understands:

  - How annual members and casual riders differ.
  - Why casual riders would buy a membership.
  - How digital media could affect their marketing tactics.


## Data Preparation:

  I will be using Cyclistic’s past 12 month's ( May 2022 - April 2023) trip data from [here](https://divvy-tripdata.s3.amazonaws.com/index.html) to analyze and identify trends The data has been made available by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement).

  I have downloaded the data file and stored it in the E drive of my computer.Then, I extracted the files and converted the xlsx files to csv format using Excel for convenience in importing the files into R.
## Data Processing:
  Firstly, we would need to install & load the packages required for this process, which in this case will be: Tidyverse, Janitor & Lubridate,skimr
```{r}
install.packages("tidyverse")
install.packages("janitor")
install.packages("lubridate")
install.packages("skimr")
library(tidyverse)
library(janitor)
library(lubridate)
library(skimr)
```
  Set the working directory
```{r}
setwd("E:\\Work\\data\\Cyclistic\\2022-2023")
```
  Then, I inputted the 12 data sets from my hard drive into R.
```{r}
t5_22 <- read.csv("202205-divvy-tripdata.csv")
t6_22 <- read.csv("202206-divvy-tripdata.csv")
t7_22 <- read.csv("202207-divvy-tripdata.csv")
t8_22 <- read.csv("202208-divvy-tripdata.csv")
t9_22 <- read.csv("202209-divvy-tripdata.csv")
t10_22 <- read.csv("202210-divvy-tripdata.csv")
t11_22 <- read.csv("202211-divvy-tripdata.csv")
t12_22 <- read.csv("202212-divvy-tripdata.csv")
t1_23 <- read.csv("202301-divvy-tripdata.csv")
t2_23 <- read.csv("202302-divvy-tripdata.csv")
t3_23 <- read.csv("202303-divvy-tripdata.csv")
t4_23 <- read.csv("202304-divvy-tripdata.csv")
```
  and merge all datasets into one table
```{r}
trip_data <- t5_22 %>% 
  union_all(t6_22) %>% 
  union_all(t7_22) %>% 
  union_all(t8_22) %>% 
  union_all(t9_22) %>% 
  union_all(t10_22) %>% 
  union_all(t11_22) %>% 
  union_all(t12_22) %>% 
  union_all(t1_23) %>% 
  union_all(t2_23) %>% 
  union_all(t3_23) %>% 
  union_all(t4_23)
```
  also remove single table to free up  memmory
```{r}
rm(t5_22,
   t6_22,
   t7_22,
   t8_22,
   t9_22,
   t10_22,
   t11_22,
   t12_22,
   t1_23,
   t2_23,
   t3_23,
   t4_23)
```
  Remove Na, unnecessary column and objects is only a bank
```{r}
trip_data <-drop_na(trip_data)
```
```{r}
trip_data <- subset(trip_data, select = -c(ride_id, start_station_id, end_station_id))

```

```{r}
all_trips <- trip_data %>% 
  filter(rideable_type!="",
         started_at!="",
         ended_at!="",
         start_station_name!="",
         end_station_name!="",
         start_lat!="",
         start_lng!="",
         end_lat!="",
         end_lng!="",
         member_casual!="")
```


  Add columns that list the date, month, day, and year
```{r}
all_trips$date <- as.Date(all_trips$started_at)
all_trips$month <- format(as.Date(all_trips$date), "%B")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- weekdays(all_trips$date)
```

  Order the days of the week. Will also order the month
```{r}
all_trips$day_of_week <- ordered(all_trips$day_of_week, levels=c( "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"))

all_trips$month <- ordered(all_trips$month, levels=c("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"))
```


  Calculate the trip length and convert it to Minutes
```{r}
all_trips$trip_length <- difftime(all_trips$ended_at,all_trips$started_at, units="mins")  
all_trips$trip_length <- round(all_trips$trip_length, 2)
```

  Check the data structure
```{r}
str(all_trips)
```
![str](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/5ce0d497-3d5d-4214-86c4-8b62a78859c2)

  Convert Trip_length to numeric 
```{r}
all_trips$trip_length <- as.numeric(all_trips$trip_length)

```

  Remove observations where the trip length is less than 0
```{r}
all_trips <- filter(all_trips, trip_length > 0)
```
  export file
```{r}
write.csv(all_trips, 
          file = file.path("E:\\Work\\data\\Cyclistic\\2022-2023", "all_trips.csv"))
```
## Analysis
  
  Comparing trip_length members and casual users

```{r}
aggregate(all_trips$trip_length ~ all_trips$member_casual , FUN = mean)
aggregate(all_trips$trip_length ~ all_trips$member_casual , FUN = median)
aggregate(all_trips$trip_length ~ all_trips$member_casual , FUN = max)
aggregate(all_trips$trip_length ~ all_trips$member_casual , FUN = min)
```
![ss](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/93d8167e-07bc-4533-8b18-c563d9ca4ca4)

  The most popular start station and end station by member

```{r}
count(filter(all_trips, member_casual=='member'), start_station_name, sort = T ,name = "total_trip")
```
![popular_start_st](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/818f98ac-02a5-412e-869d-f81b51792884)

```{r}
count(filter(all_trips, member_casual=='member'), end_station_name, sort = T ,name = "total_trip")
```
![end_st](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/191efb58-e774-4730-be85-7f38ccce91be)

  The most popular start station and end station by casual

```{r}
count(filter(all_trips, member_casual=='casual'), start_station_name, sort = T , name = "total_trip")

```
![popular_start_st_by casual](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/b301af79-37d9-438f-9340-f6742024077d)

```{r}
count(filter(all_trips, member_casual=='casual'), end_station_name, sort = T , name = "total_trip")
```
![popular_end_st by casual](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/80241b15-01cd-4958-b456-8c4a26c05c09)


## Share(RStudio)

  Because the total number of trips will become very big. The value of ‘Number of trips’ would be displayed as 2e+05, 4e+05 so i use options(scipen =) to remove scientific values from our graphs

```{r}
options(scipen = 999)

```

  Total Number of Trips by Day

```{r}
ggplot(data = all_trips) +
  aes(x = day_of_week, fill = member_casual) +
  geom_bar(position = 'dodge') +
  labs(x = 'Day of week', y = 'Number of trips', fill = 'Member type', title = 'Number of trips by member type') +
    theme(axis.text.x = element_text(angle = 60, hjust = 1))

```
![trip per day](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/e13da215-6bf2-43c5-bd37-557268907a75)


  Total Number of Trips by Month

```{r}
ggplot(data = all_trips) +
  aes(x = month, fill = member_casual) +
  geom_bar(position = 'dodge') +
  labs(x = 'Month', y = 'Number of trips', fill = 'Member type', title = 'Number of trips per month') + 
  theme(axis.text.x = element_text(angle = 60, hjust = 1))
```
![trip per month](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/1e181b3a-62b7-4f17-8cd2-db784f822882)

## Share (Tableau)
  For interactive Tableau Dashboard, click in this [link](https://public.tableau.com/app/profile/le.nam/viz/Googledataanalystcasestudy1Cyclistic/Dashboard2).
By using a visualization tool such as Tableau, we can create  well-designed visuals to assist in the share phase. Here are my findings.

![view_data](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/9d9c1206-1b97-4446-9fb8-8c718abbcd28)

  As we can observe, annual members constitute 60.5% of the total trips, while casual riders account for 39.5% of the trips. The number of trips for annual members remains relatively stable throughout the week and month. This suggests a possibility that annual members are using the bikes as their primary mode of transportation.
  
  The other hand, casual members have considerably longer average trip durations compared to annual members. This suggests that casual members primarily use the bikes for leisure activities or possibly sports activities, which could explain the extended trip durations.
![aver_week](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/b4f9bf4f-3f4a-4f34-ac2d-7bbcfb6c0144)
 
 Let's see the start and end time of trip
 
 ![s_e](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/36d0fe4b-a3b4-4b6f-ba6b-01a9932c333c)
  Based on the above chart, it is evident that members have peak usage times between 6-9 am and 5-7 pm, which align with the typical rush hour periods. These peak periods strongly indicate that members are local residents who rely on Cyclistic's bikes as a part of their daily routines, likely for commuting to and from work or other regular activities.

  Both of these insights strongly support our hypothesis and we can strongly belive that
- The members and casual customers are two different customer groups.
- The members are working locals who use the bikes to commute daily to and from their workplaces, while casual customers are tourists and use the bikes for sightseeing around Chicago.

 
Now let's dive deeper and look at a map of Chicago.
![viz_ppst](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/504b5c7b-ac2a-495f-b0ee-8dda8c0a411c)
  
  From the map charts, it is clear that the starting and ending locations of members are concentrated in the downtown area of Chicago, indicating that they predominantly use Cyclistic's bikes for commuting to businesses and offices in that area. On the other hand, casual customers' locations are more spread out,  near the beach and big parks, that suggesting  they use the bikes for exploring different areas of Chicago and enjoying leisurely rides.

  And another thing  that can be noticed from this chart.

![bike_type](https://github.com/KhacNam998/Google-Data-Analytics-Case-Study-1-using-RStudio-/assets/128809896/2dac2d57-0fdc-449e-8667-13d748ad7291)
  Since only casual customers make use of the dock_type, this presents an opportunity to strategize advertising campaigns on dock station to converting casual customers into members.
  
## BUSINESS SUGGESTIONS

  This is my first experience working on a real-life data analyst project. I acknowledge that I have limited expertise in offering business suggestions or working in marketing. Therefore, I would highly appreciate any feedback or input regarding this matter.
  
  These are my suggestions for the marketing team to convert casual riders to annual members:
- The marketing team should prioritize the top 10 stations that experience high casual trip volumes from May to September. This timeframe presents the best opportunity to launch a new marketing campaign, such as weekend membership,sesonal membership.

- Highlight the advantages of memberships, such as offering discounts during peak periods like the summer season or on weekends.

- Social media ads targeting people who have already experienced any bike-sharing service and who are interested in travel, outdoor sports, parks, museum, Chicago touristic landmarks, etc.

*** That concludes this case, thank you for reading my case study till the end, I hope you may have found inspiration from it!










