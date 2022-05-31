## 1162\. As Far from Land as Possible

Medium

Given an `n x n` `grid` containing only values `0` and `1`, where `0` represents water and `1` represents land, find a water cell such that its distance to the nearest land cell is maximized, and return the distance. If no land or water exists in the grid, return `-1`.

The distance used in this problem is the Manhattan distance: the distance between two cells `(x0, y0)` and `(x1, y1)` is `|x0 - x1| + |y0 - y1|`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/03/1336_ex1.JPG)

**Input:** grid = \[\[1,0,1],[0,0,0],[1,0,1]]

**Output:** 2

**Explanation:** The cell (1, 1) is as far as possible from all the land with distance 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/05/03/1336_ex2.JPG)

**Input:** grid = \[\[1,0,0],[0,0,0],[0,0,0]]

**Output:** 4

**Explanation:** The cell (2, 2) is as far as possible from all the land with distance 4.

**Constraints:**

*   `n == grid.length`
*   `n == grid[i].length`
*   `1 <= n <= 100`
*   `grid[i][j]` is `0` or `1`

## Solution

```java
import java.util.LinkedList;
import java.util.Objects;
import java.util.Queue;

public class Solution {
    public int maxDistance(int[][] grid) {
        Queue<int[]> q = new LinkedList<>();
        int n = grid.length;
        int m = grid[0].length;
        boolean[][] vis = new boolean[n][m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 1) {
                    q.add(new int[] {i, j});
                    vis[i][j] = true;
                }
            }
        }
        if (q.isEmpty() || q.size() == n * m) {
            return -1;
        }
        int[] dir = {-1, 0, 1, 0, -1};
        int maxDistance = 0;
        int level = 1;
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                int[] top = q.poll();
                int currX = Objects.requireNonNull(top)[0];
                int currY = top[1];
                for (int j = 0; j < dir.length - 1; j++) {
                    int x = currX + dir[j];
                    int y = currY + dir[j + 1];

                    if (x >= 0 && x != n && y >= 0 && y != n && !vis[x][y]) {
                        maxDistance = Math.max(maxDistance, level);
                        vis[x][y] = true;
                        q.add(new int[] {x, y});
                    }
                }
            }
            level++;
        }
        return maxDistance;
    }
}
```