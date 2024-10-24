# Healthcare-Analysis-using-SQL-and-Excel
To structure this for your GitHub profile, you can organize it into clear sections and add comments to explain each query's purpose. Here's how you could do it:

---

# Patient Data Analysis Queries

This repository contains SQL queries to analyze patient data, specifically focused on OCD (Obsessive-Compulsive Disorder) statistics. Below are queries that explore gender, ethnicity, and other factors in relation to OCD and its symptoms.

### 1. Count & Percentage of Male vs Female Patients with OCD and Their Average Obsession Score
```sql
with cte as (
  select
    Gender,
    count(distinct `Patient ID`) as patient_count,
    round(avg(`Y-BOCS Score (Obsessions)`), 2) as avg_obs_score
  from patient_dataset
  group by 1
  order by 2
)

select
  sum(case when Gender = 'male' then patient_count else 0 end) as male_count,
  sum(case when Gender = 'female' then patient_count else 0 end) as female_count,
  round(sum(case when Gender = 'male' then patient_count else 0 end) /
    (sum(case when Gender = 'male' then patient_count else 0 end) + sum(case when Gender = 'female' then patient_count else 0 end)) * 100, 2) as male_percentage,
  round(sum(case when Gender = 'female' then patient_count else 0 end) /
    (sum(case when Gender = 'male' then patient_count else 0 end) + sum(case when Gender = 'female' then patient_count else 0 end)) * 100, 2) as female_percentage
from cte;
```

### 2. Count of Patients by Ethnicity and Their Average Obsession Score
```sql
select
  Ethnicity,
  count(distinct `Patient ID`) as patient_count,
  round(avg(`Y-BOCS Score (Obsessions)`), 2) as Ave_Obsession_Score
from patient_dataset
group by 1
order by 2;
```

### 3. Month-over-Month Count of People Diagnosed with OCD
```sql
select
  substring(`OCD Diagnosis Date`, 1, 7) as month,
  count(`Patient ID`) as OCD_patient_count
from patient_dataset
group by 1
order by 1;
```

### 4. Most Common Obsession Type and its Average Obsession Score
```sql
select
  `Obsession Type`,
  count(`Obsession Type`) as obs_count,
  round(avg(`Y-BOCS Score (Obsessions)`), 2) as avg_obs_score
from patient_dataset
group by 1
order by 2 desc;
```

### 5. Most Common Compulsion Type and its Average Obsession Score
```sql
select
  `Compulsion Type`,
  count(*) as compulsion_count,
  round(avg(`Y-BOCS Score (Obsessions)`), 2) as avg_obs_score
from patient_dataset
group by 1
order by 2 desc;
```

### 6. OCD Distribution Across Genders and Ethnicities
```sql
with cte as (
  select
    Ethnicity,
    Gender,
    count(`OCD Diagnosis Date`) as distribution_count
  from patient_dataset
  group by 1, 2
)

select
  Ethnicity,
  round(sum(case when Gender = 'male' then distribution_count else 0 end) /
    (sum(case when Gender = 'male' then distribution_count else 0 end) + sum(case when Gender = 'female' then distribution_count else 0 end)) * 100, 2) as male_percentage,
  round(sum(case when Gender = 'female' then distribution_count else 0 end) /
    (sum(case when Gender = 'male' then distribution_count else 0 end) + sum(case when Gender = 'female' then distribution_count else 0 end)) * 100, 2) as female_percentage
from cte
group by 1;
```

### 7. Relationship Between Marital Status, Education Level, and Severity of OCD Symptoms
```sql
select
  `Education Level`,
  count(case when `Marital Status` = 'Single' then `OCD Diagnosis Date` else null end) as Single,
  count(case when `Marital Status` = 'Married' then `OCD Diagnosis Date` else null end) as Married,
  count(case when `Marital Status` = 'Divorced' then `OCD Diagnosis Date` else null end) as Divorced
from LoanDB.patient_dataset
group by 1
order by 2 desc, 3 desc, 4 desc;
```

---