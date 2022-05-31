## 1001\. Grid Illumination

Hard

There is a 2D `grid` of size `n x n` where each cell of this grid has a lamp that is initially **turned off**.

You are given a 2D array of lamp positions `lamps`, where <code>lamps[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> indicates that the lamp at <code>grid[row<sub>i</sub>][col<sub>i</sub>]</code> is **turned on**. Even if the same lamp is listed more than once, it is turned on.

When a lamp is turned on, it **illuminates its cell** and **all other cells** in the same **row, column, or diagonal**.

You are also given another 2D array `queries`, where <code>queries[j] = [row<sub>j</sub>, col<sub>j</sub>]</code>. For the <code>j<sup>th</sup></code> query, determine whether <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code> is illuminated or not. After answering the <code>j<sup>th</sup></code> query, **turn off** the lamp at <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code> and its **8 adjacent lamps** if they exist. A lamp is adjacent if its cell shares either a side or corner with <code>grid[row<sub>j</sub>][col<sub>j</sub>]</code>.

Return _an array of integers_ `ans`_,_ _where_ `ans[j]` _should be_ `1` _if the cell in the_ <code>j<sup>th</sup></code> _query was illuminated, or_ `0` _if the lamp was not._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/19/illu_1.jpg)

**Input:** n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,0]]

**Output:** [1,0]

**Explanation:** We have the initial grid with all lamps turned off. In the above picture we see the grid after turning on the lamp at grid[0][0] then turning on the lamp at grid[4][4]. The 0<sup>th</sup> query asks if the lamp at grid[1][1] is illuminated or not (the blue square). It is illuminated, so set ans[0] = 1. Then, we turn off all lamps in the red square. ![](https://assets.leetcode.com/uploads/2020/08/19/illu_step1.jpg) The 1<sup>st</sup> query asks if the lamp at grid[1][0] is illuminated or not (the blue square). It is not illuminated, so set ans[1] = 0. Then, we turn off all lamps in the red rectangle. ![](https://assets.leetcode.com/uploads/2020/08/19/illu_step2.jpg)

**Example 2:**

**Input:** n = 5, lamps = [[0,0],[4,4]], queries = [[1,1],[1,1]]

**Output:** [1,1]

**Example 3:**

**Input:** n = 5, lamps = [[0,0],[0,4]], queries = [[0,4],[0,1],[1,4]]

**Output:** [1,1,0]

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   `0 <= lamps.length <= 20000`
*   `0 <= queries.length <= 20000`
*   `lamps[i].length == 2`
*   <code>0 <= row<sub>i</sub>, col<sub>i</sub> < n</code>
*   `queries[j].length == 2`
*   <code>0 <= row<sub>j</sub>, col<sub>j</sub> < n</code>

## Solution

```java
import java.util.HashMap;

public class Solution {
    public int[] gridIllumination(int n, int[][] lamps, int[][] queries) {
        HashMap<Integer, Integer> row = new HashMap<>();
        HashMap<Integer, Integer> col = new HashMap<>();
        HashMap<Integer, Integer> d1 = new HashMap<>();
        HashMap<Integer, Integer> d2 = new HashMap<>();
        HashMap<Integer, Integer> cellno = new HashMap<>();
        for (int[] lamp : lamps) {
            int row1 = lamp[0];
            int col1 = lamp[1];
            row.put(row1, row.getOrDefault(row1, 0) + 1);
            col.put(col1, col.getOrDefault(col1, 0) + 1);
            d1.put((row1 + col1), d1.getOrDefault((row1 + col1), 0) + 1);
            d2.put((row1 - col1), d2.getOrDefault((row1 - col1), 0) + 1);
            int cell = row1 * n + col1;
            cellno.put(cell, cellno.getOrDefault(cell, 0) + 1);
        }
        int[][] dir = { {-1, 0}, {-1, 1}, {0, 1}, {1, 1}, {1, 0}, {1, -1}, {0, -1}, {-1, -1}};
        int[] ans = new int[queries.length];
        for (int i = 0; i < queries.length; i++) {
            int row1 = queries[i][0];
            int col1 = queries[i][1];
            int cell = row1 * n + col1;
            ans[i] =
                    (row.containsKey(row1)
                                    || col.containsKey(col1)
                                    || d1.containsKey(row1 + col1)
                                    || d2.containsKey(row1 - col1)
                                    || cellno.containsKey(cell))
                            ? 1
                            : 0;
            if (cellno.containsKey(cell)) {
                int val = cellno.get(cell);
                cellno.remove(cell);
                if (row.containsKey(row1)) {
                    int rowval = row.get(row1);
                    rowval -= val;
                    if (rowval == 0) {
                        row.remove(row1);
                    } else {
                        row.put(row1, rowval);
                    }
                }
                if (col.containsKey(col1)) {
                    int colval = col.get(col1);
                    colval -= val;
                    if (colval == 0) {
                        col.remove(col1);
                    } else {
                        col.put(col1, colval);
                    }
                }
                if (d1.containsKey(row1 + col1)) {
                    int d1val = d1.get(row1 + col1);
                    d1val -= val;
                    if (d1val == 0) {
                        d1.remove(row1 + col1);
                    } else {
                        row.put((row1 + col1), d1val);
                    }
                }
                if (d2.containsKey(row1 - col1)) {
                    int d2val = d2.get(row1 - col1);
                    d2val -= val;
                    if (d2val == 0) {
                        d2.remove(row1 - col1);
                    } else {
                        d2.put((row1 - col1), d2val);
                    }
                }
            }
            for (int[] ints : dir) {
                int rowdash = row1 + ints[0];
                int coldash = col1 + ints[1];
                int cellno1 = rowdash * n + coldash;
                if (cellno.containsKey(cellno1)) {
                    int val = cellno.get(cellno1);
                    cellno.remove(cellno1);
                    if (row.containsKey(rowdash)) {
                        int rowval = row.get(rowdash);
                        rowval -= val;
                        if (rowval == 0) {
                            row.remove(rowdash);
                        } else {
                            row.put(rowdash, rowval);
                        }
                    }
                    if (col.containsKey(coldash)) {
                        int colval = col.get(coldash);
                        colval -= val;
                        if (colval == 0) {
                            col.remove(coldash);
                        } else {
                            col.put(coldash, colval);
                        }
                    }
                    if (d1.containsKey(rowdash + coldash)) {
                        int d1val = d1.get(rowdash + coldash);
                        d1val -= val;
                        if (d1val == 0) {
                            d1.remove(rowdash + coldash);
                        } else {
                            row.put((rowdash + coldash), d1val);
                        }
                    }
                    if (d2.containsKey(rowdash - coldash)) {
                        int d2val = d2.get(rowdash - coldash);
                        d2val -= val;
                        if (d2val == 0) {
                            d2.remove(rowdash - coldash);
                        } else {
                            d2.put((rowdash - coldash), d2val);
                        }
                    }
                }
            }
        }
        return ans;
    }
}
```