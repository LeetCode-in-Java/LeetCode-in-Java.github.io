[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 495\. Teemo Attacking

Easy

Our hero Teemo is attacking an enemy Ashe with poison attacks! When Teemo attacks Ashe, Ashe gets poisoned for a exactly `duration` seconds. More formally, an attack at second `t` will mean Ashe is poisoned during the **inclusive** time interval `[t, t + duration - 1]`. If Teemo attacks again **before** the poison effect ends, the timer for it is **reset**, and the poison effect will end `duration` seconds after the new attack.

You are given a **non-decreasing** integer array `timeSeries`, where `timeSeries[i]` denotes that Teemo attacks Ashe at second `timeSeries[i]`, and an integer `duration`.

Return _the **total** number of seconds that Ashe is poisoned_.

**Example 1:**

**Input:** timeSeries = [1,4], duration = 2

**Output:** 4

**Explanation:** Teemo's attacks on Ashe go as follows: - At second 1, Teemo attacks, and Ashe is poisoned for seconds 1 and 2. - At second 4, Teemo attacks, and Ashe is poisoned for seconds 4 and 5. Ashe is poisoned for seconds 1, 2, 4, and 5, which is 4 seconds in total.

**Example 2:**

**Input:** timeSeries = [1,2], duration = 2

**Output:** 3

**Explanation:** 

Teemo's attacks on Ashe go as follows: 

- At second 1, Teemo attacks, and Ashe is poisoned for seconds 1 and 2. 

- At second 2 however, Teemo attacks again and resets the poison timer. Ashe is poisoned for seconds 2 and 3. Ashe is poisoned for seconds 1, 2, and 3, which is 3 seconds in total.

**Constraints:**

*   <code>1 <= timeSeries.length <= 10<sup>4</sup></code>
*   <code>0 <= timeSeries[i], duration <= 10<sup>7</sup></code>
*   `timeSeries` is sorted in **non-decreasing** order.

## Solution

```java
public class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        if (duration == 0) {
            return 0;
        }
        int start = timeSeries[0];
        int end = timeSeries[0] + duration - 1;
        int poisonDuration = end - start + 1;
        for (int i = 1; i < timeSeries.length; i++) {
            if (timeSeries[i] <= end) {
                poisonDuration += (duration - (end - timeSeries[i] + 1));
                end += (duration - (end - timeSeries[i] + 1));
            } else {
                start = timeSeries[i];
                end = timeSeries[i] + duration - 1;
                poisonDuration += end - start + 1;
            }
        }
        return poisonDuration;
    }
}
```