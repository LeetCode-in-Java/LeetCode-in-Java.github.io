[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3238\. Find the Number of Winning Players

Easy

You are given an integer `n` representing the number of players in a game and a 2D array `pick` where <code>pick[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents that the player <code>x<sub>i</sub></code> picked a ball of color <code>y<sub>i</sub></code>.

Player `i` **wins** the game if they pick **strictly more** than `i` balls of the **same** color. In other words,

*   Player 0 wins if they pick any ball.
*   Player 1 wins if they pick at least two balls of the _same_ color.
*   ...
*   Player `i` wins if they pick at least`i + 1` balls of the _same_ color.

Return the number of players who **win** the game.

**Note** that _multiple_ players can win the game.

**Example 1:**

**Input:** n = 4, pick = \[\[0,0],[1,0],[1,0],[2,1],[2,1],[2,0]]

**Output:** 2

**Explanation:**

Player 0 and player 1 win the game, while players 2 and 3 do not win.

**Example 2:**

**Input:** n = 5, pick = \[\[1,1],[1,2],[1,3],[1,4]]

**Output:** 0

**Explanation:**

No player wins the game.

**Example 3:**

**Input:** n = 5, pick = \[\[1,1],[2,4],[2,4],[2,4]]

**Output:** 1

**Explanation:**

Player 2 wins the game by picking 3 balls with color 4.

**Constraints:**

*   `2 <= n <= 10`
*   `1 <= pick.length <= 100`
*   `pick[i].length == 2`
*   <code>0 <= x<sub>i</sub> <= n - 1</code>
*   <code>0 <= y<sub>i</sub> <= 10</code>

## Solution

```java
@SuppressWarnings({"unused", "java:S1172"})
public class Solution {
    public int winningPlayerCount(int n, int[][] pick) {
        int[][] dp = new int[11][11];
        for (int[] ints : pick) {
            int p = ints[0];
            int pi = ints[1];
            dp[p][pi] += 1;
        }
        int count = 0;
        for (int i = 0; i < 11; i++) {
            boolean win = false;
            for (int j = 0; j < 11; j++) {
                if (dp[i][j] >= i + 1) {
                    win = true;
                    break;
                }
            }
            if (win) {
                count += 1;
            }
        }
        return count;
    }
}
```