# Salary Data Analysis

## Abstract 

An organization has been having issues with staff retention. The main reasons cited amongst staff is inequitable compensation practices. Specifically length of employment and amount of salary. This comes after a recent compensation audit with the intention to make compensation more comprehensive and equitable at the organization.

## Variables


![Salary_Data_Project (1)](https://user-images.githubusercontent.com/112409778/226190531-26196558-55aa-4854-80f8-1874ec2d9c11.png)


There are several variables in the dataset that will have an impact on the outcome of the analysis:

- LocalStaffIdentifier: Staff local ID
- StaffCompensationTotalSalary: Employee salary in USD 
- Sex 
  - Male 
  - Female
 -Race 
  - African-American
  - American Indian
  - Asian
  - Hispanic or Latinx
  - White 
 - DOB: Date of Birth 
 - Age: Age of employee in years
 - Employment_Date: Date of employment - YYYY-MM-DD
 - Employement_Length: Length of employment in years
    - Tier 1: Employees that have been employed at the organization for less than 5 years.
    - Tier 2:Employees that have been employed at the organization between 5 and 10 years.
    - Tier 3: Employees that have been employed at the organization for more than 10 years .
    - New: Employees that have been employed at the organization for less than 1 year.
    - Return_Employee: Employees that have been employed at the organization for more than 1 year.

## Objective 

The objective of the analysis is to identify key variables that have an effect on an employee’s salary.The analysis seeks to answer the following questions:

1. What are the organizational metrics for the following demographics within the organization listed below:
  - Sex
  - Race
  - Length of Employment 

2. What is the average salary of the following demographics within the organization listed below:
  - Sex
  - Race
  - Length of Employment 

3. Is there a correlation between length of employment and salary?

## Analysis 

1. What are the organizational metrics for the following demographics within the organization listed below:
  - Sex
  - Race
  - Length of Employment 


In total there 154 employees at the organization, 117 (76%) identify as Female and 37 (24%) identify as Male. Additionally, there are 81 (53%) employees  who identify as African-American, 43 (28%) of employees who identify as White, 27 (18%) who identify as Hispanic/Latinx,  2 (1%) employees  who identify as Asian, and 1(1%) of employees identify as American Indian. 

![Length of Employment Histogram](https://user-images.githubusercontent.com/112409778/226191881-df8a004d-7195-42c1-aaff-7a569fcbfcc5.jpg)

Historically, the organization has had a high turnover rate and this indicated in the analysis. 49 (32%) of the employees have been at the organization for less than 1 year, indicating an attrition rate of 32% from the previous calendar year. Additionally, 112 (73%) of employees are in Tier 1, indicating that they’ve been at the organization for less than 5 years. With that being said, the organization has an average employment length of approximately 3.60 years. However, due to the right skew of the histogram of employment length frequency above, the median would be a better indication of typical employment length at this organization. The median employment length at the organization is approximately 1.79 years.


2. What is the average salary of the following demographics within the organization listed below:
  - Sex
  - Race
  - Length of Employment 

The organization has a minimum salary of $11,250 and a maximum salary of $175,000. The average salary within the organization is $73,362.42. 

![Compensation Histogram](https://user-images.githubusercontent.com/112409778/226191977-8b3e746c-29b1-4f0a-9963-7a5884b91c93.jpg)

However, there is uneven distribution of salaries within the organization. There is a slight right skew of the histogram with outliers that impact the average salary metric.Also, majority of the salaries are above the median. The organization has a median salary of $71,870, and it is a better indicator of the typical salary within the organization. 

The average salary of males and females is $73,348.25 and $73,366.97 respectively.As it pertains to race, average salary of White employees is $78,035.67, African-American employees have a average salary of $75,659.33, Asian employees have an average salary of  $68,050, Hispanic/Latinx employees have an average salary of $60,654.85, and American Indian employees have an average salary of $56,250.

![Employee Tier Compensation Histogram](https://user-images.githubusercontent.com/112409778/226192026-59326232-bc6e-4b40-bf14-b80a9dc00ac4.jpg)

Tier 1 employees have an average salary of $69,598.97. However, the histogram above displays a right skew and is indicative there are outliers in the salary distribution of Tier 1 employees.The median salary, $67,870, is a better indication of the typical salary of a Tier 1 employee. 

![Employee Tier  2 Compensation Histogram](https://user-images.githubusercontent.com/112409778/226210348-2a8730ef-e6b5-429b-a97d-ac677a783453.jpg)

Tier 2 employees have an average salary of $79,390.20 , but have a median salary of $86,930.00  indicating a left skew in the distribution of Tier 2 Employee salaries as indicated in the histogram above.The median salary of Tier 2 employees is a better indication of a typical salary within the range.

![Employee Tier 3 Compensation Histogram](https://user-images.githubusercontent.com/112409778/226192069-b48ce58f-df58-4c8e-8400-33299371b4df.jpg)

Tier 3 employees have an average salary of $95,616.42.  As displayed in the histogram above there are outliers in the dataset and the median salary, $92,000, is a better representation of the typical salary of a Tier 3 employee.

![New Employee Compensation Histogram](https://user-images.githubusercontent.com/112409778/226192101-cbd0b3c5-2a27-4c4b-ac58-bc23a1f9c4d1.jpg)

Additionally, New employees have an average salary of $69,681.96 . There are some outliers that are inflating the average salary of the New Employees and the median salary, $65,450, is a better indication of the typical salary of New Employees. The distribution of Returning Salaries is slightly more uniform and the average salary,$75,184.06,is indicative of the typical salary of a Returning Employee.

3. Is there a correlation between length of employment and salary?

![Length of Employment Scatter Plot - Length of Employment and Salary](https://user-images.githubusercontent.com/112409778/226192194-682549d8-2ab5-4cb9-8f6f-68ea5142ce1b.jpg)

In the previous analysis there appears to be a positive correlation between the length of time an employee has been employed at the organization and the average salary. However, the inverse is true, there is no correlation. The correlation coefficient for the employment length and compensation is approximately 0.30, well below 0.9 which is needed to establish a strong positive correlation between two variables and closer to 0 indicating a neutral slope. This is also evident in the line of best fit.

## Conclusion

There is no correlation between length of employment and employee salary and it is possible that the lack of equitable compensation practices is the primary cause of high employment turnover. Despite the company recently completing a compensation audit, I recommend another one with the intention to make compensation and length of employment uniform. Additionally, I recommend using performance bonuses and other incentives, e.g. hybrid roles, to boost retention as well. 

