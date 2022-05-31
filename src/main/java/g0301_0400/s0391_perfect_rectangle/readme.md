## 391\. Perfect Rectangle

Hard

Given an array `rectangles` where <code>rectangles[i] = [x<sub>i</sub>, y<sub>i</sub>, a<sub>i</sub>, b<sub>i</sub>]</code> represents an axis-aligned rectangle. The bottom-left point of the rectangle is <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and the top-right point of it is <code>(a<sub>i</sub>, b<sub>i</sub>)</code>.

Return `true` _if all the rectangles together form an exact cover of a rectangular region_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/27/perectrec1-plane.jpg)

**Input:** rectangles = \[\[1,1,3,3],[3,1,4,2],[3,2,4,4],[1,3,2,4],[2,3,3,4]]

**Output:** true

**Explanation:** All 5 rectangles together form an exact cover of a rectangular region.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/27/perfectrec2-plane.jpg)

**Input:** rectangles = \[\[1,1,2,3],[1,3,2,4],[3,1,4,2],[3,2,4,4]]

**Output:** false

**Explanation:** Because there is a gap between the two rectangular regions.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/27/perfectrec3-plane.jpg)

**Input:** rectangles = \[\[1,1,3,3],[3,1,4,2],[1,3,2,4],[3,2,4,4]]

**Output:** false

**Explanation:** Because there is a gap in the top center.

**Example 4:**

![](https://assets.leetcode.com/uploads/2021/03/27/perfecrrec4-plane.jpg)

**Input:** rectangles = \[\[1,1,3,3],[3,1,4,2],[1,3,2,4],[2,2,4,4]]

**Output:** false

**Explanation:** Because two of the rectangles overlap with each other.

**Constraints:**

*   <code>1 <= rectangles.length <= 2 * 10<sup>4</sup></code>
*   `rectangles[i].length == 4`
*   <code>-10<sup>5</sup> <= x<sub>i</sub>, y<sub>i</sub>, a<sub>i</sub>, b<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```java
import java.util.HashSet;
import java.util.Objects;
import java.util.Set;

public class Solution {
    public boolean isRectangleCover(int[][] rectangles) {
        Set<Point> container = new HashSet<>();
        // add each rectangle area to totalArea
        int totalArea = 0;
        // A rectangle has four points, if a point appears twice, it will be deleted it from the set
        for (int[] rectangle : rectangles) {
            totalArea += (rectangle[2] - rectangle[0]) * (rectangle[3] - rectangle[1]);
            Point p1 = new Point(rectangle[0], rectangle[1]);
            Point p2 = new Point(rectangle[2], rectangle[1]);
            Point p3 = new Point(rectangle[2], rectangle[3]);
            Point p4 = new Point(rectangle[0], rectangle[3]);
            if (container.contains(p1)) {
                container.remove(p1);
            } else {
                container.add(p1);
            }
            if (container.contains(p2)) {
                container.remove(p2);
            } else {
                container.add(p2);
            }
            if (container.contains(p3)) {
                container.remove(p3);
            } else {
                container.add(p3);
            }
            if (container.contains(p4)) {
                container.remove(p4);
            } else {
                container.add(p4);
            }
        }
        // A perfect rectangle must has four points
        if (container.size() != 4) {
            return false;
        }

        // these four points represent the last perfect rectangle, check this rectangle area to the
        // totalArea
        int minX = Integer.MAX_VALUE;
        int maxX = Integer.MIN_VALUE;
        int minY = Integer.MAX_VALUE;
        int maxY = Integer.MIN_VALUE;
        for (Point p : container) {
            minX = Math.min(minX, p.x);
            maxX = Math.max(maxX, p.x);
            minY = Math.min(minY, p.y);
            maxY = Math.max(maxY, p.y);
        }
        return totalArea == (maxX - minX) * (maxY - minY);
    }

    private static class Point {
        private final int x;
        private final int y;

        public Point(int x, int y) {
            this.x = x;
            this.y = y;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) {
                return true;
            }
            if (o == null || getClass() != o.getClass()) {
                return false;
            }
            Point point = (Point) o;
            return x == point.x && y == point.y;
        }

        @Override
        public int hashCode() {
            return Objects.hash(x, y);
        }
    }
}
```