[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2428\. Maximum Sum of an Hourglass

Medium

You are given an `m x n` integer matrix `grid`.

We define an **hourglass** as a part of the matrix with the following form:

![](https://assets.leetcode.com/uploads/2022/08/21/img.jpg)

Return _the **maximum** sum of the elements of an hourglass_.

**Note** that an hourglass cannot be rotated and must be entirely contained within the matrix.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/08/21/1.jpg)

**Input:** grid = \[\[6,2,1,3],[4,2,1,5],[9,2,8,7],[4,1,2,9]]

**Output:** 30

**Explanation:** The cells shown above represent the hourglass with the maximum sum: 6 + 2 + 1 + 2 + 9 + 2 + 8 = 30.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/08/21/2.jpg)

**Input:** grid = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** 35

**Explanation:** There is only one hourglass in the matrix, with the sum: 1 + 2 + 3 + 5 + 7 + 8 + 9 = 35.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `3 <= m, n <= 150`
*   <code>0 <= grid[i][j] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int maxSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (isHourGlass(i, j, m, n)) {
                    res = Math.max(res, calculate(i, j, grid));
                } else {
                    // If we cannot form an hour glass from the current row anymore, just move to
                    // the next one
                    break;
                }
            }
        }
        return res;
    }

    // Check if an hour glass can be formed from the current position
    private boolean isHourGlass(int r, int c, int m, int n) {
        return r + 2 < m && c + 2 < n;
    }

    // Once we know an hourglass can be formed, just traverse the value
    private int calculate(int r, int c, int[][] grid) {
        int sum = 0;
        // Traverse the bottom and the top row of the hour glass at the same time
        for (int i = c; i <= c + 2; i++) {
            sum += grid[r][i];
            sum += grid[r + 2][i];
        }
        // Add the middle vlaue
        sum += grid[r + 1][c + 1];
        return sum;
    }
}
```