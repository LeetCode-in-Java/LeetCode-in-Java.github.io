[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1031\. Maximum Sum of Two Non-Overlapping Subarrays

Medium

Given an integer array `nums` and two integers `firstLen` and `secondLen`, return _the maximum sum of elements in two non-overlapping **subarrays** with lengths_ `firstLen` _and_ `secondLen`.

The array with length `firstLen` could occur before or after the array with length `secondLen`, but they have to be non-overlapping.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [0,6,5,2,2,5,1,9,4], firstLen = 1, secondLen = 2

**Output:** 20

**Explanation:** One choice of subarrays is [9] with length 1, and [6,5] with length 2.

**Example 2:**

**Input:** nums = [3,8,1,3,2,1,8,9,0], firstLen = 3, secondLen = 2

**Output:** 29

**Explanation:** One choice of subarrays is [3,8,1] with length 3, and [8,9] with length 2.

**Example 3:**

**Input:** nums = [2,1,5,6,0,9,5,0,3,8], firstLen = 4, secondLen = 3

**Output:** 31

**Explanation:** One choice of subarrays is [5,6,0,9] with length 4, and [3,8] with length 3.

**Constraints:**

*   `1 <= firstLen, secondLen <= 1000`
*   `2 <= firstLen + secondLen <= 1000`
*   `firstLen + secondLen <= nums.length <= 1000`
*   `0 <= nums[i] <= 1000`

## Solution

```java
public class Solution {
    public int maxSumTwoNoOverlap(int[] nums, int f, int s) {
        int sum = 0;
        int n = nums.length;
        int[] pref = new int[n];
        int[] suff = new int[n];
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            if (i < f - 1) {
                continue;
            }
            pref[i] = Math.max(i > 0 ? pref[i - 1] : 0, sum);
            sum -= nums[i + 1 - f];
        }
        sum = 0;
        for (int i = n - 1; i >= 0; i--) {
            sum += nums[i];
            if (i > n - f) {
                continue;
            }
            suff[i] = Math.max(i < n - 1 ? suff[i + 1] : 0, sum);
            sum -= nums[i + f - 1];
        }
        sum = 0;
        for (int i = 0; i < s - 1; i++) {
            sum += nums[i];
        }
        int ans = Integer.MIN_VALUE;
        for (int i = s - 1; i < n; i++) {
            sum += nums[i];
            if (i >= s) {
                ans = Math.max(ans, pref[i - s] + sum);
            }
            if (i < n - 1) {
                ans = Math.max(ans, suff[i + 1] + sum);
            }
            sum -= nums[i + 1 - s];
        }
        return ans;
    }
}
```