[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2054\. Two Best Non-Overlapping Events

Medium

You are given a **0-indexed** 2D integer array of `events` where <code>events[i] = [startTime<sub>i</sub>, endTime<sub>i</sub>, value<sub>i</sub>]</code>. The <code>i<sup>th</sup></code> event starts at <code>startTime<sub>i</sub></code> and ends at <code>endTime<sub>i</sub></code>, and if you attend this event, you will receive a value of <code>value<sub>i</sub></code>. You can choose **at most** **two** **non-overlapping** events to attend such that the sum of their values is **maximized**.

Return _this **maximum** sum._

Note that the start time and end time is **inclusive**: that is, you cannot attend two events where one of them starts and the other ends at the same time. More specifically, if you attend an event with end time `t`, the next event must start at or after `t + 1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/09/21/picture5.png)

**Input:** events = \[\[1,3,2],[4,5,2],[2,4,3]]

**Output:** 4

**Explanation:** Choose the green events, 0 and 1 for a sum of 2 + 2 = 4. 

**Example 2:**

![Example 1 Diagram](https://assets.leetcode.com/uploads/2021/09/21/picture1.png)

**Input:** events = \[\[1,3,2],[4,5,2],[1,5,5]]

**Output:** 5

**Explanation:** Choose event 2 for a sum of 5. 

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/09/21/picture3.png)

**Input:** events = \[\[1,5,3],[1,5,1],[6,6,5]]

**Output:** 8

**Explanation:** Choose events 0 and 2 for a sum of 3 + 5 = 8.

**Constraints:**

*   <code>2 <= events.length <= 10<sup>5</sup></code>
*   `events[i].length == 3`
*   <code>1 <= startTime<sub>i</sub> <= endTime<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= value<sub>i</sub> <= 10<sup>6</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxTwoEvents(int[][] events) {
        Arrays.sort(events, (a, b) -> a[0] - b[0]);
        int[] max = new int[events.length];
        for (int i = events.length - 1; i >= 0; i--) {
            if (i == events.length - 1) {
                max[i] = events[i][2];
            } else {
                max[i] = Math.max(events[i][2], max[i + 1]);
            }
        }
        int ans = 0;
        for (int i = 0; i < events.length; i++) {
            int end = events[i][1];
            int left = i + 1;
            int right = events.length;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (events[mid][0] <= end) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            int value = events[i][2];
            if (right >= 0 && right < events.length) {
                value += max[right];
            }
            ans = Math.max(ans, value);
        }
        return ans;
    }
}
```