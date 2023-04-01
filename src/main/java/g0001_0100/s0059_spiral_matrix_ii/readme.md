[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 59\. Spiral Matrix II

Medium

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

**Input:** n = 3

**Output:** [[1,2,3],[8,9,4],[7,6,5]] 

**Example 2:**

**Input:** n = 1

**Output:** [[1]] 

**Constraints:**

*   `1 <= n <= 20`

## Solution

```java
public class Solution {
    public int[][] generateMatrix(int n) {
        int num = 1;
        int rStart = 0;
        int rEnd = n - 1;
        int cStart = 0;
        int cEnd = n - 1;
        int[][] spiral = new int[n][n];
        while (rStart <= rEnd && cStart <= cEnd) {
            for (int k = cStart; k <= cEnd; k++) {
                spiral[rStart][k] = num++;
            }
            rStart++;
            for (int k = rStart; k <= rEnd; k++) {
                spiral[k][cEnd] = num++;
            }
            cEnd--;
            if (rStart <= rEnd) {
                for (int k = cEnd; k >= cStart; k--) {
                    spiral[rEnd][k] = num++;
                }
            }
            rEnd--;
            if (cStart <= cEnd) {
                for (int k = rEnd; k >= rStart; k--) {
                    spiral[k][cStart] = num++;
                }
            }
            cStart++;
        }
        return spiral;
    }
}
```