[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 504\. Base 7

Easy

Given an integer `num`, return _a string of its **base 7** representation_.

**Example 1:**

**Input:** num = 100

**Output:** "202"

**Example 2:**

**Input:** num = -7

**Output:** "-10"

**Constraints:**

*   <code>-10<sup>7</sup> <= num <= 10<sup>7</sup></code>

## Solution

```java
public class Solution {
    public String convertToBase7(int num) {
        return Integer.toString(num, 7);
    }
}
```