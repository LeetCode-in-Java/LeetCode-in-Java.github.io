[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 688\. Knight Probability in Chessboard

Medium

On an `n x n` chessboard, a knight starts at the cell `(row, column)` and attempts to make exactly `k` moves. The rows and columns are **0-indexed**, so the top-left cell is `(0, 0)`, and the bottom-right cell is `(n - 1, n - 1)`.

A chess knight has eight possible moves it can make, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![](https://assets.leetcode.com/uploads/2018/10/12/knight.png)

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly `k` moves or has moved off the chessboard.

Return _the probability that the knight remains on the board after it has stopped moving_.

**Example 1:**

**Input:** n = 3, k = 2, row = 0, column = 0

**Output:** 0.06250

**Explanation:** There are two moves (to (1,2), (2,1)) that will keep the knight on the board. From each of those positions, there are also two moves that will keep the knight on the board. The total probability the knight stays on the board is 0.0625.

**Example 2:**

**Input:** n = 1, k = 0, row = 0, column = 0

**Output:** 1.00000

**Constraints:**

*   `1 <= n <= 25`
*   `0 <= k <= 100`
*   `0 <= row, column <= n`

## Solution

```java
public class Solution {
    private final int[][] directions =
            new int[][] { {-2, -1}, {-2, 1}, {-1, 2}, {1, 2}, {2, -1}, {2, 1}, {1, -2}, {-1, -2}};
    private double[][][] probabilityGiven;

    public double knightProbability(int n, int k, int row, int column) {
        probabilityGiven = new double[n][n][k + 1];
        return probability(row, column, k, n);
    }

    private double probability(int row, int column, int k, int n) {
        if (k == 0) {
            return 1.0;
        } else if (probabilityGiven[row][column][k] != 0) {
            return probabilityGiven[row][column][k];
        } else {
            double p = 0d;
            for (int[] dir : directions) {
                if (isValid(row + dir[0], column + dir[1], n)) {
                    p += probability(row + dir[0], column + dir[1], k - 1, n);
                }
            }
            probabilityGiven[row][column][k] = p / 8.0;
            return probabilityGiven[row][column][k];
        }
    }

    private boolean isValid(int row, int column, int n) {
        return (row >= 0 && row < n && column >= 0 && column < n);
    }
}
```