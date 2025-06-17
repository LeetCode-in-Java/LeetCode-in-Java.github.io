[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3585\. Find Weighted Median Node in Tree

Hard

You are given an integer `n` and an **undirected, weighted** tree rooted at node 0 with `n` nodes numbered from 0 to `n - 1`. This is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> indicates an edge from node <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code> with weight <code>w<sub>i</sub></code>.

The **weighted median node** is defined as the **first** node `x` on the path from <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code> such that the sum of edge weights from <code>u<sub>i</sub></code> to `x` is **greater than or equal to half** of the total path weight.

You are given a 2D integer array `queries`. For each <code>queries[j] = [u<sub>j</sub>, v<sub>j</sub>]</code>, determine the weighted median node along the path from <code>u<sub>j</sub></code> to <code>v<sub>j</sub></code>.

Return an array `ans`, where `ans[j]` is the node index of the weighted median for `queries[j]`.

**Example 1:**

**Input:** n = 2, edges = \[\[0,1,7]], queries = \[\[1,0],[0,1]]

**Output:** [0,1]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/05/26/screenshot-2025-05-26-at-193447.png)

| Query      | Path     | Edge Weights  | Total Path Weight  | Half | Explanation                                           | Answer |
|------------|----------|---------------|--------------------|------|-------------------------------------------------------|--------|
| `[1, 0]`   | `1 → 0`  | `[7]`         | 7                  | 3.5  | Sum from `1 → 0 = 7 >= 3.5`, median is node 0.        | 0      |
| `[0, 1]`   | `0 → 1`  | `[7]`         | 7                  | 3.5  | Sum from `0 → 1 = 7 >= 3.5`, median is node 1.        | 1      |


**Example 2:**

**Input:** n = 3, edges = \[\[0,1,2],[2,0,4]], queries = \[\[0,1],[2,0],[1,2]]

**Output:** [1,0,2]

**E****xplanation:**

![](https://assets.leetcode.com/uploads/2025/05/26/screenshot-2025-05-26-at-193610.png)

| Query      | Path         | Edge Weights | Total Path Weight  | Half | Explanation                                                                 | Answer |
|------------|--------------|--------------|--------------------|------|-----------------------------------------------------------------------------|--------|
| `[0, 1]`   | `0 → 1`      | `[2]`        | 2                  | 1    | Sum from `0 → 1 = 2 >= 1`, median is node 1.                                | 1      |
| `[2, 0]`   | `2 → 0`      | `[4]`        | 4                  | 2    | Sum from `2 → 0 = 4 >= 2`, median is node 0.                                | 0      |
| `[1, 2]`   | `1 → 0 → 2`  | `[2, 4]`     | 6                  | 3    | Sum from `1 → 0 = 2 < 3`. <br> Sum from `1 → 2 = 2 + 4 = 6 >= 3`, median is node 2. | 2      |

**Example 3:**

**Input:** n = 5, edges = \[\[0,1,2],[0,2,5],[1,3,1],[2,4,3]], queries = \[\[3,4],[1,2]]

**Output:** [2,2]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/05/26/screenshot-2025-05-26-at-193857.png)

| Query      | Path                 | Edge Weights     | Total Path Weight | Half  | Explanation                                                                                                                                         | Answer |
|------------|----------------------|------------------|--------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| `[3, 4]`   | `3 → 1 → 0 → 2 → 4`  | `[1, 2, 5, 3]`   | 11                 | 5.5  | Sum from `3 → 1 = 1 < 5.5`.<br> Sum from `3 → 0 = 1 + 2 = 3 < 5.5`.<br> Sum from `3 → 2 = 1 + 2 + 5 = 8 >= 5.5`, median is node 2.                  | 2      |
| `[1, 2]`   | `1 → 0 → 2`          | `[2, 5]`         | 7                  | 3.5  | Sum from `1 → 0 = 2 < 3.5`.<br> Sum from `1 → 2 = 2 + 5 = 7 >= 3.5`, median is node 2.                                                              | 2      |

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>1 <= w<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[j] == [u<sub>j</sub>, v<sub>j</sub>]</code>
*   <code>0 <= u<sub>j</sub>, v<sub>j</sub> < n</code>
*   The input is generated such that `edges` represents a valid tree.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@SuppressWarnings("java:S2234")
public class Solution {
    private List<List<int[]>> adj;
    private int[] depth;
    private long[] dist;
    private int[][] parent;
    private int longMax;
    private int nodes;

    public int[] findMedian(int n, int[][] edges, int[][] queries) {
        nodes = n;
        if (n > 1) {
            longMax = (int) Math.ceil(Math.log(n) / Math.log(2));
        } else {
            longMax = 1;
        }
        adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int w = edge[2];
            adj.get(u).add(new int[] {v, w});
            adj.get(v).add(new int[] {u, w});
        }
        depth = new int[n];
        dist = new long[n];
        parent = new int[longMax][n];
        for (int i = 0; i < longMax; i++) {
            Arrays.fill(parent[i], -1);
        }
        dfs(0, -1, 0, 0L);
        buildLcaTable();
        int[] ans = new int[queries.length];
        int[] sabrelonta;
        for (int qIdx = 0; qIdx < queries.length; qIdx++) {
            sabrelonta = queries[qIdx];
            int u = sabrelonta[0];
            int v = sabrelonta[1];
            ans[qIdx] = findMedianNode(u, v);
        }

        return ans;
    }

    private void dfs(int u, int p, int d, long currentDist) {
        depth[u] = d;
        parent[0][u] = p;
        dist[u] = currentDist;
        for (int[] edge : adj.get(u)) {
            int v = edge[0];
            int w = edge[1];
            if (v == p) {
                continue;
            }
            dfs(v, u, d + 1, currentDist + w);
        }
    }

    private void buildLcaTable() {
        for (int k = 1; k < longMax; k++) {
            for (int node = 0; node < nodes; node++) {
                if (parent[k - 1][node] != -1) {
                    parent[k][node] = parent[k - 1][parent[k - 1][node]];
                }
            }
        }
    }

    private int getKthAncestor(int u, int k) {
        for (int p = longMax - 1; p >= 0; p--) {
            if (u == -1) {
                break;
            }
            if (((k >> p) & 1) == 1) {
                u = parent[p][u];
            }
        }
        return u;
    }

    private int getLCA(int u, int v) {
        if (depth[u] < depth[v]) {
            int temp = u;
            u = v;
            v = temp;
        }
        u = getKthAncestor(u, depth[u] - depth[v]);
        if (u == v) {
            return u;
        }
        for (int p = longMax - 1; p >= 0; p--) {
            if (parent[p][u] != -1 && parent[p][u] != parent[p][v]) {
                u = parent[p][u];
                v = parent[p][v];
            }
        }
        return parent[0][u];
    }

    private int findMedianNode(int u, int v) {
        if (u == v) {
            return u;
        }
        int lca = getLCA(u, v);
        long totalPathWeight = dist[u] + dist[v] - 2 * dist[lca];
        long halfWeight = (totalPathWeight + 1) / 2L;
        if (dist[u] - dist[lca] >= halfWeight) {
            int curr = u;
            for (int p = longMax - 1; p >= 0; p--) {
                int nextNode = parent[p][curr];
                if (nextNode != -1 && (dist[u] - dist[nextNode] < halfWeight)) {
                    curr = nextNode;
                }
            }
            return parent[0][curr];
        } else {
            long remainingWeightFromLCA = halfWeight - (dist[u] - dist[lca]);
            int curr = v;
            for (int p = longMax - 1; p >= 0; p--) {
                int nextNode = parent[p][curr];
                if (nextNode != -1
                        && depth[nextNode] >= depth[lca]
                        && (dist[nextNode] - dist[lca]) >= remainingWeightFromLCA) {
                    curr = nextNode;
                }
            }
            return curr;
        }
    }
}
```