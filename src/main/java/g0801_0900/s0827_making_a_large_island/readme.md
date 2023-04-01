[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 827\. Making A Large Island

Hard

You are given an `n x n` binary matrix `grid`. You are allowed to change **at most one** `0` to be `1`.

Return _the size of the largest **island** in_ `grid` _after applying this operation_.

An **island** is a 4-directionally connected group of `1`s.

**Example 1:**

**Input:** grid = \[\[1,0],[0,1]]

**Output:** 3

**Explanation:** Change one 0 to 1 and connect two 1s, then we get an island with area = 3. 

**Example 2:**

**Input:** grid = \[\[1,1],[1,0]]

**Output:** 4

**Explanation:** Change the 0 to 1 and make the island bigger, only one island with area = 4.

**Example 3:**

**Input:** grid = \[\[1,1],[1,1]]

**Output:** 4

**Explanation:** Can't change any 0 to 1, only one island with area = 4. 

**Constraints:**

*   `n == grid.length`
*   `n == grid[i].length`
*   `1 <= n <= 500`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```java
import java.util.HashMap;

public class Solution {
    private int[] p;
    private int[] s;

    private void makeSet(int x, int y, int rl) {
        int a = x * rl + y;
        p[a] = a;
        s[a] = 1;
    }

    private void comb(int x1, int y1, int x2, int y2, int rl) {
        int a = find(x1 * rl + y1);
        int b = find(x2 * rl + y2);
        if (a == b) {
            return;
        }
        if (s[a] < s[b]) {
            int t = a;
            a = b;
            b = t;
        }
        p[b] = a;
        s[a] += s[b];
    }

    private int find(int a) {
        if (p[a] == a) {
            return a;
        }
        p[a] = find(p[a]);
        return p[a];
    }

    public int largestIsland(int[][] grid) {
        int rl = grid.length;
        int cl = grid[0].length;
        p = new int[rl * cl];
        s = new int[rl * cl];
        for (int i = 0; i < rl; i++) {
            for (int j = 0; j < cl; j++) {
                if (grid[i][j] == 0) {
                    continue;
                }
                makeSet(i, j, rl);
                if (i > 0 && grid[i - 1][j] == 1) {
                    comb(i, j, i - 1, j, rl);
                }
                if (j > 0 && grid[i][j - 1] == 1) {
                    comb(i, j, i, j - 1, rl);
                }
            }
        }
        int m = 0;
        int t;
        HashMap<Integer, Integer> sz = new HashMap<>();
        for (int i = 0; i < rl; i++) {
            for (int j = 0; j < cl; j++) {
                if (grid[i][j] == 0) {
                    // find root, check if same and combine size
                    t = 1;
                    if (i > 0 && grid[i - 1][j] == 1) {
                        sz.put(find((i - 1) * rl + j), s[find((i - 1) * rl + j)]);
                    }
                    if (j > 0 && grid[i][j - 1] == 1) {
                        sz.put(find(i * rl + j - 1), s[find(i * rl + j - 1)]);
                    }
                    if ((i < rl - 1) && grid[i + 1][j] == 1) {
                        sz.put(find((i + 1) * rl + j), s[find((i + 1) * rl + j)]);
                    }
                    if ((j < cl - 1) && grid[i][j + 1] == 1) {
                        sz.put(find(i * rl + j + 1), s[find(i * rl + j + 1)]);
                    }
                    for (int val : sz.values()) {
                        t += val;
                    }
                    m = Math.max(m, t);
                    sz.clear();
                } else {
                    m = Math.max(m, s[i * rl + j]);
                }
            }
        }
        return m;
    }
}
```