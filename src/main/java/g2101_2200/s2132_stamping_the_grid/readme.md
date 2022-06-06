## 2132\. Stamping the Grid

Hard

You are given an `m x n` binary matrix `grid` where each cell is either `0` (empty) or `1` (occupied).

You are then given stamps of size `stampHeight x stampWidth`. We want to fit the stamps such that they follow the given **restrictions** and **requirements**:

1.  Cover all the **empty** cells.
2.  Do not cover any of the **occupied** cells.
3.  We can put as **many** stamps as we want.
4.  Stamps can **overlap** with each other.
5.  Stamps are not allowed to be **rotated**.
6.  Stamps must stay completely **inside** the grid.

Return `true` _if it is possible to fit the stamps while following the given restrictions and requirements. Otherwise, return_ `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/03/ex1.png)

**Input:** grid = \[\[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0]], stampHeight = 4, stampWidth = 3

**Output:** true

**Explanation:** We have two overlapping stamps (labeled 1 and 2 in the image) that are able to cover all the empty cells.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/03/ex2.png)

**Input:** grid = \[\[1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]], stampHeight = 2, stampWidth = 2

**Output:** false

**Explanation:** There is no way to fit the stamps onto all the empty cells without the stamps going outside the grid.

**Constraints:**

*   `m == grid.length`
*   `n == grid[r].length`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 2 * 10<sup>5</sup></code>
*   `grid[r][c]` is either `0` or `1`.
*   <code>1 <= stampHeight, stampWidth <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    private boolean canPaved(int[][] grid, int is, int js, int ie, int je) {
        for (int i = is; i <= ie; i++) {
            for (int j = js; j <= je; j++) {
                if (grid[i][j] == 1) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean possibleToStamp(int[][] grid, int h, int w) {
        int rl = grid[0].length;
        for (int i = 0; i < grid.length; i++) {
            int[] row = grid[i];
            int prev = -1;
            for (int j = 0; j < row.length; j++) {
                if (row[j] == 0) {
                    if (j + 1 < rl
                            && row[j + 1] == 1
                            && j - w + 1 >= 0
                            && i + 1 < grid.length
                            && grid[i + 1][j] == 1
                            && i - h + 1 >= 0
                            && canPaved(grid, i - h + 1, j - w + 1, i, j)) {
                        return false;
                    }
                    continue;
                }
                if (1 < j - prev && j - prev <= w) {
                    return false;
                }
                prev = j;
            }
            if (1 < row.length - prev && row.length - prev <= w) {
                return false;
            }
        }
        for (int i = 0; i < rl; i++) {
            int prev = -1;
            for (int j = 0; j < grid.length; j++) {
                if (grid[j][i] == 0) {
                    continue;
                }
                if (1 < j - prev && j - prev <= h) {
                    return false;
                }
                prev = j;
            }
            if (1 < grid.length - prev && grid.length - prev <= h) {
                return false;
            }
        }
        return true;
    }
}
```