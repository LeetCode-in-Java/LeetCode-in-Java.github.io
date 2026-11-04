[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3976\. Maximum Subarray Sum After Multiplier

Medium

You are given an integer array `nums` and a positive integer `k`.

You must choose **exactly** one non-empty subarrays of `nums` and perform **exactly** one of the following operations:

1.  Multiply each number in the chosen subarray by `k`.
2.  Divide each number in the chosen subarray by `k`.
    *   When dividing a positive number by `k`, use the **floor** value of the division result.
    *   When dividing a negative number by `k`, use the **ceiling** value of the division result.

Return the **maximum** possible sum of a **non-empty** subarray in the resulting array.

Note that the subarray chosen for the operation and the subarray chosen for the sum may be **different**.

**Example 1:**

**Input:** nums = [1,-2,3,4,-5], k = 2

**Output:** 14

**Explanation:**

*   Multiply each number in the subarray `[3, 4]` by 2.
*   This results in `nums = [1, -2, 6, 8, -5]`.
*   The subarray with the largest sum is `[6, 8]`, so the output is `6 + 8 = 14`.

**Example 2:**

**Input:** nums = [-5,-4,-3], k = 2

**Output:** \-1

**Explanation:**

*   Divide each number in the subarray `[-3]` by 2.
*   This results in `nums = [-5, -4, -1]`.
*   The subarray with the largest sum is `[-1]`, so the output is -1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public long maxSubarraySum(int[] nums, int k) {
        long max = Long.MIN_VALUE;
        long used = 0;
        long div = 0;
        long sum = 0;
        for (int x : nums) {
            used = Math.max(used, div) + x;
            div = Math.max(sum, div) + x / k;
            sum += x;
            max = Math.max(Math.max(max, sum * k), Math.max(used, div));
            sum = Math.max(sum, 0);
        }
        return max;
    }
}
```