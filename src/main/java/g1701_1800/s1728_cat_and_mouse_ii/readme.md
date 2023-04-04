[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1728\. Cat and Mouse II

Hard

A game is played by a cat and a mouse named Cat and Mouse.

The environment is represented by a `grid` of size `rows x cols`, where each element is a wall, floor, player (Cat, Mouse), or food.

*   Players are represented by the characters `'C'`(Cat)`,'M'`(Mouse).
*   Floors are represented by the character `'.'` and can be walked on.
*   Walls are represented by the character `'#'` and cannot be walked on.
*   Food is represented by the character `'F'` and can be walked on.
*   There is only one of each character `'C'`, `'M'`, and `'F'` in `grid`.

Mouse and Cat play according to the following rules:

*   Mouse **moves first**, then they take turns to move.
*   During each turn, Cat and Mouse can jump in one of the four directions (left, right, up, down). They cannot jump over the wall nor outside of the `grid`.
*   `catJump, mouseJump` are the maximum lengths Cat and Mouse can jump at a time, respectively. Cat and Mouse can jump less than the maximum length.
*   Staying in the same position is allowed.
*   Mouse can jump over Cat.

The game can end in 4 ways:

*   If Cat occupies the same position as Mouse, Cat wins.
*   If Cat reaches the food first, Cat wins.
*   If Mouse reaches the food first, Mouse wins.
*   If Mouse cannot get to the food within 1000 turns, Cat wins.

Given a `rows x cols` matrix `grid` and two integers `catJump` and `mouseJump`, return `true` _if Mouse can win the game if both Cat and Mouse play optimally, otherwise return_ `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/12/sample_111_1955.png)

**Input:** grid = ["####F","#C...","M...."], catJump = 1, mouseJump = 2

**Output:** true

**Explanation:** Cat cannot catch Mouse on its turn nor can it get the food before Mouse.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/12/sample_2_1955.png)

**Input:** grid = ["M.C...F"], catJump = 1, mouseJump = 4

**Output:** true

**Example 3:**

**Input:** grid = ["M.C...F"], catJump = 1, mouseJump = 3

**Output:** false

**Constraints:**

*   `rows == grid.length`
*   `cols = grid[i].length`
*   `1 <= rows, cols <= 8`
*   `grid[i][j]` consist only of characters `'C'`, `'M'`, `'F'`, `'.'`, and `'#'`.
*   There is only one of each character `'C'`, `'M'`, and `'F'` in `grid`.
*   `1 <= catJump, mouseJump <= 8`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    private static final int MOUSE_TURN = 0;
    private final List<Integer>[][] graphs = new List[2][];
    private int foodPos;
    private int[][][] memo;

    public boolean canMouseWin(String[] grid, int catJump, int mouseJump) {
        int m = grid.length;
        int n = grid[0].length();
        int mousePos = 0;
        int catPos = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                char c = grid[i].charAt(j);
                if (c == 'F') {
                    foodPos = i * n + j;
                } else if (c == 'C') {
                    catPos = i * n + j;
                } else if (c == 'M') {
                    mousePos = i * n + j;
                }
            }
        }
        graphs[0] = buildGraph(mouseJump, grid);
        graphs[1] = buildGraph(catJump, grid);
        memo = new int[m * n][m * n][2];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                char c = grid[i].charAt(j);
                if (c == '#' || c == 'F') {
                    continue;
                }
                int catTurn = 1;
                dfs(i * n + j, foodPos, catTurn);
            }
        }
        return memo[mousePos][catPos][MOUSE_TURN] < 0;
    }

    private List<Integer>[] buildGraph(int jump, String[] grid) {
        int[][] dirs = { {-1, 0}, {1, 0}, {0, 1}, {0, -1}};
        int m = grid.length;
        int n = grid[0].length();
        List<Integer>[] graph = new List[m * n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                List<Integer> list = new ArrayList<>();
                graph[i * n + j] = list;
                if (grid[i].charAt(j) == '#') {
                    continue;
                }
                list.add(i * n + j);
                for (int[] dir : dirs) {
                    for (int step = 1; step <= jump; step++) {
                        int x = i + dir[0] * step;
                        int y = j + dir[1] * step;
                        if (x < 0 || x >= m || y < 0 || y >= n || grid[x].charAt(y) == '#') {
                            break;
                        }
                        list.add(x * n + y);
                    }
                }
            }
        }
        return graph;
    }

    private void dfs(int p1, int p2, int turn) {
        if (p1 == p2) {
            return;
        }
        if ((turn == 0 ? p2 : p1) == foodPos) {
            return;
        }
        if (memo[p1][p2][turn] < 0) {
            return;
        }
        memo[p1][p2][turn] = -1;
        turn ^= 1;
        for (int w : graphs[turn][p2]) {
            if (turn == MOUSE_TURN || ++memo[w][p1][turn] == graphs[turn][w].size()) {
                dfs(w, p1, turn);
            }
        }
    }
}
```