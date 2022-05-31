## 1443\. Minimum Time to Collect All Apples in a Tree

Medium

Given an undirected tree consisting of `n` vertices numbered from `0` to `n-1`, which has some apples in their vertices. You spend 1 second to walk over one edge of the tree. _Return the minimum time in seconds you have to spend to collect all apples in the tree, starting at **vertex 0** and coming back to this vertex._

The edges of the undirected tree are given in the array `edges`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> means that exists an edge connecting the vertices <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>. Additionally, there is a boolean array `hasApple`, where `hasApple[i] = true` means that vertex `i` has an apple; otherwise, it does not have any apple.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_1.png)

**Input:** n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]

**Output:** 8

**Explanation:** The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_2.png)

**Input:** n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]

**Output:** 6

**Explanation:** The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.

**Example 3:**

**Input:** n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]

**Output:** 0

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub> < b<sub>i</sub> <= n - 1</code>
*   <code>from<sub>i</sub> < to<sub>i</sub></code>
*   `hasApple.length == n`

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

@SuppressWarnings("java:S1172")
public class Solution {
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        Set<Integer> visited = new HashSet<>();
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] edge : edges) {
            int vertexA = edge[0];
            int vertexB = edge[1];
            graph.computeIfAbsent(vertexA, key -> new ArrayList<>()).add(vertexB);
            graph.computeIfAbsent(vertexB, key -> new ArrayList<>()).add(vertexA);
        }
        visited.add(0);
        int steps = helper(graph, hasApple, 0, visited);
        return steps > 0 ? steps - 2 : 0;
    }

    private int helper(
            Map<Integer, List<Integer>> graph,
            List<Boolean> hasApple,
            int node,
            Set<Integer> visited) {
        int steps = 0;
        for (int child : graph.getOrDefault(node, Collections.emptyList())) {
            if (visited.contains(child)) {
                continue;
            } else {
                visited.add(child);
            }
            steps += helper(graph, hasApple, child, visited);
        }
        if (steps > 0) {
            return steps + 2;
        } else if (Boolean.TRUE.equals(hasApple.get(node))) {
            return 2;
        } else {
            return 0;
        }
    }
}
```