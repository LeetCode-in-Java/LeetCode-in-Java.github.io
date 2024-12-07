[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 488\. Zuma Game

Hard

You are playing a variation of the game Zuma.

In this variation of Zuma, there is a **single row** of colored balls on a board, where each ball can be colored red `'R'`, yellow `'Y'`, blue `'B'`, green `'G'`, or white `'W'`. You also have several colored balls in your hand.

Your goal is to **clear all** of the balls from the board. On each turn:

*   Pick **any** ball from your hand and insert it in between two balls in the row or on either end of the row.
*   If there is a group of **three or more consecutive balls** of the **same color**, remove the group of balls from the board.
    *   If this removal causes more groups of three or more of the same color to form, then continue removing each group until there are none left.
*   If there are no more balls on the board, then you win the game.
*   Repeat this process until you either win or do not have any more balls in your hand.

Given a string `board`, representing the row of balls on the board, and a string `hand`, representing the balls in your hand, return _the **minimum** number of balls you have to insert to clear all the balls from the board. If you cannot clear all the balls from the board using the balls in your hand, return_ `-1`.

**Example 1:**

**Input:** board = "WRRBBW", hand = "RB"

**Output:** -1

**Explanation:** It is impossible to clear all the balls. The best you can do is:

- Insert 'R' so the board becomes WRRRBBW. WRRRBBW -> WBBW.

- Insert 'B' so the board becomes WBBBW. WBBBW -> WW.

There are still balls remaining on the board, and you are out of balls to insert.

**Example 2:**

**Input:** board = "WWRRBBWW", hand = "WRBRW"

**Output:** 2

**Explanation:** To make the board empty:

- Insert 'R' so the board becomes WWRRRBBWW. WWRRRBBWW -> WWBBWW.

- Insert 'B' so the board becomes WWBBBWW. WWBBBWW -> WWWW -> empty.

2 balls from your hand were needed to clear the board.

**Example 3:**

**Input:** board = "G", hand = "GGGGG"

**Output:** 2

**Explanation:** To make the board empty:

- Insert 'G' so the board becomes GG.

- Insert 'G' so the board becomes GGG. GGG -> empty.

2 balls from your hand were needed to clear the board.

**Constraints:**

*   `1 <= board.length <= 16`
*   `1 <= hand.length <= 5`
*   `board` and `hand` consist of the characters `'R'`, `'Y'`, `'B'`, `'G'`, and `'W'`.
*   The initial row of balls on the board will **not** have any groups of three or more consecutive balls of the same color.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int findMinStep(String board, String hand) {
        return dfs(board, hand);
    }

    private int dfs(String board, String hand) {
        return findMinStepDp(board, hand, new HashMap<>());
    }

    private int findMinStepDp(String board, String hand, Map<String, Map<String, Integer>> dp) {
        if (board.isEmpty()) {
            return 0;
        }
        if (hand.isEmpty()) {
            return -1;
        }
        if (dp.get(board) != null && dp.get(board).get(hand) != null) {
            return dp.get(board).get(hand);
        }
        int min = -1;
        for (int i = 0; i <= board.length(); i++) {
            for (int j = 0; j < hand.length(); j++) {
                if ((j == 0 || hand.charAt(j) != hand.charAt(j - 1))
                        && (i == 0 || board.charAt(i - 1) != hand.charAt(j))
                        && ((i < board.length() && board.charAt(i) == hand.charAt(j))
                                || (i > 0
                                        && i < board.length()
                                        && board.charAt(i - 1) == board.charAt(i)
                                        && board.charAt(i) != hand.charAt(j)))) {

                    StringBuilder newS = new StringBuilder(board);
                    newS.insert(i, hand.charAt(j));
                    int sR =
                            findMinStepDp(
                                    removeRepeated(newS.toString()),
                                    hand.substring(0, j) + hand.substring(j + 1, hand.length()),
                                    dp);
                    if (sR != -1) {
                        min = min == -1 ? sR + 1 : Integer.min(min, sR + 1);
                    }
                }
            }
        }
        dp.putIfAbsent(board, new HashMap<>());
        dp.get(board).put(hand, min);
        return min;
    }

    private String removeRepeated(String original) {
        int count = 1;
        int i = 1;
        while (i < original.length()) {
            if (original.charAt(i) == original.charAt(i - 1)) {
                count++;
                i++;
            } else {
                if (count >= 3) {
                    return removeRepeated(
                            original.substring(0, i - count)
                                    + original.substring(i, original.length()));
                } else {
                    count = 1;
                    i++;
                }
            }
        }
        if (count >= 3) {
            return removeRepeated(original.substring(0, original.length() - count));
        } else {
            return original;
        }
    }
}
```