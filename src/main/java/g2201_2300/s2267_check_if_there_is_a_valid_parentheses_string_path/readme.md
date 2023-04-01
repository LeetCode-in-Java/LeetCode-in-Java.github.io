[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2267\. Check if There Is a Valid Parentheses String Path

Hard

A parentheses string is a **non-empty** string consisting only of `'('` and `')'`. It is **valid** if **any** of the following conditions is **true**:

*   It is `()`.
*   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
*   It can be written as `(A)`, where `A` is a valid parentheses string.

You are given an `m x n` matrix of parentheses `grid`. A **valid parentheses string path** in the grid is a path satisfying **all** of the following conditions:

*   The path starts from the upper left cell `(0, 0)`.
*   The path ends at the bottom-right cell `(m - 1, n - 1)`.
*   The path only ever moves **down** or **right**.
*   The resulting parentheses string formed by the path is **valid**.

Return `true` _if there exists a **valid parentheses string path** in the grid._ Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/15/example1drawio.png)

**Input:** grid = \[\["(","(","("],[")","(",")"],["(","(",")"],["(","(",")"]]

**Output:** true

**Explanation:** The above diagram shows two possible paths that form valid parentheses strings. 

The first path shown results in the valid parentheses string "()(())". 

The second path shown results in the valid parentheses string "((()))". 

Note that there may be other valid parentheses string paths.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/15/example2drawio.png)

**Input:** grid = \[\[")",")"],["(","("]]

**Output:** false

**Explanation:** The two possible paths form the parentheses strings "))(" and ")((". Since neither of them are valid parentheses strings, we return false.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 100`
*   `grid[i][j]` is either `'('` or `')'`.

## Solution

```java
public class Solution {
    private char[][] grid;
    private int m;
    private int n;
    private static final char LFTPAR = '(';
    private static final char RGTPAR = ')';

    public boolean hasValidPath(char[][] grid) {
        this.grid = grid;
        this.m = grid.length;
        this.n = grid[0].length;
        Boolean[][][] dp = new Boolean[m][n][m + n + 1];
        if (grid[0][0] == RGTPAR) {
            return false;
        }
        if ((m + n) % 2 == 0) {
            return false;
        }
        return dfs(0, 0, 0, 0, dp);
    }

    private boolean dfs(int u, int v, int open, int close, Boolean[][][] dp) {
        if (grid[u][v] == LFTPAR) {
            open++;
        } else {
            close++;
        }
        if (u == m - 1 && v == n - 1) {
            return open == close;
        }
        if (open < close) {
            return false;
        }
        if (dp[u][v][open - close] != null) {
            return dp[u][v][open - close];
        }
        if (u == m - 1) {
            boolean result = dfs(u, v + 1, open, close, dp);
            dp[u][v][open - close] = result;
            return result;
        }
        if (v == n - 1) {
            return dfs(u + 1, v, open, close, dp);
        }
        boolean rslt;
        if (grid[u][v] == LFTPAR) {
            rslt = dfs(u + 1, v, open, close, dp) || dfs(u, v + 1, open, close, dp);
        } else {
            rslt = dfs(u, v + 1, open, close, dp) || dfs(u + 1, v, open, close, dp);
        }
        dp[u][v][open - close] = rslt;
        return rslt;
    }
}
```