## 2008\. Maximum Earnings From Taxi

Medium

There are `n` points on a road you are driving your taxi on. The `n` points on the road are labeled from `1` to `n` in the direction you are going, and you want to drive from point `1` to point `n` to make money by picking up passengers. You cannot change the direction of the taxi.

The passengers are represented by a **0-indexed** 2D integer array `rides`, where <code>rides[i] = [start<sub>i</sub>, end<sub>i</sub>, tip<sub>i</sub>]</code> denotes the <code>i<sup>th</sup></code> passenger requesting a ride from point <code>start<sub>i</sub></code> to point <code>end<sub>i</sub></code> who is willing to give a <code>tip<sub>i</sub></code> dollar tip.

For **each** passenger `i` you pick up, you **earn** <code>end<sub>i</sub> - start<sub>i</sub> + tip<sub>i</sub></code> dollars. You may only drive **at most one** passenger at a time.

Given `n` and `rides`, return _the **maximum** number of dollars you can earn by picking up the passengers optimally._

**Note:** You may drop off a passenger and pick up a different passenger at the same point.

**Example 1:**

**Input:** n = 5, rides = [[2,5,4],[1,5,1]]

**Output:** 7

**Explanation:** We can pick up passenger 0 to earn 5 - 2 + 4 = 7 dollars.

**Example 2:**

**Input:** n = 20, rides = [[1,6,1],[3,10,2],[10,12,3],[11,12,2],[12,15,2],[13,18,1]]

**Output:** 20

**Explanation:** We will pick up the following passengers: 

- Drive passenger 1 from point 3 to point 10 for a profit of 10 - 3 + 2 = 9 dollars. 

- Drive passenger 2 from point 10 to point 12 for a profit of 12 - 10 + 3 = 5 dollars.

- Drive passenger 5 from point 13 to point 18 for a profit of 18 - 13 + 1 = 6 dollars. 
  
We earn 9 + 5 + 6 = 20 dollars in total.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= rides.length <= 3 * 10<sup>4</sup></code>
*   `rides[i].length == 3`
*   <code>1 <= start<sub>i</sub> < end<sub>i</sub> <= n</code>
*   <code>1 <= tip<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public long maxTaxiEarnings(int n, int[][] rides) {
        if (rides.length == 1) {
            return calculateEarnings(rides[0]);
        }

        Map<Integer, List<int[]>> map = new HashMap<>();
        for (int[] ride : rides) {
            map.compute(ride[1], (k, v) -> (v == null) ? new ArrayList<>() : v).add(ride);
        }

        long[] maximisedEarnings = new long[n + 1];
        Arrays.fill(maximisedEarnings, 0);
        for (int i = 1; i < maximisedEarnings.length; i++) {
            maximisedEarnings[i] = maximisedEarnings[i - 1];
            List<int[]> passengers = map.get(i);
            if (passengers != null) {
                for (int[] passenger : passengers) {
                    final int earning = calculateEarnings(passenger);
                    maximisedEarnings[i] =
                            Math.max(
                                    maximisedEarnings[i],
                                    maximisedEarnings[passenger[0]] + earning);
                }
            }
        }
        return maximisedEarnings[n];
    }

    private int calculateEarnings(final int[] currentRide) {
        return currentRide[1] - currentRide[0] + currentRide[2];
    }
}
```