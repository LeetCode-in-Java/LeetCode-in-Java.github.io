[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2332\. The Latest Time to Catch a Bus

Medium

You are given a **0-indexed** integer array `buses` of length `n`, where `buses[i]` represents the departure time of the <code>i<sup>th</sup></code> bus. You are also given a **0-indexed** integer array `passengers` of length `m`, where `passengers[j]` represents the arrival time of the <code>j<sup>th</sup></code> passenger. All bus departure times are unique. All passenger arrival times are unique.

You are given an integer `capacity`, which represents the **maximum** number of passengers that can get on each bus.

The passengers will get on the next available bus. You can get on a bus that will depart at `x` minutes if you arrive at `y` minutes where `y <= x`, and the bus is not full. Passengers with the **earliest** arrival times get on the bus first.

Return _the latest time you may arrive at the bus station to catch a bus_. You **cannot** arrive at the same time as another passenger.

**Note:** The arrays `buses` and `passengers` are not necessarily sorted.

**Example 1:**

**Input:** buses = [10,20], passengers = [2,17,18,19], capacity = 2

**Output:** 16

**Explanation:**

The 1<sup>st</sup> bus departs with the 1<sup>st</sup> passenger.

The 2<sup>nd</sup> bus departs with you and the 2<sup>nd</sup> passenger.

Note that you must not arrive at the same time as the passengers, which is why you must arrive before the 2<sup>nd</sup> passenger to catch the bus.

**Example 2:**

**Input:** buses = [20,30,10], passengers = [19,13,26,4,25,11,21], capacity = 2

**Output:** 20

**Explanation:**

The 1<sup>st</sup> bus departs with the 4<sup>th</sup> passenger.

The 2<sup>nd</sup> bus departs with the 6<sup>th</sup> and 2<sup>nd</sup> passengers.

The 3<sup>rd</sup> bus departs with the 1<sup>s</sup><sup>t</sup> passenger and you. 

**Constraints:**

*   `n == buses.length`
*   `m == passengers.length`
*   <code>1 <= n, m, capacity <= 10<sup>5</sup></code>
*   <code>2 <= buses[i], passengers[i] <= 10<sup>9</sup></code>
*   Each element in `buses` is **unique**.
*   Each element in `passengers` is **unique**.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int latestTimeCatchTheBus(int[] buses, int[] passengers, int capacity) {
        // sort arrays and move in arrays from left to right and find capacity in last bus
        // if capcity is full in last bus then find time last passenger might have boarded then go
        // backward till find a slot to replace last passenger
        // if capacity is not full in last bus then start with last bus departure time and check if
        // can board on last moment and go backward till find a available time slot
        Arrays.sort(buses);
        Arrays.sort(passengers);
        int blen = buses.length;
        int plen = passengers.length;
        int b = 0;
        int p = 0;
        int c = 0;
        // find capacity in last bus
        while (b < blen && p < plen) {
            if (passengers[p] <= buses[b] && c < capacity) {
                c++;
                p++;
            }
            if (c == capacity || (p < plen && passengers[p] > buses[b])) {
                if (b < (blen - 1)) {
                    c = 0;
                }
                b++;
            }
        }
        int start = 0;
        if (c == capacity) {
            // capcity is full in last bus, find time last passenger might have boarded
            start = Math.min(passengers[p - 1], buses[blen - 1]);
        } else {
            // capacity is not full in last bus, start with last bus departure time and check if can
            // board on last moment
            start = buses[blen - 1];
        }
        // go backward till find a slot
        while (p > 0 && start == passengers[p - 1]) {
            start--;
            p--;
        }
        return start;
    }
}
```