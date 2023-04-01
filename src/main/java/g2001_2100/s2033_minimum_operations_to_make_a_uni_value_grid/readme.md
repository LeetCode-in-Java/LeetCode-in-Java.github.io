[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2033\. Minimum Operations to Make a Uni-Value Grid

Medium

You are given a 2D integer `grid` of size `m x n` and an integer `x`. In one operation, you can **add** `x` to or **subtract** `x` from any element in the `grid`.

A **uni-value grid** is a grid where all the elements of it are equal.

Return _the **minimum** number of operations to make the grid **uni-value**_. If it is not possible, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt.png)

**Input:** grid = \[\[2,4],[6,8]], x = 2

**Output:** 4

**Explanation:** We can make every element equal to 4 by doing the following: 

- Add x to 2 once. 

- Subtract x from 6 once. 

- Subtract x from 8 twice. 
  
A total of 4 operations were used.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-1.png)

**Input:** grid = \[\[1,5],[2,3]], x = 1

**Output:** 5

**Explanation:** We can make every element equal to 3.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/09/21/gridtxt-2.png)

**Input:** grid = \[\[1,2],[3,4]], x = 2

**Output:** -1

**Explanation:** It is impossible to make every element equal.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   <code>1 <= x, grid[i][j] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minOperations(int[][] grid, int x) {
        int[] arr = new int[grid.length * grid[0].length];
        int k = 0;
        for (int[] ints : grid) {
            for (int j = 0; j < grid[0].length; j++) {
                arr[k] = ints[j];
                k++;
            }
        }
        Arrays.sort(arr);
        int target = arr[arr.length / 2];
        int res = 0;
        for (int i = 0; i < arr.length; i++) {
            if (i < arr.length / 2) {
                int rem = target - arr[i];
                if (rem % x != 0) {
                    return -1;
                }
                res += rem / x;
            } else {
                int rem = arr[i] - target;
                if (rem % x != 0) {
                    return -1;
                }
                res += rem / x;
            }
        }
        return res;
    }
}
```