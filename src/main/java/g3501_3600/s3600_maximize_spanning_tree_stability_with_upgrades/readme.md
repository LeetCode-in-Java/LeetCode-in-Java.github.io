[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3600\. Maximize Spanning Tree Stability with Upgrades

Hard

You are given an integer `n`, representing `n` nodes numbered from 0 to `n - 1` and a list of `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, s<sub>i</sub>, must<sub>i</sub>]</code>:

*   <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> indicates an undirected edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.
*   <code>s<sub>i</sub></code> is the strength of the edge.
*   <code>must<sub>i</sub></code> is an integer (0 or 1). If <code>must<sub>i</sub> == 1</code>, the edge **must** be included in the **spanning tree**. These edges **cannot** be **upgraded**.

You are also given an integer `k`, the **maximum** number of upgrades you can perform. Each upgrade **doubles** the strength of an edge, and each eligible edge (with <code>must<sub>i</sub> == 0</code>) can be upgraded **at most** once.

The **stability** of a spanning tree is defined as the **minimum** strength score among all edges included in it.

Return the **maximum** possible stability of any valid spanning tree. If it is impossible to connect all nodes, return `-1`.

**Note**: A **spanning tree** of a graph with `n` nodes is a subset of the edges that connects all nodes together (i.e. the graph is **connected**) _without_ forming any cycles, and uses **exactly** `n - 1` edges.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1,2,1],[1,2,3,0]], k = 1

**Output:** 2

**Explanation:**

*   Edge `[0,1]` with strength = 2 must be included in the spanning tree.
*   Edge `[1,2]` is optional and can be upgraded from 3 to 6 using one upgrade.
*   The resulting spanning tree includes these two edges with strengths 2 and 6.
*   The minimum strength in the spanning tree is 2, which is the maximum possible stability.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1,4,0],[1,2,3,0],[0,2,1,0]], k = 2

**Output:** 6

**Explanation:**

*   Since all edges are optional and up to `k = 2` upgrades are allowed.
*   Upgrade edges `[0,1]` from 4 to 8 and `[1,2]` from 3 to 6.
*   The resulting spanning tree includes these two edges with strengths 8 and 6.
*   The minimum strength in the tree is 6, which is the maximum possible stability.

**Example 3:**

**Input:** n = 3, edges = \[\[0,1,1,1],[1,2,1,1],[2,0,1,1]], k = 0

**Output:** \-1

**Explanation:**

*   All edges are mandatory and form a cycle, which violates the spanning tree property of acyclicity. Thus, the answer is -1.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>1 <= edges.length <= 10<sup>5</sup></code>
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, s<sub>i</sub>, must<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= s<sub>i</sub> <= 10<sup>5</sup></code>
*   <code>must<sub>i</sub></code> is either `0` or `1`.
*   `0 <= k <= n`
*   There are no duplicate edges.

## Solution

```java
public class Solution {
    public int maxStability(int n, int[][] edges, int k) {
        int low = 0;
        int high = 0;
        for (int[] edge : edges) {
            high = Math.max(high, edge[2]);
        }
        high *= 2;
        int ans = -1;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (feasible(mid, n, edges, k)) {
                ans = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return ans;
    }

    private boolean feasible(int t, int n, int[][] edges, int k) {
        int[] par = new int[n];
        int[] rnk = new int[n];
        int[] comp = new int[] {n};
        for (int i = 0; i < n; i++) {
            par[i] = i;
        }
        UnionFind uf = new UnionFind(par, rnk, comp);
        int cost = 0;
        int half = (t + 1) / 2;
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int s = edge[2];
            int m = edge[3];
            if (m == 1 && (s < t || !uf.union(u, v))) {
                return false;
            }
        }
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int s = edge[2];
            int m = edge[3];
            if (m == 0 && s >= t) {
                uf.union(u, v);
            }
        }
        if (comp[0] == 1) {
            return true;
        }
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int s = edge[2];
            int m = edge[3];
            if (m == 0 && s >= half && s < t && uf.union(u, v)) {
                cost++;
                if (cost > k) {
                    return false;
                }
            }
        }
        return comp[0] == 1;
    }

    private static class UnionFind {
        int[] par;
        int[] rnk;
        int[] comp;

        UnionFind(int[] par, int[] rnk, int[] comp) {
            this.par = par;
            this.rnk = rnk;
            this.comp = comp;
        }

        int find(int x) {
            if (par[x] != x) {
                par[x] = find(par[x]);
            }
            return par[x];
        }

        boolean union(int a, int b) {
            int ra = find(a);
            int rb = find(b);
            if (ra == rb) {
                return false;
            }
            if (rnk[ra] < rnk[rb]) {
                int temp = ra;
                ra = rb;
                rb = temp;
            }
            par[rb] = ra;
            if (rnk[ra] == rnk[rb]) {
                rnk[ra]++;
            }
            comp[0]--;
            return true;
        }
    }
}
```