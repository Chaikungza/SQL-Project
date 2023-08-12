# SQL-Project - Case study 1
This is SQL case study project for practice SQL skill.

Reference case study from https://8weeksqlchallenge.com/case-study-5

Case Study : Danny's Diner

Background : Data set for restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Purpose for this project
1. Data Exploration
2. Data Analysis

# Entity Relationship Diagram & Table
![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/75f9baf1-9bdd-452a-be49-3ff145dbfbd6)

![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/bfaed5c9-1b41-430d-8ea8-984ce05c5002)

![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/378c2863-3ac3-4534-a463-faeba0c70e58)

![image](https://github.com/Chaikungza/SQL-Project/assets/121532457/5452e12f-eaa5-4218-bb67-5c6c4e7f4d69)

# Question
1.What is the total amount each customer spent at the restaurant ?

```sql
with sub AS
(
SELECT 
customer_id,
order_date,
product_name,
price
from sales
JOIN menu
on sales.product_id = menu.product_id
)
SELECT customer_id,
sum(price) as total_amount
from sub
GROUP by customer_id
ORDER by total_amount DESC
```
<ins>Explain the code<ins>

1.Create "sub query" for join 2 table (sales & menu) because individule table have not enough information for calculate total amount of each customer while product_id are is primary key.

2.Sum Total amout by customer id and sort by value.

<ins>Answer the question<ins>
| customer_id | total_amount |
|---|---|
| A | 76 |
| B | 74 |
| C | 36 |
________________________________________________________

2.How many days has each customer visited the restaurant?
```sql
SELECT customer_id,
COUNT(DISTINCT order_date) as Qty_Day

from sales
group by customer_id
```
<ins>Explain the code<ins>

1.Use "Distinct" for count the day (Not repeat day) of visited each customer from sales table.

<ins>Answer the question<ins> 
| customer_id | Qty_day |
|---|---|
| A | 4 |
| B | 6 |
| C | 2 |
________________________________________________________

3.What was the first item from the menu purchased by each customer?
```sql
with sub AS
(
SELECT 
customer_id,
order_date,
product_name,
price,
ROW_NUMBER() OVER (PARTITION BY customer_id) AS rn
from sales
JOIN menu
on sales.product_id = menu.product_id
)
SELECT  customer_id,
order_date,
product_name
from sub
WHERE rn = 1
```
<ins>Explain the code<ins>

1.Use sub query from Problem no.1 .

2.Use "ROW_NUMBER" for ranking order that buy by each customer because customer can buy more than 1 time per day.

3.Refer from the problem,  filter of first item of each customer by "Where ROW_NUMBER = 1"

<ins>Answer the question<ins>
| customer_id | product_name | order_date |
|---|---|---|
| A | sushi | 2021-01-01 |
| B | currey | 2021-01-01 |
| C | ramen | 2021-01-01 |
________________________________________________________

4.What is the most purchased item on the menu and how many times was it purchased by all customers?
```sql
with sub AS
(
SELECT 
customer_id,
order_date,
product_name,
price
from sales
JOIN menu
on sales.product_id = menu.product_id
)
SELECT 
product_name,
COUNT(product_name) as Qty_pur
from sub
GROUP by product_name
ORDER by Qty_pur desc
```
<ins>Explain the code<ins>

1.Use "count" for calculate quantity total times to buy by each product.

2.Group data by product and sort by times of buy.

<ins>Answer the question<ins>
| product_name | Qty_pur |
|---|---|
| ramen | 8 |
| currey | 4 |
| sushi | 3 |

5.Which item was the most popular for each customer?
```sql
with sub AS
(
SELECT 
customer_id,
order_date,
product_name,
price
from sales
JOIN menu
on sales.product_id = menu.product_id
)
SELECT
customer_id,
product_name,
COUNT(product_name) as qty_pur
from sub
GROUP by customer_id, product_name
order by qty_pur desc
```

<ins>Explain the code<ins>

1.Use "count" for calculate quantity total times to buy by each customer.

2.Group data by product name and sort by times of buy.

<ins>Answer the question<ins>
| customer_id | product_name | qty_pur |
|---|---|---|
|A| ramen | 3 |
|C| ramen | 3 |
|A| curry | 2 |
|B| curry | 2 |
|B| ramen | 2 |
|B| sushi | 2 |
|A| sushi | 1 |

________________________________________________________
