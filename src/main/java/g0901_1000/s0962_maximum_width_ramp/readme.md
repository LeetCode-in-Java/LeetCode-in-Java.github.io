[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 962\. Maximum Width Ramp

Medium

A **ramp** in an integer array `nums` is a pair `(i, j)` for which `i < j` and `nums[i] <= nums[j]`. The **width** of such a ramp is `j - i`.

Given an integer array `nums`, return _the maximum width of a **ramp** in_ `nums`. If there is no **ramp** in `nums`, return `0`.

**Example 1:**

**Input:** nums = [6,0,8,2,1,5]

**Output:** 4

**Explanation:** The maximum width ramp is achieved at (i, j) = (1, 5): nums[1] = 0 and nums[5] = 5.

**Example 2:**

**Input:** nums = [9,8,1,0,1,9,4,0,4,1]

**Output:** 7

**Explanation:** The maximum width ramp is achieved at (i, j) = (2, 9): nums[2] = 1 and nums[9] = 1.

**Constraints:**

*   <code>2 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>0 <= nums[i] <= 5 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int maxWidthRamp(int[] nums) {
        int m = nums.length;
        int[] dp = new int[m];
        int minInd = 0;
        int ramp = 0;
        for (int i = 0; i < m; i++) {
            int prInd = minInd;
            while (prInd > 0 && nums[i] >= nums[dp[prInd]]) {
                prInd = dp[prInd];
            }
            dp[i] = prInd;
            if (nums[i] >= nums[prInd]) {
                ramp = Math.max(ramp, i - prInd);
            }
            minInd = nums[i] < nums[minInd] ? i : minInd;
        }
        return ramp;
    }
}
```