[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2718\. Sum of Matrix After Queries

Medium

You are given an integer `n` and a **0-indexed** **2D array** `queries` where <code>queries[i] = [type<sub>i</sub>, index<sub>i</sub>, val<sub>i</sub>]</code>.

Initially, there is a **0-indexed** `n x n` matrix filled with `0`'s. For each query, you must apply one of the following changes:

*   if <code>type<sub>i</sub> == 0</code>, set the values in the row with <code>index<sub>i</sub></code> to <code>val<sub>i</sub></code>, overwriting any previous values.
*   if <code>type<sub>i</sub> == 1</code>, set the values in the column with <code>index<sub>i</sub></code> to <code>val<sub>i</sub></code>, overwriting any previous values.

Return _the sum of integers in the matrix after all queries are applied_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/05/11/exm1.png)

**Input:** n = 3, queries = \[\[0,0,1],[1,2,2],[0,2,3],[1,0,4]]

**Output:** 23

**Explanation:** The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 23.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/05/11/exm2.png)

**Input:** n = 3, queries = \[\[0,0,4],[0,1,2],[1,0,1],[0,2,3],[1,2,1]]

**Output:** 17

**Explanation:** The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 17.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   `queries[i].length == 3`
*   <code>0 <= type<sub>i</sub> <= 1</code>
*   <code>0 <= index<sub>i</sub> < n</code>
*   <code>0 <= val<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public long matrixSumQueries(int n, int[][] queries) {
        boolean[] queriedRow = new boolean[n];
        boolean[] queriedCol = new boolean[n];
        long sum = 0;
        int remainingRows = n;
        int remainingCols = n;
        for (int i = queries.length - 1; i >= 0; i--) {
            int type = queries[i][0];
            int index = queries[i][1];
            int value = queries[i][2];
            if ((type == 0 && !queriedRow[index]) || (type == 1 && !queriedCol[index])) {
                sum += (long) value * (type == 0 ? remainingCols : remainingRows);
                if (type == 0) {
                    remainingRows--;
                    queriedRow[index] = true;
                } else {
                    remainingCols--;
                    queriedCol[index] = true;
                }
            }
        }
        return sum;
    }
}
```