[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2883\. Drop Missing Data

Easy

DataFrame students 

    +-------------+--------+ 
    | Column Name | Type   | 
    +-------------+--------+ 
    | student_id | int    | 
    | name        | object | 
    | age         | int    | 
    +-------------+--------+

There are some rows having missing values in the `name` column.

Write a solution to remove the rows with missing values.

The result format is in the following example.

**Example 1:**

**Input:** 

    +------------+---------+-----+ 
    | student_id | name    | age | 
    +------------+---------+-----+ 
    | 32         | Piper   | 5   | 
    | 217        | None    | 19  | 
    | 779        | Georgia | 20  | 
    | 849        | Willow  | 14  | 
    +------------+---------+-----+

**Output:** 

    +------------+---------+-----+ 
    | student_id | name    | age | 
    +------------+---------+-----+ 
    | 32         | Piper   | 5   | 
    | 779        | Georgia | 20  | 
    | 849        | Willow  | 14  | 
    +------------+---------+-----+

**Explanation:** Student with id 217 havs empty value in the name column, so it will be removed.

## Solution

```python
import pandas as pd

def dropMissingData(students: pd.DataFrame) -> pd.DataFrame:
   r = pd.DataFrame(students)
   r.dropna(subset='name', inplace=True)
   return r
```