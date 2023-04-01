[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 633\. Sum of Square Numbers

Medium

Given a non-negative integer `c`, decide whether there're two integers `a` and `b` such that <code>a<sup>2</sup> + b<sup>2</sup> = c</code>.

**Example 1:**

**Input:** c = 5

**Output:** true

**Explanation:** 1 \* 1 + 2 \* 2 = 5

**Example 2:**

**Input:** c = 3

**Output:** false

**Constraints:**

*   <code>0 <= c <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public boolean judgeSquareSum(int c) {
        int right = (int) Math.sqrt(c);
        int left = (int) Math.sqrt((double) c / 2);
        for (int i = left; i <= right; i++) {
            int j = (int) Math.sqrt(c - (double) (i * i));
            if (i * i + j * j == c) {
                return true;
            }
        }
        return false;
    }
}
```