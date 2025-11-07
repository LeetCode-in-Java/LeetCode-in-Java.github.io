[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 189\. Rotate Array

Medium

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3

**Output:** [5,6,7,1,2,3,4]

**Explanation:**

    rotate 1 steps to the right: [7,1,2,3,4,5,6]
    rotate 2 steps to the right: [6,7,1,2,3,4,5]
    rotate 3 steps to the right: [5,6,7,1,2,3,4] 

**Example 2:**

**Input:** nums = [-1,-100,3,99], k = 2

**Output:** [3,99,-1,-100]

**Explanation:**

    rotate 1 steps to the right: [99,-1,-100,3]
    rotate 2 steps to the right: [3,99,-1,-100] 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   <code>0 <= k <= 10<sup>5</sup></code>

**Follow up:**

*   Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
*   Could you do it in-place with `O(1)` extra space?

## Solution

```java
public class Solution {
    private void reverse(int[] nums, int l, int r) {
        while (l <= r) {
            int temp = nums[l];
            nums[l] = nums[r];
            nums[r] = temp;
            l++;
            r--;
        }
    }

    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int t = n - (k % n);
        reverse(nums, 0, t - 1);
        reverse(nums, t, n - 1);
        reverse(nums, 0, n - 1);
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where n is the length of the input array `nums`. Here's why:

1. The `reverse` method has a while loop that iterates from `l` to `r`, where `l` and `r` are indices within the array. The number of iterations in this loop is proportional to the size of the subarray being reversed.

2. In the `rotate` method, there are three calls to the `reverse` method:
   - The first `reverse` call reverses the subarray from index 0 to `t - 1`, where `t` is calculated as `n - (k % n)`.
   - The second `reverse` call reverses the subarray from index `t` to `n - 1`.
   - The third `reverse` call reverses the entire array from index 0 to `n - 1`.

All three `reverse` calls have time complexity proportional to the size of the subarray being reversed, and they operate on non-overlapping subarrays. Therefore, the overall time complexity is O(n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(1), indicating that it uses a constant amount of additional memory regardless of the size of the input array. The program performs the rotation in-place, and the space used for variables and computations remains constant.

In summary, the time complexity is O(n), where n is the length of the input array, and the space complexity is O(1), indicating constant space usage.
