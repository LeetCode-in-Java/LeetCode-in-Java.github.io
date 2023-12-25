[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2891\. Method Chaining

Easy

DataFrame `animals` 

    +-------------+--------+ 
    | Column Name | Type   | 
    +-------------+--------+ 
    | name        | object | 
    | species     | object | 
    | age         | int    | 
    | weight      | int    | 
    +-------------+--------+

Write a solution to list the names of animals that weigh **strictly more than** `100` kilograms.

Return the animals sorted by weight in **descending order**.

The result format is in the following example.

**Example 1:**

**Input:** DataFrame animals: 

    +----------+---------+-----+--------+ 
    | name     | species | age | weight | 
    +----------+---------+-----+--------+ 
    | Tatiana  | Snake   | 98  | 464    | 
    | Khaled   | Giraffe | 50  | 41     | 
    | Alex     | Leopard | 6   | 328    | 
    | Jonathan | Monkey  | 45  | 463    | 
    | Stefan   | Bear    | 100 | 50     | 
    | Tommy    | Panda   | 26  | 349    | 
    +----------+---------+-----+--------+

**Output:**

    +----------+ 
    | name     | 
    +----------+ 
    | Tatiana  | 
    | Jonathan | 
    | Tommy    | 
    | Alex     | 
    +----------+

**Explanation:** All animals weighing more than 100 should be included in the results table. Tatiana's weight is 464, Jonathan's weight is 463, Tommy's weight is 349, and Alex's weight is 328. The results should be sorted in descending order of weight.

In Pandas, **method chaining** enables us to perform operations on a DataFrame without breaking up each operation into a separate line or creating multiple temporary variables.

Can you complete this task in just **one line** of code using method chaining?

## Solution

```python
import pandas as pd

def findHeavyAnimals(animals: pd.DataFrame) -> pd.DataFrame:
    animal_data = {}
    for index in animals.index:
        animal = animals.iloc[index]
        if animal['weight'] > 100:
            animal_data[animal['name']] = animal['weight']

    animal_data = dict(sorted(animal_data.items() , key = lambda x : x[1] , reverse = True))
    result = pd.DataFrame(animal_data.keys() , columns = ['name'])
    return result
```