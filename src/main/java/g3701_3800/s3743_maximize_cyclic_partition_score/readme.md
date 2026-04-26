[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3743\. Maximize Cyclic Partition Score

Hard

You are given a **cyclic** array `nums` and an integer `k`.

**Partition** `nums` into **at most** `k` non-empty subarrays. As `nums` is cyclic, these subarrays may wrap around from the end of the array back to the beginning.

The **range** of a subarray is the difference between its **maximum** and **minimum** values. The **score** of a partition is the sum of subarray **ranges**.

Return the **maximum** possible **score** among all cyclic partitions.

**Example 1:**

**Input:** nums = [1,2,3,3], k = 2

**Output:** 3

**Explanation:**

*   Partition `nums` into `[2, 3]` and `[3, 1]` (wrapped around).
*   The range of `[2, 3]` is `max(2, 3) - min(2, 3) = 3 - 2 = 1`.
*   The range of `[3, 1]` is `max(3, 1) - min(3, 1) = 3 - 1 = 2`.
*   The score is `1 + 2 = 3`.

**Example 2:**

**Input:** nums = [1,2,3,3], k = 1

**Output:** 2

**Explanation:**

*   Partition `nums` into `[1, 2, 3, 3]`.
*   The range of `[1, 2, 3, 3]` is `max(1, 2, 3, 3) - min(1, 2, 3, 3) = 3 - 1 = 2`.
*   The score is 2.

**Example 3:**

**Input:** nums = [1,2,3,3], k = 4

**Output:** 3

**Explanation:**

Identical to Example 1, we partition `nums` into `[2, 3]` and `[3, 1]`. Note that `nums` may be partitioned into fewer than `k` subarrays.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= nums.length`

## Solution

```java
public class Solution {
    public long maximumScore(int[] nums, int k) {
        // Find index of minimum element
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < nums[j]) {
                j = i;
            }
        }
        // Build array 'a'
        int n = nums.length;
        int[] a = new int[n];
        for (int i = 0; i < n; i++) {
            a[i] = nums[(j + i) % n];
        }
        // Build array 'b' (rotated and reversed)
        int[] b = new int[n];
        for (int i = 0; i < n; i++) {
            b[i] = nums[(j + 1 + i) % n];
        }
        reverse(b);
        // Compute and return max of f(a, k) and f(b, k)
        return Math.max(f(a, k), f(b, k));
    }

    private long f(int[] a, int k) {
        int n = a.length;
        long[][] dp = new long[k + 1][n + 1];
        long mn = Long.MAX_VALUE;
        long mx = Long.MIN_VALUE;
        // Initialize dp[1][j+1]
        for (int j = 0; j < n; j++) {
            mn = Math.min(mn, a[j]);
            mx = Math.max(mx, a[j]);
            dp[1][j + 1] = mx - mn;
        }
        long res = dp[1][n];
        for (int i = 2; i <= k; i++) {
            long x = Long.MIN_VALUE;
            long y = Long.MIN_VALUE;
            for (int j = i - 1; j < n; j++) {
                x = Math.max(x, dp[i - 1][j] - a[j]);
                y = Math.max(y, dp[i - 1][j] + a[j]);
                dp[i][j + 1] = Math.max(dp[i][j], Math.max(x + a[j], y - a[j]));
            }
            res = Math.max(res, dp[i][n]);
        }
        return res;
    }

    private void reverse(int[] arr) {
        int left = 0;
        int right = arr.length - 1;
        while (left < right) {
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
}
```