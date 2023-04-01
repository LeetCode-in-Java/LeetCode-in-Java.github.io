[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1203\. Sort Items by Groups Respecting Dependencies

Hard

There are `n` items each belonging to zero or one of `m` groups where `group[i]` is the group that the `i`\-th item belongs to and it's equal to `-1` if the `i`\-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

*   The items that belong to the same group are next to each other in the sorted list.
*   There are some relations between these items where `beforeItems[i]` is a list containing all the items that should come before the `i`\-th item in the sorted array (to the left of the `i`\-th item).

Return any solution if there is more than one solution and return an **empty list** if there is no solution.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/11/1359_ex1.png)**

**Input:** n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = \[\[],[6],[5],[6],[3,6],[],[],[]]

**Output:** [6,3,4,1,5,2,0,7]

**Example 2:**

**Input:** n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = \[\[],[6],[5],[6],[3],[],[4],[]]

**Output:** []

**Explanation:** This is the same as example 1 except that 4 needs to be before 6 in the sorted list.

**Constraints:**

*   <code>1 <= m <= n <= 3 * 10<sup>4</sup></code>
*   `group.length == beforeItems.length == n`
*   `-1 <= group[i] <= m - 1`
*   `0 <= beforeItems[i].length <= n - 1`
*   `0 <= beforeItems[i][j] <= n - 1`
*   `i != beforeItems[i][j]`
*   `beforeItems[i] `does not contain duplicates elements.

## Solution

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

@SuppressWarnings("java:S1149")
public class Solution {

    private static int[] topoSort(int temp, ArrayList<ArrayList<Integer>> adj) {
        int[] indegree = new int[temp];
        for (ArrayList<Integer> edges : adj) {
            for (Integer edge : edges) {
                indegree[edge]++;
            }
        }
        Stack<Integer> q = new Stack<>();
        for (int i = 0; i < temp; ++i) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        if (q.isEmpty()) {
            return new int[0];
        }
        ArrayList<Integer> topo = new ArrayList<>();
        while (!q.isEmpty()) {
            int v = q.pop();
            topo.add(v);
            ArrayList<Integer> edges = adj.get(v);
            for (int nbr : edges) {
                indegree[nbr]--;
                if (indegree[nbr] == 0) {
                    q.push(nbr);
                }
            }
        }
        if (topo.size() != temp) {
            return new int[0];
        } else {
            int[] ans = new int[topo.size()];
            for (int i = 0; i < topo.size(); ++i) {
                ans[i] = topo.get(i);
            }
            return ans;
        }
    }

    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        ArrayList<ArrayList<Integer>> graph = new ArrayList<>();
        int nodes = n + 2 * m;
        for (int i = 0; i < nodes; ++i) {
            graph.add(new ArrayList<>());
        }
        for (int i = 0; i < n; ++i) {
            List<Integer> before = beforeItems.get(i);
            if (group[i] == -1 && before.isEmpty()) {
                // continue
            } else if (before.isEmpty()) {
                int groupStart = n + group[i] * 2;
                int ge = n + group[i] * 2 + 1;
                graph.get(groupStart).add(i);
                graph.get(i).add(ge);
            } else if (group[i] == -1) {
                for (int temp : before) {
                    if (group[temp] == -1) {
                        graph.get(temp).add(i);
                    } else {
                        int ge = n + group[temp] * 2 + 1;
                        graph.get(ge).add(i);
                    }
                }
            } else {
                int groupStart = n + group[i] * 2;
                int groupEnd = n + group[i] * 2 + 1;
                graph.get(groupStart).add(i);
                graph.get(i).add(groupEnd);
                for (int temp : before) {
                    if (group[temp] == -1) {
                        graph.get(temp).add(groupStart);
                    } else {
                        if (group[temp] == group[i]) {
                            graph.get(temp).add(i);
                        } else {
                            groupEnd = n + group[temp] * 2 + 1;
                            graph.get(groupEnd).add(groupStart);
                        }
                    }
                }
            }
        }
        int[] temp = topoSort(nodes, graph);
        if (temp.length == 0) {
            return temp;
        }
        int[] ans = new int[n];
        int j = 0;
        for (int k : temp) {
            if (k < n) {
                ans[j++] = k;
            }
        }
        return ans;
    }
}
```