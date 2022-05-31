## 847\. Shortest Path Visiting All Nodes

Hard

You have an undirected, connected graph of `n` nodes labeled from `0` to `n - 1`. You are given an array `graph` where `graph[i]` is a list of all the nodes connected with node `i` by an edge.

Return _the length of the shortest path that visits every node_. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg)

**Input:** graph = [[1,2,3],[0],[0],[0]]

**Output:** 4

**Explanation:** One possible path is [1,0,2,0,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg)

**Input:** graph = [[1],[0,2,4],[1,3,4],[2],[1,2]]

**Output:** 4

**Explanation:** One possible path is [0,1,4,2,3]

**Constraints:**

*   `n == graph.length`
*   `1 <= n <= 12`
*   `0 <= graph[i].length < n`
*   `graph[i]` does not contain `i`.
*   If `graph[a]` contains `b`, then `graph[b]` contains `a`.
*   The input graph is always connected.

## Solution

```java
import java.util.ArrayDeque;
import java.util.Objects;
import java.util.Queue;

public class Solution {
    public int shortestPathLength(int[][] graph) {
        int target = (1 << graph.length) - 1;
        Queue<int[]> q = new ArrayDeque<>();
        for (int i = 0; i < graph.length; ++i) {
            q.offer(new int[] {i, 1 << i});
        }
        int steps = 0;
        boolean[][] visited = new boolean[graph.length][target + 1];
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                int[] curr = q.poll();
                int currNode = Objects.requireNonNull(curr)[0];
                int currState = curr[1];
                if (currState == target) {
                    return steps;
                }
                for (int n : graph[currNode]) {
                    int newState = currState | 1 << n;
                    if (visited[n][newState]) {
                        continue;
                    }
                    visited[n][newState] = true;
                    q.offer(new int[] {n, newState});
                }
            }
            ++steps;
        }
        return -1;
    }
}
```