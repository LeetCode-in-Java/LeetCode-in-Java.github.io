[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 184\. Department Highest Salary

Medium

SQL Schema

Table: `Employee`

    +--------------+---------+
    | Column Name  | Type    |
    +--------------+---------+
    | id           | int     |
    | name         | varchar |
    | salary       | int     |
    | departmentId | int     |
    +--------------+---------+
    id is the primary key column for this table.
    departmentId is a foreign key of the ID from the `Department` table.
    Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department. 

Table: `Department`

    +-------------+---------+
    | Column Name | Type    |
    +-------------+---------+
    | id          | int     |
    | name        | varchar |
    +-------------+---------+
    id is the primary key column for this table.
    Each row of this table indicates the ID of a department and its name. 

Write an SQL query to find employees who have the highest salary in each of the departments.

Return the result table in **any order**.

The query result format is in the following example.

**Example 1:**

**Input:**

    Employee table:
    +----+-------+--------+--------------+
    | id | name  | salary | departmentId |
    +----+-------+--------+--------------+
    | 1  | Joe   | 70000  | 1            |
    | 2  | Jim   | 90000  | 1            |
    | 3  | Henry | 80000  | 2            |
    | 4  | Sam   | 60000  | 2            |
    | 5  | Max   | 90000  | 1            |
    +----+-------+--------+--------------+

    Department table:
    +----+-------+
    | id | name  |
    +----+-------+
    | 1  | IT    |
    | 2  | Sales |
    +----+-------+

**Output:**

    +------------+----------+--------+
    | Department | Employee | Salary |
    +------------+----------+--------+
    | IT         | Jim      | 90000  |
    | Sales      | Henry    | 80000  |
    | IT         | Max      | 90000  |
    +------------+----------+--------+

**Explanation:** Max and Jim both have the highest salary in the IT department and Henry has the highest salary in the Sales department.

## Solution

```sql
# Write your MySQL query statement below
SELECT
    d.Name AS Department,
    Sel.Name AS Employee,
    Sel.Salary AS Salary
FROM
    (
        SELECT
            Name,
            Salary,
            DepartmentId,
            DENSE_RANK() OVER (
                PARTITION BY DepartmentId
                ORDER BY Salary DESC
            ) AS dr
        FROM
            Employee
    ) AS Sel
    INNER JOIN Department d ON d.Id = Sel.DepartmentId
WHERE
    Sel.dr = 1;
```