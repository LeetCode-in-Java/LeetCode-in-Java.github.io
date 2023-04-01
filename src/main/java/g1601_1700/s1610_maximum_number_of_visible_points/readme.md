[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1610\. Maximum Number of Visible Points

Hard

You are given an array `points`, an integer `angle`, and your `location`, where <code>location = [pos<sub>x</sub>, pos<sub>y</sub>]</code> and <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> both denote **integral coordinates** on the X-Y plane.

Initially, you are facing directly east from your position. You **cannot move** from your position, but you can **rotate**. In other words, <code>pos<sub>x</sub></code> and <code>pos<sub>y</sub></code> cannot be changed. Your field of view in **degrees** is represented by `angle`, determining how wide you can see from any given view direction. Let `d` be the amount in degrees that you rotate counterclockwise. Then, your field of view is the **inclusive** range of angles `[d - angle/2, d + angle/2]`.

Your browser does not support the video tag or this video format.

You can **see** some set of points if, for each point, the **angle** formed by the point, your position, and the immediate east direction from your position is **in your field of view**.

There can be multiple points at one coordinate. There may be points at your location, and you can always see these points regardless of your rotation. Points do not obstruct your vision to other points.

Return _the maximum number of points you can see_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/30/89a07e9b-00ab-4967-976a-c723b2aa8656.png)

**Input:** points = \[\[2,1],[2,2],[3,3]], angle = 90, location = [1,1]

**Output:** 3

**Explanation:** The shaded region represents your field of view. All points can be made visible in your field of view, including [3,3] even though [2,2] is in front and in the same line of sight.

**Example 2:**

**Input:** points = \[\[2,1],[2,2],[3,4],[1,1]], angle = 90, location = [1,1]

**Output:** 4

**Explanation:** All points can be made visible in your field of view, including the one at your location.

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/09/30/5010bfd3-86e6-465f-ac64-e9df941d2e49.png)

**Input:** points = \[\[1,0],[2,1]], angle = 13, location = [1,1]

**Output:** 1

**Explanation:** You can only see one of the two points, as shown above.

**Constraints:**

*   <code>1 <= points.length <= 10<sup>5</sup></code>
*   `points[i].length == 2`
*   `location.length == 2`
*   `0 <= angle < 360`
*   <code>0 <= pos<sub>x</sub>, pos<sub>y</sub>, x<sub>i</sub>, y<sub>i</sub> <= 100</code>

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    public int visiblePoints(List<List<Integer>> points, int angle, List<Integer> location) {
        int max = 0;
        int count = 0;
        List<Double> angles = new ArrayList<>(points.size());
        for (List<Integer> point : points) {
            double a = calculateAngle(location, point);
            if (a == 360.0) {
                count++;
            } else {
                angles.add(a);
            }
        }
        Collections.sort(angles);
        int s = 0;
        int e = 0;
        int size;
        int n = angles.size();
        while (s < n && max < n) {
            while (true) {
                int index = (e + 1) % n;
                if (s == index || (360 + angles.get(index) - angles.get(s)) % 360 > angle) {
                    break;
                }
                e = index;
            }
            size = e >= s ? (e - s + 1) : (n - s + e + 1);
            max = Math.max(max, size);
            if (e == s) {
                e++;
            }
            s++;
        }
        return max + count;
    }

    private double calculateAngle(List<Integer> location, List<Integer> point) {
        int x1 = location.get(0);
        int y1 = location.get(1);
        int x2 = point.get(0);
        int y2 = point.get(1);
        if (x1 == x2) {
            if (y2 > y1) {
                return 90.0;
            }
            if (y2 < y1) {
                return 270.0;
            }
            return 360.0;
        }
        double angle = Math.toDegrees(Math.atan((double) (y2 - y1) / (x2 - x1)));
        if (x2 > x1) {
            angle = (angle + 360.0) % 360.0;
        } else {
            angle += 180.0;
        }
        return angle;
    }
}
```