[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3212\. Count Submatrices With Equal Frequency of X and Y

Medium

Given a 2D character matrix `grid`, where `grid[i][j]` is either `'X'`, `'Y'`, or `'.'`, return the number of submatrices that contains:

*   `grid[0][0]`
*   an **equal** frequency of `'X'` and `'Y'`.
*   **at least** one `'X'`.

**Example 1:**

**Input:** grid = \[\["X","Y","."],["Y",".","."]]

**Output:** 3

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/06/07/examplems.png)**

**Example 2:**

**Input:** grid = \[\["X","X"],["X","Y"]]

**Output:** 0

**Explanation:**

No submatrix has an equal frequency of `'X'` and `'Y'`.

**Example 3:**

**Input:** grid = \[\[".","."],[".","."]]

**Output:** 0

**Explanation:**

No submatrix has at least one `'X'`.

**Constraints:**

*   `1 <= grid.length, grid[i].length <= 1000`
*   `grid[i][j]` is either `'X'`, `'Y'`, or `'.'`.

## Solution

```java
public class Solution {
    public int numberOfSubmatrices(char[][] grid) {
        int n = grid[0].length;
        int ans = 0;
        int[][] row = new int[n][2];
        for (char[] chars : grid) {
            int[] count = new int[2];
            for (int j = 0; j < n; j++) {
                if (chars[j] != '.') {
                    count[chars[j] - 'X']++;
                }
                row[j][0] += count[0];
                row[j][1] += count[1];
                if (row[j][0] > 0 && row[j][0] == row[j][1]) {
                    ans++;
                }
            }
        }
        return ans;
    }
}
```