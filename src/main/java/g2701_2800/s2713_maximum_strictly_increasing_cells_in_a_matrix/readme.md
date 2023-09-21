[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2713\. Maximum Strictly Increasing Cells in a Matrix

Hard

Given a **1-indexed** `m x n` integer matrix `mat`, you can select any cell in the matrix as your **starting cell**.

From the starting cell, you can move to any other cell **in the** **same row or column**, but only if the value of the destination cell is **strictly greater** than the value of the current cell. You can repeat this process as many times as possible, moving from cell to cell until you can no longer make any moves.

Your task is to find the **maximum number of cells** that you can visit in the matrix by starting from some cell.

Return _an integer denoting the maximum number of cells that can be visited._

**Example 1:**

**![](https://assets.leetcode.com/uploads/2023/04/23/diag1drawio.png)**

**Input:** mat = \[\[3,1],[3,4]]

**Output:** 2

**Explanation:** The image shows how we can visit 2 cells starting from row 1, column 2. It can be shown that we cannot visit more than 2 cells no matter where we start from, so the answer is 2.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2023/04/23/diag3drawio.png)**

**Input:** mat = \[\[1,1],[1,1]]

**Output:** 1

**Explanation:** Since the cells must be strictly increasing, we can only visit one cell in this example.

**Example 3:**

**![](https://assets.leetcode.com/uploads/2023/04/23/diag4drawio.png)**

**Input:** mat = \[\[3,1,6],[-9,5,7]]

**Output:** 4

**Explanation:** The image above shows how we can visit 4 cells starting from row 2, column 1. It can be shown that we cannot visit more than 4 cells no matter where we start from, so the answer is 4.

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= mat[i][j] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.concurrent.atomic.AtomicInteger;

public class Solution {
    public int maxIncreasingCells(int[][] mat) {
        int n = mat.length;
        int m = mat[0].length;
        Map<Integer, List<int[]>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                int val = mat[i][j];
                map.computeIfAbsent(val, k -> new ArrayList<>()).add(new int[] {i, j});
            }
        }
        int[][] memo = new int[n][m];
        int[] res = new int[n + m];
        AtomicInteger max = new AtomicInteger();
        map.keySet().stream()
                .sorted()
                .forEach(
                        a -> {
                            for (int[] pos : map.get(a)) {
                                int i = pos[0];
                                int j = pos[1];
                                memo[i][j] = Math.max(res[i], res[n + j]) + 1;
                                max.set(Math.max(max.get(), memo[i][j]));
                            }
                            for (int[] pos : map.get(a)) {
                                int i = pos[0];
                                int j = pos[1];
                                res[n + j] = Math.max(res[n + j], memo[i][j]);
                                res[i] = Math.max(res[i], memo[i][j]);
                            }
                        });
        return max.get();
    }
}
```