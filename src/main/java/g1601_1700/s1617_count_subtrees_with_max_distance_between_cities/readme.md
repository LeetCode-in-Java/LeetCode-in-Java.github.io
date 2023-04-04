[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1617\. Count Subtrees With Max Distance Between Cities

Hard

There are `n` cities numbered from `1` to `n`. You are given an array `edges` of size `n-1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> represents a bidirectional edge between cities <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>. There exists a unique path between each pair of cities. In other words, the cities form a **tree**.

A **subtree** is a subset of cities where every city is reachable from every other city in the subset, where the path between each pair passes through only the cities from the subset. Two subtrees are different if there is a city in one subtree that is not present in the other.

For each `d` from `1` to `n-1`, find the number of subtrees in which the **maximum distance** between any two cities in the subtree is equal to `d`.

Return _an array of size_ `n-1` _where the_ <code>d<sup>th</sup></code> _element **(1-indexed)** is the number of subtrees in which the **maximum distance** between any two cities is equal to_ `d`.

**Notice** that the **distance** between the two cities is the number of edges in the path between them.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/09/21/p1.png)**

**Input:** n = 4, edges = \[\[1,2],[2,3],[2,4]]

**Output:** [3,4,0]

**Explanation:** 

The subtrees with subsets {1,2}, {2,3} and {2,4} have a max distance of 1. 

The subtrees with subsets {1,2,3}, {1,2,4}, {2,3,4} and {1,2,3,4} have a max distance of 2. 

No subtree has two nodes where the max distance between them is 3.

**Example 2:**

**Input:** n = 2, edges = \[\[1,2]]

**Output:** [1]

**Example 3:**

**Input:** n = 3, edges = \[\[1,2],[2,3]]

**Output:** [2,1]

**Constraints:**

*   `2 <= n <= 15`
*   `edges.length == n-1`
*   `edges[i].length == 2`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   All pairs <code>(u<sub>i</sub>, v<sub>i</sub>)</code> are distinct.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@SuppressWarnings("java:S5329")
public class Solution {
    private int ans = 0;
    private int vis = 0;

    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        ans = 0;
        vis = 0;
        int[] dist = new int[n - 1];
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] i : edges) {
            graph.computeIfAbsent(1 << (i[0] - 1), ArrayList::new).add((1 << (i[1] - 1)));
            graph.computeIfAbsent(1 << (i[1] - 1), ArrayList::new).add((1 << (i[0] - 1)));
        }
        int ps = ((int) Math.pow(2, n)) - 1;
        for (int set = 3; set <= ps; set++) {
            // is power of 2
            boolean isp2 = set != 0 && ((set & (set - 1)) == 0);
            if (!isp2) {
                ans = 0;
                vis = 0;
                dfs(graph, set, Integer.highestOneBit(set), -1);
                if (vis == set) {
                    dist[ans - 1]++;
                }
            }
        }
        return dist;
    }

    private int dfs(Map<Integer, List<Integer>> graph, int set, int c, int p) {
        if ((set & c) == 0) {
            return 0;
        }
        vis = vis | c;
        int fdist = 0;
        int sdist = 0;
        for (int i : graph.get(c)) {
            if (i != p) {
                int dist = dfs(graph, set, i, c);
                if (dist > fdist) {
                    sdist = fdist;
                    fdist = dist;
                } else {
                    sdist = Math.max(sdist, dist);
                }
            }
        }
        ans = Math.max(ans, fdist + sdist);
        return 1 + fdist;
    }
}
```