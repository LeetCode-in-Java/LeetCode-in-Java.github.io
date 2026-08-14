[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3887\. Incremental Even-Weighted Cycle Queries

Hard

You are given a positive integer `n`.

There is an **undirected** graph with `n` nodes labeled from 0 to `n - 1`. Initially, the graph has no edges.

You are also given a 2D integer array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> represents an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with weight <code>w<sub>i</sub></code>. The weight <code>w<sub>i</sub></code> is either 0 or 1.

Process the edges in `edges` in the given order. For each edge, add it to the graph only if, after adding it, the sum of the weights of the edges in **every** cycle in the resulting graph is **even**.

Return an integer denoting the number of edges that are successfully added to the graph.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1,1],[1,2,1],[0,2,1]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/03/21/hmadizgovu.png)

*   `[0, 1, 1]`: We add the edge between vertex 0 and vertex 1 with weight 1.
*   `[1, 2, 1]`: We add the edge between vertex 1 and vertex 2 with weight 1.
*   `[0, 2, 1]`: The edge between vertex 0 and vertex 2 (the dashed edge in the diagram) is not added because the cycle `0 - 1 - 2 - 0` has total edge weight `1 + 1 + 1 = 3`, which is an odd number.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1,1],[1,2,1],[0,2,0]]

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2026/03/21/rbdgrefwok.png)

*   `[0, 1, 1]`: We add the edge between vertex 0 and vertex 1 with weight 1.
*   `[1, 2, 1]`: We add the edge between vertex 1 and vertex 2 with weight 1.
*   `[0, 2, 0]`: We add the edge between vertex 0 and vertex 2 with weight 0.
*   Note that the cycle `0 - 1 - 2 - 0` has total edge weight `1 + 1 + 0 = 2`, which is an even number.

**Constraints:**

*   <code>3 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= edges.length <= 5 * 10<sup>4</sup></code>
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub> < v<sub>i</sub> < n</code>
*   All edges are distinct.
*   <code>w<sub>i</sub> = 0 or w<sub>i</sub> = 1</code>

## Solution

```java
public class Solution {
    private int[] parent;
    private int[] parity;

    private int find(int x) {
        if (parent[x] == x) {
            return x;
        }
        int p = parent[x];
        parent[x] = find(parent[x]);
        parity[x] ^= parity[p];
        return parent[x];
    }

    public int numberOfEdgesAdded(int n, int[][] edges) {
        parent = new int[n];
        int[] rank = new int[n];
        parity = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
        int ans = 0;
        for (int[] e : edges) {
            int u = e[0];
            int v = e[1];
            int w = e[2];
            int ru = find(u);
            int rv = find(v);
            int pu = parity[u];
            int pv = parity[v];
            if (ru == rv) {
                if ((pu ^ pv) == w) {
                    ans++;
                }
            } else {
                if (rank[ru] < rank[rv]) {
                    int temp = ru;
                    ru = rv;
                    rv = temp;
                    temp = pu;
                    pu = pv;
                    pv = temp;
                }
                parent[rv] = ru;
                parity[rv] = pu ^ pv ^ w;
                if (rank[ru] == rank[rv]) {
                    rank[ru]++;
                }
                ans++;
            }
        }
        return ans;
    }
}
```