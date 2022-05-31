## 130\. Surrounded Regions

Medium

Given an `m x n` matrix `board` containing `'X'` and `'O'`, _capture all regions that are 4-directionally surrounded by_ `'X'`.

A region is **captured** by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

**Input:** board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

**Output:** [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

**Explanation:** Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically. 

**Example 2:**

**Input:** board = [["X"]]

**Output:** [["X"]] 

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 200`
*   `board[i][j]` is `'X'` or `'O'`.

## Solution

```java
public class Solution {
    public void solve(char[][] board) {
        // Edge case, empty grid
        if (board.length == 0) {
            return;
        }
        // Traverse first and last rows ( boundaries)
        for (int i = 0; i < board[0].length; i++) {
            // first row
            if (board[0][i] == 'O') {
                // It will covert O and all it's touching O's to #
                dfs(board, 0, i);
            }
            // last row
            if (board[board.length - 1][i] == 'O') {
                // Coverts O's to #'s (same thing as above)
                dfs(board, board.length - 1, i);
            }
        }
        // Traverse first and last Column (boundaries)
        for (int i = 0; i < board.length; i++) {
            // first Column
            if (board[i][0] == 'O') {
                // Converts O's to #'s
                dfs(board, i, 0);
            }
            // last Column
            if (board[i][board[0].length - 1] == 'O') {
                // Coverts O's to #'s
                dfs(board, i, board[0].length - 1);
            }
        }
        // Traverse through entire matrix
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == 'O') {
                    // Convert O's to X's
                    board[i][j] = 'X';
                }
                if (board[i][j] == '#') {
                    // Convert #'s to O's
                    board[i][j] = 'O';
                }
            }
        }
    }

    void dfs(char[][] board, int row, int column) {
        if (row < 0
                || row >= board.length
                || column < 0
                || column >= board[0].length
                || board[row][column] != 'O') {
            return;
        }
        board[row][column] = '#';
        dfs(board, row + 1, column);
        dfs(board, row - 1, column);
        dfs(board, row, column + 1);
        dfs(board, row, column - 1);
    }
}
```