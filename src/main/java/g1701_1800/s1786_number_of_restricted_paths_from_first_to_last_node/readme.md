[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1786\. Number of Restricted Paths From First to Last Node

Medium

There is an undirected weighted connected graph. You are given a positive integer `n` which denotes that the graph has `n` nodes labeled from `1` to `n`, and an array `edges` where each <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, weight<sub>i</sub>]</code> denotes that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with weight equal to <code>weight<sub>i</sub></code>.

A path from node `start` to node `end` is a sequence of nodes <code>[z<sub>0</sub>, z<sub>1</sub>, z<sub>2</sub>, ..., z<sub>k</sub>]</code> such that <code>z<sub>0</sub> = start</code> and <code>z<sub>k</sub> = end</code> and there is an edge between <code>z<sub>i</sub></code> and <code>z<sub>i+1</sub></code> where `0 <= i <= k-1`.

The distance of a path is the sum of the weights on the edges of the path. Let `distanceToLastNode(x)` denote the shortest distance of a path between node `n` and node `x`. A **restricted path** is a path that also satisfies that <code>distanceToLastNode(z<sub>i</sub>) > distanceToLastNode(z<sub>i+1</sub>)</code> where `0 <= i <= k-1`.

Return _the number of restricted paths from node_ `1` _to node_ `n`. Since that number may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex1.png)

**Input:** n = 5, edges = \[\[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]

**Output:** 3

**Explanation:** Each circle contains the node number in black and its `distanceToLastNode value in blue.` The three restricted paths are:

1) 1 --> 2 --> 5

2) 1 --> 2 --> 3 --> 5

3) 1 --> 3 --> 5 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex22.png)

**Input:** n = 7, edges = \[\[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]

**Output:** 1

**Explanation:** Each circle contains the node number in black and its `distanceToLastNode value in blue.` The only restricted path is 1 --> 3 --> 7. 

**Constraints:**

*   <code>1 <= n <= 2 * 10<sup>4</sup></code>
*   <code>n - 1 <= edges.length <= 4 * 10<sup>4</sup></code>
*   `edges[i].length == 3`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= weight<sub>i</sub> <= 10<sup>5</sup></code>
*   There is at most one edge between any two nodes.
*   There is at least one path between any two nodes.

## Solution

```java
import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;

@SuppressWarnings("java:S1210")
public class Solution {
    private static class Pair implements Comparable<Pair> {
        int v;
        int cwt;

        Pair(int v, int cwt) {
            this.v = v;
            this.cwt = cwt;
        }

        public int compareTo(Pair o) {
            return this.cwt - o.cwt;
        }
    }

    private static class Edge {
        int v;
        int wt;

        Edge(int v, int wt) {
            this.v = v;
            this.wt = wt;
        }
    }

    private int[] dtl;
    private int[] dp;
    private static final int M = 1000000007;

    public int countRestrictedPaths(int n, int[][] edges) {
        List<List<Edge>> graph = buildGraph(n, edges);
        PriorityQueue<Pair> pq = new PriorityQueue<>();
        boolean[] vis = new boolean[n + 1];
        dtl = new int[n + 1];
        pq.add(new Pair(n, 0));
        while (!pq.isEmpty()) {
            Pair rem = pq.remove();
            if (vis[rem.v]) {
                continue;
            }
            dtl[rem.v] = rem.cwt;
            vis[rem.v] = true;
            for (Edge edge : graph.get(rem.v)) {
                if (!vis[edge.v]) {
                    pq.add(new Pair(edge.v, rem.cwt + edge.wt));
                }
            }
        }
        dp = new int[n + 1];
        return dfs(graph, 1, new boolean[n + 1], n);
    }

    private int dfs(List<List<Edge>> graph, int vtx, boolean[] vis, int n) {
        if (vtx == n) {
            return 1;
        }
        long ans = 0;
        vis[vtx] = true;
        for (Edge edge : graph.get(vtx)) {
            if (!vis[edge.v] && dtl[edge.v] < dtl[vtx]) {
                int x = dfs(graph, edge.v, vis, n);
                ans = (ans + x) % M;
            } else if (dtl[edge.v] < dtl[vtx] && vis[edge.v]) {
                ans = (ans + dp[edge.v]) % M;
            }
        }
        dp[vtx] = (int) ans;
        return (int) ans;
    }

    private List<List<Edge>> buildGraph(int n, int[][] edges) {
        List<List<Edge>> graph = new ArrayList<>();
        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int wt = edge[2];
            graph.get(u).add(new Edge(v, wt));
            graph.get(v).add(new Edge(u, wt));
        }
        return graph;
    }
}
```