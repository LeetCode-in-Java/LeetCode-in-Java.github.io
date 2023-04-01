[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1316\. Distinct Echo Substrings

Hard

Return the number of **distinct** non-empty substrings of `text` that can be written as the concatenation of some string with itself (i.e. it can be written as `a + a` where `a` is some string).

**Example 1:**

**Input:** text = "abcabcabc"

**Output:** 3

**Explanation:** The 3 substrings are "abcabc", "bcabca" and "cabcab".

**Example 2:**

**Input:** text = "leetcodeleetcode"

**Output:** 2

**Explanation:** The 2 substrings are "ee" and "leetcodeleetcode".

**Constraints:**

*   `1 <= text.length <= 2000`
*   `text` has only lowercase English letters.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    private static final int PRIME = 101;
    private static final int MOD = 1_000_000_007;

    public int distinctEchoSubstrings(String text) {
        int n = text.length();
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) {
            long hash = 0;
            for (int j = i; j < n; j++) {
                hash = hash * PRIME + (text.charAt(j) - 'a' + 1);
                hash %= MOD;
                dp[i][j] = (int) hash;
            }
        }

        Set<Integer> set = new HashSet<>();
        int res = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i; 2 * j - i + 1 < n; j++) {
                if (dp[i][j] == dp[j + 1][2 * j - i + 1] && set.add(dp[i][j])) {
                    res++;
                }
            }
        }

        return res;
    }
}
```