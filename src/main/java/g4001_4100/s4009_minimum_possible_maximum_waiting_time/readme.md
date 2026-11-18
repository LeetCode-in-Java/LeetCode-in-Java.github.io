[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4009\. Minimum Possible Maximum Waiting Time

Hard

You are given an integer array `demand`, where `demand[i]` is the amount of fuel required by the <code>i<sup>th</sup></code> car.

You are also given an integer array `fuel` of length 2. There are **exactly** two fuel dispensers, numbered 0 and 1, where `fuel[j]` is the initial amount of fuel available in dispenser `j`.

Cars are allowed to start refueling in **increasing** index order. Car 0 becomes allowed at time 0, and for each `i > 0`, car `i` becomes allowed **exactly** when car `i - 1` starts refueling.

The refueling process follows these rules:

*   Each dispenser can serve **at most** one car at a time.
*   When a car becomes allowed, you must choose a dispenser with **at least** `demand[i]` fuel remaining. If both dispensers have enough fuel remaining, you may choose **either** of them, regardless of when they become free.
*   The car waits until the chosen dispenser becomes free and starts refueling **immediately**. It cannot switch dispensers or intentionally wait after the chosen dispenser becomes free.
*   When a car starts refueling, the remaining fuel in the chosen dispenser decreases by `demand[i]`, and the dispenser remains occupied for `demand[i]` seconds.
*   Once started, refueling cannot be interrupted.
*   If neither dispenser has at least `demand[i]` fuel remaining when car `i` becomes allowed, the process terminates and no further cars can be served.

The **waiting time** of a car is the time between when it becomes allowed to start refueling and when it actually starts.

Return the **minimum** possible value of the **maximum** waiting time among all served cars over all assignments that **maximize** the number of served cars. If no car can be served, return -1.

OriginalJava**Example 1:**

**Input:** demand = [6,8,4,6,5], fuel = [16,13]

**Output:** 6

**Explanation:**

The following assignment serves all five cars:

| Car | Becomes allowed at | Starts refueling at | Dispenser used | Remaining fuel before start <br> (dispenser 0, dispenser 1) | Waiting time |
| --- | --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 0 | (16, 13) | 0 |
| 1 | 0 | 0 | 1 | (10, 13) | 0 |
| 2 | 0 | 6 | 0 | (10, 5) | 6 |
| 3 | 6 | 10 | 0 | (6, 5) | 4 |
| 4 | 10 | 10 | 1 | (0, 5) | 0 |

Thus, all five cars are served, and the maximum waiting time is 6.

To serve all five cars, dispenser 0 must serve the cars with demands 6, 4, and 6, while dispenser 1 must serve the cars with demands 8 and 5. Therefore, car 2 must wait until time 6 for dispenser 0 to become free, so no assignment serving all five cars can have a maximum waiting time less than 6.

**Example 2:**

**Input:** demand = [10,15], fuel = [12,17]

**Output:** 0

**Explanation:**

*   At time 0, Car 0 becomes allowed and starts refuelling using dispenser 0.
*   Car 1 becomes allowed at time 0 (when Car 0 starts) and immediately starts refuelling using dispenser 1.
*   Both cars start without waiting, so the maximum waiting time is 0.

**Example 3:**

**Input:** demand = [10,5], fuel = [8,8]

**Output:** \-1

**Explanation:**

*   At time 0, Car 0 becomes allowed. However, neither dispenser has enough fuel to serve it, so the process terminates immediately.
*   No car is served, so the answer is -1.

**Constraints:**

*   `1 <= demand.length <= 50`
*   `1 <= demand[i] <= 20`
*   `fuel.length == 2`
*   `1 <= fuel[i] <= 50`

## Solution

```java
public class Solution {
    public int minMaxWaitingTime(int[] demand, int[] fuel) {
        int result = solve(0, fuel[0], fuel[1], 0, 0, demand, new java.util.HashMap<>());
        return (result >>> 5) == 0 ? -1 : result & 31;
    }

    private int solve(
            int index,
            int fuel0,
            int fuel1,
            int busy0,
            int busy1,
            int[] demand,
            java.util.HashMap<Integer, Integer> memo) {
        if (index == demand.length) {
            return 0;
        }
        int key = (index << 22) | (fuel0 << 16) | (fuel1 << 10) | (busy0 << 5) | busy1;
        Integer cached = memo.get(key);
        if (cached != null) {
            return cached;
        }
        int best = 0;
        int required = demand[index];
        if (fuel0 >= required) {
            int wait = busy0;
            int child =
                    solve(
                            index + 1,
                            fuel0 - required,
                            fuel1,
                            required,
                            Math.max(0, busy1 - wait),
                            demand,
                            memo);
            best = (1 + (child >>> 5)) * 32 + Math.max(wait, child & 31);
        }
        if (fuel1 >= required) {
            int wait = busy1;
            int child =
                    solve(
                            index + 1,
                            fuel0,
                            fuel1 - required,
                            Math.max(0, busy0 - wait),
                            required,
                            demand,
                            memo);
            int candidate = (1 + (child >>> 5)) * 32 + Math.max(wait, child & 31);
            if ((candidate >>> 5) > (best >>> 5)
                    || ((candidate >>> 5) == (best >>> 5) && (candidate & 31) < (best & 31))) {
                best = candidate;
            }
        }
        memo.put(key, best);
        return best;
    }
}
```