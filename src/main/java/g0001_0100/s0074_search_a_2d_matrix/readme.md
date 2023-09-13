[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 74\. Search a 2D Matrix

Medium

Write an efficient algorithm that searches for a value in an `m x n` matrix. This matrix has the following properties:

*   Integers in each row are sorted from left to right.
*   The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

**Input:** matrix = \[\[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3

**Output:** true 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

**Input:** matrix = \[\[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13

**Output:** false 

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 100`
*   <code>-10<sup>4</sup> <= matrix[i][j], target <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int endRow = matrix.length;
        int endCol = matrix[0].length;
        int targetRow = 0;
        boolean result = false;
        for (int i = 0; i < endRow; i++) {
            if (matrix[i][endCol - 1] >= target) {
                targetRow = i;
                break;
            }
        }
        for (int i = 0; i < endCol; i++) {
            if (matrix[targetRow][i] == target) {
                result = true;
                break;
            }
        }
        return result;
    }
}
```

**Time Complexity (Big O Time):**

1. The program first determines the number of rows (`endRow`) and columns (`endCol`) in the matrix, which takes O(1) time.

2. It then iterates through the rows once to find the target row where the last element of each row is greater than or equal to the target. This loop runs in O(endRow) time, where `endRow` is the number of rows in the matrix.

3. Once the target row is identified, the program iterates through the elements in that row to check if the target exists in that row. This loop runs in O(endCol) time, where `endCol` is the number of columns in the matrix.

4. Therefore, the overall time complexity of the program is O(endRow + endCol), where `endRow` is the number of rows and `endCol` is the number of columns in the matrix.

**Space Complexity (Big O Space):**

1. The program uses a few integer variables (such as `endRow`, `endCol`, and `targetRow`) and a boolean variable (`result`) to store intermediate values. These variables consume constant space, which does not depend on the size of the input matrix.

2. The program does not use any additional data structures or arrays that scale with the size of the input matrix.

3. Therefore, the space complexity of the program is O(1) or constant space.

In summary, the time complexity of the provided program is O(endRow + endCol), and the space complexity is O(1). The program efficiently searches for a target element in a 2D matrix, considering the properties of the matrix, without using additional memory for data structures.
