## 2133\. Check if Every Row and Column Contains All Numbers

Easy

An `n x n` matrix is **valid** if every row and every column contains **all** the integers from `1` to `n` (**inclusive**).

Given an `n x n` integer matrix `matrix`, return `true` _if the matrix is **valid**._ Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/21/example1drawio.png)

**Input:** matrix = \[\[1,2,3],[3,1,2],[2,3,1]]

**Output:** true

**Explanation:** In this case, n = 3, and every row and column contains the numbers 1, 2, and 3. Hence, we return true.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/21/example2drawio.png)

**Input:** matrix = \[\[1,1,1],[1,2,3],[1,2,3]]

**Output:** false

**Explanation:** In this case, n = 3, but the first row and the first column do not contain the numbers 2 or 3. Hence, we return false.

**Constraints:**

*   `n == matrix.length == matrix[i].length`
*   `1 <= n <= 100`
*   `1 <= matrix[i][j] <= n`

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public boolean checkValid(int[][] matrix) {
        int n = matrix.length;
        Set<Integer> set = new HashSet<>();
        for (int[] ints : matrix) {
            for (int anInt : ints) {
                set.add(anInt);
            }
            if (set.size() != n) {
                return false;
            }
            set.clear();
        }
        for (int i = 0; i < matrix[0].length; i++) {
            for (int[] ints : matrix) {
                set.add(ints[i]);
            }
            if (set.size() != n) {
                return false;
            }
            set.clear();
        }
        return true;
    }
}
```