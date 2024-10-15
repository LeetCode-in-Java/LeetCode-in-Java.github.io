[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3310\. Remove Methods From Project

Medium

You are maintaining a project that has `n` methods numbered from `0` to `n - 1`.

You are given two integers `n` and `k`, and a 2D integer array `invocations`, where <code>invocations[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that method <code>a<sub>i</sub></code> invokes method <code>b<sub>i</sub></code>.

There is a known bug in method `k`. Method `k`, along with any method invoked by it, either **directly** or **indirectly**, are considered **suspicious** and we aim to remove them.

A group of methods can only be removed if no method **outside** the group invokes any methods **within** it.

Return an array containing all the remaining methods after removing all the **suspicious** methods. You may return the answer in _any order_. If it is not possible to remove **all** the suspicious methods, **none** should be removed.

**Example 1:**

**Input:** n = 4, k = 1, invocations = \[\[1,2],[0,1],[3,2]]

**Output:** [0,1,2,3]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/18/graph-2.png)

Method 2 and method 1 are suspicious, but they are directly invoked by methods 3 and 0, which are not suspicious. We return all elements without removing anything.

**Example 2:**

**Input:** n = 5, k = 0, invocations = \[\[1,2],[0,2],[0,1],[3,4]]

**Output:** [3,4]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/18/graph-3.png)

Methods 0, 1, and 2 are suspicious and they are not directly invoked by any other method. We can remove them.

**Example 3:**

**Input:** n = 3, k = 2, invocations = \[\[1,2],[0,1],[2,0]]

**Output:** []

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/20/graph.png)

All methods are suspicious. We can remove them.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= k <= n - 1`
*   <code>0 <= invocations.length <= 2 * 10<sup>5</sup></code>
*   <code>invocations[i] == [a<sub>i</sub>, b<sub>i</sub>]</code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> <= n - 1</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   `invocations[i] != invocations[j]`

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    private int[][] graph;
    private boolean[] suspicious;
    private boolean[] visited;

    public List<Integer> remainingMethods(int n, int k, int[][] invocations) {
        pack(invocations, n);
        suspicious = new boolean[n];
        visited = new boolean[n];
        dfs(k, true);
        Arrays.fill(visited, false);
        for (int i = 0; i < n; i++) {
            if (!suspicious[i] && dfs2(i)) {
                Arrays.fill(visited, false);
                dfs(k, false);
                break;
            }
        }
        ArrayList<Integer> rst = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (!suspicious[i]) {
                rst.add(i);
            }
        }
        return rst;
    }

    public void dfs(int u, boolean sus) {
        if (visited[u]) {
            return;
        }
        visited[u] = true;
        suspicious[u] = sus;
        for (int v : graph[u]) {
            dfs(v, sus);
        }
    }

    public boolean dfs2(int u) {
        if (suspicious[u]) {
            return true;
        }
        if (visited[u]) {
            return false;
        }
        visited[u] = true;
        for (int v : graph[u]) {
            if (dfs2(v)) {
                return true;
            }
        }
        return false;
    }

    private void pack(int[][] edges, int n) {
        int[] adj = new int[n];
        for (int[] edge : edges) {
            adj[edge[0]]++;
        }
        graph = new int[n][];
        for (int i = 0; i < n; i++) {
            graph[i] = new int[adj[i]];
        }
        for (int[] edge : edges) {
            graph[edge[0]][--adj[edge[0]]] = edge[1];
        }
    }
}
```