[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3429\. Paint House IV

Medium

You are given an **even** integer `n` representing the number of houses arranged in a straight line, and a 2D array `cost` of size `n x 3`, where `cost[i][j]` represents the cost of painting house `i` with color `j + 1`.

The houses will look **beautiful** if they satisfy the following conditions:

*   No **two** adjacent houses are painted the same color.
*   Houses **equidistant** from the ends of the row are **not** painted the same color. For example, if `n = 6`, houses at positions `(0, 5)`, `(1, 4)`, and `(2, 3)` are considered equidistant.

Return the **minimum** cost to paint the houses such that they look **beautiful**.

**Example 1:**

**Input:** n = 4, cost = \[\[3,5,7],[6,2,9],[4,8,1],[7,3,5]]

**Output:** 9

**Explanation:**

The optimal painting sequence is `[1, 2, 3, 2]` with corresponding costs `[3, 2, 1, 3]`. This satisfies the following conditions:

*   No adjacent houses have the same color.
*   Houses at positions 0 and 3 (equidistant from the ends) are not painted the same color `(1 != 2)`.
*   Houses at positions 1 and 2 (equidistant from the ends) are not painted the same color `(2 != 3)`.

The minimum cost to paint the houses so that they look beautiful is `3 + 2 + 1 + 3 = 9`.

**Example 2:**

**Input:** n = 6, cost = \[\[2,4,6],[5,3,8],[7,1,9],[4,6,2],[3,5,7],[8,2,4]]

**Output:** 18

**Explanation:**

The optimal painting sequence is `[1, 3, 2, 3, 1, 2]` with corresponding costs `[2, 8, 1, 2, 3, 2]`. This satisfies the following conditions:

*   No adjacent houses have the same color.
*   Houses at positions 0 and 5 (equidistant from the ends) are not painted the same color `(1 != 2)`.
*   Houses at positions 1 and 4 (equidistant from the ends) are not painted the same color `(3 != 1)`.
*   Houses at positions 2 and 3 (equidistant from the ends) are not painted the same color `(2 != 3)`.

The minimum cost to paint the houses so that they look beautiful is `2 + 8 + 1 + 2 + 3 + 2 = 18`.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `n` is even.
*   `cost.length == n`
*   `cost[i].length == 3`
*   <code>0 <= cost[i]\[j] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public long minCost(int n, int[][] cost) {
        long dp0 = 0;
        long dp1 = 0;
        long dp2 = 0;
        long dp3 = 0;
        long dp4 = 0;
        long dp5 = 0;
        for (int i = 0; i < n / 2; ++i) {
            long nextdp0 = Math.min(Math.min(dp2, dp3), dp4) + cost[i][0] + cost[n - i - 1][1];
            long nextdp1 = Math.min(Math.min(dp2, dp4), dp5) + cost[i][0] + cost[n - i - 1][2];
            long nextdp2 = Math.min(Math.min(dp0, dp1), dp5) + cost[i][1] + cost[n - i - 1][0];
            long nextdp3 = Math.min(Math.min(dp0, dp4), dp5) + cost[i][1] + cost[n - i - 1][2];
            long nextdp4 = Math.min(Math.min(dp0, dp1), dp3) + cost[i][2] + cost[n - i - 1][0];
            long nextdp5 = Math.min(Math.min(dp1, dp2), dp3) + cost[i][2] + cost[n - i - 1][1];
            dp0 = nextdp0;
            dp1 = nextdp1;
            dp2 = nextdp2;
            dp3 = nextdp3;
            dp4 = nextdp4;
            dp5 = nextdp5;
        }
        long ans = Long.MAX_VALUE;
        for (long x : new long[] {dp0, dp1, dp2, dp3, dp4, dp5}) {
            ans = Math.min(ans, x);
        }
        return ans;
    }
}
```