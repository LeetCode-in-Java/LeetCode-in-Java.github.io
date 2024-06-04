[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3165\. Maximum Sum of Subsequence With Non-adjacent Elements

Hard

You are given an array `nums` consisting of integers. You are also given a 2D array `queries`, where <code>queries[i] = [pos<sub>i</sub>, x<sub>i</sub>]</code>.

For query `i`, we first set <code>nums[pos<sub>i</sub>]</code> equal to <code>x<sub>i</sub></code>, then we calculate the answer to query `i` which is the **maximum** sum of a subsequence of `nums` where **no two adjacent elements are selected**.

Return the _sum_ of the answers to all queries.

Since the final answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [3,5,9], queries = \[\[1,-2],[0,-3]]

**Output:** 21

**Explanation:**   
 After the 1<sup>st</sup> query, `nums = [3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is `3 + 9 = 12`.   
 After the 2<sup>nd</sup> query, `nums = [-3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is 9.

**Example 2:**

**Input:** nums = [0,-1], queries = \[\[0,-5]]

**Output:** 0

**Explanation:**   
 After the 1<sup>st</sup> query, `nums = [-5,-1]` and the maximum sum of a subsequence with non-adjacent elements is 0 (choosing an empty subsequence).

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>queries[i] == [pos<sub>i</sub>, x<sub>i</sub>]</code>
*   <code>0 <= pos<sub>i</sub> <= nums.length - 1</code>
*   <code>-10<sup>5</sup> <= x<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```java
import java.util.stream.Stream;

public class Solution {
    private static final int MOD = 1_000_000_007;

    public int maximumSumSubsequence(int[] nums, int[][] queries) {
        int ans = 0;
        SegTree segTree = new SegTree(nums);
        for (int[] q : queries) {
            int idx = q[0];
            int val = q[1];
            segTree.update(idx, val);
            ans = (ans + segTree.getMax()) % MOD;
        }
        return ans;
    }

    static class SegTree {
        private static class Record {
            int takeFirstTakeLast;
            int takeFirstSkipLast;
            int skipFirstSkipLast;
            int skipFirstTakeLast;

            public Integer getMax() {
                return Stream.of(
                                this.takeFirstSkipLast,
                                this.takeFirstTakeLast,
                                this.skipFirstSkipLast,
                                this.skipFirstTakeLast)
                        .max(Integer::compare)
                        .orElse(null);
            }

            public Integer skipLast() {
                return Stream.of(this.takeFirstSkipLast, this.skipFirstSkipLast)
                        .max(Integer::compare)
                        .orElse(null);
            }

            public Integer takeLast() {
                return Stream.of(this.skipFirstTakeLast, this.takeFirstTakeLast)
                        .max(Integer::compare)
                        .orElse(null);
            }
        }

        private final Record[] seg;
        private final int[] nums;

        public SegTree(int[] nums) {
            this.nums = nums;
            seg = new Record[4 * nums.length];
            for (int i = 0; i < 4 * nums.length; ++i) {
                seg[i] = new Record();
            }
            build(0, nums.length - 1, 0);
        }

        private void build(int i, int j, int k) {
            if (i == j) {
                seg[k].takeFirstTakeLast = nums[i];
                return;
            }
            int mid = (i + j) >> 1;
            build(i, mid, 2 * k + 1);
            build(mid + 1, j, 2 * k + 2);
            merge(k);
        }

        // merge [2*k+1, 2*k+2] into k
        private void merge(int k) {
            seg[k].takeFirstSkipLast =
                    Math.max(
                            seg[2 * k + 1].takeFirstSkipLast + seg[2 * k + 2].skipLast(),
                            seg[2 * k + 1].takeFirstTakeLast + seg[2 * k + 2].skipFirstSkipLast);

            seg[k].takeFirstTakeLast =
                    Math.max(
                            seg[2 * k + 1].takeFirstSkipLast + seg[2 * k + 2].takeLast(),
                            seg[2 * k + 1].takeFirstTakeLast + seg[2 * k + 2].skipFirstTakeLast);

            seg[k].skipFirstTakeLast =
                    Math.max(
                            seg[2 * k + 1].skipFirstSkipLast + seg[2 * k + 2].takeLast(),
                            seg[2 * k + 1].skipFirstTakeLast + seg[2 * k + 2].skipFirstTakeLast);

            seg[k].skipFirstSkipLast =
                    Math.max(
                            seg[2 * k + 1].skipFirstSkipLast + seg[2 * k + 2].skipLast(),
                            seg[2 * k + 1].skipFirstTakeLast + seg[2 * k + 2].skipFirstSkipLast);
        }

        // child -> parent
        public void update(int idx, int val) {
            int i = 0;
            int j = nums.length - 1;
            int k = 0;
            update(idx, val, k, i, j);
        }

        private void update(int idx, int val, int k, int i, int j) {
            if (i == j) {
                seg[k].takeFirstTakeLast = val;
                return;
            }
            int mid = (i + j) >> 1;
            if (idx <= mid) {
                update(idx, val, 2 * k + 1, i, mid);
            } else {
                update(idx, val, 2 * k + 2, mid + 1, j);
            }
            merge(k);
        }

        public int getMax() {
            return seg[0].getMax();
        }
    }
}
```