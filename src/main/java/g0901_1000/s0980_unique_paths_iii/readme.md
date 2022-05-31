## 980\. Unique Paths III

Hard

You are given an `m x n` integer array `grid` where `grid[i][j]` could be:

*   `1` representing the starting square. There is exactly one starting square.
*   `2` representing the ending square. There is exactly one ending square.
*   `0` representing empty squares we can walk over.
*   `-1` representing obstacles that we cannot walk over.

Return _the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square exactly once_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique1.jpg)

**Input:** grid = \[\[1,0,0,0],[0,0,0,0],[0,0,2,-1]]

**Output:** 2

**Explanation:** We have the following two paths:

1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)

2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique2.jpg)

**Input:** grid = \[\[1,0,0,0],[0,0,0,0],[0,0,0,2]]

**Output:** 4

**Explanation:** We have the following four paths:

1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)

2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)

3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)

4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/08/02/lc-unique3-.jpg)

**Input:** grid = \[\[0,1],[2,0]]

**Output:** 0

**Explanation:** There is no path that walks over every empty square exactly once.

Note that the starting and ending square can be anywhere in the grid.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 20`
*   `1 <= m * n <= 20`
*   `-1 <= grid[i][j] <= 2`
*   There is exactly one starting cell and one ending cell.

## Solution

```java
public class Solution {
    private final int[] row = {0, 0, 1, -1};
    private final int[] col = {1, -1, 0, 0};

    private int isSafe(int[][] grid, int rows, int cols, int i, int j) {
        if (i < 0 || j < 0 || i >= rows || j >= cols || grid[i][j] == -1) {
            return 0;
        }
        if (grid[i][j] == 2) {
            for (int l = 0; l < rows; l++) {
                for (int m = 0; m < cols; m++) {
                    if (grid[l][m] == 0) {
                        /* Return 0 if all zeros in the path are not covered */
                        return 0;
                    }
                }
            }
            /* Return 1, as we covered all zeros in the path */
            return 1;
        }
        /* mark as visited */
        grid[i][j] = -1;
        int result = 0;
        for (int k = 0; k < 4; k++) {
            /* travel in all four directions (up,down,right,left) */
            result = result + isSafe(grid, rows, cols, (i + row[k]), (j + col[k]));
        }
        /* Mark unvisited again to backtrack */
        grid[i][j] = 0;
        return result;
    }

    public int uniquePathsIII(int[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;
        int result = 0;
        for (int k = 0; k < rows; k++) {
            for (int m = 0; m < cols; m++) {
                if (grid[k][m] == 1) {
                    /* find indexes where 1 is located and start covering paths */
                    result = isSafe(grid, rows, cols, k, m);
                    break;
                }
            }
        }
        return result;
    }
}
```