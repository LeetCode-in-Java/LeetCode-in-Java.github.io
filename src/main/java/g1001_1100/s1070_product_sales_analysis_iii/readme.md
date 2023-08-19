[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1070\. Product Sales Analysis III

Medium

SQL Schema

Table: `Sales`

    +-------------+-------+ 
    | Column Name | Type  | 
    +-------------+-------+ 
    | sale_id     | int   | 
    | product_id  | int   | 
    | year        | int   | 
    | quantity    | int   | 
    | price       | int   | 
    +-------------+-------+ 

(sale_id, year) is the primary key of this table. product_id is a foreign key to `Product` table.

Each row of this table shows a sale on the product product_id in a certain year.

Note that the price is per unit.

Table: `Product`

    +--------------+---------+ 
    | Column Name  | Type    | 
    +--------------+---------+ 
    | product_id   | int     | 
    | product_name | varchar | 
    +--------------+---------+ 

product_id is the primary key of this table.

Each row of this table indicates the product name of each product.

Write an SQL query that selects the **product id**, **year**, **quantity**, and **price** for the **first year** of every product sold.

Return the resulting table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:** Sales table:

    +---------+------------+------+----------+-------+ 
    | sale_id | product_id | year | quantity | price | 
    +---------+------------+------+----------+-------+ 
    | 1       | 100        | 2008 | 10       | 5000  | 
    | 2       | 100        | 2009 | 12       | 5000  | 
    | 7       | 200        | 2011 | 15       | 9000  | 
    +---------+------------+------+----------+-------+ 

Product table:

    +------------+--------------+ 
    | product_id | product_name | 
    +------------+--------------+ 
    | 100        | Nokia        | 
    | 200        | Apple        | 
    | 300        | Samsung      | 
    +------------+--------------+

**Output:**

    +------------+------------+----------+-------+ 
    | product_id | first_year | quantity | price | 
    +------------+------------+----------+-------+ 
    | 100        | 2008       | 10       | 5000  | 
    | 200        | 2011       | 15       | 9000  | 
    +------------+------------+----------+-------+

## Solution

```sql
# Write your MySQL query statement below
WITH ab AS (SELECT *, RANK() OVER(PARTITION BY product_id ORDER BY sale_year ASC) AS rk
FROM Sales)
SELECT product_id, sale_year AS first_year, quantity, price
FROM ab
WHERE rk = 1;
```