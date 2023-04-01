[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1232\. Check If It Is a Straight Line

Easy

You are given an array `coordinates`, `coordinates[i] = [x, y]`, where `[x, y]` represents the coordinate of a point. Check if these points make a straight line in the XY plane.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/10/15/untitled-diagram-2.jpg)

**Input:** coordinates = \[\[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]

**Output:** true

**Example 2:**

**![](https://assets.leetcode.com/uploads/2019/10/09/untitled-diagram-1.jpg)**

**Input:** coordinates = \[\[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]

**Output:** false

**Constraints:**

*   `2 <= coordinates.length <= 1000`
*   `coordinates[i].length == 2`
*   `-10^4 <= coordinates[i][0], coordinates[i][1] <= 10^4`
*   `coordinates` contains no duplicate point.

## Solution

```java
public class Solution {
    public boolean checkStraightLine(int[][] coordinates) {
        int deltaX1 = coordinates[0][0] - coordinates[1][0];
        int deltaY1 = coordinates[0][1] - coordinates[1][1];
        int[] prev = coordinates[1];
        for (int i = 2; i < coordinates.length; ++i) {
            int[] point = coordinates[i];
            int deltaX2 = point[0] - prev[0];
            int deltaY2 = point[1] - prev[1];
            if (deltaX1 * deltaY2 != deltaX2 * deltaY1) {
                return false;
            }
        }
        return true;
    }
}
```