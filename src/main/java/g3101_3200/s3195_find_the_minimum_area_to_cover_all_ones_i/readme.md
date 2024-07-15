[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3195\. Find the Minimum Area to Cover All Ones I

Medium

You are given a 2D **binary** array `grid`. Find a rectangle with horizontal and vertical sides with the **smallest** area, such that all the 1's in `grid` lie inside this rectangle.

Return the **minimum** possible area of the rectangle.

**Example 1:**

**Input:** grid = \[\[0,1,0],[1,0,1]]

**Output:** 6

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/05/08/examplerect0.png)

The smallest rectangle has a height of 2 and a width of 3, so it has an area of `2 * 3 = 6`.

**Example 2:**

**Input:** grid = \[\[1,0],[0,0]]

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/05/08/examplerect1.png)

The smallest rectangle has both height and width 1, so its area is `1 * 1 = 1`.

**Constraints:**

*   `1 <= grid.length, grid[i].length <= 1000`
*   `grid[i][j]` is either 0 or 1.
*   The input is generated such that there is at least one 1 in `grid`.

## Solution

```java
public class Solution {
    public int minimumArea(int[][] grid) {
        int xmin = Integer.MAX_VALUE;
        int xmax = -1;
        int ymin = Integer.MAX_VALUE;
        int ymax = -1;
        for (int i = 0, m = grid.length, n = grid[0].length; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    xmin = Math.min(xmin, i);
                    xmax = Math.max(xmax, i);
                    ymin = Math.min(ymin, j);
                    ymax = Math.max(ymax, j);
                }
            }
        }
        return (xmax - xmin + 1) * (ymax - ymin + 1);
    }
}
```