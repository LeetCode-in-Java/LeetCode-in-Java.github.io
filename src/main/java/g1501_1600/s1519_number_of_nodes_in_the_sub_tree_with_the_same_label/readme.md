[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1519\. Number of Nodes in the Sub-Tree With the Same Label

Medium

You are given a tree (i.e. a connected, undirected graph that has no cycles) consisting of `n` nodes numbered from `0` to `n - 1` and exactly `n - 1` `edges`. The **root** of the tree is the node `0`, and each node of the tree has **a label** which is a lower-case character given in the string `labels` (i.e. The node with the number `i` has the label `labels[i]`).

The `edges` array is given on the form <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>, which means there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

Return _an array of size `n`_ where `ans[i]` is the number of nodes in the subtree of the <code>i<sup>th</sup></code> node which have the same label as node `i`.

A subtree of a tree `T` is the tree consisting of a node in `T` and all of its descendant nodes.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/01/q3e1.jpg)

**Input:** n = 7, edges = \[\[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], labels = "abaedcd"

**Output:** [2,1,1,1,1,1,1]

**Explanation:** Node 0 has label 'a' and its sub-tree has node 2 with label 'a' as well, thus the answer is 2. Notice that any node is part of its sub-tree. 

Node 1 has a label 'b'. The sub-tree of node 1 contains nodes 1,4 and 5, as nodes 4 and 5 have different labels than node 1, the answer is just 1 (the node itself).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/07/01/q3e2.jpg)

**Input:** n = 4, edges = \[\[0,1],[1,2],[0,3]], labels = "bbbb"

**Output:** [4,2,1,1]

**Explanation:** The sub-tree of node 2 contains only node 2, so the answer is 1.

The sub-tree of node 3 contains only node 3, so the answer is 1. 

The sub-tree of node 1 contains nodes 1 and 2, both have label 'b', thus the answer is 2. 

The sub-tree of node 0 contains nodes 0, 1, 2 and 3, all with label 'b', thus the answer is 4.

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/07/01/q3e3.jpg)

**Input:** n = 5, edges = \[\[0,1],[0,2],[1,3],[0,4]], labels = "aabab"

**Output:** [3,2,1,1,1]

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   `labels.length == n`
*   `labels` is consisting of only of lowercase English letters.

## Solution

```java
import java.util.ArrayList;

public class Solution {
    public int[] countSubTrees(int n, int[][] edges, String labelsString) {
        int[] labelsCount = new int[n];
        if (n == 0 || edges == null || labelsString == null) {
            return labelsCount;
        }

        int[] labels = new int[n];
        int nodeNumber = 0;
        for (char label : labelsString.toCharArray()) {
            labels[nodeNumber++] = label - 'a';
        }

        ArrayList<ArrayList<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] edge : edges) {
            int parent = edge[0];
            int child = edge[1];
            graph.get(parent).add(child);
            graph.get(child).add(parent);
        }

        getLabelsFrequency(0, graph, labels, labelsCount, 0);

        return labelsCount;
    }

    private int[] getLabelsFrequency(
            int root,
            ArrayList<ArrayList<Integer>> graph,
            int[] labels,
            int[] labelsCount,
            int parent) {
        int[] labelsFrequency = new int[26];
        int rootLabel = labels[root];
        labelsFrequency[rootLabel]++;

        for (int child : graph.get(root)) {
            if (child == parent) {
                continue;
            }
            int[] childLabelsFrequency =
                    getLabelsFrequency(child, graph, labels, labelsCount, root);
            for (int i = 0; i < childLabelsFrequency.length; i++) {
                labelsFrequency[i] += childLabelsFrequency[i];
            }
        }

        labelsCount[root] = labelsFrequency[rootLabel];
        return labelsFrequency;
    }
}
```