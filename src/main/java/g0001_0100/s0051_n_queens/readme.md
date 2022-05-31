## 51\. N-Queens

Hard

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4

**Output:** [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]

**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above 

**Example 2:**

**Input:** n = 1

**Output:** [["Q"]] 

**Constraints:**

*   `1 <= n <= 9`

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public List<List<String>> solveNQueens(int n) {
        boolean[] pos = new boolean[n + 2 * n - 1 + 2 * n - 1];
        int[] pos2 = new int[n];
        List<List<String>> ans = new ArrayList<>();
        helper(n, 0, pos, pos2, ans);
        return ans;
    }

    private void helper(int n, int row, boolean[] pos, int[] pos2, List<List<String>> ans) {
        if (row == n) {
            construct(n, pos2, ans);
            return;
        }
        for (int i = 0; i < n; i++) {
            int index = n + 2 * n - 1 + n - 1 + i - row;
            if (pos[i] || pos[n + i + row] || pos[index]) {
                continue;
            }
            pos[i] = true;
            pos[n + i + row] = true;
            pos[index] = true;
            pos2[row] = i;
            helper(n, row + 1, pos, pos2, ans);
            pos[i] = false;
            pos[n + i + row] = false;
            pos[index] = false;
        }
    }

    private void construct(int n, int[] pos, List<List<String>> ans) {
        List<String> sol = new ArrayList<>();
        for (int r = 0; r < n; r++) {
            char[] queenRow = new char[n];
            Arrays.fill(queenRow, '.');
            queenRow[pos[r]] = 'Q';
            sol.add(new String(queenRow));
        }
        ans.add(sol);
    }
}
```