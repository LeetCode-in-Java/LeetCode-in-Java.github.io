## 56\. Merge Intervals

Medium

Given an array of `intervals` where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = \[\[1,3],[2,6],[8,10],[15,18]]

**Output:** [[1,6],[8,10],[15,18]]

**Explanation:** Since intervals [1,3] and [2,6] overlaps, merge them into [1,6]. 

**Example 2:**

**Input:** intervals = \[\[1,4],[4,5]]

**Output:** [[1,5]]

**Explanation:** Intervals [1,4] and [4,5] are considered overlapping. 

**Constraints:**

*   <code>1 <= intervals.length <= 10<sup>4</sup></code>
*   `intervals[i].length == 2`
*   <code>0 <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int[][] merge(int[][] intervals) {
        // sorting
        // so that we can perform the task linearly
        Arrays.sort(
                intervals,
                (int[] a, int[] b) -> {
                    if (a[0] == b[0]) {
                        return Integer.compare(a[1], b[1]);
                    }
                    return Integer.compare(a[0], b[0]);
                });

        List<List<Integer>> list = new ArrayList<>();
        int i = 0;
        while (i < intervals.length) {
            // storing start
            int start = intervals[i][0];
            int end = intervals[i][1];
            i++;
            while (i < intervals.length && intervals[i][0] <= end) {
                // making sure range is not shrinking
                if (intervals[i][1] > end) {
                    end = intervals[i][1];
                }
                i++;
            }
            List<Integer> temp = new ArrayList<>();
            temp.add(start);
            temp.add(end);
            list.add(temp);
        }
        int[][] arr = new int[list.size()][2];
        i = 0;
        for (List<Integer> l : list) {
            arr[i][0] = l.get(0);
            arr[i][1] = l.get(1);
            i++;
        }
        return arr;
    }
}
```