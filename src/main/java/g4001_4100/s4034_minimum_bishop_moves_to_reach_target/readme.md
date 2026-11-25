[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4034\. Minimum Bishop Moves to Reach Target

Medium

There is an `8 x 8` empty chessboard with **1-indexed** rows and columns.

You are given an array `source = [sr, sc]` representing the starting position of a **bishop**, and an array `target = [tr, tc]` representing the target position.

In one move, the bishop travels one or more squares along a single **diagonal** direction, staying within the board.

Return the **minimum** number of moves for the bishop to land **exactly** on `target`. If it can never reach `target`, return -1.

**Example 1:**

**Input:** source = [8,1], target = [1,8]

**Output:** 1

**Explanation:**

**![](https://assets.leetcode.com/uploads/2026/08/31/111.png)**

A single diagonal move takes the bishop straight from `(8, 1)` to `(1, 8)`.

**Example 2:**

**Input:** source = [4,2], target = [1,3]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/08/31/22-ezgifcom-invert-colors.png)

The bishop moves from `(4, 2)` to `(3, 1)`, then from `(3, 1)` to `(1, 3)`, reaching the target in 2 moves.

**Example 3:**

**Input:** source = [1,1], target = [3,4]

**Output:** \-1

**Explanation:**

No matter how many diagonal moves it makes, the bishop starting at `(1, 1)` can never land on `(3, 4)`. Thus, the answer is -1.

**Constraints:**

*   `source.length == target.length == 2`
*   `1 <= sr, sc, tr, tc <= 8`
*   `source != target`

## Solution

```java
public class Solution {
    public int minBishopMoves(int[] source, int[] target) {
        int r1 = source[0];
        int c1 = source[1];
        int r2 = target[0];
        int c2 = target[1];
        if (r1 == r2 && c1 == c2) {
            return 0;
        }
        if (Math.abs(r1 - r2) == Math.abs(c1 - c2)) {
            return 1;
        }
        if ((r1 + c1) % 2 != (r2 + c2) % 2) {
            return -1;
        }
        return 2;
    }
}
```