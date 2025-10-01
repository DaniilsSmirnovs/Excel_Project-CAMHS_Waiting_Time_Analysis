# CAMHS Waiting Time Analysis

## Introduction

Child and Adolescent Mental Health Services (CAMHS) provide assessment and treatment for children and young people under 18 who are struggling with their mental health. Early service delivery is key to producing positive outcomes, as it increases the likelihood of recovery and reduces the impact of mental health problems on the education and social development. In this project, I set out to analyse waiting times for CAMHS across 14 NHS Health Boards in Scotland.

## NHS Health Boards in Scotland

There are 14 NHS Health Boards in Scotland:

![NHS_HB_Scotland.png](/Resources/Images/NHS_HB_Scotland.png)

*NHS National Services Scotland. Service Map [Online]. Place of publication: Network for Inherited Cardiac Conditions Scotland. Available from: [URL](https://www.nn.nhs.scot/niccs/about-us/service-map) [Accessed: 03/09/2025]*

## Questions to Analyse

To understand how CAMHS perform in Scotland, I asked the following:

1. Do Health Boards meet their 18-week target for CAMHS?
2. What are the trends in Health Boards performance over time?
3. Do more referrals mean longer waiting time?
4. How many children and adolescents are waiting longer than recommended in the most recent month available?

## Excel Skills Used
The following Excel skills were utilised for analysis:

- 📊 PivotTable
- 📈 PivotChart
- 🧮 DAX (Data Analysis Expressions)
- 🔍 Power Query
- 🔗 Power Pivot

## Waiting Times Datasets

The data for this project were obtained from [Public Health Scotland](https://www.opendata.nhs.scot/dataset/child-and-adolescent-mental-health-waiting-times). The analysis used five datasets:

- `camhs-adjusted-patients-seen` — monthly data on how long 90% of patients waited from referral to their first treatment
- `health_board_labels` — names and unique identifier codes for each Health Board
- `scotland_label` — Scotland-level identifier code
- `referrals` — monthly referral counts
- `camhs-adjusted-patients-waiting` — monthly data on how many patients are still waiting for their first treatment

## 1️⃣ Do Health Boards meet their 18-week target for CAMHS?

Since December 2014, CAMHS in Scotland have had a target to deliver treatment within 18 weeks of referral for at least 90% of patients. But do the Health Boards manage to meet this target?

### 🔍 Skill: Power Query

I used Power Query to load three datasets: `camhs-adjusted-patients-seen`, `health_board_labels`, and `scotland_label`.

- For `camhs-adjusted-patients-seen`, I added index and date columns, changed column types, and removed unnecessary fields.

![patients_seen_info-query_steps.png](/Resources/Images/patients_seen_info-query_steps.png)

- For `health_board_labels`, I changed column types, appended the `scotland_label` dataset, and removed redundant columns and words.

![health_board_labels-query_steps.png](/Resources/Images/health_board_labels-query_steps.png)

### 🔗🧮 Skill: Power Pivot & DAX

- I created a data model by integrating the `camhs-adjusted-patients-seen` and `health_board_labels` tables, and added a `calendar table`.

![data_model_stage_1.png](/Resources/Images/data_model_stage_1.png)

- I also built a weighted calculation of waiting time (for 90% of patients) using DAX:

```
weighted_90th_median = [90th_percentile_weeks_patients_seen] * [total_patient_seen]
```

- I will use this calculation to determine the annual average seen time (for 90% of patients), taking into account that different months have different numbers of referrals.

### 📊🧮 Skill: PivotTable & DAX

- I then created a PivotTable using the Power Pivot data model.
- I placed `health_board_name` in the rows area and `Date Hierarchy` in the columns area.
- I wrote a DAX measure to calculate the weighted annual average seen time (for 90% of patients):
  
```
Average Seen Time (for 9 in 10 patients) = SUM([weighted_90th_median]) / SUM([total_patient_seen])
```

- I then added this measure to the values area for analysis.
- 🚦 I applied conditional formatting to highlight the years and months when the 18-month target was met, as well as when the average waiting time for 90% of patients exceeded one year. 

### 📊 Analysis

#### 💡 Insights

- There are clear differences between NHS Health Boards. Some, such as Western Isles, Ayrshire & Arran, and Dumfries & Galloway, meet the target quite consistently, while others, including Lothian, Fife, Boarders and Highland, often fall short.
- The remaining health boards show mixed performance, sometimes meeting the target but lacking consistency.
- At the national level, Scotland has never met the target on an annual basis, with the exception of the most recent year.

![18_week_target_dashboard.png](/Resources/Images/18_week_target_dashboard.png)

#### 🤔 So What

- It’s clear that children and adolescents in Scotland experience unequal access to timely care, with outcomes depending heavily on where they live. Despite the target being in place for over a decade, Scotland as a whole continues to fall short.

## 2️⃣ What are the trends in Health Boards performance over time?

### 📈🧮 Skill: PivotChart & DAX

- I created a combo PivotChart to plot the median seen time for a selected Health Board, the median seen time across all of Scotland, and the target seen time.
  - Lines: Median seen time for a selected Health Board and median seen time across all of Scotland
  - Area: Target seen time
- To customise the chart, I added an axis title, a slicer, and a timeline.

- To create a target seen time measure, I wrote the following DAX formula:

```
Target Seen Time = IF(MAX('patients_seen_info'[date]) >= DATE(2014,12,1), 18,
                      IF(ISBLANK(MAX('patients_seen_info'[date])), BLANK(), 26))
```

### 📊 Analysis

#### 💡 Insights

- The Covid-19 pandemic had a clear impact on access to CAMHS services, with waiting times generally increasing between years 2020 and 2022, before beginning to improve from 2023 onwards.
- NHS Health Boards varied in how they coped during the pandemic: some, such as Forth Valley, Greater Glasgow and Clyde, and Lanarkshire, saw sharp rises in waiting times, while others, including Grampian, Tayside, and Shetland, were able to maintain performance and meet the 18-week target.
- Although many Health Boards have improved since the pandemic, some — particularly Lothian and Highland — continue to face long waiting times.

![HB_performance_over_time.gif](/Resources/Images/HB_performance_over_time.gif)

#### 🤔 So What

- The pandemic highlighted differences in preparedness across NHS Health Boards in Scotland to manage disruptions to their services, with some being more resiliant than others.

## 3️⃣ Do more referrals mean longer waiting time?

### 🔍🔗 Skill: Power Query & Power Pivot

I used Power Query to load the forth dataset – `referrals` and added index and date columns, changed column types, and removed unnecessary fields:

![referrals-query_steps.png](/Resources/Images/referrals-query_steps.png)

I then added the new dataset to the existing data model:

![data_model_stage_2.png](/Resources/Images/data_model_stage_2.png)

### 📊 Analysis

#### 💡 Insights

- There is a positive correlation between the median number of referrals received by each Health Board per month and the median seen time (for 9 in 10 patients).
- However, there are notable exceptions — in particular, Lothian and Greater Glasgow and Clyde.
- Lothian has by far the highest median seen time and deviates strongly from the correlation line, even when accounting for its high monthly referral numbers.
- Greater Glasgow and Clyde, on the other hand, despite having the highest number of referrals, has a median seen time lower than some Health Boards that receive at least five times fewer referrals.

![referrals_and_seen_time.png](/Resources/Images/referrals_and_seen_time.png)

#### 🤔 So What

- The number of referrals and how busy the Health Board is not the only factor that explains the seen time and some health boards are managing extremely well despite having really high volume of patients while others fail short.

## 4️⃣ How many children and adolescents are waiting longer than recommended in the most recent month available?

### 🔍🔗 Skill: Power Query & Power Pivot

I used Power Query to load the fifth and final dataset, amhs-adjusted-patients-waiting, and added index and date columns, changed column types, and removed unnecessary fields:

![camhs_adjusted_patients_waiting-query_steps.png](/Resources/Images/camhs_adjusted_patients_waiting-query_steps.png)

I then added the new dataset to the existing data model:

![data_model_stage_3.png](/Resources/Images/data_model_stage_3.png)

### 🧮 Skill: DAX

- I calculated the number of minors waiting longer than recommended in the latest month available using DAX for each waiting category:
  1. Waiting 19–35 Weeks
  2. Waiting 36–52 Weeks
  3. Over 52 Weeks
  
```
Number of Minors Waiting 19-35 Weeks = CALCULATE(
     MEDIAN('patients_waiting_info'[19-35_weeks]),
     FILTER(
        'patients_waiting_info',
        'patients_waiting_info'[date] = MAX('patients_waiting_info'[date])
    )
)
```

### 📊 Analysis

#### 💡 Insights

- The three Health Boards — Lothian, Lanarkshire, and Highland — appear consistently across all three categories.
- Lothian has the highest number of minors waiting longer than recommended.
  
![minors_waiting_longer_than_recommended.png](/Resources/Images/minors_waiting_longer_than_recommended.png)

#### 🤔 So What

- The findings suggest that targeted support is needed in Lothian Health Board, where the number of minors waiting beyond recommended times is considerably higher than elsewhere.

# Conclusion

This analysis highlights the ongoing challenges faced by Child and Adolescent Mental Health Services (CAMHS) across Scotland. Despite a national target that has been in place for over a decade, access to timely care remains highly variable between NHS Health Boards. Some Boards consistently meet the 18-week target, while others, most notably Lothian and Highland, continue to experience long waiting times.

The impact of external pressures such as the Covid-19 pandemic further emphasised differences in resilience and service delivery across regions. While referral numbers are linked to waiting times, they do not fully explain performance; several Health Boards demonstrate that it is possible to manage high patient volumes while maintaining reasonable waiting times.

The findings also show that a considerable number of children and adolescents are waiting well beyond the recommended thresholds for care, particularly in Lothian, where additional and targeted support appears most needed.

Overall, the results underscore the importance of consistent monitoring and data-driven decision-making in ensuring equitable access to timely mental health support for all young people in Scotland, regardless of where they live.
