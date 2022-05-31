## 1314\. Matrix Block Sum

Medium

Given a `m x n` matrix `mat` and an integer `k`, return _a matrix_ `answer` _where each_ `answer[i][j]` _is the sum of all elements_ `mat[r][c]` _for_:

*   `i - k <= r <= i + k,`
*   `j - k <= c <= j + k`, and
*   `(r, c)` is a valid position in the matrix.

**Example 1:**

**Input:** mat = [[1,2,3],[4,5,6],[7,8,9]], k = 1

**Output:** [[12,21,16],[27,45,33],[24,39,28]]

**Example 2:**

**Input:** mat = [[1,2,3],[4,5,6],[7,8,9]], k = 2

**Output:** [[45,45,45],[45,45,45],[45,45,45]]

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= m, n, k <= 100`
*   `1 <= mat[i][j] <= 100`

## Solution

```java
public class Solution {
    public int[][] matrixBlockSum(int[][] mat, int k) {
        int rows = mat.length;
        int cols = mat[0].length;
        int[][] prefixSum = new int[rows + 1][cols + 1];
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                prefixSum[i][j] =
                        mat[i - 1][j - 1]
                                - prefixSum[i - 1][j - 1]
                                + prefixSum[i - 1][j]
                                + prefixSum[i][j - 1];
            }
        }

        int[][] result = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                int iMin = Math.max(i - k, 0);
                int iMax = Math.min(i + k, rows - 1);
                int jMin = Math.max(j - k, 0);
                int jMax = Math.min(j + k, cols - 1);
                result[i][j] =
                        prefixSum[iMin][jMin]
                                + prefixSum[iMax + 1][jMax + 1]
                                - prefixSum[iMax + 1][jMin]
                                - prefixSum[iMin][jMax + 1];
            }
        }
        return result;
    }
}
```