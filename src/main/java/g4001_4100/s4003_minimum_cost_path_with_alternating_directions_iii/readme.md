[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4003\. Minimum Cost Path with Alternating Directions III

Hard

You are given two integers `m` and `n` representing the number of rows and columns of a grid. Your goal is to reach cell `(m - 1, n - 1)`. You are also given a 2D integer array `penalty`.

The cost to enter cell `(i, j)` is `(i + 1) * (j + 1)`.

You begin at cell `(0, 0)` and initially pay its entrance cost. Actions performed after entering `(0, 0)` are numbered starting from 1.

On each action, you may move to an **adjacent** cell or wait in the current cell. A move follows the parity rule if:

*   On an **odd-numbered** action, you move **right** or **down**.
*   On an **even-numbered** action, you move **left** or **up**.

The cost of an action is determined as follows:

*   If you move according to the parity rule, pay only the entrance cost of the destination cell.
*   If you move in a direction that **violates** the parity rule, pay the entrance cost of the destination cell plus `penalty[i][j]`, where `(i, j)` is the cell you move from.
*   If you **wait** in cell `(i, j)`, pay `penalty[i][j]`.

After every move or wait, the action number increases by 1. Therefore, the required parity alternates after every action, regardless of whether a penalty was paid.

Return the **minimum** total cost required to reach `(m - 1, n - 1)`.

OriginalJava**Example 1:**

**Input:** m = 2, n = 2, penalty = \[\[5,3],[1,4]]

**Output:** 8

**Explanation:**

The optimal path is:

*   Start at cell `(0, 0)` with entry cost `(0 + 1) * (0 + 1) = 1`.
*   **Move 1**: Move down to cell `(1, 0)` with entry cost `(1 + 1) * (0 + 1) = 2`.
*   **Move 2**: Move right to cell `(1, 1)` with entry cost `(1 + 1) * (1 + 1) = 4` and an extra cost of `penalty[1][0] = 1` for violating the even parity rule.

Thus, the total cost is `1 + 2 + 4 + 1 = 8`.

**Example 2:**

**Input:** m = 2, n = 2, penalty = \[\[0,7],[3,2]]

**Output:** 7

**Explanation:**

The optimal path is:

*   Start at cell `(0, 0)` with entry cost `(0 + 1) * (0 + 1) = 1`.
*   **Move 1**: Wait at cell `(0, 0)` with an extra cost of `penalty[0][0] = 0` to flip to even parity.
*   **Move 2**: Move right to cell `(0, 1)` with entry cost `(0 + 1) * (1 + 1) = 2` and an extra cost of `penalty[0][0] = 0` for violating the even parity rule.
*   **Move 3**: Move down to cell `(1, 1)` with entry cost `(1 + 1) * (1 + 1) = 4`.

Thus, the total cost is `1 + 0 + 2 + 0 + 4 = 7`.

**Example 3:**

**Input:** m = 2, n = 3, penalty = \[\[8,0,9],[7,4,1]]

**Output:** 12

**Explanation:**

The optimal path is:

*   Start at cell `(0, 0)` with entry cost `(0 + 1) * (0 + 1) = 1`.
*   **Move 1**: Move right to cell `(0, 1)` with entry cost `(0 + 1) * (1 + 1) = 2`.
*   **Move 2**: Move right to cell `(0, 2)` with entry cost `(0 + 1) * (2 + 1) = 3` and an extra cost of `penalty[0][1] = 0` for violating the even parity rule.
*   **Move 3**: Move down to cell `(1, 2)` with entry cost `(1 + 1) * (2 + 1) = 6`.

Thus, the total cost is `1 + 2 + 3 + 0 + 6 = 12`.

**Constraints:**

*   <code>1 <= m, n <= 10<sup>5</sup></code>
*   <code>2 <= m * n <= 10<sup>5</sup></code>
*   `penalty.length == m`
*   `penalty[i].length == n`
*   <code>0 <= penalty[i][j] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;

public class Solution {
    public long minCost(int m, int n, int[][] penalty) {
        long inf = Long.MAX_VALUE / 4;
        long[] dist = new long[m * n * 2];
        Arrays.fill(dist, inf);
        PriorityQueue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[0]));
        dist[0] = 1;
        pq.add(new long[] {1, 0});
        int[] dx = {1, 0, -1, 0};
        int[] dy = {0, 1, 0, -1};
        while (!pq.isEmpty()) {
            long[] top = pq.poll();
            int state = (int) top[1];
            if (top[0] > dist[state]) {
                continue;
            }
            int parity = state & 1;
            int cell = state >> 1;
            int i = cell / n;
            int j = cell % n;
            if (i == m - 1 && j == n - 1) {
                return top[0];
            }
            int np = parity ^ 1;
            // wait
            long wait = top[0] + penalty[i][j];
            int ws = (cell << 1) | np;
            if (wait < dist[ws]) {
                dist[ws] = wait;
                pq.add(new long[] {wait, ws});
            }
            for (int d = 0; d < 4; d++) {
                int x = i + dx[d];
                int y = j + dy[d];
                if (x < 0 || y < 0 || x >= m || y >= n) {
                    continue;
                }
                // right/down
                boolean forward = d < 2;
                // parity 0: next action is odd-numbered
                boolean follows = (parity == 0) == forward;
                long c = top[0] + (long) (x + 1) * (y + 1) + (follows ? 0 : penalty[i][j]);
                int ns = ((x * n + y) << 1) | np;
                if (c < dist[ns]) {
                    dist[ns] = c;
                    pq.add(new long[] {c, ns});
                }
            }
        }
        return -1;
    }
}
```