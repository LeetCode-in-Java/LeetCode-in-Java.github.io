[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2881\. Create a New Column

Easy

DataFrame `employees` 

    +-------------+--------+ 
    | Column Name | Type.  | 
    +-------------+--------+ 
    | name        | object | 
    | salary      | int.   | 
    +-------------+--------+

A company plans to provide its employees with a bonus.

Write a solution to create a new column name `bonus` that contains the **doubled values** of the `salary` column.

The result format is in the following example.

**Example 1:**

**Input:** DataFrame employees 

    +---------+--------+ 
    | name    | salary | 
    +---------+--------+ 
    | Piper   | 4548   | 
    | Grace   | 28150  | 
    | Georgia | 1103   | 
    | Willow  | 6593   | 
    | Finn    | 74576  | 
    | Thomas  | 24433  | 
    +---------+--------+

**Output:** 

    +---------+--------+--------+ 
    | name    | salary | bonus  | 
    +---------+--------+--------+ 
    | Piper   | 4548   | 9096   | 
    | Grace   | 28150  | 56300  | 
    | Georgia | 1103   | 2206   | 
    | Willow  | 6593   | 13186  | 
    | Finn    | 74576  | 149152 | 
    | Thomas  | 24433  | 48866  | 
    +---------+--------+--------+

**Explanation:** A new column bonus is created by doubling the value in the column salary.

## Solution

```python
import pandas as pd

def createBonusColumn(employees: pd.DataFrame) -> pd.DataFrame:
    employees["bonus"] = employees["salary"] * 2
    return employees
```