[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3177\. Find the Maximum Length of a Good Subsequence II

Hard

You are given an integer array `nums` and a **non-negative** integer `k`. A sequence of integers `seq` is called **good** if there are **at most** `k` indices `i` in the range `[0, seq.length - 2]` such that `seq[i] != seq[i + 1]`.

Return the **maximum** possible length of a **good** subsequence of `nums`.

**Example 1:**

**Input:** nums = [1,2,1,1,3], k = 2

**Output:** 4

**Explanation:**

The maximum length subsequence is <code>[<ins>1</ins>,<ins>2</ins>,<ins>1</ins>,<ins>1</ins>,3]</code>.

**Example 2:**

**Input:** nums = [1,2,3,4,5,1], k = 0

**Output:** 2

**Explanation:**

The maximum length subsequence is <code>[<ins>1</ins>,2,3,4,5,<ins>1</ins>]</code>.

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>3</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `0 <= k <= min(50, nums.length)`

## Solution

```java
import java.util.HashMap;

public class Solution {
    public int maximumLength(int[] nums, int k) {
        HashMap<Integer, Integer> hm = new HashMap<>();
        int n = nums.length;
        int[] pre = new int[n];
        for (int i = 0; i < n; i++) {
            pre[i] = hm.getOrDefault(nums[i], -1);
            hm.put(nums[i], i);
        }
        int[][] dp = new int[k + 1][n];
        for (int i = 0; i < n; i++) {
            dp[0][i] = 1;
            if (pre[i] >= 0) {
                dp[0][i] = dp[0][pre[i]] + 1;
            }
        }
        for (int i = 1; i <= k; i++) {
            int max = 0;
            for (int j = 0; j < n; j++) {
                if (pre[j] >= 0) {
                    dp[i][j] = dp[i][pre[j]] + 1;
                }
                dp[i][j] = Math.max(dp[i][j], max + 1);
                max = Math.max(max, dp[i - 1][j]);
            }
        }
        int max = 0;
        for (int i = 0; i < n; i++) {
            max = Math.max(max, dp[k][i]);
        }
        return max;
    }
}
```