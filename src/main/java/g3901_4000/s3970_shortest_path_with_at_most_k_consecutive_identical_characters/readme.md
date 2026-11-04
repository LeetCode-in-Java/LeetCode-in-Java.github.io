[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3970\. Shortest Path With At Most K Consecutive Identical Characters

Medium

You are given an integer `n` representing the number of nodes in a **directed weighted** graph, numbered from 0 to `n - 1`. This is represented by a 2D integer array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> represents a directed edge from node <code>u<sub>i</sub></code> to node <code>v<sub>i</sub></code> with weight <code>w<sub>i</sub></code>.

You are also given a string `labels` of length `n`, where `labels[i]` is the character assigned to node `i`, and an integer `k`.

Return the **minimum** **total** edge weight of a path from node 0 to node `n - 1` such that the concatenation of the labels of the nodes along the path contains **at most** `k` **consecutive** **identical** characters. If no valid path exists, return -1.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1,1],[1,2,1],[0,2,3]], labels = "aab", k = 1

**Output:** 3

**Explanation:**

The optimal valid path from node 0 to node 2 is as follows:

*   Use `edges[2] = [0, 2, 3]` to reach node 2 with a weight <code>w<sub>i</sub> = 3</code>.

The corresponding concatenation of labels is `"ab"`, which satisfies at most `k = 1` consecutive identical characters. Thus, the answer is 3.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1,1],[1,2,1],[0,2,3]], labels = "aab", k = 2

**Output:** 2

**Explanation:**

The optimal valid path from node 0 to node 2 is as follows:

*   Use `edges[0] = [0, 1, 1]` to reach node 1 with weight <code>w<sub>i</sub> = 1</code>.
*   Use `edges[1] = [1, 2, 1]` to reach node 2 with weight <code>w<sub>i</sub> = 1</code>.

The corresponding concatenation of labels is `"aab"`, which satisfies at most `k = 2` consecutive identical characters. Thus, the answer is 2.

**Example 3:**

**Input:** n = 3, edges = \[\[0,1,1],[1,2,1]], labels = "aaa", k = 2

**Output:** \-1

**Explanation:**

There is no valid path from node 0 to node 2 that satisfies at most `k = 2` consecutive identical characters. Thus, the answer is -1.

**Constraints:**

*   <code>1 <= n == labels.length <= 5 * 10<sup>4</sup></code>
*   <code>0 <= edges.length <= 5 * 10<sup>4</sup></code>
*   <code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= w<sub>i</sub> <= 10<sup>4</sup></code>
*   `labels` consists of lowercase English letters
*   `1 <= k <= 50`

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;

public class Solution {
    private static class Node {
        long w;
        int ch;
        int city;

        public Node(int city, long w, int ch) {
            this.city = city;
            this.w = w;
            this.ch = ch;
        }
    }

    public int shortestPath(int n, int[][] edges, String labelschars, int k) {
        int[] chars = new int[n];
        char[] labels = labelschars.toCharArray();
        Arrays.fill(chars, Integer.MAX_VALUE);
        List<List<int[]>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        for (int[] edge : edges) {
            adj.get(edge[0]).add(new int[] {edge[1], edge[2]});
        }
        PriorityQueue<Node> pq = new PriorityQueue<>((a, b) -> Long.compare(a.w, b.w));
        pq.add(new Node(0, 0, 1));
        while (!pq.isEmpty()) {
            Node nd = pq.poll();
            if (nd.ch >= chars[nd.city]) {
                continue;
            }
            chars[nd.city] = nd.ch;
            if (nd.city == n - 1) {
                return (int) nd.w;
            }
            addNeighbors(nd, adj, labels, k, pq);
        }
        return -1;
    }

    private void addNeighbors(
            Node nd, List<List<int[]>> adj, char[] labels, int k, PriorityQueue<Node> pq) {
        for (int[] nbr : adj.get(nd.city)) {
            int nextCity = nbr[0];
            int totalCh = getNextCharCount(nd.ch, labels[nd.city], labels[nextCity]);
            if (nd.city != nextCity && totalCh <= k) {
                long totalWeight = nd.w + nbr[1];
                pq.add(new Node(nextCity, totalWeight, totalCh));
            }
        }
    }

    private int getNextCharCount(int ch, char current, char next) {
        return current == next ? ch + 1 : 1;
    }
}
```