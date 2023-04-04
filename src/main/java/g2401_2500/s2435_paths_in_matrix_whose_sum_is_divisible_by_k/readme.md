[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2435\. Paths in Matrix Whose Sum Is Divisible by K

Hard

You are given a **0-indexed** `m x n` integer matrix `grid` and an integer `k`. You are currently at position `(0, 0)` and you want to reach position `(m - 1, n - 1)` moving only **down** or **right**.

Return _the number of paths where the sum of the elements on the path is divisible by_ `k`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/08/13/image-20220813183124-1.png)

**Input:** grid = \[\[5,2,4],[3,0,5],[0,7,2]], k = 3

**Output:** 2

**Explanation:** There are two paths where the sum of the elements on the path is divisible by k. The first path highlighted in red has a sum of 5 + 2 + 4 + 5 + 2 = 18 which is divisible by 3. The second path highlighted in blue has a sum of 5 + 3 + 0 + 5 + 2 = 15 which is divisible by 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/08/17/image-20220817112930-3.png)

**Input:** grid = \[\[0,0]], k = 5

**Output:** 1

**Explanation:** The path highlighted in red has a sum of 0 + 0 = 0 which is divisible by 5.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/08/12/image-20220812224605-3.png)

**Input:** grid = \[\[7,3,4,9],[2,3,6,2],[2,3,7,0]], k = 1

**Output:** 10

**Explanation:** Every integer is divisible by 1 so the sum of the elements on every possible path is divisible by k.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m, n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= m * n <= 5 * 10<sup>4</sup></code>
*   `0 <= grid[i][j] <= 100`
*   `1 <= k <= 50`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[][] p = new int[50005][50];
    private int[][] r = new int[50005][50];

    public int numberOfPaths(int[][] grid, int k) {
        int m = grid.length;
        int n = grid[0].length;
        int x = 0;
        for (int i = 0; i < n; ++i) {
            Arrays.fill(r[i], 0, k, 0);
            x += grid[0][i];
            r[i][x % k] = 1;
        }
        for (int i = 1; i < m; ++i) {
            int[][] t = p;
            p = r;
            r = t;
            x = grid[i][0] % k;
            for (int j = 0; j < k; ++j) {
                r[0][(x + j) % k] = p[0][j];
            }
            for (int j = 1, pj = 0; j < n; ++j, ++pj) {
                x = grid[i][j] % k;
                int y = (x > 0) ? (k - x) : 0;
                for (int l = 0; l < k; ++l, ++y) {
                    if (y == k) {
                        y = 0;
                    }
                    int modulo = 1000000007;
                    r[j][l] = (p[j][y] + r[pj][y]) % modulo;
                }
            }
        }
        return r[n - 1][0];
    }
}
```