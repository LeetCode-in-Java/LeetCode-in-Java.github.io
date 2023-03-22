[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2503\. Maximum Number of Points From Grid Queries

Hard

You are given an `m x n` integer matrix `grid` and an array `queries` of size `k`.

Find an array `answer` of size `k` such that for each integer `queries[i]` you start in the **top left** cell of the matrix and repeat the following process:

*   If `queries[i]` is **strictly** greater than the value of the current cell that you are in, then you get one point if it is your first time visiting this cell, and you can move to any **adjacent** cell in all `4` directions: up, down, left, and right.
*   Otherwise, you do not get any points, and you end this process.

After the process, `answer[i]` is the **maximum** number of points you can get. **Note** that for each query you are allowed to visit the same cell **multiple** times.

Return _the resulting array_ `answer`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/10/19/yetgriddrawio.png)

**Input:** grid = \[\[1,2,3],[2,5,7],[3,5,1]], queries = [5,6,2]

**Output:** [5,8,1]

**Explanation:** The diagrams above show which cells we visit to get points for each query.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/10/20/yetgriddrawio-2.png)

**Input:** grid = \[\[5,2,1],[1,1,2]], queries = [3]

**Output:** [0]

**Explanation:** We can not get any points because the value of the top left cell is already greater than or equal to 3.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `2 <= m, n <= 1000`
*   <code>4 <= m * n <= 10<sup>5</sup></code>
*   `k == queries.length`
*   <code>1 <= k <= 10<sup>4</sup></code>
*   <code>1 <= grid[i][j], queries[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

public class Solution {
    private final int[][] dirs = { {-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public int[] maxPoints(int[][] grid, int[] q) {
        int r = grid.length;
        int c = grid[0].length;
        int[] res = new int[q.length];
        Integer[] index = new Integer[q.length];
        for (int i = 0; i < q.length; i++) {
            index[i] = i;
        }
        Arrays.sort(index, Comparator.comparingInt(o -> q[o]));
        Queue<int[]> q1 = new ArrayDeque<>();
        PriorityQueue<int[]> q2 = new PriorityQueue<>(Comparator.comparingInt(a -> a[2]));
        q2.offer(new int[] {0, 0, grid[0][0]});
        boolean[][] visited = new boolean[r][c];
        int count = 0;
        visited[0][0] = true;
        for (int i = 0; i < q.length; i++) {
            int currLimit = q[index[i]];
            while (!q2.isEmpty() && q2.peek()[2] < currLimit) {
                q1.offer(q2.poll());
            }
            while (!q1.isEmpty()) {
                int[] curr = q1.poll();
                count++;
                for (int[] dir : dirs) {
                    int x = dir[0] + curr[0];
                    int y = dir[1] + curr[1];
                    if (x < 0 || y < 0 || x >= r || y >= c || visited[x][y]) {
                        continue;
                    }
                    if (!visited[x][y] && grid[x][y] < currLimit) {
                        q1.offer(new int[] {x, y, grid[x][y]});
                    } else {
                        q2.offer(new int[] {x, y, grid[x][y]});
                    }
                    visited[x][y] = true;
                }
            }
            res[index[i]] = count;
        }
        return res;
    }
}
```