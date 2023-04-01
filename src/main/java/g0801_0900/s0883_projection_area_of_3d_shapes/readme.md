[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 883\. Projection Area of 3D Shapes

Easy

You are given an `n x n` `grid` where we place some `1 x 1 x 1` cubes that are axis-aligned with the `x`, `y`, and `z` axes.

Each value `v = grid[i][j]` represents a tower of `v` cubes placed on top of the cell `(i, j)`.

We view the projection of these cubes onto the `xy`, `yz`, and `zx` planes.

A **projection** is like a shadow, that maps our **3-dimensional** figure to a **2-dimensional** plane. We are viewing the "shadow" when looking at the cubes from the top, the front, and the side.

Return _the total area of all three projections_.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/02/shadow.png)

**Input:** grid = \[\[1,2],[3,4]]

**Output:** 17

**Explanation:** Here are the three projections ("shadows") of the shape made with each axis-aligned plane.

**Example 2:**

**Input:** grid = \[\[2]]

**Output:** 5

**Example 3:**

**Input:** grid = \[\[1,0],[0,2]]

**Output:** 8

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `1 <= n <= 50`
*   `0 <= grid[i][j] <= 50`

## Solution

```java
public class Solution {
    public int projectionArea(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int sum = n * m;
        int count = 0;
        for (int[] ints : grid) {
            int max = Integer.MIN_VALUE;
            for (int j = 0; j < m; j++) {
                if (ints[j] == 0) {
                    count++;
                }
                if (max < ints[j]) {
                    max = ints[j];
                }
            }
            sum += max;
        }
        for (int i = 0; i < n; i++) {
            int max = Integer.MIN_VALUE;
            for (int j = 0; j < m; j++) {
                if (max < grid[j][i]) {
                    max = grid[j][i];
                }
            }
            sum += max;
        }
        return sum - count;
    }
}
```