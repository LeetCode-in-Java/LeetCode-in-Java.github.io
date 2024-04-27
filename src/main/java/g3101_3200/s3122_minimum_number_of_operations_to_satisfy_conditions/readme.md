[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3122\. Minimum Number of Operations to Satisfy Conditions

Medium

You are given a 2D matrix `grid` of size `m x n`. In one **operation**, you can change the value of **any** cell to **any** non-negative number. You need to perform some **operations** such that each cell `grid[i][j]` is:

*   Equal to the cell below it, i.e. `grid[i][j] == grid[i + 1][j]` (if it exists).
*   Different from the cell to its right, i.e. `grid[i][j] != grid[i][j + 1]` (if it exists).

Return the **minimum** number of operations needed.

**Example 1:**

**Input:** grid = \[\[1,0,2],[1,0,2]]

**Output:** 0

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/15/examplechanged.png)**

All the cells in the matrix already satisfy the properties.

**Example 2:**

**Input:** grid = \[\[1,1,1],[0,0,0]]

**Output:** 3

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/03/27/example21.png)**

The matrix becomes `[[1,0,1],[1,0,1]]` which satisfies the properties, by doing these 3 operations:

*   Change `grid[1][0]` to 1.
*   Change `grid[0][1]` to 0.
*   Change `grid[1][2]` to 1.

**Example 3:**

**Input:** grid = \[\[1],[2],[3]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/03/31/changed.png)

There is a single column. We can change the value to 1 in each cell using 2 operations.

**Constraints:**

*   `1 <= n, m <= 1000`
*   `0 <= grid[i][j] <= 9`

## Solution

```java
public class Solution {
    public int minimumOperations(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int[][] dp = new int[m][10];
        int[][] cnt = new int[m][10];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; j++) {
                cnt[j][grid[i][j]]++;
            }
        }
        int first = Integer.MAX_VALUE;
        int second = Integer.MAX_VALUE;
        int firstId = -1;
        int secondId = -1;
        for (int i = 0; i < 10; i++) {
            dp[0][i] = n - cnt[0][i];
            if (dp[0][i] <= first) {
                second = first;
                first = dp[0][i];
                secondId = firstId;
                firstId = i;
            } else if (dp[0][i] < second) {
                second = dp[0][i];
                secondId = i;
            }
        }
        for (int j = 1; j < m; ++j) {
            int lastFirstId = firstId;
            int lastSecondId = secondId;
            first = second = Integer.MAX_VALUE;
            firstId = secondId = -1;
            for (int i = 0; i < 10; ++i) {
                int tmp;
                int fix = n - cnt[j][i];
                if (i == lastFirstId) {
                    tmp = fix + dp[j - 1][lastSecondId];
                } else {
                    tmp = fix + dp[j - 1][lastFirstId];
                }
                if (tmp <= first) {
                    second = first;
                    first = tmp;
                    secondId = firstId;
                    firstId = i;
                } else if (tmp < second) {
                    second = tmp;
                    secondId = i;
                }
                dp[j][i] = tmp;
            }
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < 10; ++i) {
            ans = Math.min(ans, dp[m - 1][i]);
        }
        return ans;
    }
}
```