[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 598\. Range Addition II

Easy

You are given an `m x n` matrix `M` initialized with all `0`'s and an array of operations `ops`, where <code>ops[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> means `M[x][y]` should be incremented by one for all <code>0 <= x < a<sub>i</sub></code> and <code>0 <= y < b<sub>i</sub></code>.

Count and return _the number of maximum integers in the matrix after performing all the operations_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/02/ex1.jpg)

**Input:** m = 3, n = 3, ops = \[\[2,2],[3,3]]

**Output:** 4

**Explanation:** The maximum integer in M is 2, and there are four of it in M. So return 4. 

**Example 2:**

**Input:** m = 3, n = 3, ops = \[\[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3]]

**Output:** 4 

**Example 3:**

**Input:** m = 3, n = 3, ops = []

**Output:** 9 

**Constraints:**

*   <code>1 <= m, n <= 4 * 10<sup>4</sup></code>
*   <code>0 <= ops.length <= 10<sup>4</sup></code>
*   `ops[i].length == 2`
*   <code>1 <= a<sub>i</sub> <= m</code>
*   <code>1 <= b<sub>i</sub> <= n</code>

## Solution

```java
public class Solution {
    /*
     * Since the incrementing starts from zero to op[0] and op[1], we only need to find the range that
     * has the most overlaps. Thus we keep finding the minimum of both x and y.
     */
    public int maxCount(int m, int n, int[][] ops) {
        int x = m;
        int y = n;
        for (int[] op : ops) {
            x = Math.min(x, op[0]);
            y = Math.min(y, op[1]);
        }
        return x * y;
    }
}
```