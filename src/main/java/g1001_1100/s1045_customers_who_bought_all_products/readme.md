[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1045\. Customers Who Bought All Products

Medium

SQL Schema

Table: `Customer`

    +-------------+---------+ 
    | Column Name | Type    | 
    +-------------+---------+ 
    | customer_id | int     | 
    | product_key | int     | 
    +-------------+---------+ 

There is no primary key for this table. It may contain duplicates. `customer_id` is not NULL`.` product\_key is a foreign key to `Product` table.

Table: `Product`

    +-------------+---------+ 
    | Column Name | Type    | 
    +-------------+---------+ 
    | product_key | int     | 
    +-------------+---------+ 

    product_key is the primary key column for this table.

Write an SQL query to report the customer ids from the `Customer` table that bought all the products in the `Product` table.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:** Customer table: 

    +-------------+-------------+ 
    | customer_id | product_key | 
    +-------------+-------------+ 
    | 1           | 5           | 
    | 2           | 6           | 
    | 3           | 5           | 
    | 3           | 6           | 
    | 1           | 6           | 
    +-------------+-------------+ 

Product table: 

    +-------------+ 
    | product_key | 
    +-------------+ 
    | 5           | 
    | 6           | 
    +-------------+

**Output:** 

    +-------------+ 
    | customer_id | 
    +-------------+ 
    | 1           | 
    | 3           | 
    +-------------+

**Explanation:** The customers who bought all the products (5 and 6) are customers with IDs 1 and 3.

## Solution

```sql
# Write your MySQL query statement below
select customer_id
from customer
group by customer_id
having count(distinct product_key)=(select count(*) from product);
```