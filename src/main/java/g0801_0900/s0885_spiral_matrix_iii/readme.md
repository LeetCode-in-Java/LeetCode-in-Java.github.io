[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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
        int localX = x;
        int localY = y;
        int[] i = {0};
        int[] moves = {0};
        int[][] result = new int[rows * cols][2];
        result[0][0] = y;
        result[0][1] = x;
        i[0]++;
        while (i[0] < result.length) {
            moves[0]++;
            // Move right
            localX = getXRight(rows, cols, localX, localY, i, moves, result);
            if (i[0] >= result.length) {
                break;
            }
            // Move down
            localY = getYDown(rows, cols, localX, localY, i, moves, result);
            if (i[0] >= result.length) {
                break;
            }
            moves[0]++;
            // Move left
            localX = getXLeft(rows, cols, localX, localY, i, moves, result);
            if (i[0] >= result.length) {
                break;
            }
            // Move up
            localY = getYUp(rows, cols, localX, localY, i, moves, result);
        }
        return result;
    }

    private int getYUp(
            int rows, int cols, int localX, int localY, int[] i, int[] moves, int[][] result) {
        if (localX < 0 || localX >= cols) {
            localY -= moves[0];
        } else {
            for (int j = 0; j < moves[0]; j++) {
                localY--;
                if (localY >= 0 && localY < rows) {
                    result[i[0]][0] = localY;
                    result[i[0]][1] = localX;
                    i[0]++;
                }
            }
        }
        return localY;
    }

    private int getXLeft(
            int rows, int cols, int localX, int localY, int[] i, int[] moves, int[][] result) {
        if (localY < 0 || localY >= rows) {
            localX -= moves[0];
        } else {
            for (int j = 0; j < moves[0]; j++) {
                localX--;
                if (localX >= 0 && localX < cols) {
                    result[i[0]][0] = localY;
                    result[i[0]][1] = localX;
                    i[0]++;
                }
            }
        }
        return localX;
    }

    private int getYDown(
            int rows, int cols, int localX, int localY, int[] i, int[] moves, int[][] result) {
        if (localX < 0 || localX >= cols) {
            localY += moves[0];
        } else {
            for (int j = 0; j < moves[0]; j++) {
                localY++;
                if (localY >= 0 && localY < rows) {
                    result[i[0]][0] = localY;
                    result[i[0]][1] = localX;
                    i[0]++;
                }
            }
        }
        return localY;
    }

    private int getXRight(
            int rows, int cols, int localX, int localY, int[] i, int[] moves, int[][] result) {
        if (localY < 0 || localY >= rows) {
            localX += moves[0];
        } else {
            for (int j = 0; j < moves[0]; j++) {
                localX++;
                if (localX >= 0 && localX < cols) {
                    result[i[0]][0] = localY;
                    result[i[0]][1] = localX;
                    i[0]++;
                }
            }
        }
        return localX;
    }
}
```