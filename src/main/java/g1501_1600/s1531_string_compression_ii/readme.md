[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1531\. String Compression II

Hard

[Run-length encoding](http://en.wikipedia.org/wiki/Run-length_encoding) is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string `"aabccc"` we replace `"aa"` by `"a2"` and replace `"ccc"` by `"c3"`. Thus the compressed string becomes `"a2bc3"`.

Notice that in this problem, we are not adding `'1'` after single characters.

Given a string `s` and an integer `k`. You need to delete **at most** `k` characters from `s` such that the run-length encoded version of `s` has minimum length.

Find the _minimum length of the run-length encoded version of_ `s` _after deleting at most_ `k` _characters_.

**Example 1:**

**Input:** s = "aaabcccd", k = 2

**Output:** 4

**Explanation:** Compressing s without deleting anything will give us "a3bc3d" of length 6. Deleting any of the characters 'a' or 'c' would at most decrease the length of the compressed string to 5, for instance delete 2 'a' then we will have s = "abcccd" which compressed is abc3d. Therefore, the optimal way is to delete 'b' and 'd', then the compressed version of s will be "a3c3" of length 4.

**Example 2:**

**Input:** s = "aabbaa", k = 2

**Output:** 2

**Explanation:** If we delete both 'b' characters, the resulting compressed string would be "a4" of length 2.

**Example 3:**

**Input:** s = "aaaaaaaaaaa", k = 0

**Output:** 3

**Explanation:** Since k is zero, we cannot delete anything. The compressed string is "a11" of length 3.

**Constraints:**

*   `1 <= s.length <= 100`
*   `0 <= k <= s.length`
*   `s` contains only lowercase English letters.

## Solution

```java
public class Solution {
    private final int[][] dp = new int[101][101];

    private int getLen(int cnt) {
        if (cnt == 0) {
            return 0;
        }
        if (cnt == 1) {
            return 1;
        }
        if (cnt < 10) {
            return 2;
        }
        if (cnt < 100) {
            return 3;
        }
        return 4;
    }

    public int getLengthOfOptimalCompression(String s, int k) {
        char[] sarr = s.toCharArray();
        for (int i = 0; i <= s.length(); i++) {
            for (int j = 0; j <= k; j++) {
                dp[i][j] = -1;
            }
        }
        return dfs(sarr, 0, k);
    }

    private int dfs(char[] sarr, int pos, int k) {
        if (k < 0) {
            return -1;
        }
        if (pos == sarr.length || sarr.length - pos <= k) {
            return 0;
        }
        if (dp[pos][k] != -1) {
            return dp[pos][k];
        }
        int[] cnts = new int[26];
        int most = 0;
        int res = -1;
        for (int j = pos; j < sarr.length; j++) {
            cnts[sarr[j] - 'a']++;
            most = Math.max(most, cnts[sarr[j] - 'a']);
            int cost = dfs(sarr, j + 1, k - (j - pos + 1 - most));
            if (cost == -1) {
                continue;
            }
            if (res == -1) {
                res = cost + getLen(most);
            } else {
                res = Math.min(res, cost + getLen(most));
            }
        }
        dp[pos][k] = res;
        return res;
    }
}
```