[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1738\. Find Kth Largest XOR Coordinate Value

Medium

You are given a 2D `matrix` of size `m x n`, consisting of non-negative integers. You are also given an integer `k`.

The **value** of coordinate `(a, b)` of the matrix is the XOR of all `matrix[i][j]` where `0 <= i <= a < m` and `0 <= j <= b < n` **(0-indexed)**.

Find the <code>k<sup>th</sup></code> largest value **(1-indexed)** of all the coordinates of `matrix`.

**Example 1:**

**Input:** matrix = \[\[5,2],[1,6]], k = 1

**Output:** 7

**Explanation:** The value of coordinate (0,1) is 5 XOR 2 = 7, which is the largest value.

**Example 2:**

**Input:** matrix = \[\[5,2],[1,6]], k = 2

**Output:** 5

**Explanation:** The value of coordinate (0,0) is 5 = 5, which is the 2nd largest value.

**Example 3:**

**Input:** matrix = \[\[5,2],[1,6]], k = 3

**Output:** 4

**Explanation:** The value of coordinate (1,0) is 5 XOR 1 = 4, which is the 3rd largest value.

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 1000`
*   <code>0 <= matrix[i][j] <= 10<sup>6</sup></code>
*   `1 <= k <= m * n`

## Solution

```java
public class Solution {
    public int kthLargestValue(int[][] matrix, int k) {
        int t = 0;
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] prefixXor = new int[rows + 1][cols + 1];
        int[] array = new int[rows * cols];

        for (int r = 1; r <= rows; r++) {
            for (int c = 1; c <= cols; c++) {
                prefixXor[r][c] =
                        matrix[r - 1][c - 1]
                                ^ prefixXor[r - 1][c]
                                ^ prefixXor[r][c - 1]
                                ^ prefixXor[r - 1][c - 1];
                array[t++] = prefixXor[r][c];
            }
        }

        int target = array.length - k;
        quickSelect(array, 0, array.length - 1, target);
        return array[target];
    }

    private int quickSelect(int[] array, int left, int right, int target) {
        if (left == right) {
            return left;
        }

        int pivot = array[right];
        int j = left;
        int k = right - 1;
        while (j <= k) {
            if (array[j] < pivot) {
                j++;
            } else if (array[k] > pivot) {
                k--;
            } else {
                swap(array, j++, k--);
            }
        }
        swap(array, j, right);
        if (j == target) {
            return j;
        } else if (j > target) {
            return quickSelect(array, left, j - 1, target);
        } else {
            return quickSelect(array, j + 1, right, target);
        }
    }

    private void swap(int[] array, int i, int j) {
        int tmp = array[i];
        array[i] = array[j];
        array[j] = tmp;
    }
}
```