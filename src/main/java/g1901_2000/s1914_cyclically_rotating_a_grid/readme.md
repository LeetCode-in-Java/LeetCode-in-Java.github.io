[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1914\. Cyclically Rotating a Grid

Medium

You are given an `m x n` integer matrix `grid`, where `m` and `n` are both **even** integers, and an integer `k`.

The matrix is composed of several layers, which is shown in the below image, where each color is its own layer:

![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid.png)

A cyclic rotation of the matrix is done by cyclically rotating **each layer** in the matrix. To cyclically rotate a layer once, each element in the layer will take the place of the adjacent element in the **counter-clockwise** direction. An example rotation is shown below:

![](https://assets.leetcode.com/uploads/2021/06/22/explanation_grid.jpg)

Return _the matrix after applying_ `k` _cyclic rotations to it_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/19/rod2.png)

**Input:** grid = \[\[40,10],[30,20]], k = 1

**Output:** [[10,20],[40,30]]

**Explanation:** The figures above represent the grid at every state.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid5.png)** **![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid6.png)** **![](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid7.png)**

**Input:** grid = \[\[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], k = 2

**Output:** [[3,4,8,12],[2,11,10,16],[1,7,6,15],[5,9,13,14]]

**Explanation:** The figures above represent the grid at every state.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `2 <= m, n <= 50`
*   Both `m` and `n` are **even** integers.
*   `1 <= grid[i][j] <= 5000`
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int[][] rotateGrid(int[][] grid, int k) {
        rotateInternal(grid, 0, grid[0].length - 1, 0, grid.length - 1, k);
        return grid;
    }

    private void rotateInternal(int[][] grid, int left, int right, int up, int bottom, int k) {
        if (left > right || up > bottom) {
            return;
        }
        int loopLen = (right - left + 1) * 2 + (bottom - up + 1) * 2 - 4;
        int realK = k % loopLen;
        if (realK != 0) {
            rotateLayer(grid, left, right, up, bottom, realK);
        }
        rotateInternal(grid, left + 1, right - 1, up + 1, bottom - 1, k);
    }

    private void rotateLayer(int[][] grid, int left, int right, int up, int bottom, int k) {
        int[] startPoint = new int[] {up, left};
        int loopLen = (right - left + 1) * 2 + (bottom - up + 1) * 2 - 4;
        int[] arr = new int[loopLen];
        int idx = 0;
        int[] currPoint = startPoint;
        int[] startPointAfterRotation = null;

        while (idx < arr.length) {
            arr[idx] = grid[currPoint[0]][currPoint[1]];
            idx++;
            currPoint = getNextPosCC(left, right, up, bottom, currPoint);
            if (idx == k) {
                startPointAfterRotation = currPoint;
            }
        }

        idx = 0;
        currPoint = startPointAfterRotation;
        if (currPoint != null) {
            while (idx < arr.length) {
                grid[currPoint[0]][currPoint[1]] = arr[idx];
                idx++;
                currPoint = getNextPosCC(left, right, up, bottom, currPoint);
            }
        }
    }

    private int[] getNextPosCC(int left, int right, int up, int bottom, int[] curr) {
        int x = curr[0];
        int y = curr[1];
        if (x == up && y > left) {
            return new int[] {x, y - 1};
        } else if (y == left && x < bottom) {
            return new int[] {x + 1, y};
        } else if (x == bottom && y < right) {
            return new int[] {x, y + 1};
        } else {
            return new int[] {x - 1, y};
        }
    }
}
```