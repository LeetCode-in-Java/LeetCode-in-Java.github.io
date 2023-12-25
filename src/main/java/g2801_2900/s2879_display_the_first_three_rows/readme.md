[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2879\. Display the First Three Rows

Easy

DataFrame: `employees` 

    +-------------+--------+ 
    | Column Name | Type   | 
    +-------------+--------+ 
    | employee_id | int    | 
    | name        | object | 
    | department  | object | 
    | salary      | int    | 
    +-------------+--------+

Write a solution to display the **first `3`** rows of this DataFrame.

**Example 1:**

**Input:** DataFrame employees 

    +-------------+-----------+-----------------------+--------+ 
    | employee_id | name      | department            | salary | 
    +-------------+-----------+-----------------------+--------+ 
    | 3           | Bob       | Operations            | 48675  | 
    | 90          | Alice     | Sales                 | 11096  | 
    | 9           | Tatiana   | Engineering           | 33805  | 
    | 60          | Annabelle | InformationTechnology | 37678  | 
    | 49          | Jonathan  | HumanResources        | 23793  | 
    | 43          | Khaled    | Administration        | 40454  | 
    +-------------+-----------+-----------------------+--------+

**Output:** 

    +-------------+---------+-------------+--------+ 
    | employee_id | name    | department  | salary | 
    +-------------+---------+-------------+--------+ 
    | 3           | Bob     | Operations  | 48675  | 
    | 90          | Alice   | Sales       | 11096  | 
    | 9           | Tatiana | Engineering | 33805  | 
    +-------------+---------+-------------+--------+

**Explanation:** Only the first 3 rows are displayed.

## Solution

```python
import pandas as pd

def selectFirstRows(zs: pd.DataFrame) -> pd.DataFrame:
    return zs.head(3)
```