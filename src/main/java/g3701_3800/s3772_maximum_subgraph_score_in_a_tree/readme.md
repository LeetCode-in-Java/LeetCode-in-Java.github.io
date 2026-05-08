[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3772\. Maximum Subgraph Score in a Tree

Hard

You are given an **undirected tree** with `n` nodes, numbered from 0 to `n - 1`. It is represented by a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

You are also given an integer array `good` of length `n`, where `good[i]` is 1 if the <code>i<sup>th</sup></code> node is good, and 0 if it is bad.

Define the **score** of a **subgraph** as the number of good nodes minus the number of bad nodes in that subgraph.

For each node `i`, find the **maximum** possible score among all **connected subgraphs** that contain node `i`.

Return an array of `n` integers where the <code>i<sup>th</sup></code> element is the **maximum** score for node `i`.

A **subgraph** is a graph whose vertices and edges are subsets of the original graph.

A **connected subgraph** is a subgraph in which every pair of its vertices is reachable from one another using only its edges.

**Example 1:**

![Tree Example 1](https://assets.leetcode.com/uploads/2025/11/17/tree1fixed.png)

**Input:** n = 3, edges = \[\[0,1],[1,2]], good = [1,0,1]

**Output:** [1,1,1]

**Explanation:**

*   Green nodes are good and red nodes are bad.
*   For each node, the best connected subgraph containing it is the whole tree, which has 2 good nodes and 1 bad node, resulting in a score of 1.
*   Other connected subgraphs containing a node may have the same score.

**Example 2:**

![Tree Example 2](https://assets.leetcode.com/uploads/2025/11/17/tree2.png)

**Input:** n = 5, edges = \[\[1,0],[1,2],[1,3],[3,4]], good = [0,1,0,1,1]

**Output:** [2,3,2,3,3]

**Explanation:**

*   Node 0: The best connected subgraph consists of nodes `0, 1, 3, 4`, which has 3 good nodes and 1 bad node, resulting in a score of `3 - 1 = 2`.
*   Nodes 1, 3, and 4: The best connected subgraph consists of nodes `1, 3, 4`, which has 3 good nodes, resulting in a score of 3.
*   Node 2: The best connected subgraph consists of nodes `1, 2, 3, 4`, which has 3 good nodes and 1 bad node, resulting in a score of `3 - 1 = 2`.

**Example 3:**

![Tree Example 3](https://assets.leetcode.com/uploads/2025/11/17/tree3.png)

**Input:** n = 2, edges = \[\[0,1]], good = [0,0]

**Output:** [-1,-1]

**Explanation:**

For each node, including the other node only adds another bad node, so the best score for both nodes is -1.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   `good.length == n`
*   `0 <= good[i] <= 1`
*   The input is generated such that `edges` represents a valid tree.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] maxSubgraphScore(int n, int[][] edges, int[] good) {
        int[] h = new int[n];
        int[] e = new int[2 * (n - 1)];
        int[] ne = new int[2 * (n - 1)];
        int idx = 0;
        Arrays.fill(h, -1);
        for (int[] ed : edges) {
            int a = ed[0];
            int b = ed[1];
            e[idx] = b;
            ne[idx] = h[a];
            h[a] = idx;
            idx++;
            e[idx] = a;
            ne[idx] = h[b];
            h[b] = idx;
            idx++;
        }
        int[] v = new int[n];
        for (int i = 0; i < n; i++) {
            v[i] = good[i] == 1 ? 1 : -1;
        }
        int[] dp = new int[n];
        int[] p = new int[n];
        int[] ord = new int[n];
        int top = 0;
        Arrays.fill(p, -1);
        int[] st = new int[n];
        int sp = 0;
        boolean[] vis = new boolean[n];
        st[sp++] = 0;
        vis[0] = true;
        while (sp > 0) {
            int u = st[--sp];
            ord[top++] = u;
            for (int i = h[u]; i != -1; i = ne[i]) {
                int w = e[i];
                if (!vis[w]) {
                    vis[w] = true;
                    p[w] = u;
                    st[sp++] = w;
                }
            }
        }
        for (int i = n - 1; i >= 0; i--) {
            int u = ord[i];
            dp[u] = v[u];
            for (int j = h[u]; j != -1; j = ne[j]) {
                int w = e[j];
                if (p[w] == u && dp[w] > 0) {
                    dp[u] += dp[w];
                }
            }
        }
        int[] ans = new int[n];
        ans[0] = dp[0];
        for (int i = 1; i < n; i++) {
            int u = ord[i];
            int par = p[u];
            int pc = ans[par] - Math.max(0, dp[u]);
            ans[u] = dp[u] + Math.max(0, pc);
        }
        return ans;
    }
}
```