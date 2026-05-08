[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3768\. Minimum Inversion Count in Subarrays of Fixed Length

Hard

You are given an integer array `nums` of length `n` and an integer `k`.

An **inversion** is a pair of indices `(i, j)` from `nums` such that `i < j` and `nums[i] > nums[j]`.

The **inversion count** of a **non-empty subarrays** is the number of inversions within it.

Return the **minimum** inversion count among all **subarrays** of `nums` with length `k`.

**Example 1:**

**Input:** nums = [3,1,2,5,4], k = 3

**Output:** 0

**Explanation:**

We consider all subarrays of length `k = 3` (indices below are relative to each subarray):

*   `[3, 1, 2]` has 2 inversions: `(0, 1)` and `(0, 2)`.
*   `[1, 2, 5]` has 0 inversions.
*   `[2, 5, 4]` has 1 inversion: `(1, 2)`.

The minimum inversion count among all subarrays of length `3` is 0, achieved by subarray `[1, 2, 5]`.

**Example 2:**

**Input:** nums = [5,3,2,1], k = 4

**Output:** 6

**Explanation:**

There is only one subarray of length `k = 4`: `[5, 3, 2, 1]`.   
Within this subarray, the inversions are: `(0, 1)`, `(0, 2)`, `(0, 3)`, `(1, 2)`, `(1, 3)`, and `(2, 3)`.   
Total inversions is 6, so the minimum inversion count is 6.

**Example 3:**

**Input:** nums = [2,1], k = 1

**Output:** 0

**Explanation:**

All subarrays of length `k = 1` contain only one element, so no inversions are possible.   
The minimum inversion count is therefore 0.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= n`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long minInversionCount(int[] nums, int k) {
        int n = nums.length;
        // discretization of coordinates
        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        for (int i = 0; i < n; i++) {
            nums[i] = Arrays.binarySearch(sorted, nums[i]) + 1;
        }
        BinaryIndexedTree bit = new BinaryIndexedTree(n);
        long invCount = 0L;
        long ans = Long.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            bit.add(nums[i], 1);
            invCount += Math.min(i + 1, k) - bit.presum(nums[i]);
            if (i < k - 1) {
                continue;
            }
            ans = Math.min(ans, invCount);
            invCount -= bit.presum(nums[i - k + 1] - 1);
            bit.add(nums[i - k + 1], -1);
        }
        return ans;
    }

    private class BinaryIndexedTree {
        private final int[] tree;

        public BinaryIndexedTree(int n) {
            tree = new int[n + 1];
        }

        public void add(int i, int val) {
            while (i < tree.length) {
                tree[i] += val;
                i += lowbit(i);
            }
        }

        public int presum(int i) {
            int sum = 0;
            while (i > 0) {
                sum += tree[i];
                i -= lowbit(i);
            }
            return sum;
        }

        private int lowbit(int n) {
            return n & (-n);
        }
    }
}
```