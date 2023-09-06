[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2608\. Shortest Cycle in a Graph

Hard

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1`. The edges in the graph are represented by a given 2D integer array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> denotes an edge between vertex <code>u<sub>i</sub></code> and vertex <code>v<sub>i</sub></code>. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

Return _the length of the **shortest** cycle in the graph_. If no cycle exists, return `-1`.

A cycle is a path that starts and ends at the same node, and each edge in the path is used only once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/01/04/cropped.png)

**Input:** n = 7, edges = \[\[0,1],[1,2],[2,0],[3,4],[4,5],[5,6],[6,3]]

**Output:** 3

**Explanation:** The cycle with the smallest length is : 0 -> 1 -> 2 -> 0

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/01/04/croppedagin.png)

**Input:** n = 4, edges = \[\[0,1],[0,2]]

**Output:** -1

**Explanation:** There are no cycles in this graph.

**Constraints:**

*   `2 <= n <= 1000`
*   `1 <= edges.length <= 1000`
*   `edges[i].length == 2`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   There are no repeated edges.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    private int output = 1001;

    public int findShortestCycle(int n, int[][] edges) {
        List<Integer>[] nexts = new ArrayList[n];
        int[] ranks = new int[n];
        for (int i = 0; i < n; i++) {
            nexts[i] = new ArrayList<>();
        }
        for (int[] edge : edges) {
            for (int i = 0; i < 2; i++) {
                nexts[edge[i]].add(edge[1 - i]);
            }
        }
        for (int i = 0; i < n; i++) {
            if (ranks[i] == 0) {
                findShortestCycle(nexts, i, -1, -1001, ranks);
            }
        }
        return output == 1001 ? -1 : output;
    }

    private void findShortestCycle(List<Integer>[] nexts, int c, int p, int r, int[] ranks) {
        ranks[c] = r;
        for (int n : nexts[c]) {
            if (n != p) {
                if (ranks[n] > r + 1) {
                    findShortestCycle(nexts, n, c, r + 1, ranks);
                } else if (ranks[c] > ranks[n]) {
                    output = Math.min(output, ranks[c] - ranks[n] + 1);
                }
            }
        }
    }
}
```