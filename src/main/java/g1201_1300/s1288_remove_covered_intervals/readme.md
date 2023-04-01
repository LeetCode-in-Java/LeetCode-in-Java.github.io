[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1288\. Remove Covered Intervals

Medium

Given an array `intervals` where <code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> represent the interval <code>[l<sub>i</sub>, r<sub>i</sub>)</code>, remove all intervals that are covered by another interval in the list.

The interval `[a, b)` is covered by the interval `[c, d)` if and only if `c <= a` and `b <= d`.

Return _the number of remaining intervals_.

**Example 1:**

**Input:** intervals = \[\[1,4],[3,6],[2,8]]

**Output:** 2

**Explanation:** Interval [3,6] is covered by [2,8], therefore it is removed.

**Example 2:**

**Input:** intervals = \[\[1,4],[2,3]]

**Output:** 1

**Constraints:**

*   `1 <= intervals.length <= 1000`
*   `intervals[i].length == 2`
*   <code>0 <= l<sub>i</sub> < r<sub>i</sub> <= 10<sup>5</sup></code>
*   All the given intervals are **unique**.

## Solution

```java
import java.util.PriorityQueue;
import java.util.Queue;

public class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        Queue<int[]> q = new PriorityQueue<>((a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        for (int[] interval : intervals) {
            q.offer(interval);
        }
        int[] prev = q.poll();
        int count = 0;
        while (!q.isEmpty()) {
            int[] curr = q.poll();
            if (curr[0] >= prev[0] && curr[1] <= prev[1]) {
                count++;
            } else {
                prev = curr;
            }
        }
        return intervals.length - count;
    }
}
```