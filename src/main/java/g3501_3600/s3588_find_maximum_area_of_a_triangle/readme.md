[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3588\. Find Maximum Area of a Triangle

Medium

You are given a 2D array `coords` of size `n x 2`, representing the coordinates of `n` points in an infinite Cartesian plane.

Find **twice** the **maximum** area of a triangle with its corners at _any_ three elements from `coords`, such that at least one side of this triangle is **parallel** to the x-axis or y-axis. Formally, if the maximum area of such a triangle is `A`, return `2 * A`.

If no such triangle exists, return -1.

**Note** that a triangle _cannot_ have zero area.

**Example 1:**

**Input:** coords = \[\[1,1],[1,2],[3,2],[3,3]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/19/image-20250420010047-1.png)

The triangle shown in the image has a base 1 and height 2. Hence its area is `1/2 * base * height = 1`.

**Example 2:**

**Input:** coords = \[\[1,1],[2,2],[3,3]]

**Output:** \-1

**Explanation:**

The only possible triangle has corners `(1, 1)`, `(2, 2)`, and `(3, 3)`. None of its sides are parallel to the x-axis or the y-axis.

**Constraints:**

*   <code>1 <= n == coords.length <= 10<sup>5</sup></code>
*   <code>1 <= coords[i][0], coords[i][1] <= 10<sup>6</sup></code>
*   All `coords[i]` are **unique**.

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.TreeSet;

public class Solution {
    public long maxArea(int[][] coords) {
        Map<Integer, TreeSet<Integer>> xMap = new HashMap<>();
        Map<Integer, TreeSet<Integer>> yMap = new HashMap<>();
        TreeSet<Integer> allX = new TreeSet<>();
        TreeSet<Integer> allY = new TreeSet<>();
        for (int[] coord : coords) {
            int x = coord[0];
            int y = coord[1];
            xMap.computeIfAbsent(x, k -> new TreeSet<>()).add(y);
            yMap.computeIfAbsent(y, k -> new TreeSet<>()).add(x);
            allX.add(x);
            allY.add(y);
        }
        long ans = Long.MIN_VALUE;
        for (Map.Entry<Integer, TreeSet<Integer>> entry : xMap.entrySet()) {
            int x = entry.getKey();
            TreeSet<Integer> ySet = entry.getValue();
            if (ySet.size() < 2) {
                continue;
            }
            int minY = ySet.first();
            int maxY = ySet.last();
            int base = maxY - minY;
            int minX = allX.first();
            int maxX = allX.last();
            if (minX != x) {
                ans = Math.max(ans, (long) Math.abs(x - minX) * base);
            }
            if (maxX != x) {
                ans = Math.max(ans, (long) Math.abs(x - maxX) * base);
            }
        }

        for (Map.Entry<Integer, TreeSet<Integer>> entry : yMap.entrySet()) {
            int y = entry.getKey();
            TreeSet<Integer> xSet = entry.getValue();
            if (xSet.size() < 2) {
                continue;
            }
            int minX = xSet.first();
            int maxX = xSet.last();
            int base = maxX - minX;
            int minY = allY.first();
            int maxY = allY.last();
            if (minY != y) {
                ans = Math.max(ans, (long) Math.abs(y - minY) * base);
            }
            if (maxY != y) {
                ans = Math.max(ans, (long) Math.abs(y - maxY) * base);
            }
        }
        return ans == Long.MIN_VALUE ? -1 : ans;
    }
}
```