[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 343\. Integer Break

Medium

Given an integer `n`, break it into the sum of `k` **positive integers**, where `k >= 2`, and maximize the product of those integers.

Return _the maximum product you can get_.

**Example 1:**

**Input:** n = 2

**Output:** 1

**Explanation:** 2 = 1 + 1, 1 × 1 = 1.

**Example 2:**

**Input:** n = 10

**Output:** 36

**Explanation:** 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.

**Constraints:**

*   `2 <= n <= 58`

## Solution

```java
public class Solution {
    private int[] arr;

    public int integerBreak(int n) {
        arr = new int[n + 1];
        arr[2] = 1;
        // only case involve with 1 other than 2 is 3
        return n == 3 ? 2 : dp(n);
    }

    private int dp(int n) {
        if (n <= 2) {
            return arr[2];
        } else if (arr[n] != 0) {
            return arr[n];
        } else {
            int prod = 1;
            for (int i = 2; i <= n; i++) {
                prod = Math.max(prod, i * dp(n - i));
            }
            arr[n] = prod;
        }
        return arr[n];
    }
}
```