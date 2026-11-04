[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3989\. Maximum Consistent Columns in a Grid

Hard

You are given a 2D integer array `grid` of size `m x n`, and an integer `limit`.

You may remove zero or more columns from the grid, but at least one column must remain. The **relative** order of the remaining columns must be preserved.

A grid is called **consistent** if for every row `i`, and for every pair of adjacent remaining columns `a` and `b` with `a < b`, the following holds: `|grid[i][b] - grid[i][a]| <= limit`.

Return the **maximum** number of columns that can remain such that the resulting grid is **consistent**.

**Example 1:**

**Input:** grid = \[\[-2,0,3]], limit = 2

**Output:** 2

**Explanation:**

*   Remove column 2 and keep columns 0 and 1, which gives `|grid[0][1] − grid[0][0]| = |0 − (−2)| = 2 <= limit`.
*   Thus, the maximum number of columns that can remain is 2.

**Example 2:**

**Input:** grid = \[\[1,-1,1],[2,2,2]], limit = 1

**Output:** 2

**Explanation:**

*   Remove column 1 and keep columns 0 and 2, which gives
    *   `|grid[0][2] − grid[0][0]| = |1 − 1| = 0 <= limit` and
    *   `|grid[1][2] − grid[1][0]| = |2 − 2| = 0 <= limit`.
*   Thus, the maximum number of columns that can remain is 2.

**Example 3:**

**Input:** grid = \[\[-5,5]], limit = 9

**Output:** 1

**Explanation:**

*   Remove either column 0 or column 1, since `|grid[0][1] − grid[0][0]| = |5 − (−5)| = 10 > limit`.
*   Thus, the maximum number of columns that can remain is 1.

**Constraints:**

*   `1 <= m == grid.length <= 250`
*   `1 <= n == grid[i].length <= 250`
*   <code>-10<sup>5</sup> <= grid[i][j] <= 10<sup>5</sup></code>
*   <code>0 <= limit <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int maxConsistentColumns(int[][] grid, int limit) {
        int n = grid[0].length;
        int max = 1;
        int[] dp = new int[n];
        for (int col = 0; col < n; ++col) {
            dp[col] = 1;
            for (int p = col - 1; p >= 0; --p) {
                if (dp[p] + 1 >= dp[col] && areColumnsConsistent(grid, col, p, limit)) {
                    dp[col] = dp[p] + 1;
                }
            }
            max = Math.max(max, dp[col]);
        }
        return max;
    }

    private boolean areColumnsConsistent(int[][] grid, int col, int previousCol, int limit) {
        for (int[] row : grid) {
            if (Math.abs(row[col] - row[previousCol]) > limit) {
                return false;
            }
        }
        return true;
    }
}
```