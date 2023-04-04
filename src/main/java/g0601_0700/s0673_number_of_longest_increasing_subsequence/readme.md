[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 673\. Number of Longest Increasing Subsequence

Medium

Given an integer array `nums`, return _the number of longest increasing subsequences._

**Notice** that the sequence has to be **strictly** increasing.

**Example 1:**

**Input:** nums = [1,3,5,4,7]

**Output:** 2

**Explanation:** The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].

**Example 2:**

**Input:** nums = [2,2,2,2,2]

**Output:** 5

**Explanation:** The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.

**Constraints:**

*   `1 <= nums.length <= 2000`
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int findNumberOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int[] count = new int[nums.length];
        dp[0] = 1;
        count[0] = 1;
        int result = 0;
        int max = Integer.MIN_VALUE;
        for (int i = 1; i < nums.length; i++) {
            dp[i] = 1;
            count[i] = 1;
            for (int j = i - 1; j >= 0; j--) {
                if (nums[j] < nums[i]) {
                    if (dp[i] < dp[j] + 1) {
                        dp[i] = dp[j] + 1;
                        count[i] = count[j];
                    } else if (dp[i] == dp[j] + 1) {
                        count[i] += count[j];
                    }
                }
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if (max < dp[i]) {
                result = count[i];
                max = dp[i];
            } else if (max == dp[i]) {
                result += count[i];
            }
        }
        return result;
    }
}
```