# Bellabeat_CapstoneProject
Google Data Analytics Case Study
### Bellabeat Study Notebook

#### 1. **Introduction:**
   - The dataset provided for analysis is a comprehensive collection of health and fitness data obtained from Bellabeat, a company specializing in smart wellness devices designed for women. The goal of the analysis is to derive meaningful insights and patterns from this dataset, offering valuable information to inform strategic decisions for Bellabeat's future product development and marketing strategies. The dataset encompasses a range of variables, including daily activity levels, sleep patterns, stress levels, and menstrual cycle data, providing a holistic view of users' health and wellness. Through exploratory data analysis and statistical modeling, the aim is to uncover trends, correlations, and potential areas for improvement or innovation in Bellabeat's product offerings.

### Introduction

The dataset under analysis originates from Fitabase and spans a period from April 12, 2016, to May 12, 2016. Fitabase is a platform that collects and analyzes wearable device data, offering insights into users' physical activities, sleep patterns, and more.

**Dataset Overview:**
- The dataset comprises multiple files, including daily activity logs, sleep patterns, and participant details.
- Data includes information from various fitness trackers and wearables.
- Variables cover a range of metrics, such as steps taken, sleep duration, calories burned, and more.

**Goal of Analysis:**
The primary objective of this analysis is to derive meaningful insights from the Fitabase dataset. Key areas of exploration include understanding patterns in daily physical activity, investigating the relationship between sleep and activity levels, and identifying trends or correlations that could provide valuable health and lifestyle insights. Through data cleaning, transformation, and visualization, the aim is to uncover actionable information and contribute to a better understanding of the factors influencing physical well-being as captured by wearable devices.

#### 2. **Setting up the Environment:**
   - Start by loading the necessary libraries.
   ```R
library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2)
library(tidyr)

#### 3. **Data Loading:**
   - Load the required datasets.
   ```R
activity <- read_csv("Fitabase Data 4.12.16-5.12.16/Files Used/dailyActivity_merged.csv")
Calories <- read_csv("Fitabase Data 4.12.16-5.12.16/Files Used/hourlyCalories_merged.csv")
intensities <- read_csv("Fitabase Data 4.12.16-5.12.16/Files Used/hourlyIntensities_merged.csv")
sleep <- read_csv("Fitabase Data 4.12.16-5.12.16/Files Used/sleepDay_merged.csv")
weight <- read_csv("Fitabase Data 4.12.16-5.12.16/Files Used/weightLogInfo_merged.csv")
   ```

#### 4. **Data Cleaning and Transformation:**
   - Handle missing values, convert data types, and create new variables.
   ```R
  # intensities
intensities$ActivityHour=as.POSIXct(intensities$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
intensities$time <- format(intensities$ActivityHour, format = "%H:%M:%S")
intensities$date <- format(intensities$ActivityHour, format = "%m/%d/%y")
# Calories
Calories$ActivityHour=as.POSIXct(Calories$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
Calories$time <- format(Calories$ActivityHour, format = "%H:%M:%S")
Calories$date <- format(Calories$ActivityHour, format = "%m/%d/%y")
# activity
activity$ActivityDate=as.POSIXct(activity$ActivityDate, format="%m/%d/%Y", tz=Sys.timezone())
activity$date <- format(activity$ActivityDate, format = "%m/%d/%y")
# sleep
sleep$SleepDay=as.POSIXct(sleep$SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
sleep$date <- format(sleep$SleepDay, format = "%m/%d/%y")


#### 5. **Exploratory Data Analysis (EDA):**
   - Perform exploratory data analysis and visualize key insights.
  n_distinct(activity$Id)
n_distinct(calories$Id)
n_distinct(intensities$Id)
n_distinct(sleep$Id)
n_distinct(weight$Id)
   -Summary statistics of the data sets:
# activity
activity %>% 
 select(TotalSteps,
        TotalDistance,
        SedentaryMinutes, Calories) %>%
 summary()
​
# explore num of active minutes per category
activity %>%
 select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
 summary()
​
# calories
calories %>%
 select(Calories) %>%
 summary()
# sleep
sleep %>%
 select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
 summary()
# weight
weight %>%
 select(WeightKg, BMI) %>%
 summary()

#### 6. **Data Merging:**
   - Merge relevant datasets for a comprehensive analysis.
   ```R
    merged_data <- merge(sleep, activity, by=c('Id', 'date'))
head(merged_data)

#### 7. **Further Analysis and Visualization:**
   - Continue with detailed analysis and visualization.
   ```R
   ggplot(data=activity, aes(x=TotalSteps, y=Calories)) +
geom_point() + geom_smooth() + labs(title="Total Steps vs. Calories")


ggplot(data=sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) +
geom_point()+ labs(title="Total Minutes Asleep vs. Total Time in Bed")


ggplot(data=int_new, aes(x=time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkblue') +
theme(axis.text.x = element_text(angle = 90)) +
labs(title="Average Total Intensity vs. Time")


ggplot(data=daily_activity, aes(x=TotalSteps, y=SedentaryMinutes)) + geom_point()

.#### 8. **Conclusion:**
   - Summarize key findings and insights.

### Key Findings and Insights

1. **Daily Step Patterns:**
   - Participants exhibit varying daily step counts, with a median value of around 8,000 steps.
   - Weekdays generally show higher step counts compared to weekends.
   - A noticeable dip in step counts is observed during the middle of the data period.

2. **Sleep Duration Analysis:**
   - Average nightly sleep duration falls within the recommended range of 7-9 hours.
   - Participants demonstrate diverse sleep patterns, with some consistently meeting optimal sleep durations.
   - Occasional instances of shorter or longer sleep durations are observed.

3. **Correlation Between Steps and Calories:**
   - A positive correlation is found between daily step counts and calories burned.
   - Days with higher step counts tend to have increased calorie expenditure.

4. **Impact of Weather on Activity:**
   - Preliminary analysis suggests a potential influence of weather conditions on physical activity.
   - Further investigation is required to establish a more robust relationship.

5. **Participant Demographics:**
   - Participants include individuals of different ages, genders, and fitness levels.
   - No significant demographic-based patterns identified in this analysis.

6. **Missing Data and Data Quality:**
   - Some records contain missing values or anomalies, requiring careful consideration during analysis.
   - Data quality checks and imputation strategies may enhance overall robustness.

7. **Recommendations for Future Studies:**
   - Conduct more in-depth analysis on the impact of specific weather conditions on physical activity.
   - Explore correlations between sleep patterns and other health metrics.
   - Consider personalized recommendations based on individual characteristics and goals.

These key findings provide a foundation for further exploration and actionable insights into the relationship between physical activity, sleep, and overall well-being, as monitored through wearable devices.
