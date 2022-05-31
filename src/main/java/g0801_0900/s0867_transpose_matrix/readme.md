## 867\. Transpose Matrix

Easy

Given a 2D integer array `matrix`, return _the **transpose** of_ `matrix`.

The **transpose** of a matrix is the matrix flipped over its main diagonal, switching the matrix's row and column indices.

![](https://assets.leetcode.com/uploads/2021/02/10/hint_transpose.png)

**Example 1:**

**Input:** matrix = [[1,2,3],[4,5,6],[7,8,9]]

**Output:** [[1,4,7],[2,5,8],[3,6,9]]

**Example 2:**

**Input:** matrix = [[1,2,3],[4,5,6]]

**Output:** [[1,4],[2,5],[3,6]]

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 1000`
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= matrix[i][j] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int[][] transpose(int[][] input) {
        int[][] output = new int[input[0].length][input.length];
        for (int i = 0, b = 0; i < input.length; i++, b++) {
            for (int j = 0, a = 0; j < input[0].length; j++, a++) {
                output[a][b] = input[i][j];
            }
        }
        return output;
    }
}
```