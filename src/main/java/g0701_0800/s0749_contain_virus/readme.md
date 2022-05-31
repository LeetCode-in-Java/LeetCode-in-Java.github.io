## 749\. Contain Virus

Hard

A virus is spreading rapidly, and your task is to quarantine the infected area by installing walls.

The world is modeled as an `m x n` binary grid `isInfected`, where `isInfected[i][j] == 0` represents uninfected cells, and `isInfected[i][j] == 1` represents cells contaminated with the virus. A wall (and only one wall) can be installed between any two **4-directionally** adjacent cells, on the shared boundary.

Every night, the virus spreads to all neighboring cells in all four directions unless blocked by a wall. Resources are limited. Each day, you can install walls around only one region (i.e., the affected area (continuous block of infected cells) that threatens the most uninfected cells the following night). There **will never be a tie**.

Return _the number of walls used to quarantine all the infected regions_. If the world will become fully infected, return the number of walls used.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/01/virus11-grid.jpg)

**Input:** isInfected = \[\[0,1,0,0,0,0,0,1],[0,1,0,0,0,0,0,1],[0,0,0,0,0,0,0,1],[0,0,0,0,0,0,0,0]]

**Output:** 10

**Explanation:** There are 2 contaminated regions. On the first day, add 5 walls to quarantine the viral region on the left. The board after the virus spreads is: ![](https://assets.leetcode.com/uploads/2021/06/01/virus12edited-grid.jpg) On the second day, add 5 walls to quarantine the viral region on the right. The virus is fully contained. ![](https://assets.leetcode.com/uploads/2021/06/01/virus13edited-grid.jpg)

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/01/virus2-grid.jpg)

**Input:** isInfected = \[\[1,1,1],[1,0,1],[1,1,1]]

**Output:** 4

**Explanation:** Even though there is only one cell saved, there are 4 walls built. Notice that walls are only built on the shared boundary of two different cells.

**Example 3:**

**Input:** isInfected = \[\[1,1,1,0,0,0,0,0,0],[1,0,1,0,1,1,1,1,1],[1,1,1,0,0,0,0,0,0]]

**Output:** 13

**Explanation:** The region on the left only builds two new walls.

**Constraints:**

*   `m == isInfected.length`
*   `n == isInfected[i].length`
*   `1 <= m, n <= 50`
*   `isInfected[i][j]` is either `0` or `1`.
*   There is always a contiguous viral region throughout the described process that will **infect strictly more uncontaminated squares** in the next round.

## Solution

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

@SuppressWarnings("java:S107")
public class Solution {
    private int m;
    private int n;
    private final int[][] dirs = { {-1, 0}, {1, 0}, {0, 1}, {0, -1}};

    public int containVirus(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        int res = 0;
        while (true) {
            int id = 0;
            Set<Integer> visited = new HashSet<>();
            Map<Integer, Set<Integer>> islands = new HashMap<>();
            Map<Integer, Set<Integer>> scores = new HashMap<>();
            Map<Integer, Integer> walls = new HashMap<>();
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (grid[i][j] == 1 && !visited.contains(i * n + j)) {
                        dfs(i, j, visited, grid, islands, scores, walls, id++);
                    }
                }
            }
            if (islands.isEmpty()) {
                break;
            }
            int maxVirus = 0;
            for (int i = 0; i < id; i++) {
                if (scores.getOrDefault(maxVirus, new HashSet<>()).size()
                        < scores.getOrDefault(i, new HashSet<>()).size()) {
                    maxVirus = i;
                }
            }
            res += walls.getOrDefault(maxVirus, 0);
            for (int i = 0; i < islands.size(); i++) {
                for (int island : islands.get(i)) {
                    int x = island / n;
                    int y = island % n;
                    if (i == maxVirus) {
                        grid[x][y] = -1;
                    } else {
                        for (int[] dir : dirs) {
                            int nx = x + dir[0];
                            int ny = y + dir[1];
                            if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] == 0) {
                                grid[nx][ny] = 1;
                            }
                        }
                    }
                }
            }
        }
        return res;
    }

    private void dfs(
            int i,
            int j,
            Set<Integer> visited,
            int[][] grid,
            Map<Integer, Set<Integer>> islands,
            Map<Integer, Set<Integer>> scores,
            Map<Integer, Integer> walls,
            int id) {
        if (!visited.add(i * n + j)) {
            return;
        }
        islands.computeIfAbsent(id, value -> new HashSet<>()).add(i * n + j);
        for (int[] dir : dirs) {
            int x = i + dir[0];
            int y = j + dir[1];
            if (x < 0 || x >= m || y < 0 || y >= n) {
                continue;
            }
            if (grid[x][y] == 1) {
                dfs(x, y, visited, grid, islands, scores, walls, id);
            }
            if (grid[x][y] == 0) {
                scores.computeIfAbsent(id, value -> new HashSet<>()).add(x * n + y);
                walls.put(id, walls.getOrDefault(id, 0) + 1);
            }
        }
    }
}
```