[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2528\. Maximize the Minimum Powered City

Hard

You are given a **0-indexed** integer array `stations` of length `n`, where `stations[i]` represents the number of power stations in the <code>i<sup>th</sup></code> city.

Each power station can provide power to every city in a fixed **range**. In other words, if the range is denoted by `r`, then a power station at city `i` can provide power to all cities `j` such that `|i - j| <= r` and `0 <= i, j <= n - 1`.

*   Note that `|x|` denotes **absolute** value. For example, `|7 - 5| = 2` and `|3 - 10| = 7`.

The **power** of a city is the total number of power stations it is being provided power from.

The government has sanctioned building `k` more power stations, each of which can be built in any city, and have the same range as the pre-existing ones.

Given the two integers `r` and `k`, return _the **maximum possible minimum power** of a city, if the additional power stations are built optimally._

**Note** that you can build the `k` power stations in multiple cities.

**Example 1:**

**Input:** stations = [1,2,4,5,0], r = 1, k = 2

**Output:** 5

**Explanation:** 

One of the optimal ways is to install both the power stations at city 1. 

So stations will become [1,4,4,5,0]. 

- City 0 is provided by 1 + 4 = 5 power stations. 

- City 1 is provided by 1 + 4 + 4 = 9 power stations. 

- City 2 is provided by 4 + 4 + 5 = 13 power stations. 

- City 3 is provided by 5 + 4 = 9 power stations.

- City 4 is provided by 5 + 0 = 5 power stations. 

So the minimum power of a city is 5. 

Since it is not possible to obtain a larger power, we return 5.

**Example 2:**

**Input:** stations = [4,4,4,4], r = 0, k = 3

**Output:** 4

**Explanation:** It can be proved that we cannot make the minimum power of a city greater than 4.

**Constraints:**

*   `n == stations.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= stations[i] <= 10<sup>5</sup></code>
*   `0 <= r <= n - 1`
*   <code>0 <= k <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    private boolean canIBeTheMinimum(long[] power, long minimum, int k, int r) {
        int n = power.length;
        long[] extraPower = new long[n];
        for (int i = 0; i < n; i++) {
            if (i > 0) {
                extraPower[i] += extraPower[i - 1];
            }
            long curPower = power[i] + extraPower[i];
            long req = minimum - curPower;
            if (req <= 0) {
                continue;
            }
            if (req > k) {
                return false;
            }
            k -= (int) req;
            extraPower[i] += (req);
            if (i + 2 * r + 1 < n) {
                extraPower[i + 2 * r + 1] -= (req);
            }
        }
        return true;
    }

    private long[] calculatePowerArray(int[] stations, int r) {
        int n = stations.length;
        long[] preSum = new long[n];
        for (int i = 0; i < n; i++) {
            int st = i - r;
            int last = i + r + 1;
            if (st < 0) {
                st = 0;
            }
            preSum[st] += stations[i];
            if (last < n) {
                preSum[last] -= stations[i];
            }
        }
        for (int i = 1; i < n; i++) {
            preSum[i] += preSum[i - 1];
        }
        return preSum;
    }

    public long maxPower(int[] stations, int r, int k) {
        long min = 0;
        long sum = (long) Math.pow(10, 10) + (long) Math.pow(10, 9);
        long[] power = calculatePowerArray(stations, r);
        long ans = -1;
        while (min <= sum) {
            long mid = (min + sum) >> 1;
            if (canIBeTheMinimum(power, mid, k, r)) {
                ans = mid;
                min = mid + 1;
            } else {
                sum = mid - 1;
            }
        }
        return ans;
    }
}
```