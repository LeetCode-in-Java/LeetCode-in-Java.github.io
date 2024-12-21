[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3380\. Maximum Area Rectangle With Point Constraints I

Medium

You are given an array `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the coordinates of a point on an infinite plane.

Your task is to find the **maximum** area of a rectangle that:

*   Can be formed using **four** of these points as its corners.
*   Does **not** contain any other point inside or on its border.
*   Has its edges **parallel** to the axes.

Return the **maximum area** that you can obtain or -1 if no such rectangle is possible.

**Example 1:**

**Input:** points = \[\[1,1],[1,3],[3,1],[3,3]]

**Output:** 4

**Explanation:**

**![Example 1 diagram](https://assets.leetcode.com/uploads/2024/11/02/example1.png)**

We can make a rectangle with these 4 points as corners and there is no other point that lies inside or on the border. Hence, the maximum possible area would be 4.

**Example 2:**

**Input:** points = \[\[1,1],[1,3],[3,1],[3,3],[2,2]]

**Output:** \-1

**Explanation:**

**![Example 2 diagram](https://assets.leetcode.com/uploads/2024/11/02/example2.png)**

There is only one rectangle possible is with points `[1,1], [1,3], [3,1]` and `[3,3]` but `[2,2]` will always lie inside it. Hence, returning -1.

**Example 3:**

**Input:** points = \[\[1,1],[1,3],[3,1],[3,3],[1,2],[3,2]]

**Output:** 2

**Explanation:**

**![Example 3 diagram](https://assets.leetcode.com/uploads/2024/11/02/example3.png)**

The maximum area rectangle is formed by the points `[1,3], [1,2], [3,2], [3,3]`, which has an area of 2. Additionally, the points `[1,1], [1,2], [3,1], [3,2]` also form a valid rectangle with the same area.

**Constraints:**

*   `1 <= points.length <= 10`
*   `points[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 100</code>
*   All the given points are **unique**.

## Solution

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int maxRectangleArea(int[][] points) {
        Set<String> set = new HashSet<>();
        for (int[] p : points) {
            set.add(Arrays.toString(p));
        }
        int maxArea = -1;
        for (int[] point : points) {
            for (int j = 1; j < points.length; j++) {
                int[] p2 = points[j];
                if (point[0] == p2[0]
                        || point[1] == p2[1]
                        || !set.contains(Arrays.toString(new int[] {point[0], p2[1]}))
                        || !set.contains(Arrays.toString(new int[] {p2[0], point[1]}))
                        || !validate(points, point, p2)) {
                    continue;
                }
                maxArea = Math.max(maxArea, (p2[1] - point[1]) * (p2[0] - point[0]));
            }
        }
        return maxArea;
    }

    private boolean validate(int[][] points, int[] p1, int[] p2) {
        int top = Math.max(p1[1], p2[1]);
        int bot = Math.min(p1[1], p2[1]);
        int left = Math.min(p1[0], p2[0]);
        int right = Math.max(p1[0], p2[0]);
        for (int[] p : points) {
            int x = p[0];
            int y = p[1];
            if ((y == top || y == bot) && x > left && x < right
                    || (x == left || x == right) && y > bot && y < top
                    || (x > left && x < right && y > bot && y < top)) {
                return false;
            }
        }
        return true;
    }
}
```