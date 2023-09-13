[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 215\. Kth Largest Element in an Array

Medium

Given an integer array `nums` and an integer `k`, return _the_ <code>k<sup>th</sup></code> _largest element in the array_.

Note that it is the <code>k<sup>th</sup></code> largest element in the sorted order, not the <code>k<sup>th</sup></code> distinct element.

**Example 1:**

**Input:** nums = [3,2,1,5,6,4], k = 2

**Output:** 5 

**Example 2:**

**Input:** nums = [3,2,3,1,2,4,5,5,6], k = 4

**Output:** 4 

**Constraints:**

*   <code>1 <= k <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int findKthLargest(int[] nums, int k) {
        int n = nums.length;
        Arrays.sort(nums);
        return nums[n - k];
    }
}
```

**Time Complexity (Big O Time):**

1. **Sorting (Arrays.sort):**
   - The primary operation in this program is sorting the input array `nums` using the built-in sorting algorithm.
   - The time complexity of the sorting operation in Java's `Arrays.sort` is O(n * log(n)), where "n" is the number of elements in the array.
   - Sorting is the most time-consuming step in this algorithm.

2. **Accessing the kth Largest Element:**
   - After sorting, accessing the kth largest element by index (nums[n - k]) takes constant time, O(1).

Overall, the dominant factor in terms of time complexity is the sorting step, which is O(n * log(n)).

**Space Complexity (Big O Space):**

1. **Sorting Space:**
   - The space complexity for the sorting algorithm used by `Arrays.sort` is typically O(log(n)) for the stack space required for recursion or iteration.

2. **Other Variables:**
   - The `int n` variable, which stores the length of the input array, takes constant space (O(1)).
   - The sorted array `nums` is modified in place, so it does not contribute to additional space complexity.

In summary, the space complexity is mainly determined by the space required for the sorting algorithm, which is typically O(log(n)). The time complexity is dominated by the sorting operation, which is O(n * log(n)).
