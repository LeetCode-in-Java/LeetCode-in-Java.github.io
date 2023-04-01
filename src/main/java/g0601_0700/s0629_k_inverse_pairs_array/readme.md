[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 629\. K Inverse Pairs Array

Hard

For an integer array `nums`, an **inverse pair** is a pair of integers `[i, j]` where `0 <= i < j < nums.length` and `nums[i] > nums[j]`.

Given two integers n and k, return the number of different arrays consist of numbers from `1` to `n` such that there are exactly `k` **inverse pairs**. Since the answer can be huge, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, k = 0

**Output:** 1

**Explanation:** Only the array [1,2,3] which consists of numbers from 1 to 3 has exactly 0 inverse pairs.

**Example 2:**

**Input:** n = 3, k = 1

**Output:** 2

**Explanation:** The array [1,3,2] and [2,1,3] have exactly 1 inverse pair.

**Constraints:**

*   `1 <= n <= 1000`
*   `0 <= k <= 1000`

## Solution

```java
public class Solution {
    public int kInversePairs(int n, int k) {
        k = Math.min(k, n * (n - 1) / 2 - k);
        if (k < 0) {
            return 0;
        }
        int[] dp = new int[k + 1];
        int[] dp1 = new int[k + 1];
        dp[0] = 1;
        dp1[0] = 1;
        int mod = 1_000_000_007;
        for (int i = 1; i <= n; i++) {
            int[] temp = dp;
            dp = dp1;
            dp1 = temp;
            for (int j = 1, m = Math.min(k, i * (i - 1) / 2); j <= m; j++) {
                dp[j] = (dp1[j] + dp[j - 1] - (j >= i ? dp1[j - i] : 0)) % mod;
                if (dp[j] < 0) {
                    dp[j] += mod;
                }
            }
        }
        return dp[k];
    }
}
```