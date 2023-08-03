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
Explain the code

Answer the question

________________________________________________________

2.How many days has each customer visited the restaurant?
```sql
SELECT customer_id,
COUNT(DISTINCT order_date) as Qty_Day

from sales
group by customer_id
```
Explain the code

Answer the question 

________________________________________________________

3.What was the first item from the menu purchased by each customer?
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
product_name,
order_date
from sub
where order_date = '2021-01-01'
GROUP by customer_id
```
Explain the code

Answer the question

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
Explain the code

Answer the question

________________________________________________________
