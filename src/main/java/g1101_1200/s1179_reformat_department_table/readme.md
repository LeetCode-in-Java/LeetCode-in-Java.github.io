[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1179\. Reformat Department Table

Easy

SQL Schema

Table: `Department`

    +-------------+---------+
    | Column Name |  Type   |
    +-------------+---------+
    | id          | int     |
    | revenue     | int     |
    | month       | varchar |
    +-------------+---------+

(id, month) is the primary key of this table.

The table has information about the revenue of each department per month.

The month has values in ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"].

Write an SQL query to reformat the table such that there is a department id column and a revenue column **for each month**.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

Department table:

    +------+---------+-------+
    | id   | revenue | month |
    +------+---------+-------+
    | 1    | 8000    | Jan   |
    | 2    | 9000    | Jan   |
    | 3    | 10000   | Feb   |
    | 1    | 7000    | Feb   |
    | 1    | 6000    | Mar   |
    +------+---------+-------+

**Output:**

    +------+-------------+-------------+-------------+-----+-------------+
    | id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
    +------+-------------+-------------+-------------+-----+-------------+
    | 1    | 8000        | 7000        | 6000        | ... | null        |
    | 2    | 9000        | null        | null        | ... | null        |
    | 3    | null        | 10000       | null        | ... | null        |
    +------+-------------+-------------+-------------+-----+-------------+

**Explanation:** The revenue from Apr to Dec is null.

Note that the result table has 13 columns (1 for the department id + 12 for the months).

## Solution

```sql
# Write your MySQL query statement below
SELECT
id,
SUM(CASE WHEN month = 'Jan' THEN revenue ELSE NULL END) AS Jan_Revenue,
SUM(CASE WHEN month = 'Feb' THEN revenue ELSE NULL END) AS Feb_Revenue,
SUM(CASE WHEN month = 'Mar' THEN revenue ELSE NULL END) AS Mar_Revenue,
SUM(CASE WHEN month = 'Apr' THEN revenue ELSE NULL END) AS Apr_Revenue,
SUM(CASE WHEN month = 'May' THEN revenue ELSE NULL END) AS May_Revenue,
SUM(CASE WHEN month = 'Jun' THEN revenue ELSE NULL END) AS Jun_Revenue,
SUM(CASE WHEN month = 'Jul' THEN revenue ELSE NULL END) AS Jul_Revenue,
SUM(CASE WHEN month = 'Aug' THEN revenue ELSE NULL END) AS Aug_Revenue,
SUM(CASE WHEN month = 'Sep' THEN revenue ELSE NULL END) AS Sep_Revenue,
SUM(CASE WHEN month = 'Oct' THEN revenue ELSE NULL END) AS Oct_Revenue,
SUM(CASE WHEN month = 'Nov' THEN revenue ELSE NULL END) AS Nov_Revenue,
SUM(CASE WHEN month = 'Dec' THEN revenue ELSE NULL END) AS Dec_Revenue
FROM Department
GROUP BY id
```