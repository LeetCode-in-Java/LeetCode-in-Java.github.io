[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 207\. Course Schedule

Medium

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where <code>prerequisites[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that you **must** take course <code>b<sub>i</sub></code> first if you want to take course <code>a<sub>i</sub></code>.

*   For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

**Input:** numCourses = 2, prerequisites = \[\[1,0]]

**Output:** true

**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible. 

**Example 2:**

**Input:** numCourses = 2, prerequisites = \[\[1,0],[0,1]]

**Output:** false

**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible. 

**Constraints:**

*   <code>1 <= numCourses <= 10<sup>5</sup></code>
*   `0 <= prerequisites.length <= 5000`
*   `prerequisites[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < numCourses</code>
*   All the pairs prerequisites[i] are **unique**.

## Solution

```java
import java.util.ArrayList;

@SuppressWarnings("unchecked")
public class Solution {
    private static final int WHITE = 0;
    private static final int GRAY = 1;
    private static final int BLACK = 2;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        ArrayList<Integer>[] adj = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            adj[i] = new ArrayList<>();
        }
        for (int[] pre : prerequisites) {
            adj[pre[1]].add(pre[0]);
        }
        int[] colors = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            if (colors[i] == WHITE && !adj[i].isEmpty() && hasCycle(adj, i, colors)) {
                return false;
            }
        }
        return true;
    }

    private boolean hasCycle(ArrayList<Integer>[] adj, int node, int[] colors) {
        colors[node] = GRAY;
        for (int nei : adj[node]) {
            if (colors[nei] == GRAY) {
                return true;
            }
            if (colors[nei] == WHITE && hasCycle(adj, nei, colors)) {
                return true;
            }
        }
        colors[node] = BLACK;
        return false;
    }
}
```

**Time Complexity (Big O Time):**

1. The program constructs an adjacency list representation of the directed graph, where each course is a node and prerequisites define edges. Constructing this graph takes O(E) time, where E is the number of prerequisites (edges). In the worst case, E can be proportional to the number of courses (N), so this step is O(N).

2. The program performs a depth-first search (DFS) on the graph to detect cycles. In the worst case, the DFS may visit all nodes, which is O(N).

3. Within the DFS, for each node, the program explores all of its neighbors. In the worst case, this exploration takes O(N) time.

4. Combining all these steps, the overall time complexity is O(N) + O(N) + O(N) = O(N).

So, the time complexity of this program is O(N).

**Space Complexity (Big O Space):**

1. The program uses additional space for several data structures:
   - `adj`: An adjacency list representation of the graph. In the worst case, it has O(E) space, which can be proportional to O(N) in certain scenarios.
   - `colors`: An array to keep track of the colors (WHITE, GRAY, BLACK) of nodes. This array requires O(N) space.
   - Recursive function call stack for DFS, which can go up to O(N) in the worst case when there are no cycles.

2. Ignoring constant factors and smaller terms, the dominant space complexity factors are O(N).

So, the space complexity of this program is O(N).

In summary, the program has a time complexity of O(N) and a space complexity of O(N), where N is the number of courses or nodes in the graph.
