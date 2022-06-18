## 2258\. Escape the Spreading Fire

Hard

You are given a **0-indexed** 2D integer array `grid` of size `m x n` which represents a field. Each cell has one of three values:

*   `0` represents grass,
*   `1` represents fire,
*   `2` represents a wall that you and fire cannot pass through.

You are situated in the top-left cell, `(0, 0)`, and you want to travel to the safehouse at the bottom-right cell, `(m - 1, n - 1)`. Every minute, you may move to an **adjacent** grass cell. **After** your move, every fire cell will spread to all **adjacent** cells that are not walls.

Return _the **maximum** number of minutes that you can stay in your initial position before moving while still safely reaching the safehouse_. If this is impossible, return `-1`. If you can **always** reach the safehouse regardless of the minutes stayed, return <code>10<sup>9</sup></code>.

Note that even if the fire spreads to the safehouse immediately after you have reached it, it will be counted as safely reaching the safehouse.

A cell is **adjacent** to another cell if the former is directly north, east, south, or west of the latter (i.e., their sides are touching).

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/10/ex1new.jpg)

**Input:** grid = \[\[0,2,0,0,0,0,0],[0,0,0,2,2,1,0],[0,2,0,0,1,2,0],[0,0,2,2,2,0,2],[0,0,0,0,0,0,0]]

**Output:** 3

**Explanation:** The figure above shows the scenario where you stay in the initial position for 3 minutes.

You will still be able to safely reach the safehouse.

Staying for more than 3 minutes will not allow you to safely reach the safehouse.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/10/ex2new2.jpg)

**Input:** grid = \[\[0,0,0,0],[0,1,2,0],[0,2,0,0]]

**Output:** -1

**Explanation:** The figure above shows the scenario where you immediately move towards the safehouse.

Fire will spread to any cell you move towards and it is impossible to safely reach the safehouse.

Thus, -1 is returned.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/03/10/ex3new.jpg)

**Input:** grid = \[\[0,0,0],[2,2,0],[1,2,0]]

**Output:** 1000000000

**Explanation:** The figure above shows the initial grid. Notice that the fire is contained by walls and you will always be able to safely reach the safehouse. Thus, 10<sup>9</sup> is returned. 

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `2 <= m, n <= 300`
*   <code>4 <= m * n <= 2 * 10<sup>4</sup></code>
*   `grid[i][j]` is either `0`, `1`, or `2`.
*   `grid[0][0] == grid[m - 1][n - 1] == 0`

## Solution

```java
import java.util.ArrayDeque;

public class Solution {
    private int[][] setFire(int[][] grid, int[][] dir) {
        int n = grid.length;
        int m = grid[0].length;
        ArrayDeque<Integer> bfs = new ArrayDeque<>();
        int[][] fire = new int[n][m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                fire[i][j] = Integer.MAX_VALUE;
                if (grid[i][j] == 1) {
                    fire[i][j] = 0;
                    bfs.add(i * m + j);
                }
                if (grid[i][j] == 2) {
                    fire[i][j] = 0;
                }
            }
        }
        while (!bfs.isEmpty()) {
            int rm = bfs.removeFirst();
            int x = rm / m;
            int y = rm % m;
            for (int d = 0; d < 4; d++) {
                int nx = x + dir[d][0];
                int ny = y + dir[d][1];
                if (nx >= 0 && ny >= 0 && nx < n && ny < m && fire[nx][ny] == Integer.MAX_VALUE) {
                    fire[nx][ny] = fire[x][y] + 1;
                    bfs.add(nx * m + ny);
                }
            }
        }
        return fire;
    }

    private boolean isPoss(int[][] fire, int[][] dir, int time) {
        if (time >= fire[0][0]) {
            return false;
        }
        int n = fire.length;
        int m = fire[0].length;
        ArrayDeque<Integer> bfs = new ArrayDeque<>();
        bfs.add(0);
        boolean[][] isVis = new boolean[n][m];
        isVis[0][0] = true;
        while (!bfs.isEmpty()) {
            int size = bfs.size();
            while (size-- > 0) {
                int rm = bfs.removeFirst();
                int x = rm / m;
                int y = rm % m;
                if (x == n - 1 && y == m - 1) {
                    return true;
                }
                for (int d = 0; d < 4; d++) {
                    int nx = x + dir[d][0];
                    int ny = y + dir[d][1];
                    if (nx >= 0 && ny >= 0 && nx < n && ny < m && !isVis[nx][ny]) {
                        if (nx == n - 1 && ny == m - 1) {
                            if (time + 1 <= fire[nx][ny]) {
                                isVis[nx][ny] = true;
                                bfs.add(nx * m + ny);
                            }
                        } else {
                            if (time + 1 < fire[nx][ny]) {
                                isVis[nx][ny] = true;
                                bfs.add(nx * m + ny);
                            }
                        }
                    }
                }
            }
            time++;
        }
        return false;
    }

    public int maximumMinutes(int[][] grid) {
        int[][] dir = { {0, 1}, {1, 0}, {-1, 0}, {0, -1}};
        int[][] fire = setFire(grid, dir);
        int lo = 0;
        int hi = (int) 1e9;
        while (lo <= hi) {
            int mid = ((hi - lo) >> 1) + lo;
            if (isPoss(fire, dir, mid)) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
        return hi;
    }
}
```