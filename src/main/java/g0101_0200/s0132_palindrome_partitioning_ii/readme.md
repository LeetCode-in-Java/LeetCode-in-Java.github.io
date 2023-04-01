[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 132\. Palindrome Partitioning II

Hard

Given a string `s`, partition `s` such that every substring of the partition is a palindrome.

Return _the minimum cuts needed_ for a palindrome partitioning of `s`.

**Example 1:**

**Input:** s = "aab"

**Output:** 1

**Explanation:** The palindrome partitioning ["aa","b"] could be produced using 1 cut. 

**Example 2:**

**Input:** s = "a"

**Output:** 0 

**Example 3:**

**Input:** s = "ab"

**Output:** 1 

**Constraints:**

*   `1 <= s.length <= 2000`
*   `s` consists of lower-case English letters only.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minCut(String s) {
        int n = s.length();
        char[] t = s.toCharArray();
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = -1;
        int i = 0;
        while (i < n) {
            expandAround(t, i, i, dp);
            expandAround(t, i, i + 1, dp);
            i++;
        }
        return dp[n];
    }

    private void expandAround(char[] t, int i, int j, int[] dp) {
        while (i >= 0 && j < t.length && t[i] == t[j]) {
            dp[j + 1] = Math.min(dp[j + 1], dp[i] + 1);
            i--;
            j++;
        }
    }
}
```