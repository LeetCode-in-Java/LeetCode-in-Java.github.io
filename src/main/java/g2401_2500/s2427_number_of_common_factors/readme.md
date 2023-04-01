[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2427\. Number of Common Factors

Easy

Given two positive integers `a` and `b`, return _the number of **common** factors of_ `a` _and_ `b`.

An integer `x` is a **common factor** of `a` and `b` if `x` divides both `a` and `b`.

**Example 1:**

**Input:** a = 12, b = 6

**Output:** 4

**Explanation:** The common factors of 12 and 6 are 1, 2, 3, 6.

**Example 2:**

**Input:** a = 25, b = 30

**Output:** 2

**Explanation:** The common factors of 25 and 30 are 1, 5.

**Constraints:**

*   `1 <= a, b <= 1000`

## Solution

```java
public class Solution {
    public int commonFactors(int a, int b) {
        int ans = 0;
        for (int i = 1; i <= Math.min(a, b); i++) {
            if (a % i == 0 && b % i == 0) {
                ans++;
            }
        }
        return ans;
    }
}
```