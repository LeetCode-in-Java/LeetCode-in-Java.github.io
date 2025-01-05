[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3392\. Count Subarrays of Length Three With a Condition

Easy

Given an integer array `nums`, return the number of subarrays of length 3 such that the sum of the first and third numbers equals _exactly_ half of the second number.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,1,4,1]

**Output:** 1

**Explanation:**

Only the subarray `[1,4,1]` contains exactly 3 elements where the sum of the first and third numbers equals half the middle number.

**Example 2:**

**Input:** nums = [1,1,1]

**Output:** 0

**Explanation:**

`[1,1,1]` is the only subarray of length 3. However, its first and third numbers do not add to half the middle number.

**Constraints:**

*   `3 <= nums.length <= 100`
*   `-100 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int countSubarrays(int[] nums) {
        int window = 3;
        int cnt = 0;
        for (int i = 0; i <= nums.length - window; i++) {
            float first = nums[i];
            float second = nums[i + 1];
            float third = nums[i + 2];
            if (second / 2 == first + third) {
                cnt++;
            }
        }
        return cnt;
    }
}
```