## 1129\. Shortest Path with Alternating Colors

Medium

You are given an integer `n`, the number of nodes in a directed graph where the nodes are labeled from `0` to `n - 1`. Each edge is red or blue in this graph, and there could be self-edges and parallel edges.

You are given two arrays `redEdges` and `blueEdges` where:

*   <code>redEdges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is a directed red edge from node <code>a<sub>i</sub></code> to node <code>b<sub>i</sub></code> in the graph, and
*   <code>blueEdges[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> indicates that there is a directed blue edge from node <code>u<sub>j</sub></code> to node <code>v<sub>j</sub></code> in the graph.

Return an array `answer` of length `n`, where each `answer[x]` is the length of the shortest path from node `0` to node `x` such that the edge colors alternate along the path, or `-1` if such a path does not exist.

**Example 1:**

**Input:** n = 3, redEdges = [[0,1],[1,2]], blueEdges = []

**Output:** [0,1,-1]

**Example 2:**

**Input:** n = 3, redEdges = [[0,1]], blueEdges = [[2,1]]

**Output:** [0,1,-1]

**Constraints:**

*   `1 <= n <= 100`
*   `0 <= redEdges.length, blueEdges.length <= 400`
*   `redEdges[i].length == blueEdges[j].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub>, u<sub>j</sub>, v<sub>j</sub> < n</code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    private static class Pair {
        int color;
        int node;

        Pair(int node, int color) {
            this.node = node;
            this.color = color;
        }
    }

    private void bfs(
            Queue<Integer> q,
            boolean[][] vis,
            List<List<Pair>> graph,
            boolean blue,
            int[] shortestPaths) {
        int level = 0;
        q.add(0);
        if (blue) {
            vis[0][1] = true;
        } else {
            vis[0][0] = true;
        }
        while (!q.isEmpty()) {
            int size = q.size();
            while (size-- > 0) {
                int curr = q.poll();
                shortestPaths[curr] = Math.min(level, shortestPaths[curr]);
                for (Pair nextNode : graph.get(curr)) {
                    if (nextNode.color == 1 && blue && !vis[nextNode.node][1]) {
                        q.add(nextNode.node);
                        vis[nextNode.node][1] = true;
                    }
                    if (!blue && nextNode.color == 0 && !vis[nextNode.node][0]) {
                        q.add(nextNode.node);
                        vis[nextNode.node][0] = true;
                    }
                }
            }
            blue = !blue;
            level++;
        }
    }

    public int[] shortestAlternatingPaths(int n, int[][] redEdges, int[][] blueEdges) {
        List<List<Pair>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] edge : redEdges) {
            int a = edge[0];
            int b = edge[1];
            // red -> 0
            graph.get(a).add(new Pair(b, 0));
        }
        for (int[] edge : blueEdges) {
            int u = edge[0];
            int v = edge[1];
            // blue -> 1
            graph.get(u).add(new Pair(v, 1));
        }
        int[] shortestPaths = new int[n];
        Queue<Integer> q = new LinkedList<>();
        Arrays.fill(shortestPaths, Integer.MAX_VALUE);
        bfs(q, new boolean[n][2], graph, true, shortestPaths);
        bfs(q, new boolean[n][2], graph, false, shortestPaths);
        for (int i = 0; i < n; i++) {
            if (shortestPaths[i] == Integer.MAX_VALUE) {
                shortestPaths[i] = -1;
            }
        }
        return shortestPaths;
    }
}
```