[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2242\. Maximum Score of a Node Sequence

Hard

There is an **undirected** graph with `n` nodes, numbered from `0` to `n - 1`.

You are given a **0-indexed** integer array `scores` of length `n` where `scores[i]` denotes the score of node `i`. You are also given a 2D integer array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> denotes that there exists an **undirected** edge connecting nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>.

A node sequence is **valid** if it meets the following conditions:

*   There is an edge connecting every pair of **adjacent** nodes in the sequence.
*   No node appears more than once in the sequence.

The score of a node sequence is defined as the **sum** of the scores of the nodes in the sequence.

Return _the **maximum score** of a valid node sequence with a length of_ `4`_._ If no such sequence exists, return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/04/15/ex1new3.png)

**Input:** scores = [5,2,9,8,4], edges = \[\[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]

**Output:** 24

**Explanation:** The figure above shows the graph and the chosen node sequence [0,1,2,3].

The score of the node sequence is 5 + 2 + 9 + 8 = 24.

It can be shown that no other node sequence has a score of more than 24.

Note that the sequences [3,1,2,0] and [1,0,2,3] are also valid and have a score of 24.

The sequence [0,3,2,4] is not valid since no edge connects nodes 0 and 3. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/17/ex2.png)

**Input:** scores = [9,20,6,4,11,12], edges = \[\[0,3],[5,3],[2,4],[1,3]]

**Output:** -1

**Explanation:** The figure above shows the graph.

There are no valid node sequences of length 4, so we return -1. 

**Constraints:**

*   `n == scores.length`
*   <code>4 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= scores[i] <= 10<sup>8</sup></code>
*   <code>0 <= edges.length <= 5 * 10<sup>4</sup></code>
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> <= n - 1</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no duplicate edges.

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S135")
public class Solution {
    public int maximumScore(int[] scores, int[][] edges) {
        // store only top 3 nodes (having highest scores)
        int[][] graph = new int[scores.length][3];
        for (int[] a : graph) {
            Arrays.fill(a, -1);
        }
        for (int[] edge : edges) {
            insert(edge[0], graph[edge[1]], scores);
            insert(edge[1], graph[edge[0]], scores);
        }
        int maxScore = -1;
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int score = scores[u] + scores[v];
            for (int i = 0; i < 3; i++) {
                // if neighbour is current node, skip
                if (graph[u][i] == -1 || graph[u][i] == v) {
                    continue;
                }
                for (int j = 0; j < 3; j++) {
                    // if neighbour is current node or already choosen node, skip
                    if (graph[v][j] == -1 || graph[v][j] == u) {
                        continue;
                    }
                    if (graph[v][j] == graph[u][i]) {
                        continue;
                    }
                    maxScore =
                            Math.max(maxScore, score + scores[graph[u][i]] + scores[graph[v][j]]);
                }
            }
        }
        return maxScore;
    }

    private void insert(int n, int[] arr, int[] scores) {
        if (arr[0] == -1) {
            arr[0] = n;
        } else if (arr[1] == -1) {
            if (scores[arr[0]] < scores[n]) {
                arr[1] = arr[0];
                arr[0] = n;
            } else {
                arr[1] = n;
            }
        } else if (arr[2] == -1) {
            if (scores[arr[0]] < scores[n]) {
                arr[2] = arr[1];
                arr[1] = arr[0];
                arr[0] = n;
            } else if (scores[arr[1]] < scores[n]) {
                arr[2] = arr[1];
                arr[1] = n;
            } else {
                arr[2] = n;
            }
        } else {
            if (scores[arr[1]] < scores[n]) {
                arr[2] = arr[1];
                arr[1] = n;
            } else if (scores[arr[2]] < scores[n]) {
                arr[2] = n;
            }
        }
    }
}
```