[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 974\. Subarray Sums Divisible by K

Medium

Given an integer array `nums` and an integer `k`, return _the number of non-empty **subarrays** that have a sum divisible by_ `k`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [4,5,0,-2,-3,1], k = 5

**Output:** 7

**Explanation:**

There are 7 subarrays with a sum divisible by k = 5:

[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]

**Example 2:**

**Input:** nums = [5], k = 9

**Output:** 0

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   <code>2 <= k <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        int[] map = new int[k];
        int ans = 0;
        int sum = 0;
        map[0] = 1;
        for (int num : nums) {
            sum += num;
            int temp = sum % k;
            if (temp < 0) {
                temp += k;
            }
            ans += map[temp]++;
        }
        return ans;
    }
}
```