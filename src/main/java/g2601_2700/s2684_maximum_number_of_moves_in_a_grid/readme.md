[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2684\. Maximum Number of Moves in a Grid

Medium

You are given a **0-indexed** `m x n` matrix `grid` consisting of **positive** integers.

You can start at **any** cell in the first column of the matrix, and traverse the grid in the following way:

*   From a cell `(row, col)`, you can move to any of the cells: `(row - 1, col + 1)`, `(row, col + 1)` and `(row + 1, col + 1)` such that the value of the cell you move to, should be **strictly** bigger than the value of the current cell.

Return _the **maximum** number of **moves** that you can perform._

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/04/11/yetgriddrawio-10.png)

**Input:** grid = \[\[2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15]]

**Output:** 3

**Explanation:** We can start at the cell (0, 0) and make the following moves: 
- (0, 0) -> (0, 1). 
- (0, 1) -> (1, 2). 
- (1, 2) -> (2, 3). 

It can be shown that it is the maximum number of moves that can be made.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/04/12/yetgrid4drawio.png) **Input:** grid = \[\[3,2,4],[2,1,9],[1,1,7]]

**Output:** 0

**Explanation:** Starting from any cell in the first column we cannot perform any moves.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `2 <= m, n <= 1000`
*   <code>4 <= m * n <= 10<sup>5</sup></code>
*   <code>1 <= grid[i][j] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxMoves(int[][] grid) {
        int h = grid.length;
        boolean[] dp1 = new boolean[h];
        boolean[] dp2 = new boolean[h];
        int rtn = 0;
        Arrays.fill(dp1, true);
        for (int col = 1; col < grid[0].length; col++) {
            boolean f = false;
            for (int row = 0; row < h; row++) {
                int pr = row - 1;
                int nr = row + 1;
                dp2[row] = false;
                if (pr >= 0 && dp1[pr] && grid[pr][col - 1] < grid[row][col]) {
                    dp2[row] = true;
                    f = true;
                }
                if (nr < h && dp1[nr] && grid[nr][col - 1] < grid[row][col]) {
                    dp2[row] = true;
                    f = true;
                }
                if (dp1[row] && grid[row][col - 1] < grid[row][col]) {
                    dp2[row] = true;
                    f = true;
                }
            }
            boolean[] t = dp1;
            dp1 = dp2;
            dp2 = t;
            if (f) {
                rtn++;
            } else {
                break;
            }
        }
        return rtn;
    }
}
```