[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2877\. Create a DataFrame from List

Easy

Write a solution to **create** a DataFrame from a 2D list called `student_data`. This 2D list contains the IDs and ages of some students.

The DataFrame should have two columns, `student_id` and `age`, and be in the same order as the original 2D list.

The result format is in the following example.

**Example 1:**

**Input:** student\_data: `[ [1, 15], [2, 11], [3, 11], [4, 20] ]`

**Output:** 
    
    +------------+-----+ 
    | student_id | age | 
    +------------+-----+ 
    | 1          | 15  | 
    | 2          | 11  | 
    | 3          | 11  | 
    | 4          | 20  | 
    +------------+-----+

**Explanation:** A DataFrame was created on top of student\_data, with two columns named `student_id` and `age`.

## Solution

```python
import pandas as pd

def createDataframe(student_data: List[List[int]]) -> pd.DataFrame:
    column_name = ['student_id','age']
    result = pd.DataFrame(student_data, columns=column_name)
    return result
```