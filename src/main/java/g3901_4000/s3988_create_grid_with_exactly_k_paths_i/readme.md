[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3988\. Create Grid With Exactly K Paths I

Medium

You are given three integers `m`, `n`, and `k`.

Construct **any** `m x n` grid consisting only of the characters `'.'` and `'#'`, where:

*   `'.'` represents a free cell.
*   `'#'` represents an obstacle cell.

A **valid path** is a sequence of free cells that:

*   Starts at the top-left cell `(0, 0)`.
*   Ends at the bottom-right cell `(m - 1, n - 1)`.
*   Moves only:
    *   Right, from `(i, j)` to `(i, j + 1)`, or
    *   Down, from `(i, j)` to `(i + 1, j)`.

Return any grid such that there are **exactly** `k` **valid paths** from the top-left cell to the bottom-right cell. If no such grid exists, return an empty array.

**Example 1:**

**Input:** m = 2, n = 3, k = 2

**Output:** ["...","#.."]

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/05/26/screenshot-2026-05-27-at-113554am.png)

There are exactly `k = 2` valid paths from `(0, 0)` to `(1, 2)`:

*   `(0, 0) → (0, 1) → (0, 2) → (1, 2)`
*   `(0, 0) → (0, 1) → (1, 1) → (1, 2)`

**Example 2:**

**Input:** m = 3, n = 3, k = 4

**Output:** ["..#","...","#.."]

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/05/26/screenshot-2026-05-27-at-113452am.png)

There are exactly `k = 4` valid paths from `(0, 0)` to `(2, 2)`:

*   `(0, 0) → (0, 1) → (1, 1) → (1, 2) → (2, 2)`
*   `(0, 0) → (0, 1) → (1, 1) → (2, 1) → (2, 2)`
*   `(0, 0) → (1, 0) → (1, 1) → (1, 2) → (2, 2)`
*   `(0, 0) → (1, 0) → (1, 1) → (2, 1) → (2, 2)`

**Example 3:**

**Input:** m = 1, n = 4, k = 2

**Output:** []

**Explanation:**

No grid exists with exactly `k = 2` valid paths for a `1 x 4` grid, so the answer is an empty array.

**Constraints:**

*   `1 <= m, n <= 10`
*   `1 <= k <= 4`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public String[] createGrid(int m, int n, int k) {
        int[] dimensions = getDimensions(m, n, k);
        if (dimensions.length == 0) {
            return new String[] {};
        }
        int dm = dimensions[0];
        int dn = dimensions[1];
        char[][] grid = initializeGrid(m, n);
        createRequiredPaths(grid, dm, dn);
        addObstacles(grid, dm, dn, k);
        connectRemainingGrid(grid, m, n, dm, dn);
        return convertToStrings(grid, m);
    }

    private int[] getDimensions(int m, int n, int k) {
        if (k == 1) {
            return new int[] {0, 0};
        }
        if (k == 2 && m >= 2 && n >= 2) {
            return new int[] {1, 1};
        }
        if (k == 3) {
            if (m >= 3 && n >= 2) {
                return new int[] {2, 1};
            }
            if (m >= 2 && n >= 3) {
                return new int[] {1, 2};
            }
        }
        if (k == 4) {
            return getFourPathDimensions(m, n);
        }
        return new int[0];
    }

    private int[] getFourPathDimensions(int m, int n) {
        if (m >= 4 && n >= 2) {
            return new int[] {3, 1};
        }
        if (m >= 2 && n >= 4) {
            return new int[] {1, 3};
        }
        if (m >= 3 && n >= 3) {
            return new int[] {2, 2};
        }
        return new int[0];
    }

    private char[][] initializeGrid(int m, int n) {
        char[][] grid = new char[m][n];
        for (char[] row : grid) {
            Arrays.fill(row, '#');
        }
        return grid;
    }

    private void createRequiredPaths(char[][] grid, int dm, int dn) {
        for (int i = 0; i <= dm; i++) {
            for (int j = 0; j <= dn; j++) {
                grid[i][j] = '.';
            }
        }
    }

    private void addObstacles(char[][] grid, int dm, int dn, int k) {
        if (dm == 2 && dn == 2 && k == 4) {
            grid[0][2] = '#';
            grid[2][0] = '#';
        }
    }

    private void connectRemainingGrid(char[][] grid, int m, int n, int dm, int dn) {
        connectVertically(grid, dm, dn, m);
        connectHorizontally(grid, m, dn, n);
    }

    private void connectVertically(char[][] grid, int dm, int dn, int m) {
        for (int i = dm + 1; i < m; i++) {
            grid[i][dn] = '.';
        }
    }

    private void connectHorizontally(char[][] grid, int m, int dn, int n) {
        for (int j = dn + 1; j < n; j++) {
            grid[m - 1][j] = '.';
        }
    }

    private String[] convertToStrings(char[][] grid, int m) {
        String[] ans = new String[m];
        for (int i = 0; i < m; i++) {
            ans[i] = new String(grid[i]);
        }
        return ans;
    }
}
```