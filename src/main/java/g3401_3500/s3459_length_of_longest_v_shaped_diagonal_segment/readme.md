[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3459\. Length of Longest V-Shaped Diagonal Segment

Hard

You are given a 2D integer matrix `grid` of size `n x m`, where each element is either `0`, `1`, or `2`.

A **V-shaped diagonal segment** is defined as:

*   The segment starts with `1`.
*   The subsequent elements follow this infinite sequence: `2, 0, 2, 0, ...`.
*   The segment:
    *   Starts **along** a diagonal direction (top-left to bottom-right, bottom-right to top-left, top-right to bottom-left, or bottom-left to top-right).
    *   Continues the **sequence** in the same diagonal direction.
    *   Makes **at most one clockwise 90-degree** **turn** to another diagonal direction while **maintaining** the sequence.

![](https://assets.leetcode.com/uploads/2025/01/11/length_of_longest3.jpg)

Return the **length** of the **longest** **V-shaped diagonal segment**. If no valid segment _exists_, return 0.

**Example 1:**

**Input:** grid = \[\[2,2,1,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]

**Output:** 5

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/12/09/matrix_1-2.jpg)

The longest V-shaped diagonal segment has a length of 5 and follows these coordinates: `(0,2) → (1,3) → (2,4)`, takes a **90-degree clockwise turn** at `(2,4)`, and continues as `(3,3) → (4,2)`.

**Example 2:**

**Input:** grid = \[\[2,2,2,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]

**Output:** 4

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/12/09/matrix_2.jpg)**

The longest V-shaped diagonal segment has a length of 4 and follows these coordinates: `(2,3) → (3,2)`, takes a **90-degree clockwise turn** at `(3,2)`, and continues as `(2,1) → (1,0)`.

**Example 3:**

**Input:** grid = \[\[1,2,2,2,2],[2,2,2,2,0],[2,0,0,0,0],[0,0,2,2,2],[2,0,0,2,0]]

**Output:** 5

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/12/09/matrix_3.jpg)**

The longest V-shaped diagonal segment has a length of 5 and follows these coordinates: `(0,0) → (1,1) → (2,2) → (3,3) → (4,4)`.

**Example 4:**

**Input:** grid = \[\[1]]

**Output:** 1

**Explanation:**

The longest V-shaped diagonal segment has a length of 1 and follows these coordinates: `(0,0)`.

**Constraints:**

*   `n == grid.length`
*   `m == grid[i].length`
*   `1 <= n, m <= 500`
*   `grid[i][j]` is either `0`, `1` or `2`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private final int[][] ds = { {1, 1}, {1, -1}, {-1, -1}, {-1, 1}};
    private final int[] nx = {2, 2, 0};
    private int[][] grid;
    private int n;
    private int m;
    private int[][][][] dp;

    public int lenOfVDiagonal(int[][] g) {
        this.grid = g;
        this.n = g.length;
        this.m = g[0].length;
        this.dp = new int[n][m][4][2];
        for (int[][][] d1 : dp) {
            for (int[][] d2 : d1) {
                for (int[] d3 : d2) {
                    Arrays.fill(d3, -1);
                }
            }
        }
        int res = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (g[i][j] == 1) {
                    for (int d = 0; d < 4; d++) {
                        res = Math.max(res, dp(i, j, 1, d, 1));
                    }
                }
            }
        }
        return res;
    }

    private int dp(int i, int j, int x, int d, int k) {
        if (i < 0 || i >= n || j < 0 || j >= m) {
            return 0;
        }
        if (grid[i][j] != x) {
            return 0;
        }
        if (dp[i][j][d][k] != -1) {
            return dp[i][j][d][k];
        }
        int res = dp(i + ds[d][0], j + ds[d][1], nx[x], d, k) + 1;
        if (k > 0) {
            int d2 = (d + 1) % 4;
            int res2 = dp(i + ds[d2][0], j + ds[d2][1], nx[x], d2, 0) + 1;
            res = Math.max(res, res2);
        }
        dp[i][j][d][k] = res;
        return res;
    }
}
```