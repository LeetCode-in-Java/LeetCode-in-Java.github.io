## 120\. Triangle

Medium

Given a `triangle` array, return _the minimum path sum from top to bottom_.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example 1:**

**Input:** triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]

**Output:** 11

**Explanation:**

    The triangle looks like:
        2
       3 4
      6 5 7
     4 1 8 3
     The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above). 

**Example 2:**

**Input:** triangle = [[-10]]

**Output:** -10 

**Constraints:**

*   `1 <= triangle.length <= 200`
*   `triangle[0].length == 1`
*   `triangle[i].length == triangle[i - 1].length + 1`
*   <code>-10<sup>4</sup> <= triangle[i][j] <= 10<sup>4</sup></code>

**Follow up:** Could you do this using only `O(n)` extra space, where `n` is the total number of rows in the triangle?

## Solution

```java
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle == null || triangle.isEmpty()) {
            return 0;
        }
        int[][] dp = new int[triangle.size()][triangle.get(triangle.size() - 1).size()];
        for (int[] temp : dp) {
            Arrays.fill(temp, -10001);
        }
        return dfs(triangle, dp, 0, 0);
    }

    private int dfs(List<List<Integer>> triangle, int[][] dp, int row, int col) {
        if (row >= triangle.size()) {
            return 0;
        }
        if (dp[row][col] != -10001) {
            return dp[row][col];
        }
        int sum =
                triangle.get(row).get(col)
                        + Math.min(
                                dfs(triangle, dp, row + 1, col),
                                dfs(triangle, dp, row + 1, col + 1));
        dp[row][col] = sum;
        return sum;
    }
}
```