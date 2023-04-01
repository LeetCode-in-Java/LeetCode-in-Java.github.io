[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1342\. Number of Steps to Reduce a Number to Zero

Easy

Given an integer `num`, return _the number of steps to reduce it to zero_.

In one step, if the current number is even, you have to divide it by `2`, otherwise, you have to subtract `1` from it.

**Example 1:**

**Input:** num = 14

**Output:** 6

**Explanation:** 

Step 1) 14 is even; divide by 2 and obtain 7. 

Step 2) 7 is odd; subtract 1 and obtain 6. 

Step 3) 6 is even; divide by 2 and obtain 3. 

Step 4) 3 is odd; subtract 1 and obtain 2. 

Step 5) 2 is even; divide by 2 and obtain 1. 

Step 6) 1 is odd; subtract 1 and obtain 0.

**Example 2:**

**Input:** num = 8

**Output:** 4

**Explanation:** 

Step 1) 8 is even; divide by 2 and obtain 4. 

Step 2) 4 is even; divide by 2 and obtain 2. 

Step 3) 2 is even; divide by 2 and obtain 1. 

Step 4) 1 is odd; subtract 1 and obtain 0.

**Example 3:**

**Input:** num = 123

**Output:** 12

**Constraints:**

*   <code>0 <= num <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int numberOfSteps(int num) {
        int steps = 0;
        while (num != 0) {
            if (num % 2 == 0) {
                num /= 2;
            } else {
                num--;
            }
            steps++;
        }
        return steps;
    }
}
```