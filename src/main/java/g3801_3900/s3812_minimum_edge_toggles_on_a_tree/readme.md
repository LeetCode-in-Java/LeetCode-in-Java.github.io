[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3812\. Minimum Edge Toggles on a Tree

Hard

You are given an **undirected tree** with `n` nodes, numbered from 0 to `n - 1`. It is represented by a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

You are also given two **binary** strings `start` and `target` of length `n`. For each node `x`, `start[x]` is its initial color and `target[x]` is its desired color.

In one operation, you may pick an edge with index `i` and **toggle** both of its endpoints. That is, if the edge is `[u, v]`, then the colors of nodes `u` and `v` **each** flip from `'0'` to `'1'` or from `'1'` to `'0'`.

Return an array of edge indices whose operations transform `start` into `target`. Among all valid sequences with **minimum possible length**, return the edge indices in **increasing** order.

If it is impossible to transform `start` into `target`, return an array containing a single element equal to -1.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2025/12/18/example1.png)**

**Input:** n = 3, edges = \[\[0,1],[1,2]], start = "010", target = "100"

**Output:** [0]

**Explanation:**

Toggle edge with index 0, which flips nodes 0 and 1.   
The string changes from `"010"` to `"100"`, matching the target.

**Example 2:**

**![](https://assets.leetcode.com/uploads/2025/12/18/example2.png)**

**Input:** n = 7, edges = \[\[0,1],[1,2],[2,3],[3,4],[3,5],[1,6]], start = "0011000", target = "0010001"

**Output:** [1,2,5]

**Explanation:**

*   Toggle edge with index 1, which flips nodes 1 and 2.
*   Toggle edge with index 2, which flips nodes 2 and 3.
*   Toggle edge with index 5, which flips nodes 1 and 6.

After these operations, the resulting string becomes `"0010001"`, which matches the target.

**Example 3:**

**![](https://assets.leetcode.com/uploads/2025/12/18/example3.png)**

**Input:** n = 2, edges = \[\[0,1]], start = "00", target = "01"

**Output:** [-1]

**Explanation:**

There is no sequence of edge toggles that transforms `"00"` into `"01"`. Therefore, we return `[-1]`.

**Constraints:**

*   <code>2 <= n == start.length == target.length <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   `start[i]` is either `'0'` or `'1'`.
*   `target[i]` is either `'0'` or `'1'`.
*   The input is generated such that `edges` represents a valid tree.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    private boolean[] visited;
    private List<int[]>[] adjList;
    private int[] flips;
    private List<Integer> ans;
    private String s;
    private String t;

    @SuppressWarnings("unchecked")
    public List<Integer> minimumFlips(int n, int[][] edges, String start, String target) {
        s = start;
        t = target;
        flips = new int[n];
        adjList = new List[n];
        for (int i = 0; i < n; i++) {
            adjList[i] = new ArrayList<>();
        }
        for (int i = 0; i < edges.length; i++) {
            int[] edge = edges[i];
            adjList[edge[0]].add(new int[] {edge[1], i});
            adjList[edge[1]].add(new int[] {edge[0], i});
        }
        ans = new ArrayList<>();
        visited = new boolean[n];
        dfs(0, -1, -1);
        // Sort indices as required
        Collections.sort(ans);
        // If root is still mismatched, it's impossible (no parent to flip)
        if (mismatch(0)) {
            ans = new ArrayList<>();
            ans.add(-1);
        }
        return ans;
    }

    // Helper to check if current state matches target
    // Logic: If flips is even, bit is original. If flips is odd, bit is inverted.
    private boolean mismatch(int u) {
        return ((flips[u] % 2 == 0) && s.charAt(u) != t.charAt(u))
                || ((flips[u] % 2 == 1) && s.charAt(u) == t.charAt(u));
    }

    private void dfs(int u, int parent, int index) {
        visited[u] = true;
        for (int[] v : adjList[u]) {
            if (!visited[v[0]]) {
                dfs(v[0], u, v[1]);
            }
        }
        if (parent != -1 && (mismatch(u))) {
            ans.add(index);
            flips[u]++;
            flips[parent]++;
        }
    }
}
```