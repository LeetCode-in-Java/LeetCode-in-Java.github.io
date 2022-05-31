## 1275\. Find Winner on a Tic Tac Toe Game

Easy

**Tic-tac-toe** is played by two players `A` and `B` on a `3 x 3` grid. The rules of Tic-Tac-Toe are:

*   Players take turns placing characters into empty squares `' '`.
*   The first player `A` always places `'X'` characters, while the second player `B` always places `'O'` characters.
*   `'X'` and `'O'` characters are always placed into empty squares, never on filled ones.
*   The game ends when there are **three** of the same (non-empty) character filling any row, column, or diagonal.
*   The game also ends if all squares are non-empty.
*   No more moves can be played if the game is over.

Given a 2D integer array `moves` where <code>moves[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> indicates that the <code>i<sup>th</sup></code> move will be played on <code>grid[row<sub>i</sub>][col<sub>i</sub>]</code>. return _the winner of the game if it exists_ (`A` or `B`). In case the game ends in a draw return `"Draw"`. If there are still movements to play return `"Pending"`.

You can assume that `moves` is valid (i.e., it follows the rules of **Tic-Tac-Toe**), the grid is initially empty, and `A` will play first.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/22/xo1-grid.jpg)

**Input:** moves = \[\[0,0],[2,0],[1,1],[2,1],[2,2]]

**Output:** "A"

**Explanation:** A wins, they always play first.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/09/22/xo2-grid.jpg)

**Input:** moves = \[\[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]

**Output:** "B"

**Explanation:** B wins.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/09/22/xo3-grid.jpg)

**Input:** moves = \[\[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]

**Output:** "Draw"

**Explanation:** The game ends in a draw since there are no moves to make.

**Constraints:**

*   `1 <= moves.length <= 9`
*   `moves[i].length == 2`
*   <code>0 <= row<sub>i</sub>, col<sub>i</sub> <= 2</code>
*   There are no repeated elements on `moves`.
*   `moves` follow the rules of tic tac toe.

## Solution

```java
public class Solution {
    public String tictactoe(int[][] moves) {
        String[][] board = new String[3][3];
        for (int i = 0; i < moves.length; i++) {
            if (i % 2 == 0) {
                board[moves[i][0]][moves[i][1]] = "X";
            } else {
                board[moves[i][0]][moves[i][1]] = "O";
            }
            if (i > 3 && !wins(board).equals("")) {
                return wins(board);
            }
        }
        return moves.length == 9 ? "Draw" : "Pending";
    }

    private String wins(String[][] board) {
        for (int i = 0; i < 3; i++) {
            if (board[i][0] == null) {
                break;
            }
            String str = board[i][0];
            if (str.equals(board[i][1]) && str.equals(board[i][2])) {
                return getWinner(str);
            }
        }

        for (int j = 0; j < 3; j++) {
            if (board[0][j] == null) {
                break;
            }
            String str = board[0][j];
            if (str.equals(board[1][j]) && str.equals(board[2][j])) {
                return getWinner(str);
            }
        }

        if (board[1][1] == null) {
            return "";
        }
        String str = board[1][1];
        if (str.equals(board[0][0]) && str.equals(board[2][2])
                || (str.equals(board[0][2]) && str.equals(board[2][0]))) {
            return getWinner(str);
        }
        return "";
    }

    private String getWinner(String str) {
        if (str.equals("X")) {
            return "A";
        } else {
            return "B";
        }
    }
}
```