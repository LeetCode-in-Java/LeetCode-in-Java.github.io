## 1515\. Best Position for a Service Centre

Hard

A delivery company wants to build a new service center in a new city. The company knows the positions of all the customers in this city on a 2D-Map and wants to build the new center in a position such that **the sum of the euclidean distances to all customers is minimum**.

Given an array `positions` where <code>positions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> is the position of the `ith` customer on the map, return _the minimum sum of the euclidean distances_ to all customers.

In other words, you need to choose the position of the service center <code>[x<sub>centre</sub>, y<sub>centre</sub>]</code> such that the following formula is minimized:

![](https://assets.leetcode.com/uploads/2020/06/25/q4_edited.jpg)

Answers within <code>10<sup>-5</sup></code> of the actual value will be accepted.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/06/25/q4_e1.jpg)

**Input:** positions = \[\[0,1],[1,0],[1,2],[2,1]]

**Output:** 4.00000

**Explanation:** As shown, you can see that choosing [x<sub>centre</sub>, y<sub>centre</sub>] = [1, 1] will make the distance to each customer = 1, the sum of all distances is 4 which is the minimum possible we can achieve.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/06/25/q4_e3.jpg)

**Input:** positions = \[\[1,1],[3,3]]

**Output:** 2.82843

**Explanation:** The minimum possible sum of distances = sqrt(2) + sqrt(2) = 2.82843

**Constraints:**

*   `1 <= positions.length <= 50`
*   `positions[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 100</code>

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public double getMinDistSum(int[][] positions) {
        double minX = Integer.MAX_VALUE;
        double minY = Integer.MAX_VALUE;
        double maxX = Integer.MIN_VALUE;
        double maxY = Integer.MIN_VALUE;
        for (int[] pos : positions) {
            maxX = Math.max(maxX, pos[0]);
            maxY = Math.max(maxY, pos[1]);
            minX = Math.min(minX, pos[0]);
            minY = Math.min(minY, pos[1]);
        }
        double xMid = minX + (maxX - minX) / 2;
        double yMid = minY + (maxY - minY) / 2;

        double jump = Math.max(maxX - minX, maxY - minY);

        double ans = getTotalDistance(xMid, yMid, positions);

        while (jump > 0.00001) {
            List<double[]> list = getFourCorners(xMid, yMid, jump);
            boolean found = false;
            for (double[] point : list) {
                double pointAns = getTotalDistance(point[0], point[1], positions);
                if (ans > pointAns) {
                    xMid = point[0];
                    yMid = point[1];
                    ans = pointAns;
                    found = true;
                }
            }

            if (!found) {
                jump = jump / 2;
            }
        }

        return ans;
    }

    private List<double[]> getFourCorners(double xMid, double yMid, double jump) {
        List<double[]> list = new ArrayList<>();
        list.add(new double[] {xMid - jump, yMid + jump});
        list.add(new double[] {xMid + jump, yMid + jump});
        list.add(new double[] {xMid - jump, yMid - jump});
        list.add(new double[] {xMid + jump, yMid - jump});

        return list;
    }

    private double getTotalDistance(double x, double y, int[][] positions) {
        double totalDistanceSum = 0;
        for (int[] point : positions) {
            double xDistance = x - point[0];
            double yDistance = y - point[1];
            totalDistanceSum += Math.sqrt(xDistance * xDistance + yDistance * yDistance);
        }

        return totalDistanceSum;
    }
}
```