[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2472\. Maximum Number of Non-overlapping Palindrome Substrings

Hard

You are given a string `s` and a **positive** integer `k`.

Select a set of **non-overlapping** substrings from the string `s` that satisfy the following conditions:

*   The **length** of each substring is **at least** `k`.
*   Each substring is a **palindrome**.

Return _the **maximum** number of substrings in an optimal selection_.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "abaccdbbd", k = 3

**Output:** 2

**Explanation:** We can select the substrings underlined in s = "<ins>**aba**</ins>cc<ins>**dbbd**</ins>". Both "aba" and "dbbd" are palindromes and have a length of at least k = 3. It can be shown that we cannot find a selection with more than two valid substrings.

**Example 2:**

**Input:** s = "adbcda", k = 2

**Output:** 0

**Explanation:** There is no palindrome substring of length at least 2 in the string.

**Constraints:**

*   `1 <= k <= s.length <= 2000`
*   `s` consists of lowercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxPalindromes(String s, int k) {
        int[] dp = new int[s.length()];
        Arrays.fill(dp, -1);
        return dfs(s, dp, k, 0);
    }

    private int dfs(String s, int[] dp, int k, int start) {
        if (start >= s.length()) {
            return 0;
        }
        if (dp[start] != -1) {
            return dp[start];
        }
        int ans = 0;
        for (int n = 0; n <= 1; n++) {
            for (int i = start; i <= s.length() - k - n; i++) {
                int left = i;
                int right = i + k + n - 1;
                while (left < right) {
                    if (s.charAt(left) != s.charAt(right)) {
                        break;
                    }
                    left++;
                    right--;
                }
                if (left >= right) {
                    ans = Math.max(ans, 1 + dfs(s, dp, k, i + k + n));
                    break;
                }
            }
        }
        dp[start] = ans;
        return ans;
    }
}
```