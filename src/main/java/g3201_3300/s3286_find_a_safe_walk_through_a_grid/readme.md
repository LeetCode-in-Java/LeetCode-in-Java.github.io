[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3286\. Find a Safe Walk Through a Grid

Medium

You are given an `m x n` binary matrix `grid` and an integer `health`.

You start on the upper-left corner `(0, 0)` and would like to get to the lower-right corner `(m - 1, n - 1)`.

You can move up, down, left, or right from one cell to another adjacent cell as long as your health _remains_ **positive**.

Cells `(i, j)` with `grid[i][j] = 1` are considered **unsafe** and reduce your health by 1.

Return `true` if you can reach the final cell with a health value of 1 or more, and `false` otherwise.

**Example 1:**

**Input:** grid = \[\[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]], health = 1

**Output:** true

**Explanation:**

The final cell can be reached safely by walking along the gray cells below.

![](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_1drawio.png)

**Example 2:**

**Input:** grid = \[\[0,1,1,0,0,0],[1,0,1,0,0,0],[0,1,1,1,0,1],[0,0,1,0,1,0]], health = 3

**Output:** false

**Explanation:**

A minimum of 4 health points is needed to reach the final cell safely.

![](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_2drawio.png)

**Example 3:**

**Input:** grid = \[\[1,1,1],[1,0,1],[1,1,1]], health = 5

**Output:** true

**Explanation:**

The final cell can be reached safely by walking along the gray cells below.

![](https://assets.leetcode.com/uploads/2024/08/04/3868_examples_3drawio.png)

Any path that does not go through the cell `(1, 1)` is unsafe since your health will drop to 0 when reaching the final cell.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 50`
*   `2 <= m * n`
*   `1 <= health <= m + n`
*   `grid[i][j]` is either 0 or 1.

## Solution

```java
import java.util.LinkedList;
import java.util.List;
import java.util.Objects;
import java.util.Queue;

public class Solution {
    public boolean findSafeWalk(List<List<Integer>> grid, int health) {
        int n = grid.size();
        int m = grid.get(0).size();
        int[] dr = new int[] {0, 0, 1, -1};
        int[] dc = new int[] {1, -1, 0, 0};
        boolean[][][] visited = new boolean[n][m][health + 1];
        Queue<int[]> bfs = new LinkedList<>();
        bfs.add(new int[] {0, 0, health - grid.get(0).get(0)});
        visited[0][0][health - grid.get(0).get(0)] = true;
        while (!bfs.isEmpty()) {
            int size = bfs.size();
            while (size-- > 0) {
                int[] currNode = bfs.poll();
                int r = Objects.requireNonNull(currNode)[0];
                int c = currNode[1];
                int h = currNode[2];
                if (r == n - 1 && c == m - 1 && h > 0) {
                    return true;
                }
                for (int k = 0; k < 4; k++) {
                    int nr = r + dr[k];
                    int nc = c + dc[k];
                    if (isValidMove(nr, nc, n, m)) {
                        int nh = h - grid.get(nr).get(nc);
                        if (nh >= 0 && !visited[nr][nc][nh]) {
                            visited[nr][nc][nh] = true;
                            bfs.add(new int[] {nr, nc, nh});
                        }
                    }
                }
            }
        }
        return false;
    }

    private boolean isValidMove(int r, int c, int n, int m) {
        return r >= 0 && c >= 0 && r < n && c < m;
    }
}
```