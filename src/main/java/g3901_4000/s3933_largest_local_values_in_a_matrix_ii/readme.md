[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3933\. Largest Local Values in a Matrix II

Medium

You are given an `n x m` integer matrix `matrix` containing non-negative integers.

A **non-zero** cell `(row, col)` checks the cells near it as follows:

*   Let `x = matrix[row][col]`.
*   Consider every cell within `x` rows and `x` columns of `(row, col)`.
*   Ignore cells that are outside the matrix.
*   Ignore the cells where both the row distance and column distance are exactly `x`.

The cell `(row, col)` is a **local maximum** if it is **non-zero** and no considered cell has a value **greater than** `x`.

Return an integer denoting the number of **local maximums** in `matrix`.

**Example 1:**

**Input:** matrix = \[\[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,2,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0]]

**Output:** 1

![](https://assets.leetcode.com/uploads/2026/05/13/chatgpt-image-may-14-2026-01_53_19-am.png)

**Explanation:**

*   For the non-zero cell `(3, 3)`, `x = matrix[3][3] = 2`.
*   The highlighted cells are the considered cells within `x` rows and `x` columns of `(3, 3)`.
*   The four cells with both row and column distances equal to `x = 2` are ignored.
*   No considered cell has a value greater than 2, so `(3, 3)` is a local maximum.
*   There are no other non-zero cells, so the answer is 1.

**Example 2:**

**Input:** matrix = \[\[1,2],[3,4]]

**Output:** 1

**Explanation:**

Only the cell with value 4 is a local maximum. Every other non-zero cell considers a cell with a greater value.

**Example 3:**

**Input:** matrix = \[\[1,0,1],[0,1,0],[1,0,1]]

**Output:** 5

**Explanation:**

*   For a cell with value 1, the considered cells are the cell itself and its 4-directionally adjacent cells that are inside the matrix.
*   Each of the five cells with value 1 only considers cells with values 0 or 1, so all five of them are local maximums.

**Example 4:**

**Input:** matrix = \[\[1,1],[1,1]]

**Output:** 4

**Explanation:**

All cells have the same value. Therefore, no cell considers another cell with a greater value, so all 4 cells are local maximums.

**Constraints:**

*   `1 <= n == matrix.length <= 200`
*   `1 <= m == matrix[i].length <= 200`
*   `0 <= matrix[i][j] <= 200`

## Solution

```java
public class Solution {
    public int countLocalMaximums(int[][] matrix) {
        int n = matrix.length;
        int m = matrix[0].length;
        int wn = 32 - Integer.numberOfLeadingZeros(n);
        int wm = 32 - Integer.numberOfLeadingZeros(m);
        int[][][][] st = buildSparseTable(matrix, n, m, wn, wm);
        return countMaximums(matrix, st, n, m);
    }

    private int[][][][] buildSparseTable(int[][] matrix, int n, int m, int wn, int wm) {
        int[][][][] st = new int[wn][wm][n][m];
        st[0][0] = matrix;
        buildHorizontalLevels(st, n, m, wm);
        buildVerticalLevels(st, n, m, wn, wm);
        return st;
    }

    private void buildHorizontalLevels(int[][][][] st, int n, int m, int wm) {
        for (int k2 = 1; k2 < wm; k2++) {
            int half = 1 << (k2 - 1);
            int width = 1 << k2;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j <= m - width; j++) {
                    st[0][k2][i][j] = Math.max(st[0][k2 - 1][i][j], st[0][k2 - 1][i][j + half]);
                }
            }
        }
    }

    private void buildVerticalLevels(int[][][][] st, int n, int m, int wn, int wm) {
        for (int k1 = 1; k1 < wn; k1++) {
            int half = 1 << (k1 - 1);
            int height = 1 << k1;
            for (int k2 = 0; k2 < wm; k2++) {
                int width = 1 << k2;
                for (int i = 0; i <= n - height; i++) {
                    for (int j = 0; j <= m - width; j++) {
                        st[k1][k2][i][j] =
                                Math.max(st[k1 - 1][k2][i][j], st[k1 - 1][k2][i + half][j]);
                    }
                }
            }
        }
    }

    private int countMaximums(int[][] matrix, int[][][][] st, int n, int m) {
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (isLocalMaximum(matrix, st, i, j, n, m)) {
                    ans++;
                }
            }
        }
        return ans;
    }

    private boolean isLocalMaximum(int[][] matrix, int[][][][] st, int i, int j, int n, int m) {
        int x = matrix[i][j];
        if (x == 0) {
            return false;
        }
        int max1 = query(st, i - x, j - x + 1, i + x + 1, j + x, n, m);
        int max2 = query(st, i - x + 1, j - x, i + x, j + x + 1, n, m);
        return Math.max(max1, max2) <= x;
    }

    private int query(int[][][][] st, int r1, int c1, int r2, int c2, int n, int m) {
        r1 = Math.max(r1, 0);
        c1 = Math.max(c1, 0);
        r2 = Math.min(r2, n);
        c2 = Math.min(c2, m);
        int k1 = 31 - Integer.numberOfLeadingZeros(r2 - r1);
        int k2 = 31 - Integer.numberOfLeadingZeros(c2 - c1);
        int bottom = r2 - (1 << k1);
        int right = c2 - (1 << k2);
        return Math.max(
                Math.max(st[k1][k2][r1][c1], st[k1][k2][bottom][c1]),
                Math.max(st[k1][k2][r1][right], st[k1][k2][bottom][right]));
    }
}
```