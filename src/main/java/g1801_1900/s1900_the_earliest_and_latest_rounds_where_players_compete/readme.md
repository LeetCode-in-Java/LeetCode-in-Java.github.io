[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1900\. The Earliest and Latest Rounds Where Players Compete

Hard

There is a tournament where `n` players are participating. The players are standing in a single row and are numbered from `1` to `n` based on their **initial** standing position (player `1` is the first player in the row, player `2` is the second player in the row, etc.).

The tournament consists of multiple rounds (starting from round number `1`). In each round, the <code>i<sup>th</sup></code> player from the front of the row competes against the <code>i<sup>th</sup></code> player from the end of the row, and the winner advances to the next round. When the number of players is odd for the current round, the player in the middle automatically advances to the next round.

*   For example, if the row consists of players `1, 2, 4, 6, 7`
    *   Player `1` competes against player `7`.
    *   Player `2` competes against player `6`.
    *   Player `4` automatically advances to the next round.

After each round is over, the winners are lined back up in the row based on the **original ordering** assigned to them initially (ascending order).

The players numbered `firstPlayer` and `secondPlayer` are the best in the tournament. They can win against any other player before they compete against each other. If any two other players compete against each other, either of them might win, and thus you may **choose** the outcome of this round.

Given the integers `n`, `firstPlayer`, and `secondPlayer`, return _an integer array containing two values, the **earliest** possible round number and the **latest** possible round number in which these two players will compete against each other, respectively_.

**Example 1:**

**Input:** n = 11, firstPlayer = 2, secondPlayer = 4

**Output:** [3,4]

**Explanation:**

One possible scenario which leads to the earliest round number:

First round: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11

Second round: 2, 3, 4, 5, 6, 11

Third round: 2, 3, 4

One possible scenario which leads to the latest round number:

First round: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11

Second round: 1, 2, 3, 4, 5, 6

Third round: 1, 2, 4

Fourth round: 2, 4 

**Example 2:**

**Input:** n = 5, firstPlayer = 1, secondPlayer = 5

**Output:** [1,1]

**Explanation:** The players numbered 1 and 5 compete in the first round.

There is no way to make them compete in any other round. 

**Constraints:**

*   `2 <= n <= 28`
*   `1 <= firstPlayer < secondPlayer <= n`

## Solution

```java
public class Solution {
    public int[] earliestAndLatest(int n, int firstPlayer, int secondPlayer) {
        int p1 = Math.min(firstPlayer, secondPlayer);
        int p2 = Math.max(firstPlayer, secondPlayer);
        if (p1 + p2 == n + 1) {
            // p1 and p2 compete in the first round
            return new int[] {1, 1};
        }
        if (n == 3 || n == 4) {
            // p1 and p2 must compete in the second round (only two rounds).
            return new int[] {2, 2};
        }
        // Flip to make p1 be more closer to left than p2 to right end for convenience
        if (p1 - 1 > n - p2) {
            int t = n + 1 - p1;
            p1 = n + 1 - p2;
            p2 = t;
        }
        int m = (n + 1) / 2;
        int min = n;
        int max = 1;
        if (p2 * 2 <= n + 1) {
            // p2 is in first half (n odd or even) or exact middle (n odd)
            //   1  2  3  4  5  6  7  8  9 10 11 12 13 14
            //   .  .  *  .  .  *  .  .  .  .  .  .  .  .
            //         ^        ^
            //         p1       p2
            // Group A are players in front of p1
            // Group B are players between p1 and p2
            int a = p1 - 1;
            int b = p2 - p1 - 1;
            // i represents number of front players in A wins
            // j represents number of front players in B wins
            for (int i = 0; i <= a; i++) {
                for (int j = 0; j <= b; j++) {
                    int[] ret = earliestAndLatest(m, i + 1, i + j + 2);
                    min = Math.min(min, 1 + ret[0]);
                    max = Math.max(max, 1 + ret[1]);
                }
            }
        } else {
            // p2 is in the later half (and has >= p1 distance to the end)
            //    1  2  3  4  5  6  7  8  9 10 11 12 13 14
            //    .  .  *  .  .  .  .  .  .  *  .  .  .  .
            //          ^                    ^
            //          p1   p4             p2    p3
            //                ^--------------^
            //          ^--------------------------^
            int p4 = n + 1 - p2;
            int a = p1 - 1;
            int b = p4 - p1 - 1;
            // Group C are players between p4 and p2, (c+1)/2 will advance to next round.
            int c = p2 - p4 - 1;
            for (int i = 0; i <= a; i++) {
                for (int j = 0; j <= b; j++) {
                    int[] ret = earliestAndLatest(m, i + 1, i + j + 1 + (c + 1) / 2 + 1);
                    min = Math.min(min, 1 + ret[0]);
                    max = Math.max(max, 1 + ret[1]);
                }
            }
        }
        return new int[] {min, max};
    }
}
```