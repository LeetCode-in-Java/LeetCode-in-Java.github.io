## 959\. Regions Cut By Slashes

Medium

An `n x n` grid is composed of `1 x 1` squares where each `1 x 1` square consists of a `'/'`, `'\'`, or blank space `' '`. These characters divide the square into contiguous regions.

Given the grid `grid` represented as a string array, return _the number of regions_.

Note that backslash characters are escaped, so a `'\'` is represented as `'\\'`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/15/1.png)

**Input:** grid = [" /","/ "]

**Output:** 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/15/2.png)

**Input:** grid = [" /"," "]

**Output:** 1

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/15/4.png)

**Input:** grid = ["/\\\\","\\\\/"]

**Output:** 5

**Explanation:** Recall that because \\ characters are escaped, "\\\\/" refers to \\/, and "/\\\\" refers to /\\.

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `1 <= n <= 30`
*   `grid[i][j]` is either `'/'`, `'\'`, or `' '`.

## Solution

```java
public class Solution {
    private int regions;
    private int[] parent;

    public int regionsBySlashes(String[] grid) {
        int n = grid.length;
        regions = n * n * 4;
        unionFind(regions);
        for (int row = 0; row < grid.length; ++row) {
            int col = 0;
            String str = grid[row];
            char[] ch = str.toCharArray();
            for (char c : ch) {
                int index = row * n * 4 + col * 4;
                if (c == '/') {
                    union(index, index + 3);
                    union(index + 1, index + 2);
                } else if (c == ' ') {
                    union(index, index + 1);
                    union(index + 1, index + 2);
                    union(index + 2, index + 3);

                } else {
                    union(index, index + 1);
                    union(index + 2, index + 3);
                    // ++i;
                }
                if (row != n - 1) {
                    union(index + 2, index + 4 * n);
                }
                if (col != n - 1) {
                    union(index + 1, index + 7);
                }
                ++col;
            }
        }
        return regions;
    }

    private void unionFind(int n) {
        parent = new int[n];
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }

    private void union(int p, int q) {
        if (connected(p, q)) {
            return;
        }
        --regions;
        int i = root(p);
        int j = root(q);
        if (i > j) {
            parent[i] = j;
        } else {
            parent[j] = i;
        }
    }

    private boolean connected(int p, int q) {
        return root(p) == root(q);
    }

    private int root(int index) {
        while (index != parent[index]) {
            parent[index] = parent[parent[index]];
            index = parent[index];
        }
        return index;
    }
}
```