[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2018\. Check if Word Can Be Placed In Crossword

Medium

You are given an `m x n` matrix `board`, representing the **current** state of a crossword puzzle. The crossword contains lowercase English letters (from solved words), `' '` to represent any **empty** cells, and `'#'` to represent any **blocked** cells.

A word can be placed **horizontally** (left to right **or** right to left) or **vertically** (top to bottom **or** bottom to top) in the board if:

*   It does not occupy a cell containing the character `'#'`.
*   The cell each letter is placed in must either be `' '` (empty) or **match** the letter already on the `board`.
*   There must not be any empty cells `' '` or other lowercase letters **directly left or right** of the word if the word was placed **horizontally**.
*   There must not be any empty cells `' '` or other lowercase letters **directly above or below** the word if the word was placed **vertically**.

Given a string `word`, return `true` _if_ `word` _can be placed in_ `board`_, or_ `false` _**otherwise**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex1-1.png)

**Input:** board = \[\["#", " ", "#"], [" ", " ", "#"], ["#", "c", " "]], word = "abc"

**Output:** true

**Explanation:** The word "abc" can be placed as shown above (top to bottom).

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex2-1.png)

**Input:** board = \[\[" ", "#", "a"], [" ", "#", "c"], [" ", "#", "a"]], word = "ac"

**Output:** false

**Explanation:** It is impossible to place the word because there will always be a space/letter above or below it.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex3-1.png)

**Input:** board = \[\["#", " ", "#"], [" ", " ", "#"], ["#", " ", "c"]], word = "ca"

**Output:** true

**Explanation:** The word "ca" can be placed as shown above (right to left).

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   <code>1 <= m * n <= 2 * 10<sup>5</sup></code>
*   `board[i][j]` will be `' '`, `'#'`, or a lowercase English letter.
*   `1 <= word.length <= max(m, n)`
*   `word` will contain only lowercase English letters.

## Solution

```java
public class Solution {
    public boolean placeWordInCrossword(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if ((board[i][j] == ' ' || board[i][j] == word.charAt(0))
                        && (canPlaceTopDown(word, board, i, j)
                                || canPlaceLeftRight(word, board, i, j)
                                || canPlaceBottomUp(word, board, i, j)
                                || canPlaceRightLeft(word, board, i, j))) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean canPlaceRightLeft(String word, char[][] board, int row, int col) {
        if (col + 1 < board[0].length
                && (Character.isLowerCase(board[row][col + 1]) || board[row][col + 1] == ' ')) {
            return false;
        }
        int k = 0;
        int j = col;
        for (; j >= 0 && k < word.length(); j--) {
            if (board[row][j] != word.charAt(k) && board[row][j] != ' ') {
                return false;
            } else {
                k++;
            }
        }
        return k == word.length() && (j < 0 || board[row][j] == '#');
    }

    private boolean canPlaceBottomUp(String word, char[][] board, int row, int col) {
        if (row + 1 < board.length
                && (Character.isLowerCase(board[row + 1][col]) || board[row + 1][col] == ' ')) {
            return false;
        }
        int k = 0;
        int i = row;
        for (; i >= 0 && k < word.length(); i--) {
            if (board[i][col] != word.charAt(k) && board[i][col] != ' ') {
                return false;
            } else {
                k++;
            }
        }
        return k == word.length() && (i < 0 || board[i][col] == '#');
    }

    private boolean canPlaceLeftRight(String word, char[][] board, int row, int col) {
        if (col > 0 && (Character.isLowerCase(board[row][col - 1]) || board[row][col - 1] == ' ')) {
            return false;
        }
        int k = 0;
        int j = col;
        for (; j < board[0].length && k < word.length(); j++) {
            if (board[row][j] != word.charAt(k) && board[row][j] != ' ') {
                return false;
            } else {
                k++;
            }
        }
        return k == word.length() && (j == board[0].length || board[row][j] == '#');
    }

    private boolean canPlaceTopDown(String word, char[][] board, int row, int col) {
        if (row > 0 && (Character.isLowerCase(board[row - 1][col]) || board[row - 1][col] == ' ')) {
            return false;
        }
        int k = 0;
        int i = row;
        for (; i < board.length && k < word.length(); i++) {
            if (board[i][col] != word.charAt(k) && board[i][col] != ' ') {
                return false;
            } else {
                k++;
            }
        }
        return k == word.length() && (i == board.length || board[i][col] == '#');
    }
}
```