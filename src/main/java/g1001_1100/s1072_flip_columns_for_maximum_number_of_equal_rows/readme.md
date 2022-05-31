## 1072\. Flip Columns For Maximum Number of Equal Rows

Medium

You are given an `m x n` binary matrix `matrix`.

You can choose any number of columns in the matrix and flip every cell in that column (i.e., Change the value of the cell from `0` to `1` or vice versa).

Return _the maximum number of rows that have all values equal after some number of flips_.

**Example 1:**

**Input:** matrix = \[\[0,1],[1,1]]

**Output:** 1

**Explanation:** After flipping no values, 1 row has all values equal.

**Example 2:**

**Input:** matrix = \[\[0,1],[1,0]]

**Output:** 2

**Explanation:** After flipping values in the first column, both rows have equal values.

**Example 3:**

**Input:** matrix = \[\[0,0,0],[0,0,1],[1,1,0]]

**Output:** 2

**Explanation:** After flipping values in the first two columns, the last two rows have equal values.

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 300`
*   `matrix[i][j]` is either `0` or `1`.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int maxEqualRowsAfterFlips(int[][] matrix) {
        /*
        Idea:
        For a given row[i], 0<=i<m, row[j], 0<=j<m and j!=i, if either of them can have
        all values equal after some number of flips, then
         row[i]==row[j]  <1> or
         row[i]^row[j] == 111...111 <2> (xor result is a row full of '1')

        Go further, in case<2> row[j] can turn to row[i] by flipping each column of row[j]
        IF assume row[i][0] is 0, then question is convert into:
         1> flipping each column of each row if row[i][0] is not '0',
         2> count the frequency of each row.
         The biggest number of frequencies is the answer.
         */

        // O(M*N), int M = matrix.length, N = matrix[0].length;
        int answer = 0;
        Map<String, Integer> frequency = new HashMap<>();
        for (int[] row : matrix) {
            StringBuilder rowStr = new StringBuilder();
            for (int c : row) {
                if (row[0] == 1) {
                    rowStr.append(c == 1 ? 0 : 1);
                } else {
                    rowStr.append(c);
                }
            }
            String key = rowStr.toString();
            int value = frequency.getOrDefault(key, 0) + 1;
            frequency.put(key, value);
            answer = Math.max(answer, value);
        }
        return answer;
    }
}
```