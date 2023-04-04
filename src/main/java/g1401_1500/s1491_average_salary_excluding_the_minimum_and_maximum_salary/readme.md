[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1491\. Average Salary Excluding the Minimum and Maximum Salary

Easy

You are given an array of **unique** integers `salary` where `salary[i]` is the salary of the <code>i<sup>th</sup></code> employee.

Return _the average salary of employees excluding the minimum and maximum salary_. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

**Input:** salary = [4000,3000,1000,2000]

**Output:** 2500.00000

**Explanation:** Minimum salary and maximum salary are 1000 and 4000 respectively.

Average salary excluding minimum and maximum salary is (2000+3000) / 2 = 2500

**Example 2:**

**Input:** salary = [1000,2000,3000]

**Output:** 2000.00000

**Explanation:** Minimum salary and maximum salary are 1000 and 3000 respectively.

Average salary excluding minimum and maximum salary is (2000) / 1 = 2000

**Constraints:**

*   `3 <= salary.length <= 100`
*   <code>1000 <= salary[i] <= 10<sup>6</sup></code>
*   All the integers of `salary` are **unique**.

## Solution

```java
public class Solution {
    public double average(int[] salary) {
        int n = salary.length;
        int min = salary[0];
        int max = salary[0];
        int sum = salary[0];
        for (int i = 1; i < n; ++i) {
            if (salary[i] < min) {
                min = salary[i];
            } else if (salary[i] > max) {
                max = salary[i];
            }
            sum += salary[i];
        }
        return (double) (sum - min - max) / (n - 2);
    }
}
```