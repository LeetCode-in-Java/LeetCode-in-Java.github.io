[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1143\. Longest Common Subsequence

Medium

Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

*   For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace"

**Output:** 3

**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"

**Output:** 3

**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"

**Output:** 0

**Explanation:** There is no such common subsequence, so the result is 0.

**Constraints:**

*   `1 <= text1.length, text2.length <= 1000`
*   `text1` and `text2` consist of only lowercase English characters.

## Solution

```java
public class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n = text1.length();
        int m = text2.length();
        int[][] dp = new int[n + 1][m + 1];
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[n][m];
    }
}
```

**Time Complexity (Big O Time):**

The program uses dynamic programming to fill in a 2D array `dp` of dimensions `(n+1) x (m+1)`, where `n` is the length of `text1`, and `m` is the length of `text2`. It iterates through this 2D array using nested loops:

- The outer loop runs `n` times, where `n` is the length of `text1`.
- The inner loop runs `m` times, where `m` is the length of `text2`.

Inside the nested loops, each iteration involves simple constant-time operations (comparisons, assignments, and max calculations). Therefore, the overall time complexity of the program is O(n * m), where 'n' and 'm' are the lengths of the input strings `text1` and `text2`, respectively.

**Space Complexity (Big O Space):**

The program uses a 2D array `dp` of dimensions `(n+1) x (m+1)` to store intermediate results for dynamic programming. As such, the space complexity is determined by the size of this array:

- The number of rows in `dp` is `(n+1)` where 'n' is the length of `text1`.
- The number of columns in `dp` is `(m+1)` where 'm' is the length of `text2`.

Therefore, the space complexity of the program is O(n * m) because it depends on the lengths of both input strings `text1` and `text2`.

In summary, the provided program has a time complexity of O(n * m) and a space complexity of O(n * m), where 'n' and 'm' are the lengths of the input strings `text1` and `text2`, respectively.
