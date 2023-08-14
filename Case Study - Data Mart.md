# SQL-Project - Case study 2
This is SQL case study project for practice SQL skill.

Reference case study from https://8weeksqlchallenge.com/case-study-5

Case Study : Data Mart

Background : Large scale supply changes were made at Data Mart. All Data Mart products now use sustainable packaging methods in every single step from the farm all the way to the customer

Purpose for this project
1. Data Cleansing
2. Data Analysis

# Entity Relationship Diagram & Table
![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/aab3d920-e565-4f86-a7c2-77189f7fe411)


![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/2734061f-4b3b-4b1f-b46e-23cad1643f98)

# Case Study Questions
The following case study questions require some data cleaning steps before start to unpack Danny’s key business questions in more depth.

Data Cleansing Steps
In a single query, perform the following operations and generate a new table in the data_mart schema named weekly_sales_cleansing:
```sql
CREATE TABLE weekly_sales_cleansing as  (
SELECT

TO_DATE(week_date, 'DD/MM/YY') AS week_dates,
DATE_PART('week', TO_DATE(week_date, 'DD/MM/YY')) AS week_nos,
EXTRACT(YEAR FROM TO_DATE(week_date, 'DD/MM/YY')) AS years,
EXTRACT(MONTH FROM TO_DATE(week_date, 'DD/MM/YY')) AS months,

	CASE
    	when RIGHT(segment,1) in ('1') then 'Young Adults'
        when RIGHT(segment,1)in ('2') then 'Middle Aged'
        when RIGHT(segment,1)in ('3','4') then 'Retirees'
        ELSE 'unknown'
        end as segment,

	CASE
    	when LEFT(segment,1) in ('C') then 'Couples'
        when LEFT(segment,1) in ('F') then 'Families'
        ELSE 'unknown'
       end as demographic,
       
Round(sales/transactions,2) as avg_transaction
FROM weekly_sales
);
```
# Step of cleansing
1.Convert the week_date to a DATE format.

```sql
SELECT
TO_DATE(week_date, 'DD/MM/YY') AS week_dates
FROM weekly_sales
```

2.Add a week_number as the second column for each week_date value.

3.Add a month_number with the calendar month for each week_date value as the 3rd column.

4.Add a calendar_year column as the 4th column containing either 2018, 2019 or 2020 values.

```sql
TO_DATE(week_date, 'DD/MM/YY') AS week_dates,
DATE_PART('week', TO_DATE(week_date, 'DD/MM/YY')) AS week_nos,
EXTRACT(YEAR FROM TO_DATE(week_date, 'DD/MM/YY')) AS years,
EXTRACT(MONTH FROM TO_DATE(week_date, 'DD/MM/YY')) AS months,
```
5.Add a new column called age_band after the original segment column using the following mapping on the number inside the segment value.

![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/b1b2090c-06c7-4231-b09c-7f5c5ec14f33)
```sql
	CASE
    	when RIGHT(segment,1) in ('1') then 'Young Adults'
        when RIGHT(segment,1)in ('2') then 'Middle Aged'
        when RIGHT(segment,1)in ('3','4') then 'Retirees'
        ELSE 'unknown'
        end as segment
```

6.Add a new demographic column using the following mapping for the first letter in the segment values.

![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/e3888db3-14fc-41ec-8d81-dd03d1eb410f)

```sql
CASE
    	when LEFT(segment,1) in ('C') then 'Couples'
        when LEFT(segment,1) in ('F') then 'Families'
        ELSE 'unknown'
       end as demographic
```

7.Generate a new avg_transaction column as the sales value divided by transactions rounded to 2 decimal places for each record.

```sql
Round(sales/transactions,2) as avg_transaction
```

# Data Exploration

1.How many row for this table?

```sql
SELECT
COUNT() as n_row
from weekly_sales
```

<ins>Explain the code<ins>

<ins>Answer the question<ins>
| n_row |
|---|
| 17,117 |

2.What day of the week is used for each week_date value?

```sql
SELECT 
EXTRACT(DOW FROM week_dates) AS day_of_week,
COUNT(*)
from weekly_sales_cleansing
GROUP by day_of_week
```

| day_of_week | count |
|---| ---|
| 1 | 17,117 |

<ins>Explain the code<ins>

- Use "EXTRACT" for change format date to week of day format (returns the day of the week as an integer. (0 = Sunday, 1 = Monday, ..., 6 = Saturday)

- Use "Group by for checking  quantity day of week in data set.

<ins>Answer the question<ins>

- Refer to previous question total transaction = 17,117 rows.

- For the result is "1" so the day of the week for the data set = "Monday"

3.What range of week numbers are missing from the dataset?

```sql
WITH All_Weeks AS (
    SELECT generate_series(1, 52) AS week_number
)
      
SELECT week_number,
week_nos
FROM All_weeks
left join weekly_sales_cleansing
on All_Weeks.week_number = weekly_sales_cleansing.week_nos
where week_nos is NULL
```
<ins>Explain the code<ins>
- Create new column Week number 1-52 week by sub query
- Left join Week no from data set with Week no. colimn (1-52)
- Week no. that missing in data set will return "Null"

 <ins>Answer the question<ins>
 
 28 week no. are missing (1-12, 37-52)

4.How many total transactions were there for each year in the dataset?
```sql
SELECT years,
SUM(transactions) as Sum_transaction
FROM weekly_sales_cleansing
GROUP by years
order by years
```
 <ins>Answer the question<ins>
| years | Sum_transaction |
|---| ---|
| 2018 | 346,406,460 |
| 2019 | 365,639,285 |
| 2020 | 375,813,651 |

5.What is the total sales for each region for each month?

```sql
SELECT 
region,
years,
months,
SUM(transactions)
FROM weekly_sales_cleansing
GROUP by years, months, region
ORDER by years, months
```
<ins>Answer the question<ins>
![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/fbc2d02d-9be6-4d72-9f5c-6bb89ee95fa0)

6.What is the total count of transactions for each platform?

```sql
SELECT 
platform,
years,
COUNT(platform) as transactions,
sum(COUNT(platform)) OVER (PARTITION by platform) as all_total_transaction

FROM weekly_sales_cleansing
GROUP by platform,years
ORDER by years
```
<ins>Explain the code<ins>
- Group by Platform as request and by year and add column total of count transaction for easy understanding.

<ins>Answer the question<ins>
| platform | years |transaction |all_total_transaction |
|---| ---| ---| ---|
| Retail| 2018 | 2,856 |8,568|
| Shopofy| 2018 | 2,842 |8,549|
| Retail| 2019 | 2,856 |8,568|
| Shopofy| 2019 | 2,852 |8,549|
| Retail| 2020 | 2,856 |8,568|
| Shopofy| 2020 | 2,855 |8,549|
