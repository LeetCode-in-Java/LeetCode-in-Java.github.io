[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 258\. Add Digits

Easy

Given an integer `num`, repeatedly add all its digits until the result has only one digit, and return it.

**Example 1:**

**Input:** num = 38

**Output:** 2

**Explanation:**

    The process is
    38 --> 3 + 8 --> 11
    11 --> 1 + 1 --> 2
    Since 2 has only one digit, return it. 

**Example 2:**

**Input:** num = 0

**Output:** 0 

**Constraints:**

*   <code>0 <= num <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you do it without any loop/recursion in `O(1)` runtime?

## Solution

```java
public class Solution {
    public int addDigits(int num) {
        if (num == 0) {
            return 0;
        }
        if (num % 9 == 0) {
            return 9;
        }
        return num % 9;
    }
}
```