# Marketing-Funnel-and-Sales-Performance-Analytics

## SQL Data Models and Queries

This repository contains three data models and the accompanying SQL queries used for analyzing and processing the data.

## Data Models

Below are the three data models used in this project:

### Data Model 1
![image](https://github.com/user-attachments/assets/213662be-ebb6-4d49-80b2-fd668a7e18bb)

### Data Model 2
![image](https://github.com/user-attachments/assets/9bade7b8-9e06-432c-8ac7-769e57c2ba0a)

### Data Model 3
![image](https://github.com/user-attachments/assets/8832cd75-3da6-484d-8f83-f54c7bf3c020)


---

## SQL Queries for DATA MODEL 1

### Query 1: Find out the Minimum, maximum, average, and how Revenue fluctuates throughout the year.

```sql
WITH rev_monthly_prod AS (
    SELECT
        date_trunc('month', S.ORDERDATE) AS Month,
        P.productname,
        SUM(revenue) AS total_rev
    FROM
        subscriptions S
    INNER JOIN products P
        ON S.ProductId = P.ProductID
    WHERE
        S.ORDERDATE BETWEEN '2022-01-01' AND '2022-12-31'
    GROUP BY
        date_trunc('month', S.ORDERDATE),
        P.productname
)

SELECT
    REVM.productname,
    MIN(total_rev) AS min_rev,
    MAX(total_rev) AS max_rev,
    AVG(total_rev) AS avg_rev,
    STDDEV(total_rev) AS std_dev_rev
FROM
    rev_monthly_prod AS REVM
GROUP BY
    REVM.productname;
```
![image](https://github.com/user-attachments/assets/8066c48b-181e-4ed7-839c-b4ef7f8e0bd4)

<br>

## SQL Queries for DATA MODEL 2

### Query 1: [Description of what the query does]
```sql
-- SQL code for Query 1
SELECT column1, column2
FROM table1
WHERE condition;
```
