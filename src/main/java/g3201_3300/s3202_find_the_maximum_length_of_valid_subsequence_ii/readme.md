[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3202\. Find the Maximum Length of Valid Subsequence II

Medium

You are given an integer array `nums` and a **positive** integer `k`.

A subsequence `sub` of `nums` with length `x` is called **valid** if it satisfies:

*   `(sub[0] + sub[1]) % k == (sub[1] + sub[2]) % k == ... == (sub[x - 2] + sub[x - 1]) % k.`

Return the length of the **longest** **valid** subsequence of `nums`.

**Example 1:**

**Input:** nums = [1,2,3,4,5], k = 2

**Output:** 5

**Explanation:**

The longest valid subsequence is `[1, 2, 3, 4, 5]`.

**Example 2:**

**Input:** nums = [1,4,2,3,1,4], k = 3

**Output:** 4

**Explanation:**

The longest valid subsequence is `[1, 4, 1, 4]`.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>3</sup></code>
*   <code>1 <= nums[i] <= 10<sup>7</sup></code>
*   <code>1 <= k <= 10<sup>3</sup></code>

## Solution

```java
public class Solution {
    public int maximumLength(int[] nums, int k) {
        // dp array to store the index against each possible modulo
        int[][] dp = new int[nums.length + 1][k + 1];
        int longest = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                // Checking the modulo with each previous number
                int val = (nums[i] + nums[j]) % k;
                // storing the number of pairs that have the same modulo.
                // it would be one more than the number of pairs with the same modulo at the last
                // index
                dp[i][val] = dp[j][val] + 1;
                // Calculating the max seen till now
                longest = Math.max(longest, dp[i][val]);
            }
        }
        // total number of elements in the subsequence would be 1 more than the number of pairs
        return longest + 1;
    }
}
```