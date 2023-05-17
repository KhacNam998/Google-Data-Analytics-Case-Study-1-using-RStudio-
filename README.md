# Google-Data-Analytics-Cyclistic bike-share analysis case study by using R-
Hello! I've been working on completing the Google Data Analytics Professional Certificate on Coursera. While it was challenging and stressful at times, it was also a great learning experience!

I will approach this case study by using RStudio and Tableau.
## Scenario

I am assigned as a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.


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

I will be using Cyclistic’s past 12 month's ( May 2022 - April 2023) trip data from [here](https://divvy-tripdata.s3.amazonaws.com/index.html) to analyze and identify trends The data has been made available by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement)
I have downloaded the data file and stored it in the E drive of my computer.
Then, I extracted the files and converted the xlsx files to csv format using Excel for convenience in importing the files into R.
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

