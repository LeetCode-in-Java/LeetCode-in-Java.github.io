[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3350\. Adjacent Increasing Subarrays Detection II

Medium

Given an array `nums` of `n` integers, your task is to find the **maximum** value of `k` for which there exist **two** adjacent subarrays of length `k` each, such that both subarrays are **strictly** **increasing**. Specifically, check if there are **two** subarrays of length `k` starting at indices `a` and `b` (`a < b`), where:

*   Both subarrays `nums[a..a + k - 1]` and `nums[b..b + k - 1]` are **strictly increasing**.
*   The subarrays must be **adjacent**, meaning `b = a + k`.

Return the **maximum** _possible_ value of `k`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [2,5,7,8,9,2,3,4,3,1]

**Output:** 3

**Explanation:**

*   The subarray starting at index 2 is `[7, 8, 9]`, which is strictly increasing.
*   The subarray starting at index 5 is `[2, 3, 4]`, which is also strictly increasing.
*   These two subarrays are adjacent, and 3 is the **maximum** possible value of `k` for which two such adjacent strictly increasing subarrays exist.

**Example 2:**

**Input:** nums = [1,2,3,4,4,4,4,5,6,7]

**Output:** 2

**Explanation:**

*   The subarray starting at index 0 is `[1, 2]`, which is strictly increasing.
*   The subarray starting at index 2 is `[3, 4]`, which is also strictly increasing.
*   These two subarrays are adjacent, and 2 is the **maximum** possible value of `k` for which two such adjacent strictly increasing subarrays exist.

**Constraints:**

*   <code>2 <= nums.length <= 2 * 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.List;

public class Solution {
    public int maxIncreasingSubarrays(List<Integer> nums) {
        int n = nums.size();
        int[] a = new int[n];
        for (int i = 0; i < n; ++i) {
            a[i] = nums.get(i);
        }
        int ans = 1;
        int previousLen = Integer.MAX_VALUE;
        int i = 0;
        while (i < n) {
            int j = i + 1;
            while (j < n && a[j - 1] < a[j]) {
                ++j;
            }
            int len = j - i;
            ans = Math.max(ans, len / 2);
            if (previousLen != Integer.MAX_VALUE) {
                ans = Math.max(ans, Math.min(previousLen, len));
            }
            previousLen = len;
            i = j;
        }
        return ans;
    }
}
```