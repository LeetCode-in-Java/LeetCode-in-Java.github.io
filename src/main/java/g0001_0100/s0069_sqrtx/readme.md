[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 69\. Sqrt(x)

Easy

Given a non-negative integer `x`, return _the square root of_ `x` _rounded down to the nearest integer_. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

*   For example, do not use `pow(x, 0.5)` in c++ or <code>x ** 0.5</code> in python.

**Example 1:**

**Input:** x = 4

**Output:** 2

**Explanation:** The square root of 4 is 2, so we return 2. 

**Example 2:**

**Input:** x = 8

**Output:** 2

**Explanation:** The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned. 

**Constraints:**

*   <code>0 <= x <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public int mySqrt(int x) {
        int start = 1;
        int end = x / 2;
        int sqrt = start + (end - start) / 2;
        if (x == 0) {
            return 0;
        }
        while (start <= end) {
            if (sqrt == x / sqrt) {
                return sqrt;
            } else if (sqrt > x / sqrt) {
                end = sqrt - 1;
            } else if (sqrt < x / sqrt) {
                start = sqrt + 1;
            }
            sqrt = start + (end - start) / 2;
        }
        if (sqrt > x / sqrt) {
            return sqrt - 1;
        } else {
            return sqrt;
        }
    }
}
```