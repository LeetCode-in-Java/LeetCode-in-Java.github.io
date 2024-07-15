[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3203\. Find Minimum Diameter After Merging Two Trees

Hard

There exist two **undirected** trees with `n` and `m` nodes, numbered from `0` to `n - 1` and from `0` to `m - 1`, respectively. You are given two 2D integer arrays `edges1` and `edges2` of lengths `n - 1` and `m - 1`, respectively, where <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the first tree and <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> in the second tree.

You must connect one node from the first tree with another node from the second tree with an edge.

Return the **minimum** possible **diameter** of the resulting tree.

The **diameter** of a tree is the length of the _longest_ path between any two nodes in the tree.

**Example 1:**![](https://assets.leetcode.com/uploads/2024/04/22/example11-transformed.png)

**Input:** edges1 = \[\[0,1],[0,2],[0,3]], edges2 = \[\[0,1]]

**Output:** 3

**Explanation:**

We can obtain a tree of diameter 3 by connecting node 0 from the first tree with any node from the second tree.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/04/22/example211.png)

**Input:** edges1 = \[\[0,1],[0,2],[0,3],[2,4],[2,5],[3,6],[2,7]], edges2 = \[\[0,1],[0,2],[0,3],[2,4],[2,5],[3,6],[2,7]]

**Output:** 5

**Explanation:**

We can obtain a tree of diameter 5 by connecting node 0 from the first tree with node 0 from the second tree.

**Constraints:**

*   <code>1 <= n, m <= 10<sup>5</sup></code>
*   `edges1.length == n - 1`
*   `edges2.length == m - 1`
*   `edges1[i].length == edges2[i].length == 2`
*   <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < m</code>
*   The input is generated such that `edges1` and `edges2` represent valid trees.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minimumDiameterAfterMerge(int[][] edges1, int[][] edges2) {
        int n = edges1.length + 1;
        int[][] g = packU(n, edges1);
        int m = edges2.length + 1;
        int[][] h = packU(m, edges2);
        int[] d1 = diameter(g);
        int[] d2 = diameter(h);
        int ans = Math.max(d1[0], d2[0]);
        ans = Math.max((d1[0] + 1) / 2 + (d2[0] + 1) / 2 + 1, ans);
        return ans;
    }

    private int[] diameter(int[][] g) {
        int n = g.length;
        int f0;
        int f1;
        int d01;
        int[] q = new int[n];
        boolean[] ved = new boolean[n];
        int qp = 0;
        q[qp++] = 0;
        ved[0] = true;
        for (int i = 0; i < qp; i++) {
            int cur = q[i];
            for (int e : g[cur]) {
                if (!ved[e]) {
                    ved[e] = true;
                    q[qp++] = e;
                }
            }
        }
        f0 = q[n - 1];
        int[] d = new int[n];
        qp = 0;
        Arrays.fill(ved, false);
        q[qp++] = f0;
        ved[f0] = true;
        for (int i = 0; i < qp; i++) {
            int cur = q[i];
            for (int e : g[cur]) {
                if (!ved[e]) {
                    ved[e] = true;
                    q[qp++] = e;
                    d[e] = d[cur] + 1;
                }
            }
        }
        f1 = q[n - 1];
        d01 = d[f1];
        return new int[] {d01, f0, f1};
    }

    private int[][] packU(int n, int[][] ft) {
        int[][] g = new int[n][];
        int[] p = new int[n];
        for (int[] u : ft) {
            p[u[0]]++;
            p[u[1]]++;
        }
        for (int i = 0; i < n; i++) {
            g[i] = new int[p[i]];
        }
        for (int[] u : ft) {
            g[u[0]][--p[u[0]]] = u[1];
            g[u[1]][--p[u[1]]] = u[0];
        }
        return g;
    }
}
```