[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3819\. Rotate Non Negative Elements

Medium

You are given an integer array `nums` and an integer `k`.

Rotate only the **non-negative** elements of the array to the **left** by `k` positions, in a cyclic manner.

All **negative** elements must stay in their original positions and must not move.

After rotation, place the **non-negative** elements back into the array in the new order, filling only the positions that originally contained **non-negative** values and **skipping all negative** positions.

Return the resulting array.

**Example 1:**

**Input:** nums = [1,-2,3,-4], k = 3

**Output:** [3,-2,1,-4]

**Explanation:**

*   The non-negative elements, in order, are `[1, 3]`.
*   Left rotation with `k = 3` results in:
    *   `[1, 3] -> [3, 1] -> [1, 3] -> [3, 1]`
*   Placing them back into the non-negative indices results in `[3, -2, 1, -4]`.

**Example 2:**

**Input:** nums = [-3,-2,7], k = 1

**Output:** [-3,-2,7]

**Explanation:**

*   The non-negative elements, in order, are `[7]`.
*   Left rotation with `k = 1` results in `[7]`.
*   Placing them back into the non-negative indices results in `[-3, -2, 7]`.

**Example 3:**

**Input:** nums = [5,4,-9,6], k = 2

**Output:** [6,5,-9,4]

**Explanation:**

*   The non-negative elements, in order, are `[5, 4, 6]`.
*   Left rotation with `k = 2` results in `[6, 5, 4]`.
*   Placing them back into the non-negative indices results in `[6, 5, -9, 4]`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   <code>0 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int[] rotateElements(int[] nums, int k) {
        int n = nums.length;
        int m = 0;
        int[] a = new int[n];
        for (int x : nums) {
            if (x >= 0) {
                a[m++] = x;
            }
        }
        if (m == 0) {
            return nums;
        }
        k %= m;
        int j = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] >= 0) {
                int index = (j + k) % m;
                nums[i] = a[index];
                j++;
            }
        }
        return nums;
    }
}
```