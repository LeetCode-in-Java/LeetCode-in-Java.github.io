[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 596\. Classes More Than 5 Students

Easy

SQL Schema

Table: `Courses`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | student     | varchar |
    | class       | varchar |
    +-------------+---------+
    (student, class) is the primary key column for this table.
    Each row of this table indicates the name of a student and the class in which they are enrolled. 

Write an SQL query to report all the classes that have **at least five students**.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Courses table:
    +---------+----------+
    | student | class    |
    +---------+----------+
    | A       | Math     |
    | B       | English  |
    | C       | Math     |
    | D       | Biology  |
    | E       | Math     |
    | F       | Computer |
    | G       | Math     |
    | H       | Math     |
    | I       | Math     |
    +---------+----------+

**Output:**

    +---------+
    | class   |
    +---------+
    | Math    |
    +---------+

**Explanation:**

    - Math has 6 students, so we include it.
    - English has 1 student, so we do not include it.
    - Biology has 1 student, so we do not include it.
    - Computer has 1 student, so we do not include it.

## Solution

```sql
# Write your MySQL query statement below
select class
from Courses
group by class
having count(student) >= 5
```