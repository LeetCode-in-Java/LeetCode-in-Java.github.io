[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1572\. Matrix Diagonal Sum

Easy

Given a square matrix `mat`, return the sum of the matrix diagonals.

Only include the sum of all the elements on the primary diagonal and all the elements on the secondary diagonal that are not part of the primary diagonal.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/14/sample_1911.png)

**Input:** mat = \[\[**1**,2,**3**], 
                  [4,**5**,6], 
                  [**7**,8,**9**]]

**Output:** 25

**Explanation:** Diagonals sum: 1 + 5 + 9 + 3 + 7 = 25 Notice that element mat[1][1] = 5 is counted only once.

**Example 2:**

**Input:** mat = \[\[**1**,1,1,**1**], 
                  [1,**1**,**1**,1], 
                  [1,**1**,**1**,1], 
                  [**1**,1,1,**1**]]

**Output:** 8

**Example 3:**

**Input:** mat = \[\[**5**]]

**Output:** 5

**Constraints:**

*   `n == mat.length == mat[i].length`
*   `1 <= n <= 100`
*   `1 <= mat[i][j] <= 100`

## Solution

```java
class Solution {
    public int diagonalSum(int[][] mat) {
        int len = mat.length;
        int sum = 0;
        for (int i = 0; i < len; i++) {
            sum += mat[i][i] + mat[i][len - 1 - i];
        }
        if (len % 2 != 0) {
            sum -= mat[len / 2][len / 2];
        }
        return sum;
    }
}
```