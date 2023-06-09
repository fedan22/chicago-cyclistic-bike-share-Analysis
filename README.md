


Author: Boluwatife Afe


Date: May 7, 2023


[Tableau Dashboard](https://public.tableau.com/app/profile/boluwatife.afe/viz/CYCLISTICBIKESHAREANALYSISDASHBOARD/Dashboard1#1)



[Tableau story Presentation to Stakeholders](https://public.tableau.com/app/profile/boluwatife.afe/viz/CYCLISTICBIKESHAREANALYSISPRESENTATION/Story1?publish=yes)





The Case study follows the six steps analysis process:

❓ Ask


🖥 Prepare


🛠 Process


📈 Analyze


📋 Share


🔌 Act



SCENARIO
In 2016, Cyclistic launched a successful bike-share offering. The company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members.


1. ASK


 BUSINESS TASK: is to analyze Divvy's riding data to develop digital marketing strategies to convert casual riders into annual members.
 

 Primary stakeholders: The director of marketing Lily Moreno and Cyclistic executive team.
 

 Secondary stakeholders: Cyclistic marketing analytics team.
 


2. PREPARE



Data Source: 12 Month (Aug 2020 to August 2021) of Cyclistic trip Data from Motivate International Inc: [data source link](https://divvy-tripdata.s3.amazonaws.com/index.html) with [license](https://ride.divvybikes.com/data-license-agreement)



The dataset has 12 CSV, 13 columns and 4.9 million rows. The data also follow a ROCCC approach:


- Reliability: the data includes complete and accurate ride data from Divvy. Divvy is program of the Chicago Department of Transportation (CDOT), which owns the city’s bikes, stations and vehicles.


- Original: the data is from Motivate International Inc, which operates the City of Chicago’s Divvy bicycle sharing service.


- Comprehensive: The data incudes type of bikes, start and end station name, start and end time, station ID, station longtitude and latitude, membership types.


- Current: data is up to date to August 2021


- Cited: the data is cited and under current [license](https://github.com/emily1618/Google-Data-Analytics-Cyclistic-Case-Study#2-prepare) agreement.




The dataset has some limitations:


- NA values: after checking sum(is.na(bike_data)), we see the dataset has 1893790 NA values, such as in starting_station_id, end_station_id. Further investigation we noticed the NA values are mostly under rideable type: electric bike. Future investigations may be needed by the station names are not entered for electric bike.


```  
head(count(bike_data, start_station_name, member_casual,  rideable_type, sort= TRUE))
head(count(bike_data, end_station_name, member_casual,  rideable_type, sort= TRUE)) 
                              
```

- Personally identifiable information: the dataset has a restriction of personally identifiable information, so we have no data if that a ride is by an unique rider or the same rider who ride more than once as a casual rider or a member.




3. PROCESS


Examine the data:
 
 ```
 head(bike_data)
dim(bike_data)
colnames(bike_data)
summary(bike_data)

```


Identify unnecessary data and remove those columns:

```
bike_data <- bike_data %>% select(-c(start_lat, start_lng, end_lat, end_lng))

```


Add two columns: ride length and day of the week:

```
bike_data <- bike_data %>% mutate(ride_length = ended_at - started_at) %>% mutate(day_of_week = weekdays(as.Date(bike_data$started_at)))

#Convert ride_length from from seconds into minutes
bike_data$ride_length <- as.numeric(bike_data$ride_length)
bike_data$ride_length <- as.numeric(bike_data$ride_length/60)

```

The started and ended time is in a yyyy-mm-dd hh-mm-ss format. We can further divide this into two columns: date and time:

```
bike_data <- separate(bike_data,"started_at",into=c('start_date','start_time'), sep=' ')
bike_data <- separate(bike_data,"ended_at",into=c('start_date','start_time'), sep=' ')

```

Remove data error:

```
#check for data with negative ride length:
bike_data <- bike_data[bike_data$ride_length>0,]

#check for data with ride length  more than 1 day (86400 seconds or 1440 mins):
sum(bike_data$ride_length > 1440)

```



4. Analyze


Check min, max, mean, median and any outlier on the ride length.


```
summary(bike_data$ride_length)

```

Aggregate the data based on user types.

```
aggregate(bike_data$ride_length ~ bike_data$member_casual, FUN = mean)
aggregate(bike_data$ride_length ~ bike_data$member_casual, FUN = median)
aggregate(bike_data$ride_length ~ bike_data$member_casual + bike_data$day_of_week, FUN = mean)

```


Analyze ridership by user types and day of the week.

```
bike_data %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  
  group_by(member_casual, weekday) %>%  
  summarise(number_of_rides = n()							
  ,average_duration = mean(ride_length)) %>% 		
  arrange(member_casual, weekday)
  
```


<img src="boluwatife.png"> 



⛔️For the complete R code and analyze the data using ggplot for graphical interpretation, please view the rmd file on this [R code link](https://github.com/fedan22/chicago-cyclistic-bike-share-Analysis/blob/main/cyclistic.Rmd).




5. SHARE

[Tableau Dashboard on Bike Sharing Case Study.](https://public.tableau.com/app/profile/boluwatife.afe/viz/CYCLISTICBIKESHAREANALYSISDASHBOARD/Dashboard1#1)


<img src="Dashboard 1.png">




[Tableau Presentation on Cyclistic Bike Sharing Case Study.](https://public.tableau.com/app/profile/boluwatife.afe/viz/CYCLISTICBIKESHAREANALYSISPRESENTATION/Story1?publish=yes)


<img src="story 2.png">





6. ACT


Conclusion based on my analysis:

 - Casual riders ride longer duration, but least total trips.
 - Most popular station for casual riders are: Streeter Dr & Grand Ave, Lake Shore Dr & Monroe St, Millennium Park.
 - Casual riders rides mostly during the weekends.
 - Casual riders rides longer on docked bike.



Marketing recommendations to convert casual riders to annual members:

- Promotional weekend term membership for the weekends.


- Point-award incentive system for riding more trips in a membership format to receive discount and partnership offers.


- Marketing effort on the top 5 most popular stations for the causal riders. It can be a booth, print media on the bike or the locking station area, or social media post on contest starting from the most popular stations.

























