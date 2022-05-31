## 1761\. Minimum Degree of a Connected Trio in a Graph

Hard

You are given an undirected graph. You are given an integer `n` which is the number of nodes in the graph and an array `edges`, where each <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that there is an undirected edge between <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

A **connected trio** is a set of **three** nodes where there is an edge between **every** pair of them.

The **degree of a connected trio** is the number of edges where one endpoint is in the trio, and the other is not.

Return _the **minimum** degree of a connected trio in the graph, or_ `-1` _if the graph has no connected trios._

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/26/trios1.png)

**Input:** n = 6, edges = [[1,2],[1,3],[3,2],[4,1],[5,2],[3,6]]

**Output:** 3

**Explanation:** There is exactly one trio, which is [1,2,3]. The edges that form its degree are bolded in the figure above.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/26/trios2.png)

**Input:** n = 7, edges = [[1,3],[4,1],[4,3],[2,5],[5,6],[6,7],[7,5],[2,6]]

**Output:** 0

**Explanation:** There are exactly three trios: 

1) [1,4,3] with degree 0. 

2) [2,5,6] with degree 2. 

3) [5,6,7] with degree 2.

**Constraints:**

*   `2 <= n <= 400`
*   `edges[i].length == 2`
*   `1 <= edges.length <= n * (n-1) / 2`
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   There are no repeated edges.

## Solution

```java
public class Solution {
    public int minTrioDegree(int n, int[][] edges) {
        int[] degrees = new int[n + 1];
        int[][] adjMatrix = new int[n + 1][n + 1];
        for (int[] edge : edges) {
            adjMatrix[edge[0]][edge[1]] = 1;
            adjMatrix[edge[1]][edge[0]] = 1;
            degrees[edge[0]]++;
            degrees[edge[1]]++;
        }
        int minTrios = Integer.MAX_VALUE;
        for (int i = 1; i <= n; i++) {
            for (int j = i + 1; j <= n; j++) {
                if (adjMatrix[i][j] == 0) {
                    continue;
                }
                for (int k = j + 1; k <= n; k++) {
                    if (adjMatrix[j][k] == 0 || adjMatrix[i][k] == 0) {
                        continue;
                    }
                    int trioDegree = degrees[i] + degrees[j] + degrees[k] - 6;
                    minTrios = Math.min(minTrios, trioDegree);
                }
            }
        }
        return minTrios == Integer.MAX_VALUE ? -1 : minTrios;
    }
}
```