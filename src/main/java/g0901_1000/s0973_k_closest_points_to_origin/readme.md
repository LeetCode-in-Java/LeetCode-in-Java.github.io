## 973\. K Closest Points to Origin

Medium

Given an array of `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., <code>√(x<sub>1</sub> - x<sub>2</sub>)<sup>2</sup> + (y<sub>1</sub> - y<sub>2</sub>)<sup>2</sup></code>).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

**Input:** points = \[\[1,3],[-2,2]], k = 1

**Output:** [[-2,2]]

**Explanation:**

The distance between (1, 3) and the origin is sqrt(10).

The distance between (-2, 2) and the origin is sqrt(8).

Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.

We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].

**Example 2:**

**Input:** points = \[\[3,3],[5,-1],[-2,4]], k = 2

**Output:** [[3,3],[-2,4]]

**Explanation:** The answer [[-2,4],[3,3]] would also be accepted.

**Constraints:**

*   <code>1 <= k <= points.length <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> < x<sub>i</sub>, y<sub>i</sub> < 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int calDistSqr(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }

    private void swap(int[][] points, int i, int j) {
        int[] temp = points[i];
        points[i] = points[j];
        points[j] = temp;
    }

    private void quickSelect(int[][] points, int k, int left, int right) {
        if (left >= right) {
            return;
        }
        // choose pivot index, could be randomly
        int pIdx = (right - left) / 2 + left;
        int[] pivot = points[pIdx];
        int pivotDist = calDistSqr(pivot);
        // put pivot into the last position
        swap(points, pIdx, right);
        // i: for iterating the array; curPIdx: the proper position to put pivot later
        int i = left;
        int curPIdx = left;
        while (i < right) {
            if (calDistSqr(points[i]) <= pivotDist) {
                // put all the smaller item to the left side of the array
                swap(points, i, curPIdx);
                // move proper pivot position forward
                curPIdx++;
            }
            i++;
        }
        // put pivot back into the correct position
        swap(points, curPIdx, right);
        // for ele in arr[0:curPIdx] => ele <= arr[curPIdx]
        int num = curPIdx - left + 1;
        if (num == k) {
            return;
        }
        if (num < k) {
            quickSelect(points, k - num, curPIdx + 1, right);
        }
        if (num > k) {
            quickSelect(points, k, left, curPIdx - 1);
        }
    }

    public int[][] kClosest(int[][] points, int k) {
        quickSelect(points, k, 0, points.length - 1);
        return Arrays.copyOf(points, k);
    }
}
```