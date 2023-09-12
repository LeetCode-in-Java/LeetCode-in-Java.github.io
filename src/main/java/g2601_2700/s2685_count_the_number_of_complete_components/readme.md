[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2685\. Count the Number of Complete Components

Medium

You are given an integer `n`. There is an **undirected** graph with `n` vertices, numbered from `0` to `n - 1`. You are given a 2D integer array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> denotes that there exists an **undirected** edge connecting vertices <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>.

Return _the number of **complete connected components** of the graph_.

A **connected component** is a subgraph of a graph in which there exists a path between any two vertices, and no vertex of the subgraph shares an edge with a vertex outside of the subgraph.

A connected component is said to be **complete** if there exists an edge between every pair of its vertices.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-31-23.png)**

**Input:** n = 6, edges = \[\[0,1],[0,2],[1,2],[3,4]]

**Output:** 3

**Explanation:** From the picture above, one can see that all of the components of this graph are complete.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-32-00.png)**

**Input:** n = 6, edges = \[\[0,1],[0,2],[1,2],[3,4],[3,5]]

**Output:** 1

**Explanation:** The component containing vertices 0, 1, and 2 is complete since there is an edge between every pair of two vertices. On the other hand, the component containing vertices 3, 4, and 5 is not complete since there is no edge between vertices 4 and 5. Thus, the number of complete components in this graph is 1.

**Constraints:**

*   `1 <= n <= 50`
*   `0 <= edges.length <= n * (n - 1) / 2`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> <= n - 1</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no repeated edges.

## Solution

```java
public class Solution {
    private static class DSU {
        int[] roots;
        int[] sizes;

        DSU(int n) {
            roots = new int[n];
            sizes = new int[n];
            for (int i = 0; i < n; i++) {
                sizes[i] = 1;
                roots[i] = i;
            }
        }

        public int find(int v) {
            if (roots[v] != v) {
                roots[v] = find(roots[v]);
            }
            return roots[v];
        }

        public void union(int a, int b) {
            int rootA = find(a);
            int rootB = find(b);
            if (rootA == rootB) {
                return;
            }
            roots[rootB] = rootA;
            sizes[rootA] += sizes[rootB];
        }
    }

    public int countCompleteComponents(int n, int[][] edges) {
        DSU dsu = new DSU(n);
        int[] indegree = new int[n];
        for (int[] e : edges) {
            dsu.union(e[0], e[1]);
            indegree[e[0]]++;
            indegree[e[1]]++;
        }
        int[] gcount = new int[n];
        int res = 0;
        for (int i = 0; i < n; i++) {
            int root = dsu.find(i);
            if (dsu.sizes[root] == (indegree[i] + 1)) {
                gcount[root]++;
            }
            if (gcount[root] == dsu.sizes[root]) {
                res++;
            }
        }
        return res;
    }
}
```