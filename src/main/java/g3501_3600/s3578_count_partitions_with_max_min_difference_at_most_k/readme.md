[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3578\. Count Partitions With Max-Min Difference at Most K

Medium

You are given an integer array `nums` and an integer `k`. Your task is to partition `nums` into one or more **non-empty** contiguous segments such that in each segment, the difference between its **maximum** and **minimum** elements is **at most** `k`.

Return the total number of ways to partition `nums` under this condition.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [9,4,1,3,7], k = 4

**Output:** 6

**Explanation:**

There are 6 valid partitions where the difference between the maximum and minimum elements in each segment is at most `k = 4`:

*   `[[9], [4], [1], [3], [7]]`
*   `[[9], [4], [1], [3, 7]]`
*   `[[9], [4], [1, 3], [7]]`
*   `[[9], [4, 1], [3], [7]]`
*   `[[9], [4, 1], [3, 7]]`
*   `[[9], [4, 1, 3], [7]]`

**Example 2:**

**Input:** nums = [3,3,4], k = 0

**Output:** 2

**Explanation:**

There are 2 valid partitions that satisfy the given conditions:

*   `[[3], [3], [4]]`
*   `[[3, 3], [4]]`

**Constraints:**

*   <code>2 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    private static final int MOD = 1_000_000_007;

    public int countPartitions(int[] nums, int k) {
        int n = nums.length;
        int[] dp = new int[n + 1];
        dp[0] = 1;
        int[] prefix = new int[n + 1];
        prefix[0] = 1;
        int[] maxDeque = new int[n];
        int maxFront = 0;
        int maxBack = 0;
        int[] minDeque = new int[n];
        int minFront = 0;
        int minBack = 0;
        int start = 0;
        for (int end = 0; end < n; end++) {
            while (maxBack > maxFront && nums[maxDeque[maxBack - 1]] <= nums[end]) {
                maxBack--;
            }
            maxDeque[maxBack++] = end;
            while (minBack > minFront && nums[minDeque[minBack - 1]] >= nums[end]) {
                minBack--;
            }
            minDeque[minBack++] = end;
            while (nums[maxDeque[maxFront]] - nums[minDeque[minFront]] > k) {
                if (maxDeque[maxFront] == start) {
                    maxFront++;
                }
                if (minDeque[minFront] == start) {
                    minFront++;
                }
                start++;
            }
            int sum = prefix[end] - (start > 0 ? prefix[start - 1] : 0);
            if (sum < 0) {
                sum += MOD;
            }
            dp[end + 1] = sum % MOD;
            prefix[end + 1] = (prefix[end] + dp[end + 1]) % MOD;
        }
        return dp[n];
    }
}
```