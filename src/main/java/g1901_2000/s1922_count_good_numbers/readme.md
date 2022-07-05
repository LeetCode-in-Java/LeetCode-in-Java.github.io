[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1922\. Count Good Numbers

Medium

A digit string is **good** if the digits **(0-indexed)** at **even** indices are **even** and the digits at **odd** indices are **prime** (`2`, `3`, `5`, or `7`).

*   For example, `"2582"` is good because the digits (`2` and `8`) at even positions are even and the digits (`5` and `2`) at odd positions are prime. However, `"3245"` is **not** good because `3` is at an even index but is not even.

Given an integer `n`, return _the **total** number of good digit strings of length_ `n`. Since the answer may be large, **return it modulo** <code>10<sup>9</sup> + 7</code>.

A **digit string** is a string consisting of digits `0` through `9` that may contain leading zeros.

**Example 1:**

**Input:** n = 1

**Output:** 5

**Explanation:** The good numbers of length 1 are "0", "2", "4", "6", "8".

**Example 2:**

**Input:** n = 4

**Output:** 400

**Example 3:**

**Input:** n = 50

**Output:** 564908303

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    public int countGoodNumbers(long n) {
        long mod = 1000000007L;
        long result = n % 2 == 0 ? 1L : 5L;

        long base = 20L;
        long time = n / 2L;
        while (time > 0) {
            if (time % 2L > 0) {
                result *= base;
                result %= mod;
            }
            time /= 2L;
            base = base * base % mod;
        }
        return (int) result;
    }
}
```