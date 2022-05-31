## 329\. Longest Increasing Path in a Matrix

Hard

Given an `m x n` integers `matrix`, return _the length of the longest increasing path in_ `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

**Input:** matrix = \[\[9,9,4],[6,6,8],[2,1,1]]

**Output:** 4

**Explanation:** The longest increasing path is `[1, 2, 6, 9]`. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

**Input:** matrix = \[\[3,4,5],[3,2,6],[2,2,1]]

**Output:** 4

**Explanation:** The longest increasing path is `[3, 4, 5, 6]`. Moving diagonally is not allowed. 

**Example 3:**

**Input:** matrix = \[\[1]]

**Output:** 1 

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 200`
*   <code>0 <= matrix[i][j] <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        int maxIncreasingSequenceCount = 0;
        int n = matrix.length - 1;
        int m = matrix[0].length - 1;
        int[][] memory = new int[n + 1][m + 1];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                maxIncreasingSequenceCount =
                        Math.max(maxIncreasingSequenceCount, move(i, j, n, m, matrix, memory));
            }
        }
        return maxIncreasingSequenceCount;
    }

    private int move(int row, int col, int n, int m, int[][] matrix, int[][] memory) {
        if (memory[row][col] == 0) {
            int count = 1;
            // move down
            if (row < n && matrix[row + 1][col] > matrix[row][col]) {
                count = Math.max(count, move(row + 1, col, n, m, matrix, memory) + 1);
            }
            // move right
            if (col < m && matrix[row][col + 1] > matrix[row][col]) {
                count = Math.max(count, move(row, col + 1, n, m, matrix, memory) + 1);
            }
            // move up
            if (row > 0 && matrix[row - 1][col] > matrix[row][col]) {
                count = Math.max(count, move(row - 1, col, n, m, matrix, memory) + 1);
            }
            // move left
            if (col > 0 && matrix[row][col - 1] > matrix[row][col]) {
                count = Math.max(count, move(row, col - 1, n, m, matrix, memory) + 1);
            }
            memory[row][col] = count;
        }
        return memory[row][col];
    }
}
```