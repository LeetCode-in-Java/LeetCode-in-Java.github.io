## 1074\. Number of Submatrices That Sum to Target

Hard

Given a `matrix` and a `target`, return the number of non-empty submatrices that sum to target.

A submatrix `x1, y1, x2, y2` is the set of all cells `matrix[x][y]` with `x1 <= x <= x2` and `y1 <= y <= y2`.

Two submatrices `(x1, y1, x2, y2)` and `(x1', y1', x2', y2')` are different if they have some coordinate that is different: for example, if `x1 != x1'`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/mate1.jpg)

**Input:** matrix = \[\[0,1,0],[1,1,1],[0,1,0]], target = 0

**Output:** 4

**Explanation:** The four 1x1 submatrices that only contain 0.

**Example 2:**

**Input:** matrix = \[\[1,-1],[-1,1]], target = 0

**Output:** 5

**Explanation:** The two 1x2 submatrices, plus the two 2x1 submatrices, plus the 2x2 submatrix.

**Example 3:**

**Input:** matrix = \[\[904]], target = 0

**Output:** 0

**Constraints:**

*   `1 <= matrix.length <= 100`
*   `1 <= matrix[0].length <= 100`
*   `-1000 <= matrix[i] <= 1000`
*   `-10^8 <= target <= 10^8`

## Solution

```java
import java.util.HashMap;

public class Solution {
    public int numSubmatrixSumTarget(int[][] matrix, int target) {
        int rows = matrix.length;
        int columns = matrix[0].length;
        for (int i = 0; i < rows; i++) {
            for (int j = 1; j < columns; j++) {
                matrix[i][j] += matrix[i][j - 1];
            }
        }
        HashMap<Integer, Integer> sumMap = new HashMap<>();
        int cur;
        int res = 0;
        for (int i = 0; i < columns; i++) {
            for (int j = i; j < columns; j++) {
                sumMap.clear();
                sumMap.put(0, 1);
                cur = 0;
                for (int[] ints : matrix) {
                    cur += ints[j] - (i > 0 ? ints[i - 1] : 0);
                    res += sumMap.getOrDefault(cur - target, 0);
                    sumMap.put(cur, sumMap.getOrDefault(cur, 0) + 1);
                }
            }
        }
        return res;
    }
}
```