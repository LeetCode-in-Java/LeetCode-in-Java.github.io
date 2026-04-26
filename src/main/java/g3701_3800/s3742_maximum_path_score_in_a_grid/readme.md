[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3742\. Maximum Path Score in a Grid

Medium

You are given an `m x n` grid where each cell contains one of the values 0, 1, or 2. You are also given an integer `k`.

You start from the top-left corner `(0, 0)` and want to reach the bottom-right corner `(m - 1, n - 1)` by moving only **right** or **down**.

Each cell contributes a specific score and incurs an associated cost, according to their cell values:

*   0: adds 0 to your score and costs 0.
*   1: adds 1 to your score and costs 1.
*   2: adds 2 to your score and costs 1.

Return the **maximum** score achievable without exceeding a total cost of `k`, or -1 if no valid path exists.

**Note:** If you reach the last cell but the total cost exceeds `k`, the path is invalid.

**Example 1:**

**Input:** grid = \[\[0, 1],[2, 0]], k = 1

**Output:** 2

**Explanation:**

The optimal path is:

| Cell   | grid[i][j] | Score | Total Score | Cost | Total Cost |
|--------|------------|-------|-------------|------|------------|
| (0, 0) | 0          | 0     | 0           | 0    | 0          |
| (1, 0) | 2          | 2     | 2           | 1    | 1          |
| (1, 1) | 0          | 0     | 2           | 0    | 1          |

Thus, the maximum possible score is 2.

**Example 2:**

**Input:** grid = \[\[0, 1],[1, 2]], k = 1

**Output:** \-1

**Explanation:**

There is no path that reaches cell `(1, 1)` without exceeding cost k. Thus, the answer is -1.

**Constraints:**

*   `1 <= m, n <= 200`
*   <code>0 <= k <= 10<sup>3</sup></code>
*   `grid[0][0] == 0`
*   `0 <= grid[i][j] <= 2`

## Solution

```java
public class Solution {
    public int maxPathScore(int[][] grid, int k) {
        int n = grid.length;
        int m = grid[0].length;
        int mxc = Math.min(k + 1, n + m + 5);
        int[][][] dp = new int[n][m][mxc];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                for (int c = 0; c < mxc; c++) {
                    dp[i][j][c] = -1;
                }
            }
        }
        dp[0][0][0] = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                for (int c = 0; c < mxc; c++) {
                    if (dp[i][j][c] == -1) {
                        continue;
                    }
                    // move right
                    if (j + 1 < m) {
                        int cost = c + (grid[i][j + 1] > 0 ? 1 : 0);
                        if (cost < mxc) {
                            dp[i][j + 1][cost] =
                                    Math.max(dp[i][j + 1][cost], dp[i][j][c] + grid[i][j + 1]);
                        }
                    }
                    // move down
                    if (i + 1 < n) {
                        int cost = c + (grid[i + 1][j] > 0 ? 1 : 0);
                        if (cost < mxc) {
                            dp[i + 1][j][cost] =
                                    Math.max(dp[i + 1][j][cost], dp[i][j][c] + grid[i + 1][j]);
                        }
                    }
                }
            }
        }
        int ans = -1;
        for (int c = 0; c < mxc; c++) {
            ans = Math.max(ans, dp[n - 1][m - 1][c]);
        }
        return ans;
    }
}
```