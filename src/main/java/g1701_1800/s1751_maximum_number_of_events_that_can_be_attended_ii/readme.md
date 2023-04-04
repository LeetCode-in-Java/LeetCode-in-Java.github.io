[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1751\. Maximum Number of Events That Can Be Attended II

Hard

You are given an array of `events` where <code>events[i] = [startDay<sub>i</sub>, endDay<sub>i</sub>, value<sub>i</sub>]</code>. The <code>i<sup>th</sup></code> event starts at <code>startDay<sub>i</sub></code> and ends at <code>endDay<sub>i</sub></code>, and if you attend this event, you will receive a value of <code>value<sub>i</sub></code>. You are also given an integer `k` which represents the maximum number of events you can attend.

You can only attend one event at a time. If you choose to attend an event, you must attend the **entire** event. Note that the end day is **inclusive**: that is, you cannot attend two events where one of them starts and the other ends on the same day.

Return _the **maximum sum** of values that you can receive by attending events._

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60048-pm.png)

**Input:** events = \[\[1,2,4],[3,4,3],[2,3,1]], k = 2

**Output:** 7

**Explanation:** Choose the green events, 0 and 1 (0-indexed) for a total value of 4 + 3 = 7.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60150-pm.png)

**Input:** events = \[\[1,2,4],[3,4,3],[2,3,10]], k = 2

**Output:** 10

**Explanation:** Choose event 2 for a total value of 10. Notice that you cannot attend any other event as they overlap, and that you do **not** have to attend k events.

**Example 3:**

**![](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60703-pm.png)**

**Input:** events = \[\[1,1,1],[2,2,2],[3,3,3],[4,4,4]], k = 3

**Output:** 9

**Explanation:** Although the events do not overlap, you can only attend 3 events. Pick the highest valued three.

**Constraints:**

*   `1 <= k <= events.length`
*   <code>1 <= k * events.length <= 10<sup>6</sup></code>
*   <code>1 <= startDay<sub>i</sub> <= endDay<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= value<sub>i</sub> <= 10<sup>6</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.Optional;

public class Solution {
    public int maxValue(int[][] events, int k) {
        if (k == 1) {
            Optional<int[]> value = Arrays.stream(events).max(Comparator.comparingInt(e -> e[2]));
            if (value.isPresent()) {
                return value.get()[2];
            } else {
                throw new NullPointerException();
            }
        }

        int n = events.length;
        Arrays.sort(events, Comparator.comparingInt(a -> a[0]));

        int[][] memo = new int[n][k + 1];

        return dfs(events, 0, k, memo);
    }

    private int dfs(int[][] events, int i, int k, int[][] memo) {
        if (k == 0 || i >= events.length) {
            return 0;
        }
        if (memo[i][k] > 0) {
            return memo[i][k];
        }

        int idx = binarySearch(events, events[i][1] + 1, i + 1);
        int use = events[i][2] + dfs(events, idx, k - 1, memo);

        int notUse = dfs(events, i + 1, k, memo);

        int res = Math.max(use, notUse);
        memo[i][k] = res;

        return res;
    }

    private int binarySearch(int[][] events, int i, int st) {

        if (st >= events.length) {
            return st;
        }

        int end = events.length - 1;
        while (st < end) {
            int mid = st + (end - st) / 2;
            if (events[mid][0] < i) {
                st = mid + 1;
            } else {
                end = mid;
            }
        }

        return events[st][0] >= i ? st : st + 1;
    }
}
```