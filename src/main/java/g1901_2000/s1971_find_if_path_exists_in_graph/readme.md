[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1971\. Find if Path Exists in Graph

Easy

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1` (**inclusive**). The edges in the graph are represented as a 2D integer array `edges`, where each <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> denotes a bi-directional edge between vertex <code>u<sub>i</sub></code> and vertex <code>v<sub>i</sub></code>. Every vertex pair is connected by **at most one** edge, and no vertex has an edge to itself.

You want to determine if there is a **valid path** that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` _if there is a **valid path** from_ `source` _to_ `destination`_, or_ `false` _otherwise__._

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

**Input:** n = 3, edges = \[\[0,1],[1,2],[2,0]], source = 0, destination = 2

**Output:** true

**Explanation:** There are two paths from vertex 0 to vertex 2: - 0 → 1 → 2 - 0 → 2

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

**Input:** n = 6, edges = \[\[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5

**Output:** false

**Explanation:** There is no path from vertex 0 to vertex 5.

**Constraints:**

*   <code>1 <= n <= 2 * 10<sup>5</sup></code>
*   <code>0 <= edges.length <= 2 * 10<sup>5</sup></code>
*   `edges[i].length == 2`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   `0 <= source, destination <= n - 1`
*   There are no duplicate edges.
*   There are no self edges.

## Solution

```java
public class Solution {
    public boolean validPath(int n, int[][] edges, int start, int end) {
        boolean[] visited = new boolean[n];
        visited[start] = true;
        boolean newVisit = true;
        while (!visited[end] && newVisit) {
            newVisit = false;
            for (int i = edges.length - 1; i >= 0; i--) {
                if (visited[edges[i][0]]) {
                    if (!visited[edges[i][1]]) {
                        visited[edges[i][1]] = newVisit = true;
                    }
                } else if (visited[edges[i][1]]) {
                    visited[edges[i][0]] = newVisit = true;
                }
            }
        }

        return visited[end];
    }
}
```