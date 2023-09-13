[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 33\. Search in Rotated Sorted Array

Medium

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0

**Output:** 4 

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3

**Output:** -1 

**Example 3:**

**Input:** nums = [1], target = 0

**Output:** -1 

**Constraints:**

*   `1 <= nums.length <= 5000`
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   All values of `nums` are **unique**.
*   `nums` is an ascending array that is possibly rotated.
*   <code>-10<sup>4</sup> <= target <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int search(int[] nums, int target) {
        int mid;
        int lo = 0;
        int hi = nums.length - 1;
        while (lo <= hi) {
            mid = ((hi - lo) >> 1) + lo;
            if (target == nums[mid]) {
                return mid;
            }
            // if this is true, then the possible rotation can only be in the second half
            if (nums[lo] <= nums[mid]) {
                // the target is in the first half only if it's
                if (nums[lo] <= target && target <= nums[mid]) {
                    // included
                    hi = mid - 1;
                } else {
                    // between nums[lo] and nums[mid]
                    lo = mid + 1;
                }
                // otherwise, the possible rotation can only be in the first half
            } else if (nums[mid] <= target && target <= nums[hi]) {
                // the target is in the second half only if it's included
                lo = mid + 1;
            } else {
                // between nums[hi] and nums[mid]
                hi = mid - 1;
            }
        }
        return -1;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(log n), where n is the number of elements in the input `nums` array. Here's why:

1. The program performs a binary search to find the target element. In each step of the binary search, it reduces the search range by half.

2. The while loop runs until the `lo` pointer is less than or equal to the `hi` pointer, and in each iteration, it reduces the search range by half.

3. Therefore, the number of iterations in the binary search is proportional to log2(n), where n is the number of elements in the input array. This gives us the time complexity of O(log n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(1), which means it uses a constant amount of additional space regardless of the size of the input array `nums`. The program only uses a few integer variables (`mid`, `lo`, `hi`) and does not use any additional data structures or memory that scales with the input size.

In summary, the time complexity of the provided program is O(log n), and the space complexity is O(1), where n is the number of elements in the input array `nums`.
