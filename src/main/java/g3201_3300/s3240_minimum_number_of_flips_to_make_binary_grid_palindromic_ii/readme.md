[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3240\. Minimum Number of Flips to Make Binary Grid Palindromic II

Medium

You are given an `m x n` binary matrix `grid`.

A row or column is considered **palindromic** if its values read the same forward and backward.

You can **flip** any number of cells in `grid` from `0` to `1`, or from `1` to `0`.

Return the **minimum** number of cells that need to be flipped to make **all** rows and columns **palindromic**, and the total number of `1`'s in `grid` **divisible** by `4`.

**Example 1:**

**Input:** grid = \[\[1,0,0],[0,1,0],[0,0,1]]

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/01/image.png)

**Example 2:**

**Input:** grid = \[\[0,1],[0,1],[0,0]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/08/screenshot-from-2024-07-09-01-37-48.png)

**Example 3:**

**Input:** grid = \[\[1],[1]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/01/screenshot-from-2024-08-01-23-05-26.png)

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m * n <= 2 * 10<sup>5</sup></code>
*   `0 <= grid[i][j] <= 1`

## Solution

```java
public class Solution {
    public int minFlips(int[][] grid) {
        int res = 0;
        int one = 0;
        int diff = 0;
        int m = grid.length;
        int n = grid[0].length;
        // Handle quadrants
        for (int i = 0; i < m / 2; ++i) {
            for (int j = 0; j < n / 2; ++j) {
                int v =
                        grid[i][j]
                                + grid[i][n - 1 - j]
                                + grid[m - 1 - i][j]
                                + grid[m - 1 - i][n - 1 - j];
                res += Math.min(v, 4 - v);
            }
        }
        // Handle middle column
        if (n % 2 > 0) {
            for (int i = 0; i < m / 2; ++i) {
                diff += grid[i][n / 2] ^ grid[m - 1 - i][n / 2];
                one += grid[i][n / 2] + grid[m - 1 - i][n / 2];
            }
        }
        // Handle middle row
        if (m % 2 > 0) {
            for (int j = 0; j < n / 2; ++j) {
                diff += grid[m / 2][j] ^ grid[m / 2][n - 1 - j];
                one += grid[m / 2][j] + grid[m / 2][n - 1 - j];
            }
        }
        // Handle center point
        if (n % 2 > 0 && m % 2 > 0) {
            res += grid[m / 2][n / 2];
        }
        // Divisible by 4
        if (diff == 0 && one % 4 > 0) {
            res += 2;
        }
        return res + diff;
    }
}
```