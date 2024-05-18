[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1391\. Check if There is a Valid Path in a Grid

Medium

You are given an `m x n` `grid`. Each cell of `grid` represents a street. The street of `grid[i][j]` can be:

*   `1` which means a street connecting the left cell and the right cell.
*   `2` which means a street connecting the upper cell and the lower cell.
*   `3` which means a street connecting the left cell and the lower cell.
*   `4` which means a street connecting the right cell and the lower cell.
*   `5` which means a street connecting the left cell and the upper cell.
*   `6` which means a street connecting the right cell and the upper cell.

![](https://assets.leetcode.com/uploads/2020/03/05/main.png)

You will initially start at the street of the upper-left cell `(0, 0)`. A valid path in the grid is a path that starts from the upper left cell `(0, 0)` and ends at the bottom-right cell `(m - 1, n - 1)`. **The path should only follow the streets**.

**Notice** that you are **not allowed** to change any street.

Return `true` _if there is a valid path in the grid or_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/03/05/e1.png)

**Input:** grid = \[\[2,4,3],[6,5,2]]

**Output:** true

**Explanation:** As shown you can start at cell (0, 0) and visit all the cells of the grid to reach (m - 1, n - 1).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/03/05/e2.png)

**Input:** grid = \[\[1,2,1],[1,2,1]]

**Output:** false

**Explanation:** As shown you the street at cell (0, 0) is not connected with any street of any other cell and you will get stuck at cell (0, 0)

**Example 3:**

**Input:** grid = \[\[1,1,2]]

**Output:** false

**Explanation:** You will get stuck at cell (0, 1) and you cannot reach cell (0, 2).

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 300`
*   `1 <= grid[i][j] <= 6`

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    private int[][][] dirs = {
        { {0, -1}, {0, 1}},
        { {-1, 0}, {1, 0}},
        { {0, -1}, {1, 0}},
        { {0, 1}, {1, 0}},
        { {0, -1}, {-1, 0}},
        { {0, 1}, {-1, 0}}
    };

    // the idea is you need to check port direction match, you can go to next cell and check whether
    // you can come back.
    public boolean hasValidPath(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> q = new LinkedList<>();
        q.add(new int[] {0, 0});
        visited[0][0] = true;
        while (!q.isEmpty()) {
            int[] cur = q.poll();
            int x = cur[0];
            int y = cur[1];
            int num = grid[x][y] - 1;
            for (int[] dir : dirs[num]) {
                int nx = x + dir[0];
                int ny = y + dir[1];
                if (nx < 0 || nx >= m || ny < 0 || ny >= n || visited[nx][ny]) {
                    continue;
                }
                // go to the next cell and come back to orign to see if port directions are same
                for (int[] backDir : dirs[grid[nx][ny] - 1]) {
                    if (nx + backDir[0] == x && ny + backDir[1] == y) {
                        visited[nx][ny] = true;
                        q.add(new int[] {nx, ny});
                    }
                }
            }
        }
        return visited[m - 1][n - 1];
    }
}
```