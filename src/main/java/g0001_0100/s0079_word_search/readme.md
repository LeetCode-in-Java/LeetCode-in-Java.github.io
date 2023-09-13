[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 79\. Word Search

Medium

Given an `m x n` grid of characters `board` and a string `word`, return `true` _if_ `word` _exists in the grid_.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

**Input:** board = \[\["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"

**Output:** true 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

**Input:** board = \[\["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"

**Output:** true 

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

**Input:** board = \[\["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"

**Output:** false 

**Constraints:**

*   `m == board.length`
*   `n = board[i].length`
*   `1 <= m, n <= 6`
*   `1 <= word.length <= 15`
*   `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

## Solution

```java
public class Solution {
    private boolean backtrace(
            char[][] board, boolean[][] visited, String word, int index, int x, int y) {
        if (index == word.length()) {
            return true;
        }
        if (x < 0 || x >= board.length || y < 0 || y >= board[0].length || visited[x][y]) {
            return false;
        }
        visited[x][y] = true;
        if (word.charAt(index) == board[x][y]) {
            boolean res =
                    backtrace(board, visited, word, index + 1, x, y + 1)
                            || backtrace(board, visited, word, index + 1, x, y - 1)
                            || backtrace(board, visited, word, index + 1, x + 1, y)
                            || backtrace(board, visited, word, index + 1, x - 1, y);
            if (!res) {
                visited[x][y] = false;
            }
            return res;
        } else {
            visited[x][y] = false;
            return false;
        }
    }

    public boolean exist(char[][] board, String word) {
        boolean[][] visited = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; ++i) {
            for (int j = 0; j < board[0].length; ++j) {
                if (backtrace(board, visited, word, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses a backtracking approach to explore all possible paths in the 2D board while searching for the target word.

2. The `exist` function iterates over all cells in the board, and for each cell, it invokes the `backtrace` function to start the backtracking process.

3. The `backtrace` function explores all four possible directions (up, down, left, right) from the current cell.

4. In the worst case, the `backtrace` function explores all possible paths in the board until it either finds the target word or determines that it cannot be formed.

5. The time complexity depends on the number of cells in the board (m * n), where 'm' is the number of rows, and 'n' is the number of columns. For each cell, the `backtrace` function explores four possible directions. Therefore, the overall time complexity is O(4^(m*n)), where 'm' and 'n' are the dimensions of the board.

**Space Complexity (Big O Space):**

1. The primary space usage in the program comes from the recursive call stack and the `visited` array.

2. The recursive call stack's maximum depth depends on the number of cells explored during backtracking. In the worst case, where the entire board is explored, the depth can be as large as 'm * n'. Therefore, the space complexity due to the call stack is O(m * n).

3. The `visited` array is used to keep track of visited cells to avoid revisiting them during the backtracking process. Its size is equal to the number of cells in the board, which is 'm * n'. Therefore, the space complexity due to the `visited` array is also O(m * n).

4. Combining the space complexities of the call stack and the `visited` array, the overall space complexity is O(m * n).

In summary, the time complexity of the program is O(4^(m*n)), and the space complexity is O(m * n), where 'm' and 'n' are the dimensions of the board. This program efficiently explores all possible paths in the board to search for the target word using a backtracking approach.
