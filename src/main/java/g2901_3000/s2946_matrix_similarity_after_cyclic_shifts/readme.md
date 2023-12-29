[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2946\. Matrix Similarity After Cyclic Shifts

Easy

You are given a **0-indexed** `m x n` integer matrix `mat` and an integer `k`. You have to cyclically **right** shift **odd** indexed rows `k` times and cyclically **left** shift **even** indexed rows `k` times.

Return `true` _if the initial and final matrix are exactly the same and_ `false` _otherwise._

**Example 1:**

**Input:** mat = \[\[1,2,1,2],[5,5,5,5],[6,3,6,3]], k = 2

**Output:** true

**Explanation:** ![](https://assets.leetcode.com/uploads/2023/10/29/similarmatrix.png) Initially, the matrix looks like the first figure. Second figure represents the state of the matrix after one right and left cyclic shifts to even and odd indexed rows. Third figure is the final state of the matrix after two cyclic shifts which is similar to the initial matrix. Therefore, return true.

**Example 2:**

**Input:** mat = \[\[2,2],[2,2]], k = 3

**Output:** true

**Explanation:** As all the values are equal in the matrix, even after performing cyclic shifts the matrix will remain the same. Therefeore, we return true.

**Example 3:**

**Input:** mat = \[\[1,2]], k = 1

**Output:** false

**Explanation:** After one cyclic shift, mat = \[\[2,1]] which is not equal to the initial matrix. Therefore we return false.

**Constraints:**

*   `1 <= mat.length <= 25`
*   `1 <= mat[i].length <= 25`
*   `1 <= mat[i][j] <= 25`
*   `1 <= k <= 50`

## Solution

```java
public class Solution {
    public boolean areSimilar(int[][] mat, int k) {
        int m = mat.length;
        int n = mat[0].length;
        k %= n;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if ((i & 1) != 0) {
                    if (mat[i][j] != mat[i][(j - k + n) % n]) {
                        return false;
                    }
                } else {
                    if (mat[i][j] != mat[i][(j + k) % n]) {
                        return false;
                    }
                }
            }
        }
        return true;
    }
}
```