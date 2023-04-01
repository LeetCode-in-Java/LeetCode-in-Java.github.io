[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1260\. Shift 2D Grid

Easy

Given a 2D `grid` of size `m x n` and an integer `k`. You need to shift the `grid` `k` times.

In one shift operation:

*   Element at `grid[i][j]` moves to `grid[i][j + 1]`.
*   Element at `grid[i][n - 1]` moves to `grid[i + 1][0]`.
*   Element at `grid[m - 1][n - 1]` moves to `grid[0][0]`.

Return the _2D grid_ after applying shift operation `k` times.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/11/05/e1.png)

**Input:** `grid` = \[\[1,2,3],[4,5,6],[7,8,9]], k = 1

**Output:** [[9,1,2],[3,4,5],[6,7,8]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/05/e2.png)

**Input:** `grid` = \[\[3,8,1,9],[19,7,2,5],[4,6,11,10],[12,0,21,13]], k = 4

**Output:** [[12,0,21,13],[3,8,1,9],[19,7,2,5],[4,6,11,10]]

**Example 3:**

**Input:** `grid` = \[\[1,2,3],[4,5,6],[7,8,9]], k = 9

**Output:** [[1,2,3],[4,5,6],[7,8,9]]

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m <= 50`
*   `1 <= n <= 50`
*   `-1000 <= grid[i][j] <= 1000`
*   `0 <= k <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    public List<List<Integer>> shiftGrid(int[][] grid, int k) {
        if (grid == null) {
            return Collections.emptyList();
        }
        int[] flat = new int[grid.length * grid[0].length];
        int index = 0;
        for (int[] ints : grid) {
            for (int j = 0; j < grid[0].length; ++j) {
                flat[index++] = ints[j];
            }
        }
        int mode = k % flat.length;
        int readingIndex = flat.length - mode;
        if (readingIndex == flat.length) {
            readingIndex = 0;
        }
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 0; i < grid.length; ++i) {
            List<Integer> eachRow = new ArrayList<>();
            for (int j = 0; j < grid[0].length; ++j) {
                eachRow.add(flat[readingIndex++]);
                if (readingIndex == flat.length) {
                    readingIndex = 0;
                }
            }
            result.add(eachRow);
        }
        return result;
    }
}
```