[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2661\. First Completely Painted Row or Column

Medium

You are given a **0-indexed** integer array `arr`, and an `m x n` integer **matrix** `mat`. `arr` and `mat` both contain **all** the integers in the range `[1, m * n]`.

Go through each index `i` in `arr` starting from index `0` and paint the cell in `mat` containing the integer `arr[i]`.

Return _the smallest index_ `i` _at which either a row or a column will be completely painted in_ `mat`.

**Example 1:**

![](image explanation for example 1)![image explanation for example 1](https://assets.leetcode.com/uploads/2023/01/18/grid1.jpg)

**Input:** arr = [1,3,4,2], mat = \[\[1,4],[2,3]]

**Output:** 2

**Explanation:** The moves are shown in order, and both the first row and second column of the matrix become fully painted at arr[2].

**Example 2:**

![image explanation for example 2](https://assets.leetcode.com/uploads/2023/01/18/grid2.jpg)

**Input:** arr = [2,8,7,4,1,3,5,6,9], mat = \[\[3,2,5],[1,4,6],[8,7,9]]

**Output:** 3

**Explanation:** The second column becomes fully painted at arr[3].

**Constraints:**

*   `m == mat.length`
*   `n = mat[i].length`
*   `arr.length == m * n`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   `1 <= arr[i], mat[r][c] <= m * n`
*   All the integers of `arr` are **unique**.
*   All the integers of `mat` are **unique**.

## Solution

```java
public class Solution {
    public int firstCompleteIndex(int[] arr, int[][] mat) {
        int[] numMapIndex = new int[mat.length * mat[0].length + 1];
        for (int i = 0; i < arr.length; i++) {
            numMapIndex[arr[i]] = i;
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < mat.length; i++) {
            int rowMin = Integer.MIN_VALUE;
            for (int i1 = 0; i1 < mat[i].length; i1++) {
                int index = numMapIndex[mat[i][i1]];
                rowMin = Math.max(rowMin, index);
            }
            ans = Math.min(ans, rowMin);
        }
        for (int i = 0; i < mat[0].length; i++) {
            int colMin = Integer.MIN_VALUE;
            for (int i1 = 0; i1 < mat.length; i1++) {
                int index = numMapIndex[mat[i1][i]];
                colMin = Math.max(colMin, index);
            }
            ans = Math.min(ans, colMin);
        }
        return ans;
    }
}
```