[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 35\. Search Insert Position

Easy

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [1,3,5,6], target = 5

**Output:** 2 

**Example 2:**

**Input:** nums = [1,3,5,6], target = 2

**Output:** 1 

**Example 3:**

**Input:** nums = [1,3,5,6], target = 7

**Output:** 4 

**Example 4:**

**Input:** nums = [1,3,5,6], target = 0

**Output:** 0 

**Example 5:**

**Input:** nums = [1], target = 0

**Output:** 0 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   `nums` contains **distinct** values sorted in **ascending** order.
*   <code>-10<sup>4</sup> <= target <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int searchInsert(int[] nums, int target) {
        int lo = 0;
        int hi = nums.length - 1;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (target == nums[mid]) {
                return mid;
            } else if (target < nums[mid]) {
                hi = mid - 1;
            } else if (target > nums[mid]) {
                lo = mid + 1;
            }
        }
        return lo;
    }
}
```

﻿**Time Complexity (Big O Time):**

1. The program performs a binary search on a sorted array of length n. In each step of the binary search, it reduces the search range by half.

2. The while loop runs until the `lo` pointer is less than or equal to the `hi` pointer, and in each iteration, it reduces the search range by half.

3. Therefore, the number of iterations in the binary search is proportional to log2(n), where n is the number of elements in the input array. This gives us the time complexity of O(log n).

**Space Complexity (Big O Space):**

The space complexity of the program is O(1), which means it uses a constant amount of additional space regardless of the size of the input array `nums`. The program only uses a few integer variables (`lo`, `hi`, `mid`) and does not use any additional data structures or memory that scales with the input size.

In summary, the time complexity of the provided program is O(log n), and the space complexity is O(1), where n is the number of elements in the input array `nums`.
