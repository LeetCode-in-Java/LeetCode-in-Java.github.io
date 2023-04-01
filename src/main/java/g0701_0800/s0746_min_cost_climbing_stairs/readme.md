[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 746\. Min Cost Climbing Stairs

Easy

You are given an integer array `cost` where `cost[i]` is the cost of <code>i<sup>th</sup></code> step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return _the minimum cost to reach the top of the floor_.

**Example 1:**

**Input:** cost = [10,15,20]

**Output:** 15

**Explanation:** You will start at index 1. - Pay 15 and climb two steps to reach the top. The total cost is 15.

**Example 2:**

**Input:** cost = [1,100,1,1,1,100,1,1,100,1]

**Output:** 6

**Explanation:** You will start at index 0. 

- Pay 1 and climb two steps to reach index 2. 
- Pay 1 and climb two steps to reach index 4. 
- Pay 1 and climb two steps to reach index 6. 
- Pay 1 and climb one step to reach index 7. 
- Pay 1 and climb two steps to reach index 9. 
- Pay 1 and climb one step to reach the top. 
  The total cost is 6.

**Constraints:**

*   `2 <= cost.length <= 1000`
*   `0 <= cost[i] <= 999`

## Solution

```java
public class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length];
        dp[0] = cost[0];
        dp[1] = cost[1];
        for (int i = 2; i < cost.length; i++) {
            dp[i] = cost[i] + Math.min(dp[i - 1], dp[i - 2]);
        }
        return Math.min(dp[cost.length - 1], dp[cost.length - 2]);
    }
}
```