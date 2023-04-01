[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1667\. Fix Names in a Table

Easy

SQL Schema

Table: `Users`

    +----------------+---------+
    | Column Name    | Type    |
    +----------------+---------+
    | user_id        | int     |
    | name           | varchar |
    +----------------+---------+
    user_id is the primary key for this table.
    This table contains the ID and the name of the user. The name consists of only lowercase and uppercase characters. 

Write an SQL query to fix the names so that only the first character is uppercase and the rest are lowercase.

Return the result table ordered by `user_id`.

The query result format is in the following example.

**Example 1:**

**Input:**

    Users table:
    +---------+-------+
    | user_id | name  |
    +---------+-------+
    | 1       | aLice |
    | 2       | bOB   |
    +---------+-------+

**Output:**

    +---------+-------+
    | user_id | name  |
    +---------+-------+
    | 1       | Alice |
    | 2       | Bob   |
    +---------+-------+

## Solution

```sql
# Write your MySQL query statement below
select user_id, concat(upper(substring(name, 1, 1)), lower(substring(name, 2))) as name from Users order by user_id
```