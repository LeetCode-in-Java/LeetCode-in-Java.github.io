[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 417\. Pacific Atlantic Water Flow

Medium

There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return _a **2D list** of grid coordinates_ `result` _where_ <code>result[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> _denotes that rain water can flow from cell_ <code>(r<sub>i</sub>, c<sub>i</sub>)</code> _to **both** the Pacific and Atlantic oceans_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)

**Input:** heights = \[\[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]

**Output:** [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]] 

**Example 2:**

**Input:** heights = \[\[2,1],[1,2]]

**Output:** [[0,0],[0,1],[1,0],[1,1]] 

**Constraints:**

*   `m == heights.length`
*   `n == heights[r].length`
*   `1 <= m, n <= 200`
*   <code>0 <= heights[r][c] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private int col = 0;
    private int row = 0;

    public List<List<Integer>> pacificAtlantic(int[][] matrix) {
        List<List<Integer>> res = new ArrayList<>();
        if (matrix.length == 0) {
            return res;
        }
        col = matrix.length;
        row = matrix[0].length;
        boolean[][] pacific = new boolean[col][row];
        boolean[][] atlantic = new boolean[col][row];
        for (int i = 0; i < col; i++) {
            dfs(i, 0, matrix, pacific);
            dfs(i, row - 1, matrix, atlantic);
        }
        for (int i = 0; i < row; i++) {
            dfs(0, i, matrix, pacific);
            dfs(col - 1, i, matrix, atlantic);
        }
        for (int i = 0; i < col; i++) {
            for (int j = 0; j < row; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    List<Integer> temp = new ArrayList<>();
                    temp.add(i);
                    temp.add(j);
                    res.add(temp);
                }
            }
        }
        return res;
    }

    private void dfs(int i, int j, int[][] matrix, boolean[][] visited) {
        if (i < 0 || j < 0 || i >= matrix.length || j >= matrix[0].length || visited[i][j]) {
            return;
        }
        visited[i][j] = true;
        if (i < col - 1 && matrix[i][j] <= matrix[i + 1][j]) {
            dfs(i + 1, j, matrix, visited);
        }
        if (i > 0 && matrix[i][j] <= matrix[i - 1][j]) {
            dfs(i - 1, j, matrix, visited);
        }
        if (j < row - 1 && matrix[i][j] <= matrix[i][j + 1]) {
            dfs(i, j + 1, matrix, visited);
        }
        if (j > 0 && matrix[i][j] <= matrix[i][j - 1]) {
            dfs(i, j - 1, matrix, visited);
        }
    }
}
```