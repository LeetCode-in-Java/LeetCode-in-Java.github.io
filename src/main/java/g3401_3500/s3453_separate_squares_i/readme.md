[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3453\. Separate Squares I

Medium

You are given a 2D integer array `squares`. Each <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code> represents the coordinates of the bottom-left point and the side length of a square parallel to the x-axis.

Find the **minimum** y-coordinate value of a horizontal line such that the total area of the squares above the line _equals_ the total area of the squares below the line.

Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Note**: Squares **may** overlap. Overlapping areas should be counted **multiple times**.

**Example 1:**

**Input:** squares = \[\[0,0,1],[2,2,1]]

**Output:** 1.00000

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/06/4062example1drawio.png)

Any horizontal line between `y = 1` and `y = 2` will have 1 square unit above it and 1 square unit below it. The lowest option is 1.

**Example 2:**

**Input:** squares = \[\[0,0,2],[1,1,1]]

**Output:** 1.16667

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/15/4062example2drawio.png)

The areas are:

*   Below the line: `7/6 * 2 (Red) + 1/6 (Blue) = 15/6 = 2.5`.
*   Above the line: `5/6 * 2 (Red) + 5/6 (Blue) = 15/6 = 2.5`.

Since the areas above and below the line are equal, the output is `7/6 = 1.16667`.

**Constraints:**

*   <code>1 <= squares.length <= 5 * 10<sup>4</sup></code>
*   <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code>
*   `squares[i].length == 3`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= l<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public double separateSquares(int[][] squares) {
        long hi = 0L;
        long lo = 1_000_000_000L;
        for (int[] q : squares) {
            lo = Math.min(lo, q[1]);
            hi = Math.max(hi, q[1] + (long) q[2]);
        }
        while (lo <= hi) {
            long mid = (lo + hi) / 2;
            if (diff(mid, squares) <= 0) {
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }
        double diff1 = diff(hi, squares);
        double diff2 = diff(lo, squares);
        return hi + diff1 / (diff1 - diff2);
    }

    private double diff(long mid, int[][] squares) {
        double[] res = new double[2];
        for (int[] q : squares) {
            res[0] += Math.min(q[2], Math.max(0, mid - q[1])) * q[2];
            res[1] += Math.min(q[2], Math.max(0, q[1] + q[2] - mid)) * q[2];
        }
        return res[1] - res[0];
    }
}
```