## 1263\. Minimum Moves to Move a Box to Their Target Location

Hard

A storekeeper is a game in which the player pushes boxes around in a warehouse trying to get them to target locations.

The game is represented by an `m x n` grid of characters `grid` where each element is a wall, floor, or box.

Your task is to move the box `'B'` to the target position `'T'` under the following rules:

*   The character `'S'` represents the player. The player can move up, down, left, right in `grid` if it is a floor (empty cell).
*   The character `'.'` represents the floor which means a free cell to walk.
*   The character `'#'` represents the wall which means an obstacle (impossible to walk there).
*   There is only one box `'B'` and one target cell `'T'` in the `grid`.
*   The box can be moved to an adjacent free cell by standing next to the box and then moving in the direction of the box. This is a **push**.
*   The player cannot walk through the box.

Return _the minimum number of **pushes** to move the box to the target_. If there is no way to reach the target, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/11/06/sample_1_1620.png)

**Input:**

    grid = [["#","#","#","#","#","#"],
            ["#","T","#","#","#","#"],
            ["#",".",".","B",".","#"],
            ["#",".","#","#",".","#"],
            ["#",".",".",".","S","#"],
            ["#","#","#","#","#","#"]]

**Output:** 3

**Explanation:** We return only the number of times the box is pushed.

**Example 2:**

**Input:**

    grid = [["#","#","#","#","#","#"],
            ["#","T","#","#","#","#"],
            ["#",".",".","B",".","#"],
            ["#","#","#","#",".","#"],
            ["#",".",".",".","S","#"],
            ["#","#","#","#","#","#"]]

**Output:** -1

**Example 3:**

**Input:**

    grid = [["#","#","#","#","#","#"],
            ["#","T",".",".","#","#"],
            ["#",".","#","B",".","#"],
            ["#",".",".",".",".","#"],
            ["#",".",".",".","S","#"],
            ["#","#","#","#","#","#"]]

**Output:** 5

**Explanation:** push the box down, left, left, up and up.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 20`
*   `grid` contains only characters `'.'`, `'#'`, `'S'`, `'T'`, or `'B'`.
*   There is only one character `'S'`, `'B'`, and `'T'` in the `grid`.

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    private int n;
    private int m;
    private char[][] grid;
    private final int[][] dirs = { {1, 0}, {0, 1}, {-1, 0}, {0, -1}};

    public int minPushBox(char[][] grid) {
        n = grid.length;
        m = grid[0].length;
        this.grid = grid;
        int[] box = new int[2];
        int[] target = new int[2];
        int[] player = new int[2];
        findLocations(box, target, player);
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[] {box[0], box[1], player[0], player[1]});
        // for 4 directions
        boolean[][][] visited = new boolean[n][m][4];
        int steps = 0;
        while (!q.isEmpty()) {
            int size = q.size();
            while (size-- > 0) {
                int[] cur = q.poll();
                if (cur != null && cur[0] == target[0] && cur[1] == target[1]) {
                    return steps;
                }
                for (int i = 0; i < 4; i++) {
                    if (cur != null) {
                        int[] newPlayerLoc = new int[] {cur[0] + dirs[i][0], cur[1] + dirs[i][1]};
                        int[] newBoxLoc = new int[] {cur[0] - dirs[i][0], cur[1] - dirs[i][1]};
                        if (visited[cur[0]][cur[1]][i]
                                || isOutOfBounds(newPlayerLoc, newBoxLoc)
                                || !isReachable(newPlayerLoc, cur)) {
                            continue;
                        }
                        visited[cur[0]][cur[1]][i] = true;
                        q.offer(new int[] {newBoxLoc[0], newBoxLoc[1], cur[0], cur[1]});
                    }
                }
            }
            steps++;
        }
        return -1;
    }

    private boolean isReachable(int[] targetPlayerLoc, int[] cur) {
        boolean[][] visited = new boolean[n][m];
        visited[cur[0]][cur[1]] = true;
        visited[cur[2]][cur[3]] = true;
        Queue<int[]> q = new LinkedList<>();
        q.offer(new int[] {cur[2], cur[3]});
        while (!q.isEmpty()) {
            int[] playerLoc = q.poll();
            if (playerLoc[0] == targetPlayerLoc[0] && playerLoc[1] == targetPlayerLoc[1]) {
                return true;
            }
            for (int[] d : dirs) {
                int x = playerLoc[0] + d[0];
                int y = playerLoc[1] + d[1];
                if (isOutOfBounds(x, y) || visited[x][y]) {
                    continue;
                }
                visited[x][y] = true;
                q.offer(new int[] {x, y});
            }
        }
        return false;
    }

    private boolean isOutOfBounds(int[] player, int[] box) {
        return isOutOfBounds(player[0], player[1]) || isOutOfBounds(box[0], box[1]);
    }

    private boolean isOutOfBounds(int x, int y) {
        return (x < 0 || y < 0 || x == n || y == m || grid[x][y] == '#');
    }

    private void findLocations(int[] box, int[] target, int[] player) {
        boolean p = false;
        boolean t = false;
        boolean b = false;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 'S') {
                    player[0] = i;
                    player[1] = j;
                    p = true;
                } else if (grid[i][j] == 'T') {
                    target[0] = i;
                    target[1] = j;
                    t = true;
                } else if (grid[i][j] == 'B') {
                    box[0] = i;
                    box[1] = j;
                    b = true;
                }
                if (p && b && t) {
                    // found all
                    return;
                }
            }
        }
    }
}
```