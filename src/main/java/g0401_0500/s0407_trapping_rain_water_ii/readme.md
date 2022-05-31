## 407\. Trapping Rain Water II

Hard

Given an `m x n` integer matrix `heightMap` representing the height of each unit cell in a 2D elevation map, return _the volume of water it can trap after raining_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg)

**Input:** heightMap = \[\[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]

**Output:** 4

**Explanation:**

    After the rain, water is trapped between the blocks.
    We have two small ponds 1 and 3 units trapped.
    The total volume of water trapped is 4. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/08/trap2-3d.jpg)

**Input:** heightMap = \[\[3,3,3,3,3],[3,2,2,2,3],[3,2,1,2,3],[3,2,2,2,3],[3,3,3,3,3]]

**Output:** 10 

**Constraints:**

*   `m == heightMap.length`
*   `n == heightMap[i].length`
*   `1 <= m, n <= 200`
*   <code>0 <= heightMap[i][j] <= 2 * 10<sup>4</sup></code>

## Solution

```java
import java.util.Objects;
import java.util.PriorityQueue;

public class Solution {
    private static class Cell implements Comparable<Cell> {
        private int row;
        private int col;
        private int value;

        public Cell(int r, int c, int v) {
            this.row = r;
            this.col = c;
            this.value = v;
        }

        @Override
        public int compareTo(Cell other) {
            return value - other.value;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) {
                return true;
            }
            if (o == null || getClass() != o.getClass()) {
                return false;
            }
            Cell cell = (Cell) o;
            return row == cell.row && col == cell.col && value == cell.value;
        }

        @Override
        public int hashCode() {
            return Objects.hash(row, col, value);
        }
    }

    private int water;
    private boolean[][] visited1;

    public int trapRainWater(int[][] heightMap) {
        if (heightMap.length == 0) {
            return 0;
        }
        PriorityQueue<Cell> walls = new PriorityQueue<>();
        water = 0;
        visited1 = new boolean[heightMap.length][heightMap[0].length];
        int rows = heightMap.length;
        int cols = heightMap[0].length;
        // build wall
        for (int c = 0; c < cols; c++) {
            walls.add(new Cell(0, c, heightMap[0][c]));
            walls.add(new Cell(rows - 1, c, heightMap[rows - 1][c]));
            visited1[0][c] = true;
            visited1[rows - 1][c] = true;
        }
        for (int r = 1; r < rows - 1; r++) {
            walls.add(new Cell(r, 0, heightMap[r][0]));
            walls.add(new Cell(r, cols - 1, heightMap[r][cols - 1]));
            visited1[r][0] = true;
            visited1[r][cols - 1] = true;
        }
        // end build wall
        while (!walls.isEmpty()) {
            Cell min = walls.poll();
            visit(heightMap, min, walls);
        }
        return water;
    }

    private void visit(int[][] height, Cell start, PriorityQueue<Cell> walls) {
        fill(height, start.row + 1, start.col, walls, start.value);
        fill(height, start.row - 1, start.col, walls, start.value);
        fill(height, start.row, start.col + 1, walls, start.value);
        fill(height, start.row, start.col - 1, walls, start.value);
    }

    private void fill(int[][] height, int row, int col, PriorityQueue<Cell> walls, int min) {
        if (((row >= 0 && col >= 0) && (row < height.length && col < height[0].length))
                && !visited1[row][col]) {
            if (height[row][col] >= min) {
                walls.add(new Cell(row, col, height[row][col]));
                visited1[row][col] = true;
            } else {
                water += min - height[row][col];
                visited1[row][col] = true;
                fill(height, row + 1, col, walls, min);
                fill(height, row - 1, col, walls, min);
                fill(height, row, col + 1, walls, min);
                fill(height, row, col - 1, walls, min);
            }
        }
    }
}
```