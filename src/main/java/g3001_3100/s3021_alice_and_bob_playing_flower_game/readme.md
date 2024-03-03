[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3021\. Alice and Bob Playing Flower Game

Medium

Alice and Bob are playing a turn-based game on a circular field surrounded by flowers. The circle represents the field, and there are `x` flowers in the clockwise direction between Alice and Bob, and `y` flowers in the anti-clockwise direction between them.

The game proceeds as follows:

1.  Alice takes the first turn.
2.  In each turn, a player must choose either the clockwise or anti-clockwise direction and pick one flower from that side.
3.  At the end of the turn, if there are no flowers left at all, the **current** player captures their opponent and wins the game.

Given two integers, `n` and `m`, the task is to compute the number of possible pairs `(x, y)` that satisfy the conditions:

*   Alice must win the game according to the described rules.
*   The number of flowers `x` in the clockwise direction must be in the range `[1,n]`.
*   The number of flowers `y` in the anti-clockwise direction must be in the range `[1,m]`.

Return _the number of possible pairs_ `(x, y)` _that satisfy the conditions mentioned in the statement_.

**Example 1:**

**Input:** n = 3, m = 2

**Output:** 3

**Explanation:** The following pairs satisfy conditions described in the statement: (1,2), (3,2), (2,1).

**Example 2:**

**Input:** n = 1, m = 1

**Output:** 0

**Explanation:** No pairs satisfy the conditions described in the statement.

**Constraints:**

*   <code>1 <= n, m <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public long flowerGame(long n, long m) {
        long ans = 0;
        ans += ((n + 1) / 2) * (m / 2) + ((m + 1) / 2) * (n / 2);
        return ans;
    }
}
```