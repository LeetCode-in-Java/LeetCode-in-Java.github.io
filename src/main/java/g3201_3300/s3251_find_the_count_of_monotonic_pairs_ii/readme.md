[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3251\. Find the Count of Monotonic Pairs II

Hard

You are given an array of **positive** integers `nums` of length `n`.

We call a pair of **non-negative** integer arrays `(arr1, arr2)` **monotonic** if:

*   The lengths of both arrays are `n`.
*   `arr1` is monotonically **non-decreasing**, in other words, `arr1[0] <= arr1[1] <= ... <= arr1[n - 1]`.
*   `arr2` is monotonically **non-increasing**, in other words, `arr2[0] >= arr2[1] >= ... >= arr2[n - 1]`.
*   `arr1[i] + arr2[i] == nums[i]` for all `0 <= i <= n - 1`.

Return the count of **monotonic** pairs.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [2,3,2]

**Output:** 4

**Explanation:**

The good pairs are:

1.  `([0, 1, 1], [2, 2, 1])`
2.  `([0, 1, 2], [2, 2, 0])`
3.  `([0, 2, 2], [2, 1, 0])`
4.  `([1, 2, 2], [1, 1, 0])`

**Example 2:**

**Input:** nums = [5,5,5,5]

**Output:** 126

**Constraints:**

*   `1 <= n == nums.length <= 2000`
*   `1 <= nums[i] <= 1000`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int MOD = 1000000007;

    public int countOfPairs(int[] nums) {
        int prefixZeros = 0;
        int n = nums.length;
        // Calculate prefix zeros
        for (int i = 1; i < n; i++) {
            prefixZeros += Math.max(nums[i] - nums[i - 1], 0);
        }
        int row = n + 1;
        int col = nums[n - 1] + 1 - prefixZeros;
        if (col <= 0) {
            return 0;
        }
        // Initialize dp array
        int[] dp = new int[col];
        Arrays.fill(dp, 1);
        // Fill dp array
        for (int r = 1; r < row; r++) {
            for (int c = 1; c < col; c++) {
                dp[c] = (dp[c] + dp[c - 1]) % MOD;
            }
        }
        return dp[col - 1];
    }
}
```