[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 180\. Consecutive Numbers

Medium

SQL Schema

Table: `Logs`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | id          | int     |
    | num         | varchar |
    +-------------+---------+
    id is the primary key for this table. 

Write an SQL query to find all numbers that appear at least three times consecutively.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Logs table:
    +----+-----+
    | id | num |
    +----+-----+
    | 1  | 1   |
    | 2  | 1   |
    | 3  | 1   |
    | 4  | 2   |
    | 5  | 1   |
    | 6  | 2   |
    | 7  | 2   |
    +----+-----+

**Output:**

    +-----------------+
    | ConsecutiveNums |
    +-----------------+
    | 1               |
    +-----------------+

**Explanation:** 1 is the only number that appears consecutively for at least three times.

## Solution

```sql
# Write your MySQL query statement below
select distinct num as ConsecutiveNums from
(select num, lag(num,1) over(order by id) as l1, lag(num,2) over(order by id) as l2
from Logs) con_thr
where num = l1 and num = l2
```