[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2812\. Find the Safest Path in a Grid

Medium

You are given a **0-indexed** 2D matrix `grid` of size `n x n`, where `(r, c)` represents:

*   A cell containing a thief if `grid[r][c] = 1`
*   An empty cell if `grid[r][c] = 0`

You are initially positioned at cell `(0, 0)`. In one move, you can move to any adjacent cell in the grid, including cells containing thieves.

The **safeness factor** of a path on the grid is defined as the **minimum** manhattan distance from any cell in the path to any thief in the grid.

Return _the **maximum safeness factor** of all paths leading to cell_ `(n - 1, n - 1)`_._

An **adjacent** cell of cell `(r, c)`, is one of the cells `(r, c + 1)`, `(r, c - 1)`, `(r + 1, c)` and `(r - 1, c)` if it exists.

The **Manhattan distance** between two cells `(a, b)` and `(x, y)` is equal to `|a - x| + |b - y|`, where `|val|` denotes the absolute value of val.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/07/02/example1.png)

**Input:** grid = \[\[1,0,0],[0,0,0],[0,0,1]]

**Output:** 0

**Explanation:** All paths from (0, 0) to (n - 1, n - 1) go through the thieves in cells (0, 0) and (n - 1, n - 1). 

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/07/02/example2.png)

**Input:** grid = \[\[0,0,1],[0,0,0],[0,0,0]]

**Output:** 2

**Explanation:**

The path depicted in the picture above has a safeness factor of 2 since:

- The closest cell of the path to the thief at cell (0, 2) is cell (0, 0).

The distance between them is \| 0 - 0 \| + \| 0 - 2 \| = 2.

It can be shown that there are no other paths with a higher safeness factor. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2023/07/02/example3.png)

**Input:** grid = \[\[0,0,0,1],[0,0,0,0],[0,0,0,0],[1,0,0,0]]

**Output:** 2

**Explanation:**

The path depicted in the picture above has a safeness factor of 2 since:

- The closest cell of the path to the thief at cell (0, 3) is cell (1, 2).

The distance between them is \| 0 - 1 \| + \| 3 - 2 \| = 2.

- The closest cell of the path to the thief at cell (3, 0) is cell (3, 2).

The distance between them is \| 3 - 3 \| + \| 0 - 2 \| = 2.

It can be shown that there are no other paths with a higher safeness factor. 

**Constraints:**

*   `1 <= grid.length == n <= 400`
*   `grid[i].length == n`
*   `grid[i][j]` is either `0` or `1`.
*   There is at least one thief in the `grid`.

## Solution

```java
import java.util.Arrays;
import java.util.List;

@SuppressWarnings("java:S6541")
public class Solution {
    private static final int[][] MOVES = new int[][] { {-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public int maximumSafenessFactor(List<List<Integer>> grid) {
        final int yLen = grid.size();
        final int xLen = grid.get(0).size();
        if (grid.get(0).get(0) == 1 || grid.get(yLen - 1).get(xLen - 1) == 1) {
            return 0;
        }
        int[][] secure = new int[yLen][xLen];
        int[] deque = new int[yLen * xLen];
        int[] nDeque = new int[yLen * xLen];
        int[] tmpDeque;
        int[] queue = new int[yLen * xLen];
        int[] root = new int[yLen * xLen];
        int head = -1;
        int tail = -1;
        int qIdx = -1;
        int end = yLen * xLen - 1;
        int curY;
        int curX;
        int nextY;
        int nextX;
        int curID;
        int nextID;
        for (int y = 0; y < yLen; y++) {
            Arrays.fill(secure[y], -1);
            for (int x = 0; x < xLen; x++) {
                if (grid.get(y).get(x) == 1) {
                    secure[y][x] = 0;
                    curID = y * xLen + x;
                    root[curID] = queue[++qIdx] = nDeque[++tail] = curID;
                }
            }
        }
        int start = 0;
        int stop = qIdx;
        for (int t = 1; tail > -1; t++) {
            tmpDeque = deque;
            deque = nDeque;
            nDeque = tmpDeque;
            head = tail;
            tail = -1;
            start = qIdx;
            for (; head >= 0; head--) {
                curY = deque[head] / xLen;
                curX = deque[head] % xLen;
                for (int[] move : MOVES) {
                    nextY = curY + move[0];
                    nextX = curX + move[1];
                    if (nextY >= 0
                            && nextY < yLen
                            && nextX >= 0
                            && nextX < xLen
                            && secure[nextY][nextX] < 0) {
                        secure[nextY][nextX] = t;
                        nextID = nextY * xLen + nextX;
                        root[nextID] = queue[++qIdx] = nDeque[++tail] = nextID;
                    }
                }
            }
        }
        for (qIdx = start; qIdx > stop; qIdx--) {
            curY = queue[qIdx] / xLen;
            curX = queue[qIdx] % xLen;
            curID = curY * xLen + curX;
            for (int[] move : MOVES) {
                nextY = curY + move[0];
                nextX = curX + move[1];
                if (nextY >= 0
                        && nextY < yLen
                        && nextX >= 0
                        && nextX < xLen
                        && secure[nextY][nextX] >= secure[curY][curX]) {
                    nextID = nextY * xLen + nextX;
                    root[find(root, curID)] = find(root, nextID);
                }
            }
            if (find(root, 0) == find(root, end)) {
                return secure[curY][curX];
            }
        }
        return 0;
    }

    private int find(int[] root, int idx) {
        if (idx == root[idx]) {
            return idx;
        }
        root[idx] = find(root, root[idx]);
        return root[idx];
    }
}
```