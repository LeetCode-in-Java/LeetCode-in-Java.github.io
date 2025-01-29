[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3425\. Longest Special Path

Hard

You are given an undirected tree rooted at node `0` with `n` nodes numbered from `0` to `n - 1`, represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, length<sub>i</sub>]</code> indicates an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with length <code>length<sub>i</sub></code>. You are also given an integer array `nums`, where `nums[i]` represents the value at node `i`.

A **special path** is defined as a **downward** path from an ancestor node to a descendant node such that all the values of the nodes in that path are **unique**.

**Note** that a path may start and end at the same node.

Return an array `result` of size 2, where `result[0]` is the **length** of the **longest** special path, and `result[1]` is the **minimum** number of nodes in all _possible_ **longest** special paths.

**Example 1:**

**Input:** edges = \[\[0,1,2],[1,2,3],[1,3,5],[1,4,4],[2,5,6]], nums = [2,1,2,1,3,1]

**Output:** [6,2]

**Explanation:**

#### In the image below, nodes are colored by their corresponding values in `nums`

![](https://assets.leetcode.com/uploads/2024/11/02/tree3.jpeg)

The longest special paths are `2 -> 5` and `0 -> 1 -> 4`, both having a length of 6. The minimum number of nodes across all longest special paths is 2.

**Example 2:**

**Input:** edges = \[\[1,0,8]], nums = [2,2]

**Output:** [0,1]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/02/tree4.jpeg)

The longest special paths are `0` and `1`, both having a length of 0. The minimum number of nodes across all longest special paths is 1.

**Constraints:**

*   <code>2 <= n <= 5 * 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>1 <= length<sub>i</sub> <= 10<sup>3</sup></code>
*   `nums.length == n`
*   <code>0 <= nums[i] <= 5 * 10<sup>4</sup></code>
*   The input is generated such that `edges` represents a valid tree.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;

@SuppressWarnings("unchecked")
public class Solution {
    private ArrayList<int[]>[] adj;
    private int[] nums;
    private int[] dist;
    private int[] lastOccur;
    private ArrayList<Integer> pathStack;
    private int minIndex;
    private int maxLen;
    private int minNodesForMaxLen;

    public int[] longestSpecialPath(int[][] edges, int[] nums) {
        int n = nums.length;
        this.nums = nums;
        adj = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<>();
        }
        for (int[] e : edges) {
            int u = e[0];
            int v = e[1];
            int w = e[2];
            adj[u].add(new int[] {v, w});
            adj[v].add(new int[] {u, w});
        }
        dist = new int[n];
        buildDist(0, -1, 0);
        int maxVal = 0;
        for (int val : nums) {
            if (val > maxVal) {
                maxVal = val;
            }
        }
        lastOccur = new int[maxVal + 1];
        Arrays.fill(lastOccur, -1);
        pathStack = new ArrayList<>();
        minIndex = 0;
        maxLen = 0;
        minNodesForMaxLen = Integer.MAX_VALUE;
        dfs(0, -1);
        return new int[] {maxLen, minNodesForMaxLen};
    }

    private void buildDist(int u, int parent, int currDist) {
        dist[u] = currDist;
        for (int[] edge : adj[u]) {
            int v = edge[0];
            int w = edge[1];
            if (v == parent) {
                continue;
            }
            buildDist(v, u, currDist + w);
        }
    }

    private void dfs(int u, int parent) {
        int stackPos = pathStack.size();
        pathStack.add(u);
        int val = nums[u];
        int oldPos = lastOccur[val];
        int oldMinIndex = minIndex;
        lastOccur[val] = stackPos;
        if (oldPos >= minIndex) {
            minIndex = oldPos + 1;
        }
        if (minIndex <= stackPos) {
            int ancestor = pathStack.get(minIndex);
            int pathLength = dist[u] - dist[ancestor];
            int pathNodes = stackPos - minIndex + 1;
            if (pathLength > maxLen) {
                maxLen = pathLength;
                minNodesForMaxLen = pathNodes;
            } else if (pathLength == maxLen && pathNodes < minNodesForMaxLen) {
                minNodesForMaxLen = pathNodes;
            }
        }
        for (int[] edge : adj[u]) {
            int v = edge[0];
            if (v == parent) {
                continue;
            }
            dfs(v, u);
        }
        pathStack.remove(pathStack.size() - 1);
        lastOccur[val] = oldPos;
        minIndex = oldMinIndex;
    }
}
```