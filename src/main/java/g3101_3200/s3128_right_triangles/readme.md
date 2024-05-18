[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3128\. Right Triangles

Medium

You are given a 2D boolean matrix `grid`.

Return an integer that is the number of **right triangles** that can be made with the 3 elements of `grid` such that **all** of them have a value of 1.

**Note:**

*   A collection of 3 elements of `grid` is a **right triangle** if one of its elements is in the **same row** with another element and in the **same column** with the third element. The 3 elements do not have to be next to each other.

**Example 1:**

0 **1** 0

0 **1 1**

0 1 0

0 1 0

0 **1 1**

0 **1** 0

**Input:** grid = \[\[0,1,0],[0,1,1],[0,1,0]]

**Output:** 2

**Explanation:**

There are two right triangles.

**Example 2:**

1 0 0 0

0 1 0 1

1 0 0 0

**Input:** grid = \[\[1,0,0,0],[0,1,0,1],[1,0,0,0]]

**Output:** 0

**Explanation:**

There are no right triangles.

**Example 3:**

**1** 0 **1**

**1** 0 0

1 0 0

**1** 0 **1**

1 0 0

**1** 0 0

**Input:** grid = \[\[1,0,1],[1,0,0],[1,0,0]]

**Output: **2

**Explanation:**

There are two right triangles.

**Constraints:**

*   `1 <= grid.length <= 1000`
*   `1 <= grid[i].length <= 1000`
*   `0 <= grid[i][j] <= 1`

## Solution

```java
public class Solution {
    public long numberOfRightTriangles(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int[] columns = new int[n];
        int[] rows = new int[m];
        long sum = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                columns[i] += grid[i][j];
                rows[j] += grid[i][j];
            }
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                sum += (long) grid[i][j] * (rows[j] - 1) * (columns[i] - 1);
            }
        }
        return sum;
    }
}
```