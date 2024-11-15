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
public class Solution {
    private static final int YY = 0;
    private static final int YN = 1;
    private static final int NY = 2;
    private static final int NN = 3;
    private static final int MOD = 1_000_000_007;

    public int maximumSumSubsequence(int[] nums, int[][] queries) {
        long[][] tree = build(nums);
        long result = 0;
        for (int i = 0; i < queries.length; ++i) {
            result += set(tree, queries[i][0], queries[i][1]);
            result %= MOD;
        }
        return (int) result;
    }

    private static long[][] build(int[] nums) {
        final int len = nums.length;
        int size = 1;
        while (size < len) {
            size <<= 1;
        }
        long[][] tree = new long[size * 2][4];
        for (int i = 0; i < len; ++i) {
            tree[size + i][YY] = nums[i];
        }
        for (int i = size - 1; i > 0; --i) {
            tree[i][YY] =
                    Math.max(
                            tree[2 * i][YY] + tree[2 * i + 1][NY],
                            tree[2 * i][YN] + Math.max(tree[2 * i + 1][YY], tree[2 * i + 1][NY]));
            tree[i][YN] =
                    Math.max(
                            tree[2 * i][YY] + tree[2 * i + 1][NN],
                            tree[2 * i][YN] + Math.max(tree[2 * i + 1][YN], tree[2 * i + 1][NN]));
            tree[i][NY] =
                    Math.max(
                            tree[2 * i][NY] + tree[2 * i + 1][NY],
                            tree[2 * i][NN] + Math.max(tree[2 * i + 1][YY], tree[2 * i + 1][NY]));
            tree[i][NN] =
                    Math.max(
                            tree[2 * i][NY] + tree[2 * i + 1][NN],
                            tree[2 * i][NN] + Math.max(tree[2 * i + 1][YN], tree[2 * i + 1][NN]));
        }
        return tree;
    }

    private static long set(long[][] tree, int idx, int val) {
        int size = tree.length / 2;
        tree[size + idx][YY] = val;
        for (int i = (size + idx) / 2; i > 0; i /= 2) {
            tree[i][YY] =
                    Math.max(
                            tree[2 * i][YY] + tree[2 * i + 1][NY],
                            tree[2 * i][YN] + Math.max(tree[2 * i + 1][YY], tree[2 * i + 1][NY]));
            tree[i][YN] =
                    Math.max(
                            tree[2 * i][YY] + tree[2 * i + 1][NN],
                            tree[2 * i][YN] + Math.max(tree[2 * i + 1][YN], tree[2 * i + 1][NN]));
            tree[i][NY] =
                    Math.max(
                            tree[2 * i][NY] + tree[2 * i + 1][NY],
                            tree[2 * i][NN] + Math.max(tree[2 * i + 1][YY], tree[2 * i + 1][NY]));
            tree[i][NN] =
                    Math.max(
                            tree[2 * i][NY] + tree[2 * i + 1][NN],
                            tree[2 * i][NN] + Math.max(tree[2 * i + 1][YN], tree[2 * i + 1][NN]));
        }
        return Math.max(tree[1][YY], Math.max(tree[1][YN], Math.max(tree[1][NY], tree[1][NN])));
    }
}
```