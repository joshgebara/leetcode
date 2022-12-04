# Leetcode SQL Solutions

## 1757. Recyclable and Low Fat Products
```sql
# Write your MySQL query statement below
SELECT product_id 
FROM Products 
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

## 1741. Find Total Time Spent by Each Employee
```sql
# Write your MySQL query statement below
SELECT event_day AS day,
       emp_id, 
       SUM(out_time - in_time) AS total_time
FROM Employees
GROUP BY event_day, emp_id
```

## 1393. Capital Gain/Loss
```cpp
# Write your MySQL query statement below
SELECT stock_name,   
       SUM(IF(operation='Buy', -price, price)) AS capital_gain_loss
FROM Stocks
GROUP BY stock_name
```

## 1693. Daily Leads and Partners
```sql
# Write your MySQL query statement below
SELECT date_id, 
       make_name, 
       COUNT(DISTINCT lead_id) AS unique_leads,
       COUNT(DISTINCT partner_id) AS unique_partners
FROM DailySales
GROUP BY date_id, make_name;
```

## 1587. Bank Account Summary II
```sql
# Write your MySQL query statement below
SELECT name, 
       SUM(amount) AS balance
FROM Users
JOIN Transactions
USING(account)
GROUP BY account
HAVING balance > 10000;
```

## 1795. Rearrange Products Table
```sql
# Write your MySQL query statement below
SELECT product_id, "store1" AS store, store1 AS price
FROM Products
WHERE store1 IS NOT NULL

UNION

SELECT product_id, "store2" AS store, store2 AS price
FROM Products
WHERE store2 IS NOT NULL

UNION

SELECT product_id, "store3" AS store, store3 AS price
FROM Products
WHERE store3 IS NOT NULL
```