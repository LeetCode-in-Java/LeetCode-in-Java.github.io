## 1337\. The K Weakest Rows in a Matrix

Easy

You are given an `m x n` binary matrix `mat` of `1`'s (representing soldiers) and `0`'s (representing civilians). The soldiers are positioned **in front** of the civilians. That is, all the `1`'s will appear to the **left** of all the `0`'s in each row.

A row `i` is **weaker** than a row `j` if one of the following is true:

*   The number of soldiers in row `i` is less than the number of soldiers in row `j`.
*   Both rows have the same number of soldiers and `i < j`.

Return _the indices of the_ `k` _**weakest** rows in the matrix ordered from weakest to strongest_.

**Example 1:**

**Input:** mat = 

[[1,1,0,0,0], 

[1,1,1,1,0], 

[1,0,0,0,0], 

[1,1,0,0,0], 

[1,1,1,1,1]], k = 3

**Output:** [2,0,3]

**Explanation:** The number of soldiers in each row is: 
- Row 0: 2 
- Row 1: 4 
- Row 2: 1 
- Row 3: 2 
- Row 4: 5 
  
The rows ordered from weakest to strongest are [2,0,3,1,4].

**Example 2:**

**Input:** mat = \[\[1,0,0,0], [1,1,1,1], [1,0,0,0], [1,0,0,0]], k = 2

**Output:** [0,2]

**Explanation:** The number of soldiers in each row is: 
- Row 0: 1 
- Row 1: 4 
- Row 2: 1 
- Row 3: 1 
  
The rows ordered from weakest to strongest are [0,2,3,1].

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `2 <= n, m <= 100`
*   `1 <= k <= m`
*   `matrix[i][j]` is either 0 or 1.

## Solution

```java
public class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        int[] result = new int[mat.length];
        for (int i = 0; i < mat.length; i++) {
            int index = binarySearch(mat, i, mat[i].length - 1);
            result[i] = index;
        }
        int minValue = 101;
        int[] resultK = new int[k];
        int index = -1;
        for (int i = 0; i < k; i++) {
            for (int j = 0; j < result.length; j++) {
                if (result[j] < minValue) {
                    minValue = result[j];
                    index = j;
                }
            }
            result[index] = 110;
            resultK[i] = index;
            index = -1;
            minValue = 101;
        }
        return resultK;
    }

    private int binarySearch(int[][] mat, int row, int end) {
        int start = 0;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (mat[row][mid] == 1) {
                start = mid + 1;
            } else if (mat[row][mid] == 0) {
                end = mid - 1;
            }
        }
        return start;
    }
}
```