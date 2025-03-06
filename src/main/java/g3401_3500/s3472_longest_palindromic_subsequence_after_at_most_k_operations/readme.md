[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3472\. Longest Palindromic Subsequence After at Most K Operations

Medium

You are given a string `s` and an integer `k`.

In one operation, you can replace the character at any position with the next or previous letter in the alphabet (wrapping around so that `'a'` is after `'z'`). For example, replacing `'a'` with the next letter results in `'b'`, and replacing `'a'` with the previous letter results in `'z'`. Similarly, replacing `'z'` with the next letter results in `'a'`, and replacing `'z'` with the previous letter results in `'y'`.

Return the length of the **longest palindromic subsequence** of `s` that can be obtained after performing **at most** `k` operations.

**Example 1:**

**Input:** s = "abced", k = 2

**Output:** 3

**Explanation:**

*   Replace `s[1]` with the next letter, and `s` becomes `"acced"`.
*   Replace `s[4]` with the previous letter, and `s` becomes `"accec"`.

The subsequence `"ccc"` forms a palindrome of length 3, which is the maximum.

**Example 2:**

**Input:** s = "aaazzz", k = 4

**Output:** 6

**Explanation:**

*   Replace `s[0]` with the previous letter, and `s` becomes `"zaazzz"`.
*   Replace `s[4]` with the next letter, and `s` becomes `"zaazaz"`.
*   Replace `s[3]` with the next letter, and `s` becomes `"zaaaaz"`.

The entire string forms a palindrome of length 6.

**Constraints:**

*   `1 <= s.length <= 200`
*   `1 <= k <= 200`
*   `s` consists of only lowercase English letters.

## Solution

```java
public class Solution {
    public int longestPalindromicSubsequence(String s, int k) {
        int n = s.length();
        int[][] arr = new int[26][26];
        for (int i = 0; i < 26; i++) {
            for (int j = 0; j < 26; j++) {
                arr[i][j] = Math.min(Math.abs(i - j), 26 - Math.abs(i - j));
            }
        }
        int[][][] dp = new int[n][n][k + 1];
        for (int i = 0; i < n; i++) {
            for (int it = 0; it <= k; it++) {
                dp[i][i][it] = 1;
            }
        }
        for (int length = 2; length <= n; length++) {
            for (int i = 0; i <= n - length; i++) {
                int j = i + length - 1;
                for (int it = 0; it <= k; it++) {
                    if (s.charAt(i) == s.charAt(j)) {
                        dp[i][j][it] = 2 + dp[i + 1][j - 1][it];
                    } else {
                        int num1 = dp[i + 1][j][it];
                        int num2 = dp[i][j - 1][it];
                        int c = arr[s.charAt(i) - 'a'][s.charAt(j) - 'a'];
                        int num3 = (it >= c) ? 2 + dp[i + 1][j - 1][it - c] : 0;
                        dp[i][j][it] = Math.max(Math.max(num1, num2), num3);
                    }
                }
            }
        }
        return dp[0][n - 1][k];
    }
}
```