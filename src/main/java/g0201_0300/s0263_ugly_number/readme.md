[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 263\. Ugly Number

Easy

An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return `true` _if_ `n` _is an **ugly number**_.

**Example 1:**

**Input:** n = 6

**Output:** true

**Explanation:** 6 = 2 × 3

**Example 2:**

**Input:** n = 8

**Output:** true

**Explanation:** 8 = 2 × 2 × 2 

**Example 3:**

**Input:** n = 14

**Output:** false

**Explanation:** 14 is not ugly since it includes the prime factor 7. 

**Example 4:**

**Input:** n = 1

**Output:** true

**Explanation:** 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5. 

**Constraints:**

*   <code>-2<sup>31</sup> <= n <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public boolean isUgly(int n) {
        if (n == 1) {
            return true;
        } else if (n <= 0) {
            return false;
        }
        int[] factors = new int[] {2, 3, 5};
        for (int factor : factors) {
            while (n > 1 && n % factor == 0) {
                n /= factor;
            }
        }
        return n == 1;
    }
}
```