[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 999\. Available Captures for Rook

Easy

On an `8 x 8` chessboard, there is **exactly one** white rook `'R'` and some number of white bishops `'B'`, black pawns `'p'`, and empty squares `'.'`.

When the rook moves, it chooses one of four cardinal directions (north, east, south, or west), then moves in that direction until it chooses to stop, reaches the edge of the board, captures a black pawn, or is blocked by a white bishop. A rook is considered **attacking** a pawn if the rook can capture the pawn on the rook's turn. The **number of available captures** for the white rook is the number of pawns that the rook is **attacking**.

Return _the **number of available captures** for the white rook_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/20/1253_example_1_improved.PNG)

**Input:** board = \[\[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

**Output:** 3

**Explanation:** In this example, the rook is attacking all the pawns.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/02/19/1253_example_2_improved.PNG)

**Input:** board = \[\[".",".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

**Output:** 0

**Explanation:** The bishops are blocking the rook from attacking any of the pawns.

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/02/20/1253_example_3_improved.PNG)

**Input:** board = \[\[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]

**Output:** 3

**Explanation:** The rook is attacking the pawns at positions b5, d6, and f5.

**Constraints:**

*   `board.length == 8`
*   `board[i].length == 8`
*   `board[i][j]` is either `'R'`, `'.'`, `'B'`, or `'p'`
*   There is exactly one cell with `board[i][j] == 'R'`

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int numRookCaptures(char[][] board) {
        // Find the position of the rook
        int rookRow = -1;
        int rookCol = -1;
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 8; j++) {
                if (board[i][j] == 'R') {
                    rookRow = i;
                    rookCol = j;
                    break;
                }
            }
            if (rookRow != -1) {
                break;
            }
        }
        // Define the four directions: up, right, down, left
        int[][] directions = {
            // up
            {-1, 0},
            // right
            {0, 1},
            // down
            {1, 0},
            // left
            {0, -1}
        };
        int captureCount = 0;
        // Check each direction
        for (int[] dir : directions) {
            int row = rookRow;
            int col = rookCol;
            while (true) {
                // Move one step in the current direction
                row += dir[0];
                col += dir[1];
                // Check if out of bounds
                if (row < 0 || row >= 8 || col < 0 || col >= 8) {
                    break;
                }
                // If we hit a bishop, we're blocked
                if (board[row][col] == 'B') {
                    break;
                }
                // If we hit a pawn, we can capture it and then we're blocked
                if (board[row][col] == 'p') {
                    captureCount++;
                    break;
                }
                // Otherwise (empty square), continue in the same direction
            }
        }
        return captureCount;
    }
}
```