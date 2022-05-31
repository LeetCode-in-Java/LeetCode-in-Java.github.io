## 886\. Possible Bipartition

Medium

We want to split a group of `n` people (labeled from `1` to `n`) into two groups of **any size**. Each person may dislike some other people, and they should not go into the same group.

Given the integer `n` and the array `dislikes` where <code>dislikes[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that the person labeled <code>a<sub>i</sub></code> does not like the person labeled <code>b<sub>i</sub></code>, return `true` _if it is possible to split everyone into two groups in this way_.

**Example 1:**

**Input:** n = 4, dislikes = \[\[1,2],[1,3],[2,4]]

**Output:** true

**Explanation:** group1 [1,4] and group2 [2,3]. 

**Example 2:**

**Input:** n = 3, dislikes = \[\[1,2],[1,3],[2,3]]

**Output:** false 

**Example 3:**

**Input:** n = 5, dislikes = \[\[1,2],[2,3],[3,4],[4,5],[1,5]]

**Output:** false 

**Constraints:**

*   `1 <= n <= 2000`
*   <code>0 <= dislikes.length <= 10<sup>4</sup></code>
*   `dislikes[i].length == 2`
*   `1 <= dislikes[i][j] <= n`
*   <code>a<sub>i</sub> < b<sub>i</sub></code>
*   All the pairs of `dislikes` are **unique**.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    public boolean possibleBipartition(int n, int[][] dislikes) {
        // build graph
        Graph g = new Graph(n);
        for (int[] dislike : dislikes) {
            g.addEdge(dislike[0] - 1, dislike[1] - 1);
        }
        boolean[] marked = new boolean[n];
        boolean[] colors = new boolean[n];
        for (int v = 0; v < n; v++) {
            if (!marked[v] && !checkBipartiteDFS(g, marked, colors, v)) {
                // No need to run on other connected components if one component has failed.
                return false;
            }
        }
        return true;
    }

    private boolean checkBipartiteDFS(Graph g, boolean[] marked, boolean[] colors, int v) {
        marked[v] = true;
        for (int w : g.adj(v)) {
            if (!marked[w]) {
                colors[w] = !colors[v];
                if (!checkBipartiteDFS(g, marked, colors, w)) {
                    // this is to break for other neighbours
                    return false;
                }
            } else if (colors[v] == colors[w]) {
                return false;
            }
        }
        return true;
    }

    private static class Graph {
        private ArrayList<Integer>[] adj;

        public Graph(int v) {
            adj = new ArrayList[v];
            for (int i = 0; i < v; i++) {
                adj[i] = new ArrayList<>();
            }
        }

        private void addEdge(int v, int w) {
            adj[v].add(w);
            adj[w].add(v);
        }

        private List<Integer> adj(int v) {
            return adj[v];
        }
    }
}
```