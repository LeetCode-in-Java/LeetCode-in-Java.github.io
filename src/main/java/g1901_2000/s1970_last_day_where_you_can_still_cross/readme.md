## 1970\. Last Day Where You Can Still Cross

Hard

There is a **1-based** binary matrix where `0` represents land and `1` represents water. You are given integers `row` and `col` representing the number of rows and columns in the matrix, respectively.

Initially on day `0`, the **entire** matrix is **land**. However, each day a new cell becomes flooded with **water**. You are given a **1-based** 2D array `cells`, where <code>cells[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> represents that on the <code>i<sup>th</sup></code> day, the cell on the <code>r<sub>i</sub><sup>th</sup></code> row and <code>c<sub>i</sub><sup>th</sup></code> column (**1-based** coordinates) will be covered with **water** (i.e., changed to `1`).

You want to find the **last** day that it is possible to walk from the **top** to the **bottom** by only walking on land cells. You can start from **any** cell in the top row and end at **any** cell in the bottom row. You can only travel in the **four** cardinal directions (left, right, up, and down).

Return _the **last** day where it is possible to walk from the **top** to the **bottom** by only walking on land cells_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/27/1.png)

**Input:** row = 2, col = 2, cells = \[\[1,1],[2,1],[1,2],[2,2]]

**Output:** 2

**Explanation:** The above image depicts how the matrix changes each day starting from day 0. The last day where it is possible to cross from top to bottom is on day 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/27/2.png)

**Input:** row = 2, col = 2, cells = \[\[1,1],[1,2],[2,1],[2,2]]

**Output:** 1

**Explanation:** The above image depicts how the matrix changes each day starting from day 0. The last day where it is possible to cross from top to bottom is on day 1.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/07/27/3.png)

**Input:** row = 3, col = 3, cells = \[\[1,2],[2,1],[3,3],[2,2],[1,1],[1,3],[2,3],[3,2],[3,1]]

**Output:** 3

**Explanation:** The above image depicts how the matrix changes each day starting from day 0. The last day where it is possible to cross from top to bottom is on day 3.

**Constraints:**

*   <code>2 <= row, col <= 2 * 10<sup>4</sup></code>
*   <code>4 <= row * col <= 2 * 10<sup>4</sup></code>
*   `cells.length == row * col`
*   <code>1 <= r<sub>i</sub> <= row</code>
*   <code>1 <= c<sub>i</sub> <= col</code>
*   All the values of `cells` are **unique**.

## Solution

```java
public class Solution {
    public int latestDayToCross(int row, int col, int[][] cells) {
        Ends[][] ends = new Ends[row][col];
        for (int i = 0; i < cells.length; i++) {
            int r = cells[i][0] - 1;
            int c = cells[i][1] - 1;
            Ends curr = null;
            if (c > 0 && ends[r][c - 1] != null) {
                curr = calEnds(ends[r][c - 1], curr, c);
            }
            if (r > 0 && ends[r - 1][c] != null) {
                curr = calEnds(ends[r - 1][c], curr, c);
            }
            if (c < col - 1 && ends[r][c + 1] != null) {
                curr = calEnds(ends[r][c + 1], curr, c);
            }
            if (r < row - 1 && ends[r + 1][c] != null) {
                curr = calEnds(ends[r + 1][c], curr, c);
            }
            if (c > 0 && r > 0 && ends[r - 1][c - 1] != null) {
                curr = calEnds(ends[r - 1][c - 1], curr, c);
            }
            if (c > 0 && r < row - 1 && ends[r + 1][c - 1] != null) {
                curr = calEnds(ends[r + 1][c - 1], curr, c);
            }
            if (c < col - 1 && r > 0 && ends[r - 1][c + 1] != null) {
                curr = calEnds(ends[r - 1][c + 1], curr, c);
            }
            if (c < col - 1 && r < row - 1 && ends[r + 1][c + 1] != null) {
                curr = calEnds(ends[r + 1][c + 1], curr, c);
            }
            if (curr == null) {
                curr = new Ends(i, c, c);
            }
            if (curr.l == 0 && curr.r == col - 1) {
                return i;
            }
            ends[r][c] = curr;
        }
        return cells.length;
    }

    private Ends calEnds(Ends p, Ends curr, int c) {
        while (p.parent != null) {
            p = p.parent;
        }
        p.l = curr == null ? Math.min(p.l, c) : Math.min(p.l, curr.l);
        p.r = curr == null ? Math.max(p.r, c) : Math.max(p.r, curr.r);
        if (curr == null) {
            curr = p;
        } else if (curr.i != p.i) {
            curr.parent = p;
            curr = curr.parent;
        }
        return curr;
    }

    static class Ends {
        int i;
        int l;
        int r;
        Ends parent;

        public Ends(int i, int l, int r) {
            this.i = i;
            this.l = l;
            this.r = r;
        }
    }
}
```