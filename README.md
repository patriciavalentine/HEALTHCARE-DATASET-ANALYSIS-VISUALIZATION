# 📊 HEALTHCARE PERFORMANCE ANALYSIS
## Project Overview
This Data Analysis & Visualization project aims to provide insights into the Healthcare performance of several health facilities in the United States.

By analyzing and visualizing various parts of the healthcare data, I sought to identify trends and gain a deeper understanding of the healthcare performance by analysing information about the healthcare systems' patients' names, ages, genders, medical conditions, insurance providers, blood groups, billing amounts, doctors, days of admission and hospitals attended.

### Data Source
The primary dataset used for this analysis is the ['heathcare_dataset.csv'](https://github.com/patriciavalentine/HEALTHCARE-PERFORMANCE-ANALYSIS/blob/main/healthcare_dataset.csv) file, containing detailed information about the healthcare systems.

### Tools
- *Microsoft SQL Server - Data Cleaning & Analysis.*
- *PowerBI - Data Visualizations.*

## 📑 Insights
- Asthma was more prevalent among males, while arthritis was more frequent among females.
- The doctor-to-patient ratio was favorable, with a slightly higher number of doctors compared to patients.
- There was a proportional distribution between the number of hospitals and patient count.
- Seniors constituted the largest patient age group across most medical conditions, followed by adults, young adults, and children.
- The average billing amount per insurance provider demonstrated significant variability, contingent on the medical condition.

I consolidated and presented several key insights in [this dashboard](https://github.com/patriciavalentine/HEALTHCARE-PERFORMANCE-ANALYSIS/blob/main/Dashboard%20Capture.PNG).

## THE PROCESS

![confident-doctor-hospital-room](https://github.com/user-attachments/assets/10f4356a-8b79-4157-8e07-26e61b84b111)

## Table of Contents
1. [Data Cleaning and Preparation](#data-cleaning-and-preparation)
2. [Exploratory Data Analysis](#exploratory-data-analysis)
3. [Data Visualizations](#data-visualizations)
4. [Findings](#findings)
5. [Conclusion](#conclusion)

### Data Cleaning and Preparation
In the initial data preparation phase, I performed the following tasks:
1. Data loading & inspection.

```sql
SELECT *
FROM Healthcare_Dataset

SELECT TOP 10 *
FROM Healthcare_Dataset
-- Retrieved the first 10 rows

SELECT COUNT(*)
FROM Healthcare_Dataset
-- Counted total rows
```

2. Checking missing values and blanks.
```sql
SELECT *
FROM Healthcare_Dataset
WHERE [Name] LIKE '% %'
   OR [Name] IS NULL
   OR [Name] = ''
--Checked for Empty Values.

DELETE FROM Healthcare_Dataset
WHERE [Name] IS NULL
```

3. Data cleaning & formatting.

```sql
ALTER TABLE Healthcare_Dataset
DROP COLUMN [Room Number], [Test Results]
--Deleted some Irrelevant Columns.


UPDATE Healthcare_Dataset
SET [Name] = UPPER([Name])
--Formatted the patient Names into Uppercase; since most were not in a uniform format.

UPDATE Healthcare_Dataset
SET [Billing Amount] = ROUND([Billing Amount], 0)
--Removed the several decimals by Converting Billing Amount to the nearest Whole Number.

UPDATE Healthcare_Dataset
SET [Name] = LTRIM(RTRIM([Name])),
    [Doctor] = LTRIM(RTRIM([Doctor])),
    [Hospital] = LTRIM(RTRIM([Hospital]))
--Remove leading and trailing spaces from the columns.

ALTER TABLE Healthcare_Dataset
ADD AgeGroup VARCHAR(20)
UPDATE Healthcare_Dataset
SET AgeGroup = CASE
    WHEN Age < 18 THEN 'Child'
    WHEN Age BETWEEN 18 AND 35 THEN 'Young Adult'
    WHEN Age BETWEEN 36 AND 55 THEN 'Adult'
    ELSE 'Senior'
END;
--Added an extra column 'AgeGroup'; to help with further queries.
```

### Exploratory Data Analysis
EDA involved exploring the healthcare data to answer key questions, such as:
1. Is there a correlation between gender and specific medical conditions?
2. What are the most common medical conditions across different age groups?
3. Which doctors are preferred for specific conditions?
4. What is the average billing amount by each insurance provider?

```sql
--QUERIES FOR THE DATASET:
--For the Gender and Medical Conditions:
SELECT [Gender], [Medical Condition], COUNT(*) AS ConditionCount
FROM Healthcare_Dataset
GROUP BY [Gender], [Medical Condition]
ORDER BY [Medical Condition] ASC

--For the Medical Conditions Across Age Groups:
SELECT [AgeGroup], [Medical Condition], COUNT(*) AS ConditionCount
FROM Healthcare_Dataset
GROUP BY [AgeGroup], [Medical Condition]
ORDER BY ConditionCount DESC

--For the Doctors Preferred for Specific Conditions:
SELECT [Doctor], [Medical Condition], COUNT(*) AS PatientCount
FROM Healthcare_Dataset
GROUP BY [Doctor], [Medical Condition]
HAVING COUNT(*) >= 5
```

![Healthacare EDA Execute SQL Capture2](https://github.com/user-attachments/assets/70801ed3-8fa6-46cb-b549-0d14365c1c8b)

### Data Visualizations
The data visualization of the analyzed data is shown in the interactive Dashboard below:

![Healthcare Default Dashboard Capture](https://github.com/user-attachments/assets/24f1c773-6922-45ec-a378-78c4e2811a24)

To filter and explore the dashboards from different perspectives, download the PowerBI visualization [here](https://github.com/patriciavalentine/HEALTHCARE-PERFORMANCE-ANALYSIS/blob/main/Healthcare_Dataset%20Visualizations.pbix).

### Findings
1. Correlation Between Gender and Medical Conditions:

Observation: A slight relationship was observed between gender and specific medical conditions. Asthma had a higher prevalence among males, while arthritis was more common among females. For other medical conditions, the distribution between genders was almost identical.

Insight: This suggests potential gender-specific risk factors or susceptibilities for certain conditions, particularly for asthma and arthritis.

2. Doctor-to-Patient Ratio:

Observation: The analysis revealed that the number of doctors was slightly higher than the number of patients.

Insight: A favorable doctor-to-patient ratio is indicated, which can positively impact patient care quality and reduce wait times. This ratio is a good indicator of resource availability in the healthcare system.

3. Hospital-to-Patient Proportion:

Observation: The number of hospitals was found to be proportionate to the number of patients.

Insight: This proportionality suggests an adequate distribution of healthcare facilities relative to the patient population, which is critical for maintaining accessible healthcare services.

4. Age Group Distribution Across Medical Conditions:

Observation: Seniors represented the largest age group among patients across almost all medical conditions, followed by adults, young adults, and children.

Insight: This age distribution highlights the greater healthcare needs of senior populations, likely due to age-related health issues. The data emphasizes the importance of geriatric care and resources within the healthcare system.

5. Insurance Billing Analysis:

Observation: The average billing amount by insurance provider varied significantly, depending on the medical condition.

Insight: This variance indicates that certain medical conditions require more expensive treatments or longer hospital stays, leading to higher billing amounts. It also suggests that insurance plans may need to be tailored to better manage the costs associated with specific conditions.

### Conclusion
The analysis of the healthcare dataset provided valuable insights into the relationships between patient demographics, medical conditions, and healthcare resources.

The slight gender differences in condition prevalence, the favorable doctor-to-patient ratio, and the proportionality of hospitals to patients all contribute to an understanding of the healthcare landscape. Additionally, the significant differences in average billing amounts by insurance provider underscore the need for careful consideration of healthcare costs associated with different medical conditions.

These findings can inform strategic decisions in healthcare management and policy-making.

*This comprehensive analysis process highlights my ability to effectively clean, prepare, and analyze complex datasets using SQL, and to translate these insights into meaningful visualizations using Power BI.*

💙
