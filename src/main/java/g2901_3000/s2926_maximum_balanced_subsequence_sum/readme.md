[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2926\. Maximum Balanced Subsequence Sum

Hard

You are given a **0-indexed** integer array `nums`.

A **subsequence** of `nums` having length `k` and consisting of **indices** <code>i<sub>0</sub> < i<sub>1</sub> < ... < i<sub>k-1</sub></code> is **balanced** if the following holds:

*   <code>nums[i<sub>j</sub>] - nums[i<sub>j-1</sub>] >= i<sub>j</sub> - i<sub>j-1</sub></code>, for every `j` in the range `[1, k - 1]`.

A **subsequence** of `nums` having length `1` is considered balanced.

Return _an integer denoting the **maximum** possible **sum of elements** in a **balanced** subsequence of_ `nums`.

A **subsequence** of an array is a new **non-empty** array that is formed from the original array by deleting some (**possibly none**) of the elements without disturbing the relative positions of the remaining elements.

**Example 1:**

**Input:** nums = [3,3,5,6]

**Output:** 14

**Explanation:** In this example, the subsequence [3,5,6] consisting of indices 0, 2, and 3 can be selected. 

nums[2] - nums[0] >= 2 - 0. 

nums[3] - nums[2] >= 3 - 2. 

Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums. 

The subsequence consisting of indices 1, 2, and 3 is also valid. 

It can be shown that it is not possible to get a balanced subsequence with a sum greater than 14.

**Example 2:**

**Input:** nums = [5,-1,-3,8]

**Output:** 13

**Explanation:** In this example, the subsequence [5,8] consisting of indices 0 and 3 can be selected. 

nums[3] - nums[0] >= 3 - 0. 

Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums. 

It can be shown that it is not possible to get a balanced subsequence with a sum greater than 13.

**Example 3:**

**Input:** nums = [-2,-1]

**Output:** -1

**Explanation:** In this example, the subsequence [-1] can be selected. It is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public long maxBalancedSubsequenceSum(int[] nums) {
        int n = nums.length;
        int m = 0;
        int[] arr = new int[n];
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < n; ++i) {
            int x = nums[i];
            if (x > 0) {
                arr[m++] = x - i;
            } else if (x > max) {
                max = x;
            }
        }
        if (m == 0) {
            return max;
        }
        Arrays.sort(arr);
        Map<Integer, Integer> map = new HashMap<>(m << 1);
        int pre = Integer.MIN_VALUE;
        int index = 1;
        for (int x : arr) {
            if (x == pre) {
                continue;
            }
            map.put(x, index++);
            pre = x;
        }

        BIT bit = new BIT(index);
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            if (nums[i] <= 0) {
                continue;
            }
            index = map.get(nums[i] - i);
            long cur = bit.query(index) + nums[i];
            bit.update(index, cur);
            ans = Math.max(ans, cur);
        }
        return ans;
    }

    private static class BIT {
        long[] tree;
        int n;

        public BIT(int n) {
            this.n = n;
            tree = new long[n + 1];
        }

        public int lowbit(int index) {
            return index & (-index);
        }

        public void update(int index, long v) {
            while (index <= n && tree[index] < v) {
                tree[index] = v;
                index += lowbit(index);
            }
        }

        public long query(int index) {
            long result = 0;
            while (index > 0) {
                result = Math.max(tree[index], result);
                index -= lowbit(index);
            }
            return result;
        }
    }
}
```