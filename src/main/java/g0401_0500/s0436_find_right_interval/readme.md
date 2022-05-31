## 436\. Find Right Interval

Medium

You are given an array of `intervals`, where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> and each <code>start<sub>i</sub></code> is **unique**.

The **right interval** for an interval `i` is an interval `j` such that <code>start<sub>j</sub></code><code> >= end<sub>i</sub></code> and <code>start<sub>j</sub></code> is **minimized**.

Return _an array of **right interval** indices for each interval `i`_. If no **right interval** exists for interval `i`, then put `-1` at index `i`.

**Example 1:**

**Input:** intervals = [[1,2]]

**Output:** [-1]

**Explanation:** There is only one interval in the collection, so it outputs -1. 

**Example 2:**

**Input:** intervals = [[3,4],[2,3],[1,2]]

**Output:** [-1,0,1]

**Explanation:**

There is no right interval for [3,4].

The right interval for [2,3] is [3,4] since start<sub>0</sub> = 3 is the smallest start that is >= end<sub>1</sub> = 3.

The right interval for [1,2] is [2,3] since start<sub>1</sub> = 2 is the smallest start that is >= end<sub>2</sub> = 2.

**Example 3:**

**Input:** intervals = [[1,4],[2,3],[3,4]]

**Output:** [-1,2,-1]

**Explanation:**

There is no right interval for [1,4] and [3,4].

The right interval for [2,3] is [3,4] since start<sub>2</sub> = 3 is the smallest start that is >= end<sub>1</sub> = 3.

**Constraints:**

*   <code>1 <= intervals.length <= 2 * 10<sup>4</sup></code>
*   `intervals[i].length == 2`
*   <code>-10<sup>6</sup> <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>6</sup></code>
*   The start point of each interval is **unique**.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private int[] findminmax(int[][] num) {
        int min = num[0][0];
        int max = num[0][0];
        for (int i = 1; i < num.length; i++) {
            min = Math.min(min, num[i][0]);
            max = Math.max(max, num[i][0]);
        }
        return new int[] {min, max};
    }

    public int[] findRightInterval(int[][] intervals) {
        if (intervals.length <= 1) {
            return new int[] {-1};
        }
        int n = intervals.length;
        int[] result = new int[n];
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(intervals[i][0], i);
        }
        int[] minmax = findminmax(intervals);
        for (int i = minmax[1] - 1; i >= minmax[0] + 1; i--) {
            map.computeIfAbsent(i, k -> map.get(k + 1));
        }
        for (int i = 0; i < n; i++) {
            result[i] = map.getOrDefault(intervals[i][1], -1);
        }
        return result;
    }
}
```