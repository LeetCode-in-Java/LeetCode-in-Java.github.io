## 874\. Walking Robot Simulation

Medium

A robot on an infinite XY-plane starts at point `(0, 0)` facing north. The robot can receive a sequence of these three possible types of `commands`:

*   `-2`: Turn left `90` degrees.
*   `-1`: Turn right `90` degrees.
*   `1 <= k <= 9`: Move forward `k` units, one unit at a time.

Some of the grid squares are `obstacles`. The <code>i<sup>th</sup></code> obstacle is at grid point <code>obstacles[i] = (x<sub>i</sub>, y<sub>i</sub>)</code>. If the robot runs into an obstacle, then it will instead stay in its current location and move on to the next command.

Return _the **maximum Euclidean distance** that the robot ever gets from the origin **squared** (i.e. if the distance is_ `5`_, return_ `25`_)_.

**Note:**

*   North means +Y direction.
*   East means +X direction.
*   South means -Y direction.
*   West means -X direction.

**Example 1:**

**Input:** commands = [4,-1,3], obstacles = []

**Output:** 25

**Explanation:**

The robot starts at (0, 0):

1. Move north 4 units to (0, 4).

2. Turn right.

3. Move east 3 units to (3, 4).

The furthest point the robot ever gets from the origin is (3, 4), which squared is 3<sup>2</sup> + 4<sup>2</sup> = 25 units away.

**Example 2:**

**Input:** commands = [4,-1,4,-2,4], obstacles = \[\[2,4]]

**Output:** 65

**Explanation:**

The robot starts at (0, 0):

1. Move north 4 units to (0, 4).

2. Turn right.

3. Move east 1 unit and get blocked by the obstacle at (2, 4), robot is at (1, 4).

4. Turn left.

5. Move north 4 units to (1, 8).

The furthest point the robot ever gets from the origin is (1, 8), which squared is 1<sup>2</sup> + 8<sup>2</sup> = 65 units away.

**Example 3:**

**Input:** commands = [6,-1,-1,6], obstacles = []

**Output:** 36

**Explanation:**

The robot starts at (0, 0):

1. Move north 6 units to (0, 6).

2. Turn right.

3. Turn right.

4. Move south 6 units to (0, 0).

The furthest point the robot ever gets from the origin is (0, 6), which squared is 6<sup>2</sup> = 36 units away.

**Constraints:**

*   <code>1 <= commands.length <= 10<sup>4</sup></code>
*   `commands[i]` is either `-2`, `-1`, or an integer in the range `[1, 9]`.
*   <code>0 <= obstacles.length <= 10<sup>4</sup></code>
*   <code>-3 * 10<sup>4</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 3 * 10<sup>4</sup></code>
*   The answer is guaranteed to be less than <code>2<sup>31</sup></code>.

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.TreeSet;

public class Solution {
    public int robotSim(int[] commands, int[][] obstacles) {
        // 0=right, 1=up, 2=left, 3=down
        int direction = 1;
        int x = 0;
        int y = 0;
        int maxDis = 0;
        Map<Integer, TreeSet<Integer>> xMap = new HashMap<>(obstacles.length);
        Map<Integer, TreeSet<Integer>> yMap = new HashMap<>(obstacles.length);
        for (int[] xy : obstacles) {
            xMap.computeIfAbsent(xy[0], k -> new TreeSet<>()).add(xy[1]);
            yMap.computeIfAbsent(xy[1], k -> new TreeSet<>()).add(xy[0]);
        }
        for (int cmd : commands) {
            if (cmd == -1) {
                direction += 3;
                direction %= 4;
            } else if (cmd == -2) {
                direction += 1;
                direction %= 4;
            } else {
                Integer next = null;
                switch (direction) {
                    case 0:
                        if (yMap.containsKey(y)) {
                            next = yMap.get(y).ceiling(x + 1);
                        }
                        if (next != null) {
                            x = Math.min(x + cmd, next - 1);
                        } else {
                            x += cmd;
                        }
                        break;
                    case 1:
                        if (xMap.containsKey(x)) {
                            next = xMap.get(x).ceiling(y + 1);
                        }
                        if (next != null) {
                            y = Math.min(y + cmd, next - 1);
                        } else {
                            y += cmd;
                        }
                        break;
                    case 2:
                        if (yMap.containsKey(y)) {
                            next = yMap.get(y).floor(x - 1);
                        }
                        if (next != null) {
                            x = Math.max(x - cmd, next + 1);
                        } else {
                            x -= cmd;
                        }
                        break;
                    case 3:
                        if (xMap.containsKey(x)) {
                            next = xMap.get(x).floor(y - 1);
                        }
                        if (next != null) {
                            y = Math.max(y - cmd, next + 1);
                        } else {
                            y -= cmd;
                        }
                        break;
                    default:
                        // error
                        break;
                }
                maxDis = Math.max(maxDis, x * x + y * y);
            }
        }
        return maxDis;
    }
}
```