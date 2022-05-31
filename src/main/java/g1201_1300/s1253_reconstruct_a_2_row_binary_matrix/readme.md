## 1253\. Reconstruct a 2-Row Binary Matrix

Medium

Given the following details of a matrix with `n` columns and `2` rows :

*   The matrix is a binary matrix, which means each element in the matrix can be `0` or `1`.
*   The sum of elements of the 0-th(upper) row is given as `upper`.
*   The sum of elements of the 1-st(lower) row is given as `lower`.
*   The sum of elements in the i-th column(0-indexed) is `colsum[i]`, where `colsum` is given as an integer array with length `n`.

Your task is to reconstruct the matrix with `upper`, `lower` and `colsum`.

Return it as a 2-D integer array.

If there are more than one valid solution, any of them will be accepted.

If no valid solution exists, return an empty 2-D array.

**Example 1:**

**Input:** upper = 2, lower = 1, colsum = [1,1,1]

**Output:** [[1,1,0],[0,0,1]]

**Explanation:** [[1,0,1],[0,1,0]], and [[0,1,1],[1,0,0]] are also correct answers.

**Example 2:**

**Input:** upper = 2, lower = 3, colsum = [2,2,1,1]

**Output:** []

**Example 3:**

**Input:** upper = 5, lower = 5, colsum = [2,1,2,0,1,0,1,2,0,1]

**Output:** [[1,1,1,0,1,0,0,1,0,0],[1,0,1,0,0,0,1,1,0,1]]

**Constraints:**

*   `1 <= colsum.length <= 10^5`
*   `0 <= upper, lower <= colsum.length`
*   `0 <= colsum[i] <= 2`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> reconstructMatrix(int upper, int lower, int[] colsum) {
        List<List<Integer>> res = new ArrayList<>();
        int n = colsum.length;
        int[] upperRow = new int[n];
        int[] lowerRow = new int[n];
        int currentUpperSum = 0;
        int currentLowerSum = 0;
        for (int i = 0; i < n; i++) {
            if (colsum[i] >= 1) {
                upperRow[i] = 1;
                lowerRow[i] = 1;
                currentUpperSum++;
                currentLowerSum++;
            }
        }
        for (int i = 0; i < n; i++) {
            if (colsum[i] == 1 && currentUpperSum > upper) {
                currentUpperSum--;
                upperRow[i] = 0;
            }
        }
        for (int i = 0; i < upperRow.length; i++) {
            if (colsum[i] == 1 && upperRow[i] == 1) {
                currentLowerSum--;
                lowerRow[i] = 0;
            }
        }
        if (currentUpperSum != upper || currentLowerSum != lower) {
            return res;
        }
        res.add(new ArrayList<>());
        res.add(new ArrayList<>());
        for (int i = 0; i < n; i++) {
            res.get(0).add(upperRow[i]);
            res.get(1).add(lowerRow[i]);
        }
        return res;
    }
}
```