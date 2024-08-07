[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3239\. Minimum Number of Flips to Make Binary Grid Palindromic I

Medium

You are given an `m x n` binary matrix `grid`.

A row or column is considered **palindromic** if its values read the same forward and backward.

You can **flip** any number of cells in `grid` from `0` to `1`, or from `1` to `0`.

Return the **minimum** number of cells that need to be flipped to make **either** all rows **palindromic** or all columns **palindromic**.

**Example 1:**

**Input:** grid = \[\[1,0,0],[0,0,0],[0,0,1]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/07/screenshot-from-2024-07-08-00-20-10.png)

Flipping the highlighted cells makes all the rows palindromic.

**Example 2:**

**Input:** grid = \[\[0,1],[0,1],[0,0]]

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/07/screenshot-from-2024-07-08-00-31-23.png)

Flipping the highlighted cell makes all the columns palindromic.

**Example 3:**

**Input:** grid = \[\[1],[0]]

**Output:** 0

**Explanation:**

All rows are already palindromic.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m * n <= 2 * 10<sup>5</sup></code>
*   `0 <= grid[i][j] <= 1`

## Solution

```java
public class Solution {
    public int minFlips(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int rowFlips = 0;
        for (int i = 0; i < m / 2; i++) {
            for (int j = 0; j < n; j++) {
                int sum = grid[i][j] + grid[m - 1 - i][j];
                rowFlips += Math.min(sum, 2 - sum);
            }
        }
        int columnFlips = 0;
        for (int j = 0; j < n / 2; j++) {
            for (int[] ints : grid) {
                int sum = ints[j] + ints[n - 1 - j];
                columnFlips += Math.min(sum, 2 - sum);
            }
        }
        return Math.min(rowFlips, columnFlips);
    }
}
```