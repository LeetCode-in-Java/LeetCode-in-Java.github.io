[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3910\. Count Connected Subgraphs with Even Node Sum

Hard

You are given an undirected graph with `n` nodes labeled from 0 to `n - 1`. Node `i` has a **value** of `nums[i]`, which is either 0 or 1. The edges of the graph are given by a 2D array `edges` where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> represents an edge between node <code>u<sub>i</sub></code> and node <code>v<sub>i</sub></code>.

For a **non-empty subset** `s` of nodes in the graph, we consider the **induced subgraph** of `s` generated as follows:

*   We keep only the nodes in `s`.
*   We keep only the edges whose two endpoints are both in `s`.

Return an integer representing the number of **non-empty** subsets `s` of nodes in the graph such that:

*   The **induced subgraph** of `s` is **connected**.
*   The **sum** of node **values** in `s` is **even**.

**Example 1:**

**Input:** nums = [1,0,1], edges = \[\[0,1],[1,2]]

**Output:** 2

**Explanation:**

| `s` | connected? | sum of node values | counted? |
|---|---|---:|---|
| `[0]` | Yes | 1 | No |
| `[1]` | Yes | 0 | Yes |
| `[2]` | Yes | 1 | No |
| `[0,1]` | Yes | 1 | No |
| `[0,2]` | No, node 0 and node 2 are disconnected. | 2 | No |
| `[1,2]` | Yes | 1 | No |
| `[0,1,2]` | Yes | 2 | Yes |

**Example 2:**

**Input:** nums = [1], edges = []

**Output:** 0

**Explanation:**

| `s` | connected? | sum of node values | counted? |
|---|---|---:|---|
| `[0]` | Yes | 1 | No |

**Constraints:**

*   `1 <= n == nums.length <= 13`
*   `nums[i]` is 0 or 1.
*   `0 <= edges.length <= n * (n - 1) / 2`
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub> < v<sub>i</sub> < n</code>
*   All edges are **distinct**.

## Solution

```java
public class Solution {
    private long[] graph;
    private int[] nums;
    private int validCount;

    public int evenSumSubgraphs(int[] nums, int[][] edges) {
        this.nums = nums;
        int nodeCount = nums.length;
        this.graph = new long[nodeCount];
        this.validCount = 0;
        buildGraph(edges);
        for (int root = 0; root < nodeCount; root++) {
            long rootMask = 1L << root;
            long allowedMask = -(1L << root);
            long candidateMask = graph[root] & allowedMask;
            search(rootMask, candidateMask, 0L, nums[root] & 1, allowedMask);
        }
        return validCount;
    }

    private void buildGraph(int[][] edgeList) {
        for (int[] edge : edgeList) {
            int firstNode = edge[0];
            int secondNode = edge[1];
            graph[firstNode] |= 1L << secondNode;
            graph[secondNode] |= 1L << firstNode;
        }
    }

    private void search(
            long selectedMask,
            long candidateMask,
            long excludedMask,
            int parity,
            long allowedMask) {
        if (parity == 0) {
            validCount++;
        }
        while (candidateMask != 0) {
            long currentBit = candidateMask & -candidateMask;
            int currentNode = Long.numberOfTrailingZeros(currentBit);
            candidateMask ^= currentBit;
            long nextSelectedMask = selectedMask | currentBit;
            long nextCandidateMask =
                    candidateMask
                            | (graph[currentNode]
                                    & allowedMask
                                    & ~nextSelectedMask
                                    & ~excludedMask);
            search(
                    nextSelectedMask,
                    nextCandidateMask,
                    excludedMask,
                    parity ^ (nums[currentNode] & 1),
                    allowedMask);
            excludedMask |= currentBit;
        }
    }
}
```