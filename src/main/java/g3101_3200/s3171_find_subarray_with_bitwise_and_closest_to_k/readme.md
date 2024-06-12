[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3171\. Find Subarray With Bitwise AND Closest to K

Hard

You are given an array `nums` and an integer `k`. You need to find a subarray of `nums` such that the **absolute difference** between `k` and the bitwise `AND` of the subarray elements is as **small** as possible. In other words, select a subarray `nums[l..r]` such that `|k - (nums[l] AND nums[l + 1] ... AND nums[r])|` is minimum.

Return the **minimum** possible value of the absolute difference.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,4,5], k = 3

**Output:** 1

**Explanation:**

The subarray `nums[2..3]` has `AND` value 4, which gives the minimum absolute difference `|3 - 4| = 1`.

**Example 2:**

**Input:** nums = [1,2,1,2], k = 2

**Output:** 0

**Explanation:**

The subarray `nums[1..1]` has `AND` value 2, which gives the minimum absolute difference `|2 - 2| = 0`.

**Example 3:**

**Input:** nums = [1], k = 10

**Output:** 9

**Explanation:**

There is a single subarray with `AND` value 1, which gives the minimum absolute difference `|10 - 1| = 9`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int minimumDifference(int[] nums, int k) {
        int res = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            res = Math.min(res, Math.abs(nums[i] - k));
            for (int j = i - 1; j >= 0 && (nums[j] & nums[i]) != nums[j]; j--) {
                nums[j] &= nums[i];
                res = Math.min(res, Math.abs(nums[j] - k));
            }
        }
        return res;
    }
}
```