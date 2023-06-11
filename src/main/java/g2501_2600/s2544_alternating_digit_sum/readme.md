[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2544\. Alternating Digit Sum

Easy

You are given a positive integer `n`. Each digit of `n` has a sign according to the following rules:

*   The **most significant digit** is assigned a **positive** sign.
*   Each other digit has an opposite sign to its adjacent digits.

Return _the sum of all digits with their corresponding sign_.

**Example 1:**

**Input:** n = 521

**Output:** 4

**Explanation:** (+5) + (-2) + (+1) = 4. 

**Example 2:**

**Input:** n = 111

**Output:** 1

**Explanation:** (+1) + (-1) + (+1) = 1. 

**Example 3:**

**Input:** n = 886996

**Output:** 0

**Explanation:** (+8) + (-8) + (+6) + (-9) + (+9) + (-6) = 0. 

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int alternateDigitSum(int n) {
        String s = Integer.toString(n);
        char[] arr = s.toCharArray();
        int res = 0;
        for (int i = 0; i < arr.length; i++) {
            res += (int) Math.pow(-1, i) * (arr[i] - '0');
        }
        return res;
    }
}
```