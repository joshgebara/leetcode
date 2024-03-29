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

## 1581. Customer Who Visited but Did Not Make Any Transactions
### Sub-Query
```sql
# Write your MySQL query statement below

SELECT customer_id, 
       COUNT(*) AS count_no_trans
FROM Visits
WHERE visit_id NOT IN (SELECT visit_id FROM transactions)
GROUP BY customer_id
```

### Left Join
```sql
# Write your MySQL query statement below

SELECT customer_id, 
       COUNT(*) AS count_no_trans
FROM Visits AS v
LEFT JOIN Transactions AS t
USING(visit_id)
WHERE t.transaction_id is NULL
GROUP BY customer_id
```

## 1484. Group Sold Products By The Date
```sql
# Write your MySQL query statement below
SELECT sell_date, 
       COUNT(DISTINCT product) AS num_sold, 
       GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
FROM Activities
GROUP BY sell_date
```

## 627. Swap Salary
```sql
# Write your MySQL query statement below
UPDATE Salary 
SET sex = if(sex = 'm', 'f', 'm')
```

## 1148. Article Views I
```sql
# Write your MySQL query statement below
SELECT DISTINCT author_id AS id
From Views
WHERE author_id = viewer_id
ORDER BY author_id
```

## 175. Combine Two Tables
```sql
# Write your MySQL query statement below
SELECT firstName, lastName, city, state
FROM Person
LEFT JOIN Address
USING(personId)
```

## 608. Tree Node
```cpp
# Write your MySQL query statement below
SELECT DISTINCT 
    currNode.id, 
    CASE
        WHEN currNode.p_id IS NULL THEN "Root"
        WHEN childNode.id IS NULL THEN "Leaf"
        ELSE "Inner"
    END AS type
FROM Tree AS currNode
LEFT JOIN Tree AS childNode
ON currNode.id = childNode.p_id
```

## 620. Not Boring Movies
```sql
# Write your MySQL query statement below
SELECT *
FROM Cinema
WHERE id % 2 = 1 AND description <> "boring"
ORDER BY rating DESC
```

## 595. Big Countries
```sql
# Write your MySQL query statement below
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000
```

## 1050. Actors and Directors Who Cooperated At Least Three Times
```sql
# Write your MySQL query statement below
SELECT actor_id, director_id
FROM ActorDirector
GROUP BY actor_id, director_id
HAVING COUNT(*) >= 3
```

## 607. Sales Person
```cpp
# Write your MySQL query statement below
SELECT name
FROM SalesPerson
WHERE sales_id NOT IN (
    SELECT sales_id
    FROM Orders AS o
    INNER JOIN Company AS c
    USING(com_id)
    WHERE c.name = 'RED'
)
```

## 586. Customer Placing the Largest Number of Orders
```sql
# Write your MySQL query statement below
SELECT customer_number
FROM Orders
GROUP BY customer_number
HAVING COUNT(*) = (
    SELECT COUNT(*) AS count
    FROM Orders
    GROUP BY customer_number
    ORDER BY count DESC
    LIMIT 1
)
```

## 584. Find Customer Referee
```sql
SELECT name
FROM customer
WHERE referee_id <> 2 OR referee_id IS NULL
```

## 1729. Find Followers Count
```sql
# Write your MySQL query statement below
SELECT user_id, COUNT(*) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id
```

## 182. Duplicate Emails
```sql
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1
```

## 626. Exchange Seats
```cpp
# Write your MySQL query statement below
SELECT 
    CASE
        WHEN id % 2 = 0 THEN id - 1
        WHEN id % 2 = 1 AND id + 1 <= (SELECT COUNT(*) FROM Seat) THEN id + 1
        ELSE id
    END AS id,
    student
FROM Seat
ORDER BY id
```

## 181. Employees Earning More Than Their Managers
```sql
# Write your MySQL query statement below
SELECT e1.name AS employee
FROM Employee AS e1
INNER JOIN Employee AS e2
ON e1.managerId = e2.id
WHERE e1.salary > e2.salary
```

## 183. Customers Who Never Order
```sql
# Write your MySQL query statement below
SELECT name AS Customers
FROM Customers
WHERE id NOT IN (SELECT DISTINCT customerId from Orders)
```

## 1407. Top Travellers
```cpp
# Write your MySQL query statement below
SELECT name, SUM(IFNULL(distance, 0)) AS travelled_distance
FROM USERS AS u
LEFT JOIN Rides AS r
ON u.id = r.user_id
GROUP BY u.id
ORDER BY travelled_distance DESC, name ASC
```

## 1667. Fix Names in a Table
```sql
# Write your MySQL query statement below
SELECT user_id, 
       CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM Users
ORDER BY user_id
```