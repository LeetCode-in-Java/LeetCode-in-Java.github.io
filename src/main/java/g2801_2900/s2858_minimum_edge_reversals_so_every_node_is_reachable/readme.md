[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2858\. Minimum Edge Reversals So Every Node Is Reachable

Hard

There is a **simple directed graph** with `n` nodes labeled from `0` to `n - 1`. The graph would form a **tree** if its edges were bi-directional.

You are given an integer `n` and a **2D** integer array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> represents a **directed edge** going from node <code>u<sub>i</sub></code> to node <code>v<sub>i</sub></code>.

An **edge reversal** changes the direction of an edge, i.e., a directed edge going from node <code>u<sub>i</sub></code> to node <code>v<sub>i</sub></code> becomes a directed edge going from node <code>v<sub>i</sub></code> to node <code>u<sub>i</sub></code>.

For every node `i` in the range `[0, n - 1]`, your task is to **independently** calculate the **minimum** number of **edge reversals** required so it is possible to reach any other node starting from node `i` through a **sequence** of **directed edges**.

Return _an integer array_ `answer`_, where_ `answer[i]` _is the_ _**minimum** number of **edge reversals** required so it is possible to reach any other node starting from node_ `i` _through a **sequence** of **directed edges**._

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/08/26/image-20230826221104-3.png)

**Input:** n = 4, edges = \[\[2,0],[2,1],[1,3]]

**Output:** [1,1,0,2]

**Explanation:** The image above shows the graph formed by the edges. 

For node 0: after reversing the edge [2,0], it is possible to reach any other node starting from node 0. 

So, answer[0] = 1. 

For node 1: after reversing the edge [2,1], it is possible to reach any other node starting from node 1. 

So, answer[1] = 1. 

For node 2: it is already possible to reach any other node starting from node 2. 

So, answer[2] = 0. 

For node 3: after reversing the edges [1,3] and [2,1], it is possible to reach any other node starting from node 3. 

So, answer[3] = 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/08/26/image-20230826225541-2.png)

**Input:** n = 3, edges = \[\[1,2],[2,0]]

**Output:** [2,0,1]

**Explanation:** The image above shows the graph formed by the edges. 

For node 0: after reversing the edges [2,0] and [1,2], it is possible to reach any other node starting from node 0. 

So, answer[0] = 2. 

For node 1: it is already possible to reach any other node starting from node 1. 

So, answer[1] = 0. 

For node 2: after reversing the edge [1, 2], it is possible to reach any other node starting from node 2. 

So, answer[2] = 1.

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= u<sub>i</sub> == edges[i][0] < n</code>
*   <code>0 <= v<sub>i</sub> == edges[i][1] < n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   The input is generated such that if the edges were bi-directional, the graph would be a tree.

## Solution

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

@SuppressWarnings("unchecked")
public class Solution {
    public int[] minEdgeReversals(int n, int[][] edges) {
        List<int[]>[] nexts = new List[n];
        for (int i = 0; i < n; i++) {
            nexts[i] = new ArrayList<>();
        }
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            nexts[u].add(new int[] {1, v});
            nexts[v].add(new int[] {-1, u});
        }
        int[] res = new int[n];
        for (int i = 0; i < n; i++) {
            res[i] = -1;
        }
        res[0] = dfs(nexts, 0, -1);
        Queue<Integer> queue = new LinkedList<>();
        queue.add(0);
        while (!queue.isEmpty()) {
            Integer index = queue.remove();
            int val = res[index];
            List<int[]> next = nexts[index];
            for (int[] node : next) {
                if (res[node[1]] == -1) {
                    if (node[0] == 1) {
                        res[node[1]] = val + 1;
                    } else {
                        res[node[1]] = val - 1;
                    }
                    queue.add(node[1]);
                }
            }
        }
        return res;
    }

    private int dfs(List<int[]>[] nexts, int index, int pre) {
        int res = 0;
        List<int[]> next = nexts[index];
        for (int[] node : next) {
            if (node[1] != pre) {
                if (node[0] == -1) {
                    res++;
                }
                res += dfs(nexts, node[1], index);
            }
        }
        return res;
    }
}
```