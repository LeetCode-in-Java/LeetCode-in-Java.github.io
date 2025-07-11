[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1138\. Alphabet Board Path

Medium

On an alphabet board, we start at position `(0, 0)`, corresponding to character `board[0][0]`.

Here, `board = ["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"]`, as shown in the diagram below.

![](https://assets.leetcode.com/uploads/2019/07/28/azboard.png)

We may make the following moves:

*   `'U'` moves our position up one row, if the position exists on the board;
*   `'D'` moves our position down one row, if the position exists on the board;
*   `'L'` moves our position left one column, if the position exists on the board;
*   `'R'` moves our position right one column, if the position exists on the board;
*   `'!'` adds the character `board[r][c]` at our current position `(r, c)` to the answer.

(Here, the only positions that exist on the board are positions with letters on them.)

Return a sequence of moves that makes our answer equal to `target` in the minimum number of moves. You may return any path that does so.

**Example 1:**

**Input:** target = "leet"

**Output:** "DDR!UURRR!!DDD!"

**Example 2:**

**Input:** target = "code"

**Output:** "RR!DDRR!UUL!R!"

**Constraints:**

*   `1 <= target.length <= 100`
*   `target` consists only of English lowercase letters.

## Solution

```java
public class Solution {
    public String alphabetBoardPath(String target) {
        if (target.isEmpty()) {
            return "";
        }
        int sourceRow = 0;
        int sourceCol = 0;
        StringBuilder path = new StringBuilder();
        for (char c : target.toCharArray()) {
            int position = c - 97;
            int targetRow = position / 5;
            int targetCol = position % 5;
            if (targetCol < sourceCol) {
                path.append(helper("L", sourceCol - targetCol));
            }
            if (targetRow < sourceRow) {
                path.append(helper("U", sourceRow - targetRow));
            }
            if (targetRow > sourceRow) {
                path.append(helper("D", targetRow - sourceRow));
            }
            if (targetCol > sourceCol) {
                path.append(helper("R", targetCol - sourceCol));
            }
            path.append("!");
            sourceRow = targetRow;
            sourceCol = targetCol;
        }
        return path.toString();
    }

    public StringBuilder helper(String dir, int time) {
        StringBuilder path = new StringBuilder();
        path.append(String.valueOf(dir).repeat(Math.max(0, time)));
        return path;
    }
}
```