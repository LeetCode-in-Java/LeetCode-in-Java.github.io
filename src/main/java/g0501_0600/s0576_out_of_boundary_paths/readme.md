[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 576\. Out of Boundary Paths

Medium

There is an `m x n` grid with a ball. The ball is initially at the position `[startRow, startColumn]`. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply **at most** `maxMove` moves to the ball.

Given the five integers `m`, `n`, `maxMove`, `startRow`, `startColumn`, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)

**Input:** m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0

**Output:** 6

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png)

**Input:** m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1

**Output:** 12

**Constraints:**

*   `1 <= m, n <= 50`
*   `0 <= maxMove <= 50`
*   `0 <= startRow < m`
*   `0 <= startColumn < n`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private final int[][] dRowCol = { {1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    private int dfs(int m, int n, int remainingMoves, int currRow, int currCol, int[][][] cache) {
        if (currRow < 0 || currRow == m || currCol < 0 || currCol == n) {
            return 1;
        }
        if (remainingMoves == 0) {
            return 0;
        }

        if (cache[currRow][currCol][remainingMoves] == -1) {
            int paths = 0;
            for (int i = 0; i < 4; i++) {
                int newRow = currRow + dRowCol[i][0];
                int newCol = currCol + dRowCol[i][1];
                int m1 = 1000000007;
                paths = (paths + dfs(m, n, remainingMoves - 1, newRow, newCol, cache)) % m1;
            }
            cache[currRow][currCol][remainingMoves] = paths;
        }
        return cache[currRow][currCol][remainingMoves];
    }

    public int findPaths(int m, int n, int maxMoves, int startRow, int startCol) {
        int[][][] cache = new int[m][n][maxMoves + 1];
        for (int[][] c1 : cache) {
            for (int[] c2 : c1) {
                Arrays.fill(c2, -1);
            }
        }

        return dfs(m, n, maxMoves, startRow, startCol, cache);
    }
}
```