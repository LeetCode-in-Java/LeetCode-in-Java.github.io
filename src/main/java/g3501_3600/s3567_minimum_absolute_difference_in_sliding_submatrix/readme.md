[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3567\. Minimum Absolute Difference in Sliding Submatrix

Medium

You are given an `m x n` integer matrix `grid` and an integer `k`.

For every contiguous `k x k` **submatrix** of `grid`, compute the **minimum absolute** difference between any two **distinct** values within that **submatrix**.

Return a 2D array `ans` of size `(m - k + 1) x (n - k + 1)`, where `ans[i][j]` is the minimum absolute difference in the submatrix whose top-left corner is `(i, j)` in `grid`.

**Note**: If all elements in the submatrix have the same value, the answer will be 0.

A submatrix `(x1, y1, x2, y2)` is a matrix that is formed by choosing all cells `matrix[x][y]` where `x1 <= x <= x2` and `y1 <= y <= y2`.

**Example 1:**

**Input:** grid = \[\[1,8],[3,-2]], k = 2

**Output:** [[2]]

**Explanation:**

*   There is only one possible `k x k` submatrix: `[[1, 8], [3, -2]]`.
*   Distinct values in the submatrix are `[1, 8, 3, -2]`.
*   The minimum absolute difference in the submatrix is `|1 - 3| = 2`. Thus, the answer is `[[2]]`.

**Example 2:**

**Input:** grid = \[\[3,-1]], k = 1

**Output:** [[0,0]]

**Explanation:**

*   Both `k x k` submatrix has only one distinct element.
*   Thus, the answer is `[[0, 0]]`.

**Example 3:**

**Input:** grid = \[\[1,-2,3],[2,3,5]], k = 2

**Output:** [[1,2]]

**Explanation:**

*   There are two possible `k × k` submatrix:
    *   Starting at `(0, 0)`: `[[1, -2], [2, 3]]`.
        *   Distinct values in the submatrix are `[1, -2, 2, 3]`.
        *   The minimum absolute difference in the submatrix is `|1 - 2| = 1`.
    *   Starting at `(0, 1)`: `[[-2, 3], [3, 5]]`.
        *   Distinct values in the submatrix are `[-2, 3, 5]`.
        *   The minimum absolute difference in the submatrix is `|3 - 5| = 2`.
*   Thus, the answer is `[[1, 2]]`.

**Constraints:**

*   `1 <= m == grid.length <= 30`
*   `1 <= n == grid[i].length <= 30`
*   <code>-10<sup>5</sup> <= grid[i][j] <= 10<sup>5</sup></code>
*   `1 <= k <= min(m, n)`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[][] minAbsDiff(int[][] grid, int k) {
        int rows = grid.length;
        int cols = grid[0].length;
        int[][] result = new int[rows - k + 1][cols - k + 1];
        for (int x = 0; x <= rows - k; x++) {
            for (int y = 0; y <= cols - k; y++) {
                int size = k * k;
                int[] elements = new int[size];
                int idx = 0;
                for (int i = x; i < x + k; i++) {
                    for (int j = y; j < y + k; j++) {
                        elements[idx++] = grid[i][j];
                    }
                }
                Arrays.sort(elements);
                int minDiff = Integer.MAX_VALUE;
                for (int i = 1; i < size; i++) {
                    if (elements[i] != elements[i - 1]) {
                        minDiff = Math.min(minDiff, elements[i] - elements[i - 1]);
                        if (minDiff == 1) {
                            break;
                        }
                    }
                }
                result[x][y] = minDiff == Integer.MAX_VALUE ? 0 : minDiff;
            }
        }
        return result;
    }
}
```