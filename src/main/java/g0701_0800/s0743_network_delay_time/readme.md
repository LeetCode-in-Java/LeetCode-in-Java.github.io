[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 743\. Network Delay Time

Medium

You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges <code>times[i] = (u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>)</code>, where <code>u<sub>i</sub></code> is the source node, <code>v<sub>i</sub></code> is the target node, and <code>w<sub>i</sub></code> is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return the time it takes for all the `n` nodes to receive the signal. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

**Input:** times = \[\[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2

**Output:** 2

**Example 2:**

**Input:** times = \[\[1,2,1]], n = 2, k = 1

**Output:** 1

**Example 3:**

**Input:** times = \[\[1,2,1]], n = 2, k = 2

**Output:** -1

**Constraints:**

*   `1 <= k <= n <= 100`
*   `1 <= times.length <= 6000`
*   `times[i].length == 3`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>0 <= w<sub>i</sub> <= 100</code>
*   All the pairs <code>(u<sub>i</sub>, v<sub>i</sub>)</code> are **unique**. (i.e., no multiple edges.)

## Solution

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        int[][] graph = new int[n + 1][n + 1];
        for (int[] g : graph) {
            Arrays.fill(g, -1);
        }
        for (int[] t : times) {
            graph[t[0]][t[1]] = t[2];
        }
        boolean[] visited = new boolean[n + 1];
        int[] dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[k] = 0;
        Queue<Integer> spfa = new LinkedList<>();
        spfa.add(k);
        visited[k] = true;
        while (!spfa.isEmpty()) {
            int curr = spfa.poll();
            visited[curr] = false;
            for (int i = 1; i <= n; i++) {
                if (graph[curr][i] != -1 && dist[i] > dist[curr] + graph[curr][i]) {
                    dist[i] = dist[curr] + graph[curr][i];
                    if (!visited[i]) {
                        spfa.add(i);
                        visited[i] = true;
                    }
                }
            }
        }
        int result = 0;
        for (int i = 1; i <= n; i++) {
            result = Math.max(dist[i], result);
        }
        return result == Integer.MAX_VALUE ? -1 : result;
    }
}
```