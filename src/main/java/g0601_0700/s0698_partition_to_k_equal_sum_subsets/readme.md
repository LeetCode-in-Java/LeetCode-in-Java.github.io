[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 698\. Partition to K Equal Sum Subsets

Medium

Given an integer array `nums` and an integer `k`, return `true` if it is possible to divide this array into `k` non-empty subsets whose sums are all equal.

**Example 1:**

**Input:** nums = [4,3,2,3,5,2,1], k = 4

**Output:** true

**Explanation:** It is possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums. 

**Example 2:**

**Input:** nums = [1,2,3,4], k = 3

**Output:** false 

**Constraints:**

*   `1 <= k <= nums.length <= 16`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>
*   The frequency of each element is in the range `[1, 4]`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return false;
        }
        int n = nums.length;
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (sum % k != 0) {
            return false;
        }
        // sum of each subset = sum / k
        sum /= k;
        int[] dp = new int[1 << n];
        Arrays.fill(dp, -1);
        dp[0] = 0;
        for (int i = 0; i < (1 << n); i++) {
            if (dp[i] == -1) {
                continue;
            }
            int rem = sum - (dp[i] % sum);
            for (int j = 0; j < n; j++) {
                // bitmask
                int tmp = i | (1 << j);
                // skip if the bit is already taken
                if (tmp != i) {
                    // num too big for current subset
                    if (nums[j] > rem) {
                        break;
                    }
                    // cumulative sum
                    dp[tmp] = dp[i] + nums[j];
                }
            }
        }
        // true if total sum of all nums is the same
        return dp[(1 << n) - 1] == k * sum;
    }
}
```