[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1473\. Paint House III

Hard

There is a row of `m` houses in a small city, each house must be painted with one of the `n` colors (labeled from `1` to `n`), some houses that have been painted last summer should not be painted again.

A neighborhood is a maximal group of continuous houses that are painted with the same color.

*   For example: `houses = [1,2,2,3,3,2,1,1]` contains `5` neighborhoods `[{1}, {2,2}, {3,3}, {2}, {1,1}]`.

Given an array `houses`, an `m x n` matrix `cost` and an integer `target` where:

*   `houses[i]`: is the color of the house `i`, and `0` if the house is not painted yet.
*   `cost[i][j]`: is the cost of paint the house `i` with the color `j + 1`.

Return _the minimum cost of painting all the remaining houses in such a way that there are exactly_ `target` _neighborhoods_. If it is not possible, return `-1`.

**Example 1:**

**Input:** houses = [0,0,0,0,0], cost = \[\[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3

**Output:** 9

**Explanation:** Paint houses of this way [1,2,2,1,1] 

This array contains target = 3 neighborhoods, [{1}, {2,2}, {1,1}]. 

Cost of paint all houses (1 + 1 + 1 + 1 + 5) = 9.

**Example 2:**

**Input:** houses = [0,2,1,2,0], cost = \[\[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3

**Output:** 11

**Explanation:** Some houses are already painted, Paint the houses of this way [2,2,1,2,2] 

This array contains target = 3 neighborhoods, [{2,2}, {1}, {2,2}].

Cost of paint the first and last house (10 + 1) = 11.

**Example 3:**

**Input:** houses = [3,1,2,3], cost = \[\[1,1,1],[1,1,1],[1,1,1],[1,1,1]], m = 4, n = 3, target = 3

**Output:** -1

**Explanation:** Houses are already painted with a total of 4 neighborhoods [{3},{1},{2},{3}] different of target = 3.

**Constraints:**

*   `m == houses.length == cost.length`
*   `n == cost[i].length`
*   `1 <= m <= 100`
*   `1 <= n <= 20`
*   `1 <= target <= m`
*   `0 <= houses[i] <= n`
*   <code>1 <= cost[i][j] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private int[][][] memo;
    private int[] houses;
    private int nColors;
    private int[][] cost;

    public int minCost(int[] houses, int[][] cost, int nColors, int tGroups) {
        this.cost = cost;
        this.houses = houses;
        this.memo = new int[houses.length][nColors + 1][tGroups + 1];
        this.nColors = nColors;

        int dp = dp(0, 0, tGroups);
        return dp == Integer.MAX_VALUE ? -1 : dp;
    }

    private int dp(int ithEl, int prevClr, int tGroups) {
        if (ithEl == houses.length) {
            return tGroups == 0 ? 0 : Integer.MAX_VALUE;
        }
        if (ithEl < houses.length && tGroups < 0) {
            return Integer.MAX_VALUE;
        }
        if (memo[ithEl][prevClr][tGroups] == 0) {
            int currC = houses[ithEl];
            int res = Integer.MAX_VALUE;
            if (currC != 0) {
                int grpLeft = currC == prevClr ? tGroups : tGroups - 1;
                res = dp(ithEl + 1, currC, grpLeft);
            } else {
                for (int clr = 1; clr <= nColors; clr++) {
                    int grpLeft = clr == prevClr ? tGroups : tGroups - 1;
                    int dp = dp(ithEl + 1, clr, grpLeft);
                    res =
                            Math.min(
                                    res,
                                    dp != Integer.MAX_VALUE
                                            ? cost[ithEl][clr - 1] + dp
                                            : Integer.MAX_VALUE);
                }
            }
            memo[ithEl][prevClr][tGroups] = res;
        }
        return memo[ithEl][prevClr][tGroups];
    }
}
```