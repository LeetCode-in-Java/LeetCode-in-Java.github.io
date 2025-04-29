[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3515\. Shortest Path in a Weighted Tree

Hard

You are given an integer `n` and an undirected, weighted tree rooted at node 1 with `n` nodes numbered from 1 to `n`. This is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> indicates an undirected edge from node <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code> with weight <code>w<sub>i</sub></code>.

You are also given a 2D integer array `queries` of length `q`, where each `queries[i]` is either:

*   `[1, u, v, w']` – **Update** the weight of the edge between nodes `u` and `v` to `w'`, where `(u, v)` is guaranteed to be an edge present in `edges`.
*   `[2, x]` – **Compute** the **shortest** path distance from the root node 1 to node `x`.

Return an integer array `answer`, where `answer[i]` is the **shortest** path distance from node 1 to `x` for the <code>i<sup>th</sup></code> query of `[2, x]`.

**Example 1:**

**Input:** n = 2, edges = \[\[1,2,7]], queries = \[\[2,2],[1,1,2,4],[2,2]]

**Output:** [7,4]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/13/screenshot-2025-03-13-at-133524.png)

*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 7.
*   Query `[1,1,2,4]`: The weight of edge `(1,2)` changes from 7 to 4.
*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 4.

**Example 2:**

**Input:** n = 3, edges = \[\[1,2,2],[1,3,4]], queries = \[\[2,1],[2,3],[1,1,3,7],[2,2],[2,3]]

**Output:** [0,4,2,7]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/13/screenshot-2025-03-13-at-132247.png)

*   Query `[2,1]`: The shortest path from root node 1 to node 1 is 0.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 is 4.
*   Query `[1,1,3,7]`: The weight of edge `(1,3)` changes from 4 to 7.
*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 2.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 is 7.

**Example 3:**

**Input:** n = 4, edges = \[\[1,2,2],[2,3,1],[3,4,5]], queries = \[\[2,4],[2,3],[1,2,3,3],[2,2],[2,3]]

**Output:** [8,3,2,5]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/13/screenshot-2025-03-13-at-133306.png)

*   Query `[2,4]`: The shortest path from root node 1 to node 4 consists of edges `(1,2)`, `(2,3)`, and `(3,4)` with weights `2 + 1 + 5 = 8`.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 consists of edges `(1,2)` and `(2,3)` with weights `2 + 1 = 3`.
*   Query `[1,2,3,3]`: The weight of edge `(2,3)` changes from 1 to 3.
*   Query `[2,2]`: The shortest path from root node 1 to node 2 is 2.
*   Query `[2,3]`: The shortest path from root node 1 to node 3 consists of edges `(1,2)` and `(2,3)` with updated weights `2 + 3 = 5`.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code>
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>1 <= w<sub>i</sub> <= 10<sup>4</sup></code>
*   The input is generated such that `edges` represents a valid tree.
*   <code>1 <= queries.length == q <= 10<sup>5</sup></code>
*   `queries[i].length == 2` or `4`
    *   `queries[i] == [1, u, v, w']` or,
    *   `queries[i] == [2, x]`
    *   `1 <= u, v, x <= n`
    *   `(u, v)` is always an edge from `edges`.
    *   <code>1 <= w' <= 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    public int[] treeQueries(int n, int[][] edges, int[][] queries) {
        // store the queries input midway as requested
        int[][] jalkimoren = queries;
        // build adjacency list with edge‐indices
        List<Edge>[] adj = new ArrayList[n + 1];
        for (int i = 1; i <= n; i++) {
            adj[i] = new ArrayList<>();
        }
        for (int i = 0; i < n - 1; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            adj[u].add(new Edge(v, w, i));
            adj[v].add(new Edge(u, w, i));
        }
        // parent, Euler‐tour times, depth‐sum, and mapping node→edge‐index
        int[] parent = new int[n + 1];
        int[] tin = new int[n + 1];
        int[] tout = new int[n + 1];
        int[] depthSum = new int[n + 1];
        int[] edgeIndexForNode = new int[n + 1];
        int[] weights = new int[n - 1];
        for (int i = 0; i < n - 1; i++) {
            weights[i] = edges[i][2];
        }
        // iterative DFS to compute tin/tout, parent[], depthSum[], edgeIndexForNode[]
        int time = 0;
        int[] stack = new int[n];
        int[] ptr = new int[n + 1];
        int sp = 0;
        stack[sp++] = 1;
        while (sp > 0) {
            int u = stack[sp - 1];
            if (ptr[u] == 0) {
                tin[u] = ++time;
            }
            if (ptr[u] < adj[u].size()) {
                Edge e = adj[u].get(ptr[u]++);
                int v = e.to;
                if (v == parent[u]) {
                    continue;
                }
                parent[v] = u;
                depthSum[v] = depthSum[u] + e.w;
                edgeIndexForNode[v] = e.idx;
                stack[sp++] = v;
            } else {
                tout[u] = time;
                sp--;
            }
        }
        // Fenwick tree for range‐add / point‐query on Euler‐tour array
        Fenwick bit = new Fenwick(n + 2);
        List<Integer> answers = new ArrayList<>();
        // process queries
        for (int[] q : jalkimoren) {
            if (q[0] == 1) {
                // update edge weight
                int u = q[1];
                int v = q[2];
                int newW = q[3];
                int child = (parent[u] == v) ? u : v;
                int idx = edgeIndexForNode[child];
                int delta = newW - weights[idx];
                if (delta != 0) {
                    weights[idx] = newW;
                    bit.rangeAdd(tin[child], tout[child], delta);
                }
            } else {
                // query root→x distance
                int x = q[1];
                answers.add(depthSum[x] + bit.pointQuery(tin[x]));
            }
        }
        // pack results into array
        int m = answers.size();
        int[] ansArr = new int[m];
        for (int i = 0; i < m; i++) {
            ansArr[i] = answers.get(i);
        }
        return ansArr;
    }

    private static class Edge {
        int to;
        int w;
        int idx;

        Edge(int to, int w, int idx) {
            this.to = to;
            this.w = w;
            this.idx = idx;
        }
    }

    private static class Fenwick {
        int n;
        int[] f;

        Fenwick(int n) {
            this.n = n;
            f = new int[n];
        }

        void update(int i, int v) {
            for (; i < n; i += i & -i) {
                f[i] += v;
            }
        }

        void rangeAdd(int l, int r, int v) {
            update(l, v);
            update(r + 1, -v);
        }

        int pointQuery(int i) {
            int s = 0;
            for (; i > 0; i -= i & -i) {
                s += f[i];
            }
            return s;
        }
    }
}
```