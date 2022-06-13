## 2249\. Count Lattice Points Inside a Circle

Medium

Given a 2D integer array `circles` where <code>circles[i] = [x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub>]</code> represents the center <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and radius <code>r<sub>i</sub></code> of the <code>i<sup>th</sup></code> circle drawn on a grid, return _the **number of lattice points**_ _that are present inside **at least one** circle_.

**Note:**

*   A **lattice point** is a point with integer coordinates.
*   Points that lie **on the circumference of a circle** are also considered to be inside it.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/02/exa-11.png)

**Input:** circles = \[\[2,2,1]]

**Output:** 5

**Explanation:** 

The figure above shows the given circle. 

The lattice points present inside the circle are (1, 2), (2, 1), (2, 2), (2, 3), and (3, 2) and are shown in green. 

Other points such as (1, 1) and (1, 3), which are shown in red, are not considered inside the circle. 

Hence, the number of lattice points present inside at least one circle is 5.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/02/exa-22.png)

**Input:** circles = \[\[2,2,2],[3,4,1]]

**Output:** 16

**Explanation:** 

The figure above shows the given circles. 

There are exactly 16 lattice points which are present inside at least one circle. 

Some of them are (0, 2), (2, 0), (2, 4), (3, 2), and (4, 4).

**Constraints:**

*   `1 <= circles.length <= 200`
*   `circles[i].length == 3`
*   <code>1 <= x<sub>i</sub>, y<sub>i</sub> <= 100</code>
*   <code>1 <= r<sub>i</sub> <= min(x<sub>i</sub>, y<sub>i</sub>)</code>

## Solution

```java
public class Solution {
    public int countLatticePoints(int[][] circles) {
        int xMin = 200;
        int xMax = -1;
        int yMin = 200;
        int yMax = -1;
        for (int[] c : circles) {
            xMin = Math.min(xMin, c[0] - c[2]);
            xMax = Math.max(xMax, c[0] + c[2]);
            yMin = Math.min(yMin, c[1] - c[2]);
            yMax = Math.max(yMax, c[1] + c[2]);
        }
        int ans = 0;
        for (int x = xMin; x <= xMax; x++) {
            for (int y = yMin; y <= yMax; y++) {
                for (int[] c : circles) {
                    if ((c[0] - x) * (c[0] - x) + (c[1] - y) * (c[1] - y) <= c[2] * c[2]) {
                        ans++;
                        break;
                    }
                }
            }
        }
        return ans;
    }
}
```