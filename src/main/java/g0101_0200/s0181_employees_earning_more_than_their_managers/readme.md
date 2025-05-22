[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 181\. Employees Earning More Than Their Managers

Easy

SQL Schema

Table: `Employee`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | id          | int     |
    | name        | varchar |
    | salary      | int     |
    | managerId   | int     |
    +-------------+---------+
    id is the primary key column for this table.
    Each row of this table indicates the ID of an employee, their name, salary, and the ID of their manager. 

Write an SQL query to find the employees who earn more than their managers.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Employee table:
    +----+-------+--------+-----------+
    | id | name  | salary | managerId |
    +----+-------+--------+-----------+
    | 1  | Joe   | 70000  | 3         |
    | 2  | Henry | 80000  | 4         |
    | 3  | Sam   | 60000  | Null      |
    | 4  | Max   | 90000  | Null      |
    +----+-------+--------+-----------+

**Output:**

    +----------+
    | Employee |
    +----------+
    | Joe      |
    +----------+

**Explanation:** Joe is the only employee who earns more than his manager.

## Solution

```sql
# Write your MySQL query statement below
SELECT
    a.Name AS Employee
FROM
    Employee a
    LEFT JOIN Employee b ON a.ManagerId = b.Id
WHERE
    a.Salary > b.Salary
    AND a.ManagerId IS NOT NULL;
```