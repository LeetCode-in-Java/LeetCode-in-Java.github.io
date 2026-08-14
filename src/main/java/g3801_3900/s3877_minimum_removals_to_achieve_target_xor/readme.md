[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3877\. Minimum Removals to Achieve Target XOR

Medium

You are given an integer array `nums` and an integer `target`.

You may remove **any** number of elements from `nums` (possibly zero).

Return the **minimum** number of removals required so that the **bitwise XOR** of the remaining elements equals `target`. If it is impossible to achieve `target`, return -1.

The bitwise XOR of an empty array is 0.

**Example 1:**

**Input:** nums = [1,2,3], target = 2

**Output:** 1

**Explanation:**

*   Removing `nums[1] = 2` leaves `[nums[0], nums[2]] = [1, 3]`.
*   The XOR of `[1, 3]` is 2, which equals `target`.
*   It is not possible to achieve XOR = 2 in less than one removal, therefore the answer is 1.

**Example 2:**

**Input:** nums = [2,4], target = 1

**Output:** \-1

**Explanation:**

It is impossible to remove elements to achieve `target`. Thus, the answer is -1.

**Example 3:**

**Input:** nums = [7], target = 7

**Output:** 0

**Explanation:**

The XOR of all elements is `nums[0] = 7`, which equals `target`. Thus, no removal is needed.

**Constraints:**

*   `1 <= nums.length <= 40`
*   <code>0 <= nums[i] <= 10<sup>4</sup></code>
*   <code>0 <= target <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minRemovals(int[] nums, int target) {
        int max = 0;
        for (int n : nums) {
            max = Math.max(max, n);
        }
        int u = 1 << (32 - Integer.numberOfLeadingZeros(max));
        if (target >= u) {
            return -1;
        }
        int n = nums.length;
        int[][] f = new int[n + 1][u];
        Arrays.fill(f[0], Integer.MAX_VALUE / 2);
        f[0][0] = 0;
        for (int i = 0; i < n; i++) {
            for (int x = 0; x < u; x++) {
                f[i + 1][x] = Math.min(f[i][x] + 1, f[i][x ^ nums[i]]);
            }
        }
        return f[n][target] == Integer.MAX_VALUE / 2 ? -1 : f[n][target];
    }
}
```