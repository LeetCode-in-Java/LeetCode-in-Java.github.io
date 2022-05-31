## 1210\. Minimum Moves to Reach Target with Rotations

Hard

In an `n*n` grid, there is a snake that spans 2 cells and starts moving from the top left corner at `(0, 0)` and `(0, 1)`. The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at `(n-1, n-2)` and `(n-1, n-1)`.

In one move the snake can:

*   Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
*   Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
*   Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from `(r, c)` and `(r, c+1)` to `(r, c)` and `(r+1, c)`.  
    ![](https://assets.leetcode.com/uploads/2019/09/24/image-2.png)
*   Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from `(r, c)` and `(r+1, c)` to `(r, c)` and `(r, c+1)`.  
    ![](https://assets.leetcode.com/uploads/2019/09/24/image-1.png)

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return `-1`.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/24/image.png)**

**Input:** 

    grid = [ [0,0,0,0,0,1], 
            [1,1,0,0,1,0], 
            [0,0,0,0,1,1], 
            [0,0,1,0,1,0], 
            [0,1,1,0,0,0], 
            [0,1,1,0,0,0]]

**Output:** 11

**Explanation:** One possible solution is [right, right, rotate clockwise, right, down, down, down, down, rotate counterclockwise, right, down].

**Example 2:**

**Input:** 

    grid = [ [0,0,1,1,1,1], 
            [0,0,0,0,1,1], 
            [1,1,0,0,0,1], 
            [1,1,1,0,0,1], 
            [1,1,1,0,0,1], 
            [1,1,1,0,0,0]]

**Output:** 9

**Constraints:**

*   `2 <= n <= 100`
*   `0 <= grid[i][j] <= 1`
*   It is guaranteed that the snake starts at empty cells.

## Solution

```java
import java.util.LinkedList;
import java.util.Objects;
import java.util.Queue;

public class Solution {
    public int minimumMoves(int[][] grid) {
        int n = grid.length;
        int[][] visited = new int[n][n];
        Queue<int[]> bq = new LinkedList<>();
        bq.offer(new int[] {0, 0, 1});
        visited[0][0] |= 1;
        int level = 0;
        while (!bq.isEmpty()) {
            int levelSize = bq.size();
            for (int l = 0; l < levelSize; l++) {
                int[] cur = bq.poll();
                int xtail = Objects.requireNonNull(cur)[0];
                int ytail = cur[1];
                int dir = cur[2];
                if (xtail == n - 1 && ytail == n - 2 && dir == 1) {
                    return level;
                }
                int xhead = xtail + (dir == 1 ? 0 : 1);
                int yhead = ytail + (dir == 1 ? 1 : 0);
                if (dir == 2) {
                    if (ytail + 1 < n
                            && grid[xtail][ytail + 1] != 1
                            && grid[xtail + 1][ytail + 1] != 1) {
                        if ((visited[xtail][ytail] & 1) == 0) {
                            bq.offer(new int[] {xtail, ytail, 1});
                            visited[xtail][ytail] |= 1;
                        }
                        if ((visited[xtail][ytail + 1] & 2) == 0) {
                            bq.offer(new int[] {xtail, ytail + 1, 2});
                            visited[xtail][ytail + 1] |= 2;
                        }
                    }
                    if (xhead + 1 < n
                            && grid[xhead + 1][yhead] != 1
                            && (visited[xhead][yhead] & 2) == 0) {
                        bq.offer(new int[] {xhead, yhead, 2});
                        visited[xhead][yhead] |= 2;
                    }
                } else {
                    if (xtail + 1 < n
                            && grid[xtail + 1][ytail] != 1
                            && grid[xtail + 1][ytail + 1] != 1) {
                        if ((visited[xtail][ytail] & 2) == 0) {
                            bq.offer(new int[] {xtail, ytail, 2});
                            visited[xtail][ytail] |= 2;
                        }
                        if ((visited[xtail + 1][ytail] & 1) == 0) {
                            bq.offer(new int[] {xtail + 1, ytail, 1});
                            visited[xtail + 1][ytail] |= 1;
                        }
                    }
                    if (yhead + 1 < n
                            && grid[xhead][yhead + 1] != 1
                            && (visited[xhead][yhead] & 1) == 0) {
                        bq.offer(new int[] {xhead, yhead, 1});
                        visited[xhead][yhead] |= 1;
                    }
                }
            }
            level += 1;
        }
        return -1;
    }
}
```