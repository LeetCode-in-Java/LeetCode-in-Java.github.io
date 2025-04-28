[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3529\. Count Cells in Overlapping Horizontal and Vertical Substrings

Medium

You are given an `m x n` matrix `grid` consisting of characters and a string `pattern`.

A **horizontal substring** is a contiguous sequence of characters read from left to right. If the end of a row is reached before the substring is complete, it wraps to the first column of the next row and continues as needed. You do **not** wrap from the bottom row back to the top.

A **vertical substring** is a contiguous sequence of characters read from top to bottom. If the bottom of a column is reached before the substring is complete, it wraps to the first row of the next column and continues as needed. You do **not** wrap from the last column back to the first.

Count the number of cells in the matrix that satisfy the following condition:

*   The cell must be part of **at least** one horizontal substring and **at least** one vertical substring, where **both** substrings are equal to the given `pattern`.

Return the count of these cells.

**Example 1:**

![](https://assets.leetcode.com/uploads/2025/03/03/gridtwosubstringsdrawio.png)

**Input:** grid = \[\["a","a","c","c"],["b","b","b","c"],["a","a","b","a"],["c","a","a","c"],["a","a","c","c"]], pattern = "abaca"

**Output:** 1

**Explanation:**

The pattern `"abaca"` appears once as a horizontal substring (colored blue) and once as a vertical substring (colored red), intersecting at one cell (colored purple).

**Example 2:**

![](https://assets.leetcode.com/uploads/2025/03/03/gridexample2fixeddrawio.png)

**Input:** grid = \[\["c","a","a","a"],["a","a","b","a"],["b","b","a","a"],["a","a","b","a"]], pattern = "aba"

**Output:** 4

**Explanation:**

The cells colored above are all part of at least one horizontal and one vertical substring matching the pattern `"aba"`.

**Example 3:**

**Input:** grid = \[\["a"]], pattern = "a"

**Output:** 1

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 1000`
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   `1 <= pattern.length <= m * n`
*   `grid` and `pattern` consist of only lowercase English letters.

## Solution

```java
public class Solution {
    public int countCells(char[][] grid, String pattern) {
        int k = pattern.length();
        int[] lps = makeLps(pattern);
        int m = grid.length;
        int n = grid[0].length;
        int[][] horiPats = new int[m][n];
        int[][] vertPats = new int[m][n];
        int i = 0;
        int j = 0;
        while (i < m * n) {
            if (grid[i / n][i % n] == pattern.charAt(j)) {
                i++;
                if (++j == k) {
                    int d = i - j;
                    horiPats[d / n][d % n]++;
                    if (i < m * n) {
                        horiPats[i / n][i % n]--;
                    }
                    j = lps[j - 1];
                }
            } else if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
        i = 0;
        j = 0;
        // now do vert pattern, use i = 0 to m*n -1 but instead index as grid[i % m][i/m]
        while (i < m * n) {
            if (grid[i % m][i / m] == pattern.charAt(j)) {
                i++;
                if (++j == k) {
                    int d = i - j;
                    vertPats[d % m][d / m]++;
                    if (i < m * n) {
                        vertPats[i % m][i / m]--;
                    }
                    j = lps[j - 1];
                }
            } else if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
        for (i = 1; i < m * n; i++) {
            vertPats[i % m][i / m] += vertPats[(i - 1) % m][(i - 1) / m];
            horiPats[i / n][i % n] += horiPats[(i - 1) / n][(i - 1) % n];
        }
        int res = 0;
        for (i = 0; i < m; i++) {
            for (j = 0; j < n; j++) {
                if (horiPats[i][j] > 0 && vertPats[i][j] > 0) {
                    res++;
                }
            }
        }
        return res;
    }

    private int[] makeLps(String pattern) {
        int n = pattern.length();
        int[] lps = new int[n];
        int len = 0;
        int i = 1;
        lps[0] = 0;
        while (i < n) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                lps[i++] = ++len;
            } else if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i++] = 0;
            }
        }
        return lps;
    }
}
```