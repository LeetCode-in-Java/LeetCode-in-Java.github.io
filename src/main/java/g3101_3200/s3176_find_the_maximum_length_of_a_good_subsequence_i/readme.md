[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3176\. Find the Maximum Length of a Good Subsequence I

Medium

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

*   `1 <= nums.length <= 500`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `0 <= k <= min(nums.length, 25)`

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;

public class Solution {
    public int maximumLength(int[] nums, int k) {
        int n = nums.length;
        int count = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] != nums[i + 1]) {
                count++;
            }
        }
        if (count <= k) {
            return n;
        }
        int[] max = new int[k + 1];
        Arrays.fill(max, 1);
        int[] vis = new int[n];
        Arrays.fill(vis, -1);
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            if (!map.containsKey(nums[i])) {
                map.put(nums[i], i + 1);
            } else {
                vis[i] = map.get(nums[i]) - 1;
                map.put(nums[i], i + 1);
            }
        }
        int[][] dp = new int[n][k + 1];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= k; j++) {
                dp[i][j] = 1;
            }
        }
        for (int i = 1; i < n; i++) {
            for (int j = k - 1; j >= 0; j--) {
                dp[i][j + 1] = Math.max(dp[i][j + 1], 1 + max[j]);
                max[j + 1] = Math.max(max[j + 1], dp[i][j + 1]);
            }
            if (vis[i] != -1) {
                int a = vis[i];
                for (int j = 0; j <= k; j++) {
                    dp[i][j] = Math.max(dp[i][j], 1 + dp[a][j]);
                    max[j] = Math.max(dp[i][j], max[j]);
                }
            }
        }
        int ans = 1;
        for (int i = 0; i <= k; i++) {
            ans = Math.max(ans, max[i]);
        }
        return ans;
    }
}
```