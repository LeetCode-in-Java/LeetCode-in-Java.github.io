[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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

**Time Complexity (Big O Time):**

1. The program uses a recursive backtracking algorithm to explore all possible solutions. It places queens row by row, and for each row, it iterates through the columns to find a valid placement.

2. The main recursive function `helper` is called for each row, and for each row, it iterates through the columns. In the worst case, this results in exploring all possible combinations of queen placements.

3. For each queen placement, it checks whether it's valid based on the positions of previously placed queens. The checks include validating the columns, diagonals, and anti-diagonals.

4. The time complexity of the recursive algorithm can be analyzed as follows:
   - For the first row, there are N possibilities.
   - For the second row, there are N-2 possibilities (two columns in the same column as the first queen are not allowed).
   - For the third row, there are N-4 possibilities (four columns are eliminated due to diagonal and anti-diagonal constraints), and so on.

5. The worst-case time complexity of exploring all possible combinations is exponential, and it's typically represented as O(N!) for the N-Queens problem, where N is the size of the chessboard (number of rows and columns).

6. Additionally, constructing the solution in the `construct` method takes O(N) time for each solution.

7. Therefore, the overall time complexity is dominated by the number of recursive calls and is O(N!).

**Space Complexity (Big O Space):**

1. The space complexity is determined by the additional data structures used in the program.

2. The `pos` boolean array of size `n + 2 * n - 1 + 2 * n - 1` is used to keep track of occupied columns, diagonals, and anti-diagonals. It grows with the size of the chessboard but is not dependent on the number of solutions. Therefore, its space complexity is O(N).

3. The `pos2` integer array of size `n` is used to keep track of the column positions of queens in each row. Its space complexity is O(N).

4. The `ans` list of lists stores the solutions, and its space complexity depends on the number of solutions. In the worst case, there can be N! solutions.

5. Therefore, the overall space complexity is dominated by the storage of solutions and is O(N!).

In summary, the time complexity of the provided program is O(N!) due to the combinatorial nature of the N-Queens problem, and the space complexity is O(N) due to the additional data structures used to track queen placements and store solutions.
