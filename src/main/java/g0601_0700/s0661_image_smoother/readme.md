[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 661\. Image Smoother

Easy

An **image smoother** is a filter of the size `3 x 3` that can be applied to each cell of an image by rounding down the average of the cell and the eight surrounding cells (i.e., the average of the nine cells in the blue smoother). If one or more of the surrounding cells of a cell is not present, we do not consider it in the average (i.e., the average of the four cells in the red smoother).

![](https://assets.leetcode.com/uploads/2021/05/03/smoother-grid.jpg)

Given an `m x n` integer matrix `img` representing the grayscale of an image, return _the image after applying the smoother on each cell of it_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/smooth-grid.jpg)

**Input:** img = \[\[1,1,1],[1,0,1],[1,1,1]]

**Output:** [[0,0,0],[0,0,0],[0,0,0]]

**Explanation:** 

For the points (0,0), (0,2), (2,0), (2,2): floor(3/4) = floor(0.75) = 0 

For the points (0,1), (1,0), (1,2), (2,1): floor(5/6) = floor(0.83333333) = 0 

For the point (1,1): floor(8/9) = floor(0.88888889) = 0

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/03/smooth2-grid.jpg)

**Input:** img = \[\[100,200,100],[200,50,200],[100,200,100]]

**Output:** [[137,141,137],[141,138,141],[137,141,137]]

**Explanation:** 

For the points (0,0), (0,2), (2,0), (2,2): floor((100+200+200+50)/4) = floor(137.5) = 137 

For the points (0,1), (1,0), (1,2), (2,1): floor((200+200+50+200+100+100)/6) = floor(141.666667) = 141 

For the point (1,1): floor((50+200+200+200+200+100+100+100+100)/9) = floor(138.888889) = 138

**Constraints:**

*   `m == img.length`
*   `n == img[i].length`
*   `1 <= m, n <= 200`
*   `0 <= img[i][j] <= 255`

## Solution

```java
public class Solution {
    public int[][] imageSmoother(int[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return matrix;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] result = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                bfs(matrix, i, j, result, m, n);
            }
        }
        return result;
    }

    private void bfs(int[][] matrix, int i, int j, int[][] result, int m, int n) {
        int sum = matrix[i][j];
        int denominator = 1;
        if (j + 1 < n) {
            sum += matrix[i][j + 1];
            denominator++;
        }
        if (i + 1 < m && j + 1 < n) {
            sum += matrix[i + 1][j + 1];
            denominator++;
        }
        if (i + 1 < m) {
            sum += matrix[i + 1][j];
            denominator++;
        }
        if (i + 1 < m && j - 1 >= 0) {
            sum += matrix[i + 1][j - 1];
            denominator++;
        }
        if (j - 1 >= 0) {
            sum += matrix[i][j - 1];
            denominator++;
        }
        if (i - 1 >= 0 && j - 1 >= 0) {
            sum += matrix[i - 1][j - 1];
            denominator++;
        }
        if (i - 1 >= 0) {
            sum += matrix[i - 1][j];
            denominator++;
        }
        if (i - 1 >= 0 && j + 1 < n) {
            sum += matrix[i - 1][j + 1];
            denominator++;
        }
        result[i][j] = sum / denominator;
    }
}
```