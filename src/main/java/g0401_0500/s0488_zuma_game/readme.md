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
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

@SuppressWarnings("java:S135")
public class Solution {
    public int findMinStep(String board, String hand) {
        List<Character> boardList = new ArrayList<>();
        for (char c : board.toCharArray()) {
            boardList.add(c);
        }
        List<Character> handList = new ArrayList<>();
        for (char c : hand.toCharArray()) {
            handList.add(c);
        }
        return findMinStep(boardList, handList);
    }

    private int findMinStep(List<Character> boardList, List<Character> handList) {
        if (boardList.isEmpty()) {
            return 0;
        }
        if (handList.isEmpty()) {
            return -1;
        }
        if (!isValid(boardList, handList)) {
            return -1;
        }
        int res = Integer.MAX_VALUE;
        for (int boardListIndex = 0; boardListIndex <= boardList.size(); boardListIndex++) {
            Set<Character> duplicate = new HashSet<>();
            for (int handListIndex = 0; handListIndex < handList.size(); handListIndex++) {
                if (boardListIndex > 0
                        && boardList.get(boardListIndex - 1).equals(handList.get(handListIndex))) {
                    continue;
                }
                if (duplicate.contains(handList.get(handListIndex))) {
                    continue;
                } else {
                    duplicate.add(handList.get(handListIndex));
                }
                boolean goodCase1 =
                        (boardListIndex < boardList.size()
                                && boardList
                                        .get(boardListIndex)
                                        .equals(handList.get(handListIndex)));
                boolean goodCase2 =
                        (boardListIndex > 0
                                && boardListIndex < boardList.size()
                                && boardList
                                        .get(boardListIndex - 1)
                                        .equals(boardList.get(boardListIndex))
                                && !boardList
                                        .get(boardListIndex - 1)
                                        .equals(handList.get(handListIndex)));
                if (!goodCase1 && !goodCase2) {
                    continue;
                }
                List<Character> boardListClone = new ArrayList<>(boardList);
                List<Character> handListClone = new ArrayList<>(handList);
                boardListClone.add(boardListIndex, handListClone.remove(handListIndex));
                cleanup(boardListClone);
                int preRes = findMinStep(boardListClone, handListClone);
                if (preRes != -1 && preRes < res) {
                    res = preRes;
                }
            }
        }

        if (res == Integer.MAX_VALUE) {
            return -1;
        }
        return res + 1;
    }

    public void cleanup(List<Character> boardList) {
        boolean isCleanup = false;
        while (!isCleanup) {
            isCleanup = true;
            for (int i = 0; i < boardList.size() - 2; i++) {
                int repeatLen = 1;
                for (int j = i + 1; j < boardList.size(); j++) {
                    if (boardList.get(j).equals(boardList.get(j - 1))) {
                        repeatLen++;
                    } else {
                        break;
                    }
                }
                if (repeatLen >= 3) {
                    isCleanup = false;
                    for (; repeatLen > 0; repeatLen--) {
                        boardList.remove(i);
                    }
                }
            }
        }
    }

    public boolean isValid(List<Character> boardList, List<Character> handList) {
        int boardR = 0;
        int boardY = 0;
        int boardB = 0;
        int boardG = 0;
        int boardW = 0;
        int handR = 0;
        int handY = 0;
        int handB = 0;
        int handG = 0;
        int handW = 0;
        for (char c : boardList) {
            if (c == 'R') {
                boardR++;
            }
            if (c == 'Y') {
                boardY++;
            }
            if (c == 'B') {
                boardB++;
            }
            if (c == 'G') {
                boardG++;
            }
            if (c == 'W') {
                boardW++;
            }
        }
        for (char c : handList) {
            if (c == 'R') {
                handR++;
            }
            if (c == 'Y') {
                handY++;
            }
            if (c == 'B') {
                handB++;
            }
            if (c == 'G') {
                handG++;
            }
            if (c == 'W') {
                handW++;
            }
        }
        if (boardR < 3 && boardR > 0 && boardR + handR < 3) {
            return false;
        }
        if (boardY < 3 && boardY > 0 && boardY + handY < 3) {
            return false;
        }
        if (boardB < 3 && boardB > 0 && boardB + handB < 3) {
            return false;
        }
        if (boardG < 3 && boardG > 0 && boardG + handG < 3) {
            return false;
        }
        return boardW >= 3 || boardW <= 0 || boardW + handW >= 3;
    }
}
```