[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3854\. Minimum Operations to Make Array Parity Alternating

Medium

You are given an integer array `nums`.

An array is called **parity alternating** if for every index `i` where `0 <= i < n - 1`, `nums[i]` and `nums[i + 1]` have different parity (one is even and the other is odd).

In one operation, you may choose any index `i` and either increase `nums[i]` by 1 or decrease `nums[i]` by 1.

Return an integer array `answer` of length 2 where:

*   `answer[0]` is the **minimum** number of operations required to make the array parity alternating.
*   `answer[1]` is the **minimum** possible value of `max(nums) - min(nums)` taken over all arrays that are parity alternating and can be obtained by performing **exactly** `answer[0]` operations.

An array of length 1 is considered parity alternating.

**Example 1:**

**Input:** nums = [-2,-3,1,4]

**Output:** [2,6]

**Explanation:**

Applying the following operations:

*   Increase `nums[2]` by 1, resulting in `nums = [-2, -3, 2, 4]`.
*   Decrease `nums[3]` by 1, resulting in `nums = [-2, -3, 2, 3]`.

The resulting array is parity alternating, and the value of `max(nums) - min(nums) = 3 - (-3) = 6` is the minimum possible among all parity alternating arrays obtainable using exactly 2 operations.

**Example 2:**

**Input:** nums = [0,2,-2]

**Output:** [1,3]

**Explanation:**

Applying the following operation:

*   Decrease `nums[1]` by 1, resulting in `nums = [0, 1, -2]`.

The resulting array is parity alternating, and the value of `max(nums) - min(nums) = 1 - (-2) = 3` is the minimum possible among all parity alternating arrays obtainable using exactly 1 operation.

**Example 3:**

**Input:** nums = [7]

**Output:** [0,0]

**Explanation:**

No operations are required. The array is already parity alternating, and the value of `max(nums) - min(nums) = 7 - 7 = 0`, which is the minimum possible.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int[] makeParityAlternating(int[] nums) {
        if (nums.length == 1) {
            return new int[] {0, 0};
        }
        int zero = 0;
        int one = 0;
        for (int i = 0; i < nums.length; i++) {
            if ((nums[i] & 1) != (i & 1)) {
                zero++;
            }
            if ((nums[i] & 1) != ((i + 1) & 1)) {
                one++;
            }
        }
        int[] ans = new int[2];
        ans[0] = Math.min(one, zero);
        if (one == zero) {
            ans[1] = Math.min(fun(nums, 0), fun(nums, 1));
        } else if (one > zero) {
            ans[1] = fun(nums, 0);
        } else {
            ans[1] = fun(nums, 1);
        }
        return ans;
    }

    private int fun(int[] nums, int parity) {
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            if ((nums[i] & 1) == ((i + parity) & 1)) {
                max = Math.max(max, nums[i]);
                min = Math.min(min, nums[i]);
            } else {
                max = Math.max(max, nums[i] - 1);
                min = Math.min(min, nums[i] + 1);
            }
        }
        return Math.max(max - min, 1);
    }
}
```