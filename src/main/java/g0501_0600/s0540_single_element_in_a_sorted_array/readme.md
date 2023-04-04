[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 540\. Single Element in a Sorted Array

Medium

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return _the single element that appears only once_.

Your solution must run in `O(log n)` time and `O(1)` space.

**Example 1:**

**Input:** nums = [1,1,2,3,3,4,4,8,8]

**Output:** 2

**Example 2:**

**Input:** nums = [3,3,7,7,10,11,11]

**Output:** 10

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int singleNonDuplicate(int[] nums) {
        int start = 0;
        int end = nums.length - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (mid + 1 < nums.length
                    && nums[mid] != nums[mid + 1]
                    && mid - 1 >= 0
                    && nums[mid] != nums[mid - 1]) {
                return nums[mid];
            } else if (mid + 1 < nums.length && nums[mid] == nums[mid + 1] && mid % 2 == 0) {
                start = mid + 1;
            } else if (mid - 1 >= 0 && nums[mid] == nums[mid - 1] && mid % 2 == 1) {
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
        return nums[start];
    }
}
```