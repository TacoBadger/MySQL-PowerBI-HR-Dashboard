# MySQL-PowerBI-HR-Dashboard!
![image](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/d08d229f-dc9c-455e-8c71-a116aa86f959)

# Introduction

## Table of Contents
- [Dataset](#dataset)
- [Method](#method)
- [Ask](#ask)
- [Prepare](#prepare)
- [Process](#process)
- [Visualize](#visualize)
- [Findings](#findings)
- [What is Next](#what-is-next)




## Dataset
[Human Resources.csv](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/files/11494160/Human.Resources.csv) 
This Dataset includes 22,000 rows and the date ranges from 2000 to 2020
Data Cleaning - Microsoft SQL Server Management
Visualization - PowerBI

## Method
I will use the framework contained the following pointers:
Ask - Ask effective questions
Prepare - Verify data's integrity, Check data credibility and reliability, Check data types and Merge datasets
Process - Clean, Remove and Transform data and Document cleaning processes and results
Analyze - Identify patterns and Draw conclusions
Share - Create effective visuals
Act - Answer your questions and solve problems

## Ask
What is the gender breakdown of employees in the company?
What is the race/ethnicity breakdown of employees in the company?
What is the age distribution of employees in the company?
How many employees work at headquarters versus remote locations?
What is the average length of employment for employees who have been terminated?
How does the gender distribution vary across departments and job titles?
What is the distribution of job titles across the company?
Which department has the highest turnover rate?
What is the distribution of employees across locations by state?
How has the company's employee count changed over time based on hire and term dates?
What is the tenure distribution for each department?

## Prepare
Data Cleaning and PowerBI for Visualization

First I have the excel file here:
![image (2)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/74b1c310-cc0a-423a-a2b6-f50efd444b0b)

Data Cleaned:
![image (3)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/41088e17-a687-4c29-8473-27e475f5127e)
![image (4)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/3f39b463-fe1b-4b9f-9293-caf290eb6479)

## Process
In this step I cleaned the excel file using this script
```bash
CREATE DATABASE projects;
USE projects;
SELECT 
  * 
FROM 
  hr;
ALTER TABLE 
  hr CHANGE COLUMN ï»¿id emp_id VARCHAR(20) NULL;
DESCRIBE hr;
SELECT 
  birthdate 
FROM 
  hr;
SET 
  sql_safe_updates = 0;
UPDATE 
  hr 
SET 
  birthdate = CASE WHEN birthdate LIKE '%/%' THEN date_format(
    str_to_date(birthdate, '%m/%d/%Y'), 
    '%Y-%m-%d'
  ) WHEN birthdate LIKE '%-%' THEN date_format(
    str_to_date(birthdate, '%m-%d-%Y'), 
    '%Y-%m-%d'
  ) ELSE NULL END;
ALTER TABLE 
  hr MODIFY COLUMN birthdate DATE;
UPDATE 
  hr 
SET 
  hire_date = CASE WHEN hire_date LIKE '%/%' THEN date_format(
    str_to_date(hire_date, '%m/%d/%Y'), 
    '%Y-%m-%d'
  ) WHEN hire_date LIKE '%-%' THEN date_format(
    str_to_date(hire_date, '%m-%d-%Y'), 
    '%Y-%m-%d'
  ) ELSE NULL END;
ALTER TABLE 
  hr MODIFY COLUMN hire_date DATE;
UPDATE 
  hr 
SET 
  termdate = date(
    str_to_date(
      termdate, '%Y-%m-%d %H:%i:%s UTC'
    )
  ) 
WHERE 
  termdate IS NOT NULL 
  AND termdate != ' ';
ALTER TABLE 
  hr MODIFY COLUMN termdate DATE;
ALTER TABLE 
  hr 
ADD 
  COLUMN age INT;
UPDATE 
  hr 
SET 
  age = timestampdiff(
    YEAR, 
    birthdate, 
    CURDATE()
  );
SELECT 
  min(age) AS youngest, 
  max(age) AS oldest 
FROM 
  hr;
SELECT 
  count(*) 
FROM 
  hr 
WHERE 
  age < 18;
SELECT 
  COUNT(*) 
FROM 
  hr 
WHERE 
  termdate > CURDATE();
SELECT 
  COUNT(*) 
FROM 
  hr 
WHERE 
  termdate = '0000-00-00';
SELECT 
  location 
FROM 
  hr;

```
## Visualize



## Findings
There are more male employees
White race is the most dominant while Native Hawaiian and American Indian are the least dominant.
The youngest employee is 20 years old and the oldest is 57 years old
5 age groups were created (18-24, 25-34, 35-44, 45-54, 55-64). A large number of employees were between 25-34 followed by 35-44 while the smallest group was 55-64.
A large number of employees work at the headquarters versus remotely.
The average length of employment for terminated employees is around 7 years.
The gender distribution across departments is fairly balanced but there are generally more male than female employees.
The Marketing department has the highest turnover rate followed by Training. The least turn over rate are in the Research and development, Support and Legal departments.
A large number of employees come from the state of Ohio.
The net change in employees has increased over the years.
The average tenure for each department is about 8 years with Legal and Auditing having the highest and Services, Sales and Marketing having the lowest.
Limitation:
Some records had negative ages and these were excluded during querying(967 records). Ages used were 18 years and above

## What is Next
