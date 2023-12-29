[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2929\. Distribute Candies Among Children II

Medium

You are given two positive integers `n` and `limit`.

Return _the **total number** of ways to distribute_ `n` _candies among_ `3` _children such that no child gets more than_ `limit` _candies._

**Example 1:**

**Input:** n = 5, limit = 2

**Output:** 3

**Explanation:** There are 3 ways to distribute 5 candies such that no child gets more than 2 candies: (1, 2, 2), (2, 1, 2) and (2, 2, 1).

**Example 2:**

**Input:** n = 3, limit = 3

**Output:** 10

**Explanation:** There are 10 ways to distribute 3 candies such that no child gets more than 3 candies: (0, 0, 3), (0, 1, 2), (0, 2, 1), (0, 3, 0), (1, 0, 2), (1, 1, 1), (1, 2, 0), (2, 0, 1), (2, 1, 0) and (3, 0, 0).

**Constraints:**

*   <code>1 <= n <= 10<sup>6</sup></code>
*   <code>1 <= limit <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public long distributeCandies(long n, long k) {
        if (n <= k) {
            return (n + 1) * (n + 2) / 2;
        }
        if (n <= 2 * k) {
            return (k + 1) * (k + 1)
                    - (n - k) * (n - k + 1) / 2
                    - (2 * k - n) * (2 * k - n + 1) / 2;
        }
        if (n <= 3 * k) {
            return (3 * k - n + 1) * (3 * k - n + 2) / 2;
        }
        return 0;
    }
}
```