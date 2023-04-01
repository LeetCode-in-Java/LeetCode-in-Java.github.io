[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1621\. Number of Sets of K Non-Overlapping Line Segments

Medium

Given `n` points on a 1-D plane, where the <code>i<sup>th</sup></code> point (from `0` to `n-1`) is at `x = i`, find the number of ways we can draw **exactly** `k` **non-overlapping** line segments such that each segment covers two or more points. The endpoints of each segment must have **integral coordinates**. The `k` line segments **do not** have to cover all `n` points, and they are **allowed** to share endpoints.

Return _the number of ways we can draw_ `k` _non-overlapping line segments__._ Since this number can be huge, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/07/ex1.png)

**Input:** n = 4, k = 2

**Output:** 5

**Explanation:** The two line segments are shown in red and blue. The image above shows the 5 different ways {(0,2),(2,3)}, {(0,1),(1,3)}, {(0,1),(2,3)}, {(1,2),(2,3)}, {(0,1),(1,2)}.

**Example 2:**

**Input:** n = 3, k = 1

**Output:** 3

**Explanation:** The 3 ways are {(0,1)}, {(0,2)}, {(1,2)}.

**Example 3:**

**Input:** n = 30, k = 7

**Output:** 796297179

**Explanation:** The total number of possible ways to draw 7 line segments is 3796297200. Taking this number modulo 10<sup>9</sup> + 7 gives us 796297179.

**Constraints:**

*   `2 <= n <= 1000`
*   `1 <= k <= n-1`

## Solution

```java
public class Solution {
    public int numberOfSets(int n, int k) {
        if (n - 1 >= k) {
            int[] dp = new int[k];
            int[] sums = new int[k];
            int mod = (int) (1e9 + 7);
            for (int diff = 1; diff < n - k + 1; diff++) {
                dp[0] = ((diff + 1) * diff) >> 1;
                sums[0] = (sums[0] + dp[0]) % mod;
                for (int segments = 2; segments <= k; segments++) {
                    dp[segments - 1] = (sums[segments - 2] + dp[segments - 1]) % mod;
                    sums[segments - 1] = (sums[segments - 1] + dp[segments - 1]) % mod;
                }
            }
            return dp[k - 1];
        } else {
            return 0;
        }
    }
}
```