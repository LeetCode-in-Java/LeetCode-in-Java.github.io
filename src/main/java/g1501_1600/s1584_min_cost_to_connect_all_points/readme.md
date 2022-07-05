[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1584\. Min Cost to Connect All Points

Medium

You are given an array `points` representing integer coordinates of some points on a 2D-plane, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.

The cost of connecting two points <code>[x<sub>i</sub>, y<sub>i</sub>]</code> and <code>[x<sub>j</sub>, y<sub>j</sub>]</code> is the **manhattan distance** between them: <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>, where `|val|` denotes the absolute value of `val`.

Return _the minimum cost to make all points connected._ All points are connected if there is **exactly one** simple path between any two points.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/26/d.png)

**Input:** points = \[\[0,0],[2,2],[3,10],[5,2],[7,0]]

**Output:** 20

**Explanation:** ![](https://assets.leetcode.com/uploads/2020/08/26/c.png)

We can connect the points as shown above to get the minimum cost of 20.

Notice that there is a unique path between every pair of points.

**Example 2:**

**Input:** points = \[\[3,12],[-2,5],[-4,1]]

**Output:** 18

**Constraints:**

*   `1 <= points.length <= 1000`
*   <code>-10<sup>6</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>6</sup></code>
*   All pairs <code>(x<sub>i</sub>, y<sub>i</sub>)</code> are distinct.

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;

public class Solution {
    public int minCostConnectPoints(int[][] points) {
        int v = points.length;
        if (v == 2) {
            return getDistance(points[0], points[1]);
        }
        PriorityQueue<Pair> pq = new PriorityQueue<>(v, new Pair());
        boolean[] mst = new boolean[v];
        int[] dist = new int[v];
        int[] parent = new int[v];
        Arrays.fill(dist, 1000000);
        Arrays.fill(parent, -1);
        dist[0] = 0;
        parent[0] = 0;
        for (int i = 0; i < v; i++) {
            pq.add(new Pair(dist[i], i));
        }
        constructMST(parent, points, mst, pq, dist);
        int cost = 0;
        for (int i = 1; i < parent.length; i++) {
            cost += getDistance(points[parent[i]], points[i]);
        }
        return cost;
    }

    public void constructMST(
            int[] parent, int[][] points, boolean[] mst, PriorityQueue<Pair> pq, int[] dist) {
        if (!containsFalse(mst)) {
            return;
        }
        Pair newPair = pq.poll();
        int pointIndex = newPair.getV();
        mst[pointIndex] = true;
        for (int i = 0; i < parent.length; i++) {
            int d = getDistance(points[pointIndex], points[i]);
            if (!mst[i] && d < dist[i]) {
                dist[i] = d;
                pq.add(new Pair(dist[i], i));
                parent[i] = pointIndex;
            }
        }
        constructMST(parent, points, mst, pq, dist);
    }

    public boolean containsFalse(boolean[] mst) {
        for (boolean b : mst) {
            if (!b) {
                return true;
            }
        }
        return false;
    }

    public int getDistance(int[] p1, int[] p2) {
        return Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
    }

    private static class Pair implements Comparator<Pair> {
        int dis;
        int v;

        public Pair() {}

        public Pair(int dis, int v) {
            this.dis = dis;
            this.v = v;
        }

        public int getDis() {
            return dis;
        }

        public int getV() {
            return v;
        }

        @Override
        public int compare(Pair p1, Pair p2) {
            return p1.dis - p2.dis;
        }
    }
}
```