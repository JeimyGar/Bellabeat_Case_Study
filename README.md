# Bellabeat_Case_Study
## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](data-cleaning)
- [Exploratory Data Analysis](exploratory-data-analysis)
- [Data Analysis](data-analysis)
- [Results/Findings](results/findings)
- [Recommendations](recommendations)


Analyzing smart device fitness data to unlock new growth opportunities for Belllabeat. 

### Project Overview
Bellabeat is a high-tech company that manufactures health-focused smart devices for women. These smart devices collect data related to thier activity, sleep, stress, menstraul cycle and mindfulness habits. We are focusing on a bellabeat product and analyzing smart device usage data in order to gain ingiht into how people are already using their smart devices. With this infomration, we will give high-level recommendations for how these trends can inform bellabeat marketing strategy. 

### Data Sources
Bellabeat data: The primary datasets used for this analysis are the "Firbit Fitness Tracker Data" from Kaggle that can be found [here](https://www.kaggle.com/datasets/arashnic/fitbit). This dataset was generated between 03.12.2016 and 05.12.2016 and included minute-level output for physical activity, heart rate, and sleep monotoring for 30 individials. There were some limitations to this data as we did not know the age or gender of any of these participants. All 30 participants also did not submit their information in all categories and for all of the days of the dataset. 

### Tools

- Excel - Data Cleaning, Pivot Chart 
- SQL Bigquery - Data Analysis
- Tableau - Creating Reports
- 
### Data Cleaning 
In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection. 
2. Handling missing values.
3. Rounding numbers
4. Checking for duplicates
5. Made sure colum header descriptions are descriptive and unique
6. Data cleaning and formatting.

### Exploratory Data Analysis
EDA involved exploring the sales data to answer key questions, such as:
2. What Bellabeat feature is mostly used
3. What bellabeat feature in used the least
5. Calorie count to see what time of day id most active
6. Minutes for different minute level output for physical activity
7. Activity levels compared to BMI *
10. Days of the week most people use the device (calorie count) *
11. See BMI to see what body type od most common with sample of users *

### Data Analysis

The Data analysis was done in excel and SQL 

#### Excel
In Excel,  I cleaned and transformed the data. I ensured that there were no dublicates, nulls or outliers. I removed data that I was not working with such as: total Distance, Trackerdistance, loggedActivity, VeryActiveDistance, LightActiveDistance, ModeratelyActiveDistance, and SedentaryActiveDistance. From the weightloginfo data I erased the Weight KG and the Fat as well as the LogId. To standardize the numbers I rounded the number in the "TotalDistance", "TrackerDistance”, “VeryAcvtiveDistance”, “ModeratelyActiveDistance”, “sedentartyActiveDistance” zero decimal points. I made sure that the formatting was cinsistnent and that the column header descriptions were descriptive and unique. I created summary statistics (mean, median, standard deviation) for numeric columns. I also Extracted time from Columns with datet and time using =TEXT(B2, “hh:mm:ss AM/PM”) to make time easier to work with. 

###SQL In SQL u uploaded dafsdfasd to do exploratory data analysis. 

The first query the I made was to firgure out the average calorie count per day of the week. I made a common table expression named daysofweek that calculated the average calories per day of the week. Then I referenced the day of the week alias in the outer query to display the day names.
``` SQL
WITH daysofweek AS (
  SELECT
    EXTRACT(DAYOFWEEK FROM ActivityHour) AS dayofweek,
    ROUND(AVG(calories), 2) AS Calorie_Count
  FROM valid-bedrock-431220-f1.Fitbit.hourlyCalories_merged
  GROUP BY 1
)
SELECT
  CASE
    WHEN dayofweek = 1 THEN 'Sunday'
    WHEN dayofweek = 2 THEN 'Monday'
    WHEN dayofweek = 3 THEN 'Tuesday'
    WHEN dayofweek = 4 THEN 'Wednesday'
    WHEN dayofweek = 5 THEN 'Thursday'
    WHEN dayofweek = 6 THEN 'Friday'
    WHEN dayofweek = 7 THEN 'Saturday'
  END AS Weekday, Calorie_Count
FROM daysofweek;
```

Then I wanted to figure out the users steps compared to thier BMI. There were some limitations with this data as we do not know age or height of participlants. This would leave us with queations on whether BMI is an accurate calulcation of the indiviuals health and body type. Here we used an inner join to get ID that are in both tables.
```SQL
SELECT daily.id, ROUND(AVG(weight.BMI),0) AS Average_BMI, SUM(daily.totalsteps) AS Total_Steps
FROM valid-bedrock-431220-f1.Fitbit.DailyActivity_merged AS daily
INNER JOIN valid-bedrock-431220-f1.Fitbit.weightLoginfo AS weight
ON daily.id = weight.id
GROUP BY 1
ORDER BY 2 DESC
```
### Results/Findings

The analysis results are summarized as follows:
1. This bar graph displays the total step count on the y-axis and the weekdays on the x-axis. Each day of the week has the total sum of steps taken for that day throught the whole duration of the data. We can see that there is a spike of activity on saturday and as the week progresses the activity seems to slow down. 

![bella1](https://github.com/user-attachments/assets/c317dd0b-6f87-44b8-9bc2-eaf6d9ff51f0)

2. This Pie chart shows the break down of proportion in activity level minutes. The activity levels here mentioned are lightly active, very active and fairly active. These activity levels are determined by monitoring heart rate. As we can see, most user opt for light activities.   
![bella2](https://github.com/user-attachments/assets/b09bdca7-ca90-4c7e-b30b-eefc70959da6)

3. In this bar box and whisker plot I plotted the weight distribution of the participants. We can see that the average weight for these is 176 lbs. The interquartile range is from 200 to 138 lbs with a max weight of 286 and a min weight of 118.

 ![bella3](https://github.com/user-attachments/assets/9dd4ce56-aeb6-4a35-90a6-7ad18e6b91a5)
 
4. In this scatterplot titled "Step Count vs. BMI" we can see the relationship between body mass index (BMI) and step count. We can see that there is a negative correlation between BMI and step count. This would lead us to believe that as BMI increases, step count tends to decrease. There are some outliers in this data.
 ![bella 4](https://github.com/user-attachments/assets/516e2fc8-1498-4d96-821b-c953b9902505)
5 In excel I added up the distinct ids for the users who uploaded info for heart rate, sleep log, and weight log. There were 30 total users in the dataset. As we can see, 24 out of the 30 users input their data at some point. The data with the least imput is heart_rate with only 10 out of the 30 users submitting their data. 
<img width="319" alt="excel" src="https://github.com/user-attachments/assets/d97e6bc1-839d-4c27-8cd7-3b473231081d">

### Recommendations
Based on the results
