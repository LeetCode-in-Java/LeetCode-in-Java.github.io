[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1771\. Maximize Palindrome Length From Subsequences

Hard

You are given two strings, `word1` and `word2`. You want to construct a string in the following manner:

*   Choose some **non-empty** subsequence `subsequence1` from `word1`.
*   Choose some **non-empty** subsequence `subsequence2` from `word2`.
*   Concatenate the subsequences: `subsequence1 + subsequence2`, to make the string.

Return _the **length** of the longest **palindrome** that can be constructed in the described manner._ If no palindromes can be constructed, return `0`.

A **subsequence** of a string `s` is a string that can be made by deleting some (possibly none) characters from `s` without changing the order of the remaining characters.

A **palindrome** is a string that reads the same forward as well as backward.

**Example 1:**

**Input:** word1 = "cacb", word2 = "cbba"

**Output:** 5

**Explanation:** Choose "ab" from word1 and "cba" from word2 to make "abcba", which is a palindrome.

**Example 2:**

**Input:** word1 = "ab", word2 = "ab"

**Output:** 3

**Explanation:** Choose "ab" from word1 and "a" from word2 to make "aba", which is a palindrome.

**Example 3:**

**Input:** word1 = "aa", word2 = "bb"

**Output:** 0

**Explanation:** You cannot construct a palindrome from the described method, so return 0.

**Constraints:**

*   `1 <= word1.length, word2.length <= 1000`
*   `word1` and `word2` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public int longestPalindrome(String word1, String word2) {
        int len1 = word1.length();
        int len2 = word2.length();
        int len = len1 + len2;
        String word = word1 + word2;
        int[][] dp = new int[len][len];
        int max = 0;
        char[] arr = word.toCharArray();
        for (int d = 1; d <= len; d++) {
            for (int i = 0; i + d - 1 < len; i++) {
                if (arr[i] == arr[i + d - 1]) {
                    dp[i][i + d - 1] =
                            d == 1 ? 1 : Math.max(dp[i + 1][i + d - 2] + 2, dp[i][i + d - 1]);
                    if (i < len1 && i + d - 1 >= len1) {
                        max = Math.max(max, dp[i][i + d - 1]);
                    }
                } else {
                    dp[i][i + d - 1] = Math.max(dp[i + 1][i + d - 1], dp[i][i + d - 2]);
                }
            }
        }
        return max;
    }
}
```