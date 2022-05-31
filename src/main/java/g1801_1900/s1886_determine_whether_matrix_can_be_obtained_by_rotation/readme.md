## 1886\. Determine Whether Matrix Can Be Obtained By Rotation

Easy

Given two `n x n` binary matrices `mat` and `target`, return `true` _if it is possible to make_ `mat` _equal to_ `target` _by **rotating**_ `mat` _in **90-degree increments**, or_ `false` _otherwise._

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/20/grid3.png)

**Input:** mat = \[\[0,1],[1,0]], target = \[\[1,0],[0,1]]

**Output:** true

**Explanation:** We can rotate mat 90 degrees clockwise to make mat equal target. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/20/grid4.png)

**Input:** mat = \[\[0,1],[1,1]], target = \[\[1,0],[0,1]]

**Output:** false

**Explanation:** It is impossible to make mat equal to target by rotating mat. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/26/grid4.png)

**Input:** mat = \[\[0,0,0],[0,1,0],[1,1,1]], target = \[\[1,1,1],[0,1,0],[0,0,0]]

**Output:** true

**Explanation:** We can rotate mat 90 degrees clockwise two times to make mat equal target. 

**Constraints:**

*   `n == mat.length == target.length`
*   `n == mat[i].length == target[i].length`
*   `1 <= n <= 10`
*   `mat[i][j]` and `target[i][j]` are either `0` or `1`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean findRotation(int[][] mat, int[][] target) {
        for (int i = 0; i < 4; i++) {
            if (Arrays.deepEquals(mat, target)) {
                return true;
            }
            rotate(mat);
        }
        return false;
    }

    private void rotate(int[][] mat) {
        // Reverse Rows
        for (int i = 0, j = mat.length - 1; i < j; i++, j--) {
            int[] tempRow = mat[i];
            mat[i] = mat[j];
            mat[j] = tempRow;
        }
        // Transpose
        for (int i = 0; i < mat.length; i++) {
            for (int j = i + 1; j < mat.length; j++) {
                int temp = mat[i][j];
                mat[i][j] = mat[j][i];
                mat[j][i] = temp;
            }
        }
    }
}
```