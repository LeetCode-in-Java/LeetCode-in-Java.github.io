[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 419\. Battleships in a Board

Medium

Given an `m x n` matrix `board` where each cell is a battleship `'X'` or empty `'.'`, return _the number of the **battleships** on_ `board`.

**Battleships** can only be placed horizontally or vertically on `board`. In other words, they can only be made of the shape `1 x k` (`1` row, `k` columns) or `k x 1` (`k` rows, `1` column), where `k` can be of any size. At least one horizontal or vertical cell separates between two battleships (i.e., there are no adjacent battleships).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/10/battelship-grid.jpg)

**Input:** board = \[\["X",".",".","X"],[".",".",".","X"],[".",".",".","X"]]

**Output:** 2 

**Example 2:**

**Input:** board = \[\["."]]

**Output:** 0 

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 200`
*   `board[i][j]` is either `'.'` or `'X'`.

**Follow up:** Could you do it in one-pass, using only `O(1)` extra memory and without modifying the values `board`?

## Solution

```java
public class Solution {
    public int countBattleships(char[][] board) {
        if (board == null || board.length == 0) {
            return 0;
        }
        int count = 0;
        int m = board.length;
        int n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] != '.'
                        && (j == 0 || board[i][j - 1] != 'X')
                        && (i <= 0 || board[i - 1][j] != 'X')) {
                    count++;
                }
            }
        }
        return count;
    }
}
```