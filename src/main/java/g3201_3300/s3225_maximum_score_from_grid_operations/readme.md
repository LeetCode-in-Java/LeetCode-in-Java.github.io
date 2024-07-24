[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3225\. Maximum Score From Grid Operations

Hard

You are given a 2D matrix `grid` of size `n x n`. Initially, all cells of the grid are colored white. In one operation, you can select any cell of indices `(i, j)`, and color black all the cells of the <code>j<sup>th</sup></code> column starting from the top row down to the <code>i<sup>th</sup></code> row.

The grid score is the sum of all `grid[i][j]` such that cell `(i, j)` is white and it has a horizontally adjacent black cell.

Return the **maximum** score that can be achieved after some number of operations.

**Example 1:**

**Input:** grid = \[\[0,0,0,0,0],[0,0,3,0,0],[0,1,0,0,0],[5,0,0,3,0],[0,0,0,0,2]]

**Output:** 11

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/05/11/one.png)

In the first operation, we color all cells in column 1 down to row 3, and in the second operation, we color all cells in column 4 down to the last row. The score of the resulting grid is `grid[3][0] + grid[1][2] + grid[3][3]` which is equal to 11.

**Example 2:**

**Input:** grid = \[\[10,9,0,0,15],[7,1,0,8,0],[5,20,0,11,0],[0,0,0,1,2],[8,12,1,10,3]]

**Output:** 94

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/05/11/two-1.png)

We perform operations on 1, 2, and 3 down to rows 1, 4, and 0, respectively. The score of the resulting grid is `grid[0][0] + grid[1][0] + grid[2][1] + grid[4][1] + grid[1][3] + grid[2][3] + grid[3][3] + grid[4][3] + grid[0][4]` which is equal to 94.

**Constraints:**

*   `1 <= n == grid.length <= 100`
*   `n == grid[i].length`
*   <code>0 <= grid[i][j] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public long maximumScore(int[][] grid) {
        final int n = grid.length;
        long[] dp1 = new long[n];
        long[] dp2 = new long[n + 1];
        long[] dp3 = new long[n + 1];
        long[] dp12 = new long[n];
        long[] dp22 = new long[n + 1];
        long[] dp32 = new long[n + 1];
        long res = 0;
        for (int i = 0; i < n; ++i) {
            long sum = 0;
            long pre = 0;
            for (int[] ints : grid) {
                sum += ints[i];
            }
            for (int j = n - 1; j >= 0; --j) {
                long s2 = sum;
                dp12[j] = s2 + dp3[n];
                for (int k = 0; k <= j; ++k) {
                    s2 -= grid[k][i];
                    long v = Math.max(dp1[k] + s2, dp3[j] + s2);
                    v = Math.max(v, pre + s2);
                    dp12[j] = Math.max(dp12[j], v);
                    if (k == j) {
                        dp22[j] = dp32[j] = v;
                        res = Math.max(res, v);
                    }
                }
                if (i > 0) {
                    pre = Math.max(pre + grid[j][i], dp2[j] + grid[j][i]);
                }
                sum -= grid[j][i];
            }
            dp22[n] = dp32[n] = pre;
            res = Math.max(res, pre);
            for (int j = 1; j <= n; ++j) {
                dp32[j] = Math.max(dp32[j], dp32[j - 1]);
            }
            long[] tem = dp1;
            dp1 = dp12;
            dp12 = tem;
            tem = dp2;
            dp2 = dp22;
            dp22 = tem;
            tem = dp3;
            dp3 = dp32;
            dp32 = tem;
        }
        return res;
    }
}
```