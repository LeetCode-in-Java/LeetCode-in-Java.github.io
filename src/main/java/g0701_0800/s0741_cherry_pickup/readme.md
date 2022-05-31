## 741\. Cherry Pickup

Hard

You are given an `n x n` `grid` representing a field of cherries, each cell is one of three possible integers.

*   `0` means the cell is empty, so you can pass through,
*   `1` means the cell contains a cherry that you can pick up and pass through, or
*   `-1` means the cell contains a thorn that blocks your way.

Return _the maximum number of cherries you can collect by following the rules below_:

*   Starting at the position `(0, 0)` and reaching `(n - 1, n - 1)` by moving right or down through valid path cells (cells with value `0` or `1`).
*   After reaching `(n - 1, n - 1)`, returning to `(0, 0)` by moving left or up through valid path cells.
*   When passing through a path cell containing a cherry, you pick it up, and the cell becomes an empty cell `0`.
*   If there is no valid path between `(0, 0)` and `(n - 1, n - 1)`, then no cherries can be collected.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/14/grid.jpg)

**Input:** grid = \[\[0,1,-1],[1,0,-1],[1,1,1]]

**Output:** 5

**Explanation:** The player started at (0, 0) and went down, down, right right to reach (2, 2). 4 cherries were picked up during this single trip, and the matrix becomes [[0,1,-1],[0,0,-1],[0,0,0]]. Then, the player went left, up, up, left to return home, picking up one more cherry. The total number of cherries picked up is 5, and this is the maximum possible.

**Example 2:**

**Input:** grid = \[\[1,1,-1],[1,-1,1],[-1,1,1]]

**Output:** 0

**Constraints:**

*   `n == grid.length`
*   `n == grid[i].length`
*   `1 <= n <= 50`
*   `grid[i][j]` is `-1`, `0`, or `1`.
*   `grid[0][0] != -1`
*   `grid[n - 1][n - 1] != -1`

## Solution

```java
public class Solution {
    public int cherryPickup(int[][] grid) {
        int[][][] dp = new int[grid.length][grid.length][grid.length];
        int ans = solve(0, 0, 0, grid, dp);
        return Math.max(ans, 0);
    }

    private int solve(int r1, int c1, int r2, int[][] arr, int[][][] dp) {
        int c2 = r1 + c1 - r2;
        if (r1 >= arr.length
                || r2 >= arr.length
                || c1 >= arr[0].length
                || c2 >= arr[0].length
                || arr[r1][c1] == -1
                || arr[r2][c2] == -1) {
            return Integer.MIN_VALUE;
        }

        if (r1 == arr.length - 1 && c1 == arr[0].length - 1) {
            return arr[r1][c1];
        }

        if (dp[r1][c1][r2] != 0) {
            return dp[r1][c1][r2];
        }

        int cherries = 0;
        if (r1 == r2 && c1 == c2) {
            cherries += arr[r1][c1];
        } else {
            cherries += arr[r1][c1] + arr[r2][c2];
        }

        int a = solve(r1, c1 + 1, r2, arr, dp);
        int b = solve(r1 + 1, c1, r2 + 1, arr, dp);
        int c = solve(r1, c1 + 1, r2 + 1, arr, dp);
        int d = solve(r1 + 1, c1, r2, arr, dp);

        cherries += Math.max(Math.max(a, b), Math.max(c, d));
        dp[r1][c1][r2] = cherries;
        return cherries;
    }
}
```