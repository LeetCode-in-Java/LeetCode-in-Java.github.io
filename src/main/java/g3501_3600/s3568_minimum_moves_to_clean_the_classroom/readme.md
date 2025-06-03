[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3568\. Minimum Moves to Clean the Classroom

Medium

You are given an `m x n` grid `classroom` where a student volunteer is tasked with cleaning up litter scattered around the room. Each cell in the grid is one of the following:

*   `'S'`: Starting position of the student
*   `'L'`: Litter that must be collected (once collected, the cell becomes empty)
*   `'R'`: Reset area that restores the student's energy to full capacity, regardless of their current energy level (can be used multiple times)
*   `'X'`: Obstacle the student cannot pass through
*   `'.'`: Empty space

You are also given an integer `energy`, representing the student's maximum energy capacity. The student starts with this energy from the starting position `'S'`.

Each move to an adjacent cell (up, down, left, or right) costs 1 unit of energy. If the energy reaches 0, the student can only continue if they are on a reset area `'R'`, which resets the energy to its **maximum** capacity `energy`.

Return the **minimum** number of moves required to collect all litter items, or `-1` if it's impossible.

**Example 1:**

**Input:** classroom = ["S.", "XL"], energy = 2

**Output:** 2

**Explanation:**

*   The student starts at cell `(0, 0)` with 2 units of energy.
*   Since cell `(1, 0)` contains an obstacle 'X', the student cannot move directly downward.
*   A valid sequence of moves to collect all litter is as follows:
    *   Move 1: From `(0, 0)` → `(0, 1)` with 1 unit of energy and 1 unit remaining.
    *   Move 2: From `(0, 1)` → `(1, 1)` to collect the litter `'L'`.
*   The student collects all the litter using 2 moves. Thus, the output is 2.

**Example 2:**

**Input:** classroom = ["LS", "RL"], energy = 4

**Output:** 3

**Explanation:**

*   The student starts at cell `(0, 1)` with 4 units of energy.
*   A valid sequence of moves to collect all litter is as follows:
    *   Move 1: From `(0, 1)` → `(0, 0)` to collect the first litter `'L'` with 1 unit of energy used and 3 units remaining.
    *   Move 2: From `(0, 0)` → `(1, 0)` to `'R'` to reset and restore energy back to 4.
    *   Move 3: From `(1, 0)` → `(1, 1)` to collect the second litter `'L'`.
*   The student collects all the litter using 3 moves. Thus, the output is 3.

**Example 3:**

**Input:** classroom = ["L.S", "RXL"], energy = 3

**Output:** \-1

**Explanation:**

No valid path collects all `'L'`.

**Constraints:**

*   `1 <= m == classroom.length <= 20`
*   `1 <= n == classroom[i].length <= 20`
*   `classroom[i][j]` is one of `'S'`, `'L'`, `'R'`, `'X'`, or `'.'`
*   `1 <= energy <= 50`
*   There is exactly **one** `'S'` in the grid.
*   There are **at most** 10 `'L'` cells in the grid.

## Solution

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Queue;

@SuppressWarnings({"java:S135", "java:S6541"})
public class Solution {
    private static class State {
        int x;
        int y;
        int energy;
        int mask;
        int steps;

        State(int x, int y, int energy, int mask, int steps) {
            this.x = x;
            this.y = y;
            this.energy = energy;
            this.mask = mask;
            this.steps = steps;
        }
    }

    public int minMoves(String[] classroom, int energy) {
        int m = classroom.length;
        int n = classroom[0].length();
        char[][] grid = new char[m][n];
        for (int i = 0; i < m; i++) {
            grid[i] = classroom[i].toCharArray();
        }
        int startX = -1;
        int startY = -1;
        List<int[]> lumetarkon = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                char c = grid[i][j];
                if (c == 'S') {
                    startX = i;
                    startY = j;
                } else if (c == 'L') {
                    lumetarkon.add(new int[] {i, j});
                }
            }
        }
        int totalLitter = lumetarkon.size();
        int allMask = (1 << totalLitter) - 1;
        int[][][] visited = new int[m][n][1 << totalLitter];
        for (int[][] layer : visited) {
            for (int[] row : layer) {
                Arrays.fill(row, -1);
            }
        }
        Queue<State> queue = new ArrayDeque<>();
        queue.offer(new State(startX, startY, energy, 0, 0));
        visited[startX][startY][0] = energy;
        int[][] dirs = { {0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        while (!queue.isEmpty()) {
            State curr = queue.poll();
            if (curr.mask == allMask) {
                return curr.steps;
            }
            for (int[] dir : dirs) {
                int nx = curr.x + dir[0];
                int ny = curr.y + dir[1];
                if (nx < 0 || ny < 0 || nx >= m || ny >= n || grid[nx][ny] == 'X') {
                    continue;
                }
                int nextEnergy = curr.energy - 1;
                if (nextEnergy < 0) {
                    continue;
                }
                char cell = grid[nx][ny];
                if (cell == 'R') {
                    nextEnergy = energy;
                }
                int nextMask = curr.mask;
                if (cell == 'L') {
                    for (int i = 0; i < lumetarkon.size(); i++) {
                        int[] pos = lumetarkon.get(i);
                        if (pos[0] == nx && pos[1] == ny) {
                            nextMask |= (1 << i);
                            break;
                        }
                    }
                }
                if (visited[nx][ny][nextMask] < nextEnergy) {
                    visited[nx][ny][nextMask] = nextEnergy;
                    queue.offer(new State(nx, ny, nextEnergy, nextMask, curr.steps + 1));
                }
            }
        }
        return -1;
    }
}
```