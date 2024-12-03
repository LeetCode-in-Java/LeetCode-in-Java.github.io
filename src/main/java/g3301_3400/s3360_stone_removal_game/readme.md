[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3360\. Stone Removal Game

Easy

Alice and Bob are playing a game where they take turns removing stones from a pile, with _Alice going first_.

*   Alice starts by removing **exactly** 10 stones on her first turn.
*   For each subsequent turn, each player removes **exactly** 1 fewer stone than the previous opponent.

The player who cannot make a move loses the game.

Given a positive integer `n`, return `true` if Alice wins the game and `false` otherwise.

**Example 1:**

**Input:** n = 12

**Output:** true

**Explanation:**

*   Alice removes 10 stones on her first turn, leaving 2 stones for Bob.
*   Bob cannot remove 9 stones, so Alice wins.

**Example 2:**

**Input:** n = 1

**Output:** false

**Explanation:**

*   Alice cannot remove 10 stones, so Alice loses.

**Constraints:**

*   `1 <= n <= 50`

## Solution

```java
public class Solution {
    public boolean canAliceWin(int n) {
        if (n < 10) {
            return false;
        }
        int stonesRemaining = n - 10;
        int stonesToBeRemoved = 9;
        int i = 1;
        while (stonesRemaining >= stonesToBeRemoved) {
            stonesRemaining -= stonesToBeRemoved;
            i++;
            stonesToBeRemoved--;
        }
        return i % 2 != 0;
    }
}
```