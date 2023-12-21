[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2850\. Minimum Moves to Spread Stones Over Grid

Medium

You are given a **0-indexed** 2D integer matrix `grid` of size `3 * 3`, representing the number of stones in each cell. The grid contains exactly `9` stones, and there can be **multiple** stones in a single cell.

In one move, you can move a single stone from its current cell to any other cell if the two cells share a side.

Return _the **minimum number of moves** required to place one stone in each cell_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/08/23/example1-3.svg)

**Input:** grid = \[\[1,1,0],[1,1,1],[1,2,1]]

**Output:** 3

**Explanation:** One possible sequence of moves to place one stone in each cell is: 

1- Move one stone from cell (2,1) to cell (2,2). 

2- Move one stone from cell (2,2) to cell (1,2). 

3- Move one stone from cell (1,2) to cell (0,2). 

In total, it takes 3 moves to place one stone in each cell of the grid. 

It can be shown that 3 is the minimum number of moves required to place one stone in each cell.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/08/23/example2-2.svg)

**Input:** grid = \[\[1,3,0],[1,0,0],[1,0,3]]

**Output:** 4

**Explanation:** One possible sequence of moves to place one stone in each cell is: 

1- Move one stone from cell (0,1) to cell (0,2). 

2- Move one stone from cell (0,1) to cell (1,1). 

3- Move one stone from cell (2,2) to cell (1,2). 

4- Move one stone from cell (2,2) to cell (2,1). 

In total, it takes 4 moves to place one stone in each cell of the grid. 

It can be shown that 4 is the minimum number of moves required to place one stone in each cell.

**Constraints:**

*   `grid.length == grid[i].length == 3`
*   `0 <= grid[i][j] <= 9`
*   Sum of `grid` is equal to `9`.

## Solution

```java
public class Solution {
    public int minimumMoves(int[][] grid) {
        int a = grid[0][0] - 1;
        int b = grid[0][1] - 1;
        int c = grid[0][2] - 1;
        int d = grid[1][0] - 1;
        int f = grid[1][2] - 1;
        int g = grid[2][0] - 1;
        int h = grid[2][1] - 1;
        int i = grid[2][2] - 1;
        int minCost = Integer.MAX_VALUE;
        for (int x = Math.min(a, 0); x <= Math.max(a, 0); x++) {
            for (int y = Math.min(c, 0); y <= Math.max(c, 0); y++) {
                for (int z = Math.min(i, 0); z <= Math.max(i, 0); z++) {
                    for (int t = Math.min(g, 0); t <= Math.max(g, 0); t++) {
                        int cost =
                                Math.abs(x)
                                        + Math.abs(y)
                                        + Math.abs(z)
                                        + Math.abs(t)
                                        + Math.abs(x - a)
                                        + Math.abs(y - c)
                                        + Math.abs(z - i)
                                        + Math.abs(t - g)
                                        + Math.abs(x - y + b + c)
                                        + Math.abs(y - z + i + f)
                                        + Math.abs(z - t + g + h)
                                        + Math.abs(t - x + a + d);
                        if (cost < minCost) {
                            minCost = cost;
                        }
                    }
                }
            }
        }
        return minCost;
    }
}
```