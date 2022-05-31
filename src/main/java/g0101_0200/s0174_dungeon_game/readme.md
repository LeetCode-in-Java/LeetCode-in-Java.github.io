## 174\. Dungeon Game

Hard

The demons had captured the princess and imprisoned her in **the bottom-right corner** of a `dungeon`. The `dungeon` consists of `m x n` rooms laid out in a 2D grid. Our valiant knight was initially positioned in **the top-left room** and must fight his way through `dungeon` to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to `0` or below, he dies immediately.

Some of the rooms are guarded by demons (represented by negative integers), so the knight loses health upon entering these rooms; other rooms are either empty (represented as 0) or contain magic orbs that increase the knight's health (represented by positive integers).

To reach the princess as quickly as possible, the knight decides to move only **rightward** or **downward** in each step.

Return _the knight's minimum initial health so that he can rescue the princess_.

**Note** that any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/13/dungeon-grid-1.jpg)

**Input:** dungeon = [[-2,-3,3],[-5,-10,1],[10,30,-5]]

**Output:** 7

**Explanation:** The initial health of the knight must be at least 7 if he follows the optimal path: RIGHT-> RIGHT -> DOWN -> DOWN. 

**Example 2:**

**Input:** dungeon = [[0]]

**Output:** 1 

**Constraints:**

*   `m == dungeon.length`
*   `n == dungeon[i].length`
*   `1 <= m, n <= 200`
*   `-1000 <= dungeon[i][j] <= 1000`

## Solution

```java
public class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length;
        int n = dungeon[0].length;
        int[][] memo = new int[m][n];
        return Math.max(1, 1 - traverse(dungeon, 0, 0, memo));
    }

    private int traverse(int[][] dungeon, int r, int c, int[][] memo) {
        if (r == dungeon.length - 1 && c == dungeon[0].length - 1) {
            return dungeon[r][c];
        }
        if (memo[r][c] != 0) {
            return memo[r][c];
        }
        int res = Integer.MIN_VALUE;
        if (r + 1 < dungeon.length) {
            res = Math.max(res, traverse(dungeon, r + 1, c, memo));
        }
        if (c + 1 < dungeon[0].length) {
            res = Math.max(res, traverse(dungeon, r, c + 1, memo));
        }
        res = Math.min(dungeon[r][c], res + dungeon[r][c]);
        memo[r][c] = res;
        return res;
    }
}
```