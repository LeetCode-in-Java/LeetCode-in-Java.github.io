[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 963\. Minimum Area Rectangle II

Medium

You are given an array of points in the **X-Y** plane `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.

Return _the minimum area of any rectangle formed from these points, with sides **not necessarily parallel** to the X and Y axes_. If there is not any such rectangle, return `0`.

Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/21/1a.png)

**Input:** points = \[\[1,2],[2,1],[1,0],[0,1]]

**Output:** 2.00000

**Explanation:** The minimum area rectangle occurs at [1,2],[2,1],[1,0],[0,1], with an area of 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/22/2.png)

**Input:** points = \[\[0,1],[2,1],[1,1],[1,0],[2,0]]

**Output:** 1.00000

**Explanation:** The minimum area rectangle occurs at [1,0],[1,1],[2,1],[2,0], with an area of 1.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/22/3.png)

**Input:** points = \[\[0,3],[1,2],[3,1],[1,3],[2,1]]

**Output:** 0

**Explanation:** There is no possible rectangle to form from these points.

**Constraints:**

*   `1 <= points.length <= 50`
*   `points[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 4 * 10<sup>4</sup></code>
*   All the given points are **unique**.

## Solution

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

@SuppressWarnings("java:S135")
public class Solution {
    public double minAreaFreeRect(int[][] points) {
        Map<Integer, Set<Integer>> map = new HashMap<>();
        double area;
        for (int[] point : points) {
            map.putIfAbsent(point[0], new HashSet<>());
            map.get(point[0]).add(point[1]);
        }
        double minArea = Double.MAX_VALUE;
        int n = points.length;
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                int dx1 = points[j][0] - points[i][0];
                int dy1 = points[j][1] - points[i][1];
                // get the 3rd point
                for (int k = j + 1; k < n; k++) {
                    int dx2 = points[k][0] - points[i][0];
                    int dy2 = points[k][1] - points[i][1];
                    if (dx1 * dx2 + dy1 * dy2 != 0) {
                        continue;
                    }
                    // find the 4th point
                    int x = dx1 + points[k][0];
                    int y = dy1 + points[k][1];
                    area = calculateArea(points, i, j, k);
                    if (area >= minArea) {
                        continue;
                    }
                    // 4th point exists
                    if (map.get(x) != null && map.get(x).contains(y)) {
                        minArea = area;
                    }
                }
            }
        }
        return minArea == Double.MAX_VALUE ? 0.0 : minArea;
    }

    private double calculateArea(int[][] points, int i, int j, int k) {
        int[] first = points[i];
        int[] second = points[j];
        int[] third = points[k];
        return Math.abs(
                first[0] * (second[1] - third[1])
                        + second[0] * (third[1] - first[1])
                        + third[0] * (first[1] - second[1]));
    }
}
```