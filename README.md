# CAMHS Waiting Time Analysis

## Introduction

Child and Adolescent Mental Health Services (CAMHS) provide assessment and treatment for children and young people under 18 who are struggling with their mental health. Early service delivery is key to producing positive outcomes, as it increases the likelihood of recovery and reduces the impact of mental health problems on the education and social development. In this project, I set out to analyse waiting times for CAMHS across 14 NHS Health Boards in Scotland.

## NHS Health Boards in Scotland

There are 14 NHS Health Boards in Scotland:

![NHS_HB_Scotland.png](/Resources/Images/NHS_HB_Scotland.png)

*NHS National Services Scotland. Service Map [Online]. Place of publication: Network for Inherited Cardiac Conditions Scotland. Available from: [URL](https://www.nn.nhs.scot/niccs/about-us/service-map) [Accessed: 03/09/2025]*

## Questions to Analyze

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
- `camhs-adjusted-patients-waiting` — monthly data on how long patients were waiting
- `referrals` — monthly referral counts

## 1️⃣ Do Health Boards meet their 18-week target for CAMHS?

Since December 2014, CAMHS in Scotland have had a target to deliver treatment within 18 weeks of referral for at least 90% of patients. But do the Health Boards manage to meet this target?

### 🔍 Skill: Power Query

I used Power Query to load three datasets: `camhs-adjusted-patients-seen`, `health_board_labels`, and `scotland_label`.

- For `camhs-adjusted-patients-seen`, I added index and date columns, changed column types, and removed unnecessary fields.

![patients_seen_info-query_steps.png](/Resources/Images/patients_seen_info-query_steps.png)

- For `health_board_labels`, I changed column types, appended the `scotland_label` dataset, and removed redundant columns and words.

![health_board_labels-query_steps.png](/Resources/Images/health_board_labels-query_steps.png)

### 🔗🧮 Skill: Power Pivot & DAX

- I created a data model by integrating the `camhs-adjusted-patients-seen` and `health_board_labels tables`, and added a `calendar table`.

![data_model_stage_1.png](/Resources/Images/data_model_stage_1.png)

- To account for patient volumes, I built a weighted calculation using DAX:

```
weighted_90th_median = [90th_percentile_weeks_patients_seen] * [total_patient_seen]
```

### 📊🧮 Skill: PivotTable & DAX

- I then created a PivotTable using the Power Pivot data model.
- I placed `health_board_name` in the rows area and `Date Hierarchy` in the columns area.
- I wrote a DAX measure to calculate the weighted annual average seen time (for 90% of patients):
  
```
Average Seen Time (for 9 in 10 patients) = SUM([weighted_90th_median]) / SUM([total_patient_seen])
```

- I then added this measure to the values area for analysis.

### 📊 Analysis

#### 💡 Insights

- There are clear differences between NHS Health Boards. Some, such as Western Isles, Ayrshire & Arran, and Dumfries & Galloway, meet the target quite consistently, while others, including Lothian, Fife, Boarders and Highland, often fall short.
- The remaining health boards show mixed performance, sometimes meeting the target but lacking consistency.
- At the national level, Scotland has never met the target on an annual basis, with the exception of the most recent year.

![18_week_target_dashboard.png](/Resources/Images/18_week_target_dashboard.png)

#### 🤔 So What

- It’s clear that children and adolescents in Scotland experience unequal access to timely care, with outcomes depending heavily on where they live. Despite the target being in place for over a decade, Scotland as a whole continues to fall short.

## 2️⃣ What are the trends in Health Boards performance over time?
