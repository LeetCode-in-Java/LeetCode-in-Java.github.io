[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3241\. Time Taken to Mark All Nodes

Hard

There exists an **undirected** tree with `n` nodes numbered `0` to `n - 1`. You are given a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> in the tree.

Initially, **all** nodes are **unmarked**. For each node `i`:

*   If `i` is odd, the node will get marked at time `x` if there is **at least** one node _adjacent_ to it which was marked at time `x - 1`.
*   If `i` is even, the node will get marked at time `x` if there is **at least** one node _adjacent_ to it which was marked at time `x - 2`.

Return an array `times` where `times[i]` is the time when all nodes get marked in the tree, if you mark node `i` at time `t = 0`.

**Note** that the answer for each `times[i]` is **independent**, i.e. when you mark node `i` all other nodes are _unmarked_.

**Example 1:**

**Input:** edges = \[\[0,1],[0,2]]

**Output:** [2,4,3]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/01/screenshot-2024-06-02-122236.png)

*   For `i = 0`:
    *   Node 1 is marked at `t = 1`, and Node 2 at `t = 2`.
*   For `i = 1`:
    *   Node 0 is marked at `t = 2`, and Node 2 at `t = 4`.
*   For `i = 2`:
    *   Node 0 is marked at `t = 2`, and Node 1 at `t = 3`.

**Example 2:**

**Input:** edges = \[\[0,1]]

**Output:** [1,2]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/01/screenshot-2024-06-02-122249.png)

*   For `i = 0`:
    *   Node 1 is marked at `t = 1`.
*   For `i = 1`:
    *   Node 0 is marked at `t = 2`.

**Example 3:**

**Input:** edges = \[\[2,4],[0,1],[2,3],[0,2]]

**Output:** [4,6,3,5,5]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/06/03/screenshot-2024-06-03-210550.png)

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   `0 <= edges[i][0], edges[i][1] <= n - 1`
*   The input is generated such that `edges` represents a valid tree.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[] head;
    private int[] nxt;
    private int[] to;
    private int[] last;
    private int[] lastNo;
    private int[] second;
    private int[] ans;

    public int[] timeTaken(int[][] edges) {
        int n = edges.length + 1;
        head = new int[n];
        nxt = new int[n << 1];
        to = new int[n << 1];
        Arrays.fill(head, -1);
        int i = 0;
        int j = 2;
        while (i < edges.length) {
            int u = edges[i][0];
            int v = edges[i][1];
            nxt[j] = head[u];
            head[u] = j;
            to[j] = v;
            j++;
            nxt[j] = head[v];
            head[v] = j;
            to[j] = u;
            j++;
            i++;
        }
        last = new int[n];
        lastNo = new int[n];
        second = new int[n];
        ans = new int[n];
        dfs(-1, 0);
        System.arraycopy(last, 0, ans, 0, n);
        dfs2(-1, 0, 0);
        return ans;
    }

    private void dfs2(int f, int u, int preLast) {
        int e = head[u];
        int v;
        while (e != -1) {
            v = to[e];
            if (f != v) {
                int pl;
                if (v == lastNo[u]) {
                    pl = Math.max(preLast, second[u]) + ((u & 1) == 0 ? 2 : 1);
                } else {
                    pl = Math.max(preLast, last[u]) + ((u & 1) == 0 ? 2 : 1);
                }
                ans[v] = Math.max(ans[v], pl);
                dfs2(u, v, pl);
            }
            e = nxt[e];
        }
    }

    private void dfs(int f, int u) {
        int e = head[u];
        int v;
        while (e != -1) {
            v = to[e];
            if (f != v) {
                dfs(u, v);
                int t = last[v] + ((v & 1) == 0 ? 2 : 1);
                if (last[u] < t) {
                    second[u] = last[u];
                    last[u] = t;
                    lastNo[u] = v;
                } else if (second[u] < t) {
                    second[u] = t;
                }
            }
            e = nxt[e];
        }
    }
}
```