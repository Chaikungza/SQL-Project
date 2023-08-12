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

