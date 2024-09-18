[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3283\. Maximum Number of Moves to Kill All Pawns

Hard

There is a `50 x 50` chessboard with **one** knight and some pawns on it. You are given two integers `kx` and `ky` where `(kx, ky)` denotes the position of the knight, and a 2D array `positions` where <code>positions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> denotes the position of the pawns on the chessboard.

Alice and Bob play a _turn-based_ game, where Alice goes first. In each player's turn:

*   The player _selects_ a pawn that still exists on the board and captures it with the knight in the **fewest** possible **moves**. **Note** that the player can select **any** pawn, it **might not** be one that can be captured in the **least** number of moves.
*   In the process of capturing the _selected_ pawn, the knight **may** pass other pawns **without** capturing them. **Only** the _selected_ pawn can be captured in _this_ turn.

Alice is trying to **maximize** the **sum** of the number of moves made by _both_ players until there are no more pawns on the board, whereas Bob tries to **minimize** them.

Return the **maximum** _total_ number of moves made during the game that Alice can achieve, assuming both players play **optimally**.

Note that in one **move,** a chess knight has eight possible positions it can move to, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![](https://assets.leetcode.com/uploads/2024/08/01/chess_knight.jpg)

**Example 1:**

**Input:** kx = 1, ky = 1, positions = \[\[0,0]]

**Output:** 4

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/16/gif3.gif)

The knight takes 4 moves to reach the pawn at `(0, 0)`.

**Example 2:**

**Input:** kx = 0, ky = 2, positions = \[\[1,1],[2,2],[3,3]]

**Output:** 8

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/08/16/gif4.gif)**

*   Alice picks the pawn at `(2, 2)` and captures it in two moves: `(0, 2) -> (1, 4) -> (2, 2)`.
*   Bob picks the pawn at `(3, 3)` and captures it in two moves: `(2, 2) -> (4, 1) -> (3, 3)`.
*   Alice picks the pawn at `(1, 1)` and captures it in four moves: `(3, 3) -> (4, 1) -> (2, 2) -> (0, 3) -> (1, 1)`.

**Example 3:**

**Input:** kx = 0, ky = 0, positions = \[\[1,2],[2,4]]

**Output:** 3

**Explanation:**

*   Alice picks the pawn at `(2, 4)` and captures it in two moves: `(0, 0) -> (1, 2) -> (2, 4)`. Note that the pawn at `(1, 2)` is not captured.
*   Bob picks the pawn at `(1, 2)` and captures it in one move: `(2, 4) -> (1, 2)`.

**Constraints:**

*   `0 <= kx, ky <= 49`
*   `1 <= positions.length <= 15`
*   `positions[i].length == 2`
*   `0 <= positions[i][0], positions[i][1] <= 49`
*   All `positions[i]` are unique.
*   The input is generated such that `positions[i] != [kx, ky]` for all `0 <= i < positions.length`.

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    private static final int[][] KNIGHT_MOVES = {
        {-2, -1}, {-2, 1}, {-1, -2}, {-1, 2},
        {1, -2}, {1, 2}, {2, -1}, {2, 1}
    };
    private int[][] distances;
    private Integer[][] memo;

    public int maxMoves(int kx, int ky, int[][] positions) {
        int n = positions.length;
        distances = new int[n + 1][n + 1];
        memo = new Integer[n + 1][1 << n];
        // Calculate distances between all pairs of positions (including knight's initial position)
        for (int i = 0; i < n; i++) {
            distances[n][i] = calculateMoves(kx, ky, positions[i][0], positions[i][1]);
            for (int j = i + 1; j < n; j++) {
                int dist =
                        calculateMoves(
                                positions[i][0], positions[i][1], positions[j][0], positions[j][1]);
                distances[i][j] = distances[j][i] = dist;
            }
        }
        return minimax(n, (1 << n) - 1, true);
    }

    private int minimax(int lastPos, int remainingPawns, boolean isAlice) {
        if (remainingPawns == 0) {
            return 0;
        }
        if (memo[lastPos][remainingPawns] != null) {
            return memo[lastPos][remainingPawns];
        }
        int result = isAlice ? 0 : Integer.MAX_VALUE;
        for (int i = 0; i < distances.length - 1; i++) {
            if ((remainingPawns & (1 << i)) != 0) {
                int newRemainingPawns = remainingPawns & ~(1 << i);
                int moveValue = distances[lastPos][i] + minimax(i, newRemainingPawns, !isAlice);

                if (isAlice) {
                    result = Math.max(result, moveValue);
                } else {
                    result = Math.min(result, moveValue);
                }
            }
        }
        memo[lastPos][remainingPawns] = result;
        return result;
    }

    private int calculateMoves(int x1, int y1, int x2, int y2) {
        if (x1 == x2 && y1 == y2) {
            return 0;
        }
        boolean[][] visited = new boolean[50][50];
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] {x1, y1, 0});
        visited[x1][y1] = true;
        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int x = current[0];
            int y = current[1];
            int moves = current[2];
            for (int[] move : KNIGHT_MOVES) {
                int nx = x + move[0];
                int ny = y + move[1];
                if (nx == x2 && ny == y2) {
                    return moves + 1;
                }
                if (nx >= 0 && nx < 50 && ny >= 0 && ny < 50 && !visited[nx][ny]) {
                    queue.offer(new int[] {nx, ny, moves + 1});
                    visited[nx][ny] = true;
                }
            }
        }
        // Should never reach here if input is valid
        return -1;
    }
}
```