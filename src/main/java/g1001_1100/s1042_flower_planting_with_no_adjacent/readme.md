## 1042\. Flower Planting With No Adjacent

Medium

You have `n` gardens, labeled from `1` to `n`, and an array `paths` where <code>paths[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> describes a bidirectional path between garden <code>x<sub>i</sub></code> to garden <code>y<sub>i</sub></code>. In each garden, you want to plant one of 4 types of flowers.

All gardens have **at most 3** paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return _**any** such a choice as an array_ `answer`_, where_ `answer[i]` _is the type of flower planted in the_ <code>(i+1)<sup>th</sup></code> _garden. The flower types are denoted_ `1`_,_ `2`_,_ `3`_, or_ `4`_. It is guaranteed an answer exists._

**Example 1:**

**Input:** n = 3, paths = \[\[1,2],[2,3],[3,1]]

**Output:** [1,2,3]

**Explanation:** 

Gardens 1 and 2 have different types. 

Gardens 2 and 3 have different types. 

Gardens 3 and 1 have different types. 

Hence, [1,2,3] is a valid answer. Other valid answers include [1,2,4], [1,4,2], and [3,2,1].

**Example 2:**

**Input:** n = 4, paths = \[\[1,2],[3,4]]

**Output:** [1,2,1,2]

**Example 3:**

**Input:** n = 4, paths = \[\[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]

**Output:** [1,2,3,4]

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>0 <= paths.length <= 2 * 10<sup>4</sup></code>
*   `paths[i].length == 2`
*   <code>1 <= x<sub>i</sub>, y<sub>i</sub> <= n</code>
*   <code>x<sub>i</sub> != y<sub>i</sub></code>
*   Every garden has **at most 3** paths coming into or leaving it.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    private List<Integer>[] graph;
    private int[] color;
    private boolean[] visited;

    public int[] gardenNoAdj(int n, int[][] paths) {
        buildGraph(n, paths);
        this.color = new int[n];
        this.visited = new boolean[n];

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i);
            }
        }

        return color;
    }

    private void dfs(int at) {
        visited[at] = true;
        int used = 0;

        for (int to : graph[at]) {
            if (color[to] != 0) {
                used |= 1 << color[to] - 1;
            }
        }

        // use available color
        for (int i = 0; i < 4; i++) {
            if ((used & 1 << i) == 0) {
                color[at] = i + 1;
                break;
            }
        }
    }

    private void buildGraph(int n, int[][] paths) {
        graph = new ArrayList[n];

        for (int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int[] path : paths) {
            int u = path[0] - 1;
            int v = path[1] - 1;
            graph[u].add(v);
            graph[v].add(u);
        }
    }
}
```