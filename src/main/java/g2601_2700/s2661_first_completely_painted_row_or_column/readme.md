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
import java.util.HashMap;

public class Solution {
    public int firstCompleteIndex(int[] arr, int[][] mat) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < arr.length; i++) {
            map.put(arr[i], i);
        }

        for (int i = 0; i < mat.length; i++) {
            for (int j = 0; j < mat[0].length; j++) {
                mat[i][j] = map.get(mat[i][j]);
            }
        }

        int ans = Integer.MAX_VALUE;
        for (int[] ints : mat) {
            int max = 0;
            for (int j = 0; j < mat[0].length; j++) {
                max = Math.max(max, ints[j]);
            }
            ans = Math.min(ans, max);
        }

        for (int i = 0; i < mat[0].length; i++) {
            int max = 0;
            for (int[] ints : mat) {
                max = Math.max(max, ints[i]);
            }
            ans = Math.min(ans, max);
        }

        return ans;
    }
}
```