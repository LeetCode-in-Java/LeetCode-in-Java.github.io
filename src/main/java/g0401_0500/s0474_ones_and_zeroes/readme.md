[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 474\. Ones and Zeroes

Medium

You are given an array of binary strings `strs` and two integers `m` and `n`.

Return _the size of the largest subset of `strs` such that there are **at most**_ `m` `0`_'s and_ `n` `1`_'s in the subset_.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

**Example 1:**

**Input:** strs = ["10","0001","111001","1","0"], m = 5, n = 3

**Output:** 4

**Explanation:** The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4. Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}. {"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.

**Example 2:**

**Input:** strs = ["10","0","1"], m = 1, n = 1

**Output:** 2

**Explanation:** The largest subset is {"0", "1"}, so the answer is 2.

**Constraints:**

*   `1 <= strs.length <= 600`
*   `1 <= strs[i].length <= 100`
*   `strs[i]` consists only of digits `'0'` and `'1'`.
*   `1 <= m, n <= 100`

## Solution

```java
public class Solution {
    /*
     * The problem can be interpreted as:
     * What's the max number of str can we pick from strs with limitation of m "0"s and n "1"s.
     *
     * Thus we can define dp[i][j] as it stands for max number of str can we pick from strs with limitation
     * of i "0"s and j "1"s.
     *
     * For each str, assume it has a "0"s and b "1"s, we update the dp array iteratively
     * and set dp[i][j] = Math.max(dp[i][j], dp[i - a][j - b] + 1).
     * So at the end, dp[m][n] is the answer.
     */
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for (String str : strs) {
            int[] count = count(str);
            for (int i = m; i >= count[0]; i--) {
                for (int j = n; j >= count[1]; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - count[0]][j - count[1]] + 1);
                }
            }
        }
        return dp[m][n];
    }

    private int[] count(String str) {
        int[] res = new int[2];
        for (int i = 0; i < str.length(); i++) {
            res[str.charAt(i) - '0']++;
        }
        return res;
    }
}
```