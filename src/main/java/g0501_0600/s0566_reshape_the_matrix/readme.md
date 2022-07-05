[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 566\. Reshape the Matrix

Easy

In MATLAB, there is a handy function called `reshape` which can reshape an `m x n` matrix into a new one with a different size `r x c` keeping its original data.

You are given an `m x n` matrix `mat` and two integers `r` and `c` representing the number of rows and the number of columns of the wanted reshaped matrix.

The reshaped matrix should be filled with all the elements of the original matrix in the same row-traversing order as they were.

If the `reshape` operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/reshape1-grid.jpg)

**Input:** mat = \[\[1,2],[3,4]], r = 1, c = 4

**Output:** [[1,2,3,4]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/reshape2-grid.jpg)

**Input:** mat = \[\[1,2],[3,4]], r = 2, c = 4

**Output:** [[1,2],[3,4]]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n <= 100`
*   `-1000 <= mat[i][j] <= 1000`
*   `1 <= r, c <= 300`

## Solution

```java
public class Solution {
    public int[][] matrixReshape(int[][] mat, int r, int c) {
        if ((mat.length * mat[0].length) != r * c) {
            return mat;
        }
        int p = 0;
        int[] flatArr = new int[mat.length * mat[0].length];
        for (int[] ints : mat) {
            for (int anInt : ints) {
                flatArr[p++] = anInt;
            }
        }
        int[][] ansMat = new int[r][c];
        int k = 0;
        for (int i = 0; i < ansMat.length; i++) {
            for (int j = 0; j < ansMat[i].length; j++) {
                ansMat[i][j] = flatArr[k++];
            }
        }
        return ansMat;
    }
}
```