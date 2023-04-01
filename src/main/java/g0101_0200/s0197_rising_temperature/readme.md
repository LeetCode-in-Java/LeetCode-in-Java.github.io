[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 197\. Rising Temperature

Easy

SQL Schema

Table: `Weather`

    +---------------+---------+
    | Column Name   | Type    |
    +---------------+---------+
    | id            | int     |
    | recordDate    | date    |
    | temperature   | int     |
    +---------------+---------+
    id is the primary key for this table.
    This table contains information about the temperature on a certain day. 

Write an SQL query to find all dates' `Id` with higher temperatures compared to its previous dates (yesterday).

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Weather table:
    +----+------------+-------------+
    | id | recordDate | temperature |
    +----+------------+-------------+
    | 1  | 2015-01-01 | 10          |
    | 2  | 2015-01-02 | 25          |
    | 3  | 2015-01-03 | 20          |
    | 4  | 2015-01-04 | 30          |
    +----+------------+-------------+

**Output:**

    +----+
    | id |
    +----+
    | 2  |
    | 4  |
    +----+

**Explanation:**

    In 2015-01-02, the temperature was higher than the previous day (10 -> 25).
    In 2015-01-04, the temperature was higher than the previous day (20 -> 30).

## Solution

```sql
# Write your MySQL query statement below
SELECT SecondDate.id as Id
FROM Weather SecondDate JOIN Weather FirstDate
ON ADDDATE(FirstDate.recordDate,1) = SecondDate.recordDate
AND SecondDate.temperature > FirstDate.temperature;
```