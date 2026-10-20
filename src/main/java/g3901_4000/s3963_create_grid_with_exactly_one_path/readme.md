[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3963\. Create Grid With Exactly One Path

Easy

You are given two integers `m` and `n`, representing the number of rows and columns of a grid.

Construct **any** `m x n` grid consisting only of the characters `'.'` and `'#'`, where:

*   `'.'` represents a free cell.
*   `'#'` represents an obstacle cell.

A **valid path** is a sequence of free cells that:

*   Starts at the top-left cell `(0, 0)`.
*   Ends at the bottom-right cell `(m - 1, n - 1)`.
*   Moves only:
    *   Right, from `(i, j)` to `(i, j + 1)`, or
    *   Down, from `(i, j)` to `(i + 1, j)`.

Return any grid such that there is **exactly one valid path** from the top-left cell to the bottom-right cell.

**Example 1:**

**Input:** m = 2, n = 3

**Output:** ["..#","#.."]

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/05/26/screenshot-2026-05-26-at-61005pm.png)

The only valid path is: `(0,0) → (0,1) → (1,1) → (1,2)`

**Example 2:**

**Input:** m = 3, n = 3

**Output:** ["..#","#..","##."]

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/05/26/screenshot-2026-05-26-at-61129pm.png)

The only valid path is: `(0,0) → (0,1) → (1,1) → (1,2) → (2,2)`

**Example 3:**

**Input:** m = 1, n = 4

**Output:** ["...."]

**Explanation:**

The only valid path is: `(0,0) → (0,1) → (0,2) → (0,3)`

**Constraints:**

*   `1 <= m, n <= 25`

## Solution

```java
public class Solution {
    public String[] createGrid(int m, int n) {
        var res = new String[m];
        var sb = new StringBuilder();
        sb.repeat(".", Math.max(0, n));
        res[0] = sb.toString();
        sb = new StringBuilder();
        sb.repeat("#", Math.max(0, n - 1));
        sb.append(".");
        for (var i = 1; i < m; i++) {
            res[i] = sb.toString();
        }
        return res;
    }
}
```