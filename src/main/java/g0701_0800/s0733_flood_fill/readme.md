[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 733\. Flood Fill

Easy

An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.

You are also given three integers `sr`, `sc`, and `newColor`. You should perform a **flood fill** on the image starting from the pixel `image[sr][sc]`.

To perform a **flood fill**, consider the starting pixel, plus any pixels connected **4-directionally** to the starting pixel of the same color as the starting pixel, plus any pixels connected **4-directionally** to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with `newColor`.

Return _the modified image after performing the flood fill_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg)

**Input:** image = \[\[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, newColor = 2

**Output:** [[2,2,2],[2,2,0],[2,0,1]]

**Explanation:** From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color. Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.

**Example 2:**

**Input:** image = \[\[0,0,0],[0,0,0]], sr = 0, sc = 0, newColor = 2

**Output:** [[2,2,2],[2,2,2]]

**Constraints:**

*   `m == image.length`
*   `n == image[i].length`
*   `1 <= m, n <= 50`
*   <code>0 <= image[i][j], newColor < 2<sup>16</sup></code>
*   `0 <= sr < m`
*   `0 <= sc < n`

## Solution

```java
public class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        int o = image[sr][sc];
        helper(image, sr, sc, newColor, o);
        return image;
    }

    private void helper(int[][] img, int r, int c, int n, int o) {
        if (r >= img.length
                || c >= img[0].length
                || r < 0
                || c < 0
                || img[r][c] == n
                || img[r][c] != o) {
            return;
        }
        img[r][c] = n;
        helper(img, r + 1, c, n, o);
        helper(img, r - 1, c, n, o);
        helper(img, r, c + 1, n, o);
        helper(img, r, c - 1, n, o);
    }
}
```