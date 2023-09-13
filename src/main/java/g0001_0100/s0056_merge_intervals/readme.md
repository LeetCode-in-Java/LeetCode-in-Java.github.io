[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> list = new ArrayList<>();
        int[] current = intervals[0];
        list.add(current);
        for (int[] next : intervals) {
            if (current[1] >= next[0]) {
                current[1] = Math.max(current[1], next[1]);
            } else {
                current = next;
                list.add(current);
            }
        }
        return list.toArray(new int[list.size()][]);
    }
}
```

**Time Complexity (Big O Time):**

1. The program first sorts the `intervals` array based on the start values of each interval. Sorting takes O(n log n) time, where 'n' is the number of intervals in the array.

2. After sorting, it iterates through the sorted intervals exactly once.

3. In the loop, it performs constant time operations for each interval, such as comparisons and updates.

4. Therefore, the overall time complexity is dominated by the sorting step and is O(n log n), where 'n' is the number of intervals in the input array `intervals`.

**Space Complexity (Big O Space):**

1. The space complexity of the program is determined by the additional data structures used.

2. It uses a `List<int[]>` called `list` to store the merged intervals. In the worst case, where there are no overlapping intervals, this list could contain all 'n' intervals. Therefore, the space complexity of this list is O(n).

3. The `current` array of size 2 is used to keep track of the current merged interval. This array requires constant space.

4. The `int[][]` array returned at the end of the program is created based on the size of the `list`. In the worst case, it would have 'n' subarrays, each with two elements.

5. Therefore, the overall space complexity is dominated by the `list` and is O(n).

In summary, the time complexity of the provided program is O(n log n) due to the sorting step, and the space complexity is O(n) due to the `list` used to store merged intervals. The program efficiently merges overlapping intervals and returns a new array of merged intervals.
