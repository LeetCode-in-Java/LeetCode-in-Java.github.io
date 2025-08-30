[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3660\. Jump Game IX

Medium

You are given an integer array `nums`.

From any index `i`, you can jump to another index `j` under the following rules:

*   Jump to index `j` where `j > i` is allowed only if `nums[j] < nums[i]`.
*   Jump to index `j` where `j < i` is allowed only if `nums[j] > nums[i]`.

For each index `i`, find the **maximum** **value** in `nums` that can be reached by following **any** sequence of valid jumps starting at `i`.

Return an array `ans` where `ans[i]` is the **maximum** **value** reachable starting from index `i`.

**Example 1:**

**Input:** nums = [2,1,3]

**Output:** [2,2,3]

**Explanation:**

*   For `i = 0`: No jump increases the value.
*   For `i = 1`: Jump to `j = 0` as `nums[j] = 2` is greater than `nums[i]`.
*   For `i = 2`: Since `nums[2] = 3` is the maximum value in `nums`, no jump increases the value.

Thus, `ans = [2, 2, 3]`.

**Example 2:**

**Input:** nums = [2,3,1]

**Output:** [3,3,3]

**Explanation:**

*   For `i = 0`: Jump forward to `j = 2` as `nums[j] = 1` is less than `nums[i] = 2`, then from `i = 2` jump to `j = 1` as `nums[j] = 3` is greater than `nums[2]`.
*   For `i = 1`: Since `nums[1] = 3` is the maximum value in `nums`, no jump increases the value.
*   For `i = 2`: Jump to `j = 1` as `nums[j] = 3` is greater than `nums[2] = 1`.

Thus, `ans = [3, 3, 3]`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int[] maxValue(int[] nums) {
        int[] f = new int[nums.length];
        int cur = 0;
        for (int i = 0; i < nums.length; i++) {
            cur = Math.max(cur, nums[i]);
            f[i] = cur;
        }
        int min = nums[nums.length - 1];
        for (int i = nums.length - 2; i >= 0; i--) {
            if (f[i] > min) {
                f[i] = Math.max(f[i], f[i + 1]);
            }
            min = Math.min(min, nums[i]);
        }
        return f;
    }
}
```