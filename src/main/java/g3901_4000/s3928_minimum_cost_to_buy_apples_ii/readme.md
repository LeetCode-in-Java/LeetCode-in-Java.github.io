[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3928\. Minimum Cost to Buy Apples II

Hard

You are given an integer `n` and an integer array `prices` of length `n`, where `prices[i]` is the price of apples at shop `i`.

You are also given a 2D integer array `roads`, where <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>, tax<sub>i</sub>]</code> represents a **bidirectional** road:

*   <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> are the shops connected by the road.
*   <code>cost<sub>i</sub></code> is the cost to travel the road **without** carrying apples.
*   <code>tax<sub>i</sub></code> is the multiplier applied to <code>cost<sub>i</sub></code> when traveling **with** apples.

For each shop `i`, you can either:

*   Buy apples locally at shop `i` for `prices[i]`.
*   Travel **empty** to any shop `j` using **any** number of roads, buy apples for `prices[j]`, and return to shop `i` while carrying apples, paying `cost * tax` on each road used for the return trip.

The forward path, where you travel empty, and the return path may be **different**.

Return an integer array `ans` of length `n`, where `ans[i]` is the **minimum** total cost to buy apples starting from shop `i`.

**Example 1:**

**Input:** n = 2, prices = [8,3], roads = \[\[0,1,1,2]]

**Output:** [6,3]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/08/22/screenshot-2025-08-23-at-23341-am.png)

| Shop `i` | `prices[i]` | Shop `j` | `prices[j]` | `costᵢ` | `taxᵢ` | Travel cost | Return cost |            Total |          Minimum |
| -------: | ----------: | -------: | ----------: | ------: | -----: | ----------: | ----------: | ---------------: | ---------------: |
|        0 |           8 |        1 |           3 |       1 |      2 |           1 | `1 * 2 = 2` |  `1 + 2 + 3 = 6` |  `min(8, 6) = 6` |
|        1 |           3 |        0 |           8 |       1 |      2 |           1 | `1 * 2 = 2` | `1 + 2 + 8 = 11` | `min(3, 11) = 3` |

Thus, the answer is `[6, 3]`.

**Example 2:**

**Input:** n = 3, prices = [9,4,6], roads = \[\[0,1,1,3],[1,2,4,2]]

**Output:** [8,4,6]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/08/22/screenshot-2025-08-23-at-23736-am.png)

| Shop `i` | `prices[i]` | Shop `j` | `prices[j]` | `costᵢ` | `taxᵢ` | Travel cost | Return cost |            Total |          Minimum |
| -------: | ----------: | -------: | ----------: | ------: | -----: | ----------: | ----------: | ---------------: | ---------------: |
|        0 |           9 |        1 |           4 |       1 |      3 |           1 | `1 * 3 = 3` |  `1 + 3 + 4 = 8` |  `min(9, 8) = 8` |
|        1 |           4 |        2 |           6 |       4 |      2 |           4 | `4 * 2 = 8` | `4 + 8 + 6 = 18` | `min(4, 18) = 4` |
|        2 |           6 |        1 |           4 |       4 |      2 |           4 | `4 * 2 = 8` | `4 + 8 + 4 = 16` | `min(6, 16) = 6` |

Thus, the answer is `[8, 4, 6]`.

**Example 3:**

**Input:** n = 3, prices = [10,11,1], roads = \[\[0,2,1,3],[1,2,3,4],[0,1,5,2]]

**Output:** [5,11,1]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/08/22/screenshot-2025-08-23-at-24644-am.png)

| Shop `i` | `prices[i]` | Shop `j` | `prices[j]` | `costᵢ` | `taxᵢ` | Travel cost |  Return cost |             Total |            Minimum |
| -------: | ----------: | -------: | ----------: | ------: | -----: | ----------: | -----------: | ----------------: | -----------------: |
|        0 |          10 |        2 |           1 |       1 |      3 |           1 |  `1 * 3 = 3` |   `1 + 3 + 1 = 5` |   `min(10, 5) = 5` |
|        1 |          11 |        2 |           1 |       3 |      4 |           3 | `3 * 4 = 12` | `3 + 12 + 1 = 16` | `min(11, 16) = 11` |
|        2 |           1 |        0 |          10 |       1 |      3 |           1 |  `1 * 3 = 3` | `1 + 3 + 10 = 14` |   `min(1, 14) = 1` |

Thus, the answer is `[5, 11, 1]`.

**Constraints:**

*   `1 <= n <= 1000`
*   `prices.length == n`
*   <code>1 <= prices[i] <= 10<sup>9</sup></code>
*   `0 <= roads.length <= min(n × (n - 1) / 2, 2000)`
*   <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, cost<sub>i</sub>, tax<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= cost<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= tax<sub>i</sub> <= 100</code>
*   There are no **repeated** edges.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;

@SuppressWarnings("java:S1210")
public class Solution {
    public int[] minCost(int n, int[] prices, int[][] roads) {
        List<List<Edge>> adj = adjacency(n, roads);
        int[] ans = Arrays.copyOf(prices, n);
        for (int i = 0; i < n; ++i) {
            long[] forward = shortestPath(adj, i, prices[i], true);
            long[] backward = shortestPath(adj, i, prices[i], false);
            for (int j = 0; j < n; ++j) {
                if (j != i && forward[j] != Long.MAX_VALUE && backward[j] != Long.MAX_VALUE) {
                    long d = forward[j] + prices[j] + backward[j];
                    if (d < ans[i]) {
                        ans[i] = (int) d;
                    }
                }
            }
        }
        return ans;
    }

    private long[] shortestPath(List<List<Edge>> adj, int src, int maxPrice, boolean forward) {
        long[] dist = new long[adj.size()];
        Arrays.fill(dist, Long.MAX_VALUE);
        dist[src] = 0L;
        PriorityQueue<Distance> pq = new PriorityQueue<>();
        pq.offer(new Distance(src, dist[src]));
        while (!pq.isEmpty() && pq.peek().dist <= maxPrice) {
            Distance distance = pq.poll();
            if (distance.dist > dist[distance.node]) {
                continue;
            }
            for (Edge edge : adj.get(distance.node)) {
                long d = forward ? edge.forward : edge.backward;
                if (dist[edge.node] > dist[distance.node] + d) {
                    dist[edge.node] = dist[distance.node] + d;
                    pq.offer(new Distance(edge.node, dist[edge.node]));
                }
            }
        }
        return dist;
    }

    private List<List<Edge>> adjacency(int n, int[][] roads) {
        List<List<Edge>> adj = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            adj.add(new ArrayList<>());
        }
        for (int[] r : roads) {
            long f = r[2];
            long b = f * r[3];
            adj.get(r[0]).add(new Edge(r[1], f, b));
            adj.get(r[1]).add(new Edge(r[0], f, b));
        }
        return adj;
    }

    private static final class Edge {
        private final int node;
        private final long forward;
        private final long backward;

        private Edge(int node, long forward, long backward) {
            this.node = node;
            this.forward = forward;
            this.backward = backward;
        }
    }

    private static final class Distance implements Comparable<Distance> {
        private final int node;
        private final long dist;

        private Distance(int node, long dist) {
            this.node = node;
            this.dist = dist;
        }

        @Override
        public int compareTo(Distance other) {
            return Long.compare(this.dist, other.dist);
        }
    }
}
```