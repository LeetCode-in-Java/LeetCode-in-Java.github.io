[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 934\. Shortest Bridge

Medium

You are given an `n x n` binary matrix `grid` where `1` represents land and `0` represents water.

An **island** is a 4-directionally connected group of `1`'s not connected to any other `1`'s. There are **exactly two islands** in `grid`.

You may change `0`'s to `1`'s to connect the two islands to form **one island**.

Return _the smallest number of_ `0`_'s you must flip to connect the two islands_.

**Example 1:**

**Input:** grid = \[\[0,1],[1,0]]

**Output:** 1

**Example 2:**

**Input:** grid = \[\[0,1,0],[0,0,0],[0,0,1]]

**Output:** 2

**Example 3:**

**Input:** grid = \[\[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]

**Output:** 1

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `2 <= n <= 100`
*   `grid[i][j]` is either `0` or `1`.
*   There are exactly two islands in `grid`.

## Solution

```java
import java.util.ArrayDeque;

public class Solution {
    private static class Pair {
        int x;
        int y;

        Pair(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    private int[][] dirs = { {1, 0}, {0, 1}, {-1, 0}, {0, -1}};

    public int shortestBridge(int[][] grid) {
        ArrayDeque<Pair> q = new ArrayDeque<>();
        boolean flag = false;
        boolean[][] visited = new boolean[grid.length][grid[0].length];
        for (int i = 0; i < grid.length && !flag; i++) {
            for (int j = 0; j < grid[i].length && !flag; j++) {
                if (grid[i][j] == 1) {
                    dfs(grid, i, j, visited, q);
                    flag = true;
                }
            }
        }
        int level = -1;
        while (!q.isEmpty()) {
            int size = q.size();
            level++;
            while (size-- > 0) {
                Pair rem = q.removeFirst();
                for (int[] dir : dirs) {
                    int newrow = rem.x + dir[0];
                    int newcol = rem.y + dir[1];
                    if (newrow >= 0
                            && newcol >= 0
                            && newrow < grid.length
                            && newcol < grid[0].length
                            && !visited[newrow][newcol]) {
                        if (grid[newrow][newcol] == 1) {
                            return level;
                        }
                        q.add(new Pair(newrow, newcol));
                        visited[newrow][newcol] = true;
                    }
                }
            }
        }
        return -1;
    }

    private void dfs(int[][] grid, int row, int col, boolean[][] visited, ArrayDeque<Pair> q) {
        visited[row][col] = true;
        q.add(new Pair(row, col));
        for (int[] dir : dirs) {
            int newrow = row + dir[0];
            int newcol = col + dir[1];
            if (newrow >= 0
                    && newcol >= 0
                    && newrow < grid.length
                    && newcol < grid[0].length
                    && !visited[newrow][newcol]
                    && grid[newrow][newcol] == 1) {
                dfs(grid, newrow, newcol, visited, q);
            }
        }
    }
}
```