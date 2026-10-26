# ANALYZING STUDENTS' MENTAL HEALTH
An exploratory data analysis project from DataCamp investigating the relationship between international students' length of stay and their mental health diagnostic scores using SQL.

**Project Source:** [DataCamp — Analyzing Students' Mental Health](https://app.datacamp.com/learn/projects/analyzing_students_mental_health)

## 📌 Business Problem
International students may experience psychological and social challenges while adjusting to a new country. Understanding how mental health indicators vary with their length of stay can help identify patterns that warrant further investigation and inform discussions about student support services.

This project examines whether the length of stay is associated with differences in three indicators among international students:
- Depression: PHQ-9 (todep)
- Social Connectedness: SCS (tosc)
- Acculturative Stress: ASISS (toas)

The analysis focuses on identifying patterns across different lengths of stay, rather than establishing a causal relationship between time abroad and mental health.

## 🎯 Objectives & Requirements
- Calculate the number of international students in each length-of-stay group.
- Compare average depression, social connectedness, and acculturative stress scores across groups.
- Summarize the results in a structured table using SQL aggregation and sorting.

## 🛠️ Approach
The analysis was performed in a DataCamp Workspace notebook using a pre-loaded dataset containing 272 records. The SQL workflow involved:
1. **Filter International Students**: Utilize the `WHERE` clause (`inter_dom = 'Inter'`) to isolate international students from domestic students in the study.
2. **Group Cohorts**: Apply `GROUP BY` on the `stay` column to organize the student data into distinct cohorts based on their length of stay in years.
3. **Count Observations**: Use `COUNT(inter_dom)` aliased as `count_int` to find the exact sample size of international students for each duration of stay.
4. **Calculate & Round Averages**: Apply the **AVG()** function on the mental health diagnostic test scores—`todep` (PHQ-9 test for depression), `tosc` (SCS test for social connectedness), and `toas` (ASISS test for acculturative stress)—and wrap them in `ROUND(..., 2) `to format the results to two decimal places.
5. **Sort Results**: Order the aggregated output in descending order by the length of stay using `ORDER BY` `stay` `DESC`.

### SQL Query
````sql
SELECT 
    stay,
    COUNT(inter_dom) AS count_int,
    ROUND(CAST(AVG(todep) AS NUMERIC), 2) AS average_phq,
    ROUND(CAST(AVG(tosc) AS NUMERIC), 2) AS average_scs,
    ROUND(CAST(AVG(toas) AS NUMERIC), 2) AS average_as
FROM 
    students
WHERE 
    inter_dom = 'Inter'
GROUP BY 
    stay
ORDER BY 
    stay DESC;
````

### 🧾 Results

<div align="center">
  <img width="692" height="342" alt="image" src="https://github.com/user-attachments/assets/6a7dd703-dd38-4cd1-b013-ad79cee3723c" />
</div>


## 🔎 Key Findings
Based on the query results:
- **Uneven Sample Distribution:** The largest groups were students who had stayed for one year (95 students), two years (39 students), and three years (46 students). Groups with longer stays had substantially fewer observations.
- **Depression Scores** (`average_phq`): The average PHQ-9 score increased from 7.48 among students who had stayed for one year to 9.09 among those who had stayed for three years.
- **Social Connectedness** (`average_scs`): Average SCS scores remained relatively stable among the one- to three-year groups, ranging from 37.08 to 38.11.
- **Acculturative Stress** (`average_as`): The average ASISS score was 72.80 for the one-year group and reached 87.71 for the four-year group. The five-year group had an average of 91.00, although its small sample size limits the reliability of this comparison.

## 💡 Implications
The results reveal differences in average mental health diagnostic scores across length-of-stay groups, suggesting areas for further investigation by researchers and student support services.
- **Student support**: Universities could consider evaluating whether international students have access to appropriate mental health and cultural adjustment resources throughout their time abroad.
- **Further investigation**: The observed differences in depression and acculturative stress scores warrant further study to determine whether they persist in larger samples and after accounting for other factors.
- **Data-informed decisions**: Segmenting students by length of stay provides a useful starting point for exploring how support needs may differ across student groups.

These findings should be interpreted cautiously. The analysis is descriptive and does not establish that length of stay causes changes in mental health. Small sample sizes in some groups, along with other potential influencing factors, limit the conclusions that can be drawn.

> **Note on Data Access & Environment:** 
> This analysis was completed in DataCamp Workspace using its pre-loaded dataset of 272 records. The findings reflect the available dataset and should not be generalized to all international students.

## ⭐ Skills Demonstrated
- SQL querying and filtering
- Data aggregation and grouping
- Aggregate functions (COUNT, AVG)
- Type casting and numerical rounding
- Group-level descriptive analysis
- Data interpretation and communication

