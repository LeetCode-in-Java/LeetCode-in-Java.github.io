[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2732\. Find a Good Subset of the Matrix

Hard

You are given a **0-indexed** `m x n` binary matrix `grid`.

Let us call a **non-empty** subset of rows **good** if the sum of each column of the subset is at most half of the length of the subset.

More formally, if the length of the chosen subset of rows is `k`, then the sum of each column should be at most `floor(k / 2)`.

Return _an integer array that contains row indices of a good subset sorted in **ascending** order._

If there are multiple good subsets, you can return any of them. If there are no good subsets, return an empty array.

A **subset** of rows of the matrix `grid` is any matrix that can be obtained by deleting some (possibly none or all) rows from `grid`.

**Example 1:**

**Input:** grid = \[\[0,1,1,0],[0,0,0,1],[1,1,1,1]]

**Output:** [0,1]

**Explanation:** We can choose the 0<sup>th</sup> and 1<sup>st</sup> rows to create a good subset of rows. The length of the chosen subset is 2. 
- The sum of the 0<sup>th</sup> column is 0 + 0 = 0, which is at most half of the length of the subset. 
- The sum of the 1<sup>st</sup> column is 1 + 0 = 1, which is at most half of the length of the subset. 
- The sum of the 2<sup>nd</sup> column is 1 + 0 = 1, which is at most half of the length of the subset. 
- The sum of the 3<sup>rd</sup> column is 0 + 1 = 1, which is at most half of the length of the subset.

**Example 2:**

**Input:** grid = \[\[0]]

**Output:** [0]

**Explanation:** We can choose the 0<sup>th</sup> row to create a good subset of rows. The length of the chosen subset is 1. 
- The sum of the 0<sup>th</sup> column is 0, which is at most half of the length of the subset.

**Example 3:**

**Input:** grid = \[\[1,1,1],[1,1,1]]

**Output:** []

**Explanation:** It is impossible to choose any subset of rows to create a good subset.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m <= 10<sup>4</sup></code>
*   `1 <= n <= 5`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public List<Integer> goodSubsetofBinaryMatrix(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        if (m == 1 && sumArray(grid[0]) == 0) {
            return List.of(0);
        }
        Map<Integer, Integer> pos = new HashMap<>();
        for (int i = 0; i < m; i++) {
            for (int mask = 0; mask < (1 << n); mask++) {
                boolean valid = true;
                for (int j = 0; j < n; j++) {
                    if ((mask & (1 << j)) != 0 && grid[i][j] + 1 > 1) {
                        valid = false;
                        break;
                    }
                }
                if (valid && pos.containsKey(mask)) {
                    return List.of(pos.get(mask), i);
                }
            }
            int curr = 0;
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    curr = curr | (1 << j);
                }
            }
            pos.put(curr, i);
        }
        return new ArrayList<>();
    }

    private int sumArray(int[] arr) {
        int sum = 0;
        for (int num : arr) {
            sum += num;
        }
        return sum;
    }
}
```