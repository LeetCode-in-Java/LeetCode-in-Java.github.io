[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4001\. Aggregate Two Time Series

Medium

You are given two 2D integer arrays `series1` and `series2`.

Each element in both series is of the form `[timestamp, value]`, where:

*   `timestamp` is an integer representing the time.
*   `value` is an integer representing the value at that timestamp.

Each array is sorted in strictly increasing order of `timestamp`.

For any timestamp **not present** in a series, its value is taken from the **next available timestamp** in the same series if one exists. Otherwise, its value is considered 0.

The **aggregated series** is formed by summing the corresponding values from both series at every timestamp that appears in either series.

Return the **aggregated series** as a 2D integer array of `[timestamp, summedValue]` pairs, sorted in **strictly increasing** order of timestamp.

**Example 1:**

**Input:** series1 = \[\[1,3],[4,1]], series2 = \[\[2,2],[5,2]]

**Output:** [[1,5],[2,3],[4,3],[5,2]]

**Explanation:**

| Timestamp | `series1` | `series2` | `summedValue` |
| --- | --- | --- | --- |
| 1 | 3 | 2 | 5 |
| 2 | 1 | 2 | 3 |
| 4 | 1 | 2 | 3 |
| 5 | 0 | 2 | 2 |

Thus, the aggregated series is `[[1, 5], [2, 3], [4, 3], [5, 2]]`.

**Example 2:**

**Input:** series1 = \[\[1,5],[3,1]], series2 = \[\[2,2]]

**Output:** [[1,7],[2,3],[3,1]]

**Explanation:**

| Timestamp | `series1` | `series2` | `summedValue` |
| --- | --- | --- | --- |
| 1 | 5 | 2 | 7 |
| 2 | 1 | 2 | 3 |
| 3 | 1 | 0 | 1 |

Thus, the aggregated series is `[[1, 7], [2, 3], [3, 1]]`.

**Example 3:**

**Input:** series1 = \[\[1,5]], series2 = \[\[1000000000,2]]

**Output:** [[1,7],[1000000000,2]]

**Explanation:**

At timestamp 1, the next available value in `series2` is 2 at timestamp 1000000000. At timestamp 1000000000, there is no later timestamp in `series1`, so its value is 0. Only timestamps that appear in at least one of the two series are included.

**Constraints:**

*   <code>1 <= series1.length, series2.length <= 10<sup>5</sup></code>
*   `series1[i].length == series2[i].length == 2`
*   <code>1 <= series1[i][0], series2[i][0] <= 10<sup>9</sup></code>
*   <code>1 <= series1[i][1], series2[i][1] <= 10<sup>9</sup></code>
*   Each series is sorted in strictly increasing order of `timestamp`.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> aggregateTimeSeries(int[][] left, int[][] right) {
        int n = left.length;
        int m = right.length;
        List<List<Integer>> result = new ArrayList<>(left.length + m);
        int i = 0;
        int j = 0;
        while (i < n && j < m) {
            int[] lp = left[i];
            int[] rp = right[j];
            int cmp = Integer.compare(lp[0], rp[0]);
            int value = lp[1] + rp[1];
            if (cmp < 0) {
                result.add(List.of(lp[0], value));
                i++;
            } else if (cmp > 0) {
                result.add(List.of(rp[0], value));
                j++;
            } else {
                result.add(List.of(rp[0], value));
                i++;
                j++;
            }
        }
        while (i < n) {
            result.add(List.of(left[i][0], left[i][1]));
            i++;
        }
        while (j < m) {
            result.add(List.of(right[j][0], right[j][1]));
            j++;
        }
        return result;
    }
}
```