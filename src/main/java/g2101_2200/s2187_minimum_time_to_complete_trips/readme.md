[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2187\. Minimum Time to Complete Trips

Medium

You are given an array `time` where `time[i]` denotes the time taken by the <code>i<sup>th</sup></code> bus to complete **one trip**.

Each bus can make multiple trips **successively**; that is, the next trip can start **immediately after** completing the current trip. Also, each bus operates **independently**; that is, the trips of one bus do not influence the trips of any other bus.

You are also given an integer `totalTrips`, which denotes the number of trips all buses should make **in total**. Return _the **minimum time** required for all buses to complete **at least**_ `totalTrips` _trips_.

**Example 1:**

**Input:** time = [1,2,3], totalTrips = 5

**Output:** 3

**Explanation:**

- At time t = 1, the number of trips completed by each bus are [1,0,0].

The total number of trips completed is 1 + 0 + 0 = 1.

- At time t = 2, the number of trips completed by each bus are [2,1,0].

The total number of trips completed is 2 + 1 + 0 = 3.

- At time t = 3, the number of trips completed by each bus are [3,1,1].

The total number of trips completed is 3 + 1 + 1 = 5.

So the minimum time needed for all buses to complete at least 5 trips is 3. 

**Example 2:**

**Input:** time = [2], totalTrips = 1

**Output:** 2

**Explanation:**

There is only one bus, and it will complete its first trip at t = 2.

So the minimum time needed to complete 1 trip is 2. 

**Constraints:**

*   <code>1 <= time.length <= 10<sup>5</sup></code>
*   <code>1 <= time[i], totalTrips <= 10<sup>7</sup></code>

## Solution

```java
public class Solution {
    public long minimumTime(int[] time, int totalTrips) {
        return bs(0, Long.MAX_VALUE, time, totalTrips);
    }

    private long bs(long left, long right, int[] time, long totalTrips) {
        if (left > right) {
            return Long.MAX_VALUE;
        }
        long mid = (left + right) >> 1;
        return isPossible(time, mid, totalTrips)
                ? Math.min(mid, bs(left, mid - 1, time, totalTrips))
                : bs(mid + 1, right, time, totalTrips);
    }

    private boolean isPossible(int[] time, long mid, long totalTrips) {
        long count = 0;
        for (int i : time) {
            count += mid / i;
            if (count >= totalTrips) {
                return true;
            }
        }
        return false;
    }
}
```