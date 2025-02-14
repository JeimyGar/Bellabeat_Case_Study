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


Analyzing smart device fitness data to unlock new growth opportunities for Bellabeat. 

### Project Overview
Bellabeat is a high-tech company that manufactures health-focused smart devices for women. These smart devices collect data on  activity, sleep, stress, menstrual cycle, and mindfulness habits. I am analyzing smart device data to understand how people already use their smart devices. With this information, I will give recommendations for how these trends can transform Bellabeats marketing strategies.  

### Data Sources
The primary dataset used for this analysis is the "Fitbit Fitness Tracker Data" from Kaggle, which can be found [here](https://www.kaggle.com/datasets/arashnic/fitbit). This dataset was generated between 03.12.2016 and 05.12.2016. It consisted of 30 Fitbit users who consented to submit personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. A distinct ID or timestamp can distinguish individual reports. This data had some limitations as we did not know the age or gender of any of these participants.

### Tools

- Excel - Data Cleaning, Pivot Chart 
- SQL Bigquery - Data Analysis
- Tableau - Creating Reports
  
### Data Cleaning 
In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection. 
2. Handled missing values.
3. Rounded lengthy numbers.
4. Checked for duplicates.
5. Made sure column header descriptions were descriptive and unique.

### Exploratory Data Analysis
EDA involved exploring the Fitbit data to answer key questions, such as:
1. What Fitbit feature is mainly used?
2. What Fitbit feature is used the least?
3. When are users the most and least active?
4. What type of activity are users spending their time on?
5. How does BMI impact user frequency?
6. How do users' activity levels impact how the features are used?

### Data Analysis

The data analysis was done using Excel and SQL. 

#### Excel
In Excel,  I cleaned and transformed the data. I ensured that there were no duplicates, nulls, or outliers. I removed data that I was not working with such as: ‘total Distance’, ‘Trackerdistance’, ‘loggedActivity’, ‘VeryActiveDistance’, ‘LightActiveDistance’, ‘ModeratelyActiveDistance’, and ‘SedentaryActiveDistance’. To standardize the numbers, I rounded the number in the ‘TotalDistance’, ‘TrackerDistance’, ‘VeryAcvtiveDistance’, ‘ModeratelyActiveDistance’, and ‘sedentartyActiveDistance’ zero decimal points. I ensured the formatting was consistent and the column header descriptions were descriptive and unique. I created summary statistics (mean, median, standard deviation) for the numeric columns. I also extracted time from columns with date and time information using =TEXT(B2, “hh:mm:ss AM/PM”) to make it simpler to work with.   

#### SQL 
I uploaded our Fitbit data in SQL to do exploratory data analysis. The queries are shown and explained below, and the results from these queries are presented and explained in the results/findings section.

My first query was to calculate the average and total number of steps per day of the week. Using a CTE, I first grouped the data by day of the week and by the average and total steps. Then, I used a case statement to convert the numeric days of the week into their corresponding weekday names. This is important information to see what days users are most and least active.  
``` SQL
WITH daysofweek AS (
  SELECT
    EXTRACT(DAYOFWEEK FROM ActivityHour) AS dayofweek,
    ROUND(AVG(StepTotal), 0) AS Average_Steps, SUM(steptotal) AS Total_Steps
  FROM `valid-bedrock-431220-f1.Fitbit.hourlySteps_merged `
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
  END AS Weekday, Average_Steps, Total_Steps
FROM daysofweek;
```

Next, I wanted to determine how the users' steps compared to their BMI. We do have to remember that there were limitations with this data as we do not know the age, height, or gender of participants. This is important because BMI is not always an accurate calculation of an individual's health and body type. In this query, we gathered the the average BMI rounded to the nearest whole number and the sum of ‘totalsteps’.  I then joined two tables using an inner join to ensure that only matching records from both tables were included. 
```SQL
SELECT daily.id, ROUND(AVG(weight.BMI),0) AS Average_BMI, SUM(daily.totalsteps) AS Total_Steps
FROM valid-bedrock-431220-f1.Fitbit.DailyActivity_merged AS daily
INNER JOIN valid-bedrock-431220-f1.Fitbit.weightLoginfo AS weight
ON daily.id = weight.id
GROUP BY 1
ORDER BY 2 DESC
```
Here, we get the average weight and average BMI for the users who submitted data. Since some of the users input their weight more than once, I got the average weight for each user. This weight was also rounded to the nearest whole number, as was the average BMI. Again, we have limitations because we don't know height, weight, or other demographics. 

```SQL
SELECT Id, Ismanualreport, ROUND(AVG(weightpounds),0) AS average_weight, ROUND(AVG(BMI),0) as Average_BMI
FROM valid-bedrock-431220-f1.Fitbit.weightLoginfo
GROUP BY 1,2;
```
Lastly, this next query is to see the total minutes of each activity level. I calculated the total number of minutes for each activity level and gave it an alias. 
```SQL
SELECT SUM(VeryActiveMinutes) AS Total_VeryActive_Minutes, SUM(FairlyActiveminutes) AS Total_FairlyActive_Minutes, SUM(LightlyActiveminutes) AS Total_LightlyActive_Minutes, SUM(sedentaryminutes) AS Total_Sedentary_Minutes, SUM(calories) AS Total_Calories_Burned
FROM valid-bedrock-431220-f1.Fitbit.DailyActivity_merged;
```
### Results/Findings

The analysis results are summarized as follows:
1. This bar graph displays the total step count on the y-axis and the weekdays on the x-axis. Each day of the week has the total sum of steps taken for that day throughout the whole duration of the data set. We can see that there is a spike of activity on saturday and as the week progresses the activity seems to slow down. 

![bella1](https://github.com/user-attachments/assets/c317dd0b-6f87-44b8-9bc2-eaf6d9ff51f0)

2. This Pie chart shows the breakdown of how much time is spent per each activity level. The activity levels here mentioned are: lightly active, very active and fairly active. These activity levels are determined by monitoring heart rate. As we can see, most users spend their time doing light activities.    
![bella2](https://github.com/user-attachments/assets/b09bdca7-ca90-4c7e-b30b-eefc70959da6)

3. In this bar box and whisker plot I plotted the weight distribution of the participants. We can see that the average weight for these is 176 lbs. The interquartile range is from 200 to 138 lbs with a max weight of 286 and a min weight of 118.

 ![bella3](https://github.com/user-attachments/assets/9dd4ce56-aeb6-4a35-90a6-7ad18e6b91a5)
 
4. In this scatterplot titled "Step Count vs. BMI" we can see the relationship between body mass index (BMI) and step count. We can see that there is a negative correlation between BMI and step count. This would lead us to believe that as BMI increases, step count tends to decrease. There are some outliers in this data.
 ![bella 4](https://github.com/user-attachments/assets/516e2fc8-1498-4d96-821b-c953b9902505)
5 In excel I added up the distinct ids for the users who uploaded info for heart rate, sleep log, and weight log. There were 30 total users in the dataset. As we can see, 24 out of the 30 users input their data at some point. The data with the least information is heart_rate with only 10 out of the 30 users submitting their data. 
<img width="319" alt="excel" src="https://github.com/user-attachments/assets/d97e6bc1-839d-4c27-8cd7-3b473231081d">

### Recommendations
Based on the results there are a few things that stand out. 

- Weekly Motivation: Based on the weekly activity chart that I created, we can see that Saturday is the most active day of the week. As the weekday progresses, the activity levels decrease. To combat this decrease in activity, we can implement several different things. We can launch special week challenges and rewards to boost participation. This would boost engagement in current and new users. We can also promote a feature where users can set goals and earn rewards, badges and discounts for achieving their milestones
- Activity Motivation: The data collected shows that most of the activity intensity levels are spent doing light activity. Bellabeat can promote activity level monitoring and encourage users to set activity level goals that the Bellabeat devices monitor and reward you on.
- Personalized Insights: There seems to be a negative correlation between BMI and step count. We can use this to offer tailored fitness and nutrition plans based on BMI and activity levels for a BMI goal or activity goal the users can set. 
- Heart rate and sleep tracking: Out of 30 users 24 logged sleep and 10 logged heart rate. We can increase the number of users by emphasizing the importance of sleep and heart rate monitoring. We can also promote insight and data summarization tools to encourage consistent use.
- Community Engagement: Based on all the data that is collected we can foster a sense of community by creating social groups where users can share their progress, achievements and rewards. This can encourage people and keep them motivated.

