[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2235\. Add Two Integers

Easy

Given two integers `num1` and `num2`, return _the **sum** of the two integers_.

**Example 1:**

**Input:** num1 = 12, num2 = 5

**Output:** 17

**Explanation:** num1 is 12, num2 is 5, and their sum is 12 + 5 = 17, so 17 is returned.

**Example 2:**

**Input:** num1 = -10, num2 = 4

**Output:** -6

**Explanation:** num1 + num2 = -6, so -6 is returned.

**Constraints:**

* `-100 <= num1, num2 <= 100`

## Solution

```java
public class Solution {
    public int sum(int num1, int num2) {
        return num1 + num2;
    }
}
```