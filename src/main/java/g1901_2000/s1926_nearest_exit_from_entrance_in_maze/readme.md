[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1926\. Nearest Exit from Entrance in Maze

Medium

You are given an `m x n` matrix `maze` (**0-indexed**) with empty cells (represented as `'.'`) and walls (represented as `'+'`). You are also given the `entrance` of the maze, where <code>entrance = [entrance<sub>row</sub>, entrance<sub>col</sub>]</code> denotes the row and column of the cell you are initially standing at.

In one step, you can move one cell **up**, **down**, **left**, or **right**. You cannot step into a cell with a wall, and you cannot step outside the maze. Your goal is to find the **nearest exit** from the `entrance`. An **exit** is defined as an **empty cell** that is at the **border** of the `maze`. The `entrance` **does not count** as an exit.

Return _the **number of steps** in the shortest path from the_ `entrance` _to the nearest exit, or_ `-1` _if no such path exists_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearest1-grid.jpg)

**Input:** maze = \[\["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]

**Output:** 1

**Explanation:** 

There are 3 exits in this maze at [1,0], [0,2], and [2,3]. Initially, you are at the entrance cell [1,2]. 

- You can reach [1,0] by moving 2 steps left. 

- You can reach [0,2] by moving 1 step up. 
  
It is impossible to reach [2,3] from the entrance. Thus, the nearest exit is [0,2], which is 1 step away.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearesr2-grid.jpg)

**Input:** maze = \[\["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]

**Output:** 2

**Explanation:** 

There is 1 exit in this maze at [1,2]. [1,0] does not count as an exit since it is the entrance cell. Initially, you are at the entrance cell [1,0]. 

- You can reach [1,2] by moving 2 steps right. 
  
Thus, the nearest exit is [1,2], which is 2 steps away.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/06/04/nearest3-grid.jpg)

**Input:** maze = \[\[".","+"]], entrance = [0,0]

**Output:** -1

**Explanation:** There are no exits in this maze.

**Constraints:**

*   `maze.length == m`
*   `maze[i].length == n`
*   `1 <= m, n <= 100`
*   `maze[i][j]` is either `'.'` or `'+'`.
*   `entrance.length == 2`
*   <code>0 <= entrance<sub>row</sub> < m</code>
*   <code>0 <= entrance<sub>col</sub> < n</code>
*   `entrance` will always be an empty cell.

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int nearestExit(char[][] maze, int[] entrance) {
        int m = maze.length;
        int n = maze[0].length;
        int[] directions = new int[] {0, 1, 0, -1, 0};
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] {entrance[0], entrance[1], 0});
        boolean[][] visited = new boolean[m][n];
        visited[entrance[0]][entrance[1]] = true;
        int shortestSteps = m * n;
        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            for (int i = 0; i < directions.length - 1; i++) {
                int nextX = curr[0] + directions[i];
                int nextY = curr[1] + directions[i + 1];
                if (nextX >= 0
                        && nextX < m
                        && nextY >= 0
                        && nextY < n
                        && maze[nextX][nextY] == '.'
                        && !visited[nextX][nextY]) {
                    visited[nextX][nextY] = true;
                    if (nextX == 0 || nextX == m - 1 || nextY == 0 || nextY == n - 1) {
                        shortestSteps = Math.min(shortestSteps, curr[2] + 1);
                    } else {
                        queue.offer(new int[] {nextX, nextY, curr[2] + 1});
                    }
                }
            }
        }
        return shortestSteps == m * n ? -1 : shortestSteps;
    }
}
```