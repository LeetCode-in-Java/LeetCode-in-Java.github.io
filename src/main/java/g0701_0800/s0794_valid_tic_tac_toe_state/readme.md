## 794\. Valid Tic-Tac-Toe State

Medium

Given a Tic-Tac-Toe board as a string array `board`, return `true` if and only if it is possible to reach this board position during the course of a valid tic-tac-toe game.

The board is a `3 x 3` array that consists of characters `' '`, `'X'`, and `'O'`. The `' '` character represents an empty square.

Here are the rules of Tic-Tac-Toe:

*   Players take turns placing characters into empty squares `' '`.
*   The first player always places `'X'` characters, while the second player always places `'O'` characters.
*   `'X'` and `'O'` characters are always placed into empty squares, never filled ones.
*   The game ends when there are three of the same (non-empty) character filling any row, column, or diagonal.
*   The game also ends if all squares are non-empty.
*   No more moves can be played if the game is over.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/15/tictactoe1-grid.jpg)

**Input:** board = ["O "," "," "]

**Output:** false

**Explanation:** The first player always plays "X". 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/15/tictactoe2-grid.jpg)

**Input:** board = ["XOX"," X "," "]

**Output:** false

**Explanation:** Players take turns making moves. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/15/tictactoe4-grid.jpg)

**Input:** board = ["XOX","O O","XOX"]

**Output:** true 

**Constraints:**

*   `board.length == 3`
*   `board[i].length == 3`
*   `board[i][j]` is either `'X'`, `'O'`, or `' '`.

## Solution

```java
public class Solution {
    public boolean validTicTacToe(String[] board) {
        // X=1,O=-1,’ ’=0
        int sum = 0;
        int[] winsCol = new int[3];
        int[] winsDiag = new int[2];
        boolean xWin = false;
        boolean oWin = false;
        for (int i = 0; i < 3; i++) {
            String str = board[i];
            int rowSum = 0;
            for (int j = 0; j < 3; j++) {
                // char chr=str.toCharArray()[j];
                int intchr = 0;
                if (str.toCharArray()[j] == 'X') {
                    intchr = 1;
                }
                if (str.toCharArray()[j] == 'O') {
                    intchr = -1;
                }
                rowSum += intchr;
                winsCol[j] += intchr;
                if (i == 2 && winsCol[j] == 3) {
                    xWin = true;
                }
                if (i == 2 && winsCol[j] == -3) {
                    oWin = true;
                }
                if (Math.abs(i - j) != 1) {
                    if (i == j && i == 1) {
                        winsDiag[0] += intchr;
                        winsDiag[1] += intchr;
                    } else if (i == j) {
                        winsDiag[0] += intchr;
                    } else {
                        winsDiag[1] += intchr;
                    }
                }
                if (i == 2 && Math.max(winsDiag[0], winsDiag[1]) == 3) {
                    xWin = true;
                }
                if (i == 2 && Math.min(winsDiag[0], winsDiag[1]) == -3) {
                    oWin = true;
                }
            }
            if (rowSum == 3) {
                xWin = true;
            }
            if (rowSum == -3) {
                oWin = true;
            }
            sum += rowSum;
        }
        if (sum == 0 && !xWin) {
            return true;
        }
        return sum == 1 && !oWin;
    }
}
```