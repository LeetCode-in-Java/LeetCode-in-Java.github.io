[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1782\. Count Pairs Of Nodes

Hard

You are given an undirected graph defined by an integer `n`, the number of nodes, and a 2D integer array `edges`, the edges in the graph, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an **undirected** edge between <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>. You are also given an integer array `queries`.

Let `incident(a, b)` be defined as the **number of edges** that are connected to **either** node `a` or `b`.

The answer to the <code>j<sup>th</sup></code> query is the **number of pairs** of nodes `(a, b)` that satisfy **both** of the following conditions:

*   `a < b`
*   `incident(a, b) > queries[j]`

Return _an array_ `answers` _such that_ `answers.length == queries.length` _and_ `answers[j]` _is the answer of the_ <code>j<sup>th</sup></code> _query_.

Note that there can be **multiple edges** between the same two nodes.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/08/winword_2021-06-08_00-58-39.png)

**Input:** n = 4, edges = \[\[1,2],[2,4],[1,3],[2,3],[2,1]], queries = [2,3]

**Output:** [6,5]

**Explanation:** The calculations for incident(a, b) are shown in the table above. The answers for each of the queries are as follows: 

- answers[0] = 6. All the pairs have an incident(a, b) value greater than 2. 

- answers[1] = 5. All the pairs except (3, 4) have an incident(a, b) value greater than 3.

**Example 2:**

**Input:** n = 5, edges = \[\[1,5],[1,5],[3,4],[2,5],[1,3],[5,1],[2,3],[2,5]], queries = [1,2,3,4,5]

**Output:** [10,10,9,8,6]

**Constraints:**

*   <code>2 <= n <= 2 * 10<sup>4</sup></code>
*   <code>1 <= edges.length <= 10<sup>5</sup></code>
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   `1 <= queries.length <= 20`
*   `0 <= queries[j] < edges.length`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int[] countPairs(int n, int[][] edges, int[] queries) {
        Map<Integer, Integer> edgeCount = new HashMap<>();
        int[] degree = new int[n];
        for (int[] e : edges) {
            int u = e[0] - 1;
            int v = e[1] - 1;
            degree[u]++;
            degree[v]++;
            int eId = Math.min(u, v) * n + Math.max(u, v);
            edgeCount.put(eId, edgeCount.getOrDefault(eId, 0) + 1);
        }
        Map<Integer, Integer> degreeCount = new HashMap<>();
        int maxDegree = 0;
        for (int d : degree) {
            degreeCount.put(d, degreeCount.getOrDefault(d, 0) + 1);
            maxDegree = Math.max(maxDegree, d);
        }
        int[] count = new int[2 * maxDegree + 1];
        for (Map.Entry<Integer, Integer> d1 : degreeCount.entrySet()) {
            for (Map.Entry<Integer, Integer> d2 : degreeCount.entrySet()) {
                count[d1.getKey() + d2.getKey()] +=
                        (d1 == d2)
                                ? d1.getValue() * (d1.getValue() - 1)
                                : d1.getValue() * d2.getValue();
            }
        }
        for (int i = 0; i < count.length; i++) {
            count[i] /= 2;
        }
        for (Map.Entry<Integer, Integer> e : edgeCount.entrySet()) {
            int u = e.getKey() / n;
            int v = e.getKey() % n;
            count[degree[u] + degree[v]]--;
            count[degree[u] + degree[v] - e.getValue()]++;
        }
        for (int i = count.length - 2; i >= 0; i--) {
            count[i] += count[i + 1];
        }
        int[] res = new int[queries.length];
        for (int q = 0; q < queries.length; q++) {
            res[q] = ((queries[q] + 1) >= count.length) ? 0 : count[queries[q] + 1];
        }
        return res;
    }
}
```