[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 939\. Minimum Area Rectangle

Medium

You are given an array of points in the **X-Y** plane `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.

Return _the minimum area of a rectangle formed from these points, with sides parallel to the X and Y axes_. If there is not any such rectangle, return `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/03/rec1.JPG)

**Input:** points = \[\[1,1],[1,3],[3,1],[3,3],[2,2]]

**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/08/03/rec2.JPG)

**Input:** points = \[\[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]

**Output:** 2

**Constraints:**

*   `1 <= points.length <= 500`
*   `points[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 4 * 10<sup>4</sup></code>
*   All the given points are **unique**.

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

public class Solution {
    public int minAreaRect(int[][] points) {
        if (points.length < 4) {
            return 0;
        }
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for (int[] p : points) {
            map.putIfAbsent(p[0], new HashSet<>());
            map.get(p[0]).add(p[1]);
        }
        Arrays.sort(
                points,
                (a, b) ->
                        (a[0] == b[0]) ? Integer.compare(a[1], b[1]) : Integer.compare(a[0], b[0]));

        int min = Integer.MAX_VALUE;
        for (int i = 0; i < points.length - 2; i++) {
            for (int j = i + 1; j < points.length - 1; j++) {
                int[] p1 = points[i];
                int[] p2 = points[j];
                int area = Math.abs((p1[0] - p2[0]) * (p1[1] - p2[1]));
                if (area >= min || area == 0) {
                    continue;
                }
                if (map.get(p1[0]).contains(p2[1]) && map.get(p2[0]).contains(p1[1])) {
                    min = area;
                }
            }
        }
        return min == Integer.MAX_VALUE ? 0 : min;
    }
}
```