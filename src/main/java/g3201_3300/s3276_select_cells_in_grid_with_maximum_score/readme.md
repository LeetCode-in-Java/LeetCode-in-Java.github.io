[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3276\. Select Cells in Grid With Maximum Score

Hard

You are given a 2D matrix `grid` consisting of positive integers.

You have to select _one or more_ cells from the matrix such that the following conditions are satisfied:

*   No two selected cells are in the **same** row of the matrix.
*   The values in the set of selected cells are **unique**.

Your score will be the **sum** of the values of the selected cells.

Return the **maximum** score you can achieve.

**Example 1:**

**Input:** grid = \[\[1,2,3],[4,3,2],[1,1,1]]

**Output:** 8

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/29/grid1drawio.png)

We can select the cells with values 1, 3, and 4 that are colored above.

**Example 2:**

**Input:** grid = \[\[8,7,6],[8,3,2]]

**Output:** 15

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/29/grid8_8drawio.png)

We can select the cells with values 7 and 8 that are colored above.

**Constraints:**

*   `1 <= grid.length, grid[i].length <= 10`
*   `1 <= grid[i][j] <= 100`

## Solution

```java
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int maxScore(List<List<Integer>> grid) {
        int n = grid.size();
        int m = grid.get(0).size();
        int[][] arr = new int[n * m][2];
        for (int i = 0; i < n; i++) {
            List<Integer> l = grid.get(i);
            for (int j = 0; j < l.size(); j++) {
                arr[i * m + j][0] = l.get(j);
                arr[i * m + j][1] = i;
            }
        }
        Arrays.sort(arr, (a, b) -> b[0] - a[0]);
        int[] dp = new int[1 << n];
        int i = 0;
        while (i < arr.length) {
            boolean[] seen = new boolean[n];
            seen[arr[i][1]] = true;
            int v = arr[i][0];
            i++;
            while (i < arr.length && arr[i][0] == v) {
                seen[arr[i][1]] = true;
                i++;
            }
            int[] next = Arrays.copyOf(dp, dp.length);
            for (int j = 0; j < n; j++) {
                if (seen[j]) {
                    int and = ((1 << n) - 1) ^ (1 << j);
                    for (int k = and; k > 0; k = (k - 1) & and) {
                        next[k | (1 << j)] = Math.max(next[k | (1 << j)], dp[k] + v);
                    }
                    next[1 << j] = Math.max(next[1 << j], v);
                }
            }
            dp = next;
        }
        return dp[dp.length - 1];
    }
}
```