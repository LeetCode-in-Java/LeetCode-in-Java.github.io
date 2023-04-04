[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1010\. Pairs of Songs With Total Durations Divisible by 60

Medium

You are given a list of songs where the <code>i<sup>th</sup></code> song has a duration of `time[i]` seconds.

Return _the number of pairs of songs for which their total duration in seconds is divisible by_ `60`. Formally, we want the number of indices `i`, `j` such that `i < j` with `(time[i] + time[j]) % 60 == 0`.

**Example 1:**

**Input:** time = [30,20,150,100,40]

**Output:** 3

**Explanation:** Three pairs have a total duration divisible by 60:

(time[0] = 30, time[2] = 150): total duration 180

(time[1] = 20, time[3] = 100): total duration 120

(time[1] = 20, time[4] = 40): total duration 60 

**Example 2:**

**Input:** time = [60,60,60]

**Output:** 3

**Explanation:** All three pairs have a total duration of 120, which is divisible by 60. 

**Constraints:**

*   <code>1 <= time.length <= 6 * 10<sup>4</sup></code>
*   `1 <= time[i] <= 500`

## Solution

```java
public class Solution {
    public int numPairsDivisibleBy60(int[] time) {
        int[] remainder = new int[60];
        int ans = 0;
        for (int j : time) {
            int rem = j % 60;
            if (rem == 0) {
                ans += remainder[0];
            } else {
                ans += remainder[60 - rem];
            }
            remainder[rem]++;
        }
        return ans;
    }
}
```