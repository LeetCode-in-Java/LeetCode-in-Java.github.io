[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1703\. Minimum Adjacent Swaps for K Consecutive Ones

Hard

You are given an integer array, `nums`, and an integer `k`. `nums` comprises of only `0`'s and `1`'s. In one move, you can choose two **adjacent** indices and swap their values.

Return _the **minimum** number of moves required so that_ `nums` _has_ `k` _**consecutive**_ `1`_'s_.

**Example 1:**

**Input:** nums = [1,0,0,1,0,1], k = 2

**Output:** 1

**Explanation:** In 1 move, nums could be [1,0,0,0,1,1] and have 2 consecutive 1's.

**Example 2:**

**Input:** nums = [1,0,0,0,0,0,1,1], k = 3

**Output:** 5

**Explanation:** In 5 moves, the leftmost 1 can be shifted right until nums = [0,0,0,0,0,1,1,1].

**Example 3:**

**Input:** nums = [1,1,0,1], k = 2

**Output:** 0

**Explanation:** nums already has 2 consecutive 1's.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is `0` or `1`.
*   `1 <= k <= sum(nums)`

## Solution

```java
public class Solution {
    public int minMoves(int[] nums, int k) {
        int len = nums.length;
        int cnt = 0;
        long min = Long.MAX_VALUE;
        for (int num : nums) {
            if (num == 1) {
                cnt++;
            }
        }
        int[] arr = new int[cnt];
        int idx = 0;
        long[] sum = new long[cnt + 1];
        for (int i = 0; i < len; i++) {
            if (nums[i] == 1) {
                arr[idx++] = i;
                sum[idx] = sum[idx - 1] + i;
            }
        }
        for (int i = 0; i + k - 1 < cnt; i++) {
            min = Math.min(min, getSum(arr, i, i + k - 1, sum));
        }
        return (int) min;
    }

    private long getSum(int[] arr, int l, int h, long[] sum) {
        int mid = l + (h - l) / 2;
        int k = h - l + 1;
        int radius = mid - l;
        long res = sum[h + 1] - sum[mid + 1] - (sum[mid] - sum[l]) - (long) (1 + radius) * radius;
        if (k % 2 == 0) {
            res = res - arr[mid] - (radius + 1);
        }
        return res;
    }
}
```