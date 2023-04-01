[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 587\. Erect the Fence

Hard

You are given an array `trees` where <code>trees[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the location of a tree in the garden.

You are asked to fence the entire garden using the minimum length of rope as it is expensive. The garden is well fenced only if **all the trees are enclosed**.

Return _the coordinates of trees that are exactly located on the fence perimeter_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/24/erect2-plane.jpg)

**Input:** points = \[\[1,1],[2,2],[2,0],[2,4],[3,3],[4,2]]

**Output:** [[1,1],[2,0],[3,3],[2,4],[4,2]] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/24/erect1-plane.jpg)

**Input:** points = \[\[1,2],[2,2],[4,2]]

**Output:** [[4,2],[2,2],[1,2]] 

**Constraints:**

*   `1 <= points.length <= 3000`
*   `points[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 100</code>
*   All the given points are **unique**.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int MAX = 100;

    public int[][] outerTrees(int[][] trees) {
        int n = trees.length;
        if (n <= 2) {
            return trees;
        }
        radixSort2D(trees);
        int[][] st = new int[n * 2][];
        int idx = 0;
        for (int[] t : trees) {
            while (idx > 1 && polarOrder(st[idx - 2], st[idx - 1], t) < 0) {
                idx--;
            }
            st[idx++] = t;
        }
        for (int i = n - 1; i >= 0; i--) {
            while (idx > 1 && polarOrder(st[idx - 2], st[idx - 1], trees[i]) < 0) {
                idx--;
            }
            st[idx++] = trees[i];
        }
        return Arrays.stream(st, 0, idx).distinct().toArray(int[][]::new);
    }

    private void radixSort2D(int[][] trees) {
        int[][] aux = new int[trees.length][];
        for (int p = 1; p >= 0; p--) {
            int[] count = new int[MAX + 2];
            for (int[] t : trees) {
                count[t[p] + 1]++;
            }
            for (int c = 0; c <= MAX; c++) {
                count[c + 1] += count[c];
            }
            for (int[] t : trees) {
                aux[count[t[p]]++] = t;
            }
            System.arraycopy(aux, 0, trees, 0, trees.length);
        }
    }

    private int polarOrder(int[] p, int[] q, int[] r) {
        return (q[0] - p[0]) * (r[1] - q[1]) - (q[1] - p[1]) * (r[0] - q[0]);
    }
}
```