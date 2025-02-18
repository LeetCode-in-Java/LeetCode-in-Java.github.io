[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3454\. Separate Squares II

Hard

You are given a 2D integer array `squares`. Each <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code> represents the coordinates of the bottom-left point and the side length of a square parallel to the x-axis.

Find the **minimum** y-coordinate value of a horizontal line such that the total area covered by squares above the line _equals_ the total area covered by squares below the line.

Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Note**: Squares **may** overlap. Overlapping areas should be counted **only once** in this version.

**Example 1:**

**Input:** squares = \[\[0,0,1],[2,2,1]]

**Output:** 1.00000

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/15/4065example1drawio.png)

Any horizontal line between `y = 1` and `y = 2` results in an equal split, with 1 square unit above and 1 square unit below. The minimum y-value is 1.

**Example 2:**

**Input:** squares = \[\[0,0,2],[1,1,1]]

**Output:** 1.00000

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/15/4065example2drawio.png)

Since the blue square overlaps with the red square, it will not be counted again. Thus, the line `y = 1` splits the squares into two equal parts.

**Constraints:**

*   <code>1 <= squares.length <= 5 * 10<sup>4</sup></code>
*   <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code>
*   `squares[i].length == 3`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= l<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@SuppressWarnings("java:S1210")
public class Solution {
    private static class Event implements Comparable<Event> {
        double y;
        int x1;
        int x2;
        int type;

        Event(double y, int x1, int x2, int type) {
            this.y = y;
            this.x1 = x1;
            this.x2 = x2;
            this.type = type;
        }

        public int compareTo(Event other) {
            return Double.compare(this.y, other.y);
        }
    }

    private static class Segment {
        double y1;
        double y2;
        double unionX;
        double cumArea;

        Segment(double y1, double y2, double unionX, double cumArea) {
            this.y1 = y1;
            this.y2 = y2;
            this.unionX = unionX;
            this.cumArea = cumArea;
        }
    }

    private static class SegmentTree {
        int[] count;
        double[] len;
        int n;
        int[] x;

        SegmentTree(int[] x) {
            this.x = x;
            n = x.length - 1;
            count = new int[4 * n];
            len = new double[4 * n];
        }

        void update(int idx, int l, int r, int ql, int qr, int val) {
            if (qr < l || ql > r) {
                return;
            }
            if (ql <= l && r <= qr) {
                count[idx] += val;
            } else {
                int mid = (l + r) / 2;
                update(2 * idx + 1, l, mid, ql, qr, val);
                update(2 * idx + 2, mid + 1, r, ql, qr, val);
            }
            if (count[idx] > 0) {
                len[idx] = x[r + 1] - (double) x[l];
            } else {
                if (l == r) {
                    len[idx] = 0;
                } else {
                    len[idx] = len[2 * idx + 1] + len[2 * idx + 2];
                }
            }
        }

        void update(int ql, int qr, int val) {
            update(0, 0, n - 1, ql, qr, val);
        }

        double query() {
            return len[0];
        }
    }

    public double separateSquares(int[][] squares) {
        int n = squares.length;
        Event[] events = new Event[2 * n];
        int idx = 0;
        List<Integer> xList = new ArrayList<>();
        for (int[] s : squares) {
            int x = s[0];
            int y = s[1];
            int l = s[2];
            int x2 = x + l;
            int y2 = y + l;
            events[idx++] = new Event(y, x, x2, 1);
            events[idx++] = new Event(y2, x, x2, -1);
            xList.add(x);
            xList.add(x2);
        }
        Arrays.sort(events);
        int m = xList.size();
        int[] xCords = new int[m];
        for (int i = 0; i < m; i++) {
            xCords[i] = xList.get(i);
        }
        Arrays.sort(xCords);
        int uniqueCount = 0;
        for (int i = 0; i < m; i++) {
            if (i == 0 || xCords[i] != xCords[i - 1]) {
                xCords[uniqueCount++] = xCords[i];
            }
        }
        int[] x = Arrays.copyOf(xCords, uniqueCount);
        SegmentTree segTree = new SegmentTree(x);
        List<Segment> segments = new ArrayList<>();
        double cumArea = 0.0;
        double lastY = events[0].y;
        int iEvent = 0;
        while (iEvent < events.length) {
            double currentY = events[iEvent].y;
            double delta = currentY - lastY;
            if (delta > 0) {
                double unionX = segTree.query();
                segments.add(new Segment(lastY, currentY, unionX, cumArea));
                cumArea += unionX * delta;
            }
            while (iEvent < events.length && events[iEvent].y == currentY) {
                Event e = events[iEvent];
                int left = Arrays.binarySearch(x, e.x1);
                int right = Arrays.binarySearch(x, e.x2);
                if (left < right) {
                    segTree.update(left, right - 1, e.type);
                }
                iEvent++;
            }
            lastY = currentY;
        }
        double totalArea = cumArea;
        double target = totalArea / 2.0;
        double answer;
        for (Segment seg : segments) {
            double segArea = seg.unionX * (seg.y2 - seg.y1);
            if (seg.cumArea + segArea >= target) {
                double needed = target - seg.cumArea;
                answer = seg.y1 + needed / seg.unionX;
                return answer;
            }
        }
        return lastY;
    }
}
```