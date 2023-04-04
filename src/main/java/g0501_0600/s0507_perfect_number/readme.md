[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 507\. Perfect Number

Easy

A [**perfect number**](https://en.wikipedia.org/wiki/Perfect_number) is a **positive integer** that is equal to the sum of its **positive divisors**, excluding the number itself. A **divisor** of an integer `x` is an integer that can divide `x` evenly.

Given an integer `n`, return `true` _if_ `n` _is a perfect number, otherwise return_ `false`.

**Example 1:**

**Input:** num = 28

**Output:** true

**Explanation:** 28 = 1 + 2 + 4 + 7 + 14 1, 2, 4, 7, and 14 are all divisors of 28.

**Example 2:**

**Input:** num = 7

**Output:** false

**Constraints:**

*   <code>1 <= num <= 10<sup>8</sup></code>

## Solution

```java
public class Solution {
    public boolean checkPerfectNumber(int num) {
        int s = 1;
        for (int sq = (int) Math.sqrt(num); sq > 1; sq--) {
            if (num % sq == 0) {
                s += sq + (num / sq);
            }
        }
        return num != 1 && s == num;
    }
}
```