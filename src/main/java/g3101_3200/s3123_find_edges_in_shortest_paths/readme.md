[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3123\. Find Edges in Shortest Paths

Hard

You are given an undirected weighted graph of `n` nodes numbered from 0 to `n - 1`. The graph consists of `m` edges represented by a 2D array `edges`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, w<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> with weight <code>w<sub>i</sub></code>.

Consider all the shortest paths from node 0 to node `n - 1` in the graph. You need to find a **boolean** array `answer` where `answer[i]` is `true` if the edge `edges[i]` is part of **at least** one shortest path. Otherwise, `answer[i]` is `false`.

Return the array `answer`.

**Note** that the graph may not be connected.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/03/05/graph35drawio-1.png)

**Input:** n = 6, edges = \[\[0,1,4],[0,2,1],[1,3,2],[1,4,3],[1,5,1],[2,3,1],[3,5,3],[4,5,2]]

**Output:** [true,true,true,false,true,true,true,false]

**Explanation:**

The following are **all** the shortest paths between nodes 0 and 5:

*   The path `0 -> 1 -> 5`: The sum of weights is `4 + 1 = 5`.
*   The path `0 -> 2 -> 3 -> 5`: The sum of weights is `1 + 1 + 3 = 5`.
*   The path `0 -> 2 -> 3 -> 1 -> 5`: The sum of weights is `1 + 1 + 2 + 1 = 5`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/03/05/graphhhh.png)

**Input:** n = 4, edges = \[\[2,0,1],[0,1,1],[0,3,4],[3,2,2]]

**Output:** [true,false,false,true]

**Explanation:**

There is one shortest path between nodes 0 and 3, which is the path `0 -> 2 -> 3` with the sum of weights `1 + 2 = 3`.

**Constraints:**

*   <code>2 <= n <= 5 * 10<sup>4</sup></code>
*   `m == edges.length`
*   <code>1 <= m <= min(5 * 10<sup>4</sup>, n * (n - 1) / 2)</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   <code>1 <= w<sub>i</sub> <= 10<sup>5</sup></code>
*   There are no repeated edges.

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;

@SuppressWarnings({"java:S135", "java:S2234"})
public class Solution {
    private int[] edge;
    private int[] weight;
    private int[] next;
    private int[] head;
    private int index;

    private void add(int u, int v, int w) {
        edge[index] = v;
        weight[index] = w;
        next[index] = head[u];
        head[u] = index++;
    }

    public boolean[] findAnswer(int n, int[][] edges) {
        int m = edges.length;
        edge = new int[m << 1];
        weight = new int[m << 1];
        next = new int[m << 1];
        head = new int[n];
        for (int i = 0; i < n; ++i) {
            head[i] = -1;
        }
        index = 0;
        for (int[] localEdge : edges) {
            int u = localEdge[0];
            int v = localEdge[1];
            int w = localEdge[2];
            add(u, v, w);
            add(v, u, w);
        }
        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> a[1] < b[1] ? -1 : 1);
        long[] distances = new long[n];
        Arrays.fill(distances, (long) 1e12);
        pq.offer(new long[] {0, 0});
        distances[0] = 0;
        while (!pq.isEmpty()) {
            long[] cur = pq.poll();
            int u = (int) cur[0];
            long distance = cur[1];
            if (distance > distances[u]) {
                continue;
            }
            if (u == n - 1) {
                break;
            }
            for (int localIndex = head[u]; localIndex != -1; localIndex = next[localIndex]) {
                int v = edge[localIndex];
                int w = weight[localIndex];
                long newDistance = distance + w;
                if (newDistance < distances[v]) {
                    distances[v] = newDistance;
                    pq.offer(new long[] {v, newDistance});
                }
            }
        }
        boolean[] ans = new boolean[m];
        if (distances[n - 1] >= (long) 1e12) {
            return ans;
        }
        dfs(distances, n - 1, -1, ans);
        return ans;
    }

    private void dfs(long[] distances, int u, int pre, boolean[] ans) {
        for (int localIndex = head[u]; localIndex != -1; localIndex = next[localIndex]) {
            int v = edge[localIndex];
            int w = weight[localIndex];
            int i = localIndex >> 1;
            if (distances[v] + w != distances[u]) {
                continue;
            }
            ans[i] = true;
            if (v == pre) {
                continue;
            }
            dfs(distances, v, u, ans);
        }
    }
}
```