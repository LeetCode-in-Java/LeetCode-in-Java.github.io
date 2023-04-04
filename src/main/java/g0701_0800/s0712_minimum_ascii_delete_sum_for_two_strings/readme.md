[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 712\. Minimum ASCII Delete Sum for Two Strings

Medium

Given two strings `s1` and `s2`, return _the lowest **ASCII** sum of deleted characters to make two strings equal_.

**Example 1:**

**Input:** s1 = "sea", s2 = "eat"

**Output:** 231

**Explanation:** Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum. Deleting "t" from "eat" adds 116 to the sum. At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.

**Example 2:**

**Input:** s1 = "delete", s2 = "leet"

**Output:** 403

**Explanation:** Deleting "dee" from "delete" to turn the string into "let", adds 100[d] + 101[e] + 101[e] to the sum. Deleting "e" from "leet" adds 101[e] to the sum. At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403. If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.

**Constraints:**

*   `1 <= s1.length, s2.length <= 1000`
*   `s1` and `s2` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int len1 = s1.length();
        int len2 = s2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];
        char c1;
        char c2;
        for (int i = 1; i < len1 + 1; i++) {
            dp[i][0] = dp[i - 1][0] + s1.charAt(i - 1);
        }
        for (int j = 1; j < len2 + 1; j++) {
            dp[0][j] = dp[0][j - 1] + s2.charAt(j - 1);
        }
        for (int i = 1; i < len1 + 1; i++) {
            c1 = s1.charAt(i - 1);
            for (int j = 1; j < len2 + 1; j++) {
                c2 = s2.charAt(j - 1);
                if (c1 == c2) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j] + c1, dp[i][j - 1] + c2);
                }
            }
        }
        return dp[len1][len2];
    }
}
```