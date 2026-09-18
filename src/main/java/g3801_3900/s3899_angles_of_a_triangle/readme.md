[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3899\. Angles of a Triangle

Medium

You are given a positive integer array `sides` of length 3.

Determine if there exists a triangle with **positive** area whose three side lengths are given by the elements of `sides`.

If such a triangle exists, return an array of three floating-point numbers representing its internal angles (in **degrees**), **sorted** in **non-decreasing** order. Otherwise, return an empty array.

Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

**Input:** sides = [3,4,5]

**Output:** [36.86990,53.13010,90.00000]

**Explanation:**

You can form a right-angled triangle with side lengths 3, 4, and 5. The internal angles of this triangle are approximately 36.869897646, 53.130102354, and 90 degrees respectively.

**Example 2:**

**Input:** sides = [2,4,2]

**Output:** []

**Explanation:**

You cannot form a triangle with positive area using side lengths 2, 4, and 2.

**Constraints:**

*   `sides.length == 3`
*   `1 <= sides[i] <= 1000`

## Solution

```java
public class Solution {
    public double[] internalAngles(int[] sides) {
        if (sides[0] + sides[1] <= sides[2]
                || sides[1] + sides[2] <= sides[0]
                || sides[0] + sides[2] <= sides[1]) {
            return new double[0];
        }
        double[] angle = new double[sides.length];
        angle[0] =
                Math.toDegrees(
                        Math.acos(
                                (double)
                                                (sides[1] * sides[1]
                                                        + sides[2] * sides[2]
                                                        - sides[0] * sides[0])
                                        / (2 * sides[1] * sides[2])));
        angle[1] =
                Math.toDegrees(
                        Math.acos(
                                (double)
                                                (sides[0] * sides[0]
                                                        + sides[2] * sides[2]
                                                        - sides[1] * sides[1])
                                        / (2 * sides[0] * sides[2])));
        angle[2] =
                Math.toDegrees(
                        Math.acos(
                                (double)
                                                (sides[1] * sides[1]
                                                        + sides[0] * sides[0]
                                                        - sides[2] * sides[2])
                                        / (2 * sides[1] * sides[0])));
        double max = angle[0];
        double mid = 0;
        double min = 0;
        for (int i = 1; i < angle.length; i++) {
            if (angle[i] > max) {
                min = mid;
                mid = max;
                max = angle[i];
            } else {
                if (angle[i] > mid) {
                    min = mid;
                    mid = angle[i];
                } else {
                    min = angle[i];
                }
            }
        }
        angle[0] = min;
        angle[1] = mid;
        angle[2] = max;
        return angle;
    }
}
```