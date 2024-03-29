[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2962\. Count Subarrays Where Max Element Appears at Least K Times

Medium

You are given an integer array `nums` and a **positive** integer `k`.

Return _the number of subarrays where the **maximum** element of_ `nums` _appears **at least**_ `k` _times in that subarray._

A **subarray** is a contiguous sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,3,2,3,3], k = 2

**Output:** 6

**Explanation:** The subarrays that contain the element 3 at least 2 times are: [1,3,2,3], [1,3,2,3,3], [3,2,3], [3,2,3,3], [2,3,3] and [3,3].

**Example 2:**

**Input:** nums = [1,4,2,1], k = 3

**Output:** 0

**Explanation:** No subarray contains the element 4 at least 3 times.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public long countSubarrays(int[] n, int k) {
        int[] st = new int[n.length + 1];
        int si = 0;
        int m = 0;
        for (int i = 0; i < n.length; i++) {
            if (m < n[i]) {
                m = n[i];
                si = 0;
            }
            if (m == n[i]) {
                st[si++] = i;
            }
        }
        if (si < k) {
            return 0;
        }
        long r = 0;
        st[si] = n.length;
        for (int i = k; i <= si; i++) {
            r += (long) (st[i - k] + 1) * (st[i] - st[i - 1]);
        }
        return r;
    }
}
```