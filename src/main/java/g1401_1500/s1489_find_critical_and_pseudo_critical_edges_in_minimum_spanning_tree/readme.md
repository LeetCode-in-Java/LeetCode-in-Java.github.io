[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1489\. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree

Hard

Given a weighted undirected connected graph with `n` vertices numbered from `0` to `n - 1`, and an array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, weight<sub>i</sub>]</code> represents a bidirectional and weighted edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>. A minimum spanning tree (MST) is a subset of the graph's edges that connects all vertices without cycles and with the minimum possible total edge weight.

Find _all the critical and pseudo-critical edges in the given graph's minimum spanning tree (MST)_. An MST edge whose deletion from the graph would cause the MST weight to increase is called a _critical edge_. On the other hand, a pseudo-critical edge is that which can appear in some MSTs but not all.

Note that you can return the indices of the edges in any order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/06/04/ex1.png)

**Input:** n = 5, edges = \[\[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]

**Output:** [[0,1],[2,3,4,5]]

**Explanation:** The figure above describes the graph.

The following figure shows all the possible MSTs:

![](https://assets.leetcode.com/uploads/2020/06/04/msts.png)

Notice that the two edges 0 and 1 appear in all MSTs, therefore they are critical edges, so we return them in the first list of the output.

The edges 2, 3, 4, and 5 are only part of some MSTs, therefore they are considered pseudo-critical edges. We add them to the second list of the output.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/06/04/ex2.png)

**Input:** n = 4, edges = \[\[0,1,1],[1,2,1],[2,3,1],[0,3,1]]

**Output:** [[],[0,1,2,3]]

**Explanation:** We can observe that since all 4 edges have equal weight, choosing any 3 edges from the given 4 will yield an MST. Therefore all 4 edges are pseudo-critical.

**Constraints:**

*   `2 <= n <= 100`
*   `1 <= edges.length <= min(200, n * (n - 1) / 2)`
*   `edges[i].length == 3`
*   <code>0 <= a<sub>i</sub> < b<sub>i</sub> < n</code>
*   <code>1 <= weight<sub>i</sub> <= 1000</code>
*   All pairs <code>(a<sub>i</sub>, b<sub>i</sub>)</code> are **distinct**.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Set;

@SuppressWarnings({"unchecked", "java:S106"})
public class Solution {
    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {
        // {w, ind}
        int[][][] g = new int[n][n][2];
        for (int i = 0; i < edges.length; i++) {
            int[] e = edges[i];
            int f = e[0];
            int t = e[1];
            int w = e[2];
            g[f][t][0] = w;
            g[t][f][0] = w;
            g[f][t][1] = i;
            g[t][f][1] = i;
        }
        List<Integer>[] mst = new List[n];
        for (int i = 0; i < n; i++) {
            mst[i] = new LinkedList<>();
        }
        boolean[] mstSet = new boolean[edges.length];
        Arrays.sort(edges, (a, b) -> Integer.compare(a[2], b[2]));
        buildMST(n, edges, mstSet, mst, g);
        List<List<Integer>> ans = new ArrayList<>(2);
        Set<Integer> pce = new HashSet<>();
        List<Integer> ce = new LinkedList<>();
        // pseudo critical edges
        for (int[] edge : edges) {
            int f = edge[0];
            int t = edge[1];
            int w = edge[2];
            int ind = g[f][t][1];
            if (!mstSet[ind]) {
                Set<Integer> cur = new HashSet<>();
                boolean p = path(f, t, w, -1, mst, g, cur);
                if (p && !cur.isEmpty()) {
                    pce.addAll(cur);
                    pce.add(ind);
                }
                if (!p) {
                    System.out.println("Should not reach here");
                }
            }
        }
        // critical edges
        for (int[] edge : edges) {
            int f = edge[0];
            int t = edge[1];
            int ind = g[f][t][1];
            if (mstSet[ind] && !pce.contains(ind)) {
                ce.add(ind);
            }
        }
        ans.add(ce);
        ans.add(new LinkedList<>(pce));
        return ans;
    }

    private boolean path(
            int f, int t, int w, int p, List<Integer>[] mst, int[][][] g, Set<Integer> ind) {
        if (f == t) {
            return true;
        }
        for (int nbr : mst[f]) {
            if (p != nbr && path(nbr, t, w, f, mst, g, ind)) {
                if (g[f][nbr][0] == w) {
                    ind.add(g[f][nbr][1]);
                }
                return true;
            }
        }
        return false;
    }

    private void buildMST(int n, int[][] edges, boolean[] mste, List<Integer>[] mstg, int[][][] g) {
        DisjointSet ds = new DisjointSet(n);
        for (int[] ints : edges) {
            if (ds.union(ints[0], ints[1])) {
                int[] edge = ints;
                mstg[edge[0]].add(edge[1]);
                mstg[edge[1]].add(edge[0]);
                mste[g[edge[0]][edge[1]][1]] = true;
            }
        }
    }

    private static class DisjointSet {
        int[] parent;

        public DisjointSet(int n) {
            parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public int find(int i) {
            if (i == parent[i]) {
                return i;
            }
            parent[i] = find(parent[i]);
            return parent[i];
        }

        public boolean union(int u, int v) {
            int pu = find(u);
            int pv = find(v);
            if (pu == pv) {
                return false;
            }
            parent[pu] = pv;
            return true;
        }
    }
}
```