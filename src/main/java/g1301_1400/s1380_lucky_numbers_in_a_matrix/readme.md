[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1380\. Lucky Numbers in a Matrix

Easy

Given an `m x n` matrix of **distinct** numbers, return _all **lucky numbers** in the matrix in **any** order_.

A **lucky number** is an element of the matrix such that it is the minimum element in its row and maximum in its column.

**Example 1:**

**Input:** matrix = \[\[3,7,8],[9,11,13],[15,16,17]]

**Output:** [15]

**Explanation:** 15 is the only lucky number since it is the minimum in its row and the maximum in its column.

**Example 2:**

**Input:** matrix = \[\[1,10,4,2],[9,3,8,7],[15,16,17,12]]

**Output:** [12]

**Explanation:** 12 is the only lucky number since it is the minimum in its row and the maximum in its column.

**Example 3:**

**Input:** matrix = \[\[7,8],[1,2]]

**Output:** [7]

**Explanation:** 7 is the only lucky number since it is the minimum in its row and the maximum in its column.

**Constraints:**

*   `m == mat.length`
*   `n == mat[i].length`
*   `1 <= n, m <= 50`
*   <code>1 <= matrix[i][j] <= 10<sup>5</sup></code>.
*   All elements in the matrix are distinct.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> luckyNumbers(int[][] matrix) {
        List<Integer> mini = new ArrayList<>();
        List<Integer> maxi = new ArrayList<>();
        for (int[] arr : matrix) {
            int min = Integer.MAX_VALUE;
            for (int j : arr) {
                if (min > j) {
                    min = j;
                }
            }
            mini.add(min);
        }
        int cols = matrix[0].length;
        for (int c = 0; c < cols; ++c) {
            int max = Integer.MIN_VALUE;
            for (int[] ints : matrix) {
                if (ints[c] > max) {
                    max = ints[c];
                }
            }
            maxi.add(max);
        }
        List<Integer> res = new ArrayList<>();
        for (int value : mini) {
            if (maxi.contains(value)) {
                res.add(value);
            }
        }
        return res;
    }
}
```