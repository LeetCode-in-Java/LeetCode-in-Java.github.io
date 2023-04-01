[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1040\. Moving Stones Until Consecutive II

Medium

There are some stones in different positions on the X-axis. You are given an integer array `stones`, the positions of the stones.

Call a stone an **endpoint stone** if it has the smallest or largest position. In one move, you pick up an **endpoint stone** and move it to an unoccupied position so that it is no longer an **endpoint stone**.

*   In particular, if the stones are at say, `stones = [1,2,5]`, you cannot move the endpoint stone at position `5`, since moving it to any position (such as `0`, or `3`) will still keep that stone as an endpoint stone.

The game ends when you cannot make any more moves (i.e., the stones are in three consecutive positions).

Return _an integer array_ `answer` _of length_ `2` _where_:

*   `answer[0]` _is the minimum number of moves you can play, and_
*   `answer[1]` _is the maximum number of moves you can play_.

**Example 1:**

**Input:** stones = [7,4,9]

**Output:** [1,2]

**Explanation:** We can move 4 -> 8 for one move to finish the game. Or, we can move 9 -> 5, 4 -> 6 for two moves to finish the game.

**Example 2:**

**Input:** stones = [6,5,4,3,10]

**Output:** [2,3]

**Explanation:** We can move 3 -> 8 then 10 -> 7 to finish the game. 

Or, we can move 3 -> 7, 4 -> 8, 5 -> 9 to finish the game. 

Notice we cannot move 10 -> 2 to finish the game, because that would be an illegal move.

**Constraints:**

*   <code>3 <= stones.length <= 10<sup>4</sup></code>
*   <code>1 <= stones[i] <= 10<sup>9</sup></code>
*   All the values of `stones` are **unique**.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] numMovesStonesII(int[] stones) {
        int n = stones.length;
        int[] ans = new int[2];
        int i = 0;
        int j = 0;
        int wsize;
        int scount;
        int minMoves = Integer.MAX_VALUE;
        Arrays.sort(stones);
        while (j < n) {
            wsize = stones[j] - stones[i] + 1;
            scount = j - i + 1;
            if (wsize > n) {
                i++;
                continue;
            }
            if (wsize == n - 1 && scount == n - 1) {
                minMoves = Math.min(minMoves, 2);
            } else {
                minMoves = Math.min(minMoves, n - scount);
            }
            j++;
        }
        ans[0] = minMoves;
        int maxMoves;
        if (stones[1] == stones[0] + 1 || stones[n - 1] == stones[n - 2] + 1) {
            maxMoves = stones[n - 1] - stones[0] + 1 - n;
        } else {
            maxMoves =
                    Math.max(
                            ((stones[n - 1] - stones[1]) - (n - 1) + 1),
                            ((stones[n - 2] - stones[0]) - (n - 1) + 1));
        }
        ans[1] = maxMoves;
        return ans;
    }
}
```