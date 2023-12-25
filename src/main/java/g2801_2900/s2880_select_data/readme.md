[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2880\. Select Data

Easy

DataFrame students 

    +-------------+--------+ 
    | Column Name | Type   | 
    +-------------+--------+ 
    | student_id  | int    | 
    | name        | object | 
    | age         | int    | 
    +-------------+--------+

Write a solution to select the name and age of the student with `student_id = 101`.

The result format is in the following example.

**Example 1: Input:** 

    +------------+---------+-----+ 
    | student_id | name    | age | 
    +------------+---------+-----+ 
    | 101        | Ulysses | 13  | 
    | 53         | William | 10  | 
    | 128        | Henry   | 6   | 
    | 3          | Henry   | 11  | 
    +------------+---------+-----+

**Output:** 

    +---------+-----+ 
    | name    | age | 
    +---------+-----+ 
    | Ulysses | 13  | 
    +---------+-----+

**Explanation:** Student Ulysses has student_id = 101, we select the name and age.

## Solution

```python
import pandas as pd

def selectData(students: pd.DataFrame) -> pd.DataFrame:
    return students[students.student_id == 101][['name','age']]
```