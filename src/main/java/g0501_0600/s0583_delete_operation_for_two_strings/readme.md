[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 583\. Delete Operation for Two Strings

Medium

Given two strings `word1` and `word2`, return _the minimum number of **steps** required to make_ `word1` _and_ `word2` _the same_.

In one **step**, you can delete exactly one character in either string.

**Example 1:**

**Input:** word1 = "sea", word2 = "eat"

**Output:** 2

**Explanation:** You need one step to make "sea" to "ea" and another step to make "eat" to "ea".

**Example 2:**

**Input:** word1 = "leetcode", word2 = "etco"

**Output:** 4

**Constraints:**

*   `1 <= word1.length, word2.length <= 500`
*   `word1` and `word2` consist of only lowercase English letters.

## Solution

```java
public class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] =
                        word1.charAt(i - 1) == word2.charAt(j - 1)
                                ? dp[i - 1][j - 1] + 1
                                : Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return m + n - 2 * dp[m][n];
    }
}
```