[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2257\. Count Unguarded Cells in the Grid

Medium

You are given two integers `m` and `n` representing a **0-indexed** `m x n` grid. You are also given two 2D integer arrays `guards` and `walls` where <code>guards[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> and <code>walls[j] = [row<sub>j</sub>, col<sub>j</sub>]</code> represent the positions of the <code>i<sup>th</sup></code> guard and <code>j<sup>th</sup></code> wall respectively.

A guard can see **every** cell in the four cardinal directions (north, east, south, or west) starting from their position unless **obstructed** by a wall or another guard. A cell is **guarded** if there is **at least** one guard that can see it.

Return _the number of unoccupied cells that are **not** **guarded**._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/10/example1drawio2.png)

**Input:** m = 4, n = 6, guards = \[\[0,0],[1,1],[2,3]], walls = \[\[0,1],[2,2],[1,4]]

**Output:** 7

**Explanation:** The guarded and unguarded cells are shown in red and green respectively in the above diagram. There are a total of 7 unguarded cells, so we return 7.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/10/example2drawio.png)

**Input:** m = 3, n = 3, guards = \[\[1,1]], walls = \[\[0,1],[1,0],[2,1],[1,2]]

**Output:** 4

**Explanation:** The unguarded cells are shown in green in the above diagram. There are a total of 4 unguarded cells, so we return 4.

**Constraints:**

*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>2 <= m * n <= 10<sup>5</sup></code>
*   <code>1 <= guards.length, walls.length <= 5 * 10<sup>4</sup></code>
*   `2 <= guards.length + walls.length <= m * n`
*   `guards[i].length == walls[j].length == 2`
*   <code>0 <= row<sub>i</sub>, row<sub>j</sub> < m</code>
*   <code>0 <= col<sub>i</sub>, col<sub>j</sub> < n</code>
*   All the positions in `guards` and `walls` are **unique**.

## Solution

```java
public class Solution {
    public int countUnguarded(int m, int n, int[][] guards, int[][] walls) {
        char[][] matrix = new char[m][n];
        int result = 0;
        for (int i = 0; i <= guards.length - 1; i++) {
            int guardRow = guards[i][0];
            int guardCol = guards[i][1];
            matrix[guardRow][guardCol] = 'G';
        }
        for (int i = 0; i <= walls.length - 1; i++) {
            int wallRow = walls[i][0];
            int wallCol = walls[i][1];
            matrix[wallRow][wallCol] = 'W';
        }
        for (int i = 0; i <= guards.length - 1; i++) {
            int currentRow = guards[i][0];
            int currentCol = guards[i][1] - 1;
            extracted(matrix, currentRow, currentCol);
            currentRow = guards[i][0];
            currentCol = guards[i][1] + 1;
            extracted(n, matrix, currentRow, currentCol);
            currentRow = guards[i][0] - 1;
            currentCol = guards[i][1];
            extracted1(matrix, currentRow, currentCol);
            currentRow = guards[i][0] + 1;
            currentCol = guards[i][1];
            extracted1(m, matrix, currentRow, currentCol);
        }
        for (int i = 0; i <= m - 1; i++) {
            for (int j = 0; j <= n - 1; j++) {
                if (matrix[i][j] != 'R' && matrix[i][j] != 'G' && matrix[i][j] != 'W') {
                    result++;
                }
            }
        }
        return result;
    }

    private void extracted1(int m, char[][] matrix, int currentRow, int currentCol) {
        while (currentRow <= m - 1) {
            if (matrix[currentRow][currentCol] != 'W' && matrix[currentRow][currentCol] != 'G') {
                matrix[currentRow][currentCol] = 'R';
            } else {
                break;
            }
            currentRow++;
        }
    }

    private void extracted1(char[][] matrix, int currentRow, int currentCol) {
        while (currentRow >= 0) {
            if (matrix[currentRow][currentCol] != 'W' && matrix[currentRow][currentCol] != 'G') {
                matrix[currentRow][currentCol] = 'R';
            } else {
                break;
            }
            currentRow--;
        }
    }

    private void extracted(int n, char[][] matrix, int currentRow, int currentCol) {
        while (currentCol <= n - 1) {
            if (matrix[currentRow][currentCol] != 'W' && matrix[currentRow][currentCol] != 'G') {
                matrix[currentRow][currentCol] = 'R';
            } else {
                break;
            }
            currentCol++;
        }
    }

    private void extracted(char[][] matrix, int currentRow, int currentCol) {
        while (currentCol >= 0) {
            if (matrix[currentRow][currentCol] != 'W' && matrix[currentRow][currentCol] != 'G') {
                matrix[currentRow][currentCol] = 'R';
            } else {
                break;
            }
            currentCol--;
        }
    }
}
```