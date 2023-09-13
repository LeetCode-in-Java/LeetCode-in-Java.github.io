[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 200\. Number of Islands

Medium

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:**

    grid = [
      ["1","1","1","1","0"],
      ["1","1","0","1","0"],
      ["1","1","0","0","0"],
      ["0","0","0","0","0"]
    ]

**Output:** 1 

**Example 2:**

**Input:**

    grid = [
      ["1","1","0","0","0"],
      ["1","1","0","0","0"],
      ["0","0","1","0","0"],
      ["0","0","0","1","1"]
    ]

**Output:** 3 

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 300`
*   `grid[i][j]` is `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public int numIslands(char[][] grid) {
        int islands = 0;
        if (grid != null && grid.length != 0 && grid[0].length != 0) {
            for (int i = 0; i < grid.length; i++) {
                for (int j = 0; j < grid[0].length; j++) {
                    if (grid[i][j] == '1') {
                        dfs(grid, i, j);
                        islands++;
                    }
                }
            }
        }
        return islands;
    }

    private void dfs(char[][] grid, int x, int y) {
        if (x < 0 || grid.length <= x || y < 0 || grid[0].length <= y || grid[x][y] != '1') {
            return;
        }
        grid[x][y] = 'x';
        dfs(grid, x + 1, y);
        dfs(grid, x - 1, y);
        dfs(grid, x, y + 1);
        dfs(grid, x, y - 1);
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(M * N), where M is the number of rows and N is the number of columns in the grid. Here's why:

1. The program uses a nested loop to iterate through each cell of the 2D grid, where the outer loop runs from 0 to the number of rows (M) and the inner loop runs from 0 to the number of columns (N). This results in a total of M * N iterations.

2. For each cell, the program performs a depth-first search (DFS) operation, which may recursively visit adjacent cells if they are part of the same island. The DFS operation has constant time complexity for each cell since it visits each cell once.

3. Therefore, the overall time complexity is O(M * N) since the program performs DFS for each cell in the grid.

**Space Complexity (Big O Space):**

The space complexity of this program is O(1) in terms of auxiliary space (additional data structures) and O(M * N) in terms of stack space due to recursive function calls. Here's why:

1. The program uses a constant amount of auxiliary space for variables like `islands`, `x`, and `y`. These variables do not depend on the size of the input grid and can be considered O(1).

2. The recursive DFS calls result in stack space usage proportional to the depth of the recursion, which is determined by the size of the island being explored. In the worst case, if the entire grid is one large island, the stack space can be as large as M * N.

3. Overall, the space complexity is dominated by the stack space used during DFS, and it can be represented as O(M * N) in the worst case.

In summary, the time complexity is O(M * N) because the program iterates through each cell in the grid, and the space complexity is O(1) for auxiliary space and O(M * N) for stack space used during recursive DFS calls.
