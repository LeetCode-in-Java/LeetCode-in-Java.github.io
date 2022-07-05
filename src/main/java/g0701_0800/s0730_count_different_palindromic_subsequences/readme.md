[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 730\. Count Different Palindromic Subsequences

Hard

Given a string s, return _the number of different non-empty palindromic subsequences in_ `s`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A subsequence of a string is obtained by deleting zero or more characters from the string.

A sequence is palindromic if it is equal to the sequence reversed.

Two sequences <code>a<sub>1</sub>, a<sub>2</sub>, ...</code> and <code>b<sub>1</sub>, b<sub>2</sub>, ...</code> are different if there is some `i` for which <code>a<sub>i</sub> != b<sub>i</sub></code>.

**Example 1:**

**Input:** s = "bccb"

**Output:** 6

**Explanation:** The 6 different non-empty palindromic subsequences are 'b', 'c', 'bb', 'cc', 'bcb', 'bccb'. Note that 'bcb' is counted only once, even though it occurs twice.

**Example 2:**

**Input:** s = "abcdabcdabcdabcdabcdabcdabcdabcddcbadcbadcbadcbadcbadcbadcbadcba"

**Output:** 104860361

**Explanation:** There are 3104860382 different non-empty palindromic subsequences, which is 104860361 modulo 10<sup>9</sup> + 7.

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'a'`, `'b'`, `'c'`, or `'d'`.

## Solution

```java
public class Solution {
    public int countPalindromicSubsequences(String s) {
        int big = 1000000007;
        int len = s.length();
        if (len < 2) {
            return len;
        }
        int[][] dp = new int[len][len];
        for (int i = 0; i < len; i++) {
            char c = s.charAt(i);
            int deta = 1;
            dp[i][i] = 1;
            int l2 = -1;
            for (int j = i - 1; j >= 0; j--) {
                if (s.charAt(j) == c) {
                    if (l2 < 0) {
                        l2 = j;
                        deta = dp[j + 1][i - 1] + 1;
                    } else {
                        deta = dp[j + 1][i - 1] - dp[j + 1][l2 - 1];
                    }
                    deta = deal(deta, big);
                }
                dp[j][i] = deal(dp[j][i - 1] + deta, big);
            }
        }
        return deal(dp[0][len - 1], big);
    }

    private int deal(int x, int big) {
        x %= big;
        if (x < 0) {
            x += big;
        }
        return x;
    }
}
```