## 1976\. Number of Ways to Arrive at Destination

Medium

You are in a city that consists of `n` intersections numbered from `0` to `n - 1` with **bi-directional** roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

You are given an integer `n` and a 2D integer array `roads` where <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, time<sub>i</sub>]</code> means that there is a road between intersections <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> that takes <code>time<sub>i</sub></code> minutes to travel. You want to know in how many ways you can travel from intersection `0` to intersection `n - 1` in the **shortest amount of time**.

Return _the **number of ways** you can arrive at your destination in the **shortest amount of time**_. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/17/graph2.png)

**Input:** n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]

**Output:** 4

**Explanation:** The shortest amount of time it takes to go from intersection 0 to intersection 6 is 7 minutes. The four ways to get there in 7 minutes are: 

- 0 ➝ 6 

- 0 ➝ 4 ➝ 6 

- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6 

- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

**Example 2:**

**Input:** n = 2, roads = [[1,0,10]]

**Output:** 1

**Explanation:** There is only one way to go from intersection 0 to intersection 1, and it takes 10 minutes.

**Constraints:**

*   `1 <= n <= 200`
*   `n - 1 <= roads.length <= n * (n - 1) / 2`
*   `roads[i].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>1 <= time<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   There is at most one road connecting any two intersections.
*   You can reach any intersection from any other intersection.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.Queue;

@SuppressWarnings("unchecked")
public class Solution {
    private int dijkstra(int[][] roads, int n) {
        long mod = (int) 1e9 + 7L;
        Queue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(l -> l[1]));
        long[] ways = new long[n];
        long[] dist = new long[n];
        Arrays.fill(dist, (long) 1e18);
        dist[0] = 0;
        ways[0] = 1;
        ArrayList<long[]>[] graph = new ArrayList[n];
        for (int i = 0; i < graph.length; i++) {
            graph[i] = new ArrayList<>();
        }
        for (int[] road : roads) {
            graph[road[0]].add(new long[] {road[1], road[2]});
            graph[road[1]].add(new long[] {road[0], road[2]});
        }
        pq.add(new long[] {0, 0});
        if (!pq.isEmpty()) {
            while (!pq.isEmpty()) {
                long[] ele = pq.remove();
                long dis = ele[1];
                long node = ele[0];
                for (long[] e : graph[(int) node]) {
                    long wt = e[1];
                    long adjNode = e[0];
                    if (wt + dis < dist[(int) adjNode]) {
                        dist[(int) adjNode] = wt + dis;
                        ways[(int) adjNode] = ways[(int) node];
                        pq.add(new long[] {adjNode, dist[(int) adjNode]});
                    } else if (wt + dis == dist[(int) adjNode]) {
                        ways[(int) adjNode] = (ways[(int) node] + ways[(int) adjNode]) % mod;
                    }
                }
            }
        }
        return (int) ways[n - 1];
    }

    public int countPaths(int n, int[][] roads) {
        return dijkstra(roads, n);
    }
}
```