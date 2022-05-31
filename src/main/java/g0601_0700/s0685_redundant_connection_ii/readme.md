## 685\. Redundant Connection II

Hard

In this problem, a rooted tree is a **directed** graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with `n` nodes (with distinct values from `1` to `n`), with one additional directed edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair <code>[u<sub>i</sub>, v<sub>i</sub>]</code> that represents a **directed** edge connecting nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>, where <code>u<sub>i</sub></code> is a parent of child <code>v<sub>i</sub></code>.

Return _an edge that can be removed so that the resulting graph is a rooted tree of_ `n` _nodes_. If there are multiple answers, return the answer that occurs last in the given 2D-array.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg)

**Input:** edges = \[\[1,2],[1,3],[2,3]]

**Output:** [2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg)

**Input:** edges = \[\[1,2],[2,3],[3,4],[4,1],[1,5]]

**Output:** [4,1]

**Constraints:**

*   `n == edges.length`
*   `3 <= n <= 1000`
*   `edges[i].length == 2`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>

## Solution

```java
public class Solution {
    private int[] par;

    public int[] findRedundantDirectedConnection(int[][] edges) {
        int n = edges.length;
        int[] haspar = new int[n + 1];
        for (int[] edge : edges) {
            int v = edge[1];
            haspar[v]++;
        }
        par = new int[n + 1];
        for (int i = 0; i < par.length; i++) {
            par[i] = i;
        }
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            if (haspar[v] == 1) {
                int lu = find(u);
                int lv = find(v);
                if (lu != lv) {
                    par[lu] = lv;
                } else {
                    return edge;
                }
            }
        }
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            if (haspar[v] > 1) {
                int lu = find(u);
                int lv = find(v);
                if (lu != lv) {
                    par[lu] = lv;
                } else {
                    return edge;
                }
            }
        }
        return new int[2];
    }

    private int find(int x) {
        if (par[x] == x) {
            return x;
        }
        return find(par[x]);
    }
}
```