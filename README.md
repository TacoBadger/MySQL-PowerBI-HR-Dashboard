# MySQL-PowerBI-HR-Dashboard!
![image](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/d08d229f-dc9c-455e-8c71-a116aa86f959)

# Introduction
Welcome to the introduction of  MySQL-PowerBI-HR-Dashboard created using PowerBI! This repository serves as a comprehensive resource for understanding and implementing an interactive HR dashboard using PowerBI, This dashboard is a tool that provides human resources with valuable insights into various HR metrics.

## Table of Contents
- [Dataset](#dataset)
- [Method](#method)
- [Ask](#ask)
- [Prepare](#prepare)
- [Process](#process)
- [Visual Assets](#visual-assets)
- [Findings](#findings)
- [Limitation](#limitation)
- [What is Next](#what-is-next)




## Dataset
[Human Resources.csv](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/files/11494160/Human.Resources.csv) 
This Dataset includes 22,000 rows and the date ranges from 2000 to 2020
Data Cleaning - Microsoft SQL Server Management
Visualization - PowerBI

## Method
I will use the framework contained the following pointers:
- Ask - Ask effective questions
- Prepare - Verify data's integrity, Check data credibility and reliability, Check data types and Merge datasets
- Process - Clean, Remove and Transform data and Document cleaning processes and results
- Analyze - Identify patterns and Draw conclusions
- Share - Create effective visuals
- Act - Answer your questions and solve problems

## Ask
- What is the gender breakdown of employees in the company?
- What is the race/ethnicity breakdown of employees in the company?
- What is the age distribution of employees in the company?
- How many employees work at headquarters versus remote locations?
- What is the average length of employment for employees who have been terminated?
- How does the gender distribution vary across departments and job titles?
- What is the distribution of job titles across the company?
- Which department has the highest turnover rate?
- What is the distribution of employees across locations by state?
- How has the company's employee count changed over time based on hire and term dates?
- What is the tenure distribution for each department?

## Prepare
Data Cleaning and PowerBI for Visualization

First I have the excel file here:
![image (2)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/74b1c310-cc0a-423a-a2b6-f50efd444b0b)


## Process
In this step I cleaned the excel file using this script on **Microsoft SQL Server Management Studio**


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
- I did this because I want to identify employees age youngest and the oldest, different ethnicity and what is the most dominant race.
- average employee that works on HQ
- Distribution of age and gender as well
- I also want to identify their locations so we will also add that on the PowerBI once the data is cleaned

First We need different fields for the names, birthdate, gender, race department, location etc etc.
![image (3)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/41088e17-a687-4c29-8473-27e475f5127e)

Data Cleaned now looked like this and ready to download.
![image (4)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/3f39b463-fe1b-4b9f-9293-caf290eb6479)

Now I can download the file and load it into my PowerBI to create Visuals.


## Visual Assets
The Average length of Employment

![image (5)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/dba3c0db-cd6c-454d-81b1-e21464c28a4b)

Distribution of Workforce: Headquarters and Remote
there is a lot of employees who work on Headquarters than remote work

![image (6)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/1c4a7041-e789-4d1e-8b35-75409885f55b)

The following PowerBI visuals, you can see high change rate of Employees

![image (7)](https://github.com/TacoBadger/MySQL-PowerBI-HR-Dashboard/assets/11693256/4cd25f6b-6cf0-4fdf-8a1f-c9bfcee283e3)




## Findings
There are more male employees
- White race is the most dominant while Native Hawaiian and American Indian are the least dominant.
- The youngest employee is 20 years old and the oldest is 57 years old
- 5 age groups were created (18-24, 25-34, 35-44, 45-54, 55-64). A large number of employees were between 25-34 followed by 35-44 while the smallest group was 55-64.
- A large number of employees work at the headquarters versus remotely.
- The average length of employment for terminated employees is around 7 years.
- The gender distribution across departments is fairly balanced but there are generally more male than female employees.
- The Marketing department has the highest turnover rate followed by Training. The least turn over rate are in the Research and development, Support and Legal departments.
- A large number of employees come from the state of Ohio.
- The net change in employees has increased over the years.
- The average tenure for each department is about 8 years with Legal and Auditing having the highest and Services, Sales and Marketing having the lowest.

## Limitation
Some records had negative ages and these were excluded during querying(967 records). Ages used were 18 years and above

## What is Next
Thank you for reading my study about Human Resources Data, this helps me improve my basic data analytical skills, and get better with visualizations.

See you in the next project!


