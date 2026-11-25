[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4024\. Nearest Available Drone

Easy

You are given a 2D integer array `drones`, where <code>drones[i] = [x<sub>i</sub>, y<sub>i</sub>, range<sub>i</sub>]</code> represents the x-coordinate, y-coordinate, and travel range of the <code>i<sup>th</sup></code> drone.

You are also given an integer array `target = [tx, ty]`, representing the coordinates of the target.

A drone `drones[i]` can reach the target if the Manhattan distance between its coordinates and the target coordinates is **less than or equal** to its <code>range<sub>i</sub></code>.

Return the **index** of the reachable drone with the **minimum Manhattan distance** to the target. If there is a tie, return the **smallest index**. If no drone can reach the target, return `-1`.

**Example 1:**

**Input:** drones = \[\[0,0,8],[2,2,9]], target = [3,4]

**Output:** 1

**Explanation:**

*   The distance between `drones[0]` and `target` is `|0 - 3| + |0 - 4| = 7`, which is within its range of 8.
*   The distance between `drones[1]` and `target` is `|2 - 3| + |2 - 4| = 3`, which is within its range of 9.
*   Since `drones[1]` is the nearest drone, the answer is 1.

**Example 2:**

**Input:** drones = \[\[2,1,5],[4,4,5],[6,6,8]], target = [5,5]

**Output:** 1

**Explanation:**

*   The distance between `drones[0]` and `target` is `|2 - 5| + |1 - 5| = 7`, which is greater than its range of 5.
*   The distance between `drones[1]` and `target` is `|4 - 5| + |4 - 5| = 2`, which is within its range of 5.
*   The distance between `drones[2]` and `target` is `|6 - 5| + |6 - 5| = 2`, which is within its range of 8.
*   Both `drones[1]` and `drones[2]` are the nearest drones. Since we should return the smallest index, the answer is 1.

**Example 3:**

**Input:** drones = \[\[4,4,5]], target = [8,6]

**Output:** \-1

**Explanation:**

*   The distance between `drones[0]` and `target` is `|4 - 8| + |4 - 6| = 6`, which is greater than its range of 5.
*   No drone can reach the target, so the answer is -1.

**Constraints:**

*   `1 <= drones.length <= 100`
*   <code>drones[i] = [x<sub>i</sub>, y<sub>i</sub>, range<sub>i</sub>]</code>
*   `target = [tx, ty]`
*   <code>-25 <= x<sub>i</sub>, y<sub>i</sub>, tx, ty <= 25</code>
*   <code>1 <= range<sub>i</sub> <= 100</code>

## Solution

```java
public class Solution {
    public int nearestDrone(int[][] drones, int[] target) {
        int min = Integer.MAX_VALUE;
        int idx = -1;
        for (int i = 0; i < drones.length; i++) {
            int[] d = drones[i];
            int dist = Math.abs(target[0] - d[0]) + Math.abs(target[1] - d[1]);
            if (dist <= d[2] && min > dist) {
                min = dist;
                idx = i;
            }
        }
        return idx;
    }
}
```