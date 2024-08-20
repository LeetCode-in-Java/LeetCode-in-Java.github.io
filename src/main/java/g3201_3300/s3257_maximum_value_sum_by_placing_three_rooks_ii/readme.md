[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3257\. Maximum Value Sum by Placing Three Rooks II

Hard

You are given a `m x n` 2D array `board` representing a chessboard, where `board[i][j]` represents the **value** of the cell `(i, j)`.

Rooks in the **same** row or column **attack** each other. You need to place _three_ rooks on the chessboard such that the rooks **do not** **attack** each other.

Return the **maximum** sum of the cell **values** on which the rooks are placed.

**Example 1:**

**Input:** board = \[\[-3,1,1,1],[-3,1,-3,1],[-3,2,1,1]]

**Output:** 4

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/08/rooks2.png)

We can place the rooks in the cells `(0, 2)`, `(1, 3)`, and `(2, 1)` for a sum of `1 + 1 + 2 = 4`.

**Example 2:**

**Input:** board = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** 15

**Explanation:**

We can place the rooks in the cells `(0, 0)`, `(1, 1)`, and `(2, 2)` for a sum of `1 + 5 + 9 = 15`.

**Example 3:**

**Input:** board = \[\[1,1,1],[1,1,1],[1,1,1]]

**Output:** 3

**Explanation:**

We can place the rooks in the cells `(0, 2)`, `(1, 1)`, and `(2, 0)` for a sum of `1 + 1 + 1 = 3`.

**Constraints:**

*   `3 <= m == board.length <= 500`
*   `3 <= n == board[i].length <= 500`
*   <code>-10<sup>9</sup> <= board[i][j] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long maximumValueSum(int[][] board) {
        int n = board.length;
        int m = board[0].length;
        int[][] tb = new int[n][m];
        tb[0] = Arrays.copyOf(board[0], m);
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < m; j++) {
                tb[i][j] = Math.max(tb[i - 1][j], board[i][j]);
            }
        }
        int[][] bt = new int[n][m];
        bt[n - 1] = Arrays.copyOf(board[n - 1], m);
        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j < m; j++) {
                bt[i][j] = Math.max(bt[i + 1][j], board[i][j]);
            }
        }
        long ans = Long.MIN_VALUE;
        for (int i = 1; i < n - 1; i++) {
            int[][] max3Top = getMax3(tb[i - 1]);
            int[][] max3Cur = getMax3(board[i]);
            int[][] max3Bottom = getMax3(bt[i + 1]);
            for (int[] topCand : max3Top) {
                for (int[] curCand : max3Cur) {
                    for (int[] bottomCand : max3Bottom) {
                        if (topCand[1] != curCand[1]
                                && topCand[1] != bottomCand[1]
                                && curCand[1] != bottomCand[1]) {
                            long cand = (long) topCand[0] + curCand[0] + bottomCand[0];
                            ans = Math.max(ans, cand);
                        }
                    }
                }
            }
        }
        return ans;
    }

    private int[][] getMax3(int[] row) {
        int m = row.length;
        int[][] ans = new int[3][2];
        Arrays.fill(ans, new int[] {Integer.MIN_VALUE, -1});
        for (int j = 0; j < m; j++) {
            if (row[j] >= ans[0][0]) {
                ans[2] = ans[1];
                ans[1] = ans[0];
                ans[0] = new int[] {row[j], j};
            } else if (row[j] >= ans[1][0]) {
                ans[2] = ans[1];
                ans[1] = new int[] {row[j], j};
            } else if (row[j] > ans[2][0]) {
                ans[2] = new int[] {row[j], j};
            }
        }
        return ans;
    }
}
```