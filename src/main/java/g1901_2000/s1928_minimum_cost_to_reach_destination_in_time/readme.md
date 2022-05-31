## 1928\. Minimum Cost to Reach Destination in Time

Hard

There is a country of `n` cities numbered from `0` to `n - 1` where **all the cities are connected** by bi-directional roads. The roads are represented as a 2D integer array `edges` where <code>edges[i] = [x<sub>i</sub>, y<sub>i</sub>, time<sub>i</sub>]</code> denotes a road between cities <code>x<sub>i</sub></code> and <code>y<sub>i</sub></code> that takes <code>time<sub>i</sub></code> minutes to travel. There may be multiple roads of differing travel times connecting the same two cities, but no road connects a city to itself.

Each time you pass through a city, you must pay a passing fee. This is represented as a **0-indexed** integer array `passingFees` of length `n` where `passingFees[j]` is the amount of dollars you must pay when you pass through city `j`.

In the beginning, you are at city `0` and want to reach city `n - 1` in `maxTime` **minutes or less**. The **cost** of your journey is the **summation of passing fees** for each city that you passed through at some moment of your journey (**including** the source and destination cities).

Given `maxTime`, `edges`, and `passingFees`, return _the **minimum cost** to complete your journey, or_ `-1` _if you cannot complete it within_ `maxTime` _minutes_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/04/leetgraph1-1.png)

**Input:** maxTime = 30, edges = \[\[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]

**Output:** 11

**Explanation:** The path to take is 0 -> 1 -> 2 -> 5, which takes 30 minutes and has $11 worth of passing fees.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2021/06/04/copy-of-leetgraph1-1.png)**

**Input:** maxTime = 29, edges = \[\[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]

**Output:** 48

**Explanation:** The path to take is 0 -> 3 -> 4 -> 5, which takes 26 minutes and has $48 worth of passing fees. You cannot take path 0 -> 1 -> 2 -> 5 since it would take too long.

**Example 3:**

**Input:** maxTime = 25, edges = \[\[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]

**Output:** -1

**Explanation:** There is no way to reach city 5 from city 0 within 25 minutes.

**Constraints:**

*   `1 <= maxTime <= 1000`
*   `n == passingFees.length`
*   `2 <= n <= 1000`
*   `n - 1 <= edges.length <= 1000`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= n - 1</code>
*   <code>1 <= time<sub>i</sub> <= 1000</code>
*   `1 <= passingFees[j] <= 1000`
*   The graph may contain multiple edges between two nodes.
*   The graph does not contain self loops.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.PriorityQueue;

public class Solution {
    public int minCost(int maxTime, int[][] edges, int[] passingFees) {
        final PriorityQueue<Tuple> pq =
                new PriorityQueue<>((a, b) -> a.cost == b.cost ? a.time - b.time : a.cost - b.cost);
        final int n = passingFees.length;
        final int[] minTime = new int[n];
        Arrays.fill(minTime, Integer.MAX_VALUE);
        final Graph graph = new Graph();
        for (final int[] edge : edges) {
            graph.addEdge(edge[0], edge[1], edge[2]);
        }
        pq.offer(new Tuple(0, passingFees[0], 0));
        while (!pq.isEmpty()) {
            final Tuple curr = pq.poll();
            if (curr.time <= maxTime && curr.time < minTime[curr.node]) {
                minTime[curr.node] = curr.time;
                if (curr.node == n - 1) {
                    return curr.cost;
                }
                for (final Edge edge : graph.getEdges(curr.node)) {
                    final int time = curr.time + edge.weight;
                    if (time > maxTime || time >= minTime[edge.dst]) {
                        continue;
                    }
                    pq.offer(new Tuple(edge.dst, curr.cost + passingFees[edge.dst], time));
                }
            }
        }
        return -1;
    }

    private static class Graph {
        private final Map<Integer, List<Edge>> edges = new HashMap<>();

        private void addEdge(final int src, final int dst, final int weight) {
            this.edges.computeIfAbsent(src, k -> new ArrayList<>()).add(new Edge(dst, weight));
            this.edges.computeIfAbsent(dst, k -> new ArrayList<>()).add(new Edge(src, weight));
        }

        private List<Edge> getEdges(final int node) {
            return this.edges.getOrDefault(node, new ArrayList<>());
        }
    }

    private static final class Edge {
        private final int dst;
        private final int weight;

        private Edge(final int dst, final int weight) {
            this.dst = dst;
            this.weight = weight;
        }
    }

    private static class Tuple {
        private final int node;
        private final int cost;
        private final int time;

        private Tuple(final int node, final int cost, final int time) {
            this.node = node;
            this.cost = cost;
            this.time = time;
        }
    }
}
```