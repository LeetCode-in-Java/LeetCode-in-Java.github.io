[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 834\. Sum of Distances in Tree

Hard

There is an undirected connected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given the integer `n` and the array `edges` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

Return an array `answer` of length `n` where `answer[i]` is the sum of the distances between the <code>i<sup>th</sup></code> node in the tree and all other nodes.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg)

**Input:** n = 6, edges = \[\[0,1],[0,2],[2,3],[2,4],[2,5]]

**Output:** [8,12,6,10,10,10]

**Explanation:** The tree is shown above. 

We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5) equals 1 + 1 + 2 + 2 + 2 = 8. 

Hence, answer[0] = 8, and so on.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist2.jpg)

**Input:** n = 1, edges = []

**Output:** [0]

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist3.jpg)

**Input:** n = 2, edges = \[\[1,0]]

**Output:** [1,1]

**Constraints:**

*   <code>1 <= n <= 3 * 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   The given input represents a valid tree.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    private int n;
    private int[] count;
    private int[] answer;
    private List<Integer>[] graph;

    private void postorder(int node, int parent) {
        for (int child : graph[node]) {
            if (child != parent) {
                postorder(child, node);
                count[node] += count[child];
                answer[node] += answer[child] + count[child];
            }
        }
    }

    private void preorder(int node, int parent) {

        for (int child : graph[node]) {
            if (child != parent) {
                answer[child] = answer[node] - count[child] + n - count[child];
                preorder(child, node);
            }
        }
    }

    public int[] sumOfDistancesInTree(int n, int[][] edges) {
        this.n = n;
        count = new int[n];
        answer = new int[n];
        graph = new List[n];
        Arrays.fill(count, 1);
        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        postorder(0, -1);
        preorder(0, -1);
        return answer;
    }
}
```