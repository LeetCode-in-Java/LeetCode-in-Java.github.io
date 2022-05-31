## 815\. Bus Routes

Hard

You are given an array `routes` representing bus routes where `routes[i]` is a bus route that the <code>i<sup>th</sup></code> bus repeats forever.

*   For example, if `routes[0] = [1, 5, 7]`, this means that the <code>0<sup>th</sup></code> bus travels in the sequence `1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ...` forever.

You will start at the bus stop `source` (You are not on any bus initially), and you want to go to the bus stop `target`. You can travel between bus stops by buses only.

Return _the least number of buses you must take to travel from_ `source` _to_ `target`. Return `-1` if it is not possible.

**Example 1:**

**Input:** routes = [[1,2,7],[3,6,7]], source = 1, target = 6

**Output:** 2

**Explanation:** The best strategy is take the first bus to the bus stop 7, then take the second bus to the bus stop 6.

**Example 2:**

**Input:** routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12

**Output:** -1

**Constraints:**

*   `1 <= routes.length <= 500`.
*   <code>1 <= routes[i].length <= 10<sup>5</sup></code>
*   All the values of `routes[i]` are **unique**.
*   <code>sum(routes[i].length) <= 10<sup>5</sup></code>
*   <code>0 <= routes[i][j] < 10<sup>6</sup></code>
*   <code>0 <= source, target < 10<sup>6</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Queue;
import java.util.Set;

@SuppressWarnings("unchecked")
public class Solution {
    public int numBusesToDestination(int[][] routes, int source, int target) {
        if (source == target) {
            return 0;
        }
        Set<Integer> targetRoutes = new HashSet<>();
        Queue<Integer> queue = new ArrayDeque<>();
        boolean[] taken = new boolean[routes.length];
        List<Integer>[] graph = buildGraph(routes, source, target, queue, targetRoutes, taken);
        if (targetRoutes.isEmpty()) {
            return -1;
        }
        int bus = 1;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int route = queue.poll();
                if (targetRoutes.contains(route)) {
                    return bus;
                }
                for (int nextRoute : graph[route]) {
                    if (!taken[nextRoute]) {
                        queue.offer(nextRoute);
                        taken[nextRoute] = true;
                    }
                }
            }
            bus++;
        }
        return -1;
    }

    private List<Integer>[] buildGraph(
            int[][] routes,
            int source,
            int target,
            Queue<Integer> queue,
            Set<Integer> targetRoutes,
            boolean[] taken) {
        int len = routes.length;
        ArrayList[] graph = new ArrayList[len];
        for (int i = 0; i < len; i++) {
            Arrays.sort(routes[i]);
            graph[i] = new ArrayList<Integer>();
            int id = Arrays.binarySearch(routes[i], source);
            if (id >= 0) {
                queue.offer(i);
                taken[i] = true;
            }
            id = Arrays.binarySearch(routes[i], target);
            if (id >= 0) {
                targetRoutes.add(i);
            }
        }
        for (int i = 0; i < len; i++) {
            for (int j = i + 1; j < len; j++) {
                if (commonStop(routes[i], routes[j])) {
                    graph[i].add(j);
                    graph[j].add(i);
                }
            }
        }
        return graph;
    }

    private boolean commonStop(int[] routeA, int[] routeB) {
        int idA = 0;
        int idB = 0;
        while (idA < routeA.length && idB < routeB.length) {
            if (routeA[idA] == routeB[idB]) {
                return true;
            } else if (routeA[idA] < routeB[idB]) {
                idA++;
            } else {
                idB++;
            }
        }
        return false;
    }
}
```