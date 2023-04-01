[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 352\. Data Stream as Disjoint Intervals

Hard

Given a data stream input of non-negative integers <code>a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub></code>, summarize the numbers seen so far as a list of disjoint intervals.

Implement the `SummaryRanges` class:

*   `SummaryRanges()` Initializes the object with an empty stream.
*   `void addNum(int val)` Adds the integer `val` to the stream.
*   `int[][] getIntervals()` Returns a summary of the integers in the stream currently as a list of disjoint intervals <code>[start<sub>i</sub>, end<sub>i</sub>]</code>.

**Example 1:**

**Input**

    ["SummaryRanges", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals"]
    [[], [1], [], [3], [], [7], [], [2], [], [6], []]

**Output:** [null, null, [[1, 1]], null, [[1, 1], [3, 3]], null, [[1, 1], [3, 3], [7, 7]], null, [[1, 3], [7, 7]], null, [[1, 3], [6, 7]]]

**Explanation:**

    SummaryRanges summaryRanges = new SummaryRanges();
    summaryRanges.addNum(1); // arr = [1]
    summaryRanges.getIntervals(); // return [[1, 1]]
    summaryRanges.addNum(3); // arr = [1, 3]
    summaryRanges.getIntervals(); // return [[1, 1], [3, 3]]
    summaryRanges.addNum(7); // arr = [1, 3, 7]
    summaryRanges.getIntervals(); // return [[1, 1], [3, 3], [7, 7]]
    summaryRanges.addNum(2); // arr = [1, 2, 3, 7]
    summaryRanges.getIntervals(); // return [[1, 3], [7, 7]]
    summaryRanges.addNum(6); // arr = [1, 2, 3, 6, 7]
    summaryRanges.getIntervals(); // return [[1, 3], [6, 7]]

**Constraints:**

*   <code>0 <= val <= 10<sup>4</sup></code>
*   At most <code>3 * 10<sup>4</sup></code> calls will be made to `addNum` and `getIntervals`.

**Follow up:** What if there are lots of merges and the number of disjoint intervals is small compared to the size of the data stream?

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class SummaryRanges {
    private static class Node {
        int min;
        int max;

        public Node(int val) {
            min = val;
            max = val;
        }
    }

    private List<Node> list;

    public SummaryRanges() {
        list = new ArrayList<>();
    }

    public void addNum(int val) {
        int left = 0;
        int right = list.size() - 1;
        int index = list.size();
        while (left <= right) {
            int mid = left + (right - left) / 2;
            Node node = list.get(mid);
            if (node.min <= val && val <= node.max) {
                return;
            } else if (val < node.min) {
                index = mid;
                right = mid - 1;
            } else if (val > node.max) {
                left = mid + 1;
            }
        }
        list.add(index, new Node(val));
    }

    public int[][] getIntervals() {
        int i = 1;
        while (i < list.size()) {
            Node prev = list.get(i - 1);
            Node curr = list.get(i);
            if (curr.min == prev.max + 1) {
                prev.max = curr.max;
                list.remove(i--);
            }
            i++;
        }
        int len = list.size();
        int[][] res = new int[len][2];
        for (int j = 0; j < len; j++) {
            Node node = list.get(j);
            res[j][0] = node.min;
            res[j][1] = node.max;
        }
        return res;
    }
}
```