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
GROUP BYnb 
    REVM.productname;
```
<img src="https://github.com/user-attachments/assets/8066c48b-181e-4ed7-839c-b4ef7f8e0bd4" alt="SQL Query Image" width="600"/>

<br>

### Query 2: Payment funnel Analysis with Multiple CTEs
1. Count the number of subscriptions in each payment funnel stage by incorporating the max status reached and current status per subscription.


```sql
	-- Create the CTE to calculate the max status for each subscription
	WITH maxstatus AS (
	    SELECT 
	        PL.SUBSCRIPTIONID,
	        MAX(PL.STATUSID) AS maxstatus
	    FROM 
	        paymentstatuslog PL
	    GROUP BY 
	        PL.SUBSCRIPTIONID
	),
	-- Create the CTE to determine the funnel stage
	funnelstage AS (
	    SELECT 
	        Sub.CurrentStatus, 
	        Sub.SubscriptionId,
	        CASE 
	            WHEN ms.maxstatus = 1 THEN 'PaymentWidgetOpened'
	            WHEN ms.maxstatus = 2 THEN 'PaymentEntered'
	            WHEN ms.maxstatus = 3 AND Sub.CurrentStatus = 0 THEN 'User Error with Payment Submission'
	            WHEN ms.maxstatus = 3 AND Sub.CurrentStatus != 0 THEN 'Payment Submitted'
	            WHEN ms.maxstatus = 4 AND Sub.CurrentStatus = 0 THEN 'Payment Processing Error with Vendor'
	            WHEN ms.maxstatus = 4 AND Sub.CurrentStatus != 0 THEN 'Payment Success'
	            WHEN ms.maxstatus = 5 THEN 'Complete'
	            WHEN ms.maxstatus IS NULL THEN 'User did not start payment process'
	            ELSE 'Unknown Status'
	        END AS paymentfunnelstage
	    FROM 
	        Subscriptions Sub
	    LEFT JOIN
	        maxstatus ms
	    ON
	        Sub.SUBSCRIPTIONID = ms.SUBSCRIPTIONID
	)
	-- Final query to count the number of subscription IDs per payment funnel stage
	SELECT 
	    FS.paymentfunnelstage, 
	    COUNT(FS.SubscriptionId) AS subscriptions
	FROM 
	    funnelstage FS
	GROUP BY 
	    FS.paymentfunnelstage
	ORDER BY 
    COUNT(FS.SubscriptionId) DESC;
```
<img src="https://github.com/user-attachments/assets/9f5d154d-0a43-4c66-b260-2858e5ece2a6" alt="SQL Query Image" width="400"/>


<br>

### Query 3: Flagging upsell opportunities for the Sales Team using CASE WHEN
Business Problem: The product team is launching a new Product offering that can be added on top of a current subscription for an increase in the customer's annual fee. The sales team has decided that they first want to reach out to a select group of cusomters to offer the new product and get feedback before offering it to the entire customer base. Below will be Query that is customized to meet the conditions of the Manager to find out potential customerS:


```sql
	SELECT 
    S.CustomerID,
    COUNT(S.ProductID) AS num_products,
    SUM(S.NumberofUsers) AS total_users,
    CASE 
        WHEN SUM(S.NumberofUsers) >= 5000 OR COUNT(S.ProductID) = 1 THEN 1
        ELSE 0  
    END AS upsell_opportunity
FROM
    subscriptions S
GROUP BY
    S.CustomerID![image](https://github.com/user-attachments/assets/e5da8bb8-b516-4f26-a20a-6f2ec9495aba)

```
<img src="https://github.com/user-attachments/assets/52f34e81-6d47-43e8-8eee-6bfe98272253" alt="SQL Query Image" width="700"/>

<br>

### Query 4: Tracking user activity with Frontend Events
AB Testing in 2 groups: control and treatment group of new landing pages. Pulled data from FrontEnd tracking buttons. The analytics team needs to track user activity via front-end events.

```sql
SELECT 
    FL.USERID,
    SUM(CASE WHEN FD.EVENTID = '1' THEN 1 ELSE 0 END) AS ViewedHelpCenterPage,
    SUM(CASE WHEN FD.EVENTID = '2' THEN 1 ELSE 0 END) AS ClickedFAQs,
    SUM(CASE WHEN FD.EVENTID = '3' THEN 1 ELSE 0 END) AS ClickedContactSupport,
    SUM(CASE WHEN FD.EVENTID = '4' THEN 1 ELSE 0 END) AS SubmittedTicket
FROM
    FrontendEventLog FL
INNER JOIN 
    FrontendEventDefinitions FD
ON
    FL.EVENTID = FD.EVENTID
WHERE
    FD.EVENTTYPE = 'Customer Support'
GROUP BY
    FL.USERID


```
<img src="https://github.com/user-attachments/assets/d25b59e4-273f-43cf-96c3-e8fca7106cf6" alt="SQL Query Image" width="700"/>

## SQL Queries for DATA MODEL 2

### Query 1: [Description of what the query does]
```sql
-- SQL code for Query 1
SELECT column1, column2
FROM table1
WHERE condition;
```
