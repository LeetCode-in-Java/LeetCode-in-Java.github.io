[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3459\. Length of Longest V-Shaped Diagonal Segment

Hard

You are given a 2D integer matrix `grid` of size `n x m`, where each element is either `0`, `1`, or `2`.

A **V-shaped diagonal segment** is defined as:

*   The segment starts with `1`.
*   The subsequent elements follow this infinite sequence: `2, 0, 2, 0, ...`.
*   The segment:
    *   Starts **along** a diagonal direction (top-left to bottom-right, bottom-right to top-left, top-right to bottom-left, or bottom-left to top-right).
    *   Continues the **sequence** in the same diagonal direction.
    *   Makes **at most one clockwise 90-degree** **turn** to another diagonal direction while **maintaining** the sequence.

![](https://assets.leetcode.com/uploads/2025/01/11/length_of_longest3.jpg)

Return the **length** of the **longest** **V-shaped diagonal segment**. If no valid segment _exists_, return 0.

**Example 1:**

**Input:** grid = \[\[2,2,1,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]

**Output:** 5

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/12/09/matrix_1-2.jpg)

The longest V-shaped diagonal segment has a length of 5 and follows these coordinates: `(0,2) → (1,3) → (2,4)`, takes a **90-degree clockwise turn** at `(2,4)`, and continues as `(3,3) → (4,2)`.

**Example 2:**

**Input:** grid = \[\[2,2,2,2,2],[2,0,2,2,0],[2,0,1,1,0],[1,0,2,2,2],[2,0,0,2,2]]

**Output:** 4

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/12/09/matrix_2.jpg)**

The longest V-shaped diagonal segment has a length of 4 and follows these coordinates: `(2,3) → (3,2)`, takes a **90-degree clockwise turn** at `(3,2)`, and continues as `(2,1) → (1,0)`.

**Example 3:**

**Input:** grid = \[\[1,2,2,2,2],[2,2,2,2,0],[2,0,0,0,0],[0,0,2,2,2],[2,0,0,2,0]]

**Output:** 5

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/12/09/matrix_3.jpg)**

The longest V-shaped diagonal segment has a length of 5 and follows these coordinates: `(0,0) → (1,1) → (2,2) → (3,3) → (4,4)`.

**Example 4:**

**Input:** grid = \[\[1]]

**Output:** 1

**Explanation:**

The longest V-shaped diagonal segment has a length of 1 and follows these coordinates: `(0,0)`.

**Constraints:**

*   `n == grid.length`
*   `m == grid[i].length`
*   `1 <= n, m <= 500`
*   `grid[i][j]` is either `0`, `1` or `2`.

## Solution

```java
public class Solution {
    private final int[][] directions = { {-1, -1}, {-1, 1}, {1, 1}, {1, -1}};

    private void initializeArrays(
            int[][] bottomLeft,
            int[][] bottomRight,
            int[][] topLeft,
            int[][] topRight,
            int m,
            int n) {
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                bottomLeft[i][j] = 1;
                bottomRight[i][j] = 1;
                topLeft[i][j] = 1;
                topRight[i][j] = 1;
            }
        }
    }

    private int processBottomDirections(
            int[][] grid, int[][] bottomLeft, int[][] bottomRight, int m, int n) {
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x = grid[i][j];
                if (x == 1) {
                    ans = 1;
                    continue;
                }
                if (i > 0 && j + 1 < n && grid[i - 1][j + 1] == 2 - x) {
                    bottomLeft[i][j] = bottomLeft[i - 1][j + 1] + 1;
                }
                if (i > 0 && j > 0 && grid[i - 1][j - 1] == 2 - x) {
                    bottomRight[i][j] = bottomRight[i - 1][j - 1] + 1;
                }
            }
        }
        return ans;
    }

    private void processTopDirections(
            int[][] grid, int[][] topLeft, int[][] topRight, int m, int n) {
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                int x = grid[i][j];
                if (x == 1) {
                    continue;
                }
                if (i + 1 < m && j + 1 < n && grid[i + 1][j + 1] == 2 - x) {
                    topLeft[i][j] = topLeft[i + 1][j + 1] + 1;
                }
                if (i + 1 < m && j > 0 && grid[i + 1][j - 1] == 2 - x) {
                    topRight[i][j] = topRight[i + 1][j - 1] + 1;
                }
            }
        }
    }

    private int findMaxDiagonal(int[][] grid, int[][][] memo, int m, int n, int initialAns) {
        int ans = initialAns;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x = grid[i][j];
                if (x == 1) {
                    continue;
                }
                x >>= 1;
                for (int k = 0; k < 4; ++k) {
                    int v = memo[k][i][j];
                    if ((v & 1) != x) {
                        continue;
                    }
                    if (v + memo[k + 3 & 3][i][j] > ans) {
                        int[] d = directions[k];
                        int ni = i - d[0] * v;
                        int nj = j - d[1] * v;
                        if (ni >= 0 && nj >= 0 && ni < m && nj < n && grid[ni][nj] == 1) {
                            ans = Math.max(ans, v + memo[k + 3 & 3][i][j]);
                        }
                    }
                }
            }
        }
        return ans;
    }

    public int lenOfVDiagonal(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[][] bottomLeft = new int[m][n];
        int[][] bottomRight = new int[m][n];
        int[][] topLeft = new int[m][n];
        int[][] topRight = new int[m][n];
        initializeArrays(bottomLeft, bottomRight, topLeft, topRight, m, n);
        int ans = processBottomDirections(grid, bottomLeft, bottomRight, m, n);
        processTopDirections(grid, topLeft, topRight, m, n);
        int[][][] memo = {topLeft, topRight, bottomRight, bottomLeft};
        return findMaxDiagonal(grid, memo, m, n, ans);
    }
}
```