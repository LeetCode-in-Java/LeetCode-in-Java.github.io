[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3599\. Partition Array to Minimize XOR

Medium

You are given an integer array `nums` and an integer `k`.

Your task is to partition `nums` into `k` non-empty ****non-empty subarrays****. For each subarray, compute the bitwise **XOR** of all its elements.

Return the **minimum** possible value of the **maximum XOR** among these `k` subarrays.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 1

**Explanation:**

The optimal partition is `[1]` and `[2, 3]`.

*   XOR of the first subarray is `1`.
*   XOR of the second subarray is `2 XOR 3 = 1`.

The maximum XOR among the subarrays is 1, which is the minimum possible.

**Example 2:**

**Input:** nums = [2,3,3,2], k = 3

**Output:** 2

**Explanation:**

The optimal partition is `[2]`, `[3, 3]`, and `[2]`.

*   XOR of the first subarray is `2`.
*   XOR of the second subarray is `3 XOR 3 = 0`.
*   XOR of the third subarray is `2`.

The maximum XOR among the subarrays is 2, which is the minimum possible.

**Example 3:**

**Input:** nums = [1,1,2,3,1], k = 2

**Output:** 0

**Explanation:**

The optimal partition is `[1, 1]` and `[2, 3, 1]`.

*   XOR of the first subarray is `1 XOR 1 = 0`.
*   XOR of the second subarray is `2 XOR 3 XOR 1 = 0`.

The maximum XOR among the subarrays is 0, which is the minimum possible.

**Constraints:**

*   `1 <= nums.length <= 250`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= n`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minXor(int[] nums, int k) {
        int n = nums.length;
        // Step 1: Prefix XOR array
        int[] pfix = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            pfix[i] = pfix[i - 1] ^ nums[i - 1];
        }
        // Step 2: DP table
        int[][] dp = new int[n + 1][k + 1];
        for (int[] row : dp) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        for (int i = 0; i <= n; i++) {
            // Base case: 1 partition
            dp[i][1] = pfix[i];
        }
        // Step 3: Fill DP for partitions 2 to k
        for (int parts = 2; parts <= k; parts++) {
            for (int end = parts; end <= n; end++) {
                for (int split = parts - 1; split < end; split++) {
                    int segmentXOR = pfix[end] ^ pfix[split];
                    int maxXOR = Math.max(dp[split][parts - 1], segmentXOR);
                    dp[end][parts] = Math.min(dp[end][parts], maxXOR);
                }
            }
        }
        return dp[n][k];
    }
}
```