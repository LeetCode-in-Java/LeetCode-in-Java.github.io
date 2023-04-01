[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1411\. Number of Ways to Paint N × 3 Grid

Hard

You have a `grid` of size `n x 3` and you want to paint each cell of the grid with exactly one of the three colors: **Red**, **Yellow,** or **Green** while making sure that no two adjacent cells have the same color (i.e., no two cells that share vertical or horizontal sides have the same color).

Given `n` the number of rows of the grid, return _the number of ways_ you can paint this `grid`. As the answer may grow large, the answer **must be** computed modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/03/26/e1.png)

**Input:** n = 1

**Output:** 12

**Explanation:** There are 12 possible way to paint the grid as shown.

**Example 2:**

**Input:** n = 5000

**Output:** 30228214

**Constraints:**

*   `n == grid.length`
*   `1 <= n <= 5000`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int MOD = 1000000007;

    public int numOfWays(int n) {
        int[][] dp = new int[n + 1][12];
        Arrays.fill(dp[1], 1);
        int[][] transfer =
                new int[][] {
                    {5, 6, 8, 9, 10},
                    {5, 8, 7, 9, -1},
                    {5, 6, 9, 10, 12},
                    {6, 10, 11, 12, -1},
                    {1, 2, 3, 11, 12},
                    {1, 3, 4, 11, -1},
                    {2, 9, 10, 12, -1},
                    {1, 2, 10, 11, 12},
                    {1, 2, 3, 7, -1},
                    {1, 3, 4, 7, 8},
                    {4, 5, 6, 8, -1},
                    {3, 4, 5, 7, 8}
                };
        for (int i = 2; i <= n; i++) {
            for (int j = 0; j < 12; j++) {
                int[] prevStates = transfer[j];
                int sum = 0;
                for (int s : prevStates) {
                    if (s == -1) {
                        break;
                    }
                    sum = (sum + dp[i - 1][s - 1]) % MOD;
                }
                dp[i][j] = sum;
            }
        }
        int total = 0;
        for (int i = 0; i < 12; i++) {
            total = (total + dp[n][i]) % MOD;
        }
        return total;
    }
}
```