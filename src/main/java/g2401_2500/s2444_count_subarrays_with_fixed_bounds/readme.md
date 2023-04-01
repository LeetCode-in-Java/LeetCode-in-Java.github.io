[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2444\. Count Subarrays With Fixed Bounds

Hard

You are given an integer array `nums` and two integers `minK` and `maxK`.

A **fixed-bound subarray** of `nums` is a subarray that satisfies the following conditions:

*   The **minimum** value in the subarray is equal to `minK`.
*   The **maximum** value in the subarray is equal to `maxK`.

Return _the **number** of fixed-bound subarrays_.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [1,3,5,2,7,5], minK = 1, maxK = 5

**Output:** 2

**Explanation:** The fixed-bound subarrays are [1,3,5] and [1,3,5,2].

**Example 2:**

**Input:** nums = [1,1,1,1], minK = 1, maxK = 1

**Output:** 10

**Explanation:** Every subarray of nums is a fixed-bound subarray. There are 10 possible subarrays.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], minK, maxK <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public long countSubarrays(int[] nums, int minK, int maxK) {
        long ans = 0;
        int i = 0;
        while (i < nums.length) {
            if (nums[i] >= minK && nums[i] <= maxK) {
                int a = i;
                int b = i;
                int mini = 0;
                int maxi = 0;
                while (i != nums.length && (nums[i] >= minK && nums[i] <= maxK)) {
                    i++;
                }
                while (true) {
                    for (; b != i && (mini == 0 || maxi == 0); b++) {
                        if (nums[b] == minK) {
                            mini++;
                        }
                        if (nums[b] == maxK) {
                            maxi++;
                        }
                    }
                    if (mini == 0 || maxi == 0) {
                        break;
                    }
                    for (; mini != 0 && maxi != 0; ans += 1 + (i - b), a++) {
                        if (nums[a] == minK) {
                            mini--;
                        }
                        if (nums[a] == maxK) {
                            maxi--;
                        }
                    }
                }
            }
            i++;
        }
        return ans;
    }
}
```