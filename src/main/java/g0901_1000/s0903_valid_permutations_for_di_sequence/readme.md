[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 903\. Valid Permutations for DI Sequence

Hard

You are given a string `s` of length `n` where `s[i]` is either:

*   `'D'` means decreasing, or
*   `'I'` means increasing.

A permutation `perm` of `n + 1` integers of all the integers in the range `[0, n]` is called a **valid permutation** if for all valid `i`:

*   If `s[i] == 'D'`, then `perm[i] > perm[i + 1]`, and
*   If `s[i] == 'I'`, then `perm[i] < perm[i + 1]`.

Return _the number of **valid permutations**_ `perm`. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "DID"

**Output:** 5

**Explanation:** The 5 valid permutations of (0, 1, 2, 3) are: (1, 0, 3, 2) (2, 0, 3, 1) (2, 1, 3, 0) (3, 0, 2, 1) (3, 1, 2, 0)

**Example 2:**

**Input:** s = "D"

**Output:** 1

**Constraints:**

*   `n == s.length`
*   `1 <= n <= 200`
*   `s[i]` is either `'I'` or `'D'`.

## Solution

```java
public class Solution {
    public int numPermsDISequence(String s) {
        int n = s.length();
        int mod = (int) 1e9 + 7;
        int[][] dp = new int[n + 1][n + 1];
        for (int j = 0; j <= n; j++) {
            dp[0][j] = 1;
        }
        for (int i = 0; i < n; i++) {
            int cur = 0;
            if (s.charAt(i) == 'I') {
                for (int j = 0; j < n - i; j++) {
                    cur = (cur + dp[i][j]) % mod;
                    dp[i + 1][j] = cur;
                }
            } else {
                for (int j = n - i - 1; j >= 0; j--) {
                    cur = (cur + dp[i][j + 1]) % mod;
                    dp[i + 1][j] = cur;
                }
            }
        }
        return dp[n][0];
    }
}
```