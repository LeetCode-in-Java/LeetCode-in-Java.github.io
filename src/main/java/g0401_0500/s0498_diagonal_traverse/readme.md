[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 498\. Diagonal Traverse

Medium

Given an `m x n` matrix `mat`, return _an array of all the elements of the array in a diagonal order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)

**Input:** mat = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [1,2,4,7,5,3,6,8,9]

**Example 2:**

**Input:** mat = \[\[1,2],[3,4]]

**Output:** [1,2,3,4]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   <code>1 <= m, n <= 10<sup>4</sup></code>
*   <code>1 <= m * n <= 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= mat[i][j] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int[] output = new int[m * n];
        int idx = 0;
        for (int diag = 0; diag <= m + n - 2; ++diag) {
            if (diag % 2 == 0) {
                for (int k = Math.max(0, diag - m + 1); k <= Math.min(diag, n - 1); ++k) {
                    output[idx++] = mat[diag - k][k];
                }
            } else {
                for (int k = Math.max(0, diag - n + 1); k <= Math.min(diag, m - 1); ++k) {
                    output[idx++] = mat[k][diag - k];
                }
            }
        }
        return output;
    }
}
```