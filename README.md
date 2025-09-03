# CAMHS Waiting Time Analysis

## Introduction

Child and Adolescent Mental Health Services (CAMHS) provide assessment and treatment for children and young people under 18 who are struggling with their mental health. Early service delivery is key to producing positive outcomes, as it increases the likelihood of recovery and reduces the impact of mental health problems on the education and social development. In this project, I set out to analyse waiting times for CAMHS across 14 NHS Health Boards in Scotland.

## NHS Health Boards in Scotland

There are 14 NHS Health Boards in Scotland:

![NHS_HB_Scotland.png](/Resources/Images/NHS_HB_Scotland.png)

*NHS National Services Scotland. Service Map [Online]. Place of publication: Network for Inherited Cardiac Conditions Scotland. Available from: [URL](https://www.nn.nhs.scot/niccs/about-us/service-map) [Accessed: 03/09/2025]*

## Questions to Analyze

To understand how CAMHS perform in Scotland, I asked the following:

- Do Health Boards meet their 18-week target for CAMHS?
- What are the trends in Health Boards performance over time?
- Do more referrals mean longer waiting time?
- How many children and adolescents are waiting longer than recommended in the most recent month available?

## Excel Skills Used
The following Excel skills were utilised for analysis:

- 📊 Pivot Tables
- 📈 Pivot Charts
- 🧮 DAX (Data Analysis Expressions)
- 🔍 Power Query
- 🔗 Power Pivot

## Waiting Times Dataset

The data for this project was obtained from [Public Health Scotland](https://www.opendata.nhs.scot/dataset/child-and-adolescent-mental-health-waiting-times).

## 1️⃣ Do Health Boards meet their 18-week target for CAMHS?

Since December 2014, CAMHS in Scotland have had a target to deliver treatment within 18 weeks of referral for at least 90% of patients. But do the Health Boards manage to meet this target?

### 🔍 Skill: Power Query 

I used Power Query to load two datasets: `camhs-adjusted-patients-seen`, with monthly data on how long 90% of patients waited from referral to their first treatment, and `health_board_labels`, with each health board’s name and unique identifier.

I transformed the `camhs-adjusted-patients-seen` query by adding index and date columns, changing column types, and removing unnecessary columns:

![NHS_HB_Scotland.png](/Resources/Images/NHS_HB_Scotland.png)

Similarly, I transformed the `health_board_labels` query by changing column types, appending it with the `scotland_label dataset`, removing unnecessary columns, and eliminating specific words:



### 🔗 Skill: Power Pivot
