[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3342\. Find Minimum Time to Reach Last Room II

Medium

There is a dungeon with `n x m` rooms arranged as a grid.

You are given a 2D array `moveTime` of size `n x m`, where `moveTime[i][j]` represents the **minimum** time in seconds when you can **start moving** to that room. You start from the room `(0, 0)` at time `t = 0` and can move to an **adjacent** room. Moving between **adjacent** rooms takes one second for one move and two seconds for the next, **alternating** between the two.

Return the **minimum** time to reach the room `(n - 1, m - 1)`.

Two rooms are **adjacent** if they share a common wall, either _horizontally_ or _vertically_.

**Example 1:**

**Input:** moveTime = \[\[0,4],[4,4]]

**Output:** 7

**Explanation:**

The minimum time required is 7 seconds.

*   At time `t == 4`, move from room `(0, 0)` to room `(1, 0)` in one second.
*   At time `t == 5`, move from room `(1, 0)` to room `(1, 1)` in two seconds.

**Example 2:**

**Input:** moveTime = \[\[0,0,0,0],[0,0,0,0]]

**Output:** 6

**Explanation:**

The minimum time required is 6 seconds.

*   At time `t == 0`, move from room `(0, 0)` to room `(1, 0)` in one second.
*   At time `t == 1`, move from room `(1, 0)` to room `(1, 1)` in two seconds.
*   At time `t == 3`, move from room `(1, 1)` to room `(1, 2)` in one second.
*   At time `t == 4`, move from room `(1, 2)` to room `(1, 3)` in two seconds.

**Example 3:**

**Input:** moveTime = \[\[0,1],[1,2]]

**Output:** 4

**Constraints:**

*   `2 <= n == moveTime.length <= 750`
*   `2 <= m == moveTime[i].length <= 750`
*   <code>0 <= moveTime[i][j] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.PriorityQueue;

public class Solution {
    private static class Node {
        int x;
        int y;
        int t;
        int turn;
    }

    private final int[][] dir = { {1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public int minTimeToReach(int[][] moveTime) {
        PriorityQueue<Node> pq = new PriorityQueue<>((a, b) -> a.t - b.t);
        int m = moveTime.length;
        int n = moveTime[0].length;
        Node node = new Node();
        node.x = 0;
        node.y = 0;
        int t = 0;
        node.t = t;
        node.turn = 0;
        pq.add(node);
        moveTime[0][0] = -1;
        while (!pq.isEmpty()) {
            Node curr = pq.poll();
            for (int i = 0; i < 4; i++) {
                int x = curr.x + dir[i][0];
                int y = curr.y + dir[i][1];
                if (x == m - 1 && y == n - 1) {
                    t = Math.max(curr.t, moveTime[x][y]) + 1 + curr.turn;
                    return t;
                }
                if (x >= 0 && x < m && y < n && y >= 0 && moveTime[x][y] != -1) {
                    Node newNode = new Node();
                    t = Math.max(curr.t, moveTime[x][y]) + 1 + curr.turn;
                    newNode.x = x;
                    newNode.y = y;
                    newNode.t = t;
                    newNode.turn = curr.turn == 1 ? 0 : 1;
                    pq.add(newNode);
                    moveTime[x][y] = -1;
                }
            }
        }
        return -1;
    }
}
```