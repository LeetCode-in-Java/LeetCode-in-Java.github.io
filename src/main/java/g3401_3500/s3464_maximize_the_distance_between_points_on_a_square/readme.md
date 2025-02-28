[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3464\. Maximize the Distance Between Points on a Square

Hard

You are given an integer `side`, representing the edge length of a square with corners at `(0, 0)`, `(0, side)`, `(side, 0)`, and `(side, side)` on a Cartesian plane.

You are also given a **positive** integer `k` and a 2D integer array `points`, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the coordinate of a point lying on the **boundary** of the square.

You need to select `k` elements among `points` such that the **minimum** Manhattan distance between any two points is **maximized**.

Return the **maximum** possible **minimum** Manhattan distance between the selected `k` points.

The Manhattan Distance between two cells <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and <code>(x<sub>j</sub>, y<sub>j</sub>)</code> is <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.

**Example 1:**

**Input:** side = 2, points = \[\[0,2],[2,0],[2,2],[0,0]], k = 4

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/28/4080_example0_revised.png)

Select all four points.

**Example 2:**

**Input:** side = 2, points = \[\[0,0],[1,2],[2,0],[2,2],[2,1]], k = 4

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/28/4080_example1_revised.png)

Select the points `(0, 0)`, `(2, 0)`, `(2, 2)`, and `(2, 1)`.

**Example 3:**

**Input:** side = 2, points = \[\[0,0],[0,1],[0,2],[1,2],[2,0],[2,2],[2,1]], k = 5

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/28/4080_example2_revised.png)

Select the points `(0, 0)`, `(0, 1)`, `(0, 2)`, `(1, 2)`, and `(2, 2)`.

**Constraints:**

*   <code>1 <= side <= 10<sup>9</sup></code>
*   <code>4 <= points.length <= min(4 * side, 15 * 10<sup>3</sup>)</code>
*   `points[i] == [xi, yi]`
*   The input is generated such that:
    *   `points[i]` lies on the boundary of the square.
    *   All `points[i]` are **unique**.
*   `4 <= k <= min(25, points.length)`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxDistance(int side, int[][] points, int k) {
        int n = points.length;
        long[] p = new long[n];
        for (int i = 0; i < n; i++) {
            int x = points[i][0];
            int y = points[i][1];
            long c;
            if (y == 0) {
                c = x;
            } else if (x == side) {
                c = side + (long) y;
            } else if (y == side) {
                c = 2L * side + (side - x);
            } else {
                c = 3L * side + (side - y);
            }
            p[i] = c;
        }
        Arrays.sort(p);
        long c = 4L * side;
        int tot = 2 * n;
        long[] dArr = new long[tot];
        for (int i = 0; i < n; i++) {
            dArr[i] = p[i];
            dArr[i + n] = p[i] + c;
        }
        int lo = 0;
        int hi = 2 * side;
        int ans = 0;
        while (lo <= hi) {
            int mid = (lo + hi) >>> 1;
            if (check(mid, dArr, n, k, c)) {
                ans = mid;
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
        return ans;
    }

    private boolean check(int d, long[] dArr, int n, int k, long c) {
        int len = dArr.length;
        int[] nxt = new int[len];
        int j = 0;
        for (int i = 0; i < len; i++) {
            if (j < i + 1) {
                j = i + 1;
            }
            while (j < len && dArr[j] < dArr[i] + d) {
                j++;
            }
            nxt[i] = (j < len) ? j : -1;
        }
        for (int i = 0; i < n; i++) {
            int cnt = 1;
            int cur = i;
            while (cnt < k) {
                int nx = nxt[cur];
                if (nx == -1 || nx >= i + n) {
                    break;
                }
                cur = nx;
                cnt++;
            }
            if (cnt == k && (dArr[i] + c - dArr[cur]) >= d) {
                return true;
            }
        }
        return false;
    }
}
```