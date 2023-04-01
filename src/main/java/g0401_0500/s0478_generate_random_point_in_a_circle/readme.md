[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 478\. Generate Random Point in a Circle

Medium

Given the radius and the position of the center of a circle, implement the function `randPoint` which generates a uniform random point inside the circle.

Implement the `Solution` class:

*   `Solution(double radius, double x_center, double y_center)` initializes the object with the radius of the circle `radius` and the position of the center `(x_center, y_center)`.
*   `randPoint()` returns a random point inside the circle. A point on the circumference of the circle is considered to be in the circle. The answer is returned as an array `[x, y]`.

**Example 1:**

**Input** ["Solution", "randPoint", "randPoint", "randPoint"] [[1.0, 0.0, 0.0], [], [], []]

**Output:** [null, [-0.02493, -0.38077], [0.82314, 0.38945], [0.36572, 0.17248]]

**Explanation:** Solution solution = new Solution(1.0, 0.0, 0.0); solution.randPoint(); // return [-0.02493, -0.38077] solution.randPoint(); // return [0.82314, 0.38945] solution.randPoint(); // return [0.36572, 0.17248]

**Constraints:**

*   <code>0 < radius <= 10<sup>8</sup></code>
*   <code>-10<sup>7</sup> <= x_center, y_center <= 10<sup>7</sup></code>
*   At most <code>3 * 10<sup>4</sup></code> calls will be made to `randPoint`.

## Solution

```java
import java.util.Random;

@SuppressWarnings("java:S2245")
public class Solution {
    private final double radius;
    private final double xCenter;
    private final double yCenter;
    private final Random random = new Random();

    public Solution(double radius, double xCenter, double yCenter) {
        this.radius = radius;
        this.xCenter = xCenter;
        this.yCenter = yCenter;
    }

    public double[] randPoint() {
        double x = getCoordinate(xCenter);
        double y = getCoordinate(yCenter);
        while (getDistance(x, y) >= radius * radius) {
            x = getCoordinate(xCenter);
            y = getCoordinate(yCenter);
        }
        return new double[] {x, y};
    }

    private double getDistance(double x, double y) {
        return (xCenter - x) * (xCenter - x) + (yCenter - y) * (yCenter - y);
    }

    private double getCoordinate(double center) {
        return center - radius + random.nextDouble() * 2 * radius;
    }
}

/*
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(radius, x_center, y_center);
 * double[] param_1 = obj.randPoint();
 */
```