[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2680\. Maximum OR

Medium

You are given a **0-indexed** integer array `nums` of length `n` and an integer `k`. In an operation, you can choose an element and multiply it by `2`.

Return _the maximum possible value of_ `nums[0] | nums[1] | ... | nums[n - 1]` _that can be obtained after applying the operation on nums at most_ `k` _times_.

Note that `a | b` denotes the **bitwise or** between two integers `a` and `b`.

**Example 1:**

**Input:** nums = [12,9], k = 1

**Output:** 30

**Explanation:** If we apply the operation to index 1, our new array nums will be equal to [12,18]. Thus, we return the bitwise or of 12 and 18, which is 30.

**Example 2:**

**Input:** nums = [8,1,2], k = 2

**Output:** 35

**Explanation:** If we apply the operation twice on index 0, we yield a new array of [32,1,2]. Thus, we return 32\|1\|2 = 35.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= 15`

## Solution

```java
public class Solution {
    public long maximumOr(int[] nums, int k) {
        int[] suffix = new int[nums.length];
        for (int i = nums.length - 2; i >= 0; i--) {
            suffix[i] = suffix[i + 1] | nums[i + 1];
        }
        long prefix = 0L;
        long max = 0L;
        for (int i = 0; i <= nums.length - 1; i++) {
            long num = nums[i];
            max = Math.max(max, prefix | (num << k) | suffix[i]);
            prefix |= num;
        }
        return max;
    }
}
```