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

1.How many row for this table?

'''sql
SELECT
COUNT() as n_row
from weekly_sales
'''

<ins>Explain the code<ins>

<ins>Answer the question<ins>
| n_row |
|---|
| 17,117 |

