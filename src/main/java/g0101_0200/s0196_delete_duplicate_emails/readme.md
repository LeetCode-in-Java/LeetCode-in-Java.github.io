[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 196\. Delete Duplicate Emails

Easy

SQL Schema

Table: `Person`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | id          | int     |
    | email       | varchar |
    +-------------+---------+
    id is the primary key column for this table.
    Each row of this table contains an email. The emails will not contain uppercase letters. 

Write an SQL query to **delete** all the duplicate emails, keeping only one unique email with the smallest `id`.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Person table:
    +----+------------------+
    | id | email            |
    +----+------------------+
    | 1  | john@example.com |
    | 2  | bob@example.com  |
    | 3  | john@example.com |
    +----+------------------+

**Output:**

    +----+------------------+
    | id | email            |
    +----+------------------+
    | 1  | john@example.com |
    | 2  | bob@example.com  |
    +----+------------------+

**Explanation:** john@example.com is repeated two times. We keep the row with the smallest Id = 1.

## Solution

```sql
# Write your MySQL query statement below
DELETE FROM Person
WHERE Id NOT IN (SELECT id FROM (SELECT Email, MIN(Id) AS id FROM Person GROUP BY Email) t)
```