# Case_study_GCS
First case study about curse Data Analitycs Google

---
title: "Case Study Bellabeat"
author: "Felipe San Martin"
date: "2023-02-04"
output:
  html_document: default
  pdf_document: default

### Introduction
 Welcome to the Bellabeat Data Analysis Case Study! In this project, we will be diving into the data from Bellabeat, a leading manufacturer of health-focused products for women. Our goal is to gain insight into how consumers are using their smart devices, and use that information to inform marketing strategy for the company. We will be working with a team of data analysts to complete this project, and following a structured process to ask questions, prepare data, process information, analyze results, share findings, and make recommendations. Join us on this exciting journey as we explore the data and make an impact on Bellabeat's future success.
![bellabeat](https://m.media-amazon.com/images/S/stores-image-uploads-na-prod/d/AmazonStores/ATVPDKIKX0DER/4bca851cfdc91b6d3943a2c22937efde.w3000.h600._CR0%2C0%2C3000%2C600_SX1920_.jpg)

### About the Company
 Bellabeat is a high-tech company founded by Urška Sršen and Sando Mur. The company specializes in manufacturing health-focused smart products designed specifically for women. Sršen, an artist by background, combines her creativity with technology to provide women with beautifully designed health-related devices. With its focus on collecting data about women's activities, sleep, stress levels, and reproductive health, Bellabeat is dedicated to empowering women with knowledge about their own well-being. Source:[Bellabeat Web](https://bellabeat.com/)


### Business task
 Bellabeat wants to gain a deeper understanding of how their customers are using their smart devices in order to inform their marketing strategy and identify opportunities for growth.

#### Objective: 
 * Analyze smart device usage data to gain insight into how consumers use non-Bellabeat smart devices.
 
 * Analyze smart device usage data for a specific Bellabeat product and provide high-level recommendations to the company on how these trends can inform their marketing strategy.
 
### Source of Data
 The data used in this analysis is sourced from the FitBit Fitness Tracker Data, which is a public domain dataset made available through Mobius. This Kaggle data set contains personal fitness tracker data from thirty eligible Fitbit users who consented to the submission of their personal tracker information. The data includes minute-level output for physical activity, heart rate, and sleep monitoring, providing a comprehensive overview of daily habits and routines of the users. This data will serve as the primary source of information for the analysis and help in exploring the habits and trends in smart device usage.

### Data Processing
The case study analysis is completed using RStudio due to the size of the data available

#### **Opening libraries**

```{r loading packages}

##Opening the libraries 
library("tidyverse")
library(dplyr)
library(ggplot2)
library(lubridate)
library(tidyr)
```

### Datasets
 We will focus on 11 of the 18 available csv files in the FitBit Fitness Tracker Data set for our analysis. These 11 files will be divided into two groups in order to find relationships. These groups will be daily tracking and hourly tracking, which will be further subdivided by activity, intensity, calories, steps, sleep and weight as the final data to add at the end of the analysis. This will provide us with a comprehensive understanding of the data and allow us to make meaningful insights and conclusions about the relationship between different aspects of fitness tracking.
 
 
* dailyActivity_merged.csv

* hourtyIntensities_merged.csv

* hourlycalories_merged.csv

* hourlySteps_merged.csv

* sleepDay_merged.csv

* weightLogInfo_merged.csv

```{r}
##Importing all the csv files we require for the analysis
dailyActivity <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
dailyCalories <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/dailyCalories_merged.csv")
dailyIntensities <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/dailyIntensities_merged.csv")
dailySteps <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/dailySteps_merged.csv")
hourlyCalories <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
hourlyIntensities <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
hourlySteps <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/hourlySteps_merged.csv")
minuteSleep <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/minuteSleep_merged.csv")
sleepDay <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
weightLogInfo <- read.csv("C:/Users/felip/OneDrive/FUTURO FELIPE/DATA ANALYST/TRABAJOS/CASO DE ESTUDIO 1/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
```

### Cheking Datos 

```{r}
# Check details of "dailyActivity" table

head(dailyActivity)

```

We identify columns of interest such as, "TotalSteps", "SedentaryMinutes", "Calories"

### Checking Unique data

```{r check unique columns }
# Check unique count in table

# To check the count of unique values for each column, we will use the n_distinct function and add the indicator [1,2] to identify the column to display.

# Check of ID and Days column in Activity table

colnames(dailyActivity)[1:2]
n_distinct(dailyActivity[,1])
n_distinct(dailyActivity[,2])

# Check of ID and Days column in Sleep table

n_distinct(sleepDay[,1])
n_distinct(sleepDay[,2])

# Check of ID column in other tables

n_distinct(hourlyCalories[,1])
n_distinct(hourlyIntensities[,1])
n_distinct(hourlySteps[,1])
n_distinct(weightLogInfo[,1])
```

 We are looking to determine the relationship between 33 IDs and 31 data days (ID sleep 24 and weight log info 8 ID) To do this, we will use data from the columns "TotalSteps", "SedentaryMinutes", "Calories", and "ActivityDate".
 
#### Create a Data Frame to hourly activitys like "dailyActivity"

```{r}
# Create using marge funcion HOURLY ACTIVITY

hourlyActivity <- merge(hourlyCalories, hourlyIntensities, by=c("Id", "ActivityHour"))
hourlyActivity <- merge(hourlyActivity, hourlySteps, by=c("Id", "ActivityHour"))

```

 Merging three datasets (hourlyCalories, hourlyIntensities, hourlySteps) into one by first merging common columns (Id, ActivityHour) using merge and then adding remaining columns (Calories, TotallIntensities, StepTotal) using cbind. Done in two steps for proper merging.

```{r}
glimpse(hourlyActivity)
```

But this is not enough, The previous syntax creates a data frame "hourlyActivity" which has a column "ActivityHour" in the format of "4/12/2016 1:00:00 AM" To find the best insights, we must configure this table in such a way that we can sort and filter the data with clear date and time references, such as days from Monday to Sunday and in a 24 hour format, so as to know the activity on the day of the week on average.

#### Transforming Data

```{r}
# first we going to use separate function on "activityhour" column.
hourlyActivity <- separate(hourlyActivity, ActivityHour, sep = " ",into = c("date", "time", "pm_am")) 


# We will now use a function called "weekdays" to create a new column called "weekday" containing the name of the day of the week.
hourlyActivity$date <- as.Date(hourlyActivity$date, "%m/%d/%Y")
hourlyActivity$weekday <- weekdays(hourlyActivity$date)

# To make it easier, we will delete the fields that we are no longer using.
hourlyActivity <- hourlyActivity[, -which(names(hourlyActivity) %in% c("AverageIntensity", "date"))]

#and voila!
head(hourlyActivity)

```

 aun no terminamos, necesitamos transformar estos datos en una nueva data frame, y calcular el promedio de los tres puntos "calorias" "intesidad", "pasos" y "time_sleep" tambien este ultimo campo debe ser agregado, para asi crear hermosos graficos ordenados y filtrados por los dias de la semana, las horas de los dias y las ids de las personas. 
 
 realizaremos los mismos procedimientos para los datos minutesSleep que nos entregara insaght a partes, 

```{r}
#transformar minutesleep en horas y agregar los dias de la semana 
  
  # first we going to use separate function on "activityhour" column.
  minuteSleep <- separate(minuteSleep, date, sep = " ",into = c("date", "time", "pm_am"))
  
  # crear dias de la semana
  minuteSleep$date <- as.POSIXct(minuteSleep$date, format = "%m/%d/%Y")
  minuteSleep$weekday <- weekdays(minuteSleep$date)
  
  # eliminar las columnas que no sirven
  minuteSleep <- minuteSleep[, -which(names(minuteSleep) %in% c("date", "value", "logId"))]
  
# se adaptaran las columnas "time" de ambos data frame "hourlyActivity" y "minuteSleep"para que coincidan asi importar los datos encontrados 

hourlyActivity$time <- substr(hourlyActivity$time, 1, nchar(hourlyActivity$time) - 6)
minuteSleep$time <- substr(minuteSleep$time, 1, nchar(minuteSleep$time) - 6)
    
```

Transformaremos los datos de la tabla hourlyActivity de tal manera de tener los promedios de las variables 

```{r}
avg_hourlyActivity <- aggregate(cbind(Calories, TotalIntensity, StepTotal) ~ Id + time + pm_am + weekday, data = hourlyActivity, mean)

# what's going on 
# Now, we use the aggregate function to group the data in our hourlyActivity data frame by Id, weekday, and time24. The aggregate function takes in three arguments: the formula, the data frame, and the function to apply to each group.

# In the formula, cbind(Calories, TotalIntensity, StepTotal) ~ Id + date + time + pm_am + weekday, we specify the columns we want to group by and calculate the mean of. The cbind function is used to combine columns into a single matrix. In this case, we are combining the columns Calories, TotalIntensity, and StepTotal into one matrix. The tilde symbol ~ separates the response variables from the predictor variables. The predictor variables Id, date, time, pm_am and weekday are the columns we want to group by.

# The second argument is the data frame, data = hourlyActivity, which we want to group. And the third argument is the function to apply to each group, mean, which calculates the mean of each group.

# The result is a new data frame grouped_data with the grouped and summarized data

head(avg_hourlyActivity)
```
 
### Visualization Variable Vs Time 

#### Calories vs Time
```{r}
ggplot(avg_hourlyActivity, aes(x = factor(time, levels = c(10, 11, 12, 1, 2, 3, 4, 5, 6, 7, 8, 9)), y = Calories, color = pm_am)) + 
  geom_line() + 
  xlab("Time") + 
  ylab("Calories") + 
  ggtitle("Calories vs Time Grouped by PM/AM") 
```


####  Intensity vs Time
```{r}
ggplot(avg_hourlyActivity, aes(x = factor(time, levels = c(10, 11, 12, 1, 2, 3, 4, 5, 6, 7, 8, 9)), y = TotalIntensity, color = pm_am)) + 
  geom_line() + 
  xlab("Time") + 
  ylab("Total Intensity") + 
  ggtitle("Total Intensity vs Time Grouped by PM/AM") 
```


####  Intensity vs Time
```{r}
ggplot(avg_hourlyActivity, aes(x = factor(time, levels = c(10, 11, 12, 1, 2, 3, 4, 5, 6, 7, 8, 9)), y = StepTotal, color = pm_am)) + 
  geom_line() + 
  xlab("Time") + 
  ylab("Step Total") + 
  ggtitle("Step Total vs Time Grouped by PM/AM")
```

### Visualization Variable Vs Days

```{r}
ggplot(avg_hourlyActivity, aes(x = weekday, y = Calories)) + 
  geom_line() + 
  xlab("Weekday") + 
  ylab("Calories") + 
  ggtitle("Calories vs Weekday")

ggplot(avg_hourlyActivity, aes(x = weekday, y = TotalIntensity)) + 
  geom_line() + 
  xlab("Weekday") + 
  ylab("Total Intensity") + 
  ggtitle("Total Intensity vs Weekday")
  
ggplot(avg_hourlyActivity, aes(x = weekday, y = StepTotal)) + 
  geom_line() + 
  xlab("Weekday") +
  ylab("Step Total") + 
  ggtitle("Step Total vs Weekday")
```

### Visualization Sleep Vs Time

```{r}
ggplot(minuteSleep, aes(x = factor(time, levels = c(10, 11, 12, 1, 2, 3, 4, 5, 6, 7, 8, 9)), fill = pm_am)) + 
  geom_bar(position = "dodge") + 
  facet_wrap(~ weekday, ncol = 2) + 
  xlab("Time") + 
  ylab("Frequency") + 
  ggtitle("Frequency of Time in the Range of 1 to 12 Grouped by PM/AM")

```

### Summarizing recommendations for the business

As we already know, collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.

After analyzing FitBit Fitness Tracker Data, I found some insights that would help influence Bellabeat marketing strategy.



### Target audience

Women who work full-time jobs (according to the hourly intensity data) and spend a lot of time at the computer/in a meeting/ focused on work they are doing (according to the sedentary time data).

These women do some light activity to stay healthy (according to the activity type analysis). Even though they need to improve their everyday activity to have health benefits. They might need some knowledge about developing healthy habits or motivation to keep going.

As there is no gender information about the participants, I assumed that all genders were presented and balanced in this data set.
The key message for the Bellabeat online campaign

The Bellabeat app is not just another fitness activity app. It’s a guide (a friend) who empowers women to balance full personal and professional life and healthy habits and routines by educating and motivating them through daily app recommendations.

### Ideas for the Bellabeat app

Average total steps per day are 7638 which a little bit less for having health benefits for according to the CDC research. They found that taking 8,000 steps per day was associated with a 51% lower risk for all-cause mortality (or death from all causes). Taking 12,000 steps per day was associated with a 65% lower risk compared with taking 4,000 steps. Bellabeat can encourage people to take at least 8 000 explaining the benefits for their health.

If users want to lose weight, it’s probably a good idea to control daily calorie consumption. Bellabeat can suggest some ideas for low-calorie lunch and dinner.

If users want to improve their sleep, Bellabeat should consider using app notifications to go to bed.

Most activity happens between 5 pm and 7 pm - I suppose, that people go to a gym or for a walk after finishing work. Bellabeat can use this time to remind and motivate users to go for a run or walk.

As an idea: if users want to improve their sleep, the Bellabeat app can recommend reducing sedentary time.
