[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1638\. Count Substrings That Differ by One Character

Medium

Given two strings `s` and `t`, find the number of ways you can choose a non-empty substring of `s` and replace a **single character** by a different character such that the resulting substring is a substring of `t`. In other words, find the number of substrings in `s` that differ from some substring in `t` by **exactly** one character.

For example, the underlined substrings in `"computer"` and `"computation"` only differ by the `'e'`/`'a'`, so this is a valid way.

Return _the number of substrings that satisfy the condition above._

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "aba", t = "baba"

**Output:** 6

**Explanation:** The following are the pairs of substrings from s and t that differ by exactly 1 character: 

("aba", "baba") 

("aba", "baba")

("aba", "baba") 

("aba", "baba")

("aba", "baba") 

("aba", "baba") 

The underlined portions are the substrings that are chosen from s and t.

**Example 2:**

**Input:** s = "ab", t = "bb"

**Output:** 3

**Explanation:** The following are the pairs of substrings from s and t that differ by 1 character: 

("ab", "bb") 

("ab", "bb")

("ab", "bb") 

The underlined portions are the substrings that are chosen from s and t.

**Constraints:**

*   `1 <= s.length, t.length <= 100`
*   `s` and `t` consist of lowercase English letters only.

## Solution

```java
public class Solution {
    public int countSubstrings(String s, String t) {
        int ans = 0;
        int n = s.length();
        int m = t.length();
        int[][] dp = new int[n + 1][m + 1];
        int[][] tp = new int[n + 1][m + 1];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                if (s.charAt(i) == t.charAt(j)) {
                    dp[i][j] = dp[i + 1][j + 1] + 1;
                    tp[i][j] = tp[i + 1][j + 1];
                } else {
                    tp[i][j] = dp[i + 1][j + 1] + 1;
                }
                ans = ans + tp[i][j];
            }
        }
        return ans;
    }
}
```