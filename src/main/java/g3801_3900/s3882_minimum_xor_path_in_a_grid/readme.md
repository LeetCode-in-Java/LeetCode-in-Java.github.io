[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3882\. Minimum XOR Path in a Grid

Medium

You are given a 2D integer array `grid` of size `m * n`.

You start at the **top-left** cell `(0, 0)` and want to reach the **bottom-right** cell `(m - 1, n - 1)`.

At each step, you **may** move either **right or down**.

The **cost** of a path is defined as the **bitwise XOR** of all the values in the cells along that path, **including** the start and end cells.

Return the **minimum** possible XOR value among all valid paths from `(0, 0)` to `(m - 1, n - 1)`.

**Example 1:**

**Input:** grid = \[\[1,2],[3,4]]

**Output:** 6

**Explanation:**

There are two valid paths:

*   `(0, 0) → (0, 1) → (1, 1)` with XOR: `1 XOR 2 XOR 4 = 7`
*   `(0, 0) → (1, 0) → (1, 1)` with XOR: `1 XOR 3 XOR 4 = 6`

The minimum XOR value among all valid paths is 6.

**Example 2:**

**Input:** grid = \[\[6,7],[5,8]]

**Output:** 9

**Explanation:**

There are two valid paths:

*   `(0, 0) → (0, 1) → (1, 1)` with XOR: `6 XOR 7 XOR 8 = 9`
*   `(0, 0) → (1, 0) → (1, 1)` with XOR: `6 XOR 5 XOR 8 = 11`

The minimum XOR value among all valid paths is 9.

**Example 3:**

**Input:** grid = \[\[2,7,5]]

**Output:** 0

**Explanation:**

There is only one valid path:

*   `(0, 0) → (0, 1) → (0, 2)` with XOR: `2 XOR 7 XOR 5 = 0`

The XOR value of this path is 0, which is the minimum possible.

**Constraints:**

*   `1 <= m == grid.length <= 1000`
*   `1 <= n == grid[i].length <= 1000`
*   `m * n <= 1000`
*   `0 <= grid[i][j] <= 1023`

## Solution

```java
public class Solution {
    private int ans;
    private boolean[][][] memo;

    public int minCost(int[][] grid) {
        ans = Integer.MAX_VALUE;
        memo = new boolean[grid.length][grid[0].length][1024];
        dfs(grid, 0, 0, 0, grid.length, grid[0].length);
        return ans;
    }

    private void dfs(int[][] grid, int i, int j, int xor, int m, int n) {
        xor ^= grid[i][j];
        if (ans == 0) {
            return;
        }
        if (memo[i][j][xor]) {
            return;
        }
        memo[i][j][xor] = true;
        if (i == m - 1 && j == n - 1) {
            ans = Math.min(ans, xor);
            return;
        }
        if (i + 1 < m) {
            dfs(grid, i + 1, j, xor, m, n);
        }
        if (j + 1 < n) {
            dfs(grid, i, j + 1, xor, m, n);
        }
    }
}
```