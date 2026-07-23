[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3840\. House Robber V

Medium

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed and is protected by a security system with a color code.

You are given two integer arrays `nums` and `colors`, both of length `n`, where `nums[i]` is the amount of money in the <code>i<sup>th</sup></code> house and `colors[i]` is the color code of that house.

You **cannot rob two adjacent** houses if they share the **same color** code.

Return the **maximum** amount of money you can rob.

**Example 1:**

**Input:** nums = [1,4,3,5], colors = [1,1,2,2]

**Output:** 9

**Explanation:**

*   Choose houses `i = 1` with `nums[1] = 4` and `i = 3` with `nums[3] = 5` because they are non-adjacent.
*   Thus, the total amount robbed is `4 + 5 = 9`.

**Example 2:**

**Input:** nums = [3,1,2,4], colors = [2,3,2,2]

**Output:** 8

**Explanation:**

*   Choose houses `i = 0` with `nums[0] = 3`, `i = 1` with `nums[1] = 1`, and `i = 3` with `nums[3] = 4`.
*   This selection is valid because houses `i = 0` and `i = 1` have different colors, and house `i = 3` is non-adjacent to `i = 1`.
*   Thus, the total amount robbed is `3 + 1 + 4 = 8`.

**Example 3:**

**Input:** nums = [10,1,3,9], colors = [1,1,1,2]

**Output:** 22

**Explanation:**

*   Choose houses `i = 0` with `nums[0] = 10`, `i = 2` with `nums[2] = 3`, and `i = 3` with `nums[3] = 9`.
*   This selection is valid because houses `i = 0` and `i = 2` are non-adjacent, and houses `i = 2` and `i = 3` have different colors.
*   Thus, the total amount robbed is `10 + 3 + 9 = 22`.

**Constraints:**

*   <code>1 <= n == nums.length == colors.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], colors[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public long rob(int[] nums, int[] colors) {
        int n = nums.length;
        long dp0 = 0;
        long dp1 = nums[0];
        for (int i = 1; i < n; i++) {
            long new0;
            long new1;
            if (colors[i] == colors[i - 1]) {
                new1 = dp0 + nums[i];
                new0 = Math.max(dp0, dp1);
            } else {
                new0 = Math.max(dp1, dp0);
                new1 = Math.max(dp0, dp1) + nums[i];
            }
            dp0 = new0;
            dp1 = new1;
        }
        return Math.max(dp0, dp1);
    }
}
```