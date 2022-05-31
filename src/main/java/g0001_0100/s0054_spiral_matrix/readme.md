## 54\. Spiral Matrix

Medium

Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

**Input:** matrix = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [1,2,3,6,9,8,7,4,5] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

**Input:** matrix = \[\[1,2,3,4],[5,6,7,8],[9,10,11,12]]

**Output:** [1,2,3,4,8,12,11,10,9,5,6,7] 

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 10`
*   `-100 <= matrix[i][j] <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> list = new ArrayList<>();
        int r = 0;
        int c = 0;
        int bigR = matrix.length - 1;
        int bigC = matrix[0].length - 1;
        while (r <= bigR && c <= bigC) {
            for (int i = c; i <= bigC; i++) {
                list.add(matrix[r][i]);
            }
            r++;
            for (int i = r; i <= bigR; i++) {
                list.add(matrix[i][bigC]);
            }
            bigC--;
            for (int i = bigC; i >= c && r <= bigR; i--) {
                list.add(matrix[bigR][i]);
            }
            bigR--;
            for (int i = bigR; i >= r && c <= bigC; i--) {
                list.add(matrix[i][c]);
            }
            c++;
        }
        return list;
    }
}
```