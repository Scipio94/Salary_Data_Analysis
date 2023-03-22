# SQL Syntax
~~~ SQL

/*Organizational Demographics Metrics - Sex*/

SELECT 
  DISTINCT Sex, 
  COUNT(*) OVER (PARTITION BY Sex) AS Sex_Count,
  CONCAT(ROUND((COUNT(*) OVER (PARTITION BY Sex)/ COUNT(*) OVER ()) * 100,2),'%') AS Percentage,
  COUNT(*) OVER () AS Total
FROM `single-being-353600.Financial_Dataset.Staff_Salary_Data` ;
~~~

**Result**
|Sex|Sex Count|Percentage|Total|
|---|---|---|--|
|F|117|75.97%|154|
|M|37|24.03|154|


~~~ SQL 
/*Organizational Demographics Metrics - Average Salary Sex*/

SELECT 
  DISTINCT Sex, 
  ROUND(AVG(StaffCompensationTotalSalary) OVER (PARTITION BY Sex),2) AS AVG_Salary_Sex,
  PERCENTILE_DISC(StaffCompensationTotalSalary,0.5) OVER(PARTITION BY Sex) AS Overall_Median_Salary_Sex,
  ROUND(AVG(StaffCompensationTotalSalary) OVER (),2) AS AVG_Salary_Overall,
  PERCENTILE_DISC(StaffCompensationTotalSalary,0.5) OVER() AS Overall_Median_Salary,
FROM `single-being-353600.Financial_Dataset.Staff_Salary_Data` 
ORDER BY AVG_Salary_Sex DESC ;

~~~

**Result**
|Sex|Avg_Salary_Sex|Overall_Median_Salary_Sex|Avg_Salary_Overall|Overall_Median|
|---|---|---|---|---|
|F|73366.97|72875|73362.42|71870|
|M|73348.25|67870|73362.42|71870|

~~~ SQL
/*Salary Distribution*/
SELECT 
  TRUNC(StaffCompensationTotalSalary, -4) AS Bins,
  COUNT(*) AS Count
 FROM `single-being-353600.Financial_Dataset.Staff_Salary_Data` 
 WHERE StaffCompensationTotalSalary IS NOT NULL
 GROUP BY Bins
 ORDER BY Bins;
~~~

**Result**
|Salary_Bins|Count|
|---|---|
|10000.0|3|
|20000.0|3|
|30000.0|9|
|40000.0|9|
|50000.0|20|
|60000.0|24|
|70000.0|19|
|80000.0|21|
|90000.0|26|
|100000.0|6|
|110000.0|2|
|120000.0|1|
|130000.0|2|
|140000.0|1|
|170000.0|2|

~~~ SQL 
/*Organizational Demographics Metrics - Average Salary Race*/

/*AVG Salary Race*/
SELECT 
  DISTINCT REPLACE(Race,'Asain', 'Asian') AS Race,
  COUNT(*) AS Count,
  SUM(COUNT(*)) OVER () AS Total_Count,
  CONCAT(ROUND((COUNT(*))/(SUM(COUNT(*)) OVER ()) * 100,2),'%') AS Percentage,
  ROUND(AVG(StaffCompensationTotalSalary),2) AS AVG_Salary_Race
FROM `single-being-353600.Financial_Dataset.Staff_Salary_Data`
GROUP BY Race
ORDER BY AVG_Salary_Race DESC;
~~~

**Result**

|Race|Count|Total_Count|Percentage|AVG_Salary_Race|
|---|---|---|---|--|
|White |43|154|27.92%|78035.67|
|African-American|81|154|52.6%|75659.33|
|Asian|2|154|1.3%|68050.0|
|Hispanic or Latinx|27|154|17.53%|60654.85|
|American Indian|1|154|0.65%|56250.0|


~~~ SQL
/*Salary Range*/

SELECT 
  DISTINCT MIN(StaffCompensationTotalSalary) AS Min,
  MAX(StaffCompensationTotalSalary) AS Max
FROM `single-being-353600.Financial_Dataset.Staff_Salary_Data`;

~~~

**Result**
|Min|Max|
|---|---|
|11250|175000|


~~~ SQL
/*Creating Dates DOB and Employeement Date*/
CREATE TEMP TABLE t1 AS -- Creating TEMP TABLE
SELECT 
   LocalStaffIdentifier,
  StaffCompensationTotalSalary,
  DateOfBirth,
  LEFT(CAST(DateOfBirth AS string),4) AS Year,--Coverting to string create substring of year 
  SUBSTR(CAST(DateOfBirth AS string),5,2) AS Month,--Coverting to string create substring of month 
  RIGHT(CAST(DateOfBirth AS string),2) AS Day, --Coverting to string create substring of day
  LEFT(CAST(DistrictEmploymentBeginDate AS string),4) AS Employee_Year,--Coverting to string create substring of year 
  SUBSTR(CAST(DistrictEmploymentBeginDate AS string),5,2) AS Employee_Month,--Coverting to string create substring of month 
  RIGHT(CAST(DistrictEmploymentBeginDate AS string),2) AS Employee_Day --Coverting to string create substring of day
FROM `single-being-353600.Financial_Dataset.Staff_Salary_Data`;

/*Calculating Employee's Age*/
SELECT 
  sub.LocalStaffIdentifier,
  sub.dob, 
  ROUND(EXTRACT(DAY FROM(CURRENT_DATE() - sub.dob))/365.00)  AS Age,--Calculating age in years
  sub.StaffCompensationTotalSalary
FROM
(SELECT
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary, 
  CAST(CONCAT(t1.year,'-',t1.month,'-',t1.day) AS date) AS DOB
FROM t1) AS Sub
ORDER BY Age;

/*Calculating Length of Employment*/

SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment,

FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub;
~~~

~~~ SQL 
/*Calculating New Staff and Returning Staff Metrics From TEMP TABLE t1*/

SELECT
   CASE
    WHEN sub1.length_employment <= 1 THEN 'New'
    ELSE 'Return Employee' END AS New_Staff,
  COUNT(*) AS Count,
  CONCAT(ROUND(COUNT(*)/SUM(COUNT(*)) OVER () * 100,2),'%') AS Percentage,
  ROUND(AVG(sub1.StaffCompensationTotalSalary),2) AS Avg_Salary

FROM
(SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment
FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub) AS sub1
GROUP BY New_staff;
~~~

**Result**

|New_Staff|Count|Percentage|Avg_Salary|
|---|---|---|---|
|New|49|31.82%|69681.96|
|Return Employee|105|68.18%|75184.06|

~~~ SQL 
/*Calculating New Staff and Returning Staff Salary Metrics From TEMP TABLE t1*/

SELECT 
 DISTINCT sub2.New_staff, 
  ROUND(AVG(sub2.StaffCompensationTotalSalary) OVER (PARTITION BY sub2.new_staff),2) AS Avg_New_Staff_Salary, 
  PERCENTILE_DISC(sub2.StaffCompensationTotalSalary, 0.5) OVER (PARTITION BY sub2.new_staff) AS Median_New_Staff_Salary
FROM
(SELECT
  sub1.StaffCompensationTotalSalary,
    CASE
    WHEN sub1.length_employment <= 1 THEN 'New'
    ELSE 'Return Employee' END AS New_Staff
FROM
(SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment
FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub) AS sub1) AS sub2;

~~~
**Result**
|New_staff|Avg_New_Staff_Salary|Median_New_Staff_Salary|
|---|---|---|
|Return Employee|75184.06|75460|
|New|69681.96|65450|
~~~ SQL 
/*Calculating Employee Length Tiers Metrics From TEMP TABLE t1*/

SELECT   
  CASE
  WHEN sub1.length_employment < 5 THEN 'Tier 1'
  WHEN sub1.length_employment BETWEEN 5 AND 10 THEN 'Tier 2'
  WHEN sub1.length_employment >= 10 THEN 'Tier 3'
  END AS Employment_Length_Tier,
ROUND(AVG(sub1.StaffCompensationTotalSalary),2) AS Avg_Salary,
COUNT(*) AS Count, 
SUM(COUNT(*)) OVER() AS Total,
CONCAT(ROUND(COUNT(*)/SUM(COUNT(*)) OVER() * 100,2),'%') AS Percentage 
FROM 
(SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment
FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub)AS sub1
GROUP BY Employment_Length_Tier
ORDER BY Count DESC ;
~~~

**Result**
|Employment_Length_Tier|Avg_Salary|Count|Total| Percentage|
|---|---|---|---|---|
|Tier 1|69598.97|112|154|72.73%|
|Tier 2|79390.2|28|154|18.18%|
|Tier 3|95616.42|14|154|9.09%|


~~~ SQL 
/*Comparing New Staff and Returning Staff Salary Metrics From TEMP TABLE t1*/
SELECT
  DISTINCT sub2. Employment_Length_Tier, 
  ROUND(AVG(sub2.StaffCompensationTotalSalary) OVER (PARTITION BY sub2.employment_length_tier),2) AS Avg_Tier_Salary,
  PERCENTILE_DISC(sub2.staffcompensationTotalSalary,0.5) OVER (PARTITION BY sub2.employment_length_tier) AS Median_Tier_Salary,
  COUNT(*) OVER (PARTITION BY sub2.Employment_Length_Tier) AS Count
FROM
(SELECT 
  sub1.LocalStaffIdentifier,
  sub1.StaffCompensationTotalSalary,
    CASE
    WHEN sub1.length_employment < 5 THEN 'Tier 1'
    WHEN sub1.length_employment BETWEEN 5 AND 10 THEN 'Tier 2'
    WHEN sub1.length_employment >= 10 THEN 'Tier 3'
    END AS Employment_Length_Tier,
FROM 
(SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment
FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub)AS sub1
ORDER BY sub1.StaffCompensationTotalSalary DESC) AS sub2;
~~~~
**Result**
|Employment_Length_Tier|Avg_Tier_Salary|Median_Tier_Salary|Count|
|---|---|---|---|
|Tier 2|79390.2|86930|28|
|Tier 1|69598.97|67870|112|
|Tier 3|95616.42|92000|14|
 
 
 ~~~ SQL 
 
 /*Calculating Average Employment Length from TEMP TABLE t1*/
 
 SELECT   
  DISTINCT ROUND(AVG(sub1.length_employment),2) AS Average_Employment_Length,
FROM 
(SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment
FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub)AS sub1 ;
 ~~~
 
 **Result**
|Average_Employment_Length|
|---|
|3.58|


~~~ SQL
/*Calculating Median Employment Length from TEMP TABLE t1*/

SELECT 
  DISTINCT ROUND(PERCENTILE_DISC(sub1.length_employment,0.5) OVER (),2) AS Median_Employment_Length
FROM 
(SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment
FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub)AS sub1;
~~~
**Result**
|Median_Employment_Length|
|---|
|1.79|


~~~ SQL 
/*Calculating Correlation Coefficient between Employment Length and Salary from TEMP TABLE t1*/
SELECT 
  ROUND(CORR(sub1.length_employment, sub1.StaffCompensationTotalSalary),2) AS CORR_EMPLOYMENT_TIME
FROM 
(SELECT 
  sub.LocalStaffIdentifier,
  sub.StaffCompensationTotalSalary,
  ROUND(EXTRACT(DAY FROM CURRENT_DATE() - sub.Employee_Start_Date)/365.00,2) AS Length_Employment
FROM 
(SELECT 
  t1.LocalStaffIdentifier, 
  t1.StaffCompensationTotalSalary,
  CAST(CONCAT(t1.Employee_Year,'-',t1.Employee_Month,'-',t1.Employee_Day) AS date) AS Employee_Start_Date
FROM t1) AS sub) AS sub1
~~~

**Result**
|CORR_EMPLOYMENT_TIME|
|---|
|0.3|
