## 1092\. Shortest Common Supersequence

Hard

Given two strings `str1` and `str2`, return _the shortest string that has both_ `str1` _and_ `str2` _as **subsequences**_. If there are multiple valid strings, return **any** of them.

A string `s` is a **subsequence** of string `t` if deleting some number of characters from `t` (possibly `0`) results in the string `s`.

**Example 1:**

**Input:** str1 = "abac", str2 = "cab"

**Output:** "cabac"

**Explanation:**

str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".

str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".

The answer provided is the shortest such string that satisfies these properties.

**Example 2:**

**Input:** str1 = "aaaaaaaa", str2 = "aaaaaaaa"

**Output:** "aaaaaaaa"

**Constraints:**

*   `1 <= str1.length, str2.length <= 1000`
*   `str1` and `str2` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int m = str1.length();
        int n = str2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                if (i == 0) {
                    dp[i][j] = j;
                } else if (j == 0) {
                    dp[i][j] = i;
                } else if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = 1 + Math.min(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        // Length of the ShortestSuperSequence
        int l = dp[m][n];
        char[] arr = new char[l];
        int i = m;
        int j = n;
        while (i > 0 && j > 0) {
            /* If current character in str1 and str2 are same, then
            current character is part of shortest supersequence */
            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                arr[--l] = str1.charAt(i - 1);
                i--;
                j--;
            } else if (dp[i - 1][j] < dp[i][j - 1]) {
                arr[--l] = str1.charAt(i - 1);
                i--;
            } else {
                arr[--l] = str2.charAt(j - 1);
                j--;
            }
        }
        while (i > 0) {
            arr[--l] = str1.charAt(i - 1);
            i--;
        }
        while (j > 0) {
            arr[--l] = str2.charAt(j - 1);
            j--;
        }
        return new String(arr);
    }
}
```