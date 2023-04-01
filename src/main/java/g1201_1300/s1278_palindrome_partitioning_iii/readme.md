[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1278\. Palindrome Partitioning III

Hard

You are given a string `s` containing lowercase letters and an integer `k`. You need to :

*   First, change some characters of `s` to other lowercase English letters.
*   Then divide `s` into `k` non-empty disjoint substrings such that each substring is a palindrome.

Return _the minimal number of characters that you need to change to divide the string_.

**Example 1:**

**Input:** s = "abc", k = 2

**Output:** 1

**Explanation:** You can split the string into "ab" and "c", and change 1 character in "ab" to make it palindrome.

**Example 2:**

**Input:** s = "aabbc", k = 3

**Output:** 0

**Explanation:** You can split the string into "aa", "bb" and "c", all of them are palindrome.

**Example 3:**

**Input:** s = "leetcode", k = 8

**Output:** 0

**Constraints:**

*   `1 <= k <= s.length <= 100`.
*   `s` only contains lowercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int palindromePartition(String s, int k) {
        int n = s.length();
        int[][] pal = new int[n + 1][n + 1];
        fillPal(s, n, pal);
        int[][] dp = new int[n + 1][k + 1];
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        return calculateMinCost(s, 0, n, k, pal, dp);
    }

    private int calculateMinCost(String s, int index, int n, int k, int[][] pal, int[][] dp) {
        if (index == n) {
            return n;
        }
        if (k == 1) {
            return pal[index][n - 1];
        }
        if (dp[index][k] != -1) {
            return dp[index][k];
        }
        int ans = Integer.MAX_VALUE;
        for (int i = index; i < n; i++) {
            ans = Math.min(ans, pal[index][i] + calculateMinCost(s, i + 1, n, k - 1, pal, dp));
        }
        dp[index][k] = ans;
        return ans;
    }

    private void fillPal(String s, int n, int[][] pal) {
        for (int gap = 0; gap < n; gap++) {
            for (int i = 0, j = gap; j < n; i++, j++) {
                if (gap == 0) {
                    pal[i][i] = 0;
                } else if (gap == 1) {
                    if (s.charAt(i) == s.charAt(i + 1)) {
                        pal[i][i + 1] = 0;
                    } else {
                        pal[i][i + 1] = 1;
                    }
                } else {
                    if (s.charAt(i) == s.charAt(j)) {
                        pal[i][j] = pal[i + 1][j - 1];
                    } else {
                        pal[i][j] = pal[i + 1][j - 1] + 1;
                    }
                }
            }
        }
    }
}
```