[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3473\. Sum of K Subarrays With Length at Least M

Medium

You are given an integer array `nums` and two integers, `k` and `m`.

Return the **maximum** sum of `k` non-overlapping subarrays of `nums`, where each subarray has a length of **at least** `m`.

**Example 1:**

**Input:** nums = [1,2,-1,3,3,4], k = 2, m = 2

**Output:** 13

**Explanation:**

The optimal choice is:

*   Subarray `nums[3..5]` with sum `3 + 3 + 4 = 10` (length is `3 >= m`).
*   Subarray `nums[0..1]` with sum `1 + 2 = 3` (length is `2 >= m`).

The total sum is `10 + 3 = 13`.

**Example 2:**

**Input:** nums = [-10,3,-1,-2], k = 4, m = 1

**Output:** \-10

**Explanation:**

The optimal choice is choosing each element as a subarray. The output is `(-10) + 3 + (-1) + (-2) = -10`.

**Constraints:**

*   `1 <= nums.length <= 2000`
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   `1 <= k <= floor(nums.length / m)`
*   `1 <= m <= 3`

## Solution

```java
public class Solution {
    public int maxSum(int[] nums, int k, int m) {
        int n = nums.length;
        // Calculate prefix sums
        int[] prefixSum = new int[n + 1];
        for (int i = 0; i < n; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }
        // using elements from nums[0...i-1]
        int[][] dp = new int[n + 1][k + 1];
        // Initialize dp array
        for (int j = 1; j <= k; j++) {
            for (int i = 0; i <= n; i++) {
                dp[i][j] = Integer.MIN_VALUE / 2;
            }
        }
        // Fill dp array
        for (int j = 1; j <= k; j++) {
            int[] maxPrev = new int[n + 1];
            for (int i = 0; i < n + 1; i++) {
                maxPrev[i] =
                        i == 0
                                ? dp[0][j - 1] - prefixSum[0]
                                : Math.max(maxPrev[i - 1], dp[i][j - 1] - prefixSum[i]);
            }
            for (int i = m; i <= n; i++) {
                // Option 1: Don't include the current element in any new subarray
                dp[i][j] = dp[i - 1][j];
                // Option 2: Form a new subarray ending at position i
                // Find the best starting position for the subarray
                dp[i][j] = Math.max(dp[i][j], prefixSum[i] + maxPrev[i - m]);
            }
        }
        return dp[n][k];
    }
}
```