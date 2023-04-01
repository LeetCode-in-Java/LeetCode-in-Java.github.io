[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2508\. Add Edges to Make Degrees of All Nodes Even

Hard

There is an **undirected** graph consisting of `n` nodes numbered from `1` to `n`. You are given the integer `n` and a **2D** array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>. The graph can be disconnected.

You can add **at most** two additional edges (possibly none) to this graph so that there are no repeated edges and no self-loops.

Return `true` _if it is possible to make the degree of each node in the graph even, otherwise return_ `false`_._

The degree of a node is the number of edges connected to it.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/10/26/agraphdrawio.png)

**Input:** n = 5, edges = \[\[1,2],[2,3],[3,4],[4,2],[1,4],[2,5]]

**Output:** true

**Explanation:** The above diagram shows a valid way of adding an edge. Every node in the resulting graph is connected to an even number of edges.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/10/26/aagraphdrawio.png)

**Input:** n = 4, edges = \[\[1,2],[3,4]]

**Output:** true

**Explanation:** The above diagram shows a valid way of adding two edges.

**Example 3:**

![](https://assets.leetcode.com/uploads/2022/10/26/aaagraphdrawio.png)

**Input:** n = 4, edges = \[\[1,2],[1,3],[1,4]]

**Output:** false

**Explanation:** It is not possible to obtain a valid graph with adding at most 2 edges.

**Constraints:**

*   <code>3 <= n <= 10<sup>5</sup></code>
*   <code>2 <= edges.length <= 10<sup>5</sup></code>
*   `edges[i].length == 2`
*   <code>1 <= a<sub>i</sub>, b<sub>i</sub> <= n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   There are no repeated edges.

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

@SuppressWarnings("unchecked")
public class Solution {
    public boolean isPossible(int n, List<List<Integer>> edges) {
        // first find odd edge nodes
        int[] degree = new int[n + 1];
        for (List<Integer> edge : edges) {
            degree[edge.get(0)]++;
            degree[edge.get(1)]++;
        }
        List<Integer> oddNodes = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            if ((degree[i] & 1) == 1) {
                oddNodes.add(i);
            }
            if (oddNodes.size() > 4) {
                // cannot be larger than four
                return false;
            }
        }
        if ((oddNodes.size() & 1) == 1) {
            // can only have even numbers of odd nodes
            return false;
        } else if (oddNodes.isEmpty()) {
            // zero situation
            return true;
        }
        // find all edges of oddListNode
        Set[] adjList = new HashSet[oddNodes.size()];
        for (int i = 0; i < oddNodes.size(); i++) {
            adjList[i] = new HashSet<>();
        }
        for (List<Integer> edge : edges) {
            for (int i = 0; i < oddNodes.size(); i++) {
                if ((int) edge.get(0) == oddNodes.get(i)) {
                    adjList[i].add(edge.get(1));
                }
                if ((int) edge.get(1) == oddNodes.get(i)) {
                    adjList[i].add(edge.get(0));
                }
            }
        }
        // to see if it is two or four
        if (oddNodes.size() == 4) {
            // can only connect each other
            // have to detect if they have an edge or not
            return adjList[0].size() < (n - 1)
                    && adjList[1].size() < (n - 1)
                    && adjList[2].size() < (n - 1)
                    && adjList[3].size() < (n - 1)
                    && ((!adjList[0].contains(oddNodes.get(1))
                                    && !adjList[1].contains(oddNodes.get(0))
                                    && !adjList[2].contains(oddNodes.get(3))
                                    && !adjList[3].contains(oddNodes.get(2)))
                            || (!adjList[0].contains(oddNodes.get(2))
                                    && !adjList[2].contains(oddNodes.get(0))
                                    && !adjList[1].contains(oddNodes.get(3))
                                    && !adjList[3].contains(oddNodes.get(1)))
                            || (!adjList[0].contains(oddNodes.get(3))
                                    && !adjList[1].contains(oddNodes.get(2))
                                    && !adjList[2].contains(oddNodes.get(1))));
        } else {
            // if two dont have an edge, could use it
            if (adjList[0].contains(oddNodes.get(1))) {
                // need to find a spare node
                for (int i = 1; i <= n; i++) {
                    if (adjList[0].contains(i) || adjList[1].contains(i)) {
                        continue;
                    }
                    return true;
                }
                return false;
            }
            return true;
        }
    }
}
```