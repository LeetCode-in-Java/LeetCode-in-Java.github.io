[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4016\. Maximum Area of Two Non-Overlapping Square Submatrices

Medium

You are given a 2D integer matrix `mat` of size `m × n`, where:

*   `mat[r][c] == 1` means the cell at row `r` and column `c` is usable.
*   `mat[r][c] == 0` means it is not usable.

Your task is to find **two submatrices** that satisfy the following conditions:

*   Both submatrices must be squares of the same side length `k`.
*   The two submatrices must not share any cell.
*   Each submatrix can only cover cells where `mat[r][c] == 1`.

Return the **maximum possible area** of each of the two squares. If it is not possible to choose two such squares, return 0.

**Example 1:**

![](https://assets.leetcode.com/uploads/2026/06/13/image.png)

**Input:** mat = \[\[1,1,1,0],[1,1,1,1],[0,0,1,1]]

**Output:** 4

**Explanation:**

The largest equal non-overlapping squares have side length `k = 2` with area 4.

*   First square starts at top-left `(0, 0)` and covers cells `(0, 0)`, `(0, 1)`, `(1, 0)`, and `(1, 1)`.
*   Second square starts at top-left `(1, 2)` and covers cells `(1, 2)`, `(1, 3)`, `(2, 2)`, and `(2, 3)`.

Thus, the answer is 4.

**Example 2:**

![](https://assets.leetcode.com/uploads/2026/06/13/screenshot-2026-06-13-at-83728pm.png)

**Input:** mat = \[\[0,1],[1,0]]

**Output:** 1

**Explanation:**

The largest equal non-overlapping squares have side length `k = 1` with area 1.

*   First square starts at top-left `(0, 1)` and covers cell `(0, 1)`.
*   Second square starts at top-left `(1, 0)` and covers cell `(1, 0)`.

Thus, the answer is 1.

**Example 3:**

![](https://assets.leetcode.com/uploads/2026/06/13/screenshot-2026-06-13-at-83751pm.png)

**Input:** mat = \[\[0,0],[0,1]]

**Output:** 0

**Explanation:**

There is only one usable cell, so it is impossible to choose two non-overlapping squares. Thus, the answer is 0.

**Constraints:**

*   `mat.length == m`
*   `mat[i].length == n`
*   `1 <= m, n <= 500`
*   `mat[i][j]` is either 0 or 1.

## Solution

```java
@SuppressWarnings("java:S6885")
public class Solution {
    public int maxArea(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int[][] dp = new int[m][n];
        int[] top = new int[m];
        int[] bottom = new int[m];
        int[] left = new int[n];
        int[] right = new int[n];
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if (mat[r][c] == 0) {
                    continue;
                }
                if (r == 0 || c == 0) {
                    dp[r][c] = 1;
                } else {
                    dp[r][c] = 1 + Math.min(dp[r - 1][c - 1], Math.min(dp[r - 1][c], dp[r][c - 1]));
                }
                int k = dp[r][c];
                top[r] = Math.max(top[r], k);
                left[c] = Math.max(left[c], k);
                bottom[r - k + 1] = Math.max(bottom[r - k + 1], k);

                right[c - k + 1] = Math.max(right[c - k + 1], k);
            }
        }
        for (int r = 1; r < m; r++) {
            top[r] = Math.max(top[r], top[r - 1]);
        }
        for (int c = 1; c < n; c++) {
            left[c] = Math.max(left[c], left[c - 1]);
        }
        for (int r = m - 2; r >= 0; r--) {
            bottom[r] = Math.max(bottom[r], bottom[r + 1]);
        }
        for (int c = n - 2; c >= 0; c--) {
            right[c] = Math.max(right[c], right[c + 1]);
        }
        int best = 0;
        for (int r = 0; r < m - 1; r++) {
            best = Math.max(best, Math.min(top[r], bottom[r + 1]));
        }
        for (int c = 0; c < n - 1; c++) {
            best = Math.max(best, Math.min(left[c], right[c + 1]));
        }
        return best * best;
    }
}
```