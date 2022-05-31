## 1139\. Largest 1-Bordered Square

Medium

Given a 2D `grid` of `0`s and `1`s, return the number of elements in the largest **square** subgrid that has all `1`s on its **border**, or `0` if such a subgrid doesn't exist in the `grid`.

**Example 1:**

**Input:** grid = [[1,1,1],[1,0,1],[1,1,1]]

**Output:** 9

**Example 2:**

**Input:** grid = [[1,1,0,0]]

**Output:** 1

**Constraints:**

*   `1 <= grid.length <= 100`
*   `1 <= grid[0].length <= 100`
*   `grid[i][j]` is `0` or `1`

## Solution

```java
public class Solution {
    public int largest1BorderedSquare(int[][] grid) {
        int[][] rightToLeft = new int[grid.length][];
        int[][] bottomToUp = new int[grid.length][];
        for (int i = 0; i < grid.length; i++) {
            rightToLeft[i] = grid[i].clone();
            bottomToUp[i] = grid[i].clone();
        }
        int row = grid.length;
        int col = grid[0].length;
        for (int i = 0; i < row; i++) {
            for (int j = col - 2; j >= 0; j--) {
                if (grid[i][j] == 1) {
                    rightToLeft[i][j] = rightToLeft[i][j + 1] + 1;
                }
            }
        }
        for (int j = 0; j < col; j++) {
            for (int i = row - 2; i >= 0; i--) {
                if (grid[i][j] == 1) {
                    bottomToUp[i][j] = bottomToUp[i + 1][j] + 1;
                }
            }
        }
        int res = 0;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                int curLen = rightToLeft[i][j];
                for (int k = curLen; k >= 1; k--) {
                    if (bottomToUp[i][j] >= k
                            && rightToLeft[i + k - 1][j] >= k
                            && bottomToUp[i][j + k - 1] >= k) {
                        if (k > res) {
                            res = k;
                        }
                        break;
                    }
                }
            }
        }
        return res * res;
    }
}
```