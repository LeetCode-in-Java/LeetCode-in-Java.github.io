[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2188\. Minimum Time to Finish the Race

Hard

You are given a **0-indexed** 2D integer array `tires` where <code>tires[i] = [f<sub>i</sub>, r<sub>i</sub>]</code> indicates that the <code>i<sup>th</sup></code> tire can finish its <code>x<sup>th</sup></code> successive lap in <code>f<sub>i</sub> * r<sub>i</sub><sup>(x-1)</sup></code> seconds.

*   For example, if <code>f<sub>i</sub> = 3</code> and <code>r<sub>i</sub> = 2</code>, then the tire would finish its <code>1<sup>st</sup></code> lap in `3` seconds, its <code>2<sup>nd</sup></code> lap in `3 * 2 = 6` seconds, its <code>3<sup>rd</sup></code> lap in <code>3 * 2<sup>2</sup> = 12</code> seconds, etc.

You are also given an integer `changeTime` and an integer `numLaps`.

The race consists of `numLaps` laps and you may start the race with **any** tire. You have an **unlimited** supply of each tire and after every lap, you may **change** to any given tire (including the current tire type) if you wait `changeTime` seconds.

Return _the **minimum** time to finish the race._

**Example 1:**

**Input:** tires = \[\[2,3],[3,4]], changeTime = 5, numLaps = 4

**Output:** 21

**Explanation:**

Lap 1: Start with tire 0 and finish the lap in 2 seconds.

Lap 2: Continue with tire 0 and finish the lap in 2 \* 3 = 6 seconds.

Lap 3: Change tires to a new tire 0 for 5 seconds and then finish the lap in another 2 seconds.

Lap 4: Continue with tire 0 and finish the lap in 2 \* 3 = 6 seconds.

Total time = 2 + 6 + 5 + 2 + 6 = 21 seconds.

The minimum time to complete the race is 21 seconds. 

**Example 2:**

**Input:** tires = \[\[1,10],[2,2],[3,4]], changeTime = 6, numLaps = 5

**Output:** 25

**Explanation:**

Lap 1: Start with tire 1 and finish the lap in 2 seconds.

Lap 2: Continue with tire 1 and finish the lap in 2 \* 2 = 4 seconds.

Lap 3: Change tires to a new tire 1 for 6 seconds and then finish the lap in another 2 seconds.

Lap 4: Continue with tire 1 and finish the lap in 2 \* 2 = 4 seconds.

Lap 5: Change tires to tire 0 for 6 seconds then finish the lap in another 1 second.

Total time = 2 + 4 + 6 + 2 + 4 + 6 + 1 = 25 seconds.

The minimum time to complete the race is 25 seconds. 

**Constraints:**

*   <code>1 <= tires.length <= 10<sup>5</sup></code>
*   `tires[i].length == 2`
*   <code>1 <= f<sub>i</sub>, changeTime <= 10<sup>5</sup></code>
*   <code>2 <= r<sub>i</sub> <= 10<sup>5</sup></code>
*   `1 <= numLaps <= 1000`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minimumFinishTime(int[][] tires, int changeTime, int numLaps) {
        int minf = Integer.MAX_VALUE;
        // find the minimum of f, to deal with special case and stronger constraints later.
        for (int[] tire : tires) {
            minf = Math.min(minf, tire[0]);
        }
        // if min-f >= changeTime, we can return early
        if (minf >= changeTime) {
            return minf * numLaps + changeTime * (numLaps - 1);
        }
        // shortest[i] record shortest time that one single tire is worth to go the i-th laps
        // worth to go means the i-th lap time is shorter than changeTime + f
        int[] shortest = new int[numLaps + 1];
        Arrays.fill(shortest, Integer.MAX_VALUE);
        int len = 0;
        // traverse all tires, and update the shortest[i]
        // this shortest time is available from [1, len] in the array
        // len is updated in the traverse
        for (int[] tire : tires) {
            int f = tire[0];
            int r = tire[1];
            // index start from 1 to be consistent with numLaps
            int index = 1;
            int t = f;
            int sum = t;
            // use changeTime + minf here, which is a strong constraints than changeTime + f
            while (t <= changeTime + minf && index <= numLaps) {
                shortest[index] = Math.min(shortest[index], sum);
                t = t * r;
                sum += t;
                index++;
            }
            len = Math.max(len, index - 1);
        }
        for (int i = 2; i <= numLaps; i++) {
            // for j > Math.min(i/2, len), it's simply recombination of the values of shortest
            // [1:len]
            // it's ok to go furthur for the loop, just repeat the Math.min computation
            for (int j = 1; j <= Math.min(i / 2, len); j++) {
                shortest[i] = Math.min(shortest[i], shortest[j] + shortest[i - j] + changeTime);
            }
        }
        return shortest[numLaps];
    }
}
```