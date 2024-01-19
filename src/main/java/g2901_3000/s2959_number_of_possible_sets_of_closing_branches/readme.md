[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2959\. Number of Possible Sets of Closing Branches

Hard

There is a company with `n` branches across the country, some of which are connected by roads. Initially, all branches are reachable from each other by traveling some roads.

The company has realized that they are spending an excessive amount of time traveling between their branches. As a result, they have decided to close down some of these branches (**possibly none**). However, they want to ensure that the remaining branches have a distance of at most `maxDistance` from each other.

The **distance** between two branches is the **minimum** total traveled length needed to reach one branch from another.

You are given integers `n`, `maxDistance`, and a **0-indexed** 2D array `roads`, where <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> represents the **undirected** road between branches <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with length <code>w<sub>i</sub></code>.

Return _the number of possible sets of closing branches, so that any branch has a distance of at most_ `maxDistance` _from any other_.

**Note** that, after closing a branch, the company will no longer have access to any roads connected to it.

**Note** that, multiple roads are allowed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/11/08/example11.png)

**Input:** n = 3, maxDistance = 5, roads = \[\[0,1,2],[1,2,10],[0,2,10]]

**Output:** 5

**Explanation:** The possible sets of closing branches are: 
- The set [2], after closing, active branches are [0,1] and they are reachable to each other within distance 2. 
- The set [0,1], after closing, the active branch is [2]. 
- The set [1,2], after closing, the active branch is [0]. 
- The set [0,2], after closing, the active branch is [1]. 
- The set [0,1,2], after closing, there are no active branches. 

It can be proven, that there are only 5 possible sets of closing branches.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/11/08/example22.png)

**Input:** n = 3, maxDistance = 5, roads = \[\[0,1,20],[0,1,10],[1,2,2],[0,2,2]]

**Output:** 7

**Explanation:** The possible sets of closing branches are: 
- The set [], after closing, active branches are [0,1,2] and they are reachable to each other within distance 4. 
- The set [0], after closing, active branches are [1,2] and they are reachable to each other within distance 2. 
- The set [1], after closing, active branches are [0,2] and they are reachable to each other within distance 2. 
- The set [0,1], after closing, the active branch is [2]. 
- The set [1,2], after closing, the active branch is [0]. 
- The set [0,2], after closing, the active branch is [1]. 
- The set [0,1,2], after closing, there are no active branches. 

It can be proven, that there are only 7 possible sets of closing branches.

**Example 3:**

**Input:** n = 1, maxDistance = 10, roads = []

**Output:** 2

**Explanation:** The possible sets of closing branches are: 
- The set [], after closing, the active branch is [0]. 
- The set [0], after closing, there are no active branches. 

It can be proven, that there are only 2 possible sets of closing branches.

**Constraints:**

*   `1 <= n <= 10`
*   <code>1 <= maxDistance <= 10<sup>5</sup></code>
*   `0 <= roads.length <= 1000`
*   `roads[i].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   <code>1 <= w<sub>i</sub> <= 1000</code>
*   All branches are reachable from each other by traveling some roads.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    private int get(int n, int maxDis, int mask, List<List<int[]>> al) {
        int nodes = 0;
        boolean[] m = new boolean[n];
        for (int i = 0; i < n; i++) {
            int val = mask & (1 << i);
            if (val > 0) {
                m[i] = true;
                nodes++;
            }
        }
        if (nodes == n) {
            return 1;
        }
        for (int startVertex = 0; startVertex < n; startVertex++) {
            if (m[startVertex]) {
                continue;
            }
            Queue<int[]> q = new LinkedList<>();
            q.add(new int[] {startVertex, 0});
            int[] dis = new int[n];
            Arrays.fill(dis, Integer.MAX_VALUE);
            dis[startVertex] = 0;
            int nodeCount = 1;
            while (!q.isEmpty()) {
                int[] curr = q.poll();
                for (int[] adj : al.get(curr[0])) {
                    if (!m[adj[0]] && curr[1] + adj[1] <= dis[adj[0]]) {
                        if (dis[adj[0]] == Integer.MAX_VALUE) {
                            nodeCount++;
                        }
                        dis[adj[0]] = curr[1] + adj[1];
                        q.add(new int[] {adj[0], dis[adj[0]]});
                    }
                }
            }
            for (int i = 0; i < n; i++) {
                if (!m[i] && dis[i] > maxDis) {
                    return 0;
                }
            }
            if (nodes != n - nodeCount) {
                return 0;
            }
        }
        return 1;
    }

    private int solve(int n, int maxDis, List<List<int[]>> al) {
        int res = 0;
        for (int i = 0; i < (1 << n); i++) {
            res += get(n, maxDis, i, al);
        }
        return res;
    }

    public int numberOfSets(int n, int maxDistance, int[][] roads) {
        List<List<int[]>> al = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            al.add(new ArrayList<>());
        }
        for (int[] edge : roads) {
            al.get(edge[0]).add(new int[] {edge[1], edge[2]});
            al.get(edge[1]).add(new int[] {edge[0], edge[2]});
        }
        return solve(n, maxDistance, al);
    }
}
```