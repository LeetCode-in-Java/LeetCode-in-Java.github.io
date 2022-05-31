## 212\. Word Search II

Hard

Given an `m x n` `board` of characters and a list of strings `words`, return _all words on the board_.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

**Input:** board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]

**Output:** ["eat","oath"] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

**Input:** board = [["a","b"],["c","d"]], words = ["abcb"]

**Output:** [] 

**Constraints:**

*   `m == board.length`
*   `n == board[i].length`
*   `1 <= m, n <= 12`
*   `board[i][j]` is a lowercase English letter.
*   <code>1 <= words.length <= 3 * 10<sup>4</sup></code>
*   `1 <= words[i].length <= 10`
*   `words[i]` consists of lowercase English letters.
*   All the strings of `words` are unique.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    private Tree root;

    public List<String> findWords(char[][] board, String[] words) {
        if (board.length < 1 || board[0].length < 1) {
            return Collections.emptyList();
        }
        root = new Tree();
        for (String word : words) {
            Tree.addWord(root, word);
        }
        List<String> collected = new ArrayList<>();
        for (int i = 0; i != board.length; ++i) {
            for (int j = 0; j != board[0].length; ++j) {
                dfs(board, i, j, root, collected);
            }
        }
        return collected;
    }

    private void dfs(char[][] board, int i, int j, Tree cur, List<String> collected) {
        char c = board[i][j];
        if (c == '-') {
            return;
        }
        cur = cur.getChild(c);
        if (cur == null) {
            return;
        }
        if (cur.end != null) {
            String s = cur.end;
            collected.add(s);
            cur.end = null;
            if (cur.len() == 0) {
                Tree.deleteWord(root, s);
            }
        }
        board[i][j] = '-';
        if (i > 0) {
            dfs(board, i - 1, j, cur, collected);
        }
        if (i + 1 < board.length) {
            dfs(board, i + 1, j, cur, collected);
        }
        if (j > 0) {
            dfs(board, i, j - 1, cur, collected);
        }
        if (j + 1 < board[0].length) {
            dfs(board, i, j + 1, cur, collected);
        }
        board[i][j] = c;
    }
}
```