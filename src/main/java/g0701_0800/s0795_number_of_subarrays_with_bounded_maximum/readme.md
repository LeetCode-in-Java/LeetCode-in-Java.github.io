[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 795\. Number of Subarrays with Bounded Maximum

Medium

Given an integer array `nums` and two integers `left` and `right`, return _the number of contiguous non-empty **subarrays** such that the value of the maximum array element in that subarray is in the range_ `[left, right]`.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**

**Input:** nums = [2,1,4,3], left = 2, right = 3

**Output:** 3

**Explanation:** There are three subarrays that meet the requirements: [2], [2, 1], [3]. 

**Example 2:**

**Input:** nums = [2,9,2,5,6], left = 2, right = 8

**Output:** 7 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= left <= right <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int numSubarrayBoundedMax(int[] nums, int left, int right) {
        int i = 0;
        int j = 0;
        int count = 0;
        int tempSum = 0;
        while (j < nums.length) {
            if (nums[j] > right) {
                tempSum = 0;
                i = ++j;
            } else if (nums[j] < left) {
                count += tempSum;
                j++;
            } else {
                tempSum = j - i + 1;
                count += tempSum;
                j++;
            }
        }
        return count;
    }
}
```