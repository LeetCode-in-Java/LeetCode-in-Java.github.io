## 994\. Rotting Oranges

Medium

You are given an `m x n` `grid` where each cell can have one of three values:

*   `0` representing an empty cell,
*   `1` representing a fresh orange, or
*   `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

**Input:** grid = [[2,1,1],[1,1,0],[0,1,1]]

**Output:** 4

**Example 2:**

**Input:** grid = [[2,1,1],[0,1,1],[1,0,1]]

**Output:** -1

**Explanation:** The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

**Example 3:**

**Input:** grid = [[0,2]]

**Output:** 0

**Explanation:** Since there are already no fresh oranges at minute 0, the answer is just 0.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 10`
*   `grid[i][j]` is `0`, `1`, or `2`.

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int orangesRotting(int[][] grid) {
        Queue<int[]> queue = new LinkedList<>();
        int row = grid.length;
        int col = grid[0].length;
        int countActive = 0;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 2) {
                    queue.add(new int[] {i, j});
                }
                if (grid[i][j] != 0) {
                    countActive++;
                }
            }
        }
        if (countActive == 0) {
            return 0;
        }
        int countCurrent = 0;
        int count = 0;
        int[] dx = {0, 0, 1, -1};
        int[] dy = {1, -1, 0, 0};
        while (!queue.isEmpty()) {
            int size = queue.size();
            count += size;
            for (int i = 0; i < size; i++) {
                int[] arr = queue.poll();
                for (int j = 0; j < 4; j++) {
                    int x = arr[0] + dx[j];
                    int y = arr[1] + dy[j];
                    if (x < 0 || y < 0 || x >= row || y >= col || grid[x][y] != 1) {
                        continue;
                    }
                    grid[x][y] = 2;
                    queue.add(new int[] {x, y});
                }
            }
            if (!queue.isEmpty()) {
                countCurrent++;
            }
        }
        return countActive == count ? countCurrent : -1;
    }
}
```