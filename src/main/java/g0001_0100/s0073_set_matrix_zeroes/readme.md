[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 73\. Set Matrix Zeroes

Medium

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s, and return _the matrix_.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

**Input:** matrix = \[\[1,1,1],[1,0,1],[1,1,1]]

**Output:** [[1,0,1],[0,0,0],[1,0,1]] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

**Input:** matrix = \[\[0,1,2,0],[3,4,5,2],[1,3,1,5]]

**Output:** [[0,0,0,0],[0,4,5,0],[0,3,1,0]] 

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[0].length`
*   `1 <= m, n <= 200`
*   <code>-2<sup>31</sup> <= matrix[i][j] <= 2<sup>31</sup> - 1</code>

**Follow up:**

*   A straightforward solution using `O(mn)` space is probably a bad idea.
*   A simple improvement uses `O(m + n)` space, but still not the best solution.
*   Could you devise a constant space solution?

## Solution

```java
public class Solution {
    // Approach: Use first row and first column for storing whether in future
    //           the entire row or column needs to be marked 0
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        boolean row0 = false;
        boolean col0 = false;
        // Check if 0th col needs to be market all 0s in future
        for (int[] ints : matrix) {
            if (ints[0] == 0) {
                col0 = true;
                break;
            }
        }
        // Check if 0th row needs to be market all 0s in future
        for (int i = 0; i < n; i++) {
            if (matrix[0][i] == 0) {
                row0 = true;
                break;
            }
        }
        // Store the signals in 0th row and column
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        // Mark 0 for all cells based on signal from 0th row and 0th column
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        // Set 0th column
        for (int i = 0; i < m; i++) {
            if (col0) {
                matrix[i][0] = 0;
            }
        }
        // Set 0th row
        for (int i = 0; i < n; i++) {
            if (row0) {
                matrix[0][i] = 0;
            }
        }
    }
}
```

**Time Complexity (Big O Time):**

1. The program first iterates through the matrix to determine whether the first row and first column need to be marked as zero (i.e., `row0` and `col0` flags).

2. Then, it iterates through the entire matrix twice:
   - The first iteration (nested loops) is used to identify cells that need to be marked as zero based on the conditions. This loop runs for `(m-1) * (n-1)` iterations, where `m` is the number of rows, and `n` is the number of columns.
   - The second iteration (nested loops) is used to actually set the cells to zero based on the signals stored in the first row and first column. This loop also runs for `(m-1) * (n-1)` iterations.

3. The program also sets the first row and first column to zero if necessary.

4. Therefore, the overall time complexity of the program is O(m * n), where `m` is the number of rows, and `n` is the number of columns.

**Space Complexity (Big O Space):**

1. The program uses only a few additional boolean variables (`row0` and `col0`) and integer variables (loop counters). These variables consume constant space, which does not depend on the size of the input matrix.

2. The program modifies the input matrix in-place without using any additional data structures. Therefore, the space complexity of the program is O(1) or constant space.

In summary, the time complexity of the provided program is O(m * n), and the space complexity is O(1). The program efficiently sets rows and columns of a matrix to zero based on specific conditions while using minimal extra space.
