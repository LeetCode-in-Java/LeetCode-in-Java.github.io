[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 593\. Valid Square

Medium

Given the coordinates of four points in 2D space `p1`, `p2`, `p3` and `p4`, return `true` _if the four points construct a square_.

The coordinate of a point <code>p<sub>i</sub></code> is represented as <code>[x<sub>i</sub>, y<sub>i</sub>]</code>. The input is **not** given in any order.

A **valid square** has four equal sides with positive length and four equal angles (90-degree angles).

**Example 1:**

**Input:** p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]

**Output:** true 

**Example 2:**

**Input:** p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,12]

**Output:** false 

**Example 3:**

**Input:** p1 = [1,0], p2 = [-1,0], p3 = [0,1], p4 = [0,-1]

**Output:** true 

**Constraints:**

*   `p1.length == p2.length == p3.length == p4.length == 2`
*   <code>-10<sup>4</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>4</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public boolean validSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        int[] distancesSquared = new int[6];
        distancesSquared[0] = getDistanceSquared(p1, p2);
        distancesSquared[1] = getDistanceSquared(p1, p3);
        distancesSquared[2] = getDistanceSquared(p1, p4);
        distancesSquared[3] = getDistanceSquared(p2, p3);
        distancesSquared[4] = getDistanceSquared(p2, p4);
        distancesSquared[5] = getDistanceSquared(p3, p4);
        Arrays.sort(distancesSquared);
        if (distancesSquared[0] == 0) {
            return false;
        }
        if (distancesSquared[0] != distancesSquared[3]) {
            return false;
        }
        if (distancesSquared[4] != distancesSquared[5]) {
            return false;
        }
        return distancesSquared[5] == 2 * distancesSquared[0];
    }

    private int getDistanceSquared(int[] p1, int[] p2) {
        int deltaX = p2[0] - p1[0];
        int deltaY = p2[1] - p1[1];
        return deltaX * deltaX + deltaY * deltaY;
    }
}
```