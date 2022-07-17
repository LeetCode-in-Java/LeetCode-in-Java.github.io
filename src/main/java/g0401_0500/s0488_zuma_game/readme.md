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
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.HashMap;

public class Solution {
    private HashMap<String, Integer> map = new HashMap<>();

    private String removeConsecutiveThreeOrMoreBalls(String board) {
        Deque<Character> st = new ArrayDeque<>();
        int n = board.length();
        for (int i = 0; i < n; i++) {
            char ch = board.charAt(i);
            st.push(ch);
            if (st.size() >= 3 && (i == n - 1 || ch != board.charAt(i + 1))) {
                char a = st.pop();
                char b = st.pop();
                char c = st.pop();
                if (a == b && b == c) {
                    while (!st.isEmpty() && st.peek() == a) {
                        st.pop();
                    }
                } else {
                    st.push(c);
                    st.push(b);
                    st.push(a);
                }
            }
        }
        StringBuilder res = new StringBuilder();
        while (!st.isEmpty()) {
            res.append(st.pop());
        }
        return res.reverse().toString();
    }

    private String ss(String s, int i, int j) {
        return s.substring(i, j);
    }

    private int solve(String board, String hand) {
        String key = board + "#" + hand;
        if (map.containsKey(key)) {
            return map.get(key);
        }
        board = removeConsecutiveThreeOrMoreBalls(board);
        int ans = 100;
        int n = board.length();
        int m = hand.length();
        if (n == 0 || m == 0) {
            map.put(key, n == 0 ? 0 : 100);
            return n == 0 ? 0 : 100;
        }
        for (int i = 0; i < hand.length(); i++) {
            for (int j = 0; j < n; j++) {
                if (board.charAt(j) == hand.charAt(i)
                        || (j < n - 1 && board.charAt(j) == board.charAt(j + 1))) {
                    ans =
                            Math.min(
                                    ans,
                                    1
                                            + solve(
                                                    ss(board, 0, j + 1)
                                                            + hand.charAt(i)
                                                            + ss(board, j + 1, n),
                                                    ss(hand, 0, i) + ss(hand, i + 1, m)));
                }
            }
        }
        map.put(key, ans);
        return ans;
    }

    public int findMinStep(String board, String hand) {
        int ans = solve(board, hand);
        return ans >= 100 ? -1 : ans;
    }
}
```