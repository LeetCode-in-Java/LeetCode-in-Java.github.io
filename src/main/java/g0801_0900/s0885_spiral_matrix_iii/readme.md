[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 885\. Spiral Matrix III

Medium

You start at the cell `(rStart, cStart)` of an `rows x cols` grid facing east. The northwest corner is at the first row and column in the grid, and the southeast corner is at the last row and column.

You will walk in a clockwise spiral shape to visit every position in this grid. Whenever you move outside the grid's boundary, we continue our walk outside the grid (but may return to the grid boundary later.). Eventually, we reach all `rows * cols` spaces of the grid.

Return _an array of coordinates representing the positions of the grid in the order you visited them_.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_1.png)

**Input:** rows = 1, cols = 4, rStart = 0, cStart = 0

**Output:** [[0,0],[0,1],[0,2],[0,3]] 

**Example 2:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/24/example_2.png)

**Input:** rows = 5, cols = 6, rStart = 1, cStart = 4

**Output:** [[1,4],[1,5],[2,5],[2,4],[2,3],[1,3],[0,3],[0,4],[0,5],[3,5],[3,4],[3,3],[3,2],[2,2],[1,2],[0,2],[4,5],[4,4],[4,3],[4,2],[4,1],[3,1],[2,1],[1,1],[0,1],[4,0],[3,0],[2,0],[1,0],[0,0]] 

**Constraints:**

*   `1 <= rows, cols <= 100`
*   `0 <= rStart < rows`
*   `0 <= cStart < cols`

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int[][] spiralMatrixIII(int rows, int cols, int y, int x) {
        int j;
        int i = 0;
        int moves = 0;
        int[][] result = new int[rows * cols][2];
        result[0][0] = y;
        result[0][1] = x;
        i++;
        while (i < result.length) {
            moves++;
            // Move right
            if (y < 0 || y >= rows) {
                x += moves;
            } else {
                for (j = 0; j < moves; j++) {
                    x++;
                    if (x >= 0 && x < cols && y >= 0 && y < rows) {
                        result[i][0] = y;
                        result[i][1] = x;
                        i++;
                    }
                }
            }
            if (i >= result.length) {
                break;
            }
            // Move down
            if (x < 0 || x >= cols) {
                y += moves;
            } else {
                for (j = 0; j < moves; j++) {
                    y++;
                    if (x >= 0 && x < cols && y >= 0 && y < rows) {
                        result[i][0] = y;
                        result[i][1] = x;
                        i++;
                    }
                }
            }
            if (i >= result.length) {
                break;
            }
            moves++;
            // Move left
            if (y < 0 || y >= rows) {
                x -= moves;
            } else {
                for (j = 0; j < moves; j++) {
                    x--;
                    if (x >= 0 && x < cols && y >= 0 && y < rows) {
                        result[i][0] = y;
                        result[i][1] = x;
                        i++;
                    }
                }
            }
            if (i >= result.length) {
                break;
            }
            // Move up
            if (x < 0 || x >= cols) {
                y -= moves;
            } else {
                for (j = 0; j < moves; j++) {
                    y--;
                    if (x >= 0 && x < cols && y >= 0 && y < rows) {
                        result[i][0] = y;
                        result[i][1] = x;
                        i++;
                    }
                }
            }
        }
        return result;
    }
}
```