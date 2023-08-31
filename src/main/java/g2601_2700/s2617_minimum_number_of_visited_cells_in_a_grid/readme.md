[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2617\. Minimum Number of Visited Cells in a Grid

Hard

You are given a **0-indexed** `m x n` integer matrix `grid`. Your initial position is at the **top-left** cell `(0, 0)`.

Starting from the cell `(i, j)`, you can move to one of the following cells:

*   Cells `(i, k)` with `j < k <= grid[i][j] + j` (rightward movement), or
*   Cells `(k, j)` with `i < k <= grid[i][j] + i` (downward movement).

Return _the minimum number of cells you need to visit to reach the **bottom-right** cell_ `(m - 1, n - 1)`. If there is no valid path, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/01/25/ex1.png)

**Input:** grid = \[\[3,4,2,1],[4,2,3,1],[2,1,0,0],[2,4,0,0]]

**Output:** 4

**Explanation:** The image above shows one of the paths that visits exactly 4 cells.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/01/25/ex2.png)

**Input:** grid = \[\[3,4,2,1],[4,2,1,1],[2,1,1,0],[3,4,1,0]]

**Output:** 3

**Explanation:** The image above shows one of the paths that visits exactly 3 cells.

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/01/26/ex3.png)

**Input:** grid = \[\[2,1,0],[1,0,0]]

**Output:** -1

**Explanation:** It can be proven that no path exists.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   `0 <= grid[i][j] < m * n`
*   `grid[m - 1][n - 1] == 0`

## Solution

```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Queue;

public class Solution {
    private static final int LIMIT = 2;

    public int minimumVisitedCells(int[][] grid) {
        int[][] len = new int[grid.length][grid[0].length];
        for (int[] ints : len) {
            Arrays.fill(ints, -1);
        }
        Queue<int[]> q = new ArrayDeque<>();
        q.add(new int[] {0, 0});
        len[0][0] = 1;
        while (!q.isEmpty()) {
            int[] tmp = q.poll();
            int i = tmp[0];
            int j = tmp[1];
            int c = 0;
            for (int k = Math.min(grid[0].length - 1, grid[i][j] + j); k > j; k--) {
                if (len[i][k] != -1) {
                    c++;
                    if (c > LIMIT) {
                        break;
                    }
                } else {
                    len[i][k] = len[i][j] + 1;
                    q.add(new int[] {i, k});
                }
            }
            if (len[grid.length - 1][grid[0].length - 1] != -1) {
                return len[grid.length - 1][grid[0].length - 1];
            }
            c = 0;
            for (int k = Math.min(grid.length - 1, grid[i][j] + i); k > i; k--) {
                if (len[k][j] != -1) {
                    c++;
                    if (c > LIMIT) {
                        break;
                    }
                } else {
                    len[k][j] = len[i][j] + 1;
                    q.add(new int[] {k, j});
                }
            }
            if (len[grid.length - 1][grid[0].length - 1] != -1) {
                return len[grid.length - 1][grid[0].length - 1];
            }
        }
        return -1;
    }
}
```