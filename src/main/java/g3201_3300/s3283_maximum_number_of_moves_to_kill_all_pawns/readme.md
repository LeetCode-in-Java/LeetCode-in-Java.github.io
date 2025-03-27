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
import java.util.ArrayDeque;
import java.util.Queue;

public class Solution {
    private static final int[][] DIRECTIONS = {
        {2, 1}, {1, 2}, {-1, 2}, {-2, 1}, {-2, -1}, {-1, -2}, {1, -2}, {2, -1}
    };

    private void initializePositions(int[][] positions, int[][] pos, int kx, int ky) {
        int n = positions.length;
        for (int i = 0; i < n; i++) {
            int x = positions[i][0];
            int y = positions[i][1];
            pos[x][y] = i;
        }
        pos[kx][ky] = n;
    }

    private void calculateDistances(int[][] positions, int[][] pos, int[][] distances) {
        int n = positions.length;
        for (int i = 0; i < n; i++) {
            int count = n - i;
            boolean[][] visited = new boolean[50][50];
            visited[positions[i][0]][positions[i][1]] = true;
            Queue<int[]> que = new ArrayDeque<>();
            que.offer(new int[] {positions[i][0], positions[i][1]});
            int steps = 1;
            while (!que.isEmpty() && count > 0) {
                int size = que.size();
                while (size-- > 0) {
                    int[] cur = que.poll();
                    int x = cur[0];
                    int y = cur[1];
                    for (int[] d : DIRECTIONS) {
                        int nx = x + d[0];
                        int ny = y + d[1];
                        if (0 <= nx && nx < 50 && 0 <= ny && ny < 50 && !visited[nx][ny]) {
                            que.offer(new int[] {nx, ny});
                            visited[nx][ny] = true;
                            int j = pos[nx][ny];
                            if (j > i) {
                                distances[i][j] = distances[j][i] = steps;
                                if (--count == 0) {
                                    break;
                                }
                            }
                        }
                    }
                    if (count == 0) {
                        break;
                    }
                }
                steps++;
            }
        }
    }

    private int calculateDP(int n, int[][] distances) {
        int m = (1 << n) - 1;
        int[][] dp = new int[1 << n][n + 1];
        for (int mask = 1; mask < (1 << n); mask++) {
            boolean isEven = (Integer.bitCount(m ^ mask)) % 2 == 0;
            for (int i = 0; i <= n; i++) {
                int result = 0;
                if (isEven) {
                    for (int j = 0; j < n; j++) {
                        if ((mask & (1 << j)) > 0) {
                            result = Math.max(result, dp[mask ^ (1 << j)][j] + distances[i][j]);
                        }
                    }
                } else {
                    result = Integer.MAX_VALUE;
                    for (int j = 0; j < n; j++) {
                        if ((mask & (1 << j)) > 0) {
                            result = Math.min(result, dp[mask ^ (1 << j)][j] + distances[i][j]);
                        }
                    }
                }
                dp[mask][i] = result;
            }
        }
        return dp[m][n];
    }

    public int maxMoves(int kx, int ky, int[][] positions) {
        int n = positions.length;
        int[][] pos = new int[50][50];
        initializePositions(positions, pos, kx, ky);
        int[][] distances = new int[n + 1][n + 1];
        calculateDistances(positions, pos, distances);
        return calculateDP(n, distances);
    }
}
```