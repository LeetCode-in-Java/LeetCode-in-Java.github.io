[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1049\. Last Stone Weight II

Medium

You are given an array of integers `stones` where `stones[i]` is the weight of the <code>i<sup>th</sup></code> stone.

We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights `x` and `y` with `x <= y`. The result of this smash is:

*   If `x == y`, both stones are destroyed, and
*   If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is **at most one** stone left.

Return _the smallest possible weight of the left stone_. If there are no stones left, return `0`.

**Example 1:**

**Input:** stones = [2,7,4,1,8,1]

**Output:** 1

**Explanation:**

We can combine 2 and 4 to get 2, so the array converts to [2,7,1,8,1] then,

we can combine 7 and 8 to get 1, so the array converts to [2,1,1,1] then,

we can combine 2 and 1 to get 1, so the array converts to [1,1,1] then,

we can combine 1 and 1 to get 0, so the array converts to [1], then that's the optimal value.

**Example 2:**

**Input:** stones = [31,26,33,21,40]

**Output:** 5

**Constraints:**

*   `1 <= stones.length <= 30`
*   `1 <= stones[i] <= 100`

## Solution

```java
public class Solution {
    public int lastStoneWeightII(int[] stones) {
        // dp[i][j] i is the index of stones, j is the current weight
        // goal is to find max closest to half and use it to get the diff
        // 0-1 knapsack problem
        int sum = 0;
        for (int stone : stones) {
            sum += stone;
        }
        int half = sum / 2;
        int[] dp = new int[half + 1];
        for (int cur : stones) {
            for (int j = half; j >= cur; j--) {
                dp[j] = Math.max(dp[j], dp[j - cur] + cur);
            }
        }
        return sum - dp[half] * 2;
    }
}
```