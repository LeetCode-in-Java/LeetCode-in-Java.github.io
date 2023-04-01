[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2478\. Number of Beautiful Partitions

Hard

You are given a string `s` that consists of the digits `'1'` to `'9'` and two integers `k` and `minLength`.

A partition of `s` is called **beautiful** if:

*   `s` is partitioned into `k` non-intersecting substrings.
*   Each substring has a length of **at least** `minLength`.
*   Each substring starts with a **prime** digit and ends with a **non-prime** digit. Prime digits are `'2'`, `'3'`, `'5'`, and `'7'`, and the rest of the digits are non-prime.

Return _the number of **beautiful** partitions of_ `s`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "23542185131", k = 3, minLength = 2

**Output:** 3

**Explanation:** There exists three ways to create a beautiful partition: 

"2354 \| 218 \| 5131" 

"2354 \| 21851 \| 31" 

"2354218 \| 51 \| 31"

**Example 2:**

**Input:** s = "23542185131", k = 3, minLength = 3

**Output:** 1

**Explanation:** There exists one way to create a beautiful partition: "2354 \| 218 \| 5131".

**Example 3:**

**Input:** s = "3312958", k = 3, minLength = 1

**Output:** 1

**Explanation:** There exists one way to create a beautiful partition: "331 \| 29 \| 58".

**Constraints:**

*   `1 <= k, minLength <= s.length <= 1000`
*   `s` consists of the digits `'1'` to `'9'`.

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int beautifulPartitions(String s, int k, int l) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        if (!prime(cs[0]) || prime(cs[n - 1])) {
            return 0;
        }
        int[][] dp = new int[k][n];
        for (int i = n - l; 0 <= i; --i) {
            dp[0][i] = prime(cs[i]) ? 1 : 0;
        }
        for (int i = 1; i < k; ++i) {
            int sum = 0;
            for (int j = n - i * l; 0 <= j; --j) {
                int mod = (int) 1e9 + 7;
                if (0 == dp[i - 1][j]) {
                    dp[i - 1][j] = sum;
                } else if (0 != j && 0 == dp[i - 1][j - 1]) {
                    sum = (sum + dp[i - 1][j]) % mod;
                }
            }
            int p = l - 1;
            for (int j = 0; j + l * i < n; ++j) {
                if (!prime(cs[j])) {
                    continue;
                }
                p = Math.max(p, j + l - 1);
                while (prime(cs[p])) {
                    p++;
                }
                if (0 == dp[i - 1][p]) {
                    break;
                }
                dp[i][j] = dp[i - 1][p];
            }
        }
        return dp[k - 1][0];
    }

    private boolean prime(char c) {
        return '2' == c || '3' == c || '5' == c || '7' == c;
    }
}
```