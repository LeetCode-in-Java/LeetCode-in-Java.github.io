[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3530\. Maximum Profit from Valid Topological Order in DAG

Hard

You are given a **Directed Acyclic Graph (DAG)** with `n` nodes labeled from `0` to `n - 1`, represented by a 2D array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates a directed edge from node <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code>. Each node has an associated **score** given in an array `score`, where `score[i]` represents the score of node `i`.

You must process the nodes in a **valid topological order**. Each node is assigned a **1-based position** in the processing order.

The **profit** is calculated by summing up the product of each node's score and its position in the ordering.

Return the **maximum** possible profit achievable with an optimal topological order.

A **topological order** of a DAG is a linear ordering of its nodes such that for every directed edge `u → v`, node `u` comes before `v` in the ordering.

**Example 1:**

**Input:** n = 2, edges = \[\[0,1]], score = [2,3]

**Output:** 8

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/10/screenshot-2025-03-11-at-021131.png)

Node 1 depends on node 0, so a valid order is `[0, 1]`.

| Node | Processing Order | Score | Multiplier | Profit Calculation |
|------|------------------|-------|------------|--------------------|
| 0    | 1st              | 2     | 1          | 2 × 1 = 2           |
| 1    | 2nd              | 3     | 2          | 3 × 2 = 6           |

The maximum total profit achievable over all valid topological orders is `2 + 6 = 8`.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1],[0,2]], score = [1,6,3]

**Output:** 25

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/03/10/screenshot-2025-03-11-at-023558.png)

Nodes 1 and 2 depend on node 0, so the most optimal valid order is `[0, 2, 1]`.

| Node | Processing Order | Score | Multiplier | Profit Calculation |
|------|------------------|-------|------------|--------------------|
| 0    | 1st              | 1     | 1          | 1 × 1 = 1           |
| 2    | 2nd              | 3     | 2          | 3 × 2 = 6           |
| 1    | 3rd              | 6     | 3          | 6 × 3 = 18          |

The maximum total profit achievable over all valid topological orders is `1 + 6 + 18 = 25`.

**Constraints:**

*   `1 <= n == score.length <= 22`
*   <code>1 <= score[i] <= 10<sup>5</sup></code>
*   `0 <= edges.length <= n * (n - 1) / 2`
*   <code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code> denotes a directed edge from <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code>.
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   The input graph is **guaranteed** to be a **DAG**.
*   There are no duplicate edges.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    private int helper(
            int mask,
            int pos,
            int[] inDegree,
            List<List<Integer>> adj,
            int[] score,
            int[] dp,
            int n) {
        if (mask == (1 << n) - 1) {
            return 0;
        }
        if (dp[mask] != -1) {
            return dp[mask];
        }
        int res = 0;
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) == 0 && inDegree[i] == 0) {
                for (int ng : adj.get(i)) {
                    inDegree[ng]--;
                }
                int val =
                        pos * score[i]
                                + helper(mask | (1 << i), pos + 1, inDegree, adj, score, dp, n);
                res = Math.max(res, val);
                for (int ng : adj.get(i)) {
                    inDegree[ng]++;
                }
            }
        }
        dp[mask] = res;
        return res;
    }

    public int maxProfit(int n, int[][] edges, int[] score) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        int[] inDegree = new int[n];
        for (int[] e : edges) {
            adj.get(e[0]).add(e[1]);
            inDegree[e[1]]++;
        }
        int[] dp = new int[1 << n];
        Arrays.fill(dp, -1);
        return helper(0, 1, inDegree, adj, score, dp, n);
    }
}
```